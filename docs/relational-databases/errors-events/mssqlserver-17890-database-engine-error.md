---
description: MSSQLSERVER_17890
title: MSSQLSERVER_17890
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17890 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 6a854486d1867e84bcd9b13cb148d3026a41d51f
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797763"
---
# <a name="mssqlserver_17890"></a>MSSQLSERVER_17890
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|17890|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|SRV_WS_TRIMMED|
|Texte du message|Une partie significative de la mémoire du processus SQL Server a été hors page. Ce problème peut entraîner une dégradation des performances. Durée : %d secondes. Plage de travail (en Ko) : %I64d, validé (en Ko) : %I64d, utilisation de la mémoire : %d%%.|
||

## <a name="explanation"></a>Explication

Vous pouvez rencontrer le message d’erreur suivant dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou dans le journal des événements de l’application Windows.

> Une partie significative de la mémoire du processus SQL Server a été hors page. Ce problème peut entraîner une dégradation des performances. Durée : 0 secondes. Plage de travail (ko) : 3383250, validée (ko) : 9112480, pourcentage d’utilisation de la mémoire : 37 %.

Vous pouvez également remarquer une dégradation soudaine des performances avec l’exécution de la requête et toutes les autres opérations sur SQL Server.

## <a name="cause"></a>Cause

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] analyse les diverses informations sur les mémoires relatives au processus de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Dans ce cas, il a détecté que la plage de travail du processus était inférieure à 50 % de la mémoire processus allouée. Par conséquent, cet avertissement est imprimé. Les causes habituelles de cet avertissement sont les suivantes :

- Le système d’exploitation pagine de grandes parties de la mémoire allouée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au fichier d’échange.
- Cela peut être dû à une demande croissante soudaine de mémoire provenant d’autres applications ou des besoins du système d’exploitation.
- Cela peut également se produire lorsque certains pilotes de périphérique demandent des allocations de mémoire contiguës pour leurs besoins.

## <a name="user-action"></a>Action requise

Vous pouvez empêcher le système d’exploitation Windows de paginer la mémoire du pool de mémoires tampons du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en verrouillant la mémoire allouée pour le pool de mémoires tampons dans la mémoire physique. Vous verrouillez la mémoire en affectant le droit utilisateur Verrouiller les pages en mémoire au compte d’utilisateur utilisé comme compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Mais avant d’implémenter cette solution, passez en revue les sections [Ce qui entraîne la pagination de la mémoire de SQL Server](#what-causes-sql-server-memory-to-be-paged-out) et Considérations importantes avant d’attribuer le droit utilisateur « Verrouiller les pages en mémoire » pour une instance SQL Server

> [!NOTE]
> L’utilisation de Verrouiller les pages en mémoire garantit que la mémoire gérée par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’est pas paginée. Toutefois, les piles de threads, l’EXE et les images DLL, la mémoire du tas et la mémoire CLR peuvent toujours être paginées par le système d’exploitation.
>
> À compter de la mise à jour cumulative 2 de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008 SP1, les éditions [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Standard et Enterprise peuvent toutes deux utiliser le droit utilisateur Verrouiller les pages en mémoire. Pour plus d’informations sur la prise en charge des pages verrouillées, consultez [KB970070 - Prise en charge des pages verrouillées sur les systèmes SQL Server Standard Edition (64 bits)](https://support.microsoft.com/help/970070).

Pour affecter le droit utilisateur Verrouiller les pages en mémoire, procédez comme suit :

1. Cliquez sur **Démarrer**, puis **Exécuter**, tapez *gpedit.msc* et cliquez sur **OK**.
1. Notez que la boîte de dialogue Stratégie de groupe s’affiche.
1. Développez **Configuration ordinateur**, puis développez **Paramètres Windows**.
1. Développez **Paramètres de sécurité**, puis **Stratégies locales**.
1. Cliquez sur Affectation des droits de l’utilisateur, puis double-cliquez sur **Verrouiller les pages** en mémoire.
1. Dans la boîte de dialogue **Paramètres de stratégie de sécurité locale**, cliquez sur **Ajouter un utilisateur** ou **Ajouter un groupe**.
1. Dans la boîte de dialogue **Sélectionner des utilisateurs** ou **Sélectionner des groupes**, ajoutez le compte qui a l’autorisation d’exécuter le fichier sqlservr.exe, puis cliquez sur **OK**.
1. Fermez la boîte de dialogue **Stratégie de groupe**.
1. Redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Une fois que vous avez affecté le droit utilisateur Verrouiller les pages en mémoire et que vous redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le système d’exploitation Windows ne met plus en page la mémoire du pool de mémoires tampons au sein du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, le système d’exploitation Windows peut toujours paginer la mémoire du pool hors tampon au sein du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Vous pouvez vérifier que le droit utilisateur est utilisé par l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en vous assurant que le message suivant est écrit dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] au démarrage : « Utilisation de pages verrouillées pour le pool de mémoires tampons »

Ce message s’applique uniquement à SQL Server. Pour plus d’informations sur ce message dans le journal des erreurs, consultez les rubriques suivantes : [Dois-je attribuer le privilège Verrouiller les pages pour la mémoire dans le système local](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426)

Quand le système d’exploitation Windows pagine la mémoire du pool hors tampon, vous pourriez toujours rencontrer des problèmes de performances. Toutefois, les messages d’erreur mentionnés dans la section « Explication » ne sont pas consignés dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

## <a name="what-causes-sql-server-memory-to-be-paged-out"></a>Ce qui entraîne la pagination de la mémoire de SQL Server

Trois grandes catégories de causes peuvent être à l’origine de ce problème :

- Problèmes liés aux applications : Toutes les applications ont épuisé la mémoire physique disponible et le système d’exploitation doit libérer de la mémoire pour les nouvelles demandes d’application pour les ressources. En règle générale, l’approche consiste à rechercher les applications qui épuisent la mémoire et à prendre les mesures nécessaires pour équilibrer la mémoire entre elles sans conduire à l’épuisement de la RAM.
- Problèmes liés aux pilotes de périphérique : Les pilotes de périphériques peuvent provoquer la pagination de tous les processus de la plage de travail si le pilote appelle une fonction d’allocation mémoire de manière incorrecte.
- Problèmes liés au système d’exploitation

Vous trouverez ci-dessous des informations sur chacune de ces catégories

- **Problèmes liés aux applications** : Ensemble, les applications peuvent consommer toute la RAM du système. Si de nouvelles demandes de mémoire sont effectuées, le système d’exploitation tente de les satisfaire, et s’il n’y a pas de mémoire disponible, il supprime la plage de travail des applications en cours d’exécution pour répondre aux demandes de mémoire. Dans ce cas, vous pouvez observer que la plage de travail de la plupart des applications (voire toutes) chute de manière significative. Pour ce faire, collectez le compteur de l’analyseur de performances suivant pour toutes les applications sur le système :

  - Objet de performance : Process
  - Compteur : Plage de travail
  
  En outre, surveillez le compteur suivant pour mettre en corrélation la quantité de mémoire physique disponible sur le système.
  
  - Objet de performance : Mémoire
  - Compteur : Mémoire disponible (Mo)
  
  Le comportement classique que vous pouvez observer à la fois la réduction de la mémoire disponible à un niveau proche de 0 Mo et une chute soudaine des compteurs de la plage de travail pour la plupart des processus (voire tous) sur le système. Si vous observez un tel comportement, vous devrez peut-être prendre des mesures pour réduire l’utilisation de la mémoire sur le système, ce qui comprend, par exemple, la réduction de la mémoire maximale du serveur pour SQL Server.
  
    Les applications peuvent également trop utiliser le cache du système, ce qui peut entraîner une croissance importante du cache système. Pour répondre à la croissance de la mémoire cache du système, le système pagine la plage de travail du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d’autres applications. Si vous rencontrez ce problème, vous pouvez utiliser certaines fonctions de gestion de la mémoire dans l’application. Ces fonctions contrôlent l’espace de la mémoire cache du système que les opérations d’E/S de fichiers peuvent utiliser dans l’application. Par exemple, vous pouvez utiliser la fonction SetSystemFileCacheSize et la fonction GetSystemFileCacheSize pour contrôler l’espace de la mémoire cache du système que les opérations d’E/S de fichier peuvent utiliser.
  
    Vous pouvez utiliser l’objet de performance mémoire pour afficher les valeurs de différents compteurs dans cet objet afin de déterminer si la plage de travail du cache système utilise trop de mémoire. Par exemple, vous pouvez afficher les compteurs Octets du cache et Octets résidents du cache système. Pour plus d’informations sur cette rubrique, consultez :
  
    - [Cache trop grand](/archive/blogs/ntdebugging/too-much-cache)
    - [Service de cache dynamique Microsoft Windows](/archive/blogs/ntdebugging/microsoft-windows-dynamic-cache-service)
    - [Vous rencontrez des problèmes de performances dans les applications et les services lorsque le cache de fichiers système consomme la majeure partie de la RAM physique](https://support.microsoft.com/help/976618)
  
    Vous pouvez télécharger et déployer le « Service de cache dynamique Microsoft Windows » pour contrôler la mémoire consommée par le cache système.

- **Problèmes de pilote de périphérique** : Si un pilote de périphérique utilise la fonction `MmAllocateContiguousMemory` et qu’il définit la valeur du paramètre HighestAcceptableAddress sur une valeur inférieure à 4 gigaoctets (Go), le système d’exploitation Windows peut paginer la plage de travail des processus sur le système, y compris le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour résoudre ce problème, contactez le fournisseur du pilote de périphérique pour les mises à jour des pilotes.

    Lorsqu’un pilote de périphérique tente d’allouer de la mémoire, le système d’exploitation Windows peut paginer le jeu de travail d’autres applications. Ce correctif logiciel Windows vous permet d’utiliser le suivi des événements pour rechercher le pilote de périphérique à l’origine du problème. Pour obtenir plus d’informations sur le pilote spécifique qui provoque le comportement de réduction de la plage de travail, consultez [Identification des pilotes qui allouent de la mémoire contiguë](/previous-versions/windows/desktop/xperf/identifying-drivers-that-allocate-contiguous-memory).

- **Problèmes liés au système d’exploitation** : Pour résoudre les problèmes connus qui obligent le système d’exploitation Windows à paginer la plage de travail du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], appliquez les correctifs logiciels décrits dans les articles suivants de la base de connaissances Microsoft.

  > [!NOTE]
  > Les correctifs sont cumulatifs. Une version ultérieure d’un correctif contient les versions antérieures de ce correctif logiciel.

  - Le jeu [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut être tronqué lorsque le système utilise des fonctionnalités TCP avancées. Pour plus d’informations, consultez [Comment résoudre les problèmes des fonctionnalités avancées de performances réseau, comme RSS et NetDMA](/troubleshoot/windows-server/networking/troubleshoot-network-performance-features-rss-netdma).

  - Si vous exécutez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows Server 2008, vous devez appliquer des correctifs pour les problèmes connus qui peuvent entraîner une réduction de la plage de travail ou une consommation de mémoire excessive inutile par d’autres composants du système d’exploitation. Pour plus d’informations, consultez les articles suivants [Le processus de génération de rapports peut cesser de répondre lorsque vous exécutez Perfmon.exe avec le modèle Active Directory Diagnostics pour générer un rapport sur un contrôleur de domaine Windows Server 2008](/troubleshoot/windows-server/performance/report-generation-process-stops-responding).

  - Si vous exécutez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur Windows Server 2008 R2, vous devez appliquer des correctifs pour les problèmes connus pouvant entraîner une réduction de la plage de travail. Pour plus d’informations, consultez les articles suivants :

    - [Un ordinateur qui exécute Windows 7 ou Windows Server 2008 R2 ne répond pas lorsque vous exécutez une application de grande taille](https://support.microsoft.com/help/979149)
    - [Des performances médiocres se produisent sur un ordinateur disposant de processeurs NUMA et exécutant Windows Server 2008 R2 ou Windows 7 si un thread demande beaucoup de mémoire dans les 4 premiers Go de mémoire](https://support.microsoft.com/help/2155311)
    - [L’ordinateur fonctionne mal par intermittence ou cesse de répondre lorsque le pilote Storport est utilisé dans Windows Server 2008 R2](https://support.microsoft.com/help/2468345)

## <a name="important-considerations-before-you-assign-the-lock-pages-in-memory-user-right"></a>Considérations importantes avant d’attribuer le droit utilisateur « Verrouiller les pages en mémoire »

Vous devez prendre en compte d’autres éléments avant d’attribuer le droit utilisateur Verrouiller les pages en mémoire. Si vous affectez ce droit utilisateur sur des systèmes qui ne sont pas configurés correctement, le système peut devenir instable ou subir une baisse des performances de l’ensemble du système. En outre, l’ID d’événement 333 peut être enregistré dans le journal des événements.

Si vous contactez le support technique Microsoft pour résoudre ces problèmes, les ingénieurs CSS peuvent vous demander de révoquer ce droit utilisateur pour le compte d’utilisateur utilisé comme compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette étape peut être nécessaire pour collecter des données de performances importantes que les ingénieurs CSS peuvent utiliser pour la configuration nécessaire des différentes options de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et pour d’autres applications qui s’exécutent sur le système. Une fois que les ingénieurs CSS recueillent les données de performances, vous pouvez affecter le droit utilisateur Verrouiller les pages en mémoire au compte de démarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Avant d’attribuer le droit utilisateur Verrouiller les pages en mémoire, assurez-vous de capturer un journal de l’analyseur de performances pour déterminer les besoins en mémoire des divers services et applications installés sur le système. Ces applications incluent également SQL Server. Pour déterminer les besoins en mémoire, collectez les informations de ligne de base suivantes :

- Veillez à définir l’option max server memory et l’option min server memory correctement. Ces options reflètent uniquement les besoins en mémoire du pool de mémoires tampons du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces options n’incluent pas la mémoire allouée pour d’autres composants au sein du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Ces composants incluent :

  - Threads de travail [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  - Divers composants et DLL que le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] charge dans l’espace d’adressage du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  - Les opérations de sauvegarde et de restauration

- Les composants et DLL incluent différents fournisseurs OLE DB, des procédures stockées étendues, des objets Microsoft COM utilisés pour la procédure stockée sp_OACreate, les serveurs liés et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR. La mémoire allouée à ces composants se trouve sous la région du pool hors mémoire tampon de l’espace d’adressage du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour déterminer idéalement la quantité maximale de mémoire que l’ensemble du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser, vous devez soustraire la mémoire allouée aux composants qui n’utilisent pas le pool de mémoires tampons de la mémoire totale que vous voulez que le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise. Ensuite, vous pouvez utiliser la valeur restante pour définir l’option max server memory. Avant de définir l’option max server memory et l’option min server memory, vous devez examiner attentivement la rubrique « Paramétrage manuel des options de mémoire » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Déterminez la mémoire requise par les autres applications et les composants du système d’exploitation Windows. Les applications peuvent inclure d’autres composants [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple l’agent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les agents de réplication [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les services de rapport [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les services d’analyse [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les services d’intégration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et la recherche en texte intégral [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Les applications qui effectuent des opérations de sauvegarde et des opérations de copie de fichiers peuvent utiliser un grand nombre de mémoires. Envisagez des opérations telles que la copie en bloc et les agents d’instantanés qui génèrent des E/S de fichier. Vous devez prendre en compte les besoins en mémoire de toutes ces applications lorsque vous déterminez la valeur de l’option max server memory et de l’option min server memory. Vous pouvez utiliser le compteur Octets privés et le compteur Jeu de travail sous l’objet Processus pour chaque processus afin de déterminer les besoins en mémoire pour un processus spécifique.

- Par défaut, le droit utilisateur Verrouiller les pages en mémoire a déjà été affecté au compte système local intégré. Pour plus d’informations, consultez le site web Microsoft suivant : [Dois-je attribuer le privilège Verrouiller les pages pour la mémoire du système local ?](https://techcommunity.microsoft.com/t5/sql-server-support/do-i-have-to-assign-the-lock-pages-in-memory-privilege-for-local/ba-p/315426?advanced=false&collapse_discussion=true&search_type=thread)

- Si vous utilisez un compte d’utilisateur Windows globalement pour tous les processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’un domaine, déterminez les droits utilisateur affectés à l’aide d’une configuration de stratégie de groupe. Un processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 32 bits peut utiliser ce compte comme compte de démarrage. Toutefois, ce compte requiert le droit utilisateur Verrouiller les pages en mémoire pour activer la fonctionnalité `Address Windowing Extensions` (AWE). Pour plus d’informations, consultez la rubrique « Apport de la quantité maximale de mémoire à SQL Server » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

- Avant de configurer l’option max server memory et l’option min server memory pour plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], prenez en compte les besoins en mémoire du pool hors mémoire tampon pour chaque instance de SQL Server. Configurez ensuite ces options pour chaque instance de SQL Server.

Dans l’idéal, vous collectez ces informations de référence pendant les pics de charge. Par conséquent, vous pouvez déterminer les besoins en mémoire pour divers composants et applications afin de pouvoir soutenir la charge maximale. Les besoins en mémoire varient d’un système à un autre, et selon les activités et applications qui s’exécutent sur le système. Vous pouvez interroger les informations fournies dans la vue de gestion dynamique sys.dm_os_process_memory pour savoir si le système rencontre des conditions de mémoire insuffisante. Pour plus d’informations, consultez [sys.dm_os_process_memory (Transact-SQL)](/sql/relational-databases/system-dynamic-management-views/sys-dm-os-process-memory-transact-sql).

## <a name="improvements-added-in-windows-server-2008-and-r2-version"></a>Améliorations apportées à Windows Server 2008 et à la version R2

Windows Server 2008 et Windows Server 2008 R2 améliorent le mécanisme d’allocation de mémoire contiguë. Cette amélioration permet à Windows Server 2008 et Windows Server 2008 R2 de réduire dans une certaine mesure les effets de la pagination de la plage de travail des applications lorsque de nouvelles demandes de mémoire arrivent.

Vous trouverez ci-dessous une explication des améliorations apportées, tirées du livre blanc Microsoft « Évolutions de la gestion de la mémoire dans Windows » :

*Dans Windows Server 2008, l’allocation de mémoire physiquement contiguë a été grandement améliorée. Les demandes d’allocation de mémoire contiguë sont plus susceptibles d’avoir lieu, car le gestionnaire de mémoire remplace désormais les pages de manière dynamique, généralement sans tronquer la plage de travail ou effectuer d’opérations d’E/S. En outre, de nombreux autres types de pages, comme les piles de noyau et les pages de métadonnées du système de fichiers, sont désormais des candidats pour le remplacement. Par conséquent, une mémoire plus contiguë est généralement disponible à tout moment donné. En outre, le coût d’obtention de ces allocations est sensiblement réduit.*

Pour plus d’informations, consultez [Problèmes de réduction de plage de travail avec SQL Server](https://techcommunity.microsoft.com/t5/sql-server-support/sql-server-working-set-trim-problems-consider/ba-p/315462?advanced=false&collapse_discussion=true&search_type=thread).

Les produits tiers mentionnés dans cet article sont fabriqués par des sociétés indépendantes de Microsoft. Microsoft ne formule aucune garantie, expresse ou implicite, concernant les performances ou la fiabilité de ces produits.
