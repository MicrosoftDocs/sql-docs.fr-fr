---
title: Génération de commandes avec CommandBuilders
description: Explique comment utiliser des générateurs de commandes pour générer automatiquement des commandes INSERT, UPDATE et DELETE pour un `DataAdapter` qui a une commande SELECT de table unique.
ms.date: 11/25/2020
dev_langs:
- csharp
ms.assetid: 6e3fb8b5-373b-4f9e-ab03-a22693df8e91
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-chmalh
ms.openlocfilehash: d738650db9dbb7b0b2ed3f9411945173f73f748f
ms.sourcegitcommit: c938c12cf157962a5541347fcfae57588b90d929
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/25/2020
ms.locfileid: "97771303"
---
# <a name="generating-commands-with-commandbuilders"></a>Génération de commandes avec CommandBuilders

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

Lorsque la propriété `SelectCommand` de l’objet <xref:System.Data.Common.DbDataAdapter> est spécifiée dynamiquement au moment de l’exécution, par exemple par le biais d’un outil de requête qui prend une commande textuelle de l’utilisateur, vous ne pourrez peut-être pas spécifier les `InsertCommand`, `UpdateCommand` ou `DeleteCommand` appropriés au moment de la conception. Si l'objet <xref:System.Data.DataTable> est mappé à une table de base de données unique ou est généré par elle, vous pouvez tirer parti de l'objet <xref:System.Data.Common.DbCommandBuilder> pour générer automatiquement les propriétés `DeleteCommand`, `InsertCommand` et `UpdateCommand` de l'objet <xref:System.Data.Common.DbDataAdapter>.

> [!NOTE]
> Dans le Fournisseur de données Microsoft SqlClient pour SQL Server, la classe <xref:Microsoft.Data.SqlClient.SqlDataAdapter> est dérivée de la classe <xref:System.Data.Common.DbDataAdapter> et la classe <xref:Microsoft.Data.SqlClient.SqlCommandBuilder> est dérivée de la classe <xref:System.Data.Common.DbCommandBuilder>.

Vous devez au minimum définir la propriété `SelectCommand` afin que la génération de automatique de commandes fonctionne. Le schéma de table extrait par la propriété `SelectCommand` détermine la syntaxe des instructions INSERT, UPDATE et DELETE générées automatiquement.

L'objet <xref:System.Data.Common.DbCommandBuilder> doit exécuter `SelectCommand` afin de retourner les métadonnées nécessaires à la construction des commandes INSERT, UPDATE et DELETE SQL. En conséquence, un trajet supplémentaire vers la source de données est nécessaire et peut gêner les performances. Pour parvenir à une performance optimale, spécifiez vos commandes explicitement plutôt que d'utiliser l'objet <xref:System.Data.Common.DbCommandBuilder>.

> [!NOTE]
> `SelectCommand` doit aussi retourner au moins une clé primaire ou une colonne unique. Si aucun n’est présent, une exception `InvalidOperation` est générée et les commandes ne sont pas générées.

Lorsqu'il est associé à `DataAdapter`, l'objet <xref:System.Data.Common.DbCommandBuilder> génère automatiquement les propriétés `InsertCommand`, `UpdateCommand` et `DeleteCommand` du `DataAdapter` si ce sont des références null. Si une `Command` existe déjà pour une propriété, la `Command` existante est utilisée.

Les vues de base de données qui sont créées en reliant deux ou plusieurs tables ne sont pas considérées comme une table de base de données unique. Dans ce cas, vous ne pouvez pas utiliser la <xref:System.Data.Common.DbCommandBuilder> pour générer automatiquement des commandes. Vous devez spécifier vos commandes explicitement. Pour plus d’informations sur la définition explicite de commandes pour résoudre les mises à jour d’un `DataSet` dans la source de données, consultez [Mise à jour de sources de données avec des DataAdapter](update-data-sources-with-dataadapters.md).

Il est possible de mapper les paramètres de sortie à la ligne mise à jour d'un `DataSet`. Une tâche courante consisterait à extraire la valeur d'un champ Identité généré automatiquement ou d'un horodatage provenant de la source de données. L'objet <xref:System.Data.Common.DbCommandBuilder> ne mappera pas les paramètres de sortie aux colonnes dans une ligne mise à jour par défaut. Dans ce cas, vous devez spécifier votre commande explicitement. Pour obtenir un exemple de mappage d’un champ Identité généré automatiquement à une colonne dans une ligne insérée, voir [Extraction de l’identité ou de valeurs à numérotation automatique](retrieve-identity-or-autonumber-values.md).

## <a name="rules-for-automatically-generated-commands"></a>Règles des commandes générées automatiquement

Le tableau suivant présente les règles de génération des commandes générées automatiquement.

|Commande|Règle|  
|-------------|----------|  
|`InsertCommand`|Insère une ligne dans la source de données pour toutes les lignes de la table dont la propriété <xref:System.Data.DataRow.RowState%2A> a la valeur <xref:System.Data.DataRowState.Added>. Insère des valeurs pour toutes les colonnes qui peuvent être mises à jour (mais pas les colonnes comme les identités, les expressions ou les horodatages).|  
|`UpdateCommand`|Met à jour les lignes dans la source de données pour toutes les lignes de la table dont la propriété `RowState` a la valeur <xref:System.Data.DataRowState.Modified>. Met à jour les valeurs de toutes les colonnes à l'exception de celles qui ne peuvent pas être mises à jour, telles que les identités ou les expressions. Met à jour toutes les lignes où les valeurs de colonne au niveau de la source de données correspondent aux valeurs de colonne de clé primaire de la ligne et où les colonnes restantes correspondent aux valeurs d'origine de la ligne. Pour plus d'informations, voir « Modèle d'accès simultané optimiste pour les mises à jour et les suppressions », plus loin dans cette rubrique.|  
|`DeleteCommand`|Supprime les lignes dans la source de données pour toutes les lignes de la table dont la propriété `RowState` a la valeur <xref:System.Data.DataRowState.Deleted>. Supprime toutes les lignes où les valeurs de colonne correspondent aux valeurs de colonne de clé primaire de la ligne et où les colonnes restantes de la source de données correspondent aux valeurs d'origine de la ligne. Pour plus d’informations, consultez [Modèle d’accès concurrentiel optimiste pour les mises à jour et les suppressions](#optimistic-concurrency-model-for-updates-and-deletes), plus loin dans cette rubrique.|

## <a name="optimistic-concurrency-model-for-updates-and-deletes"></a>Modèle d’accès concurrentiel optimiste pour les mises à jour et les suppressions

La logique de génération automatique de commandes pour les instructions UPDATE et DELETE est fondée sur *l’accès concurrentiel optimiste*. Cela signifie que l'édition des enregistrements n'est pas verrouillée et qu'ils peuvent être modifiés par d'autres utilisateurs ou processus à tout moment. Puisqu'un enregistrement aurait pu être modifié après avoir été retourné de l'instruction SELECT mais avant que l'instruction UPDATE ou DELETE ne soit émise, l'instruction UPDATE ou DELETE générée automatiquement contient une clause WHERE spécifiant qu'une ligne n'est mise à jour que si elle contient toutes les valeurs d'origine et n'a pas été supprimée de la source de données. Cela permet d'éviter le remplacement de nouvelles données.
 
> [!NOTE]
> Quand une mise à jour générée automatiquement tente de mettre à jour une ligne qui a été supprimée ou qui ne contient pas les valeurs d'origine qui se trouvent dans l'objet <xref:System.Data.DataSet>, la commande n'affecte pas d'enregistrement et un objet <xref:System.Data.DBConcurrencyException> est généré.

Si vous souhaitez que les instructions UPDATE ou DELETE soient exécutées indépendamment des valeurs d'origine, vous devez explicitement définir la propriété `UpdateCommand` pour le `DataAdapter` et ne pas compter sur la génération automatique de commandes.

## <a name="limitations-of-automatic-command-generation-logic"></a>Limitations de la logique de génération automatique de commandes

Les limites suivantes s'appliquent à la génération automatique de commandes.

### <a name="unrelated-tables-only"></a>Tables sans relation uniquement

La logique de génération automatique de commandes génère des instructions INSERT, UPDATE ou DELETE pour les tables autonomes sans prendre en compte les relations avec les autres tables au niveau de la source de données. En conséquence, il est possible que l'appel à `Update`, pour soumettre les modifications pour une colonne qui participe à une contrainte de clé étrangère dans la base de données, échoue. Pour éviter cette exception, n'utilisez pas l'objet <xref:System.Data.Common.DbCommandBuilder> pour mettre à jour les colonnes impliquées dans une contrainte de clé étrangère et spécifiez plutôt explicitement les instructions utilisées pour effectuer l'opération.

### <a name="table-and-column-names"></a>Noms de tables et de colonnes

La logique de génération automatique de commandes peut échouer si les noms de colonne ou de table contiennent des caractères spéciaux (notamment espaces, points, points d'interrogation ou autres caractères non alphanumériques), même s'ils sont délimités par des crochets. Selon le fournisseur, le fait de définir les paramètres QuotePrefix et QuoteSuffix peut permettre à la logique de génération de traiter les espaces, mais pas les caractères spéciaux d'échappement. Les noms de tables complètes sous la forme *catalog.schema.table* sont pris en charge.

## <a name="use-the-commandbuilder-to-automatically-generate-an-sql-statement"></a>Utilisation de CommandBuilder pour générer automatiquement une instruction SQL

Pour générer automatiquement des instructions SQL pour un `DataAdapter`, commencez par définir la propriété `SelectCommand` du `DataAdapter`, puis créez un objet `CommandBuilder` et spécifiez comme argument le `DataAdapter` pour lequel l’objet `CommandBuilder` génère automatiquement des instructions SQL.

[!code-csharp[SqlCommandBuilder_Create#1](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#1)]

## <a name="modify-the-selectcommand"></a>Modifier la SelectCommand

Si vous modifiez le `CommandText` du `SelectCommand` après la génération automatique des commandes INSERT, UPDATE ou DELETE, une exception peut se produire. Si le `SelectCommand.CommandText` modifié contient des informations de schéma qui ne sont pas cohérentes par rapport au `SelectCommand.CommandText` utilisé lors de la génération automatique des commandes d'insertion, de mise à jour ou de suppression, les appels ultérieurs à la méthode `DataAdapter.Update` peuvent chercher à accéder à des colonnes qui n'existent plus dans la table actuelle référencée par `SelectCommand` et une exception sera levée.

Vous pouvez actualiser les informations de schéma utilisées par `CommandBuilder` pour générer automatiquement les commandes en appelant la méthode `RefreshSchema` du `CommandBuilder`.

Si vous souhaitez connaître les commandes générées automatiquement, vous pouvez en obtenir une référence en appliquant les méthodes `GetInsertCommand`, `GetUpdateCommand` et `GetDeleteCommand` de l'objet `CommandBuilder` et en vérifiant la propriété `CommandText` de la commande associée.

L'exemple de code suivant écrit sur la console la commande de mise à jour générée automatiquement.

[!code-csharp[SqlCommandBuilder_Create#2](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#2)]

L'exemple suivant crée à nouveau la table dans le jeu de données. La méthode **RefreshSchema** est appelée pour actualiser les commandes générées automatiquement avec ces nouvelles informations de colonne.

[!code-csharp[SqlCommandBuilder_Create#3](~/../sqlclient/doc/samples/SqlCommandBuilder_Create.cs#3)]

## <a name="see-also"></a>Voir aussi

- [Commandes et paramètres](commands-parameters.md)
- [Exécution d'une commande](execute-command.md)
- [Microsoft ADO.NET pour SQL Server](microsoft-ado-net-sql-server.md)
