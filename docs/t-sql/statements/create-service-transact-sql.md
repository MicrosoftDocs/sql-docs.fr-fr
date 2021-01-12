---
description: CREATE SERVICE (Transact-SQL)
title: CREATE SERVICE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- CREATE_SERVICE_TSQL
- SERVICE
- CREATE SERVICE
- SERVICE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- services [Service Broker], creating
- CREATE SERVICE statement
- contracts [Service Broker], service creation
ms.assetid: fb804fa2-48eb-4878-a12f-4e0d5f4bc9e3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f4c3d54c74d21994974ea1cdf47dbea07d8171ca
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98096991"
---
# <a name="create-service-transact-sql"></a>CREATE SERVICE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Crée un nouveau service. Un service [!INCLUDE[ssSB](../../includes/sssb-md.md)] est un nom pour une tâche ou un ensemble de tâches spécifiques. [!INCLUDE[ssSB](../../includes/sssb-md.md)] utilise le nom du service pour acheminer les messages, les remettre à la file d'attente appropriée dans une base de données et appliquer le contrat pour une conversation.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```syntaxsql
CREATE SERVICE service_name  
   [ AUTHORIZATION owner_name ]  
   ON QUEUE [ schema_name. ]queue_name  
   [ ( contract_name | [DEFAULT][ ,...n ] ) ]  
[ ; ]  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>Arguments
 *service_name*  
 Nom du service à créer. Un nouveau service est créé dans la base de données active et il appartient au principal spécifié dans la clause AUTHORIZATION. Les noms du serveur, de la base de données et du schéma ne peuvent pas être spécifiés. *service_name* doit être un **sysname** valide.  
  
> [!NOTE]  
> Ne créez pas de service qui utilise le mot clé ANY pour *service_name*. Quand vous spécifiez `ANY` pour un nom de service dans `CREATE BROKER PRIORITY`, la priorité est considérée pour tous les services. Elle n'est pas limitée à un service dont le nom est ANY.  
  
 AUTHORIZATION *owner_name*  
 Définit le propriétaire du service comme étant l'utilisateur ou le rôle de la base de données spécifié. Quand l’utilisateur actuel est **dbo** ou **sa**, l’argument *owner_name* peut être le nom de n’importe quel utilisateur ou rôle valide. Sinon, *owner_name* doit être le nom de l’utilisateur actuel, le nom d’un utilisateur pour lequel l’utilisateur actuel a l’autorisation IMPERSONATE ou le nom d’un rôle auquel appartient l’utilisateur actuel.  
  
 ON QUEUE [ _schema_name_**.** ] *queue_name*  
 Spécifie la file d'attente qui reçoit les messages pour le service. Cette file d'attente doit exister dans la même base de données que celle du service. Si aucun argument *schema_name* n’est fourni, le schéma est le schéma par défaut de l’utilisateur qui exécute l’instruction.  
  
 *contract_name*  
 Spécifie un contrat pour lequel ce service peut être une cible. Les programmes de service initient des conversations avec le service à l'aide des contrats spécifiés. Si aucun contrat n'est précisé, le service peut seulement initier des conversations.  
  
 **[** DEFAULT **]**  
 Indique que le service peut être une cible pour des conversations respectant le contrat DEFAULT. Dans le contexte de cette clause, DEFAULT n'est pas un mot clé et il doit être délimité comme un identificateur. Le contrat DEFAULT autorise les deux côtés de la conversation à envoyer des messages de type DEFAULT. Le type de message DEFAULT utilise la validation NONE.  
  
## <a name="remarks"></a>Notes  
 Un service expose les fonctionnalités fournies par les contrats avec lesquels il est associé, pour qu'ils puissent être utilisés par d'autres services. L’instruction `CREATE SERVICE` spécifie les contrats pour lesquels ce service est la cible. Un service est une cible exclusivement pour les conversations qui utilisent les contrats spécifiés par le service. Un service qui ne spécifie aucun contrat n'expose aucune fonctionnalité aux autres services.  
  
 Les conversations lancées à partir de ce service peuvent utiliser n'importe quel contrat. Vous créez un service sans spécifier de contrat lorsque le service initie seulement des conversations.  
  
 Lorsque [!INCLUDE[ssSB](../../includes/sssb-md.md)] accepte une nouvelle conversation d'un service distant, le nom du service cible détermine la file d'attente où Service Broker place les messages dans la conversation.  
  
## <a name="permissions"></a>Autorisations  
 L’autorisation de création d’un service est accordée aux membres du rôle de base de données fixe `db_ddladmin` ou `db_owner` ou aux membres du rôle serveur fixe `sysadmin`. L’utilisateur exécutant l’instruction `CREATE SERVICE` doit disposer de l’autorisation `REFERENCES` pour la file d’attente et tous les contrats spécifiés.  
  
 L’autorisation `REFERENCES` pour un service est accordée par défaut au propriétaire du service, aux membres du rôle de base de données fixe `db_ddladmin` ou `db_owner` ou aux membres du rôle serveur fixe `sysadmin`. Les autorisations `SEND` pour un service sont accordées par défaut au propriétaire du service, aux membres du rôle de base de données fixe `db_owner` ou aux membres du rôle serveur fixe `sysadmin`.  
  
 Un service ne peut pas être un objet temporaire. Les noms de service commençant par **#** sont autorisés, mais ce sont des objets permanents.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-creating-a-service-with-one-contract"></a>R. Création d'un service avec un contrat  
 L'exemple suivant crée le service `//Adventure-Works.com/Expenses` dans la file d'attente `ExpenseQueue` du schéma `dbo`. Les dialogues qui ciblent ce service doivent respecter le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses]  
    ON QUEUE [dbo].[ExpenseQueue]  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission]) ;  
```  
  
### <a name="b-creating-a-service-with-multiple-contracts"></a>B. Création d'un service avec plusieurs contrats  
 L'exemple suivant crée le service `//Adventure-Works.com/Expenses` dans la file d'attente `ExpenseQueue`. Les dialogues qui ciblent ce service doivent respecter le contrat `//Adventure-Works.com/Expenses/ExpenseSubmission` ou le contrat `//Adventure-Works.com/Expenses/ExpenseProcessing`.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue  
    ([//Adventure-Works.com/Expenses/ExpenseSubmission],  
     [//Adventure-Works.com/Expenses/ExpenseProcessing]) ;  
```  
  
### <a name="c-creating-a-service-with-no-contracts"></a>C. Création d'un service sans contrat  
 L’exemple suivant crée la file d’attente `//Adventure-Works.com/Expenses on the ExpenseQueue` du service. Ce service ne possède pas d'informations de contrat. Par conséquent, il ne peut être que l'initiateur d'un dialogue.  
  
```sql  
CREATE SERVICE [//Adventure-Works.com/Expenses] ON QUEUE ExpenseQueue ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/alter-service-transact-sql.md)   
 [DROP SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/drop-service-transact-sql.md)   
 [EVENTDATA &#40;Transact-SQL&#41;](../../t-sql/functions/eventdata-transact-sql.md)  
  
  
