---
description: Tables de base système
title: Tables de base système | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
helpviewer_keywords:
- system base tables [SQL Server]
- hobt [SQL Server]
- base tables
ms.assetid: 31f2df90-651f-4699-8067-19f59b60904f
author: cawrites
ms.author: chadam
ms.openlocfilehash: c5805fd84eb9bce1092cdfae59a9711c2d63edb8
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98102062"
---
# <a name="system-base-tables"></a>Tables de base système
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Les tables de base système sont des tables sous-jacentes qui stockent les métadonnées pour une base de données spécifique. La base de données **Master** est spéciale dans ce cas, car elle contient des tables supplémentaires qui sont introuvables dans les autres bases de données. Ces tables contiennent des métadonnées persistantes dont l'étendue couvre le serveur.  
  
> [!IMPORTANT]  
>  Les tables de base système sont uniquement utilisées dans [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et ne sont pas destinées à un usage général. Elles peuvent faire l'objet de modifications et la compatibilité n'est pas garantie.  
  
## <a name="system-base-table-metadata"></a>Métadonnées des tables de base système  
 Un bénéficiaire qui dispose de l’autorisation CONTROL, ALTER ou VIEW DEFINITION sur une base de données peut voir les métadonnées de la table de base système dans l’affichage catalogue **sys. Objects** . Le bénéficiaire peut également résoudre les noms et les ID d’objet des tables de base système en utilisant des fonctions intégrées telles que [object_name](../../t-sql/functions/object-name-transact-sql.md) et [object_id](../../t-sql/functions/object-id-transact-sql.md).  
  
 Pour créer une liaison avec une table de base système, un utilisateur doit se connecter à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l'aide de la connexion administrateur dédiée. Une tentative d'exécution d'une requête SELECT à partir d'une table de base système sans connexion via la DAC provoque une erreur.  
  
> [!IMPORTANT]  
>  L'accès aux tables de base système via la DAC est conçu uniquement pour le personnel [!INCLUDE[msCoName](../../includes/msconame-md.md)] et n'est pas un scénario client pris en charge.  
  
## <a name="system-base-tables"></a>Tables de base système  
 Le tableau suivant répertorie et décrit l'ensemble de chaque table de base système de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
|Table de base|Description|  
|----------------|-----------------|  
|**sys.sysschobjs**|Existe dans toutes les bases de données. Chaque ligne représente un objet de la base de données.|  
|**sys.sysbinobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité de Service Broker dans la base de données. Les entités de Service Broker incluent les éléments suivants :<br /><br /> type de message<br /><br /> contrat de service<br /><br /> Service<br /><br /> Les noms et les types utilisent un classement binaire fixe.|  
|**sys.sysclsobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité classifiée qui partage les mêmes propriétés communes qui incluent les éléments suivants :<br /><br /> Assembly<br /><br /> unité de sauvegarde<br /><br /> Catalogue de texte intégral<br /><br /> Fonction de partition<br /><br /> Schéma de partition<br /><br /> Groupe de fichiers<br /><br /> clé d'obfuscation|  
|**sys.sysnsobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque entité de l'étendue de l'espace de noms. Cette table est utilisée pour le stockage des entités de collection XML.|  
|**sys.syscolpars**|Existe dans toutes les bases de données. Contient une ligne pour chaque colonne de table, chaque vue ou chaque fonction table. Contient également des lignes pour chaque paramètre d'une procédure ou d'une fonction.|  
|**sys.systypedsubobjs**|Existe dans toutes les bases de données. Contient une ligne pour chaque sous-entité typée. Seuls les paramètres de la fonction de partition appartiennent à cette catégorie.|  
|**sys.sysidxstats**|Existe dans toutes les bases de données. Contient une ligne pour chaque index ou statistique pour les tables et les vues indexées<br /><br /> Remarque : chaque index (à l’exception du segment de mémoire) est associé à une statistique qui porte le même nom que l’index.|  
|**sys.sysiscols**|Existe dans toutes les bases de données. Contient une ligne pour chaque colonne d'index et de statistiques persistante.|  
|**sys.sysscalartypes**|Existe dans toutes les bases de données. Contient une ligne pour chaque type défini par l'utilisateur ou chaque type système.|  
|**sys.sysdbreg**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque base de données inscrite.|  
|**sys.sysxsrvs**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque serveur local, lié ou distant.|  
|**sys.sysrmtlgns**|Cette table de base système existe uniquement dans la base de données **Master** . Contient une ligne pour chaque mappage d'ouverture de session distante. Cela est utilisé pour mapper les connexions entrantes issues d'un serveur correspondant et accédant à une connexion locale.|  
|**sys.syslnklgns**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque mappage de connexion liée. Les mappages de connexions liées sont utilisés par des appels de procédure distante et par des requêtes distribuées qui émanent d'un serveur local vers un serveur lié correspondant.|  
|**sys.sysxlgns**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque principal de serveur.|  
|**sys.sysdbfiles**|Existe dans toutes les bases de données. Si la colonne **dbid** est égale à zéro, la ligne représente un fichier qui appartient à cette base de données. Dans la base de données **Master** , la colonne **dbid** peut être différente de zéro. Lorsque c'est le cas, la ligne représente un fichier maître.|  
|**sys.sysusermsg**|Existe uniquement dans la base de données **Master** . Chaque ligne représente un message d'erreur défini par l'utilisateur.|  
|**sys.sysprivs**|Existe dans toutes les bases de données. Contient une ligne pour chaque autorisation de niveau base de données ou serveur.<br /><br /> Remarque : les autorisations au niveau du serveur sont stockées dans la base de données **Master** .|  
|**sys.sysowners**|Existe dans toutes les bases de données. Chaque ligne représente un principal de base de données.|  
|**sys.sysobjkeycrypts**|Existe dans toutes les bases de données. Contient une ligne pour chaque clé symétrique, chaque chiffrement ou chaque propriété de chiffrement associé à un objet.|  
|**sys.syscerts**|Existe dans toutes les bases de données. Contient une ligne pour chaque certificat dans une base de données.|  
|**sys.sysasymkeys**|Existe dans toutes les bases de données. Chaque ligne représente une clé asymétrique.|  
|**sys.ftinds**|Existe dans toutes les bases de données. Contient une ligne pour chaque index de texte intégral dans la base de données.|  
|**sys.sysxprops**|Existe dans toutes les bases de données. Contient une ligne pour chaque propriété étendue.|  
|**sys.sysallocunits**|Existe dans toutes les bases de données. Contient une ligne pour chaque unité d'allocation de stockage.|  
|**sys.sysrowsets**|Existe dans toutes les bases de données. Contient une ligne pour chaque ensemble de lignes de partition pour un index ou un segment.|  
|**sys.sysrowsetrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence d'index à un ensemble de lignes.|  
|**sys.syslogshippers**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque témoin de mise en miroir de bases de données.|  
|**sys.sysremsvcbinds**|Existe dans toutes les bases de données. Contient une ligne pour chaque liaison de service distant.|  
|**sys.sysconvgroup**|Existe dans toutes les bases de données. Contient une ligne pour chaque instance de service dans Service Broker.|  
|**sys.sysxmitqueue**|Existe dans toutes les bases de données. Contient une ligne pour chaque file d'attente de transmission de Service Broker.|  
|**sys.sysdesend**|Existe dans toutes les bases de données. Contient une ligne pour chaque point de terminaison d'envoi d'une conversation Service Broker.|  
|**sys.sysdercv**|Existe dans toutes les bases de données. Contient une ligne pour chaque point de terminaison récepteur d'une conversation Service Broker.|  
|**sys.sysendpts**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque point de terminaison créé dans le serveur.|  
|**sys.syswebmethods**|Existe uniquement dans la base de données **Master** . Contient une ligne pour chaque méthode SOAP définie sur un point de terminaison HTTP SOAP créé sur le serveur.|  
|**sys.sysqnames**|Existe dans toutes les bases de données. Contient une ligne pour chaque espace de noms ou chaque nom qualifié à un jeton d'ID de 4 octets.|  
|**sys.sysxmlcomponent**|Existe dans toutes les bases de données. Chaque ligne représente un composant de schéma XML.|  
|**sys.sysxmlfacet**|Existe dans toutes les bases de données. Contient une ligne pour chaque facette (restriction) XML d'une définition de type XML.|  
|**sys.sysxmlplacement**|Existe dans toutes les bases de données. Contient une ligne pour chaque emplacement XML pour les composants XML.|  
|**sys.syssingleobjrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence générale de N à 1.|  
|**sys.sysmultiobjrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence générale de N à N.|  
|**sys.sysobjvalues**|Existe dans toutes les bases de données. Contient une ligne pour chaque propriété de valeur générale d'une entité.|  
|**sys.sysguidrefs**|Existe dans toutes les bases de données. Contient une ligne pour chaque référence d'ID classifiée GUID.|  
  
## <a name="updating-system-base-tables"></a>Mise à jour des tables de base système    
Vous pouvez afficher les données des tables système à l’aide des vues du catalogue système. Pour mettre à jour les métadonnées dans une table de base système, utilisez l’interface TSQL appropriée (par exemple, les instructions DDL). Vous ne pouvez pas mettre à jour manuellement les tables système. SQL Server signale les messages suivants lorsque vous effectuez des mises à jour directes sur des tables système.

### <a name="a-system-table-is-manually-updated"></a>Une table système est mise à jour manuellement
Msg 17659 : Avertissement : l’ID de table système <id> a été mis à jour directement dans l’ID de base de données <id> et la cohérence du cache n’a peut-être pas été préservée. SQL Server doit être redémarré.

### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Démarrage d’une base de données avec une table système mise à jour manuellement
MSG 3859 : AVERTISSEMENT : le catalogue système a été mis à jour directement dans l’ID de base de données 17, le plus récemment à date_time.

### <a name="executing-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Exécution de la commande DBCC_CHECKDB après la mise à jour manuelle d’une table système
MSG 3859 : AVERTISSEMENT : le catalogue système a été mis à jour directement dans l’ID de base de données 17, le plus récemment à date_time.

Si vous effectuez des mises à jour manuelles vers une table système et que vous rencontrez un problème, vous pouvez être invité à effectuer une restauration à partir d’une sauvegarde ou à copier les données de la base de données affectée vers une nouvelle base de données. En savoir plus sur [les messages d’erreur](../errors-events/mssqlserver-8992-database-engine-error.md#user-action)relatifs aux actions de l’utilisateur.