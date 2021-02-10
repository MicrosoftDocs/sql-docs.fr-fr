---
title: Fonctionnalités d’accès incompatibles (AccessToSQL) | Microsoft Docs
description: Découvrez les problèmes de migration possibles avec les fonctionnalités de base de données Access qui ne sont pas compatibles avec SQL Server et comment les résoudre.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, features incompatible with SQL Azure
- Access databases, features incompatible with SQL Server
- dates
- default expressions
- foreign keys
- hyperlink columns
- incompatible Access features
- indexes
- indexes, length of
- keywords
- primary keys
- reference, incompatible features
- replication columns
- special characters
- unique indexes
- validation rules
ms.assetid: 99d45b9c-e3b9-4d56-8c25-b594b887ace1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 492add4bd73d81c8c25256e35ff1b3de37fc13de
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044529"
---
# <a name="incompatible-access-features-accesstosql"></a>Fonctionnalités d’accès incompatibles (AccessToSQL)
Toutes les fonctionnalités de base de données Access ne sont pas compatibles avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Par exemple, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et l’accès ont différents jeux de mots clés réservés. Des problèmes tels que ceux-ci peuvent empêcher la réussite de la migration vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Utilisez le tableau suivant pour en savoir plus sur les problèmes de migration possibles et sur ce que vous pouvez en faire.  
  
## <a name="database-settings-or-features-that-might-affect-migration"></a>Paramètres de base de données ou fonctionnalités susceptibles d’affecter la migration  
  
|Paramètre ou fonctionnalité de base de données Access|Problème de migration|  
|--------------------------------------|-------------------|  
|Les tables Access n’ont pas d’index uniques.|Si une table qui n’a pas d’index unique est migrée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous ne pouvez pas modifier la table après la migration. Cela peut entraîner des problèmes de compatibilité des applications.<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie répertorie les tables Access qui n’ont pas d’index uniques.<br /><br />Vous pouvez configurer l’accès pour ajouter une clé primaire sur la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table lors de la conversion. Pour plus d’informations, consultez [paramètres du projet (conversion)](./project-settings-conversion-accesstosql.md).|  
|Les tables Access ont des colonnes de réplication.|Si une table Access incluant des colonnes système de réplication est migrée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , la fonctionnalité de réplication Jet est interrompue après la migration.<br /><br />Après la migration, envisagez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’utiliser la réplication pour gérer les copies synchronisées de vos bases de données.|  
|Les tables Access qui possèdent des index uniques contiennent plusieurs valeurs NULL.|Vous ne pouvez pas transférer des tables qui ont des index uniques avec plusieurs valeurs null [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , car dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les index uniques interdisent plusieurs valeurs NULL. La migration va échouer pour ces tables.<br /><br />SSMA va signaler ce problème dans les rapports d’évaluation. Pour créer un rapport d’évaluation, consultez [évaluation des objets de base de données Access pour la conversion](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />Si ce problème persiste, vous devez vous assurer que la clé primaire n’a pas de valeurs NULL dupliquées. Ou, vous devez supprimer la clé primaire ou les index uniques qui contiennent plusieurs valeurs NULL.|  
|Les tables Access contiennent des valeurs de date qui sont en dehors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] plage.|Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] type **DateTime** accepte uniquement les dates comprises entre le 1er janvier 1753 et le 31 décembre 9999. L’accès accepte les dates comprises dans la plage comprise entre le 1er janvier 100 et le 31 décembre 9999.<br /><br />SSMA va signaler ce problème dans les rapports d’évaluation. Pour créer un rapport d’évaluation, consultez [évaluation des objets de base de données Access pour la conversion](assessing-access-database-objects-for-conversion-accesstosql.md).<br /><br />Vous pouvez configurer la façon dont SSMA résout les dates qui se trouvent en dehors de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] plage. Pour plus d’informations, consultez [paramètres du projet (migration)](./project-settings-migration-accesstosql.md).|  
|Les longueurs d’index dans Access dépassent 900 octets.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les index ont une limite de 900 octets pour la taille totale des colonnes de clés d’index. Si vos tables Access utilisent des index plus volumineux, SSMA affiche un avertissement.<br /><br />Si vous poursuivez la migration des données, la migration risque d’échouer.|  
|Les noms d’objets Access sont des [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Mots clés ou contiennent des caractères spéciaux.|Access et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont des jeux différents de mots clés réservés et de caractères spéciaux. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] accepte les objets nommés à l’aide de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Mots clés ou qui contiennent des caractères spéciaux si vous utilisez des identificateurs entre crochets ou entre guillemets, tels que « SELECT » ou [Select]. p. Pour plus d’informations, consultez « identificateurs délimités (Moteur de base de données) » dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] documentation en ligne de.<br /><br />**Remarque :** Pour utiliser des guillemets pour délimiter les identificateurs, affectez la valeur ON à QUOTED_IDENTIFIER.<br /><br />Par exemple, `CREATE TABLE [schema](c1 [FOR])` est une instruction valide, même si le **schéma** et le sont **des** Mots clés réservés. En outre, `CREATE TABLE [xxx*yyy](c1 x&y)` est une instruction valide, même si le nom de la table et de la colonne contient les caractères spéciaux **\& #42 ;** et **&amp;** .<br /><br />Toutes les requêtes qui font référence à ces objets doivent également utiliser les noms avec des crochets ou des guillemets. Par exemple, la requête `SELECT * FROM schema` échouera. La requête correcte est : `SELECT * FROM [schema]` .<br /><br />Lorsque vous convertissez des objets de base de données Access, le volet de sortie répertorie les tables Access qui utilisent des mots clés ou des caractères spéciaux. Vous pouvez modifier les tables dans Access, puis supprimer et rajouter la base de données. vous pouvez aussi modifier les requêtes qui font référence à ces objets afin que les requêtes utilisent des crochets ou des guillemets pour délimiter les identificateurs. Si vous ne modifiez pas vos requêtes, vos applications Access peuvent retourner des erreurs ou rencontrer d’autres problèmes.|  
|Les tailles de champ diffèrent dans les relations de clé primaire/clé étrangère.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge la fonctionnalité jet de liaison de colonnes avec des types de données ou des tailles différents avec des contraintes de clé étrangère.<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie répertorie les contraintes de clé primaire/clé étrangère qui ne seront pas converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez modifier les types de données et les tailles des colonnes d’accès afin qu’elles correspondent, puis supprimer et rajouter la base de données Access. Ou bien, vous pouvez migrer des données bien que ces contraintes ne soient pas créées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .|  
|Les tables référencées dans les relations d’accès ne possèdent pas de clé primaire ni d’index unique.|L’accès accepte la relation entre les tables où la table référencée n’a pas de clé primaire ou d’index unique. Toutefois, cela n’est pas pris en charge par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .<br /><br />Lorsque vous convertissez des objets de base de données Access, la fenêtre Sortie répertorie toutes les tables qui ont des relations, mais pas de clé primaire ou d’index unique. Vous pouvez modifier les tables pour ajouter des clés primaires ou des index uniques, puis supprimer et rajouter la base de données Access. Ou bien, vous pouvez migrer des données même si la relation entre les tables est rompue.|  
|Les tables Access ont des colonnes de liens hypertexte.|[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prend pas en charge les colonnes de **lien hypertexte** . Au lieu de cela, les colonnes sont traitées comme des colonnes de mémo d’accès. Par défaut, ces colonnes sont converties en colonnes **nvarchar (max)** dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Vous pouvez personnaliser le mappage. Pour plus d’informations, consultez [mappage des types de données source et cible](mapping-source-and-target-data-types-accesstosql.md).|  
|Les expressions de règle de validation ou par défaut contiennent des fonctions d’accès qui ne peuvent pas être converties en [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.|L’accès aux expressions par défaut ou aux règles de validation peut inclure des fonctions système d’accès ou des fonctions définies par l’utilisateur qui ne sont pas mappées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. L’utilisation de fonctions qui ne mappent pas à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure vous empêche de charger les expressions ou les règles de validation par défaut dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.|  
  
## <a name="see-also"></a>Voir aussi  
[Préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md)  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
