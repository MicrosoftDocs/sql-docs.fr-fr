---
title: Spécifier des formats de données pour la compatibilité avec bcp
description: Pour les exportations en bloc dans SQL Server avec bcp, les formats de données peuvent être incompatibles avec la disposition attendue. Un fichier de format non-XML spécifie les formats de données pour la compatibilité.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk exporting [SQL Server], compatibility
- bulk importing [SQL Server], compatibility
- compatibility [SQL Server], data formats
- data formats [SQL Server], compatibility
- bcp utility [SQL Server], compatibility
ms.assetid: cd5fc8c8-eab1-4165-9468-384f31e53f0a
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: afdaa7a7243c87876b55cb79155e652ae029c495
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351316"
---
# <a name="specify-compatibility-data-formats-when-using-bcp-sql-server"></a>Spécifier des formats de données pour la compatibilité lors de l’utilisation de bcp (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  Cette rubrique décrit les attributs de format de données, les invites spécifiques aux champs et le stockage des données champ par champ dans un fichier de format non-XML de la commande [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]**bcp** . La compréhension de ces notions peut être utile lorsque vous exportez des données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en bloc à des fins d'importation en bloc dans un autre programme, tel qu'un autre programme de base de données. Les formats de données par défaut (natif, caractère ou Unicode) dans la table source peuvent être incompatibles avec la disposition des données attendue par l'autre programme. S'il existe une discordance lorsque vous exportez les données, vous devez décrire la disposition des données.  
  
> [!NOTE]  
>  Si vous ne maîtrisez pas les formats de données pour l’importation ou l’exportation de données, consultez [Formats de données pour l’importation ou l’exportation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md).  
  
  
##  <a name="bcp-data-format-attributes"></a><a name="bcpDataFormatAttr"></a> Attributs de format de données bcp  
 La commande **bcp** vous permet de spécifier la structure de chaque champ dans un fichier de données, selon les attributs de format de données suivants :  
  
-   type de stockage de fichier  
  
     Le *type de stockage de fichier* décrit la façon dont les données sont stockées dans le fichier de données. Les données peuvent être exportées vers un fichier de données correspondant à son type de table de base de données (format natif), dans sa représentation caractère (format caractère) ou en tant que tout type de données pour lequel la conversion implicite est prise en charge, par exemple, en copiant un type de données **smallint** comme **int**. Les types de données définis par l'utilisateur sont exportés en tant que leurs propres types de base. Pour plus d’informations, consultez [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md).  
  
-   Longueur de préfixe  
  
     Pour un stockage de fichier plus compact lors de l’exportation en bloc de données au format natif vers un fichier de données, la commande **bcp** ajoute devant chaque champ un ou plusieurs caractères indiquant la longueur du champ. Ces caractères portent le nom de *caractères de préfixe de longueur*. Pour plus d’informations, consultez [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md).  
  
-   Longueur du champ  
  
     La longueur de champ indique le nombre maximal de caractères nécessaires pour représenter les données au format caractères. La longueur de champ est déjà connue si les données sont stockées au format natif. Pour plus d’informations, consultez [Spécifier la longueur des champs au moyen de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md).  
  
-   Marque de fin de champ  
  
     Pour les champs de données caractères, des caractères de fin facultatifs vous permettent de marquer la fin de chaque champ dans un fichier de données (à l’aide d’une *marque de fin de champ*), ainsi que la fin de chaque ligne (avec une *marque de fin de ligne*). Les caractères de fin constituent un moyen d'indiquer aux programmes lisant le fichier de données la fin d'un champ ou d'une ligne et le début du suivant. Pour plus d’informations, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
  
##  <a name="overview-of-the-field-specific-prompts"></a><a name="FieldSpecificPrompts"></a> Vue d'ensemble des invites spécifiques aux champs  
 Si une commande **bcp** interactive contient l’option **in** ou **out** , mais non le commutateur de fichier de format ( **-f**) ni le commutateur de format de données ( **-n**, **-c**, **-w** ou **-N**), pour chaque colonne de la table source ou cible, la commande vous invite à fournir chacun des attributs précédents. À chaque invite, la commande **bcp** fournit une valeur par défaut basée sur le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] de la colonne de table. L’acceptation de la valeur par défaut pour toutes les invites produit le même résultat que la spécification du format natif ( **-n**) sur la ligne de commande. Chaque invite affiche une valeur par défaut entre crochets : [*valeur par défaut*]. En appuyant sur la touche Entrée, vous acceptez les valeurs par défaut affichées. Pour spécifier une valeur différente de celle par défaut, entrez cette nouvelle valeur à l'invite.  
  
### <a name="example"></a>Exemple  
 L’exemple suivant utilise la commande **bcp** pour l’exportation en bloc interactive de données à partir de la table `HumanResources.myTeam` vers le fichier `myTeam.txt` . Avant de pouvoir exécuter cet exemple, vous devez créer cette table. Pour plus d’informations sur la table et la manière de la créer, consultez [Exemple de table HumanResources.myTeam &#40;SQL Server&#41;](../../relational-databases/import-export/humanresources-myteam-sample-table-sql-server.md).  
  
 La commande ne spécifie ni un fichier de format, ni un type de données ; l’utilitaire **bcp** vous invite donc à fournir des informations de format de données. À l'invite de commandes [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows, entrez :  
  
```cmd
bcp AdventureWorks.HumanResources.myTeam out myTeam.txt -T  
```  
  
 Pour chaque colonne, l'utilitaire bcp demande des valeurs spécifiques au champ. L'exemple suivant illustre les messages spécifiques au champ pour les colonnes `EmployeeID` et `Name` de la table. En outre, il suggère le type de stockage de fichier par défaut (le format natif) pour chaque colonne. Les longueurs de préfixe des colonnes `EmployeeID` et `Name` sont 0 et 2, respectivement. L'utilisateur spécifie une virgule (`,`) en tant que marque de fin pour chaque champ.  
  
 `Enter the file storage type of field EmployeeID [smallint]:`  
  
 `Enter prefix-length of field EmployeeID [0]:`  
  
 `Enter field terminator [none]:,`  
  
 `Enter the file storage type of field Name [nvarchar]:`  
  
 `Enter prefix length of field Name [2]:`  
  
 `Enter field terminator [none]:,`  
  
 `.`  
  
 `.`  
  
 `.`  
  
 Des messages équivalents (si nécessaire) s'affichent pour chacune des colonnes de table dans l'ordre.  
  
  
##  <a name="storing-field-by-field-data-in-a-non-xml-format-file"></a><a name="FieldByFieldNonXmlFF"></a> Stockage de données champ par champ dans un fichier de format non-XML  
 Une fois toutes les colonnes de table spécifiées, la commande **bcp** vous invite à générer facultativement un fichier de format non-XML qui stocke les informations champ par champ venant d’être fournies (voir l’exemple précédent). Si vous choisissez de générer un fichier de format, vous pouvez effectuer cette opération dès que vous exportez des données vers cette table ou que vous importez des données structurées de la sorte dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
> [!NOTE]  
>  Vous pouvez utiliser le fichier de format pour importer en bloc des données à partir du fichier de données vers une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou exporter en bloc des données depuis la table, sans devoir spécifier à nouveau le format. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  
  
 L'exemple suivant crée un fichier de format non XML nommé `myFormatFile.fmt`.  
  
 `Do you want to save this format information in a file? [Y/n] y`  
  
 `Host filename: [bcp.fmt]myFormatFile.fmt`  
  
 Le nom par défaut du fichier de format est bcp.fmt, mais vous pouvez spécifier un nom différent si vous le souhaitez.  
  
> [!NOTE]  
>  Pour un fichier de données qui utilise un seul format de données pour son type de stockage de fichiers, tel qu’un format caractère ou natif, vous pouvez créer rapidement un fichier de format sans exportation ni importation de données, à l’aide de l’option **format** . Cette approche présente les avantages d'être aisée et de vous permettre de créer un fichier de format XML ou non-XML. Pour plus d’informations, consultez [Créer un fichier de format &#40;SQL Server&#41;](../../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
  
## <a name="related-tasks"></a>Tâches associées  
  
-   [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)  
  
-   [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)  
  
-   [Spécifier la longueur des champs au moyen de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-length-by-using-bcp-sql-server.md)  
  
-   [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)  
  
## <a name="related-content"></a>Contenu associé  
 Aucun.  
  
## <a name="see-also"></a>Voir aussi  
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [Formats de données pour l’importation ou l’exportation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md)   
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)  
  
  
