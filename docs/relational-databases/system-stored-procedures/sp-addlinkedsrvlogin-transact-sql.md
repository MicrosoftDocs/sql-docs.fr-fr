---
description: sp_addlinkedsrvlogin (Transact-SQL)
title: sp_addlinkedsrvlogin (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_addlinkedsrvlogin_TSQL
- sp_addlinkedsrvlogin
dev_langs:
- TSQL
helpviewer_keywords:
- sp_addlinkedsrvlogin
ms.assetid: eb69f303-1adf-4602-b6ab-f62e028ed9f6
author: markingmyname
ms.author: maghan
ms.openlocfilehash: bcefbe0acef90deb488a1d95f162a4d94f60aa95
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99202374"
---
# <a name="sp_addlinkedsrvlogin-transact-sql"></a>sp_addlinkedsrvlogin (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Crée ou met à jour un mappage entre une connexion sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et un compte de sécurité sur un serveur distant.  
  
 ![Icône Lien de rubrique](../../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") [Conventions de la syntaxe Transact-SQL](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>Syntaxe  
  
```  
sp_addlinkedsrvlogin [ @rmtsrvname = ] 'rmtsrvname'   
     [ , [ @useself = ] { 'TRUE' | 'FALSE' | NULL } ]   
     [ , [ @locallogin = ] 'locallogin' ]   
     [ , [ @rmtuser = ] 'rmtuser' ]   
     [ , [ @rmtpassword = ] 'rmtpassword' ]   
```  
  
## <a name="arguments"></a>Arguments  
 `[ @rmtsrvname = ] 'rmtsrvname'`  
 Nom d'un serveur lié auquel s'applique le mappage de la connexion. *rmtsrvname* est de **type sysname**, sans valeur par défaut.  
  
 `[ @useself = ] { 'TRUE' | 'FALSE' | NULL }'`  
 Détermine s’il faut se connecter à *rmtsrvname* en empruntant l’identité des connexions locales ou en soumettant explicitement une connexion et un mot de passe. Le type de données est **varchar (** 8 **)**, avec true comme valeur par défaut.  
  
 La valeur TRUE spécifie que les connexions utilisent leurs propres informations d’identification pour se connecter à *rmtsrvname*, avec les arguments *rmtuser* et *rmtpassword* ignorés. FALSe spécifie que les arguments *rmtuser* et *rmtpassword* sont utilisés pour se connecter à *rmtsrvname* pour la connexion *locale locale* spécifiée. Si *rmtuser* et *rmtpassword* ont également la valeur null, aucune connexion ou aucun mot de passe n’est utilisé pour se connecter au serveur lié.  
  
 `[ @locallogin = ] 'locallogin'`  
 Connexion sur le serveur local. *LocalLogin* est de **type sysname**, avec NULL comme valeur par défaut. La valeur NULL indique que cette entrée s’applique à toutes les connexions locales qui se connectent à *rmtsrvname*. Si la valeur n’est pas NULL, la connexion *locale* peut être une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion ou une connexion Windows. La connexion Windows doit être autorisée à accéder à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] directement ou par l'intermédiaire de son appartenance à un groupe Windows qui a une autorisation d'accès.  
  
 `[ @rmtuser = ] 'rmtuser'`  
 Connexion distante utilisée pour se connecter à *rmtsrvname* lorsque la @useself valeur de est false. Lorsque le serveur distant est une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui n’utilise pas l’authentification Windows, *rmtuser* est une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion. *rmtuser* est de **type sysname**, avec NULL comme valeur par défaut.  
  
 `[ @rmtpassword = ] 'rmtpassword'`  
 Mot de passe associé à *rmtuser*. *rmtpassword* est de **type sysname**, avec NULL comme valeur par défaut.  
  
## <a name="return-code-values"></a>Codet de retour  
 0 (réussite) ou 1 (échec)  
  
## <a name="remarks"></a>Notes  
 Lorsqu'un utilisateur accède au serveur local et exécute une requête distribuée qui interroge une table sur le serveur lié, le serveur local doit se connecter au serveur lié à la place de l'utilisateur pour accéder à cette table. Utilisez sp_addlinkedsrvlogin pour spécifier les informations d'identification que le serveur local utilise pour se connecter au serveur lié.  
  
> [!NOTE]  
>  Pour créer les meilleurs plans de requête lorsque vous utilisez une table sur un serveur lié, le processeur de requêtes doit recevoir des statistiques de distribution de données du serveur lié. Les utilisateurs qui ont des autorisations limitées sur des colonnes de la table peuvent ne pas avoir d'autorisations suffisantes pour obtenir toutes les statistiques utiles, peuvent recevoir un plan de requête moins efficace et être confrontés à des performances médiocres. Si le serveur lié constitue une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], afin d'obtenir toutes les statistiques disponibles, l'utilisateur doit posséder la table ou être membre du rôle serveur fixe sysadmin, du rôle de base de données fixe db_owner ou du rôle de base de données fixe db_ddladmin sur le serveur lié. SQL Server 2012 SP1 modifie les restrictions d'autorisation pour obtenir des statistiques et autorise les utilisateurs disposant de l'autorisation SELECT à accéder aux statistiques disponibles via DBCC SHOW_STATISTICS. Pour plus d’informations, consultez la section autorisations de [DBCC SHOW_STATISTICS &#40;&#41;Transact-SQL ](../../t-sql/database-console-commands/dbcc-show-statistics-transact-sql.md).  
  
 Un mappage par défaut est créé automatiquement entre toutes les connexions sur le serveur local et les connexions distantes sur le serveur lié, par l'exécution de la procédure sp_addlinkedserver. Le mappage par défaut indique que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilise les informations d'identification de la connexion locale lors de la connexion au serveur lié. Cela équivaut à exécuter sp_addlinkedsrvlogin avec @useself défini sur **true** pour le serveur lié, sans spécifier de nom d’utilisateur local. Utilisez la procédure sp_addlinkedsrvlogin uniquement pour modifier le mappage par défaut ou ajouter de nouveaux mappages pour des connexions locales spécifiques. Pour supprimer le mappage par défaut ou tout autre mappage, utilisez la procédure sp_droplinkedsrvlogin.  
  
 Au lieu d'utiliser la procédure sp_addlinkedsrvlogin pour créer un mappage de connexion d'accès prédéterminé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser automatiquement les informations d'identification sécurisées Windows (nom et mot de passe de connexion Windows) d'un utilisateur qui émet une requête pour se connecter à un serveur lié dans les conditions suivantes :  
  
-   L'utilisateur est connecté à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant le mode d'authentification Windows.  
  
-   La délégation de compte de sécurité est disponible sur le client et le serveur demandeur.  
  
-   Le fournisseur prend en charge le mode d'authentification Windows (par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous Windows).  
  
> [!NOTE]  
>  Il n'est pas nécessaire que la délégation soit activée pour les scénarios avec un seul saut. Elle est indispensable pour les scénarios avec plusieurs sauts.  
  
 Lorsque l'authentification est effectuée par le serveur lié à l'aide des mappages définis par l'exécution de la procédure sp_addlinkedsrvlogin sur l'instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les autorisations propres à chaque objet dans la base de données distante sont déterminées par le serveur lié et non par le serveur local.  
  
 La procédure sp_addlinkedsrvlogin ne peut pas être exécutée dans une transaction définie par l'utilisateur.  
  
## <a name="permissions"></a>Autorisations  
 Nécessite l'autorisation ALTER ANY LOGIN sur le serveur.  
  
## <a name="examples"></a>Exemples  
  
### <a name="a-connecting-all-local-logins-to-the-linked-server-by-using-their-own-user-credentials"></a>R. Établissement des connexions locales au serveur lié avec leurs propres informations d'identification.  
 Le code exemple suivant crée un mappage pour que toutes les connexions au serveur local soient établies via le serveur lié `Accounts` avec leurs propres informations d'identification.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts';  
```  
  
 ou  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'true';  
```  
  
> [!NOTE]  
>  Si des mappages explicites sont créés pour des connexions individuelles, ils sont prioritaires sur les mappages globaux qui peuvent exister pour ce serveur lié.  
  
### <a name="b-connecting-a-specific-login-to-the-linked-server-by-using-different-user-credentials"></a>B. Établissement d'une connexion spécifique au serveur lié en utilisant d'autres informations d'identification.  
 Le code exemple suivant crée un mappage pour que l'utilisateur Windows `Domain\Mary` se connecte au serveur lié `Accounts` en utilisant le nom de connexion `MaryP` et le mot de passe `d89q3w4u`.  
  
```  
EXEC sp_addlinkedsrvlogin 'Accounts', 'false', 'Domain\Mary', 'MaryP', 'd89q3w4u';  
```  
  
> [!IMPORTANT]  
>  Cet exemple n'utilise pas l'authentification Windows. Les mots de passe sont transmis sans être chiffrés. Les mots de passe peuvent être visibles dans les définitions des sources de données et les scripts enregistrés sur disque, dans les sauvegardes et dans les fichiers journaux. N'utilisez jamais le mot de passe d'un administrateur pour ce type de connexion. Consultez votre administrateur réseau pour des conseils de sécurité propres à votre environnement.  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages catalogue des serveurs liés &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/linked-servers-catalog-views-transact-sql.md)   
 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)   
 [sp_droplinkedsrvlogin &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-droplinkedsrvlogin-transact-sql.md)   
 [Procédures stockées système &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)  
  
  
