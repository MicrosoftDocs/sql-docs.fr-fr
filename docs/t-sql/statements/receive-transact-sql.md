---
description: RECEIVE (Transact-SQL)
title: RECEIVE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECEIVE_TSQL
- RECEIVE
dev_langs:
- TSQL
helpviewer_keywords:
- queues [Service Broker], message retrieval
- messages [Service Broker], retrieving
- RECEIVE statement
- receiving messages
- retrieving messages
ms.assetid: 878c6c14-37ab-4b87-9854-7f8f42bac7dd
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 840f023e1a906350f34d0709459842a3fe3c796a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98080883"
---
# <a name="receive-transact-sql"></a>RECEIVE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Récupère un ou plusieurs messages dans une file d'attente. Selon le paramètre de rétention défini pour la file d'attente, supprime le message de la file d'attente ou met à jour l'état du message dans la file d'attente.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
[ WAITFOR ( ]  
    RECEIVE [ TOP ( n ) ]   
        <column_specifier> [ ,...n ]  
        FROM <queue>  
        [ INTO table_variable ]  
        [ WHERE {  conversation_handle = conversation_handle  
                 | conversation_group_id = conversation_group_id } ]  
[ ) ] [ , TIMEOUT timeout ]  
[ ; ]  
  
<column_specifier> ::=  
{    *   
  |  { column_name | [ ] expression } [ [ AS ] column_alias ]  
  |  column_alias = expression   
}     [ ,...n ]   
  
<queue> ::=  
{ database_name.schema_name.queue_name | schema_name.queue_name | queue_name }
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 WAITFOR  
 Indique que l'instruction RECEIVE attend qu'un message arrive dans la file d'attente, si aucun message n'est actuellement présent.  
  
 TOP( *n* )  
 Indique le nombre maximal de messages à retourner. Si cette clause n'est pas spécifiée, tous les messages qui satisfont aux critères de l'instruction sont retournés.  
  
 \*  
 Indique que le jeu de résultats contient toutes les colonnes de la file d'attente.  
  
 *column_name*  
 Nom de la colonne à inclure dans le jeu de résultats.  
  
 *expression*  
 Nom de colonne, constante, fonction ou toute combinaison de noms de colonnes, de constantes ou de fonctions reliés par un opérateur.  
  
 *column_alias*  
 Nom utilisé pour remplacer le nom de colonne dans le jeu de résultats.  
  
 FROM  
 Spécifie la file d'attente qui contient les messages à récupérer.  
  
 *database_name*  
 Nom de la base de données qui contient la file d'attente à partir de laquelle les messages sont envoyés. Quand aucun argument *database_name* n’est fourni, la base de données active est utilisée par défaut.  
  
 *schema_name*  
 Nom du schéma propriétaire de la file d'attente à partir de laquelle les messages sont envoyés. Quand aucun argument *schema_name* n’est fourni, le schéma par défaut de l’utilisateur actif est utilisé par défaut.  
  
 *queue_name*  
 Nom de la file d'attente à partir de laquelle les messages sont envoyés.  
  
 INTO *table_variable*  
 Spécifie la variable de table dans laquelle RECEIVE place les messages. La variable de table doit avoir un nombre de colonnes égal à celui présent dans les messages. Le type de données de chaque colonne dans la variable de table doit pouvoir être converti implicitement vers le type de données de la colonne correspondante dans les messages. Si INTO n'est pas spécifié, les messages sont retournés sous la forme d'un jeu de résultats.  
  
 WHERE  
 Indique la conversation ou le groupe de conversations des messages reçus. Si cette clause est omise, retourne les messages du groupe de conversations suivant disponible.  
  
 conversation_handle = *conversation_handle*  
 Indique la conversation pour les messages reçus. Le *handle de conversation* fourni doit être un **uniqueidentifer** ou un type convertible en **uniqueidentifier**.  
  
 conversation_group_id = *conversation_group_id*  
 Indique le groupe de conversation pour les messages reçus. L’*ID de groupe de conversations* fourni doit être un **uniqueidentifier** ou un type convertible en **uniqueidentifier**.  
  
 TIMEOUT *timeout*  
 Indique le temps, en millisecondes, pendant lequel l'instruction attend un message. Cette clause ne peut être utilisée qu'avec la clause WAITFOR. Si cette clause n’est pas spécifiée ou si le délai d’expiration a la valeur -**1**, le délai d’attente est illimité. Si le délai expire, l'instruction RECEIVE retourne un jeu de résultats vide.  
  
## <a name="remarks"></a>Remarques  
  
> [!IMPORTANT]  
>  Si l'instruction RECEIVE n'est pas la première instruction dans un lot ou une procédure stockée, l'instruction précédente doit se terminer par un point-virgule (;).  
  
 L'instruction RECEIVE lit les messages d'une file d'attente et retourne un jeu de résultats. Le jeu de résultats ne comporte aucune ligne ou comporte plusieurs lignes, chacune contenant un message unique. Si la clause INTO n’est pas utilisée et que l’argument *column_specifier* n’affecte pas les valeurs des variables locales, l’instruction retourne un jeu de résultats au programme appelant.  
  
 Les messages retournés par l'instruction RECEIVE peuvent être de types différents. Les applications peuvent utiliser la colonne **message_type_name** pour acheminer chaque message vers du code qui gère le type de message associé. Il existe deux classes de types de messages :  
  
-   Les types de messages définis par l'application, qui ont été créés à l'aide de l'instruction CREATE MESSAGE TYPE. L'ensemble de types de messages définis par l'application autorisés dans une conversation est défini par le contrat [!INCLUDE[ssSB](../../includes/sssb-md.md)] spécifié pour la conversation.  
  
-   Messages système [!INCLUDE[ssSB](../../includes/sssb-md.md)] qui retournent des informations d'état ou d'erreur.  
  
 L'instruction RECEIVE supprime les messages reçus de la file d'attente, sauf si la file d'attente spécifie une période de rétention des messages. Si la valeur ON est affectée au paramètre RETENTION, l’instruction RECEIVE met à jour la colonne **status** avec la valeur **0** et conserve les messages dans la file d’attente. Quand une transaction qui contient une instruction RECEIVE est restaurée, toutes les modifications apportées à la file d'attente dans la transaction sont également restaurées et les messages retournent dans la file d'attente.  
  
 Tous les messages retournés par une instruction RECEIVE appartiennent au même groupe de conversations. L'instruction RECEIVE verrouille le groupe de conversations pour les messages qui sont retournés jusqu'à ce que la transaction qui contient l'instruction se termine. Une instruction RECEIVE retourne des messages qui ont un **état** égal à **1**. Le jeu de résultats retourné par une instruction RECEIVE est trié implicitement :  
  
-   Si des messages de plusieurs conversations rencontrent les conditions de clause WHERE, l'instruction RECEIVE retourne tous les messages d'une conversation avant de retourner les messages pour toute autre conversation. Les conversations sont traitées dans l'ordre descendant des niveaux de priorité.  
  
-   Pour une conversation donnée, une instruction RECEIVE retourne les messages dans l’ordre croissant de l’argument **message_sequence_number**.  
  
 La clause WHERE de l’instruction RECEIVE peut contenir seulement une condition de recherche, qui utilise **conversation_handle** ou **conversation_group_id**. La condition de recherche ne peut pas contenir d'autres colonnes de la file d'attente. **conversation_handle** ou **conversation_group_id** ne peut pas être une expression. Le jeu de messages retourné dépend des conditions spécifiées dans la clause WHERE :  
  
-   Si **conversation_handle** est spécifié, RECEIVE retourne tous les messages de la conversation spécifiée qui sont disponibles dans la file d’attente.  
  
-   Si **conversation_group_id** est spécifié, RECEIVE retourne tous les messages qui sont disponibles dans la file d’attente de toute conversation qui est membre du groupe de conversations spécifié.  
  
-   En l'absence de clause WHERE, RECEIVE détermine quel groupe de conversations :  
  
    -   a un ou plusieurs messages dans la file d'attente ;  
  
    -   n'a pas été verrouillé par une autre instruction RECEIVE ;  
  
    -   a le niveau de priorité le plus élevé de tous les groupes de conversations qui répondent à ces critères.  
  
     RECEIVE retourne ensuite tous les messages disponibles dans la file d'attente de toute conversation qui est un membre du groupe de conversations sélectionné.  
  
 Si le descripteur de conversation ou l'identificateur du groupe de conversations spécifié dans la clause WHERE n'existe pas ou n'est pas associé à la file d'attente spécifiée, l'instruction RECEIVE retourne une erreur.  
  
 Si le statut de la file d'attente spécifiée dans l'instruction RECEIVE a la valeur OFF, l'instruction échoue avec une erreur [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 Quand la clause WAITFOR est spécifiée, l'instruction attend pendant le délai spécifié ou jusqu'à ce qu'un jeu de résultats soit disponible. Si la file d'attente est supprimée ou si son état est défini sur OFF pendant l'attente de l'instruction, cette dernière retourne immédiatement une erreur. Si l'instruction RECEIVE spécifie un groupe de conversations ou un descripteur de conversation et que le service de cette conversation est supprimé ou déplacé vers une autre file d'attente, l'instruction RECEIVE signale une erreur [!INCLUDE[tsql](../../includes/tsql-md.md)].  
  
 RECEIVE n'est pas valide dans une fonction définie par l'utilisateur.  
  
 L'instruction RECEIVE n'a aucune prévention de privation de priorité. Si une instruction RECEIVE unique verrouille un groupe de conversations et extrait de nombreux messages à partir de conversations de faible priorité, aucun message ne peut être reçu de conversations de haute priorité dans le groupe. Pour empêcher ceci, lorsque vous extrayez des messages de conversations de faible priorité, utilisez la clause TOP pour limiter le nombre de messages extraits par chaque instruction RECEIVE.  
  
## <a name="queue-columns"></a>Colonnes de la file d'attente  
 Le tableau suivant répertorie les colonnes d'une file d'attente :  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**statut**|**tinyint**|État du message. Pour les messages retournés par la commande RECEIVE, l’état a toujours la valeur **0**. Dans la file d'attente, les messages peuvent contenir l'une des valeurs suivantes :<br /><br /> **0**= Prêt **1**= Message reçu **2**= Pas encore terminé **3**= Message envoyé conservé|  
|**priority**|**tinyint**|Niveau de priorité de conversation appliqué au message.|  
|**queuing_order**|**bigint**|Numéro d'ordre du message dans la file d'attente.|  
|**conversation_group_id**|**uniqueidentifier**|Identificateur du groupe de conversations auquel ce message appartient.|  
|**conversation_handle**|**uniqueidentifier**|Descripteur de conversation dont ce message fait partie.|  
|**message_sequence_number**|**bigint**|Numéro de séquence du message dans la conversation.|  
|**service_name**|**nvarchar(512)**|Nom du service auquel la conversation est destinée.|  
|**service_id**|**int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du service auquel la conversation est destinée.|  
|**service_contract_name**|**nvarchar (256)**|Nom du contrat suivi par la conversation.|  
|**service_contract_id**|**int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du contrat suivi par la conversation.|  
|**message_type_name**|**nvarchar (256)**|Nom du type de message qui décrit le format du message. Les messages peuvent être des types de messages d'application ou des messages système Service Broker.|  
|**message_type_id**|**int**|Identificateur d'objet [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du type de message décrivant le message.|  
|**validation**|**nchar(2)**|Validation utilisée pour le message.<br /><br /> **E**=Empty **N**=None **X**=XML|  
|**message_body**|**varbinary(MAX)**|Contenu du message.|  
  
## <a name="permissions"></a>Autorisations  
 Pour recevoir un message, l'utilisateur en cours doit disposer de l'autorisation RECEIVE sur la file d'attente.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-receiving-all-columns-for-all-messages-in-a-conversation-group"></a>R. Réception de toutes les colonnes de tous les messages d'un groupe de conversations  
 L'exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations suivant disponible dans la file d'attente `ExpenseQueue`. L'instruction retourne les messages en tant que jeu de résultats.  
  
```sql  
RECEIVE * FROM ExpenseQueue ;  
```  
  
### <a name="b-receiving-specified-columns-for-all-messages-in-a-conversation-group"></a>B. Réception des colonnes spécifiées de tous les messages d'un groupe de conversations  
 L'exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations suivant disponible dans la file d'attente `ExpenseQueue`. L'instruction retourne les messages en tant que jeu de résultats qui contient les colonnes `conversation_handle`, `message_type_name` et `message_body`.  
  
```sql  
RECEIVE conversation_handle, message_type_name, message_body  
FROM ExpenseQueue ;  
```  
  
### <a name="c-receiving-the-first-available-message-in-the-queue"></a>C. Réception du premier message disponible de la file d'attente  
 L'exemple suivant montre comment recevoir le premier message disponible de la file d'attente `ExpenseQueue` en tant que jeu de résultats.  
  
```sql  
RECEIVE TOP (1) * FROM ExpenseQueue ;  
```  
  
### <a name="d-receiving-all-messages-for-a-specified-conversation"></a>D. Réception de tous les messages d'une conversation spécifique  
 L'exemple suivant montre comment recevoir tous les messages disponibles de la conversation spécifiée dans la file d'attente `ExpenseQueue` en tant que jeu de résultats.  
  
```sql  
DECLARE @conversation_handle UNIQUEIDENTIFIER ;  
  
SET @conversation_handle = <retrieve conversation from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_handle = @conversation_handle ;  
```  
  
### <a name="e-receiving-messages-for-a-specified-conversation-group"></a>E. Réception des messages d'un groupe de conversations spécifique  
 L'exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations spécifié dans la file d'attente `ExpenseQueue` en tant que jeu de résultats.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id =   
    <retrieve conversation group ID from database> ;  
  
RECEIVE *  
FROM ExpenseQueue  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="f-receiving-into-a-table-variable"></a>F. Réception dans une variable de table  
 L'exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations spécifié dans la file d'attente `ExpenseQueue` dans une variable de table.  
  
```sql  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
DECLARE @procTable TABLE(  
     service_instance_id UNIQUEIDENTIFIER,  
     handle UNIQUEIDENTIFIER,  
     message_sequence_number BIGINT,  
     service_name NVARCHAR(512),  
     service_contract_name NVARCHAR(256),  
     message_type_name NVARCHAR(256),  
     validation NCHAR,  
     message_body VARBINARY(MAX)) ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database> ;  
  
RECEIVE TOP (1)  
    conversation_group_id,  
    conversation_handle,  
    message_sequence_number,  
    service_name,  
    service_contract_name,  
    message_type_name,  
    validation,  
    message_body  
FROM ExpenseQueue  
INTO @procTable  
WHERE conversation_group_id = @conversation_group_id ;  
```  
  
### <a name="g-receiving-messages-and-waiting-indefinitely"></a>G. Réception des messages et délai d'attente indéfini  
 L’exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations suivant disponible dans la file d’attente `ExpenseQueue`. L'instruction attend qu'au moins un message soit disponible, puis retourne un jeu de résultats qui contient toutes les colonnes des messages.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue) ;  
```  
  
### <a name="h-receiving-messages-and-waiting-for-a-specified-interval"></a>H. Réception des messages et intervalle d'attente spécifié  
 L’exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations suivant disponible dans la file d’attente `ExpenseQueue`. L'instruction attend 60 secondes ou qu'au moins un message soit disponible, selon ce qui se produit en premier. L'instruction retourne un jeu de résultats qui contient toutes les colonnes des messages, si au moins un message est disponible. Dans le cas contraire, l'instruction retourne un jeu de résultats vide.  
  
```sql  
WAITFOR (  
    RECEIVE *  
    FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="i-receiving-messages-modifying-the-type-of-a-column"></a>I. Réception de messages, modification du type d'une colonne  
 L’exemple suivant montre comment recevoir tous les messages disponibles du groupe de conversations suivant disponible dans la file d’attente `ExpenseQueue`. Si le type de message indique que le message contient un document XML, l'instruction convertit le corps du message en document XML.  
  
```sql  
WAITFOR (  
    RECEIVE message_type_name,  
        CASE  
            WHEN validation = 'X' THEN CAST(message_body as XML)  
            ELSE NULL  
         END AS message_body   
         FROM ExpenseQueue ),  
TIMEOUT 60000 ;  
```  
  
### <a name="j-receiving-a-message-extracting-data-from-the-message-body-retrieving-conversation-state"></a>J. Réception d'un message, extraction de données du corps du message, récupération de l'état de la conversation  
 L'exemple suivant montre comment recevoir les messages disponibles suivants du groupe de conversations suivant disponible dans la file d'attente `ExpenseQueue`. Si le message est de type `//Adventure-Works.com/Expenses/SubmitExpense`, l'instruction extrait l'ID de l'employé et une liste d'éléments du corps du message. L'instruction récupère également l'état de la conversation dans la table `ConversationState`.  
  
```sql  
WAITFOR(  
    RECEIVE   
    TOP(1)  
      message_type_name,  
      COALESCE(  
           (SELECT TOP(1) ConversationState  
            FROM CurrentConversations AS cc  
            WHERE cc.ConversationHandle = conversation_handle),  
           'NEW')  
      AS ConversationState,  
      COALESCE(  
          (SELECT TOP(1) ErrorCount  
           FROM CurrentConversations AS cc  
           WHERE cc.ConversationHandle = conversation_handle),   
           0)  
      AS ConversationErrors,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).value(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"  
                   (/rpt:ExpenseReport/rpt:EmployeeID)[1]', 'nvarchar(20)')  
         ELSE NULL  
      END AS EmployeeID,  
      CASE WHEN message_type_name = N'//Adventure-Works.com/Expenses/SubmitExpense'  
          THEN CAST(message_body AS XML).query(  
                'declare namespace rpt = "https://Adventure-Works.com/schemas/expenseReport"   
                     /rpt:ExpenseReport/rpt:ItemDetail')  
          ELSE NULL  
      END AS ItemList  
    FROM ExpenseQueue   
), TIMEOUT 60000 ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [SEND &#40;Transact-SQL&#41;](../../t-sql/statements/send-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [ALTER QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-queue-transact-sql.md)   
 [DROP QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-queue-transact-sql.md)  
  
  
