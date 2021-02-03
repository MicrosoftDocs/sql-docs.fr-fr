---
description: Obtenir des informations au sujet d'une vue
title: Obtenir des informations au sujet d’une vue | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
f1_keywords:
- sql13.swb.viewproperties.general.f1
helpviewer_keywords:
- views [SQL Server], status information
- metadata [SQL Server], views
- dependencies [SQL Server], views
- displaying view information
- views [SQL Server], metadata
- viewing view information
- status information [SQL Server], views
- view dependencies
ms.assetid: 05a73e33-8f85-4fb6-80c1-1b659e753403
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: ae6a5c1c83832dc5c50d19289caec44df57ebe0f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195809"
---
# <a name="get-information-about-a-view"></a>Obtenir des informations au sujet d'une vue
[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Vous pouvez obtenir des informations sur la définition ou les propriétés d’une vue dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en utilisant [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous devrez peut-être examiner la définition de la vue pour comprendre comment les données de celle-ci sont issues des tables source ou pour connaître les données définies par la vue.  
  
> [!IMPORTANT]  
>  Si vous modifiez le nom d'un objet auquel une vue fait référence, vous devez modifier la vue pour que son texte reflète le nouveau nom de l'objet. Dès lors, avant de renommer un objet, affichez ses dépendances afin de déterminer si les vues sont affectées par la modification envisagée.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Sécurité](#Security)  
  
-   **Pour obtenir des informations sur une vue, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 L'utilisation de `sp_helptext` pour retourner la définition d'une vue nécessite l'appartenance au rôle **public** . L'utilisation de `sys.sql_expression_dependencies` pour rechercher toutes les dépendances d'une vue nécessite l'autorisation VIEW DEFINITION sur la base de données et l'autorisation SELECT sur `sys.sql_expression_dependencies` pour la base de données. Les définitions d'objets système, telles que celles retournées dans SELECT OBJECT_DEFINITION, sont visibles publiquement.  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="get-view-properties-by-using-object-explorer"></a>Obtenir les propriétés de vue à l'aide de l'Explorateur d'objets  
  
1.  Dans l' **Explorateur d'objets**, cliquez sur le signe plus (+) en regard de la base de données qui contient la vue dont vous souhaitez afficher les propriétés, puis cliquez sur le signe plus (+) pour développer le dossier **Vues** .  
  
2.  Cliquez avec le bouton droit sur la vue dont vous voulez afficher les propriétés, puis sélectionnez **Propriétés**.  

     Les propriétés suivantes s'affichent dans la boîte de dialogue **Propriétés de la vue** .  
  
     **Sauvegarde de la base de données**  
     Nom de la base de données contenant cette vue.  
  
     **Serveur**  
     Nom de l'instance actuelle du serveur.  
  
     **Utilisateur**  
     Nom de l'utilisateur de cette connexion.  
  
     **Date de création**  
     Affiche la date de création de la vue.  
  
     **Nom**  
     Nom de la vue actuelle.  
  
     **Schéma**  
     Affiche le schéma propriétaire de la vue.  
  
     **Objet système**  
     Indique si la vue est un objet système. Les valeurs sont True et False.  
  
     **Valeurs ANSI NULL**  
     Indique si l'objet a été créé avec l'option Valeurs ANSI NULL.  
  
     **Chiffré**  
     Indique si la vue est chiffrée. Les valeurs sont True et False.  
  
     **Identificateur entre guillemets**  
     Indique si l'objet a été créé avec l'option Identificateurs entre guillemets.  
  
     **Lié(e) au schéma**  
     Indique si la vue est liée au schéma. Les valeurs sont True et False. Pour plus d’informations sur les vues liées au schéma, consultez la partie SCHEMABINDING de [CREATE VIEW &#40;Transact-SQL&#41;](../../t-sql/statements/create-view-transact-sql.md).  
  
#### <a name="getting-view-properties-by-using-the-view-designer-tool"></a>Obtention des propriétés d'une vue à l'aide de l'outil Concepteur de vues  
  
1.  Dans l' **Explorateur d'objets**, développez la base de données qui contient la vue dont vous souhaitez afficher les propriétés, puis développez le dossier **Vues** .  
  
2.  Cliquez avec le bouton droit sur la vue dont vous voulez afficher les propriétés, puis sélectionnez **Conception**.  
  
3.  Cliquez avec le bouton droit dans l’espace du volet Schéma et cliquez sur **Propriétés**.  
  
     Les propriétés suivantes sont affichées dans le volet **Propriétés** .  
  
     **(Nom)**  
     Nom de la vue actuelle.  
  
     **Database Name**  
     Nom de la base de données contenant cette vue.  
  
     **Description**  
     Brève description de la vue actuelle.  
  
     **Schéma**  
     Affiche le schéma propriétaire de la vue.  
  
     **Nom de serveur**  
     Nom de l'instance actuelle du serveur.  
  
     **Lier au schéma**  
     Empêche les utilisateurs de modifier les objets sous-jacents qui contribuent à cette vue de quelque manière qui invaliderait la définition de la vue.  
  
     **Déterministe**  
     Indique si le type de données de la colonne sélectionnée peut être déterminé avec certitude  
  
     **Valeurs distinctes**  
     Spécifie que la requête filtrera les doublons dans la vue. Cette option est utile lorsque vous utilisez uniquement certaines colonnes d'une table et que ces colonnes peuvent contenir des valeurs dupliquées, ou encore lorsque le processus de jointure de plusieurs tables crée des lignes en double dans le jeu de résultats. Choisir cette option équivaut à insérer le mot-clé DISTINCT dans l’instruction à l’intérieur du volet SQL.  
  
     **Extension GROUP BY**  
     Spécifie que d'autres options sont disponibles pour les vues basées sur des requêtes d'agrégation.  
  
     **Sélectionne toutes les colonnes**  
     Indique si toutes les colonnes sont retournées par la vue sélectionnée. Ce comportement est défini lorsque la vue est créée.  
  
     **Commentaire SQL**  
     Affiche une description des instructions SQL. Pour afficher l’intégralité de la description ou la modifier, cliquez sur la description, puis sur le bouton de sélection **(...)** situé à droite de la propriété. Vos commentaires peuvent contenir les noms des utilisateurs de la vue et le moment d'utilisation.  
  
     **Spécification Top**  
     Se développe pour afficher les propriétés **Top**, **Expression**, **Pourcentage** et **With Ties** .  
  
     **(Top)**  
     Indique que la vue doit contenir une clause TOP, qui ne retourne que les n premières lignes ou n premiers pour cent des lignes du jeu de résultats. Par défaut, la vue retourne les 10 premières lignes dans le jeu de résultats. Utilisez cette option pour modifier le nombre de lignes à retourner ou pour spécifier un autre pourcentage.  
  
     **Expression**  
     Affiche le pourcentage (si **Pourcentage** a la valeur **Oui**) ou les enregistrements (si **Pourcentage** a la valeur **Non**) que la vue retourne.  
  
     **Pourcentage**  
     Indique que la requête doit contenir une clause **TOP** , qui ne retourne que les n premiers pour cent des lignes du jeu de résultats.  
  
     **Avec liens**  
     Spécifie que la vue inclura une clause **WITH TIES** . **WITH TIES** est utile si une vue inclut une clause **ORDER BY** et une clause **TOP** basée sur un pourcentage. Si cette option est activée et si le pourcentage s'arrête au milieu d'un groupe de lignes auxquelles correspondent des valeurs identiques dans la clause **ORDER BY** , la vue est agrandie de façon à inclure ces lignes.  
  
     **Spécification de mise à jour**  
     Se développe pour afficher les propriétés des propriétés **Mettre à jour en utilisant les règles de vue** et **Option Vérifier** .  
  
     **(Mettre à jour en utilisant les règles de vue)**  
     Indique que toutes les mises à jour et insertions apportées à la vue seront traduites par Microsoft Data Access Components (MDAC) en instructions SQL qui font référence à la vue, plutôt qu'en instructions SQL qui font directement référence aux tables de base de la vue.  
  
     Dans certains cas, les manifestes MDAC considèrent les opérations de mise à jour et d'insertion dans une vue comme des mises à jour et des insertions dans les tables de base sous-jacentes de la vue. En sélectionnant **Mettre à jour en utilisant les règles de vue**, vous pouvez vous assurer que MDAC génère les opérations de mise à jour et d'insertion sur la vue proprement dite.  
  
     **Option Vérifier**  
     Indique que quand vous ouvrez cette vue et modifiez le volet de **Résultats** , la source de données vérifie si les données ajoutées ou modifiées respectent la clause **WHERE** de la définition de la vue. Si votre modification ne respecte pas la clause **WHERE** , un message d'erreur contenant des informations supplémentaires s'affiche.  
  
#### <a name="to-get-dependencies-on-the-view"></a>Pour obtenir les dépendances de la vue  
  
1.  Dans l' **Explorateur d'objets**, développez la base de données qui contient la vue dont vous souhaitez afficher les propriétés, puis développez le dossier **Vues** .  
  
2.  Cliquez avec le bouton droit sur la vue dont vous voulez afficher les propriétés, puis sélectionnez **Afficher les dépendances**.  
  
3.  Sélectionnez **Objets dépendant de [nom de la vue]** pour afficher les objets qui font référence à la vue.  
  
4.  Sélectionnez **Objets dont dépend [nom de la vue]** pour afficher les objets référencés par la vue.  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-get-the-definition-and-properties-of-a-view"></a>Pour obtenir la définition et les propriétés d'une vue  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'un des exemples suivants dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT definition, uses_ansi_nulls, uses_quoted_identifier, is_schema_bound  
    FROM sys.sql_modules  
    WHERE object_id = OBJECT_ID('HumanResources.vEmployee');   
    GO  
    ```  
  
    ```  
    USE AdventureWorks2012;   
    GO  
    SELECT OBJECT_DEFINITION (OBJECT_ID('HumanResources.vEmployee')) AS ObjectDefinition;   
    GO  
    ```  
  
    ```  
    EXEC sp_helptext 'HumanResources.vEmployee';  
    ```  
  
 Pour plus d’informations, consultez [sys.sql_modules &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-modules-transact-sql.md), [OBJECT_DEFINITION &#40;Transact-SQL&#41;](../../t-sql/functions/object-definition-transact-sql.md) et [sp_helptext &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-helptext-transact-sql.md).  
  
#### <a name="to-get-the-dependencies-of-a-view"></a>Pour obtenir les dépendances d'une vue  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    USE AdventureWorks2012;  
    GO  
    SELECT OBJECT_NAME(referencing_id) AS referencing_entity_name,   
        o.type_desc AS referencing_desciption,   
        COALESCE(COL_NAME(referencing_id, referencing_minor_id), '(n/a)') AS referencing_minor_id,   
        referencing_class_desc, referenced_class_desc,  
        referenced_server_name, referenced_database_name, referenced_schema_name,  
        referenced_entity_name,   
        COALESCE(COL_NAME(referenced_id, referenced_minor_id), '(n/a)') AS referenced_column_name,  
        is_caller_dependent, is_ambiguous  
    FROM sys.sql_expression_dependencies AS sed  
    INNER JOIN sys.objects AS o ON sed.referencing_id = o.object_id  
    WHERE referencing_id = OBJECT_ID(N'Production.vProductAndDescription');  
    GO  
    ```  
  
 Pour plus d’informations, consultez [sys.sql_expression_dependencies &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-sql-expression-dependencies-transact-sql.md) et [sys.objects &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-objects-transact-sql.md).  
  
  
