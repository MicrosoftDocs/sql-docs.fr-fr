---
description: CREATE BROKER PRIORITY (Transact-SQL)
title: CREATE BROKER PRIORITY (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- CREATE BROKER PRIORITY
- PRIORITY_TSQL
- CREATE_BROKER_PRIORITY_TSQL
- PRIORITY
- CREATE BROKER
- BROKER_TSQL
- BROKER
- CREATE_BROKER_TSQL
- BROKER PRIORITY
- BROKER_PRIORITY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE BROKER PRIORITY statement
ms.assetid: e0bbebfa-b7c3-4825-8169-7281f7e6de98
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 64f5c5970e45304af41bea1c5c329c55f5b6ad70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99204979"
---
# <a name="create-broker-priority-transact-sql"></a>CREATE BROKER PRIORITY (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Définit un niveau de priorité et le jeu de critères pour déterminer les conversations [!INCLUDE[ssSB](../../includes/sssb-md.md)] auxquelles le niveau de priorité est attribué. Le niveau de priorité est attribué aux points de terminaison qui utilisent la même combinaison de contrats et de services que ceux spécifiés dans la priorité de conversation. Les priorités varient en valeur de 1 (faible) à 10 (élevée). La valeur par défaut est 5.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE BROKER PRIORITY ConversationPriorityName  
FOR CONVERSATION  
[ SET ( [ CONTRACT_NAME = {ContractName | ANY } ]  
        [ [ , ] LOCAL_SERVICE_NAME = {LocalServiceName | ANY } ]  
        [ [ , ] REMOTE_SERVICE_NAME = {'RemoteServiceName' | ANY } ]  
        [ [ , ] PRIORITY_LEVEL = {PriorityValue | DEFAULT } ]  
       )  
]  
[;]  
  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *ConversationPriorityName*  
 Spécifie le nom de cette priorité de conversation. Le nom doit être unique dans la base de données active et doit se conformer aux règles des  [identificateurs](../../relational-databases/databases/database-identifiers.md) du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 SET  
 Spécifie les critères permettant de déterminer si la priorité de conversation s'applique à une conversation. S'il est spécifié, SET doit contenir au moins un critère : CONTRACT_NAME, LOCAL_SERVICE_NAME, REMOTE_SERVICE_NAME ou PRIORITY_LEVEL. Si SET n'est pas spécifié, les valeurs par défaut sont définies pour les trois critères.  
  
 CONTRACT_NAME = {*ContractName* | **ANY**}  
 Spécifie le nom d'un contrat à utiliser comme critère pour déterminer si la priorité de conversation s'applique à une conversation. *ContractName* est un identificateur [!INCLUDE[ssDE](../../includes/ssde-md.md)], et vous devez spécifier le nom d’un contrat de base de données active.  
  
 *ContractName*  
 Indique que la priorité de conversation peut être appliquée uniquement aux conversations dans lesquelles l’instruction BEGIN DIALOG qui a démarré la conversation spécifiait ON CONTRACT *ContractName*.  
  
 ANY  
 Indique que la priorité de conversation peut être appliquée à n'importe quelle conversation, quel que soit le contrat qu'elle utilise.  
  
 La valeur par défaut est ANY.  
  
 LOCAL_SERVICE_NAME = {*LocalServiceName* | **ANY**}  
 Spécifie le nom d'un service à utiliser comme critère pour déterminer si la priorité de conversation s'applique à un point de terminaison.  
  
 *LocalServiceName* est un identificateur [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Il doit spécifier le nom d'un service dans la base de données active.  
  
 *LocalServiceName*  
 Spécifie que la priorité de conversation peut être appliquée aux éléments suivants :  
  
-   Tout point de terminaison de la conversation de l’initiateur dont le nom du service initiateur correspond à *LocalServiceName*.  
  
-   Tout point de terminaison de la conversation de la cible dont le nom du service cible correspond à *LocalServiceName*.  
  
 ANY  
 -   Spécifie que la priorité de conversation peut être appliquée à n'importe quel point de terminaison, quel que soit le nom du service local utilisé par le point de terminaison.  
  
 La valeur par défaut est ANY.  
  
 REMOTE_SERVICE_NAME = {'*RemoteServiceName*' | **ANY**}  
 Spécifie le nom d'un service à utiliser comme critère pour déterminer si la priorité de conversation s'applique à un point de terminaison.  
  
 *RemoteServiceName* est un littéral de type **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une comparaison octet par octet pour la concordance avec la chaîne *RemoteServiceName*. La comparaison respecte la casse et ne prend pas en compte le classement actuel. Le service cible peut être dans l'instance actuelle du [!INCLUDE[ssDE](../../includes/ssde-md.md)] ou dans une instance distante du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
 '*RemoteServiceName*'  
 Spécifie que la priorité de conversation peut être appliquée aux éléments suivants :  
  
-   Tout point de terminaison de la conversation de l’initiateur dont le nom du service cible associé correspond à *RemoteServiceName*.  
  
-   Tout point de terminaison de la conversation de la cible dont le nom du service cible associé correspond à *RemoteServiceName*.  
  
 ANY  
 Spécifie que la priorité de conversation peut être appliquée à n'importe quel point de terminaison, quel que soit le nom du service distant associé au point de terminaison.  
  
 La valeur par défaut est ANY.  
  
 PRIORITY_LEVEL = { *PriorityValue* | **DEFAULT** }  
 Spécifie la priorité à attribuer à un point de terminaison qui utilise les contrats et les services spécifiés dans la priorité de conversation. *PriorityValue* doit être un littéral entier compris entre 1 (priorité la plus faible) et 10 (priorité la plus élevée). La valeur par défaut est 5.  
  
## <a name="remarks"></a>Notes  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assigne des niveaux de priorité aux points de terminaison de la conversation. Les niveaux de priorité contrôlent la priorité des opérations associées au point de terminaison. Chaque conversation comporte deux points de terminaison :  
  
-   Le point de terminaison de l'initiateur associe un côté de la conversation au service et à la file d'attente de l'initiateur. Le point de terminaison de la conversation de l'initiateur est créé lors de l'exécution de l'instruction BEGIN DIALOG. Les opérations suivantes sont associées au point de terminaison de l'initiateur :  
  
    -   Envoi à partir du service initiateur.  
  
    -   Réception dans la file d'attente de l'initiateur.  
  
    -   Obtention du groupe de conversations suivant à partir de la file d'attente de l'initiateur.  
  
-   Le point de terminaison de la cible associe l'autre côté de la conversation au service cible et à la file d'attente de la cible. Le point de terminaison cible est créé lorsque la conversation est utilisée pour envoyer un message à la file d'attente de la cible. Les opérations suivantes sont associées au point de terminaison de la cible :  
  
    -   Réception dans la file d'attente de la cible.  
  
    -   Envoi à partir du service cible.  
  
    -   Obtention du groupe de conversations suivant à partir de la file d'attente de la cible.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assigne des niveaux de priorité de conversation lors de la création des points de terminaison de conversation. Les points de terminaison de la conversation conservent leur niveau de priorité jusqu'à la fin de la conversation. Les priorités nouvelles ou modifiées ne sont pas appliquées aux conversations existantes.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] assigne au point de terminaison le niveau de la priorité de conversation dont les critères de contrat et de services correspondent le mieux aux propriétés du point de terminaison. Le tableau suivant présente la précédence des correspondances :  
  
|Contrat d'opération|Service local d'opération|Service distant d'opération|  
|------------------------|-----------------------------|------------------------------|  
|*ContractName*|*LocalServiceName*|*RemoteServiceName*|  
|*ContractName*|*LocalServiceName*|ANY|  
|*ContractName*|ANY|*RemoteServiceName*|  
|*ContractName*|ANY|ANY|  
|ANY|*LocalServiceName*|*RemoteServiceName*|  
|ANY|*LocalServiceName*|ANY|  
|ANY|ANY|*RemoteServiceName*|  
|ANY|ANY|ANY|  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] recherche tout d'abord une priorité dont le contrat, le service local et le service distant spécifiés correspondent à ceux utilisés par l'opération. S'il n'en trouve pas, [!INCLUDE[ssSB](../../includes/sssb-md.md)] recherche alors une priorité dont le contrat et le service local correspondent à ceux utilisés par l'opération, et dont le service distant a la valeur ANY. Ce processus se poursuit pour toutes les variations répertoriées dans le tableau de précédence. Si aucune correspondance n'est trouvée, la priorité 5 par défaut est attribuée à l'opération.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] attribue indépendamment un niveau de priorité à chaque point de terminaison. Pour que [!INCLUDE[ssSB](../../includes/sssb-md.md)] assigne des niveaux de priorité aux points de terminaison de la conversation de l'initiateur et de la cible, vous devez vous assurer que les deux points de terminaison sont couverts par des priorités de conversation. Si les points de terminaison de l'initiateur et de la cible se trouvent dans des bases de données distinctes, vous devez créer des priorités de conversation dans chaque base de données. Le même niveau de priorité est généralement spécifié pour les deux points de terminaison pour une conversation, mais vous pouvez spécifier des niveaux de priorité différents.  
  
 Les niveaux de priorité sont toujours appliqués à des opérations qui reçoivent des messages ou des identificateurs de groupe de conversations d'une file d'attente. Les niveaux de priorité sont également appliqués lors de la transmission de messages d'une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] à une autre.  
  
 Les niveaux de priorité ne sont pas utilisés lors de la transmission des messages suivants :  
  
-   En provenance d'une base de données où l'option de base de données HONOR_BROKER_PRIORITY a la valeur OFF. Pour plus d’informations, consultez [Options ALTER DATABASE SET &#40;Transact-SQL&#41;](../../t-sql/statements/alter-database-transact-sql-set-options.md).  
  
-   Entre services dans la même instance du moteur de base de données.  
  
-   La priorité 5 par défaut est attribuée à toutes les opérations [!INCLUDE[ssSB](../../includes/sssb-md.md)] d’une base de données si aucune priorité de conversation n’a été créée dans la base de données.  
  
## <a name="permissions"></a>Autorisations  
 L'autorisation de création d'une priorité de conversation est accordée par défaut aux membres du rôle de base de données fixe db_ddladmin ou db_owner, ainsi qu'aux membres du rôle serveur fixe sysadmin. Nécessite l'autorisation ALTER sur la base de données.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-assigning-a-priority-level-to-both-directions-of-a-conversation"></a>R. Attribution d'un niveau de priorité aux deux directions d'une conversation  
 Ces deux priorités de conversation font en sorte que le niveau de priorité 3 est attribué à toutes les opérations qui utilisent `SimpleContract` entre `TargetService` et `InitiatorAService`.  
  
```sql  
CREATE BROKER PRIORITY InitiatorAToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = InitiatorServiceA,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 3);  
CREATE BROKER PRIORITY TargetToInitiatorAPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'InitiatorServiceA',  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="b-setting-the-priority-level-for-all-conversations-that-use-a-contract"></a>B. Définition du niveau de priorité pour toutes les conversations qui utilisent un contrat  
 Attribue le niveau de priorité `7` à toutes les opérations qui utilisent un contrat nommé `SimpleContract`. Cela suppose qu'il n'y a pas d'autres priorités qui spécifient à la fois `SimpleContract` et un service local ou distant.  
  
```sql 
CREATE BROKER PRIORITY SimpleContractDefaultPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 7);  
```  
  
### <a name="c-setting-a-base-priority-level-for-a-database"></a>C. Définition d'un niveau de priorité de base pour une base de données  
 Définit des priorités de conversation pour deux services spécifiques, puis une priorité de conversation qui correspond à tous les autres points de terminaison. Si cela ne remplace pas la priorité par défaut, qui est toujours 5, cela réduit le nombre d'éléments auxquels la valeur par défaut est attribuée.  
  
```sql 
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ClaimPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 9);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/ApprovalPriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = //Adventure-Works.com/Expenses/ClaimService,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY [//Adventure-Works.com/Expenses/BasePriority]  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = ANY,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 3);  
```  
  
### <a name="d-creating-three-priority-levels-for-a-target-service-by-using-services"></a>D. Création de trois niveaux de priorité pour un service cible à l'aide de services  
 Prend en charge un système qui fournit trois niveaux de performances : Or (hautes), Argent (moyennes) et Bronze (faibles). Il y a un contrat, mais chaque niveau possède un service initiateur séparé. Tous les services initiateur communiquent avec un service cible central.  
  
```sql  
CREATE BROKER PRIORITY GoldInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = GoldInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY GoldTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'GoldInitiatorService',  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = SilverInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY SilverTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'SilverInitiatorService',  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzeInitToTargetPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = BronzeInitiatorService,  
         REMOTE_SERVICE_NAME = N'TargetService',  
         PRIORITY_LEVEL = 2);  
CREATE BROKER PRIORITY BronzeTargetToInitPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SimpleContract,  
         LOCAL_SERVICE_NAME = TargetService,  
         REMOTE_SERVICE_NAME = N'BronzeInitiatorService',  
         PRIORITY_LEVEL = 2);  
```  
  
### <a name="e-creating-three-priority-levels-for-multiple-services-using-contracts"></a>E. Création de trois niveaux de priorité pour plusieurs services à l'aide de contrats  
 Prend en charge un système qui fournit trois niveaux de performances : Or (hautes), Argent (moyennes) et Bronze (faibles). Chaque niveau possède un contrat séparé. Ces priorités s'appliquent à tous les services qui sont référencés par des conversations qui utilisent les contrats.  
  
```sql  
CREATE BROKER PRIORITY GoldPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = GoldContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 6);  
CREATE BROKER PRIORITY SilverPriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = SilverContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 4);  
CREATE BROKER PRIORITY BronzePriority  
    FOR CONVERSATION  
    SET (CONTRACT_NAME = BronzeContract,  
         LOCAL_SERVICE_NAME = ANY,  
         REMOTE_SERVICE_NAME = ANY,  
         PRIORITY_LEVEL = 2);  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/alter-broker-priority-transact-sql.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [DROP BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/drop-broker-priority-transact-sql.md)   
 [GET CONVERSATION GROUP &#40;Transact-SQL&#41;](../../t-sql/statements/get-conversation-group-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [sys.conversation_priorities &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-priorities-transact-sql.md)  
  
  
