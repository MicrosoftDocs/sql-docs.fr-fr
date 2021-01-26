---
title: Configurer l’option de configuration de serveur user options | Microsoft Docs
description: Apprenez-en davantage sur l’option Options de l’utilisateur. Découvrez comment elle modifie les valeurs par défaut des options de traitement des requêtes que SQL Server établit pour les sessions de travail utilisateur.
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- global default for all users [SQL Server]
- users [SQL Server], global defaults
- user options option [SQL Server]
ms.assetid: cfed8f86-6bcf-4b90-88eb-9656e22d5dc5
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 71dcf7e52b1c58c7df868f502bc8de5da9892c6b
ms.sourcegitcommit: 2f3f5920e0b7a84135c6553db6388faf8e0abe67
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98783428"
---
# <a name="configure-the-user-options-server-configuration-option"></a>Configurer l'option de configuration de serveur user options
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer l'option de configuration de serveur **user options** dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../includes/tsql-md.md)]. L'option **user options** spécifie des valeurs par défaut globales pour tous les utilisateurs. Une liste d'options de traitement des requêtes par défaut est générée pour la durée d'une session de travail d'un utilisateur. L’option **user options** permet de modifier les valeurs par défaut des options SET (si les paramètres par défaut du serveur ne sont pas appropriés).  
  
 L'utilisateur peut remplacer ces valeurs par défaut à l'aide de l'instruction SET. Pour les nouvelles connexions, vous pouvez configurer dynamiquement l'option **user options** . Une fois la valeur de l'option **user options** modifiée, les nouvelles sessions de connexion utilisent le nouveau paramètre (les sessions de connexion en cours ne sont pas concernées par cette modification).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Recommandations](#Recommendations)  
  
     [Sécurité](#Security)  
  
-   **Pour configurer l'option de configuration user options, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
-   **Suivi :**  [Après avoir configuré l’option de configuration user options](#FollowUp)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="recommendations"></a><a name="Recommendations"></a> Recommandations  
  
-   Le tableau suivant répertorie et décrit les valeurs de configuration pour **user options**. Toutes les valeurs de configuration ne sont pas compatibles les unes avec les autres. Par exemple, il n'est pas possible de définir simultanément les valeurs ANSI_NULL_DFLT_ON et ANSI_NULL_DFLT_OFF.  
  
    |Valeur|Configuration|Description|  
    |-----------|-------------------|-----------------|  
    |1|DISABLE_DEF_CNST_CHK|Contrôle les opérations de vérification des contraintes provisoires ou différées.|  
    |2|IMPLICIT_TRANSACTIONS|Pour les connexions à la bibliothèque réseau dblib, contrôle si une transaction est lancée implicitement lors de l'exécution d'une instruction. Le paramètre IMPLICIT_TRANSACTIONS n'a aucun effet sur les connexions ODBC ou OLEDB.|  
    |4|CURSOR_CLOSE_ON_COMMIT|Contrôle le comportement des curseurs après une opération de validation.|  
    |8|ANSI_WARNINGS|Contrôle la troncature et la valeur NULL dans les avertissements relatifs aux fonctions d'agrégat.|  
    |16|ANSI_PADDING|Contrôle le remplissage de variables à longueur fixe.|  
    |32|ANSI_NULLS|Contrôle la gestion des valeurs NULL lors de l'utilisation d'opérateurs d'égalité.|  
    |64|ARITHABORT|Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution.|  
    |128|ARITHIGNORE|Renvoie NULL lorsqu'un dépassement de capacité ou une division par zéro se produit durant une requête.|  
    |256|QUOTED_IDENTIFIER|Établit la distinction entre les guillemets simples et doubles lors de l'évaluation d'une expression.|  
    |512|NOCOUNT|Supprime le message qui indique, à la fin de chaque instruction, le nombre de lignes affectées par l'instruction.|  
    |1 024|ANSI_NULL_DFLT_ON|Modifie le comportement de la session de façon à utiliser la compatibilité ANSI pour la possibilité de valeur NULL. Les nouvelles colonnes définies sans possibilité de valeur NULL explicite sont définies comme autorisant les valeurs NULL.|  
    |2 048|ANSI_NULL_DFLT_OFF|Modifie le comportement de la session afin de ne pas utiliser la possibilité de valeur NULL compatible ANSI. Les nouvelles colonnes définies sans possibilité de valeur NULL explicite n'autorisent pas les valeurs NULL.|  
    |4096|CONCAT_NULL_YIELDS_NULL|Renvoie NULL lors de la concaténation d'une valeur NULL avec une chaîne.|  
    |8 192|NUMERIC_ROUNDABORT|Génère une erreur lors d'une perte de précision dans une expression.|  
    |16384|XACT_ABORT|Annule une transaction si une instruction Transact-SQL déclenche une erreur d’exécution.|  
  
-   Dans l’option **user options**, les positions binaires sont identiques à celles figurant dans @@OPTIONS. Chaque connexion a sa propre fonction @@OPTIONS, qui représente l’environnement de configuration. Quand un utilisateur se connecte à une instance de \ [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], il reçoit un environnement par défaut qui attribue la valeur actuelle de l’option **user options** à la fonction @@OPTIONS. L’exécution d’instructions SET pour l’option **user options** affecte la valeur correspondante dans la fonction @@OPTIONS de la session. Toutes les connexions créées après la modification de ce paramètre reçoivent la nouvelle valeur.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Les autorisations d’exécution de **sp_configure** , sans paramètre ou avec le premier paramètre uniquement, sont accordées par défaut à tous les utilisateurs. Pour exécuter **sp_configure** avec les deux paramètres afin de modifier une option de configuration ou d’exécuter l’instruction RECONFIGURE, un utilisateur doit disposer de l’autorisation de niveau serveur ALTER SETTINGS. L'autorisation ALTER SETTINGS est implicitement détenue par les rôles serveur fixes **sysadmin** et **serveradmin** .  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Pour configurer l'option de configuration user options  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur un serveur et sélectionnez **Propriétés**.  
  
2.  Cliquez sur le nœud **Connexions** .  
  
3.  Dans la zone **Options de connexion par défaut** , sélectionnez un ou plusieurs attributs pour configurer les options de traitement par défaut des requêtes pour l’ensemble des utilisateurs connectés.  
  
     Par défaut, aucune option utilisateur n'est configurée.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-configure-the-user-options-configuration-option"></a>Pour configurer l'option de configuration user options  
  
1.  Connectez-vous au [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**. Cet exemple montre comment utiliser [sp_configure](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md) pour configurer `user options` afin de modifier le paramètre de l’option de serveur ANSI_WARNINGS.  
  
```sql  
USE AdventureWorks2012 ;  
GO  
EXEC sp_configure 'user options', 8 ;  
GO  
RECONFIGURE ;  
GO  
  
```  
  
##  <a name="follow-up-after-you-configure-the-user-options-configuration-option"></a><a name="FollowUp"></a> Suivi : Après avoir configuré l’option de configuration user options  
 Le paramètre prend effet immédiatement sans redémarrage du serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [RECONFIGURE &#40;Transact-SQL&#41;](../../t-sql/language-elements/reconfigure-transact-sql.md)   
 [Options de configuration de serveur &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)   
 [Instructions SET &#40;Transact-SQL&#41;](../../t-sql/statements/set-statements-transact-sql.md)  
  
  
