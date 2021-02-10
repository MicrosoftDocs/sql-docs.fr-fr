---
description: Exportation d’un inventaire des accès (AccessToSQL)
title: Exportation d’un inventaire des accès (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases
- Access databases, exporting metadata
- exporting
- exporting Access metadata
- exporting, Access metadata
- exporting, querying exported metadata
- inventories of Access databases
- querying exported metadata
ms.assetid: 7e1941fb-3d14-4265-aff6-c77a4026d0ed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 10bff5f86bec26994c143195fcbb97aa30ba709e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044749"
---
# <a name="exporting-an-access-inventory-accesstosql"></a>Exportation d’un inventaire des accès (AccessToSQL)
Si vous disposez de plusieurs bases de données Access et que vous n’êtes pas certain de celles à migrer vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez exporter un inventaire de toutes les bases de données Access dans un projet. Vous pouvez ensuite examiner et interroger les métadonnées d’inventaire pour déterminer les bases de données et les objets de ces bases de données à migrer. Cet inventaire vous permet de trouver rapidement des réponses aux questions, telles que les suivantes :  
  
-   Quelles sont les bases de données les plus volumineuses ?  
  
-   Qui possède la plupart des bases de données ?  
  
-   Quelles bases de données contiennent les mêmes tables ?  
  
-   Quelles bases de données n’ont pas été modifiées au cours des six derniers mois ?  
  
-   Quelles bases de données contiennent des informations privées ?  
  
Les exemples de requête utilisés pour répondre à ces questions sont fournis à la fin de cette rubrique.  
  
## <a name="exported-metadata"></a>Métadonnées exportées  
SSMA exporte des métadonnées sur les bases de données Access, les tables, les colonnes, les index, les clés étrangères, les requêtes, les rapports, les formulaires, les macros et les modules. Les métadonnées relatives à chacune de ces catégories d’éléments sont exportées dans une table distincte. Pour les schémas de ces tables, consultez [accéder aux schémas d’inventaire](access-inventory-schemas-accesstosql.md).  
  
## <a name="exporting-inventory-data"></a>Exportation des données d’inventaire  
Pour exporter un inventaire des accès, vous devez d’abord ouvrir ou créer un projet SSMA, puis ajouter la base de données Access que vous souhaitez analyser. Après avoir ajouté des bases de données à un projet SSMA, vous exportez les métadonnées relatives à ces bases de données vers une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données et un schéma spécifiés. Si nécessaire, SSMA crée des tables pour stocker les métadonnées. SSMA ajoute ensuite les métadonnées relatives aux bases de données Access à la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] base de données.  
  
> [!NOTE]  
> Une base de données Access peut être fractionnée en plusieurs fichiers : une base de données principale qui contient des tables et des bases de données frontales contenant des requêtes, des formulaires, des rapports, des macros, des modules et des raccourcis. Si vous souhaitez migrer une base de données fractionnée vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , ajoutez la base de données frontale à SSMA.  
  
Les instructions suivantes décrivent comment créer un projet, ajouter des bases de données au projet, vous connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , puis exporter des données d’inventaire.  
  
**Pour créer un projet**  
  
1.  Ouvrez SSMA pour accéder à.  
  
2.  Dans le menu **Fichier**, sélectionnez **Nouveau projet**.  
  
    La boîte de dialogue **Nouveau projet** apparaît.  
  
3.  Dans la zone **Nom** , tapez le nom de votre projet.  
  
4.  Dans la zone **emplacement** , entrez ou sélectionnez un dossier pour le projet.  
  
5.  Dans la zone de liste déroulante **migrer vers** , sélectionnez la version cible vers laquelle vous souhaitez effectuer la migration, puis cliquez sur **OK**.  
  
Pour plus d’informations sur la création de projets, consultez [création et gestion de projets](creating-and-managing-projects-accesstosql.md).  
  
**Pour rechercher et ajouter des bases de données**  
  
1.  Dans le menu **fichier** , cliquez sur **Rechercher des bases de données**.  
  
2.  Dans l’Assistant Rechercher des bases de données, entrez le lecteur, le chemin d’accès du fichier ou le chemin d’accès UNC que vous souhaitez rechercher. Vous pouvez également cliquer sur **Parcourir** pour sélectionner le lecteur ou le dossier réseau.  
  
3.  Cliquez sur **Ajouter** pour ajouter l’emplacement à la zone de liste.  
  
    Répétez les deux étapes précédentes pour ajouter des emplacements de recherche supplémentaires.  
  
4.  Si vous le souhaitez, ajoutez des critères de recherche pour affiner la liste des bases de données retournées.  
  
    > [!IMPORTANT]  
    > La zone **de texte tout ou partie du nom de fichier** ne prend pas en charge les caractères génériques.  
  
5.  Cliquez sur **Analyse**.  
  
    La page d’analyse s’affiche. Cela indique les bases de données qui ont été trouvées et la progression de la recherche. Pour arrêter la recherche, cliquez sur **arrêter**.  
  
6.  Dans la page Sélectionner des fichiers, sélectionnez chaque base de données que vous souhaitez ajouter au projet.  
  
    Vous pouvez utiliser les boutons **Sélectionner tout** et **Effacer tout** en haut de la liste pour sélectionner ou effacer toutes les bases de données. Vous pouvez également maintenir la touche CTRL enfoncée pour sélectionner plusieurs lignes ou maintenir la touche Maj enfoncée pour sélectionner une plage de lignes.  
  
7.  Cliquez sur **Suivant**.  
  
8.  Dans la page vérifier, cliquez sur **Terminer**.  
  
Pour plus d’informations sur l’ajout de bases de données à des projets, consultez [Ajout et suppression de fichiers de base de données Access](adding-and-removing-access-database-files-accesstosql.md).  
  
**Pour se connecter à SQL Server**  
  
1.  Dans le menu **fichier** , sélectionnez **se connecter à SQL Server**.  
  
2.  Dans la boîte de dialogue connexion, entrez ou sélectionnez le nom de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
    -   Si vous vous connectez à l’instance par défaut sur l’ordinateur local, vous pouvez entrer **localhost** ou un point (**.**).  
  
    -   Si vous vous connectez à l’instance par défaut sur un autre ordinateur, entrez le nom de l’ordinateur.  
  
    -   Si vous vous connectez à une instance nommée, entrez le nom de l’ordinateur, une barre oblique inverse et le nom de l’instance. Par exemple : MyServer\MyInstance.  
  
3.  Dans la zone **base de données** , entrez le nom de la base de données cible pour les métadonnées exportées.  
  
4.  Si votre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est configurée pour accepter les connexions sur un port autre que celui par défaut, entrez le numéro de port utilisé pour les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexions dans la zone **port du serveur** . Pour l’instance par défaut de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , le numéro de port par défaut est 1433. Pour les instances nommées, SSMA tente d’obtenir le numéro de port à partir du [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser.  
  
5.  Dans le menu déroulant **authentification** , sélectionnez le type d’authentification à utiliser pour la connexion. Pour utiliser le compte Windows actuel, sélectionnez **authentification Windows**. Pour utiliser une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] connexion, sélectionnez **SQL Server l’authentification**, puis entrez un nom d’utilisateur et un mot de passe.  
  
Pour plus d’informations sur la connexion à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , consultez [connexion à SQL Server &#40;AccessToSQL&#41;](../../ssma/access/connecting-to-sql-server-accesstosql.md).  
  
**Pour exporter des informations d’inventaire**  
  
1.  Dans l’Explorateur de métadonnées Access, développez **accès-métabase**.  
  
2.  Activez la case à cocher en regard de **bases de données**.  
  
    Pour omettre des bases de données ou des objets de base de données, développez le dossier **bases de** données, puis désactivez la case à cocher en regard de la base de données ou de l’objet de base de données.  
  
3.  Cliquez avec le bouton droit sur **bases de données** , puis sélectionnez **Exporter le schéma**.  
  
4.  Dans la boîte de dialogue **Sélectionner le schéma pour l’exportation** , sélectionnez le schéma cible pour les métadonnées exportées, puis cliquez sur **OK**.  
  
Chaque fois que vous exportez des métadonnées, SSMA ajoute les données à l’inventaire. Les données existantes dans l’inventaire ne sont pas mises à jour ou supprimées.  
  
## <a name="querying-the-exported-metadata"></a>Interrogation des métadonnées exportées  
Après avoir exporté les métadonnées relatives aux bases de données Access, vous pouvez interroger les métadonnées. Les instructions suivantes décrivent l’utilisation de la fenêtre de l’éditeur de requête dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] pour exécuter des requêtes.  
  
**Pour interroger les métadonnées**  
  
1.  Dans le **menu Démarrer** , pointez sur **tous les programmes**, sur **microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2005** ou sur Microsoft **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2008** ou sur **Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 2012**, puis cliquez sur **SQL Server Management Studio**.  
  
2.  Dans la boîte de dialogue **se connecter au serveur** , vérifiez les paramètres, puis cliquez sur **se connecter**.  
  
3.  Dans la barre d’outils Management Studio, cliquez sur **nouvelle requête** pour ouvrir l’éditeur de requête.  
  
4.  Dans la fenêtre de l’éditeur de requête, entrez une requête. Des exemples sont présentés dans la section suivante.  
  
5.  Appuyez sur la touche F5 pour exécuter la requête.  
  
## <a name="query-examples"></a>Exemples de requêtes  
Avant d’exécuter l’une des requêtes suivantes, vous devez exécuter une requête d' *database_name* pour vérifier que les requêtes sont exécutées sur la base de données qui contient les métadonnées exportées. Par exemple, si vous avez exporté les métadonnées vers une base de données nommée MyAccessMetadata, vous devez ajouter les éléments suivants au début du [!INCLUDE[tsql](../../includes/tsql-md.md)] Code :  
  
```  
USE MyAccessMetadata;  
GO  
```  
Les exemples suivants utilisent tous le schéma **dbo** . Si vous avez exporté les métadonnées vers un autre schéma, veillez à modifier le schéma lors de l’exécution de ces requêtes.  
  
### <a name="what-tables-and-columns-are-in-these-databases"></a>Quelles sont les tables et les colonnes de ces bases de données ?  
La requête suivante joint les tables qui contiennent les métadonnées de la colonne, de la table et de la base de données, puis renvoie les noms de toutes les bases de données, tables et colonnes, triées par nom de colonne :  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D.DatabaseId  
ORDER BY ColumnName;  
```  
  
### <a name="what-are-the-largest-databases"></a>Quelles sont les bases de données les plus volumineuses ?  
La requête suivante renvoie le nom de la base de données, la taille du fichier et le nombre de tables dans chaque base de données Access, triées par taille de fichier :  
  
```  
SELECT DatabaseName, FileSize, TablesCount  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileSize DESC;  
```  
  
### <a name="who-is-the-owner-of-most-of-the-databases"></a>Qui est le propriétaire de la plupart des bases de données ?  
La requête suivante renvoie le nom de la base de données et le propriétaire de chaque base de données Access, triés par propriétaire.  
  
```  
SELECT DatabaseName, FileOwner  
FROM dbo.SSMA_Access_InventoryDatabases  
ORDER BY FileOwner;  
```  
  
### <a name="which-databases-contain-the-same-tables"></a>Quelles bases de données contiennent les mêmes tables ?  
La requête suivante utilise une sous-requête pour rechercher tous les noms de tables qui apparaissent plusieurs fois dans la liste des tables, puis utilise cette liste de tables pour obtenir le nom de la base de données. Les résultats sont retournés sous la forme du nom de la base de données, puis du nom de la table et sont triés par nom de table.  
  
```  
SELECT DatabaseName, TableName   
FROM dbo.SSMA_Access_InventoryTables T  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON D.DatabaseId = T.DatabaseId  
WHERE TableName IN (  
SELECT TableName   
FROM dbo.SSMA_Access_InventoryTables  
GROUP BY TableName   
HAVING count(*)>1  
)  
ORDER BY TableName;  
```  
  
### <a name="which-databases-were-not-modified-in-the-last-six-months"></a>Quelles bases de données n’ont pas été modifiées au cours des six derniers mois ?  
La requête suivante obtient la date actuelle, obtient la valeur de mois pour il y a six mois, puis retourne une liste de bases de données dont la date de modification est supérieure à six mois.  
  
```  
SELECT DatabaseName, DateModified  
FROM dbo.SSMA_Access_InventoryDatabases  
WHERE DATEDIFF(month, DateModified, GETDATE()) > 6  
ORDER BY DateModified;  
```  
  
### <a name="which-databases-contain-private-information"></a>Quelles bases de données contiennent des informations privées ?  
Vos bases de données Access peuvent contenir des informations personnelles ou sensibles. Vous souhaiterez peut-être déplacer ces bases de données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour tirer parti de ses fonctionnalités de sécurité. Si vous savez que les colonnes contenant des données sensibles ont un nom spécifique ou contiennent des caractères spécifiques, vous pouvez utiliser une requête pour rechercher toutes les colonnes qui contiennent ces informations. Par exemple, vous pouvez rechercher toutes les colonnes qui incluent la chaîne « salary ».  La requête retourne ensuite le nom de la base de données, le nom de la table et le nom de la colonne.  
  
```  
SELECT DatabaseName, TableName, ColumnName   
FROM dbo.SSMA_Access_InventoryColumns C  
JOIN dbo.SSMA_Access_InventoryTables T  
ON C.TableId = T.TableId  
JOIN dbo.SSMA_Access_InventoryDatabases D  
ON T.DatabaseId = D. DatabaseId  
WHERE ColumnName LIKE '%salary%';  
```  
Si vous ne connaissez pas le nom de la colonne, vous pouvez écrire une requête pour retourner toutes les colonnes. Pour ce faire, supprimez la clause WHERE de la requête précédente.  
  
## <a name="see-also"></a>Voir aussi  
[Préparation des bases de données Access pour la migration](preparing-access-databases-for-migration-accesstosql.md)  
  
