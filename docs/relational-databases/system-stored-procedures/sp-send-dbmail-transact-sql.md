---
description: sp_send_dbmail (Transact-SQL)
title: sp_send_dbmail (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 08/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sendmail_sp_TSQL
- sendmail_sp
- SP_SEND_DBMAIL_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_send_dbmail
ms.assetid: f1d7a795-a3fd-4043-ac4b-c781e76dab47
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 72da3a1013869049b350440108dc4d4b631b5712
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190833"
---
# <a name="sp_send_dbmail-transact-sql"></a>sp_send_dbmail (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  Envoie un message électronique aux destinataires spécifiés. Le message peut comprendre un ensemble de résultats de requête et des fichiers joints. Lorsque les messages sont placés dans la file d’attente Database Mail, **sp_send_dbmail** retourne la **mailitem_id** du message. Cette procédure stockée se trouve dans la base de données **msdb** .  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
sp_send_dbmail [ [ @profile_name = ] 'profile_name' ]  
    [ , [ @recipients = ] 'recipients [ ; ...n ]' ]  
    [ , [ @copy_recipients = ] 'copy_recipient [ ; ...n ]' ]  
    [ , [ @blind_copy_recipients = ] 'blind_copy_recipient [ ; ...n ]' ]  
    [ , [ @from_address = ] 'from_address' ]  
    [ , [ @reply_to = ] 'reply_to' ]   
    [ , [ @subject = ] 'subject' ]   
    [ , [ @body = ] 'body' ]   
    [ , [ @body_format = ] 'body_format' ]  
    [ , [ @importance = ] 'importance' ]  
    [ , [ @sensitivity = ] 'sensitivity' ]  
    [ , [ @file_attachments = ] 'attachment [ ; ...n ]' ]  
    [ , [ @query = ] 'query' ]  
    [ , [ @execute_query_database = ] 'execute_query_database' ]  
    [ , [ @attach_query_result_as_file = ] attach_query_result_as_file ]  
    [ , [ @query_attachment_filename = ] query_attachment_filename ]  
    [ , [ @query_result_header = ] query_result_header ]  
    [ , [ @query_result_width = ] query_result_width ]  
    [ , [ @query_result_separator = ] 'query_result_separator' ]  
    [ , [ @exclude_query_output = ] exclude_query_output ]  
    [ , [ @append_query_error = ] append_query_error ]  
    [ , [ @query_no_truncate = ] query_no_truncate ]   
    [ , [ @query_result_no_padding = ] @query_result_no_padding ]   
    [ , [ @mailitem_id = ] mailitem_id ] [ OUTPUT ]  
```  
  
## <a name="arguments"></a>Arguments  
`[ @profile_name = ] 'profile_name'` Nom du profil à partir duquel envoyer le message. Le *profile_name* est de type **sysname**, avec NULL comme valeur par défaut. Le *profile_name* doit être le nom d’un profil de Database mail existant. Quand aucun *profile_name* n’est spécifié, **sp_send_dbmail** utilise le profil privé par défaut pour l’utilisateur actuel. Si l’utilisateur n’a pas de profil privé par défaut, **sp_send_dbmail** utilise le profil public par défaut pour la base de données **msdb** . Si l’utilisateur n’a pas de profil privé par défaut et qu’il n’existe pas de profil public par défaut pour la base de données, **\@ profile_name** doit être spécifié.  
  
`[ @recipients = ] 'recipients'` Liste délimitée par des points-virgules des adresses de messagerie auxquelles envoyer le message. La liste des destinataires est de type **varchar (max)**. Bien que ce paramètre soit facultatif, au moins l’un des **\@ destinataires**, **\@ copy_recipients** ou **\@ blind_copy_recipients** doit être spécifié, ou **sp_send_dbmail** retourne une erreur.  
  
`[ @copy_recipients = ] 'copy_recipients'` Liste délimitée par des points-virgules des adresses de messagerie auxquelles le message est copié. La liste des destinataires de copie est de type **varchar (max)**. Bien que ce paramètre soit facultatif, au moins l’un des **\@ destinataires**, **\@ copy_recipients** ou **\@ blind_copy_recipients** doit être spécifié, ou **sp_send_dbmail** retourne une erreur.  
  
`[ @blind_copy_recipients = ] 'blind_copy_recipients'` Liste délimitée par des points-virgules des adresses de messagerie de copie carbone invisible du message. La liste des destinataires de la copie aveugle est de type **varchar (max)**. Bien que ce paramètre soit facultatif, au moins l’un des **\@ destinataires**, **\@ copy_recipients** ou **\@ blind_copy_recipients** doit être spécifié, ou **sp_send_dbmail** retourne une erreur.  
  
`[ @from_address = ] 'from_address'` Valeur de l' « adresse de l’adresse » du message électronique. Il s'agit d'un paramètre optionnel utilisé pour remplacer les paramètres dans le profil de messagerie. Ce paramètre est de type **varchar (max)**. Les paramètres de sécurité SMTP déterminent si ces substitutions sont acceptées. Si aucun paramètre n'est spécifié, la valeur par défaut est NULL.  
  
`[ @reply_to = ] 'reply_to'` Valeur de l’adresse de réponse du message électronique. Une seule adresse de messagerie est acceptée comme valeur valide. Il s'agit d'un paramètre optionnel utilisé pour remplacer les paramètres dans le profil de messagerie. Ce paramètre est de type **varchar (max)**. Les paramètres de sécurité SMTP déterminent si ces substitutions sont acceptées. Si aucun paramètre n'est spécifié, la valeur par défaut est NULL.  
  
`[ @subject = ] 'subject'` Objet du message électronique. L’objet est de type **nvarchar (255)**. Si l'objet est omis, « Message SQL Server » est la valeur par défaut.  
  
`[ @body = ] 'body'` Corps du message électronique. Le corps du message est de type **nvarchar (max)**, avec NULL comme valeur par défaut.  
  
`[ @body_format = ] 'body_format'` Format du corps du message. Le paramètre est de type **varchar (20)**, avec NULL comme valeur par défaut. Lorsqu'il est spécifié, les en-têtes du message sortant sont définis pour indiquer le format choisi pour le corps de message. Il peut contenir l'une des valeurs suivantes :  
  
-   TEXT  
  
-   HTML  
  
 La valeur par défaut est TEXT.  
  
`[ @importance = ] 'importance'` Est l’importance du message. Le paramètre est de type **varchar (6)**. Il peut contenir l'une des valeurs suivantes :  
  
-   Faible  
  
-   Normal  
  
-   Importante  
  
 La valeur par défaut est Normal.  
  
`[ @sensitivity = ] 'sensitivity'` Sensibilité du message. Le paramètre est de type **varchar (12)**. Il peut contenir l'une des valeurs suivantes :  
  
-   Normal  
  
-   Personnel  
  
-   Privé  
  
-   Confidentiel  
  
 La valeur par défaut est Normal.  
  
`[ @file_attachments = ] 'file_attachments'` Liste délimitée par des points-virgules des noms de fichiers à joindre au message électronique. Les fichiers de la liste doivent être spécifiés sous forme de chemins d'accès absolus. La liste des pièces jointes est de type **nvarchar (max)**. Par défaut, la messagerie de base de données limite la taille des pièces jointes à 1 Mo par fichier.  
 
 > [!IMPORTANT]
 > Ce paramètre n’est pas disponible dans Azure SQL Managed Instance, car il ne peut pas accéder au système de fichiers local.
  
`[ @query = ] 'query'` Requête à exécuter. Les résultats de la requête sont inclus dans le corps du message électronique ou attachés comme pièce jointe. La requête est de type **nvarchar (max)** et peut contenir n’importe quelle [!INCLUDE[tsql](../../includes/tsql-md.md)] instruction valide. Notez que la requête est exécutée dans une session distincte, de sorte que les variables locales du script appelant **sp_send_dbmail** ne sont pas disponibles pour la requête.  
  
`[ @execute_query_database = ] 'execute_query_database'` Contexte de base de données dans lequel la procédure stockée exécute la requête. Le paramètre est de type **sysname**, avec la valeur par défaut de la base de données actuelle. Ce paramètre s’applique uniquement si la **\@ requête** est spécifiée.  
  
`[ @attach_query_result_as_file = ] attach_query_result_as_file` Spécifie si le jeu de résultats de la requête est retourné sous la forme d’un fichier joint. *attach_query_result_as_file* est de type **bit**, avec 0 comme valeur par défaut.  
  
 Lorsque la valeur est 0, les résultats de la requête sont inclus dans le corps du message électronique, après le contenu du paramètre **\@ Body** . Lorsque la valeur est 1, les résultats sont retournés comme pièce jointe. Ce paramètre s’applique uniquement si la **\@ requête** est spécifiée.  
  
`[ @query_attachment_filename = ] query_attachment_filename` Spécifie le nom de fichier à utiliser pour le jeu de résultats de la pièce jointe de la requête. *query_attachment_filename* est de type **nvarchar (255)**, avec NULL comme valeur par défaut. Ce paramètre est ignoré lorsque *attach_query_result* a la valeur 0. Lorsque *attach_query_result* a la valeur 1 et que ce paramètre a la valeur NULL, Database mail crée un nom de fichier arbitraire.  
  
`[ @query_result_header = ] query_result_header` Spécifie si les résultats de la requête incluent des en-têtes de colonnes. La valeur query_result_header est de type **bit**. Lorsque la valeur est 1, les résultats de la requête contiennent des en-têtes de colonne, et lorsque la valeur est 0, les résultats n'incluent aucun en-tête de colonne. La valeur par défaut de ce paramètre est **1**. Ce paramètre s’applique uniquement si la **\@ requête** est spécifiée.  
 
   >[!NOTE]
   > L’erreur suivante peut se produire lors de la définition \@ de query_result_header sur 0 et la définition \@ de query_no_truncate sur 1 :
   > <br> MSG 22050, niveau 16, état 1, ligne 12 : échec de l’initialisation de la bibliothèque sqlcmd avec le numéro d’erreur-2147024809.
  
`[ @query_result_width = ] query_result_width` Largeur de ligne, en caractères, à utiliser pour la mise en forme des résultats de la requête. Le *query_result_width* est de type **int**, avec 256 comme valeur par défaut. La valeur fournie doit être comprise entre 10 et 32767. Ce paramètre s’applique uniquement si la **\@ requête** est spécifiée.  
  
`[ @query_result_separator = ] 'query_result_separator'` Caractère utilisé pour séparer les colonnes dans le résultat de la requête. Le séparateur est de type **char (1)**. La valeur par défaut est ' ' (espace).  
  
`[ @exclude_query_output = ] exclude_query_output` Spécifie s’il faut retourner la sortie de l’exécution de la requête dans le message électronique. **exclude_query_output** est de bit, avec 0 comme valeur par défaut. Lorsque ce paramètre a la valeur 0, l’exécution de la procédure stockée **sp_send_dbmail** imprime le message retourné à la suite de l’exécution de la requête sur la console. Lorsque ce paramètre est 1, l’exécution de la procédure stockée **sp_send_dbmail** n’affiche aucun des messages d’exécution de la requête sur la console.  
  
`[ @append_query_error = ] append_query_error`Spécifie s’il faut envoyer le message électronique lorsqu’une erreur est retournée par la requête spécifiée dans l’argument de **\@ requête** . **append_query_error** est de **bit**, avec 0 comme valeur par défaut. Lorsque la valeur de ce paramètre est 1, la messagerie de base de données envoie le message électronique et inclut le message d'erreur de la requête dans le corps du message. Lorsque ce paramètre a la valeur 0, Database Mail n’envoie pas le message électronique et **sp_send_dbmail** se termine par le code de retour 1, ce qui indique un échec.  
  
`[ @query_no_truncate = ] query_no_truncate` Spécifie s’il faut exécuter la requête avec l’option qui évite la troncation des types de données de longueur variable volumineuses (**varchar (max)**, **nvarchar (max)**, **varbinary (max)**, **XML**, **Text**, **ntext**, **image** et types de données définis par l’utilisateur). Lorsqu'ils sont définis, les résultats de requête n'incluent pas les en-têtes de colonne. La valeur *query_no_truncate* est de type **bit**. Lorsque la valeur est définie à 0 ou lorsqu'elle n'est pas spécifiée, les colonnes de la requête sont limitées à 256 caractères, si elle est définie à 1, les colonnes ne sont pas tronquées. La valeur par défaut de ce paramètre est 0.  
  
> [!NOTE]  
>  Lorsqu’elle est utilisée avec de grandes quantités de données, l' \@ option **query_no_truncate** consomme des ressources supplémentaires et peut ralentir les performances du serveur.  
  
`[ @query_result_no_padding ] @query_result_no_padding` Le type est bit. La valeur par défaut est 0. Lorsque vous affectez la valeur 1 à, les résultats de la requête ne sont pas remplis, ce qui réduit éventuellement la taille du fichier. Si vous affectez \@ à query_result_no_padding la valeur 1 et que vous définissez le \@ paramètre query_result_width, le paramètre \@ query_result_no_padding remplace le \@ paramètre query_result_width.  
  
 Dans ce cas, aucune erreur ne se produit.  
 
  >[!NOTE]
  > L’erreur suivante peut se produire lorsque vous affectez la valeur \@ 1 à query_result_no_padding et que vous fournissez un paramètre pour \@ query_no_truncate :
  > <br> MSG 22050, niveau 16, état 1, ligne 0 : échec de l’exécution de la requête, car les \@ options query_result_no_append et \@ query_no_truncate s’excluent mutuellement. 
  
 Si vous affectez la valeur \@ 1 à la query_result_no_padding et que vous définissez le \@ paramètre query_no_truncate, une erreur est générée.  
  
`[ @mailitem_id = ] mailitem_id [ OUTPUT ]` Le paramètre de sortie facultatif retourne la *mailitem_id* du message. Le *mailitem_id* est de type **int**.  
  
## <a name="return-code-values"></a>Codet de retour  
 Un code de retour de 0 indique le succès. Une autre valeur signifie l'échec. Le code d’erreur de l’instruction qui a échoué est stocké dans la \@ \@ variable d’erreur.  
  
## <a name="result-sets"></a>Jeux de résultats  
 En cas de succès, le message « Courrier en file d'attente » est renvoyé.  
  
## <a name="remarks"></a>Notes  
 Avant de l’utiliser, Database Mail doit être activé à l’aide de l’Assistant Configuration de Database Mail ou **sp_configure**.  
  
 **sysmail_stop_sp** arrête Database mail en arrêtant les objets Service Broker que le programme externe utilise. **sp_send_dbmail** accepte toujours le courrier lorsque Database mail est arrêté à l’aide de **sysmail_stop_sp**. Pour démarrer Database Mail, utilisez **sysmail_start_sp**.  
  
 Si le **\@ Profil** n’est pas spécifié, **sp_send_dbmail** utilise un profil par défaut. Si l'utilisateur expéditeur du message électronique a un profil privé par défaut, la messagerie de base de données utilise ce profil. Si l’utilisateur n’a pas de profil privé par défaut, **sp_send_dbmail** utilise le profil public par défaut. S’il n’existe pas de profil privé par défaut pour l’utilisateur et aucun profil public par défaut, **sp_send_dbmail** retourne une erreur.  
  
 **sp_send_dbmail** ne prend pas en charge les messages électroniques sans contenu. Pour envoyer un message électronique, vous devez spécifier au moins l’un des **\@ corps**, les **\@ requêtes**, les **\@ file_attachments** ou l' **\@ objet**. Dans le cas contraire, **sp_send_dbmail** retourne une erreur.  
  
 La messagerie de base de données utilise le contexte de sécurité [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows de l'utilisateur actuel pour contrôler l'accès aux fichiers. Par conséquent, les utilisateurs authentifiés avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification ne peuvent pas joindre de fichiers à l’aide de **\@ file_attachments**. Windows n'autorise pas [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à fournir des informations d'identification d'un ordinateur distant à un autre. De cette façon, la messagerie de base de données ne peut pas joindre de fichiers aux messages depuis un partage réseau lorsque la commande est exécutée par un ordinateur autre que celui sur lequel [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'exécute.  
  
 Si les paramètres **\@ query** et **\@ file_attachments** sont spécifiés et que le fichier est introuvable, la requête est toujours exécutée, mais le message électronique n’est pas envoyé.  
  
 Lorsqu'une requête est spécifiée, l'ensemble de résultats se présente sous la forme d'un texte inséré. Les données binaires contenues dans le résultat sont envoyées au format hexadécimal.  
  
 Les paramètres **\@ destinataires**, **\@ copy_recipients** et **\@ blind_copy_recipients** sont des listes délimitées par des points-virgules d’adresses de messagerie. Au moins un de ces paramètres doit être fourni ou **sp_send_dbmail** retourne une erreur.  
  
 Lors de l’exécution de **sp_send_dbmail** sans contexte de transaction, Database mail démarre et valide une transaction implicite. Lors de l’exécution de **sp_send_dbmail** à partir d’une transaction existante, Database mail s’appuie sur l’utilisateur pour valider ou annuler les modifications. Aucune transaction interne n'est lancée.  
  
## <a name="permissions"></a>Autorisations  
 Les autorisations d’exécution pour **sp_send_dbmail** par défaut à tous les membres du rôle de base de données **DatabaseMailUser** dans la base de données **msdb** . Toutefois, lorsque l’utilisateur qui envoie le message n’a pas l’autorisation d’utiliser le profil pour la demande, **sp_send_dbmail** retourne une erreur et n’envoie pas le message.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-sending-an-e-mail-message"></a>R. Envoi d'un message électronique  
 Cet exemple envoie un message électronique à votre ami à l’aide de l’adresse de messagerie `myfriend@Adventure-Works.com` . Le message a comme objet `Automated Success Message`. Le corps du message contient la phrase `'The stored procedure finished successfully'`.  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @body = 'The stored procedure finished successfully.',  
    @subject = 'Automated Success Message' ;  
```  
  
### <a name="b-sending-an-e-mail-message-with-the-results-of-a-query"></a>B. Envoi d'un message électronique avec les résultats d'une requête  
 Cet exemple envoie un message électronique à votre ami à l’aide de l’adresse de messagerie `yourfriend@Adventure-Works.com` . Le message a comme objet `Work Order Count` et exécute une requête qui affiche le nombre d'ordres de travail pour lesquels une date `DueDate` ne dépasse pas le 30 avril 2004 de plus de deux jours. La messagerie de base de données joint le résultat au courrier sous la forme d'un fichier texte.  
  
```  
EXEC msdb.dbo.sp_send_dbmail  
    @profile_name = 'Adventure Works Administrator',  
    @recipients = 'yourfriend@Adventure-Works.com',  
    @query = 'SELECT COUNT(*) FROM AdventureWorks2012.Production.WorkOrder  
                  WHERE DueDate > ''2004-04-30''  
                  AND  DATEDIFF(dd, ''2004-04-30'', DueDate) < 2' ,  
    @subject = 'Work Order Count',  
    @attach_query_result_as_file = 1 ;  
```  
  
### <a name="c-sending-an-html-e-mail-message"></a>C. Envoi d'un message électronique au format HTML  
 Cet exemple envoie un message électronique à votre ami à l’aide de l’adresse de messagerie `yourfriend@Adventure-Works.com` . Le message a comme objet `Work Order List` et affiche un document HTML montrant les ordres de travail pour lesquels la valeur de `DueDate` ne dépasse pas le 30 avril 2004 de plus de deux jours. La messagerie de base de données envoie le message au format HTML.  
  
```  
DECLARE @tableHTML  NVARCHAR(MAX) ;  
  
SET @tableHTML =  
    N'<H1>Work Order Report</H1>' +  
    N'<table border="1">' +  
    N'<tr><th>Work Order ID</th><th>Product ID</th>' +  
    N'<th>Name</th><th>Order Qty</th><th>Due Date</th>' +  
    N'<th>Expected Revenue</th></tr>' +  
    CAST ( ( SELECT td = wo.WorkOrderID,       '',  
                    td = p.ProductID, '',  
                    td = p.Name, '',  
                    td = wo.OrderQty, '',  
                    td = wo.DueDate, '',  
                    td = (p.ListPrice - p.StandardCost) * wo.OrderQty  
              FROM AdventureWorks.Production.WorkOrder as wo  
              JOIN AdventureWorks.Production.Product AS p  
              ON wo.ProductID = p.ProductID  
              WHERE DueDate > '2004-04-30'  
                AND DATEDIFF(dd, '2004-04-30', DueDate) < 2   
              ORDER BY DueDate ASC,  
                       (p.ListPrice - p.StandardCost) * wo.OrderQty DESC  
              FOR XML PATH('tr'), TYPE   
    ) AS NVARCHAR(MAX) ) +  
    N'</table>' ;  
  
EXEC msdb.dbo.sp_send_dbmail @recipients='yourfriend@Adventure-Works.com',  
    @subject = 'Work Order List',  
    @body = @tableHTML,  
    @body_format = 'HTML' ;  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Messagerie de base de données](../../relational-databases/database-mail/database-mail.md)   
 [Objets de configuration Database Mail](../../relational-databases/database-mail/database-mail-configuration-objects.md)   
 [Database Mail des procédures stockées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/database-mail-stored-procedures-transact-sql.md)   
 [sp_addrolemember &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addrolemember-transact-sql.md)  
  
  
