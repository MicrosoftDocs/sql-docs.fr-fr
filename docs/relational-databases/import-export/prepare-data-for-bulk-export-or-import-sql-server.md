---
title: Préparer des données à une exportation ou une importation en bloc
description: Cet article décrit comment planifier des opérations d’importation et d’exportation en bloc, couvre les exigences en matière de format de fichier de données et explique quand utiliser l’utilitaire bcp.
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- bulk importing [SQL Server], planning
- bulk importing [SQL Server], from a CSV file
- data formats [SQL Server], planning operations
- CSV files [SQL Server]
- quoted fields in CSV files [SQL Server]
ms.assetid: 783fd581-2e5f-496b-b79c-d4de1e09ea30
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: b900f0967dc276509cf47fd039ea8a10bbf14c83
ms.sourcegitcommit: f30b5f61c514437ea58acc5769359c33255b85b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99077147"
---
# <a name="prepare-data-for-bulk-export-or-import"></a>Préparer des données à une exportation ou une importation en bloc
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Cette section présente les points à prendre en considération lors de la planification d'opérations d'exportation en bloc et les contraintes liées aux opérations d'importation en bloc.  
  
> [!NOTE]  
>  Si vous n’êtes pas certain de savoir mettre en forme un fichier de données pour une importation en bloc, utilisez l’utilitaire **bcp** pour exporter des données de la table dans un fichier de données. La mise en forme de chaque champ de données dans ce fichier indique la mise en forme nécessaire pour importer des données en bloc dans la colonne de table correspondante. Utilisez la même mise en forme des données pour les champs de votre fichier de données.  
  
## <a name="data-file-format-considerations-for-bulk-export"></a>Considérations sur le format du fichier de données pour l'exportation en bloc  
 Avant de réaliser une opération d’exportation en bloc à l’aide de la commande **bcp** , prenez en considération les points suivants :  
  
-   Lorsque des données sont exportées vers un fichier, la commande **bcp** crée automatiquement le fichier de données en utilisant le nom de fichier spécifié. Si ce nom de fichier est déjà utilisé, les données en cours de copie en bloc dans le fichier de données remplacent le contenu existant de ce fichier.  
  
-   L'exportation en bloc depuis une table ou une vue vers un fichier de données requiert l'existence d'une autorisation SELECT sur la table ou la vue qui fait l'objet d'une copie en bloc.  
  
-   [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser des analyses parallèles pour récupérer les données. Par conséquent, il est possible que les lignes de la table exportées en bloc depuis une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n'apparaissent pas dans un ordre spécifique dans le fichier de données. Pour que les lignes de la table exportées en bloc apparaissent selon un ordre spécifique dans le fichier de données, utilisez l’option **queryout** pour effectuer une exportation en bloc depuis une requête et spécifiez une clause ORDER BY.  
  
## <a name="data-file-format-requirements-for-bulk-import"></a>Contraintes liées au format du fichier de données pour l'importation en bloc  
 Pour que les données puissent être importées depuis un fichier de données, celui-ci doit satisfaire aux conditions de base suivantes :  
  
-   Les données doivent être organisées en lignes et en colonnes.  
  
> [!NOTE]  
>  La structure du fichier de données n'a pas besoin d'être identique à la structure de la table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , car les colonnes peuvent être ignorées ou réorganisées lors de l'importation en bloc.  
  
-   Le format des données du fichier de données doit être un format pris en charge, par exemple le format caractère ou le format natif.  
  
-   Le format des données peut être le format binaire natif ou caractère, notamment le format Unicode.  
  
-   Pour que vous puissiez importer des données à l’aide d’une commande **bcp** d’une instruction BULK INSERT ou d’une instruction INSERT... SELECT * FROM OPENROWSET(BULK...), la table de destination doit déjà exister.  
  
-   Chaque champ du fichier de données doit être compatible avec la colonne correspondante de la table cible. Par exemple, un champ **int** ne peut pas être chargé dans une colonne **datetime** . Pour plus d’informations, consultez [Formats de données pour l’importation ou l’exportation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/data-formats-for-bulk-import-or-bulk-export-sql-server.md) et [Spécifier des formats de données pour la compatibilité lors de l’utilisation de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
    > [!NOTE]  
    >  Pour spécifier un sous-ensemble de lignes d’un fichier de données à importer au lieu du fichier tout entier, vous pouvez utiliser une commande **bcp** avec le commutateur **-F** *first_row* et/ou le commutateur **-L** *last_row*. Pour plus d’informations, consultez [bcp Utility](../../tools/bcp-utility.md).  
  
-   Pour importer des données à partir de fichiers de données contenant des champs de longueur fixe ou de largeur fixe, utilisez un fichier de format. Pour plus d’informations, consultez [Fichiers de format XML &#40;SQL Server&#41;](../../relational-databases/import-export/xml-format-files-sql-server.md).  
  
-  À compter de SQL Server 2017, un fichier CSV peut être utilisé comme fichier de données pour une importation en bloc de données dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Notez que la marque de fin de champ d'un fichier CSV n'est pas nécessairement une virgule. Pour être utilisable comme fichier de données pour une importation en bloc, un fichier CSV doit être conforme aux restrictions suivantes :  
  
    -   Les champs de données ne contiennent jamais la marque de fin de champ.  
  
    -   Soit toutes les valeurs d'un champ de données sont placées entre guillemets (""), soit aucune de ces valeurs n'est placée entre guillemets.  
  
     Pour effectuer une importation en bloc des données d'un fichier de table [!INCLUDE[msCoName](../../includes/msconame-md.md)] FoxPro ou Visual FoxPro (.dbf) ou d'un fichier de feuille de calcul [!INCLUDE[ofprexcel](../../includes/ofprexcel-md.md)] (.xls), vous devez convertir les données en un fichier CSV conforme aux limitations décrites précédemment. L'extension de fichier est en général .csv. Vous pouvez alors utiliser ce fichier .csv comme fichier de données dans une opération d'importation en bloc [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
     Sur les systèmes 32 bits (SQL Server 2014 et versions antérieures), il est possible d’importer des données CSV dans une table [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sans optimisation de l’importation en bloc en utilisant [OPENROWSET](../../t-sql/functions/openrowset-transact-sql.md) avec le fournisseur OLE DB pour Jet. Jet traite les fichiers texte comme des tables, à l'aide du schéma défini par un fichier schema.ini situé dans le même répertoire que la source de données.  Pour des données CSV, l’un des paramètres du fichier schema.ini doit être « FORMAT=CSVDelimited ». Pour utiliser cette solution, vous devez comprendre le fonctionnement de Jet Text IISAM (syntaxe de sa chaîne de connexion, utilisation de schema.ini, options de configuration du Registre, etc.).  Les meilleures sources d'informations sont l'Aide de Microsoft Access et les articles de la Base de connaissances. Pour plus d’informations, consultez [Initialisation du pilote de source de données texte](/office/client-developer/access/desktop-database-reference/initializing-the-text-data-source-driver), [Comment utiliser une requête distribuée de SQL Server 7.0 avec un serveur lié aux bases de données Access sécurisés](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/246255), [Procédure : utiliser le fournisseur Jet OLE DB 4.0 pour se connecter aux bases de données ISAM](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/326548), et [Comment ouvrir des fichiers texte délimités à l’aide de Text IIsam du fournisseur Jet](https://www.betaarchive.com/wiki/index.php?title=Microsoft_KB_Archive/262537).  
  
 En outre, l'importation en bloc de données depuis un fichier de données vers une table requiert le respect des points suivants :  
  
-   Les utilisateurs doivent disposer des autorisations INSERT et SELECT sur la table. Les utilisateurs ont également besoin de l'autorisation ALTER TABLE lorsqu'ils utilisent des options qui impliquent des opérations DDL (Data Definition Language), par exemple la désactivation de contraintes.  
  
-   Lorsque vous importez en bloc des données à l'aide de l'instruction BULK INSERT ou INSERT ... SELECT * FROM OPENROWSET(BULK...), le fichier de données doit être accessible pour les opérations de lecture par le profil de sécurité du processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (si l’utilisateur se connecte à l’aide de la connexion fournie par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) ou de la connexion [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows utilisée dans le cadre d’une sécurité déléguée. En outre, l'utilisateur doit disposer de l'autorisation ADMINISTER BULK OPERATIONS pour lire le fichier.  
  
> [!NOTE]  
>  L'importation en bloc dans une vue partitionnée n'est pas prise en charge et toute tentative en ce sens est vouée à l'échec.  
  
## <a name="external-resources"></a>Ressources externes  
 [Comment importer des données d'Excel vers SQL Server](https://support.microsoft.com/kb/321686)  
  
## <a name="change-history"></a>Historique des modifications  
  
|Mise à jour du contenu|  
|---------------------|  
|Ajout d'informations sur l'utilisation du fournisseur OLE DB pour Jet afin d'importer des données CSV.|  
  
## <a name="see-also"></a>Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [BULK INSERT &#40;Transact-SQL&#41;](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)   
 [Utiliser le format natif pour importer ou exporter des données &#40;SQL Server&#41;](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
