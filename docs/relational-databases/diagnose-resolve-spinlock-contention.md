---
title: 'Livre blanc : Diagnostiquer et résoudre la contention de verrouillage tournant'
description: Cet article est une présentation détaillée du diagnostic et de la résolution de la contention de verrouillage tournant dans SQL Server. Cet article a été initialement publié par l’équipe SQLCAT de Microsoft.
ms.date: 09/30/2020
ms.prod: sql
ms.reviewer: wiassaf
ms.technology: performance
ms.topic: how-to
author: bluefooted
ms.author: pamela
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 00b4856ab0c057b7f63aae44834884bc775d8e92
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102167"
---
# <a name="diagnose-and-resolve-spinlock-contention-on-sql-server"></a>Diagnostiquer et résoudre la contention de verrouillage tournant dans SQL Server

Cet article fournit des informations détaillées sur la façon d’identifier et de résoudre les problèmes liés à la contention de verrouillage tournant (« spinlock ») dans les applications SQL Server sur des systèmes à haute concurrence.

> [!NOTE]
> Les recommandations et bonnes pratiques documentées ici sont basées sur une expérience concrète acquise durant le développement et le déploiement de systèmes OLTP réels. Cet article a été initialement publié par l’équipe de conseillers à la clientèle SQL Server (SQLCAT).

## <a name="background"></a>Arrière-plan

Par le passé, les ordinateurs Windows Server de base n’utilisaient qu’une ou deux puces microprocesseurs, lesquelles étaient conçues avec un seul processeur ou « cœur ». L’augmentation de la capacité de traitement des ordinateurs a été rendue possible grâce à l’utilisation de processeurs plus rapides, résultant en grande partie des progrès réalisés dans la densité des transistors. Selon la « loi de Moore », la densité des transistors, c’est-à-dire le nombre de transistors pouvant être placés sur un circuit intégré, double tous les deux ans, et ce depuis le développement du premier processeur monopuce universel en 1971. Ces dernières années, l’approche traditionnelle consistant à augmenter la capacité de traitement des ordinateurs avec des processeurs plus rapides a bénéficié de l’apport des ordinateurs multiprocesseurs. Au moment de la rédaction de cet article, l’architecture du processeur Intel Nehalem prend en charge jusqu’à huit cœurs par processeur. Placés dans un système à huit sockets, ces cœurs peuvent donner lieu à 128 processeurs logiques grâce à l’utilisation de la technologie Hyper-Threading. Mais l’augmentation du nombre de processeurs logiques sur les ordinateurs compatibles x86 entraîne une hausse des problèmes de concurrence à mesure que les processeurs logiques se disputent les ressources. Ce guide décrit comment identifier et résoudre les problèmes de contention de ressources particulières observés lors de l’exécution d’applications SQL Server sur des systèmes à haute concurrence avec certaines charges de travail.

Dans cette section, nous allons analyser les enseignements tirés par l’équipe SQLCAT sur le diagnostic et la résolution des problèmes de contention de verrouillage tournant. La contention de verrouillage tournant est un type de problème de concurrence observé dans des charges de travail client réelles sur des systèmes à grande échelle.

## <a name="symptoms-and-causes-of-spinlock-contention"></a>Symptômes et causes de la contention de verrouillage tournant

Cette section décrit comment diagnostiquer les problèmes de *contention de verrouillage tournant* qui nuisent aux performances des applications OLTP sur SQL Server. Le diagnostic et la résolution de ces problèmes sont des sujets avancés qui nécessitent des connaissances sur les outils de débogage et les éléments internes de Windows.

Les verrouillages tournants sont des primitives de synchronisation légères qui permettent de protéger l’accès aux structures de données. Les verrouillages tournants ne sont pas propres à SQL Server. Le système d’exploitation les utilise quand l’accès à une structure de données n’est nécessaire que pendant une brève période. Si un thread qui tente d’acquérir un verrouillage tournant ne parvient pas à obtenir l’accès, il s’exécute dans une boucle pour vérifier régulièrement si la ressource est disponible au lieu de céder immédiatement le contrôle (« yield »). Après un certain temps, un thread en attente sur un verrouillage tournant cède le contrôle avant de pouvoir acquérir la ressource. La cession du contrôle permet à d’autres threads sur le même processeur de s’exécuter. Ce comportement, appelé « backoff » (interruption), est examiné en détail plus loin dans cet article.

SQL Server utilise des verrouillages tournants pour protéger l’accès à certaines de ses structures de données internes. De manière similaire aux verrous internes, les verrouillages tournants sont utilisés dans le moteur pour sérialiser l’accès à certaines structures de données. La principale différence entre les deux est la suivante : un verrouillage tournant exécute une boucle (« spin ») pendant une période de temps pour vérifier la disponibilité d’une structure de données, tandis qu’un thread tentant d’obtenir l’accès à une structure protégée par un verrou interne cède immédiatement le contrôle si la ressource n’est pas disponible. La cession du contrôle nécessite le changement de contexte d’un thread du processeur pour qu’un autre thread puisse s’exécuter. Cette opération est relativement coûteuse. Pour les ressources qui sont conservées pendant une courte durée, il est plus rentable d’exécuter un thread dans une boucle pour vérifier régulièrement la disponibilité de la ressource.

## <a name="symptoms"></a>Symptômes

Sur tout système à haute concurrence, il est normal de voir une contention active sur les structures fréquemment utilisées et protégées par des verrouillages tournants. La situation ne devient problématique que si la contention introduit une surcharge processeur importante. Pour exposer les statistiques d’un verrouillage tournant, utilisez la vue de gestion dynamique (DMV) *sys.dm_os_spinlock_stats* dans SQL Server. Par exemple, cette requête produit la sortie suivante :

> [!NOTE]
> Vous trouverez plus de détails sur l’interprétation des informations retournées par cette DMV plus loin dans cet article.

```sql
select * from sys.dm_os_spinlock_stats
order by spins desc
```

![Sortie de sys.dm_os_spinlock_stats](./media/diagnose-resolve-spinlock-contention/image4.png)

Les statistiques exposées par cette requête sont décrites ci-dessous :

| Colonne | Description |
|---|---|
| **collisions** | Cette valeur est incrémentée chaque fois qu’un thread ne peut pas accéder à une ressource protégée par un verrouillage tournant. |
| **spins** | Cette valeur est incrémentée chaque fois qu’un thread exécute une boucle dans l’attente de la disponibilité d’un verrouillage tournant. Il s’agit d’une mesure de la quantité de travail effectuée par un thread tentant d’acquérir une ressource. |
| **spins_per_collision** | Nombre de spins par collision. |
| **sleep time** | Cette valeur est liée aux événements de backoff. Toutefois, elle ne s’applique pas aux techniques décrites dans cet article. |
| **backoffs** | Se produit quand un thread qui « tourne en boucle » pour tenter d’accéder à une ressource protégée détermine qu’il doit autoriser d’autres threads sur le même processeur à s’exécuter. |

Dans le cadre de cette discussion, les statistiques particulièrement intéressantes sont le nombre de « collisions », de « spins » et de « backoffs » qui se produisent au cours d’une période spécifique quand le système est soumis à une charge importante. Quand un thread tente d’accéder à une ressource protégée par un verrouillage tournant, une collision se produit. Le nombre de collisions est alors incrémenté et le thread commence à tourner en boucle pour vérifier périodiquement si la ressource est disponible. Chaque fois que le thread tourne (effectue une boucle), le nombre de spins est incrémenté.

Le nombre de spins par collision est une mesure de la quantité de spins se produisant quand un verrouillage tournant est détenu par un thread. Par exemple, si le nombre de spins par collision est faible et que le nombre de collisions est élevé, cela signifie que le nombre de spins dans le verrouillage tournant est modeste et que de nombreux threads sont en contention. Un grand nombre de spins signifie que le temps passé à tourner en boucle dans le code du verrouillage tournant est relativement long (autrement dit, le code a affaire à un grand nombre d’entrées dans un compartiment de hachage). À mesure que la contention augmente (et, par voie de conséquence, le nombre de collisions), le nombre de spins augmente également.

Les backoffs sont étroitement liés aux spins. Pour éviter une utilisation excessive du processeur, un verrouillage tournant, de par sa conception, ne continue pas de tourner jusqu’à ce qu’il parvienne à accéder à une ressource bloquée. Pour veiller à ce qu’un verrouillage tournant n’utilise pas excessivement les ressources du processeur, un backoff est effectué. Autrement dit, le verrouillage tournant est interrompu et est « mis en veille ». Un backoff se produit même si le verrouillage ne devient pas propriétaire de la ressource cible. D’autres threads peuvent ainsi être planifiés sur le processeur dans l’espoir d’autoriser l’exécution de tâches plus productives. Le comportement par défaut du moteur est de tourner pendant un intervalle de temps constant avant d’effectuer un backoff. La tentative d’obtention d’un verrouillage tournant nécessite le maintien d’un état de concurrence du cache, ce qui est une opération gourmande en ressources processeur par rapport à une exécution en boucle. Les tentatives d’obtention d’un verrouillage tournant sont donc effectuées avec modération et ne se produisent pas chaque fois qu’un thread s’exécute en boucle. Dans SQL Server, certains types de verrouillage tournant (par exemple, LOCK_HASH) ont été améliorés grâce à l’utilisation d’un intervalle exponentiellement croissant entre les tentatives d’acquisition du verrouillage tournant (jusqu’à une certaine limite), ce qui a permis de réduire l’impact sur les performances du processeur dans bien des cas.

Le diagramme suivant fournit une vue conceptuelle de l’algorithme de verrouillage tournant :

![Vue conceptuelle de l’algorithme de verrouillage tournant](./media/diagnose-resolve-spinlock-contention/image5.png)

## <a name="typical-scenarios"></a>Scénarios classiques

Une contention de verrouillage tournant peut se produire pour diverses raisons qui peuvent n’avoir aucun lien avec les décisions de conception de base de données. Étant donné qu’un verrouillage tournant protège l’accès à une structure de données interne, la contention de verrouillage tournant ne se manifeste pas de la même façon qu’une contention de verrou interne sur une mémoire tampon, laquelle est directement affectée par les choix de conception du schéma et les modèles d’accès aux données.

Le principal symptôme associé à la contention de verrouillage tournant est une consommation élevée du processeur, celle-ci résultant de la quantité élevée de spins et des nombreux threads tentant d’acquérir le même verrouillage tournant. En général, ce phénomène est observé sur des systèmes avec \>=24 cœurs (mais le plus souvent sur des systèmes avec \>=32 cœurs). Comme indiqué précédemment, un certain niveau de contention de verrouillage tournant est normal pour les systèmes OLTP à haute concurrence avec une charge importante. Par ailleurs, le nombre de spins signalés par la DMV *sys.dm_os_spinlock_stats* est souvent de l’ordre de plusieurs milliards sur les systèmes exécutés pendant une longue période de temps. Là encore, l’observation d’un nombre élevé de spins pour un type de verrouillage tournant donné ne suffit pas à déterminer que les performances de la charge de travail sont négativement impactées.

Une combinaison de plusieurs symptômes peut indiquer une contention de verrouillage tournant. Ces symptômes sont les suivants :

* Un nombre élevé de spins et de backoffs est observé pour un type de verrouillage tournant particulier.

* Le processeur du système fait l’objet d’une utilisation intensive ou de pics de consommation. Dans les scénarios à forte utilisation du processeur, vous voyez un nombre d’attentes élevé sur SOS_SCHEDULER_YIELD (signalé par la DMV *sys.dm_os_wait_stats*).

* Le système fait l’objet d’une haute concurrence.

* L’utilisation du processeur et le nombre de spins augmentent de manière disproportionnée par rapport au débit.

   > [!IMPORTANT]
   > Même si chacune des conditions précédentes est vraie, il est possible que la cause racine de la consommation élevée du processeur se trouve ailleurs. En fait, dans la grande majorité des cas, l’augmentation de la consommation du processeur est due à des raisons autres que la contention de verrouillage tournant. Voici quelques-unes des causes les plus courantes de la hausse de la consommation du processeur :

* Requêtes devenant plus onéreuses au fil du temps en raison de la croissance des données sous-jacentes, d’où la nécessité d’effectuer des lectures logiques supplémentaires des données résidentes en mémoire.

* Changements dans les plans de requête aboutissant à une exécution sous-optimale.

Si toutes ces conditions sont vraies, effectuez un examen plus approfondi pour déterminer la présence éventuelle d’une contention de verrouillage tournant.

Un phénomène courant facilement diagnostiqué est une divergence significative entre le débit et l’utilisation du processeur. De nombreuses charges de travail OLTP se caractérisent par une relation entre, d’une part, le rapport débit/nombre d’utilisateurs sur le système et, d’autre part, la consommation du processeur. L’observation conjointe d’un nombre élevé de spins et d’une divergence significative entre la consommation du processeur et le débit peut indiquer que la surcharge du processeur est causée par une contention de verrouillage tournant. Il est important de noter qu’il est fréquent de voir ce type de divergence sur des systèmes quand certaines requêtes deviennent plus onéreuses au fil du temps. Par exemple, des requêtes émises sur des jeux de données qui effectuent un plus grand nombre de lectures logiques au fil du temps peuvent donner lieu à des symptômes similaires.

Il est essentiel de connaître les autres causes courantes de consommation élevée du processeur lors de la résolution de ces types de problèmes.

## <a name="example"></a>Exemple

Dans l’exemple suivant, il existe une relation quasi linéaire entre la consommation du processeur et le débit mesuré en transactions par seconde. Il est normal de constater une certaine divergence ici, car la surcharge est due à l’augmentation de la charge de travail. Comme vous pouvez le voir ici, cette divergence devient importante. Vous noterez également une chute brutale du débit quand la consommation du processeur atteint 100 %.

![Chute de l’utilisation du processeur dans l’Analyseur de performances](./media/diagnose-resolve-spinlock-contention/image7.png)

Si nous mesurons le nombre de spins à 3 minutes d’intervalle, nous pouvons voir une augmentation plus exponentielle que linéaire des spins, ce qui indique que la contention de verrouillage tournant peut être problématique.

![Graphique des spins à 3 minutes d’intervalle](./media/diagnose-resolve-spinlock-contention/image8.png)

Comme indiqué précédemment, les verrouillages tournants sont les plus fréquents sur des systèmes à haute concurrence soumis à une charge importante.

Voici quelques-uns des scénarios où ce problème est susceptible de se manifester :

* Problèmes de résolution de noms causés par l’utilisation de noms d’objets non complets. Pour plus d’informations, consultez [Description du blocage SQL Server provoqué par des verrous de compilation](https://support.microsoft.com/help/263889/how-to-troubleshoot-blocking-caused-by-compile-locks). Ce problème spécifique est décrit plus en détail dans cet article.

* Contention sur des compartiments de hachage de verrou dans le gestionnaire de verrous pour les charges de travail qui accèdent fréquemment au même verrou (par exemple, un verrou partagé sur une ligne fréquemment lue). Ce type de contention est un verrouillage tournant de type LOCK_HASH. Dans un cas particulier, nous avons constaté que ce problème était dû à des modèles d’accès mal modélisés dans un environnement de test. Dans cet environnement, des threads en quantité supérieure à celle attendue accédaient en permanence à la même ligne en raison d’une mauvaise configuration des paramètres de test.

* Taux élevé de transactions DTC causé par un degré élevé de latence entre les coordinateurs de transactions MSDTC. Ce problème spécifique est documenté en détail dans le billet de blog suivant de l’équipe SQLCAT : [Resolving DTC Related Waits and Tuning Scalability of DTC](https://techcommunity.microsoft.com/t5/datacat/resolving-dtc-related-waits-and-tuning-scalability-of-dtc/ba-p/305054).

## <a name="diagnosing-spinlock-contention"></a>Diagnostic de la contention de verrouillage spinlock

Cette section fournit des informations pour diagnostiquer la contention de verrouillage tournant dans SQL Server. Les principaux outils utilisés pour diagnostiquer la contention de verrouillage tournant sont les suivants :

| Outil | Utilisez |
|---|---|
| **Analyseur de performances** | Permet de rechercher des conditions d’utilisation élevée du processeur ou une divergence entre le débit et la consommation du processeur. |
| **DMV sys.dm_os_spinlock_stats** | Permet de rechercher un nombre élevé de spins et d’événements de backoff sur des périodes de temps. |
| **Événements étendus SQL Server** | Permet de suivre des piles d’appels à la recherche de verrouillages tournants avec un grand nombre de spins. |
| **Images mémoire** | Dans certains cas, il est utile d’analyser les images mémoire du processus SQL Server avec les outils de débogage Windows. En général, cette analyse est menée en collaboration avec les équipes du support technique de Microsoft SQL Server. |

Le processus technique généralement employé pour diagnostiquer une contention de verrouillage tournant dans SQL Server est le suivant :

1. **Étape 1** : Déterminez l’existence d’une contention pouvant être liée à un verrouillage tournant.

2. **Étape 2** : Capturez les statistiques de *sys.dm\_ os_spinlock_stats* pour trouver le type de verrouillage tournant avec la plus forte contention.

3. **Étape 3** : Obtenez les symboles de débogage pour sqlservr.exe (sqlservr.pdb) et placez-les dans le même répertoire que le fichier .exe du service SQL Server (sqlservr.exe) pour l’instance de SQL Server. Pour voir les piles d’appels des événements de backoff, vous devez disposer de symboles pour la version particulière de SQL Server que vous exécutez. Les symboles pour SQL Server sont disponibles sur le serveur de symboles Microsoft. Pour plus d’informations sur le téléchargement de symboles à partir du serveur de symboles Microsoft, consultez [Débogage avec des symboles](/windows/win32/dxtecharts/debugging-with-symbols).

4. **Étape 4** : Utilisez les événements étendus SQL Server pour suivre les événements de backoff pour les types de verrouillage tournant présentant un intérêt.

Les événements étendus permettent de suivre l’événement de \"backoff\" et de capturer la pile d’appels pour la ou les opérations qui tentent le plus couramment d’obtenir le verrouillage tournant. En analysant la pile d’appels, il est possible de déterminer le type d’opération qui contribue à la contention pour un verrouillage tournant particulier.

## <a name="diagnostic-walkthrough"></a>Procédure pas à pas de diagnostic

La procédure pas à pas suivante vous montre comment utiliser les outils et techniques présentés dans cet article pour diagnostiquer un problème de contention de verrouillage tournant dans un scénario réel. Cette procédure se base sur un engagement client exécutant un test de référence pour simuler environ 6 500 utilisateurs simultanés sur un serveur contenant 8 sockets, 64 cœurs physiques et 1 To de mémoire.

### <a name="symptoms"></a>Symptômes

Des pics périodiques sont observés, provoquant une utilisation du processeur proche des 100 %. Une divergence entre le débit et la consommation du processeur est également constatée avant l’apparition du problème. Au moment du pic de consommation du processeur, un modèle composé d’un grand nombre de spins se produisant durant les périodes de forte utilisation du processeur à des intervalles particuliers est établi.

Dans ce cas extrême, la contention est telle qu’elle crée une condition de convoi de verrouillage tournant. Un convoi se produit lorsque les threads n’avancent plus dans la gestion de la charge de travail et qu’ils utilisent à la place toutes les ressources de traitement pour tenter d’accéder au verrou. Le journal de l’Analyseur de performances montre cette divergence entre le débit du journal des transactions et la consommation du processeur, et enfin le pic d’utilisation du processeur.

![Pic de l’utilisation du processeur dans l’Analyseur de performances](./media/diagnose-resolve-spinlock-contention/image9.png)

Après l’interrogation de *sys.dm_os_spinlock_stats* pour déterminer l’existence d’une contention importante sur SOS_CACHESTORE, un script d’événements étendus est utilisé pour mesurer le nombre d’événements de backoff pour les types de verrouillage tournant présentant un intérêt.

| Nom | Collisions | Spins | Spins par collision | Backoffs |
|---|---|---|---|---|
| **SOS_CACHESTORE** |       14 752 117 |   942 869 471 526 |   63 914 |                67 900 620 |
| **SOS_SUSPEND_QUEUE** |   69 267 367 |   473 760 338 765 |   6 840  |                2 167 281 |
| **LOCK_HASH** |           5 765 761 |    260 885 816 584 |   45 247 |                3 739 208 |
| **MUTEX** |               2 802 773 |    9 767 503 682 |     3 485  |                350 997 |
| **SOS_SCHEDULER** |       1 207 007 |    3 692 845 572 |     3 060  |                109 746 |

La méthode la plus simple pour quantifier l’impact des spins consiste à regarder le nombre d’événements de backoff exposés par *sys.dm_os_spinlock_stats* sur le même intervalle de 1 minute pour le ou les verrouillages tournants avec le plus grand nombre de spins. Cette méthode est idéale pour détecter une contention importante, car elle indique quand les threads arrivent à la limite de spins dans l’attente d’acquérir le verrouillage tournant. Le script suivant illustre une technique avancée qui utilise des événements étendus pour mesurer les événements de backoff associés et identifier les chemins de code spécifiques où se situe la contention.

Pour plus d’informations sur les événements étendus dans SQL Server, consultez [Présentation des événements étendus SQL Server](./extended-events/extended-events.md).

**Script**

```sql
/*
This Scriptis provided "AS IS" with no warranties, and confers no rights.

This script will monitor for backoff events over a given period of time and
capture the code paths (callstacks) for those.

--Find the spinlock types
select map_value, map_key, name from sys.dm_xe_map_values
where name = 'spinlock_types'
order by map_value asc

--Example: Get the type value for any given spinlock type
select map_value, map_key, name from sys.dm_xe_map_values
where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')

Examples:
61LOCK_HASH
144 SOS_CACHESTORE
08MUTEX

*/

--create the even session that will capture the callstacks to a bucketizer
--more information is available in this reference: http://msdn.microsoft.com/en-us/library/bb630354.aspx
create event session spin_lock_backoff on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
where 
type = 61--LOCK_HASH
or type = 144--SOS_CACHESTORE
or type = 8--MUTEX
)
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

--Ensure the session was created
select * from sys.dm_xe_sessions
where name = 'spin_lock_backoff'

--Run this section to measure the contention 
alter event session spin_lock_backoff on server state=start

--wait to measure the number of backoffs over a 1 minute period
waitfor delay '00:01:00'

--To view the data
--1. Ensure the sqlservr.pdb is in the same directory as the sqlservr.exe
--2. Enable this trace flag to turn on symbol resolution 
DBCC traceon (3656, -1)

--Get the callstacks from the bucketize target
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spin_lock_backoff'

--clean up the session 
alter event session spin_lock_backoff on server state=stop
drop event session spin_lock_backoff on server
```

En analysant la sortie, nous pouvons voir les piles d’appels pour les chemins de code les plus courants pour les spins SOS_CACHESTORE. Le script a été exécuté plusieurs fois pendant la période de forte utilisation du processeur pour vérifier la cohérence dans les piles d’appels retournées. Notez que les piles d’appels avec le nombre de compartiments d’emplacement le plus élevé sont courantes entre les deux sorties (35 668 et 8 506). Ces piles d’appels ont un nombre d’emplacements (« slot count ») deux fois supérieur à l’entrée suivante. Cette condition indique un chemin de code digne d’intérêt.

> [!NOTE]
> Il n’est pas rare de voir les piles d’appels retournées par le script précédent. Lorsque le script s’est exécuté pendant 1 minute, nous avons observé que les piles d’appels avec un nombre d’emplacements > 1 000 étaient problématiques, mais que le nombre d’emplacements > 10 000 était plus susceptible d’être problématique car il a un nombre d’emplacements supérieur.

> [!NOTE]
> La mise en forme de la sortie suivante a été nettoyée pour la rendre plus lisible.

**Sortie 1**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="35668" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid 
      CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey
      CMEDCatalogOwner::GetProxyOwnerBySID
      CMEDProxyDatabase::GetOwnerBySID
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
  </value> 
</Slot>
<Slot count="752" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep 
      SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey             CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
  </value>
  </Slot>
```

**Sortie 2**

```xml
<BucketizerTarget truncated="0" buckets="256">
<Slot count="8506" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep+c7 [ @ 0+0x0 SpinlockBase::Backoff Spinlock<144,1,0>::SpinToAcquireOptimistic
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
      NTGroupInfo::`vector deleting destructor'
</value> 
 </Slot>
<Slot count="190" trunc="0">
  <value>
      XeSosPkg::spinlock_backoff::Publish 
      SpinlockBase::Sleep
       SpinlockBase::Backoff 
      Spinlock<144,1,0>::SpinToAcquireOptimistic 
      SOS_CacheStore::GetUserData 
      OpenSystemTableRowset 
      CMEDScanBase::Rowset 
      CMEDScan::StartSearch 
      CMEDCatalogOwner::GetOwnerAliasIdFromSid CMEDCatalogOwner::LookupPrimaryIdInCatalog CMEDCacheEntryFactory::GetProxiedCacheEntryByAltKey CMEDCatalogOwner::GetProxyOwnerBySID 
      CMEDProxyDatabase::GetOwnerBySID 
      ISECTmpEntryStore::Get 
      ISECTmpEntryStore::Get
      ISECTmpEntryStore::Get
   </value> 
 </Slot>
```

Dans l’exemple précédent, les piles les plus intéressantes ont le nombre d’emplacements le plus élevé (35 668 et 8 506), qui, en fait, ont un nombre d’emplacements \>1 000.

À présent, vous vous demandez peut-être ce que vous devez faire de ces informations. En général, une connaissance approfondie du moteur SQL Server est nécessaire pour utiliser les informations de la pile d’appels. À ce stade, le processus de résolution des problèmes entre dans une zone grise. Dans ce cas particulier, l’examen des piles d’appels nous permet de voir que le chemin de code où se produit le problème est lié à la sécurité et aux recherches de métadonnées (comme en témoignent les frames de pile **CMEDCatalogOwner::GetProxyOwnerBySID & CMEDProxyDatabase::GetOwnerBySID)** .

Ces informations sont difficiles à utiliser pour isoler et résoudre le problème, mais elles nous donnent une idée de la zone où concentrer davantage nos efforts afin d’isoler le problème.

Étant donné que ce problème semble lié aux chemins de code qui effectuent des contrôles liés à la sécurité, nous décidons d’exécuter un test dans lequel l’utilisateur de l’application se connectant à la base de données se voit accorder des privilèges sysadmin. Bien que cette technique ne soit jamais recommandée dans un environnement de production, elle constitue une étape de dépannage utile dans notre environnement de test. Quand les sessions sont exécutées avec des privilèges élevés (sysadmin), les pics d’utilisation du processeur liés à la contention disparaissent.

## <a name="options-and-workarounds"></a>Options et solutions de contournement

De toute évidence, le dépannage d’une contention de verrouillage tournant est loin d’être une tâche négligeable. Malheureusement, il n’y a pas d’approche commune. La première étape en vue de résoudre des problèmes de performances est d’identifier la cause racine. Commencez par utiliser les techniques et les outils décrits dans cet article pour analyser et comprendre les points de contention liés au verrouillage tournant.

À mesure que de nouvelles versions de SQL Server sont développées, le moteur continue d’améliorer la scalabilité en implémentant du code qui est mieux optimisé pour les systèmes à haute concurrence. SQL Server a ainsi introduit de nombreuses optimisations pour les systèmes à haute concurrence, l’une d’entre elles étant le backoff exponentiel pour les points de contention les plus courants. Des améliorations spécifiques ont été apportées à partir de SQL Server 2012 pour renforcer spécifiquement cette zone particulière en tirant parti des algorithmes de backoff exponentiel pour tous les verrouillages tournants au sein du moteur.

Quand vous concevez des applications haut de gamme nécessitant des performances et une scalabilité extrêmes, pensez à réduire au maximum le chemin de code nécessaire dans SQL Server. Un chemin de code plus court permet de réduire la charge de travail du moteur de base de données et d’éviter naturellement les points de contention. De nombreuses bonnes pratiques ont pour effet secondaire de réduire la quantité de travail du moteur, d’où une optimisation des performances de la charge de travail.

Voici quelques exemples de bonnes pratiques précédemment traitées dans cet article :

* **Noms complets** : utilisez des noms complets pour tous les objets. De cette façon, SQL Server n’est pas obligé d’exécuter les chemins de code requis pour résoudre les noms. Nous avons également observé des points de contention sur le type de verrouillage tournant SOS_CACHESTORE lors de l’utilisation de noms non complets dans les appels à des procédures stockées. Si vous n’utilisez pas de noms complets, SQL Server doit rechercher le schéma par défaut de l’utilisateur, ce qui se traduit par un chemin de code plus long requis pour exécuter le code SQL.

* **Requêtes paramétrables :** un autre exemple consiste à utiliser des requêtes paramétrables et des appels de procédure stockée pour réduire le travail nécessaire à la génération de plans d’exécution. Vous obtenez là encore un chemin de code plus court pour l’exécution.

* **Contention LOCK_HASH :** dans certains cas, la contention sur des structures de verrouillage ou des compartiments de hachage est inévitable. Bien que le moteur SQL Server partitionne la majorité des structures de verrouillage, il arrive encore que l’acquisition d’un verrou entraîne l’accès au même compartiment de hachage. Cela se produit par exemple quand une application accède simultanément à la même ligne (des données de référence) par le biais de plusieurs threads. Pour aborder ce type de problème, vous pouvez faire appel à des techniques qui effectuent un scale-out des données de référence dans le schéma de base de données ou qui tirent parti des conseils NOLOCK lorsque cela est possible.

La première ligne de défense en matière de réglage des charges de travail SQL Server est de faire appel aux pratiques standard (indexation, optimisation des requêtes, optimisation des E/S, etc.). Toutefois, en plus du réglage standard, une approche importante consiste à suivre des pratiques qui réduisent la quantité de code nécessaire pour effectuer des opérations. Même si vous appliquez les bonnes pratiques, il est toujours possible qu’une contention de verrouillage tournant se produise sur des systèmes à haute concurrence. L’utilisation des outils et des techniques présentés dans cet article peut vous aider à isoler ou à exclure ces types de problèmes et à déterminer quand il est nécessaire de faire appel aux ressources Microsoft appropriées.

Espérons que ces techniques vous fourniront à la fois une méthodologie utile pour ce type de dépannage et des insights sur certaines des techniques de profilage des performances plus avancées disponibles avec SQL Server.

## <a name="appendix-automate-memory-dump-capture"></a>Annexe : Automatiser la capture d’image mémoire

Le script d’événements étendus suivant est utile pour automatiser la collecte des images mémoire quand la contention de verrouillage tournant devient importante. Dans certains cas, des images mémoire sont nécessaires pour effectuer un diagnostic complet du problème ou sont demandées par les équipes du support technique de Microsoft pour procéder à une analyse approfondie. Dans SQL Server 2008, le nombre de frames dans les piles d’appels capturées par le bucketizer est limité à 16, ce qui peut être insuffisant pour déterminer avec exactitude où la pile d’appels est entrée dans le moteur. Dans SQL Server 2012, le nombre de frames dans les piles d’appels capturées par le bucketizer passe à 32.

Vous pouvez utiliser le script SQL suivant pour automatiser le processus de capture d’images mémoire afin de faciliter l’analyse de la contention de verrouillage tournant :

```sql
/*
This script is provided "AS IS" with no warranties, and confers no rights.

Use:    This procedure will monitor for spinlocks with a high number of backoff events
        over a defined time period which would indicate that there is likely significant
        spin lock contention.
        
        Modify the variables noted below before running.


Requires:
        xp_cmdshell to be enabled
            sp_configure 'xp_cmd', 1
            go 
            reconfigure 
            go
        
*********************************************************************************************************/
use tempdb
go 
if object_id('sp_xevent_dump_on_backoffs') is not null
    drop proc sp_xevent_dump_on_backoffs
go 
create proc sp_xevent_dump_on_backoffs
(
    @sqldumper_path                       nvarchar(max)      = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
    ,@dump_threshold                      int                = 500           --capture mini dump when the slot count for the top bucket exceeds this
    ,@total_delay_time_seconds            int                = 60            --poll for 60 seconds
    ,@PID                                 int                = 0
    ,@output_path                         nvarchar(max)      = 'c:\'
    ,@dump_captured_flag                  int = 0 OUTPUT
    
)
as
/* 
    --Find the spinlock types
    select map_value, map_key, name from sys.dm_xe_map_values
    where name = 'spinlock_types'
    order by map_value asc

    --Example: Get the type value for any given spinlock type
    select map_value, map_key, name from sys.dm_xe_map_values
    where map_value IN ('SOS_CACHESTORE', 'LOCK_HASH', 'MUTEX')
*/
if exists (select * from sys.dm_xe_session_targets xst
                inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
                where xs.name = 'spinlock_backoff_with_dump')
    drop event session spinlock_backoff_with_dump on server

create event session spinlock_backoff_with_dump  on server
      add event sqlos.spinlock_backoff (action (package0.callstack)
            where
                type = 61                 --LOCK_HASH
                --or type = 144           --SOS_CACHESTORE
                --or type = 8             --MUTEX
                --or type = 53            --LOGCACHE_ACCESS
                --or type = 41            --LOGFLUSHQ
                --or type = 25            --SQL_MGR
                --or type = 39            --XDESMGR
                )
      add target package0.asynchronous_bucketizer (
            set filtering_event_name='sqlos.spinlock_backoff',
            source_type=1, source='package0.callstack')
      with (MAX_MEMORY=50MB, MEMORY_PARTITION_MODE = PER_NODE)

alter event session spinlock_backoff_with_dump  on server state=start


declare @instance_name            nvarchar(max) = @@SERVICENAME
declare @loop_count               int = 1
declare @xml_result               xml 
declare @slot_count               bigint 
declare @xp_cmdshell              nvarchar(max) = null

--start polling for the backoffs
print 'Polling for: ' + convert(varchar(32), @total_delay_time_seconds) + ' seconds'
while (@loop_count < CAST (@total_delay_time_seconds/1 as int))
begin 
    waitfor delay '00:00:01'

    --get the xml from the bucketizer for the session
    select @xml_result= CAST(target_data as xml)
    from sys.dm_xe_session_targets xst
        inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
    where xs.name = 'spinlock_backoff_with_dump'
    
    --get the highest slot count from the bucketizer
    select @slot_count = @xml_result.value(N'(//Slot/@count)[1]', 'int')

    --if the slot count is higher than the threshold in the one minute period
    --dump the process and clean up session
    if (@slot_count > @dump_threshold)
    begin 
        print 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 c:\ '''
        select @xp_cmdshell = 'exec xp_cmdshell ''' + @sqldumper_path + ' ' + convert(nvarchar(max), @PID) + ' 0 0x800 0 ' + @output_path + ' '''
        exec sp_executesql @xp_cmdshell
        print 'loop count: ' + convert (varchar(128), @loop_count)
        print 'slot count: ' + convert (varchar(128), @slot_count)
        set @dump_captured_flag = 1
        break
    end 

    --otherwise loop 
    set @loop_count = @loop_count + 1

end

--see what was collected then clean up
DBCC traceon (3656, -1)
select event_session_address, target_name, execution_count, cast (target_data as XML)
from sys.dm_xe_session_targets xst
    inner join sys.dm_xe_sessions xs on (xst.event_session_address = xs.address)
where xs.name = 'spinlock_backoff_with_dump'

alter event session spinlock_backoff_with_dump  on server state=stop
drop event session spinlock_backoff_with_dump  on server
go

/* CAPTURE THE DUMPS 
******************************************************************/
--Example: This will run continuously until a dump is created. 
declare @sqldumper_path                nvarchar(max)        = '"c:\Program Files\Microsoft SQL Server\100\Shared\SqlDumper.exe"'
declare @dump_threshold                int                  = 300            --capture mini dump when the slot count for the top bucket exceeds this
declare @total_delay_time_seconds      int                  = 60             --poll for 60 seconds 
declare @PID                           int                  = 0
declare @flag                          tinyint              = 0
declare @dump_count                    tinyint              = 0
declare @max_dumps                     tinyint              = 3              --stop after collecting this many dumps
declare @output_path                   nvarchar(max)        = 'c:\'          --no spaces in the path please :)


--Get the process id for sql server 
declare @error_log table (LogDate datetime,
    ProcessInfo varchar(255),
    Text varchar(max)
    )
insert into @error_log
    exec ('xp_readerrorlog 0, 1, ''Server Process ID''')
select @PID = convert(int, (REPLACE(REPLACE(Text, 'Server Process ID is ', ''), '.', '')))
    from @error_log where Text like ('Server Process ID is%')
print 'SQL Server PID: ' + convert (varchar(6), @PID)

--Loop to monitor the spinlocks and capture dumps. while (@dump_count < @max_dumps)
begin 

    exec sp_xevent_dump_on_backoffs @sqldumper_path             = @sqldumper_path,
                                    @dump_threshold             = @dump_threshold,
                                    @total_delay_time_seconds   = @total_delay_time_seconds,
                                    @PID                        = @PID,
                                    @output_path                = @output_path,
                                    @dump_captured_flag         = @flag OUTPUT
    if (@flag > 0) 
        set @dump_count=@dump_count + 1
    print 'Dump Count: ' + convert(varchar(2), @dump_count)
    waitfor delay '00:00:02'

end
```

## <a name="appendix-capture-spinlock-statistics-over-time"></a>Annexe : Capturer les statistiques d’un verrouillage tournant au fil du temps

Vous pouvez utiliser le script suivant pour examiner les statistiques d’un verrouillage tournant sur une période de temps spécifique. Chaque fois que vous l’exécutez, il retourne la valeur delta entre les valeurs actuelles et les valeurs précédentes collectées.

```sql
/* Snapshot the current spinlock stats and store so that this can be compared over a time period
   Return the statistics between this point in time and the last collection point in time.

   **This data is maintained in tempdb so the connection must persist between each execution**
   **alternatively this could be modified to use a persisted table in tempdb. if that
   is changed code should be included to clean up the table at some point.**
*/

use tempdb
go

declare @current_snap_time    datetime
declare @previous_snap_time   datetime

set @current_snap_time = GETDATE()

if not exists(select name from tempdb.sys.sysobjects where name like '#_spin_waits%')
    create table #_spin_waits
    (
        lock_name    varchar(128)
        ,collisions  bigint
        ,spins       bigint
        ,sleep_time  bigint
        ,backoffs    bigint
        ,snap_time   datetime
    )

--capture the current stats
insert into #_spin_waits
    (
        lock_name
        ,collisions
        ,spins
        ,sleep_time
        ,backoffs
        ,snap_time
        )
        select  name
                ,collisions
                ,spins
                ,sleep_time
                ,backoffs
                ,@current_snap_time
        from sys.dm_os_spinlock_stats

select top 1 @previous_snap_time = snap_time from #_spin_waits
                where snap_time < (select max(snap_time) from #_spin_waits)
                order by snap_time desc

--get delta in the spin locks stats   
select top 10
        spins_current.lock_name
        , (spins_current.collisions - spins_previous.collisions) as collisions
        , (spins_current.spins - spins_previous.spins) as spins
        , (spins_current.sleep_time - spins_previous.sleep_time) as sleep_time
        , (spins_current.backoffs - spins_previous.backoffs) as backoffs
        , spins_previous.snap_time as [start_time]
        , spins_current.snap_time as [end_time]
        , DATEDIFF(ss, @previous_snap_time, @current_snap_time) as [seconds_in_sample]
    from #_spin_waits spins_current
    inner join (
        select * from #_spin_waits
          where snap_time = @previous_snap_time
        ) spins_previous on (spins_previous.lock_name = spins_current.lock_name)
    where
        spins_current.snap_time = @current_snap_time
        and spins_previous.snap_time = @previous_snap_time
        and spins_current.spins > 0
    order by (spins_current.spins - spins_previous.spins) desc

--clean up table
delete from #_spin_waits
where snap_time = @previous_snap_time
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les outils de supervision des performances, consultez [Outils de supervision et d’optimisation des performances](./performance/performance-monitoring-and-tuning-tools.md).
