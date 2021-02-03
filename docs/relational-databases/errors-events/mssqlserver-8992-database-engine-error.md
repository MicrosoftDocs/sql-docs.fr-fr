---
description: MSSQLSERVER_8992
title: MSSQLSERVER_8992 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8992 (Database Engine error)
ms.assetid: 68467e6a-09d8-478f-8bd9-3bb09453ada3
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: fa54a631a021dc07728490dde490a326901fa4b1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194024"
---
# <a name="mssqlserver_8992"></a>MSSQLSERVER_8992
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
|Élément|Valeur|
|:---|:---|
|Nom du produit|SQL Server|  
|ID de l’événement|8992|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|DBCC3_CHECK_CATALOG|  
|Texte du message|Contrôle du message du catalogue ERROR, niveau LEVEL, état STATE : MESSAGE.|  

> [!NOTE]
> Le message d’erreur 8992 fait référence à un autre message spécifique (numéro compris entre 3851 et 3858) portant sur l’incohérence réelle.

## <a name="explanation"></a>Explication  
DBCC CHECKCATALOG ou DBCC CHECKDB a trouvé une incohérence dans les tables de métadonnées système pour l'objet spécifié. En d'autres termes, il y a une incohérence entre l'ID d'objet enregistré et l'objet spécifié dans le message d'erreur.  
  
Cette erreur peut se produire lorsqu'une ou plusieurs tables système ont été mises à jour manuellement de telle sorte qu'il existe une incohérence dans les métadonnées système. Par exemple, un utilisateur a pu supprimer manuellement un objet de la table **sysobjects** sans supprimer les lignes associées dans d’autres tables telles que **sysindexes** et **syscolumns**.  
  
Cette erreur peut se produire lors de l'exécution de DBCC CHECKDB sur une base de données mise à niveau de SQL Server 2000 vers SQL Server 2005 ou version ultérieure. Dans SQL Server 2000, DBCC CHECKDB n'incluait pas la fonctionnalité DBCC CHECKCATALOG ; par conséquent, l'erreur ne peut pas être interceptée avant la mise à niveau à moins que DBCC CHECKCATALOG n'ait été spécifiquement exécuté sur la base de données dans SQL Server 2000.  
  
Vous pouvez voir s'afficher les erreurs suivantes conjointement avec l'erreur 8992 :  

|ID de message|Texte du message|
|:---|:---|
|3851|Une ligne non valide (%ls) a été détectée dans la table système sys.%ls%ls.|
|3852|La ligne (%ls) de sys.%ls%ls ne correspond à aucune ligne (%ls) dans sys.%ls%ls.|
|3853|L'attribut (%ls) de la ligne (%ls) dans sys.%ls%ls ne correspond à aucune ligne (%ls) dans sys.%ls%ls.|
|3854|L'attribut (%ls) de la ligne (%ls) dans sys.%ls%ls correspond à une ligne (%ls) dans sys.%ls%ls qui est incorrecte.|
|3855|L'attribut (%ls) figure sans ligne (%ls) dans sys.%3ls%ls.|
|3856|L'attribut (%ls) existe alors qu'il ne le devrait pas pour la ligne (%ls) dans sys.%ls%4ls.|
|3857|L'attribut (%ls) est obligatoire mais il n'existe pas pour la ligne (%ls) dans sys.%ls%ls.|
|3858|L'attribut (%ls) de la ligne (%ls) dans sys.%ls%ls a une valeur non valide.|

## <a name="user-action"></a>Action de l'utilisateur  
  
### <a name="drop-and-re-create-the-specified-object"></a>Supprimer et recréer l'objet spécifié  
Si possible, supprimez et recréez l'objet spécifié. Par exemple, si l'objet est une procédure stockée ou un type défini par l'utilisateur, le fait de recréer l'objet peut résoudre le problème.  
  
### <a name="restore-from-backup"></a>Restaurer à partir de la sauvegarde  
Si le problème n'est pas matériel et si une restauration réputée en bon état est disponible, restaurez la base de données à partir de la sauvegarde. Cette action est applicable uniquement si la sauvegarde ne contient pas l'erreur de métadonnées.  
  
### <a name="export-the-data-to-a-new-database"></a>Exporter les données vers une nouvelle base de données  
Si la sauvegarde contient également une incohérence des métadonnées, vous devez créer une base de données et exporter le contenu de la base de données existante vers la nouvelle base de données.  
  
### <a name="dbcc-checkdb-cannot-repair-this-error"></a>DBCC CHECKDB ne peut pas résoudre cette erreur.  
Cette erreur ne peut pas être corrigée.  Si vous ne pouvez pas restaurer la base de données à partir d'une sauvegarde, contactez le service clientèle et le support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
### <a name="do-not-manually-update-system-tables"></a>Ne pas mettre à jour manuellement les tables système  

N'apportez pas de mises à jour manuelles aux tables système. SQL Server ne prend pas en charge les modifications manuelles apportées aux bases de données système. Si vous mettez à jour une table système dans une base de données SQL Server, les événements suivants sont journalisés :

#### <a name="when-a-system-table-is-manually-updated"></a>Quand une table système est mise à jour manuellement

Msg 17659 : Avertissement : l’ID de table système <id> a été mis à jour directement dans l’ID de base de données <id> et la cohérence du cache n’a peut-être pas été préservée. SQL Server doit être redémarré.

#### <a name="starting-a-database-with-a-system-table-that-was-manually-updated"></a>Démarrage d’une base de données avec une table système mise à jour manuellement

Msg 3859 : Avertissement : le catalogue système a été mis à jour directement dans l’ID de base de données <id>, le plus récemment à date_heure.

#### <a name="when-you-execute-the-dbcc_checkdb-command-after-a-system-table-is-manually-updated"></a>Quand vous exécutez la commande DBCC_CHECKDB après la mise à jour manuelle d’une table système

Msg 3859 : Avertissement : le catalogue système a été mis à jour directement dans l’ID de base de données <id>, le plus récemment à date_heure.  

## <a name="see-also"></a>Voir aussi

[Tables de base système](../system-tables/system-base-tables.md)
  
