---
description: sp_pdw_add_network_credentials (Azure Synapse Analytics)
title: sp_pdw_add_network_credentials
titleSuffix: Azure Synapse Analytics
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: reference
dev_langs:
- TSQL
ms.assetid: 0729eeff-ac7e-43f0-80fa-ff5346a75985
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.custom: seo-dt-2019
ms.openlocfilehash: dcc54d2d6367182ab6363ef7be2e3c9de6d615eb
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99210770"
---
# <a name="sp_pdw_add_network_credentials-azure-synapse-analytics"></a>sp_pdw_add_network_credentials (Azure Synapse Analytics)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  Cela stocke les informations d’identification réseau dans [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] et les associe à un serveur. Par exemple, utilisez cette procédure stockée pour accorder des [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] autorisations de lecture/écriture appropriées pour effectuer des opérations de sauvegarde et de restauration de base de données sur un serveur cible, ou pour créer une sauvegarde d’un certificat utilisé pour TDE.  
  
 ![Icône du lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_add_network_credentials 'target_server_name',  'user_name', 'password'  
```  
[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="arguments"></a>Arguments  
 '*target_server_name*'  
 Spécifie le nom d’hôte ou l’adresse IP du serveur cible. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] accédera à ce serveur à l’aide des informations d’identification de nom d’utilisateur et de mot de passe transmises à cette procédure stockée.  
  
 Pour vous connecter via le réseau InfiniBand, utilisez l’adresse IP InfiniBand du serveur cible.  
  
 *target_server_name* est défini en tant que nvarchar (337).  
  
 '*user_name*'  
 Spécifie le user_name qui dispose des autorisations d’accès au serveur cible. Si des informations d’identification existent déjà pour le serveur cible, elles seront mises à jour avec les nouvelles informations d’identification.  
  
 *user_name* est défini comme nvarchar (513).  
  
 *mot de passe* ꞌ  
 Spécifie le mot de passe pour *user_name*.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l’autorisation **ALTER Server State** .  
  
## <a name="error-handling"></a>Gestion des erreurs  
 Une erreur se produit si l’ajout d’informations d’identification échoue sur le nœud de contrôle et sur tous les nœuds de calcul.  
  
## <a name="general-remarks"></a>Remarques d'ordre général  
 Cette procédure stockée ajoute les informations d’identification réseau au compte NetworkService pour [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] . Le compte NetworkService exécute chaque instance de SMP [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur le nœud de contrôle et les nœuds de calcul. Par exemple, lorsqu’une opération de sauvegarde s’exécute, le nœud de contrôle et chaque nœud de calcul utilisent les informations d’identification du compte NetworkService pour obtenir des autorisations de lecture et d’écriture sur le serveur cible.  
  
## <a name="examples-sssdwfull-and-sspdw"></a>Exemples : [!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] et [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
  
### <a name="a-add-credentials-for-performing-a-database-backup"></a>R. Ajouter des informations d’identification pour effectuer une sauvegarde de base de données  
 L’exemple suivant associe les informations d’identification de nom d’utilisateur et de mot de passe pour l’utilisateur de domaine seattle\david à un serveur cible qui a l’adresse IP 10.172.63.255. L’utilisateur seattle\david dispose d’autorisations de lecture/écriture sur le serveur cible. [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] stocke ces informations d’identification et les utilise pour lire et écrire dans et à partir du serveur cible, si nécessaire pour les opérations de sauvegarde et de restauration.  
  
```sql  
EXEC sp_pdw_add_network_credentials '10.172.63.255', 'seattle\david', '********';  
```  
  
 La commande Backup exige que le nom du serveur soit entré en tant qu’adresse IP.  
  
> [!NOTE]  
>  Pour effectuer la sauvegarde de la base de données sur InfiniBand, veillez à utiliser l’adresse IP InfiniBand du serveur de sauvegarde.  
  
## <a name="see-also"></a>Voir aussi  
 [sp_pdw_remove_network_credentials &#40;&#41;Azure Synapse Analytics ](../../relational-databases/system-stored-procedures/sp-pdw-remove-network-credentials-sql-data-warehouse.md)  
  
  

