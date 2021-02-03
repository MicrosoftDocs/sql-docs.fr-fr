---
description: ALTER BROKER PRIORITY (Transact-SQL)
title: ALTER BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- ALTER_BROKER_TSQL
- ALTER BROKER PRIORITY
- ALTER BROKER
- ALTER_BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- ALTER BROKER PRIORITY statement
- ssbdiagnose
ms.assetid: 15fda1b2-e4dd-4f9d-935a-2e38926075b2
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8d9fa209e3f2c9fbfceeeec4a2a58a0fe633ba7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99191069"
---
# <a name="alter-broker-priority-transact-sql"></a>ALTER BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Modifie les propriétés d'une priorité de conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)].  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
  
ALTER BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
{ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = { PriorityValue | DEFAULT } ]  
              )  
}  
[;]  
  
```  
  

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *ConversationPriorityName*  
 Spécifie le nom de la priorité de conversation à modifier. Le nom doit faire référence à une priorité de conversation dans la base de données active.  
  
 SET  
 Spécifie les critères permettant de déterminer si la priorité de conversation s'applique à une conversation. SET est requis et doit contenir au moins un critère : CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME ou PRIORITY_LEVEL.  
  
 CONTRACT_NAME = {*ContractName* | **ANY**}  
 Spécifie le nom d'un contrat à utiliser comme critère pour déterminer si la priorité de conversation s'applique à une conversation. *ContractName* est un identificateur [!INCLUDE[ssDE](../../includes/ssde-md.md)], et vous devez spécifier le nom d’un contrat de base de données active.  
  
 *ContractName*  
 Indique que la priorité de conversation peut être appliquée uniquement aux conversations dans lesquelles l’instruction BEGIN DIALOG qui a démarré la conversation spécifiait ON CONTRACT *ContractName*.  
  
 ANY  
 Indique que la priorité de conversation peut être appliquée à n'importe quelle conversation, quel que soit le contrat qu'elle utilise.  
  
 Si CONTRACT_NAME n'est pas spécifié, la propriété de contrat de la priorité de conversation n'est pas modifiée.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Spécifie le nom d'un service à utiliser comme critère pour déterminer si la priorité de conversation s'applique à un point de terminaison.  
  
 *LocalServiceName* est un identificateur du [!INCLUDE[ssDE](../../includes/ssde-md.md)], et doit spécifier le nom d’un service de la base de données active.  
  
 *LocalServiceName*  
 Spécifie que la priorité de conversation peut être appliquée aux éléments suivants :  
  
-   Tout point de terminaison de la conversation de l’initiateur dont le nom du service initiateur correspond à *LocalServiceName*.  
  
-   Tout point de terminaison de la conversation de la cible dont le nom du service cible correspond à *LocalServiceName*.  
  
 ANY  
 -   Spécifie que la priorité de conversation peut être appliquée à n'importe quel point de terminaison, quel que soit le nom du service local utilisé par le point de terminaison.  
  
 Si LOCAL_SERVICE_NAME n'est pas spécifié, la propriété du service local de la priorité de conversation n'est pas modifiée.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Spécifie le nom d'un service à utiliser comme critère pour déterminer si la priorité de conversation s'applique à un point de terminaison.  
  
 *RemoteServiceName* est un littéral de type **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une comparaison octet par octet pour la concordance avec la chaîne *RemoteServiceName*. La comparaison respecte la casse et ne prend pas en compte le classement actuel. Le service cible peut être dans l'instance actuelle du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou dans une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 '*RemoteServiceName*'  
 Spécifie que la priorité de conversation doit être attribuée aux éléments suivants :  
  
-   Tout point de terminaison de la conversation de l’initiateur dont le nom du service cible associé correspond à *RemoteServiceName*.  
  
-   Tout point de terminaison de la conversation de la cible dont le nom du service cible associé correspond à *RemoteServiceName*.  
  
 ANY  
 Spécifie que la priorité de conversation s'applique à n'importe quel point de terminaison, quel que soit le nom du service distant associé au point de terminaison.  
  
 Si REMOTE_SERVICE_NAME n'est pas spécifié, la propriété du service distant de la priorité de conversation n'est pas modifiée.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Spécifie le niveau de priorité à attribuer à un point de terminaison qui utilise les contrats et les services spécifiés dans la priorité de conversation. *PriorityValue* doit être un littéral entier compris entre 1 (priorité la plus faible) et 10 (priorité la plus élevée).  
  
 Si PRIORITY_LEVEL n'est pas spécifié, la propriété du niveau de priorité de la conversation n'est pas modifiée.  
  
## <a name="remarks"></a>Notes  
 Aucune propriété modifiée par ALTER BROKER PRIORITY n'est appliquée aux conversations existantes. Les conversations existantes se poursuivent avec la priorité qui leur a été attribuée au démarrage.  
  
 Pour plus d’informations, consultez [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md).  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation de création d’une priorité de conversation est accordée par défaut aux membres du rôle de base de données fixe **db_ddladmin** ou **db_owner**, ainsi qu’aux membres du rôle serveur fixe **sysadmin**. Nécessite l'autorisation ALTER sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-changing-only-the-priority-level-of-an-existing-conversation-priority"></a>R. Modification portant uniquement sur le niveau de priorité d'une priorité de conversation existante  
 Modifie le niveau de priorité, mais ne modifie pas le contrat, le service local ni les propriétés du service distant.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-changing-all-of-the-properties-of-an-existing-conversation-priority"></a>B. Modification de toutes les propriétés d'une priorité de conversation existante  
 Modifie les propriétés du niveau de priorité, du contrat, du service local et du service distant.  
  
```sql  
ALTER BROKER PRIORITY SimpleContractPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContractB,  
         LOCAL_SERVICE_NAME = TargetServiceB,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceB',  
         PRIORITY_LEVEL = 8);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
