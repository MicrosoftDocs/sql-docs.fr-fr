---
title: Options de l'Éditeur Transact-SQL
description: Familiarisez-vous avec les options de l’Éditeur Transact-SQL. En savoir plus sur les propriétés d’exécution des requêtes et les propriétés des résultats de la requête et découvrir comment ajuster les valeurs.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.SqlExecutionAdvancedSettingsOption
- sql.data.tools.SqlExecutionAnsiSettingsDlg
- sql.data.tools.SqlResultToTextSettingsDlg
- sql.data.tools.SqlExecutionGeneralSettingsDlg
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.GENERAL
- sql.data.tools.unittesting.tsqleditor
- sql.data.tools.SqlResultsToGridSettingsDlg
- sql.data.tools.queryresults.executequeryfailure
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.EDITOR_TAB_AND_STATUS_BAR
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_TEXT
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ANSI
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.GENERAL
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_RESULTS.RESULTS_TO_GRID
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.ONLINE_EDITING
- VS.TOOLSOPTIONSPAGES.SQL_SERVER_TOOLS.TRANSACT-SQL_EDITOR.QUERY_EXECUTION.ADVANCED
ms.assetid: fa9a250f-7feb-433e-91bd-a09779d74c8b
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 6ae9da5c98d625843bc01bba5d12276098e3e3f1
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98594448"
---
# <a name="transact-sql-editor-options"></a>Options de l'Éditeur Transact-SQL

Cette rubrique contient des informations sur certaines options de l'Éditeur Transact-SQL. Pour définir ces options, accédez à la boîte de dialogue **Option** depuis le menu **Outils\Options**.  
  
[Exécution de la requête](#QueryExecution)  
  
[Résultats de la requête](#QueryResults)  
  
## <a name="query-execution"></a><a name="QueryExecution"></a>Exécution de la requête  
  
|Propriété|Description|  
|------------|---------------|  
|**SET ROWCOUNT**|La valeur par défaut 0 indique que SQL Server attend les résultats, tant que tous les résultats ne sont pas reçus. Spécifiez une valeur supérieure à 0 pour que SQL Server arrête la requête après avoir obtenu le nombre de lignes spécifié. Pour désactiver cette option, de manière à renvoyer toutes les lignes, spécifiez SET ROWCOUNT 0.|  
|**SET TEXTSIZE**|La valeur par défaut de 2 147 483 647 octets indique que SQL Server fournira un champ de données complet jusqu'à la limite des champs de données text, ntext, nvarchar(max) et varchar(max). Cela n'affecte pas le type de données XML. Spécifiez un nombre inférieur pour limiter les résultats en cas de valeurs importantes. Les colonnes d'une taille supérieure au nombre spécifié sont tronquées.|  
|**Délai d'exécution**|Spécifie le nombre de secondes à attendre avant d'annuler la requête. La valeur 0 indique un délai d'attente illimité ou pas de délai.|  
|**Par défaut, ouvrir les nouvelles requêtes en mode SQLCMD**|Activez cette case à cocher pour ouvrir les nouvelles requêtes en mode SQLCMD. Cette case à cocher est visible seulement lorsque la boîte de dialogue est ouverte via le menu Outils.<br /><br />Lorsque vous sélectionnez cette option, prenez en compte les limitations suivantes :<br /><br />-   IntelliSense dans l'éditeur de requête du moteur de base de données est désactivé.<br />-   L'Éditeur de requête ne s'exécutant pas à partir de la ligne de commande, vous ne pouvez pas passer de paramètres de ligne de commande tels que des variables.<br />-   L'Éditeur de requête étant incapable de répondre aux invites de système d'exploitation, vous devez prendre soin de ne pas exécuter d'instructions interactives.|  
|**SET NOCOUNT**|Empêche le message indiquant le nombre de lignes concernées par une instruction Transact-SQL d'être retourné dans le cadre des résultats. Pour plus d'informations, consultez [SET NOCOUNT](../t-sql/statements/set-nocount-transact-sql.md).|  
|**SET NOEXEC**|Si la valeur définie est **ON**, Microsoft® SQL Server™ compile chaque lot d'instructions Transact-SQL sans les exécuter. Avec la valeur **OFF**, Microsoft® SQL Server™ doit exécuter tous les lots après la compilation. Pour plus d'informations, consultez [SET NOEXEC](../t-sql/statements/set-noexec-transact-sql.md).|  
|**SET PARSEONLY**|Vérifie la syntaxe de chaque instruction Transact-SQL et renvoie tout message d'erreur éventuel sans compiler ni exécuter cette instruction. Pour plus d'informations, consultez [SET PARSEONLY](../t-sql/statements/set-parseonly-transact-sql.md).|  
|**SET CONCAT_NULL_YIELDS_NULL**|Détermine si les résultats de concaténation sont considérés comme des valeurs NULL ou des chaînes vides. Pour plus d'informations, consultez [SET CONCAT_NULL_YIELDS_NULL](../t-sql/statements/set-concat-null-yields-null-transact-sql.md).|  
|**SET ARITHABORT**|Arrête une requête lorsqu'un dépassement de capacité ou une division par zéro se produit durant son exécution. Pour plus d'informations, consultez [SET ARITHABORT](https://msdn.microsoft.com/library/aa259212(v=SQL.80).aspx).|  
|**SET SHOWPLAN_TEXT**|Empêche Microsoft® SQL Server™ d'exécuter des instructions Transact-SQL. Au lieu de cela, SQL Server retourne des informations détaillées sur l'exécution des instructions. Pour plus d'informations, consultez [SET SHOWPLAN_TEXT](../t-sql/statements/set-showplan-text-transact-sql.md).|  
|**SET STATISTICS TIME**|Affiche le nombre de millisecondes requises pour analyser, compiler et exécuter chaque instruction.|  
|**SET STATISTICS IO**|Force Microsoft® SQL Server™ à afficher des informations sur la quantité d'activité générée sur le disque par les instructions Transact-SQL.|  
|**SET TRANSACTION ISOLATION LEVEL**|Contrôle le comportement de verrouillage de transaction par défaut pour toutes les instructions Microsoft® SQL Server™ **SELECT** émises par une connexion. Pour plus d'informations, consultez  [SET TRANSACTION ISOLATION LEVEL](../t-sql/statements/set-transaction-isolation-level-transact-sql.md).|  
|**SET LOCK_TIMEOUT**|Spécifie le nombre de millisecondes qu'attend une instruction avant la libération d'un verrou. Pour plus d'informations, consultez [SET LOCK_TIMEOUT](../t-sql/statements/set-lock-timeout-transact-sql.md)|  
|**SET QUERY_GOVERNOR_COST_LIMIT**|Remplace la valeur actuellement définie pour la connexion active. Pour plus d'informations, consultez [SET QUERY_GOVERNOR_COST_LIMIT](../t-sql/statements/set-query-governor-cost-limit-transact-sql.md).|  
|**SET ANSI_DEFAULTS**|Contrôle un groupe de paramètres Microsoft® SQL Server™ qui spécifient de façon collective le comportement d'une norme SQL-92. Pour plus d'informations, consultez [SET ANSI_DEFAULTS](../t-sql/statements/set-ansi-defaults-transact-sql.md).|  
|**SET QUOTED_IDENTIFIER**|Force Microsoft® SQL Server™ à suivre les règles SQL-92 concernant les guillemets délimitant les identificateurs et les chaînes littérales. Les identificateurs entre guillemets doubles peuvent être des mots clés Transact-SQL réservés ou contenir des caractères qui ne sont généralement pas autorisés dans les règles syntaxiques Transact-SQL se rapportant aux identificateurs. Pour plus d'informations, consultez [SET QUOTED_IDENTIFIER](../t-sql/statements/set-quoted-identifier-transact-sql.md).|  
|**SET ANSI_NULL_DFLT_ON**|Modifie le comportement de la session de manière à supplanter l'acceptation par défaut des valeurs NULL dans les nouvelles colonnes lorsque l'option ANSI null default de la base de données a la valeur False. Pour plus d’informations, voir [SET ANSI_NULL_DFLT_ON](../t-sql/statements/set-ansi-null-dflt-on-transact-sql.md).|  
|**SET IMPLICIT_TRANSACTIONS**|Lorsque la valeur est **ON**, définit la connexion en mode de transaction implicite. Si la valeur est **OFF**, la connexion est remise en mode de validation automatique. Pour plus d'informations, consultez [SET IMPLICIT_TRANSACTIONS](../t-sql/statements/set-implicit-transactions-transact-sql.md).|  
|**SET CURSOR_CLOSE_ON_COMMIT**|Contrôle si un curseur est fermé ou non lorsqu'une transaction est validée. Pour plus d’informations, voir [SET CURSOR_CLOSE_ON_COMMIT](../t-sql/statements/set-cursor-close-on-commit-transact-sql.md).|  
|**SET ANSI_PADDING**|Contrôle le mode de stockage dans la colonne des valeurs dont la longueur est inférieure à la taille définie pour la colonne et de celles contenant des espaces à droite pour les données de type **char**, **varchar**, **binary** et **varbinary** . Pour plus d’informations, voir [SET ANSI_PADDING](../t-sql/statements/set-ansi-padding-transact-sql.md).|  
|**SET ANSI_WARNINGS**|Spécifie le comportement de la norme SQL-92 pour plusieurs conditions d'erreur. Pour plus d'informations, consultez [SET ANSI_WARNINGS](../t-sql/statements/set-ansi-warnings-transact-sql.md).|  
|**SET ANSI_NULLS**|Spécifie le comportement conforme à SQL-92 des opérateurs de comparaison Égal à ( **=** ) et Différent de ( **<>** ) lorsque vous les utilisez avec les valeurs Null. Pour plus d'informations, consultez [SET ANSI_NULLS](../t-sql/statements/set-ansi-nulls-transact-sql.md).|  
  
## <a name="query-results"></a><a name="QueryResults"></a>Résultats de la requête  
  
|Propriété|Description|  
|------------|---------------|  
|**Inclure la requête dans l'ensemble de résultats**|Retourne le texte de la requête dans l'ensemble de résultats.|  
|**Inclure des en-têtes de colonne lors de la copie ou de l'enregistrement de résultats**|Permet d'inclure les en-têtes (titres) des colonnes lorsque les résultats sont copiés dans le Presse-papiers ou enregistrés dans un fichier. Désactivez cette case à cocher si vous voulez que les résultats copiés ou enregistrés contiennent seulement les données, sans les en-têtes des colonnes.|  
|**Ignorer les résultats après l'exécution**|Permet de libérer de la mémoire en ignorant les résultats de la requête une fois qu'ils ont été affichés à l'écran.|  
|**Afficher les résultats dans un onglet séparé**|Permet d'afficher l'ensemble de résultats dans une nouvelle fenêtre de document et non pas en bas de la fenêtre de requête de document.|  
|**Basculer vers l'onglet des résultats après l'exécution de la requête**|Affiche automatiquement l'ensemble de résultats à l'écran.|  
|**Nombre maximal de caractères récupérés**|Données non-XML :<br /><br />Entrez un nombre compris entre 1 et 65 535 pour indiquer le nombre maximal de caractères affichés dans chaque cellule. **Remarque :** Si vous précisez un nombre trop élevé, les données de l'ensemble de résultats risquent d'être tronquées à l'affichage. Le nombre maximal de caractères affichés dans chaque cellule dépend de la taille de police. Si les ensembles de résultats retournés sont volumineux, il est préférable de ne pas spécifier une valeur trop élevée sans quoi vous risquez d'être confronté à une mémoire insuffisante pour l'exécution de SQL Server Management Studio ou à une dégradation des performances système.<br /><br />Données XML :<br /><br />Sélectionnez 1 Mo, 2 Mo ou 5 Mo. Sélectionnez Illimité pour récupérer tous les caractères.|  
|**Format de sortie**|Par défaut, la sortie est affichée sous la forme de colonnes créées par le remplissage des résultats à l'aide d'espaces. Il est également possible de séparer les colonnes à l'aide de virgules, de tabulations ou d'espaces. Activez la case à cocher **Séparateur personnalisé** pour définir un autre **caractère de délimitation** dans la zone de même nom.|  
|**Séparateur personnalisé**|Indiquez le caractère à utiliser pour séparer les colonnes. Cette option n'est disponible que lorsque la case à cocher **Séparateur personnalisé** est activée dans la zone **Format de sortie** .|  
|**Inclure des en-têtes de colonne dans l'ensemble de résultats**|Désactivez cette case à cocher si vous ne voulez pas que chaque colonne soit étiquetée au moyen d'un titre de colonne.|  
|**Défilement pendant réception des résultats**|Activez cette case à cocher pour cibler l'affichage sur les derniers enregistrements retournés au bas de la fenêtre. Désactivez-la pour cibler l'affichage sur les premières lignes retournées.|  
|**Aligner les valeurs numériques à droite**|Activez cette case à cocher pour aligner les valeurs numériques à droite de la colonne. Cette option peut faciliter l'analyse de nombres comportant un nombre de décimales invariable.|  
|**Ignorer les résultats après l'exécution de la requête**|Libère de la mémoire en ignorant les résultats de la requête une fois ceux-ci affichés à l'écran.|  
|**Afficher les résultats dans un onglet séparé**|Activez cette case à cocher pour afficher l'ensemble de résultats dans une nouvelle fenêtre de document au lieu de les afficher en bas de la fenêtre de la requête.|  
|**Basculer vers l'onglet des résultats après l'exécution de la requête**|Cliquez sur cette option pour que l'affichage soit automatiquement ciblé sur l'ensemble de résultats.|  
|**Nombre maximal de caractères affichés dans chaque colonne**|Cette valeur est par défaut 256. Augmentez cette valeur pour afficher des jeux de résultats plus grands sans les tronquer.|  
|**Rétablir les valeurs par défaut**|Rétablit toutes les valeurs par défaut initiales des options de cette page.|  
