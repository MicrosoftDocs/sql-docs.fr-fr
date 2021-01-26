---
title: Configurer l’option de configuration de serveur default full-text | Microsoft Docs
description: Apprenez-en davantage sur l’option de langue de texte intégral par défaut. Découvrez comment la configurer pour spécifier la langue par défaut utilisée par SQL Server pour les index de recherche en texte intégral.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- languages [full-text search]
- default full-text language option
ms.assetid: 0fa8785b-0830-4a52-aff5-fcf8268b72fc
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 45e05505444d00607cae875b8dfd8bd07bf9190e
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783238"
---
# <a name="configure-the-default-full-text-language-server-configuration-option"></a>Configurer l'option de configuration de serveur default full-text
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l’option de configuration de serveur **default full-text** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L’option **default full-text** spécifie une valeur de langue par défaut pour les index de recherche en texte intégral. L’analyse linguistique est effectuée sur toutes les données de texte intégral indexées et elle dépend de la langue des données. La valeur par défaut de cette option est la langue du serveur. Dans le cas d’une version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le programme d’installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définit l’option **default full-text** en fonction de la langue du serveur s’il existe une correspondance appropriée. Pour une version non localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], l’anglais est la **langue de texte intégral par défaut** .  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option default full-text, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option default full-text language](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="limitations-and-restrictions"></a><a name="Restrictions"></a> Limitations et restrictions  
  
-   La valeur de l’option **default full-text** est utilisée dans un index de recherche en texte intégral quand aucune langue n’est spécifiée pour une colonne par l’option LANGUAGE **language_term** dans les instructions CREATE FULLTEXT INDEX ou ALTER FULLTEXT INDEX. Si la langue de texte intégral par défaut n'est pas prise en charge ou si le package d'analyse linguistique n'est pas disponible, l'opération CREATE ou ALTER échouera et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] retournera un message d'erreur indiquant que la langue spécifiée n'est pas valide.  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Seul un administrateur de base de données qualifié ou un spécialiste agréé doit changer cette option avancée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
-   L’option **default full-text** nécessite une valeur LCID. Pour obtenir la liste des LCID pris en charge et des langues associées, consultez [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md). D'autres langues peuvent aussi être proposées par d'autres éditeurs de logiciels. Si aucun dialecte spécifique n'est détecté, le Moteur d'indexation et de recherche en texte intégral passe automatiquement à la langue principale.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Pour configurer l'option default full-text  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Avancé** .  
  
3.  Sous Divers, utilisez l’option **Langue de texte intégral par défaut** pour spécifier une valeur de langue par défaut pour les colonnes de texte intégral indexées.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-default-full-text-language-option"></a>Pour configurer l'option default full-text  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour attribuer à l’option `default full-text` la valeur Néerlandais (`1043`).  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'show advanced options', 1 ;  
GO  
RECONFIGURE  
GO  
EXEC sp_configure 'default full-text language', 1043 ;  
GO  
RECONFIGURE  
GO  
  
```  
  
 Pour plus d’informations, consultez [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md).  
  
##  <a name="follow-up-after-you-configure-the-default-full-text-language-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option default full-text language  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [sys.fulltext_languages &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-fulltext-languages-transact-sql.md)   
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [CREATE FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/create-fulltext-index-transact-sql.md)   
 [ALTER FULLTEXT INDEX &#40;Transact-SQL&#41;](../../t-sql/statements/alter-fulltext-index-transact-sql.md)  
  
  
