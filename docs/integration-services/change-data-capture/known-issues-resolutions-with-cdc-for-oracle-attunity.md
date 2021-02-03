---
description: Erreurs connues et résolutions avec Change Data Capture pour Oracle d’Attunity
title: Erreurs connues et résolutions avec Change Data Capture pour Oracle d’Attunity | Microsoft Docs
ms.date: 07/23/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e89a3638dc304bdda8bc11c55897975fb006e3ca
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99076837"
---
# <a name="known-errors-and-resolutions-with-change-data-capture-for-oracle-by-attunity"></a>Erreurs connues et résolutions avec Change Data Capture pour Oracle d’Attunity
[!INCLUDE[tsql-appliesto-ss2008-asdbmi-xxxx-xxx-md](../../includes/tsql-appliesto-ss2012-asdbmi-xxxx-xxx-md.md)]

Cette rubrique liste les principaux problèmes et les résolutions connues lors de l’affichage d’une instance Change Data Capture (CDC) dans l’outil de configuration Oracle CDC Designer. Cet outil fait partie de Change Data Capture pour Oracle d’Attunity, qui est inclus à compter de SQL Server 2012. 

## <a name="bug-fixes"></a>Résolution des bogues
Avant de consacrer trop de temps à la résolution des problèmes, il est important d’utiliser les dernières versions de CDC pour Oracle d’Attunity pour éviter des problèmes connus tels que ceux-ci :

### <a name="sql-server-2017"></a>SQL Server 2017

La **version 5.0.0.111** contient les correctifs suivants :
- Résolution de bogue : Oracle CDC Designer échoue avec une erreur « Syntaxe incorrecte près du mot clé ’KEY’ » lors de l’ajout d’une table Oracle. 
- Amélioration : Prise en charge améliorée de RAC, qui comprend une meilleure gestion quand un nœud RAC est redémarré. 
- Résolution de bogue : Le CDC ne fonctionne pas avec Oracle 10.2 en raison de la demande de NEXT_CHANGE# à partir de v$log. 


La **version 5.0.0.93** contient les correctifs suivants : 
- Microsoft CDC Designer pour Oracle par d’Attunity échoue avec l’erreur « Syntaxe incorrecte près du mot ’KEY’ » lors de l’ajout d’une table Oracle. 

 
### <a name="sql-server-2016"></a>SQL Server 2016

La **version 4.0.107** contient les correctifs suivants :
- Résolutions de bogue : Oracle CDC Designer échoue avec l’erreur « Syntaxe incorrecte près du mot ’KEY’ » lors de l’ajout d’une table Oracle.
- Amélioration : Prise en charge améliorée de RAC, qui comprend une meilleure gestion quand un nœud RAC est redémarré.
- Résolutions de bogue : Le CDC ne fonctionne pas avec Oracle 10.2 en raison de la demande de NEXT_CHANGE# à partir de v$log.

La **version 4.0.0.95** contient les correctifs suivants : 
- Résolutions de bogue : Oracle CDC Designer échoue avec l’erreur « Syntaxe incorrecte près du mot ’KEY’ » lors de l’ajout d’une table Oracle.

La **version 4.0.0.88** contient les correctifs suivants :
-  Les propriétés ajoutées dans les options avancées de l’instance CDC Attunity sont supprimées lors de l’ajout ou de la suppression d’une table dans CDC. 
- CDC Attunity cesse de fonctionner après l’application du correctif SQL qui ajoute la colonne __$command_id

### <a name="sql-server-2014"></a>SQL Server 2014 

La **version 2.0.0.114** contient les correctifs suivants :
- Résolutions de bogue : Oracle CDC Designer échoue avec l’erreur « Syntaxe incorrecte près du mot ’KEY’ » lors de l’ajout d’une table Oracle.
- Amélioration : Prise en charge améliorée de RAC, qui comprend une meilleure gestion quand un nœud RAC est redémarré.
- Résolutions de bogue : Le CDC ne fonctionne pas avec Oracle 10.2 en raison de la demande de NEXT_CHANGE# à partir de v$log.

La **version 2.0.0.92** contient les correctifs suivants : 
- Les propriétés ajoutées dans les options avancées de l’instance CDC Attunity sont supprimées lors de l’ajout ou de la suppression d’une table dans CDC. CDC Attunity cesse de fonctionner après l’application du correctif SQL qui ajoute la colonne __$command_id
- Échec de la validation des métadonnées pour la table Oracle CDC.nom_table. L’index de la colonne nom_colonne est hors limites.  Et le problème suivant : Le service Oracle CDC montre un état Abandonné quand vous utilisez CDC pour Oracle d’Attunity
    - Correction dans _Mise à jour cumulative 1 pour SQL Server 2014 RTM_, comme décrit dans KB [2894025](https://support.microsoft.com/kb/2894025).
- Certaines modifications sont ignorées et ne sont pas répliquées dans la base de données SQL Server. Ce problème se produit quand une table contient plusieurs objets CLOB et que l’un de ces objets a une valeur élevée. 
    - Correction dans _Mise à jour cumulative 1 pour SQL Server 2014 SP1_ et _Mise à jour cumulative 8 pour SQL Server 2014 RTM_ comme décrit dans KB [3029096](https://support.microsoft.com/kb/3029096). 
- CDC pour Oracle d’Attunity cesse de fonctionner quand des tables Oracle ont une colonne avec un type de données Long.
    - Correction dans _Mise à jour cumulative 5 pour SQL Server 2014 SP1_ et _Mise à jour cumulative 12 pour SQL 2014 RTM_ comme décrit dans [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d).

### <a name="sql-server-2012"></a>SQL Server 2012

La **version 1.1.0.102** contient les correctifs suivants : 
 
- Les propriétés ajoutées dans les options avancées de l’instance CDC Attunity sont supprimées lors de l’ajout ou de la suppression d’une table dans CDC. CDC Attunity cesse de fonctionner après l’application du correctif SQL qui ajoute la colonne __$command_id
- L’instance CDC pour Oracle se bloque quand vous la démarrez et ne capture pas les modifications. La mémoire occupée par le serveur Oracle peut augmenter jusqu’à ce qu’il manque de mémoire ou se plante.
- [2672759](https://support.microsoft.com/kb/2672759) : Message d’erreur quand vous utilisez le service Microsoft CDC pour Oracle d’Attunity : « ORA-00600 : code d’erreur interne ». Ajoutez le suivi de niveau SOURCE et vérifiez si vous recevez la même erreur ORA-00600. Résolu via le téléchargement d’un correctif Oracle.
- Partitions multiples
    - Quand vous utilisez plus de 10 partitions sur une table Oracle, l’instance CDC ne peut pas capturer toutes les modifications pour la table. Quand la table Oracle est définie avec plus de 10 partitions, les modifications sont capturées seulement pour les 10 dernières partitions. Correction dans la version _Service Pack 1 pour SQL Server 2012_. Consultez la [page de téléchargement du Feature Pack SP1](https://www.microsoft.com/download/details.aspx?id=35575). 
- Les modifications sont perdues
    - La capture d’événements peut entrer dans une boucle infinie et arrêter la capture des nouvelles modifications des données (liées au bogue Oracle 5623813). Quand il est activé, l’environnement RAC effectue un arrêt ou une reprise de l’instance CDC, les modifications peuvent être ignorées/perdues. Cela signifie que des lignes importantes vont manquer dans la capture des changements de données de SQL Server, ce qui représente une perte de données dans l’entrepôt de données ou le système abonné. Correction dans la version _Service Pack 1 pour SQL Server 2012_. Consultez la [page de téléchargement du Feature Pack SP1](https://www.microsoft.com/download/details.aspx?id=35575).
- Doublement des largeurs sur des colonnes dans SQL
    - Lors de la création d’une instance CDC pour Oracle, dans les scripts à exécuter sur SQL Server, la longueur d’une colonne de type de données de largeur variable est doublée dans les tables SQL Server créées dans le script. Par exemple, si vous essayez d’effectuer le suivi des modifications sur une colonne VARCHAR2(10) dans une table Oracle, la colonne correspondante dans la table SQL Server est NVARCHAR(20) dans le script de déploiement. Correction dans _Mise à jour cumulative 2 pour SQL Server 2012 SP1_ ou _Mise à jour cumulative 5 pour SQL Server 2012 RTM_ comme décrit dans KB [2769673](https://support.microsoft.com/kb/2769673). 
- Les données des instructions DDL sont tronquées
    - Quand vous exécutez une instruction DDL (Data Definition Language) qui dépasse 4 000 octets sur une base de données Oracle qui contient des caractères non latins, CDC pour Oracle d’Attunity échoue. En outre, vous voyez le message d’erreur `ORA-01406: fetched column value was truncated.`. Correction dans _Mise à jour cumulative 4 pour SQL Server 2012 SP1_, comme décrit dans KB [2839806](https://support.microsoft.com/kb/2839806). 
- Les modifications sont perdues dans les deux dernières colonnes
    - Correction dans _Mise à jour cumulative 6 pour SQL Server 2012 SP1_, comme décrit dans KB [2874879](https://support.microsoft.com/kb/2874879). 
- La taille du journal des transactions SQL augmente quand vous utilisez CDC pour Oracle
     - Quand des instances CDC pour Oracle sont configurées, la base de données SQL qui reçoit les données modifiées a des tables mises en miroir, avec des transactions marquées pour la réplication. Ce comportement se produit car CDC pour Oracle s’appuie sur des procédures stockées système sous-jacentes qui ressemblent à celles utilisées dans CDC pour SQL Server. Cependant, comme aucune réplication CDC SQL n’est impliquée quand CDC pour Oracle est utilisée seule, il n’existe aucun lecteur de journal pour effacer les transactions marquées pour la réplication. Comme la transaction ne doit pas être répliquée dans SQL Server, vous pouvez sans problème marquer manuellement la transaction comme étant distribuée en utilisant la solution de contournement décrite plus loin dans cet article. Vous trouverez plus d’informations dans KB [2871474](https://support.microsoft.com/kb/2871474). 
- Erreur `ORACDC000T: Error encountered at position to change event - SCN not found - EOF simulated`. Correction dans _Mise à jour cumulative 7 pour SQL Server 2012 SP1_, comme décrit dans KB [2883524](https://support.microsoft.com/kb/2883524). 
- Échec de la validation des métadonnées pour la table Oracle CDC.nom_table. L’index de la colonne nom_colonne est hors limites. Correction dans _Mise à jour cumulative 7 pour SQL Server 2012 SP1_, comme décrit dans KB [2883524](https://support.microsoft.com/kb/2883524).
- Le service Oracle CDC montre un état Abandonné quand vous utilisez CDC pour Oracle d’Attunity dans SQL Server 2012. Correction dans _Mise à jour cumulative 8 pour SQL Server 2012 SP1_, comme décrit dans KB [2923839](https://support.microsoft.com/kb/2923839).  
- Certaines modifications sont ignorées et ne sont pas répliquées dans les bases de données SQL Server. Ce problème se produit quand une table contient plusieurs objets CLOB et que l’un de ces objets a une valeur élevée. Correction dans _Mise à jour cumulative 8 pour SQL Server 2012 SP1_, comme décrit dans KB [2923839](https://support.microsoft.com/kb/2923839).   
- CDC pour Oracle d’Attunity cesse de fonctionner quand des tables Oracle ont une colonne avec un type de données Long. Correction dans _Mise à jour cumulative 2 pour SQL Server 2012 SP3_ et _Mise à jour cumulative 11 pour SQL 2012 SP2_ comme décrit dans [KB4017793](https://support.microsoft.com/topic/kb4017793-cumulative-update-12-for-sql-server-2014-sp1-bc24907b-afe2-6703-ccd9-99d6cb76a74d). 

## <a name="collect-detailed-logs"></a>Collecter des journaux détaillés 

Cette section explique comment collecter des erreurs et des événements à partir de l’instance CDC. 

### <a name="management-console"></a>Console de gestion

Vous pouvez voir les erreurs dans les messages **État** qui apparaissent dans la console de gestion d’Oracle CDC Designer quand une instance CDC est mise en surbrillance dans le volet gauche. 

### <a name="query-trace-table"></a>Interroger la table de suivi

Vous pouvez interroger la table de trace dans la base de données CDC dans SQL Server pour voir les erreurs journalisées. 

### <a name="save-output-from-basic-logging"></a>Enregistrer la sortie de la journalisation de base 

Pour capturer les diagnostics, sélectionnez **Collect Diagnostics** (Collecter les diagnostics) sur l’onglet d’état de la console de gestion d’Oracle CDC. 

![Capture d’écran montrant l’onglet Status (État) dans la console de gestion Oracle Change Data Capture avec l’option Collect Diagnostics (Collecter les diagnostics) mise en évidence.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

Choisissez une heure de début et sélectionnez un emplacement pour le fichier journal. Sélectionnez ensuite **Create** (Créer) pour démarrer la collecte des diagnostics. 

![Capture d’écran de la boîte de dialogue Collect Diagnostics for testTA (Collecter les diagnostics pour testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/start-diagnostics.png)

### <a name="detailed-errors"></a>Erreurs détaillées

Vous pouvez augmenter le niveau de suivi collecté par l’instance et répéter le scénario pour obtenir une journalisation plus détaillée. Pour cela, sélectionnez **Properties** (Propriétés) sous **Actions**, puis ajoutez une nouvelle propriété dans la grille **Advanced Settings** (Paramètres avancés) de l’onglet **Advanced** (Avancé). Définissez le nom de la propriété sur `trace`, puis définissez la valeur sur `SOURCE`. 

![Capture d’écran montrant l’option Properties (Propriétés) sous Actions.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/properties.png)

Reproduisez l’erreur, puis sélectionnez l’option **Collect Diagnostics** (Collecter les diagnostics) pour collecter les journaux. 

![Autre capture d’écran montrant l’onglet Status (État) dans la console de gestion Oracle Change Data Capture avec l’option Collect Diagnostics (Collecter les diagnostics) mise en évidence.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/collect-diagnostics.png)

## <a name="ora-00942-table-of-view-does-not-exist"></a>ORA-00942 La table de vue n’existe pas 

Il s’agit d’une erreur courante affichée dans le champ **Status** (État) du message de l’instance CDC. L’instance fait de nombreuses nouvelles tentatives : l’icône d’état passe donc momentanément au vert, mais elle finit par échouer avec le point d’exclamation rouge et l’état UNEXPECTED (Inattendu). 

![Capture d’écran d’une erreur courante affichée dans le champ Status (État) du message de l’instance CDC.](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-error.png)

```
"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC508E:Oracle method OCIStmtExecute failed with error: ORA-00942: table or view does not exist ","source","",""

"ERROR","computername","RUNNING","IDLE",
"ORACDC518E:Failed to verify archive log mode.","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC517E:Oracle Call Interface (OCI) method failed: ORA-00942: table or view does not exist .","source","",""

"ERROR","computername","ERROR","UNEXPECTED",
"ORACDC414E:The Engine component failed with return code 1.","engine","",""
```

Ceci se produit quand le compte Oracle qui se connecte à partir de l’instance CDC au serveur Oracle n’a pas l’autorisation de consulter les vues du journal système. 

### <a name="resolution"></a>Résolution

Pour résoudre cette erreur, accordez les autorisations appropriées à l’utilisateur actuellement configuré dans le système de base de données Oracle ou changez le compte utilisé pour se connecter au serveur Oracle à partir de l’instance CDC. 

La liste de toutes les autorisations nécessaires est détaillée dans le fichier d’aide inclus dans le dossier des fichiers du programme d’installation `C:\Program Files\Change Data Capture for Oracle by Attunity\Attunity.SqlServer.XdbCdcDesigner.chm`.  Consultez la page intitulée « Se connecter à une base de données source Oracle » dans le fichier .chm pour obtenir la liste complète.

Vous pouvez définir le compte d’utilisateur en sélectionnant l’instance CDC dans le volet gauche, puis en sélectionnant le bouton Properties (Propriétés) dans le volet Actions le plus à droite de la fenêtre **CDC Designer**. Vous pouvez changer le compte d’authentification de l’exploration des journaux Oracle dans la page Properties (Propriétés).

![Capture d’écran montrant l’onglet Oracle de la boîte de dialogue testTA Properties (Propriétés de testTA).](media/known-issues-resolutions-with-cdc-for-oracle-attunity/oracle-connection.png)


  
## <a name="see-also"></a>Voir aussi  
 [Suivi des modifications de données &#40;SQL Server&#41;](../../relational-databases/track-changes/track-data-changes-sql-server.md)   
 [À propos de la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)   
 [Utiliser les données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/work-with-change-data-sql-server.md)   
 [Administrer et surveiller la capture de données modifiées &#40;SQL Server&#41;](../../relational-databases/track-changes/administer-and-monitor-change-data-capture-sql-server.md)  
  
  
