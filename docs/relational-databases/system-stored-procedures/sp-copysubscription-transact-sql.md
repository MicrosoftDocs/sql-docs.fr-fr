---
description: sp_copysubscription (Transact-SQL)
title: sp_copysubscription (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: reference
f1_keywords:
- sp_copysubscription
- sp_copysubscription_TSQL
helpviewer_keywords:
- sp_copysubscription
ms.assetid: 3c56cd62-2966-4e87-a986-44cb3fd0b760
author: markingmyname
ms.author: maghan
ms.openlocfilehash: b7377048df6d32715b8f91e26b5b21617c22a0d1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192021"
---
# <a name="sp_copysubscription-transact-sql"></a>sp_copysubscription (Transact-SQL)
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

    
> [!IMPORTANT]  
>  La fonctionnalité concernant les abonnements pouvant être attachés est déconseillée et sera retirée dans une version ultérieure. Elle ne doit pas être utilisée dans tout nouveau travail de développement. Dans le cas des publications de fusion partitionnées par le biais de filtres paramétrés, nous vous recommandons d'utiliser plutôt les nouvelles fonctionnalités d'instantanés partitionnés qui simplifient l'initialisation de larges volumes d'abonnements. Pour plus d'informations, voir [Snapshots for Merge Publications with Parameterized Filters](../../relational-databases/replication/create-a-snapshot-for-a-merge-publication-with-parameterized-filters.md). Dans le cas de publications qui ne sont pas partitionnées, vous pouvez initialiser un abonnement par le biais d'une sauvegarde. Pour plus d’informations, consultez [Initialiser un abonnement transactionnel sans instantané](../../relational-databases/replication/initialize-a-transactional-subscription-without-a-snapshot.md).  
  
 Copie une base de données d'abonnement contenant des abonnements par extraction de données (pull) mais pas d'abonnements par envoi de données (push). Seules les bases de données monofichier peuvent être copiées. Cette procédure stockée est exécutée sur la base de données d'abonnement de l'Abonné.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_copysubscription [ @filename = ] 'file_name'  
    [ , [ @temp_dir = ] 'temp_dir' ]  
    [ , [ @overwrite_existing_file = ] overwrite_existing_file]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @filename = ] 'file_name'` Chaîne qui spécifie le chemin d’accès complet, y compris le nom de fichier, vers lequel une copie du fichier de données (. mdf) est enregistrée. le *nom de fichier* est de type **nvarchar (260)**, sans valeur par défaut.  
  
`[ @temp_dir = ] 'temp_dir'` Nom du répertoire qui contient les fichiers temporaires. *temp_dir* est de type **nvarchar (260)**, avec NULL comme valeur par défaut. Si la valeur est NULL, le [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Répertoire de données par défaut sera utilisé. Le répertoire doit contenir suffisamment d'espace pour stocker un fichier d'une taille équivalente à celle de tous les fichiers de bases de données d'abonnés réunis.  
  
`[ @overwrite_existing_file = ] 'overwrite_existing_file'`Indicateur booléen facultatif qui spécifie s’il faut ou non remplacer un fichier existant portant le même nom que celui spécifié dans **\@ filename**. *overwrite_existing_file* est de **bit**, avec **0** comme valeur par défaut. Si la **1** est définie, elle remplace le fichier spécifié par **\@ filename**, s’il existe. Si la **valeur est 0**, la procédure stockée échoue si le fichier existe et si le fichier n’est pas remplacé.  
  
## <a name="return-code-values"></a>Codet de retour  
 **0** (succès) ou **1** (échec)  
  
## <a name="remarks"></a>Notes  
 **sp_copysubscription** est utilisé dans tous les types de réplication pour copier une base de données d’abonnement dans un fichier comme alternative à l’application d’un instantané sur l’abonné. La base de données doit être configurée pour prendre uniquement en charge les abonnements par extraction de données (pull). Les utilisateurs détenant les autorisations appropriées peuvent réaliser des copies de la base de données d'abonnement, puis copier ou transporter le fichier d'abonnement (.msf) vers un autre Abonné, ou le lui transmettre par courrier électronique en vue de son attachement en tant qu'abonnement.  
  
 La taille de la base de données d'abonnement copiée doit être inférieure à 2 gigaoctets (Go).  
  
 **sp_copysubscription** est pris en charge uniquement pour les bases de données avec des abonnements client et ne peut pas être exécuté lorsque la base de données a des abonnements serveur.  
  
## <a name="permissions"></a>Autorisations  
 Seuls les membres du rôle serveur fixe **sysadmin** peuvent exécuter **sp_copysubscription**.  
  
## <a name="see-also"></a>Voir aussi  
 [Autres emplacements de dossier d’instantanés](../../relational-databases/replication/snapshot-options.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
