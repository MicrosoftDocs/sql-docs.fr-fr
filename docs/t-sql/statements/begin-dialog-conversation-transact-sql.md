---
description: BEGIN DIALOG CONVERSATION (Transact-SQL)
title: BEGIN DIALOG CONVERSATION (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/26/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- DIALOG CONVERSATION
- DIALOG
- BEGIN_DIALOG_TSQL
- BEGIN_DIALOG_CONVERSATION_TSQL
- DIALOG_CONVERSATION_TSQL
- DIALOG_TSQL
- BEGIN DIALOG
- BEGIN DIALOG CONVERSATION
dev_langs:
- TSQL
helpviewer_keywords:
- conversations [Service Broker]
- beginning dialogs
- dialogs [Service Broker], beginning
- BEGIN DIALOG CONVERSATION statement
- cryptography [SQL Server], conversations
- opening conversations
- encryption [SQL Server], conversations
- starting conversations
ms.assetid: 8e814f9d-77c1-4906-b8e4-668a86fc94ba
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: fdf0f646d2df5c8b0c2fc748973378426a0b96b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184465"
---
# <a name="begin-dialog-conversation-transact-sql"></a>BEGIN DIALOG CONVERSATION (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Commence un dialogue entre deux services. Un dialogue est une conversation qui fournit les messages exactement dans l'ordre entre deux services.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
BEGIN DIALOG [ CONVERSATION ] @dialog_handle  
   FROM SERVICE initiator_service_name  
   TO SERVICE 'target_service_name'  
       [ , { 'service_broker_guid' | 'CURRENT DATABASE' }]   
   [ ON CONTRACT contract_name ]  
   [ WITH  
   [  { RELATED_CONVERSATION = related_conversation_handle   
      | RELATED_CONVERSATION_GROUP = related_conversation_group_id } ]   
   [ [ , ] LIFETIME = dialog_lifetime ]   
   [ [ , ] ENCRYPTION = { ON | OFF }  ] ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 **@** _dialog_handle_  
 Variable utilisée pour stocker le handle de dialogue généré par le système pour le nouveau dialogue retourné par l'instruction BEGIN DIALOG CONVERSATION. La variable doit être de type **uniqueidentifier**.  
  
 FROM SERVICE *initiator_service_name*  
 Spécifie le service qui initie le dialogue. Le nom spécifié doit être le nom d'un service de la base de données active. La file d'attente spécifiée pour le service initiateur reçoit les messages retournés par le service cible et les messages créés par Service Broker pour cette conversation.  
  
 TO SERVICE **'** _target_service_name_ **'**  
 Spécifie le service cible avec lequel le dialogue doit être initialisé. *target_service_name* est de type **nvarchar(256)**. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise une comparaison octet par octet pour la concordance avec la chaîne *target_service_name*. La comparaison est donc sensible à la casse et ne tient pas compte du classement actuel.  
  
 *service_broker_guid*  
 Spécifie la base de données qui héberge le service cible. Quand plusieurs bases de données hébergent une instance du service cible, vous pouvez communiquer avec une base de données spécifique en spécifiant un *service_broker_guid*.  
  
 *service_broker_guid* est de type **nvarchar(128)**. Pour rechercher la chaîne *service_broker_guid* pour une base de données, exécutez la requête ci-dessous dans la base de données :  
  
```sql  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID() ;  
```  
  
> [!NOTE]  
>  Cette option n'est pas disponible dans une base de données autonome.  
  
 **'** CURRENT DATABASE **'**  
 Spécifie que la conversation utilise l’identificateur *service_broker_guid* pour la base de données actuelle.  
  
 ON CONTRACT *contract_name*  
 Spécifie le contrat suivi par cette conversation. Le contrat doit déjà exister dans la base de données active. Si le service cible n'accepte pas de nouvelles conversations dans le contrat spécifié, [!INCLUDE[ssSB](../../includes/sssb-md.md)] retourne un message d'erreur sur la conversation. Si cette clause est omise, la conversation suit le contrat nommé **DEFAULT**.  
  
 RELATED_CONVERSATION **=** _related_conversation_handle_  
 Spécifie le groupe de conversations existant auquel le nouveau dialogue est ajouté. Si cette clause est spécifiée, le nouveau dialogue appartient au même groupe de conversations que le dialogue spécifié par *related_conversation_handle*. *related_conversation_handle* doit être d’un type implicitement convertible en type **uniqueidentifier**. L’instruction échoue si *related_conversation_handle* ne référence pas un dialogue existant.  
  
 RELATED_CONVERSATION_GROUP **=** _related_conversation_group_id_  
 Spécifie le groupe de conversations existant auquel le nouveau dialogue est ajouté. Si cette clause est spécifiée, le nouveau dialogue sera ajouté au groupe de conversations spécifié par *related_conversation_group_id*. *related_conversation_group_id* doit être d’un type implicitement convertible en type **uniqueidentifier**. Si *related_conversation_group_id* ne référence pas un groupe de conversations existant, Service Broker crée un groupe de conversations à l’aide de la chaîne *related_conversation_group_id* spécifiée et met en relation le nouveau dialogue avec ce groupe de conversations.  
  
 LIFETIME **=** _dialog_lifetime_  
 Spécifie la durée maximale pendant laquelle le dialogue restera ouvert. Pour que le dialogue s'achève avec succès, les deux points de terminaison doivent explicitement se terminer avant l'expiration de la durée de vie. La valeur *dialog_lifetime* doit être exprimée en secondes. La durée de vie est de type **int**. Si la clause LIFETIME n’est pas spécifiée, la durée de vie du dialogue correspond à la valeur maximale du type de données **int**.  
  
 ENCRYPTION  
 Spécifie si les messages envoyés et reçus dans ce dialogue doivent être chiffrés quand ils sont envoyés hors d’une instance de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un dialogue qui doit être chiffré est un *dialogue sécurisé*. Si ENCRYPTION = ON et que les certificats nécessaires au chiffrement ne sont pas configurés, [!INCLUDE[ssSB](../../includes/sssb-md.md)] retourne un message d'erreur sur la conversation. Si ENCRYPTION = OFF, le chiffrement est utilisé si une liaison de service distant est configurée pour *target_service_name* ; sinon, les messages sont envoyés sans être chiffrés. Si cette clause est omise, la valeur par défaut est ON.  
  
> [!NOTE]  
>  Les messages échangés avec les services dans la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne sont jamais chiffrés. Cependant, la clé principale de base de données et les certificats nécessaires au chiffrement sont requis pour les conversations qui utilisent le chiffrement si les services de la conversation se trouvent dans des bases de données distinctes. Cela permet la poursuite des conversations dans le cas où une des bases de données est déplacée vers une autre instance au cours de la conversation.  
  
## <a name="remarks"></a>Remarques  
 Tous les messages font partie intégrante d'une conversation. Par conséquent, un service initiateur doit commencer une conversation avec le service cible avant d'envoyer un message à ce dernier. Les informations spécifiées dans l'instruction BEGIN DIALOG CONVERSATION sont identiques à celles spécifiées dans l'adresse d'une lettre ; [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise ces informations pour remettre les messages au service approprié. Le service spécifié dans la clause TO SERVICE est l'adresse à laquelle les messages sont envoyés. Le service spécifié dans la clause FROM SERVICE est l'adresse de l'expéditeur utilisée pour les messages de réponse.  
  
 La cible d'une conversation n'a pas besoin d'appeler l'instruction BEGIN DIALOG CONVERSATION. [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée une conversation dans la base de données cible lorsque le premier message de la conversation est reçu de l'initiateur.  
  
 La création d'un dialogue entraîne la création d'un point de terminaison de la conversation dans la base de données pour le service initiateur, mais ne crée pas de connexion réseau à l'instance hôte du service cible. [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'établit pas la communication avec la cible du dialogue tant que le premier message n'a pas été envoyé.  
  
 Si l'instruction BEGIN DIALOG CONVERSATION ne spécifie aucune conversation associée ni aucun groupe de conversations associé, [!INCLUDE[ssSB](../../includes/sssb-md.md)] crée un nouveau groupe de conversations pour la nouvelle conversation.  
  
 [!INCLUDE[ssSB](../../includes/sssb-md.md)] n'autorise aucun regroupement arbitraire des conversations. Pour toutes les conversations d'un groupe de conversations, le service doit être spécifié dans la clause FROM en tant qu'initiateur ou cible de la conversation.  
  
 La commande BEGIN DIALOG CONVERSATION verrouille le groupe de conversations qui contient le *dialog_handle* retourné. Quand la commande comporte une clause RELATED_CONVERSATION_GROUP, le groupe de conversations pour *dialog_handle* est le groupe de conversations spécifié dans le paramètre *related_conversation_group_id*. Quand la commande comporte une clause RELATED_CONVERSATION, le groupe de conversations pour *dialog_handle* est le groupe de conversations associé au *related_conversation_handle* spécifié.  
  
 BEGIN DIALOG CONVERSATION n'est pas valide dans une fonction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Pour commencer un dialogue, l'utilisateur actuel doit disposer de l'autorisation RECEIVE sur la file d'attente du service spécifié dans la clause FROM de la commande et de l'autorisation REFERENCES pour le contrat spécifié.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-beginning-a-dialog"></a>R. Début d'un dialogue  
 L'exemple suivant commence une conversation et stocke l'identificateur du dialogue dans `@dialog_handle.` Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="b-beginning-a-dialog-with-an-explicit-lifetime"></a>B. Début d'un dialogue avec une durée de vie explicite  
 L'exemple suivant commence une conversation et stocke l'identificateur du dialogue dans `@dialog_handle`. Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`. Si le dialogue n'est pas fermé par la commande END CONVERSATION dans les `60` secondes, Service Broker met fin au dialogue en signalant une erreur.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH LIFETIME = 60 ;  
```  
  
### <a name="c-beginning-a-dialog-with-a-specific-broker-instance"></a>C. Début d'un dialogue avec une instance de Service Broker spécifique  
 L'exemple suivant commence une conversation et stocke l'identificateur du dialogue dans `@dialog_handle`. Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`. Service Broker achemine les messages de ce dialogue au service Broker identifié par le GUID `a326e034-d4cf-4e8b-8d98-4d7e1926c904.`  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses',   
              'a326e034-d4cf-4e8b-8d98-4d7e1926c904'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission] ;  
```  
  
### <a name="d-beginning-a-dialog-and-relating-it-to-an-existing-conversation-group"></a>D. Début d'un dialogue et mise en relation avec un groupe de conversations existant  
 L'exemple suivant commence une conversation et stocke l'identificateur du dialogue dans `@dialog_handle`. Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`. Service Broker associe le dialogue au groupe de conversation identifié par `@conversation_group_id` au lieu de créer un nouveau groupe de conversations.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER ;  
DECLARE @conversation_group_id UNIQUEIDENTIFIER ;  
  
SET @conversation_group_id = <retrieve conversation group ID from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION_GROUP = @conversation_group_id ;  
```  
  
### <a name="e-beginning-a-dialog-with-an-explicit-lifetime-and-relating-the-dialog-to-an-existing-conversation"></a>E. Début d'un dialogue avec une durée de vie explicite et mise en relation avec une conversation existante  
 L'exemple suivant commence une conversation et stocke l'identificateur du dialogue dans `@dialog_handle`. Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`. Le nouveau dialogue appartient au même groupe de conversations que `@existing_conversation_handle`. Si le dialogue n'est pas fermé par la commande END CONVERSATION dans les `600` secondes, [!INCLUDE[ssSB](../../includes/sssb-md.md)] met fin au dialogue en signalant une erreur.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
DECLARE @existing_conversation_handle UNIQUEIDENTIFIER  
  
SET @existing_conversation_handle = <retrieve conversation handle from database>  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH RELATED_CONVERSATION = @existing_conversation_handle  
   LIFETIME = 600 ;  
```  
  
### <a name="f-beginning-a-dialog-with-optional-encryption"></a>F. Début d'un dialogue et chiffrement facultatif  
 L'exemple suivant commence un dialogue et stocke l'identificateur du dialogue dans `@dialog_handle`. Le service `//Adventure-Works.com/ExpenseClient` est l'initiateur du dialogue et le service `//Adventure-Works.com/Expenses` est la cible du dialogue. Le dialogue respecte le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`. Dans cet exemple, la conversation permet aux messages d'être transmis sur le réseau sans être chiffrés si le chiffrement n'est pas disponible.  
  
```sql  
DECLARE @dialog_handle UNIQUEIDENTIFIER  
  
BEGIN DIALOG CONVERSATION @dialog_handle  
   FROM SERVICE [//Adventure-Works.com/ExpenseClient]  
   TO SERVICE '//Adventure-Works.com/Expenses'  
   ON CONTRACT [//Adventure-Works.com/Expenses/ExpenseSubmission]  
   WITH ENCRYPTION = OFF ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BEGIN CONVERSATION TIMER &#40;Transact-SQL&#41;](../../t-sql/statements/begin-conversation-timer-transact-sql.md)   
 [END CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/end-conversation-transact-sql.md)   
 [MOVE CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/move-conversation-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)  
  
  
