---
title: Lier des applications d’accès à SQL Server Azure SQL Database | Microsoft Docs
description: Découvrez comment lier vos tables Access aux tables migrées afin de pouvoir utiliser vos applications Access existantes avec SQL Server ou Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Access databases, linking to SQL Azure
- Access databases, linking to SQL Server
- auto-increment columns
- data types, unsupported
- hyperlink columns
- linking tables
- migrating databases, post-migration issues
- post-migration issues
- reference, post-migration issues
- refreshing linked tables
- slow performance
- unlinking tables
ms.assetid: 82374ad2-7737-4164-a489-13261ba393d4
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 53fbbd5a865594f46eacb75c9799b9c956efb2da
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100059034"
---
# <a name="linking-access-applications-to-sql-server---azure-sql-database-accesstosql"></a>Liaison d’applications Access à des Azure SQL Database SQL Server (AccessToSQL)
Si vous souhaitez utiliser vos applications Access existantes avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez lier vos tables Access d’origine aux tables migrées [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. La liaison modifie votre base de données Access afin que vos requêtes, formulaires, rapports et pages d’accès aux données utilisent les données de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Azure SQL Database ou au lieu des données de votre base de données Access.  
  
> [!NOTE]  
> Les tables Access restent dans Access, mais elles ne sont pas mises à jour avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure mises à jour. Après avoir lié les tables et vérifié les fonctionnalités, vous souhaiterez peut-être supprimer vos tables Access.  
  
## <a name="linking-access-and-sql-server-tables"></a>Liaison des tables d’accès et de SQL Server  
Lorsque vous liez une table Access à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table ou SQL Azure, le moteur de base de données Jet stocke les informations de connexion et les métadonnées de table, mais les données sont stockées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Cette liaison permet à vos applications Access de fonctionner sur les tables Access même si les tables et les données réelles sont dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure.  
  
> [!NOTE]  
> Si vous utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’authentification, votre mot de passe est stocké en texte clair sur les tables d’accès liées. Nous vous recommandons d’utiliser l’authentification Windows.  
  
**Pour lier des tables**  
  
1.  Dans l’Explorateur de métadonnées Access, sélectionnez les tables que vous souhaitez lier.  
  
2.  Cliquez avec le bouton droit sur **tables**, puis sélectionnez **lier**.  
  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Assistant Migration (SSMA) pour Access sauvegarde la table Access d’origine et crée une table liée.  
  
Une fois que vous avez lié les tables, les tables de SSMA apparaissent avec une petite icône de lien. Dans Access, les tables apparaissent avec une icône « liée », qui est un globe avec une flèche pointant vers celle-ci.  
  
Lorsque vous ouvrez une table dans Access, les données sont récupérées à l’aide d’un curseur de jeu de clés. Par conséquent, pour les tables volumineuses, toutes les données ne sont pas récupérées en même temps. Toutefois, lorsque vous parcourez la table, Access récupère des données supplémentaires si nécessaire.  
  
> [!IMPORTANT]  
> Pour lier des tables d’accès à une base de données Azure, vous avez besoin de SQL Server Native Client (SNAC) version 10,5 ou supérieure.   
> Vous pouvez obtenir la dernière version de SNAC à partir de [Microsoft® SQL Server® 2008 R2 Feature Pack](https://www.microsoft.com/download/details.aspx?id=44272).  
  
## <a name="unlinking-access-tables"></a>Dissociation des tables d’accès  
Lorsque vous dissociez une table Access d’une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table ou SQL Azure, SSMA restaure la table Access d’origine et ses données.  
  
**Pour dissocier des tables**  
  
1.  Dans l’Explorateur de métadonnées Access, sélectionnez les tables que vous souhaitez dissocier.  
  
2.  Cliquez avec le bouton droit sur **tables**, puis sélectionnez **dissocier**.  
  
## <a name="linking-tables-to-a-different-server"></a>Liaison de tables à un autre serveur  
Si vous avez lié les tables Access à une SQL Server instance et que vous souhaitez ultérieurement modifier les liens vers une autre instance, vous devez relier les tables.  
  
**Pour lier des tables à un autre serveur**  
  
1.  Dans l’Explorateur de métadonnées Access, sélectionnez les tables que vous souhaitez dissocier.  
  
2.  Cliquez avec le bouton droit sur **tables** , puis sélectionnez **dissocier**.  
  
3.  Cliquez sur le bouton **reconnecter à SQL Server** .  
  
4.  Connectez-vous à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure à laquelle vous souhaitez lier les tables Access.  
  
5.  Dans l’Explorateur de métadonnées Access, sélectionnez les tables que vous souhaitez lier.  
  
6.  Cliquez avec le bouton droit sur **tables**, puis sélectionnez **lier**.  
  
## <a name="updating-linked-tables"></a>Mise à jour des tables liées  
Si les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] définitions de table ou SQL Azure sont modifiées, vous pouvez dissocier puis lier à nouveau les tables dans SSMA à l’aide des procédures indiquées précédemment dans cette rubrique. Vous pouvez également mettre à jour les tables à l’aide d’Access.  
  
**Pour mettre à jour des tables liées à l’aide d’Access**  
  
1.  Ouvrez la base de données Access.  
  
2.  Dans la liste **objets** , cliquez sur **tables**.  
  
3.  Cliquez avec le bouton droit sur une table liée, puis sélectionnez **Gestionnaire de tables liées**.  
  
4.  Activez la case à cocher en regard de chaque table liée que vous souhaitez mettre à jour, puis cliquez sur **OK**.  
  
## <a name="possible-post-migration-issues"></a>Problèmes de postconnexion éventuels  
Les sections suivantes répertorient les problèmes qui peuvent se produire dans les applications Access existantes après la migration des bases de données d’accès à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, puis lier les tables, ainsi que les causes et les résolutions.  
  
### <a name="slow-performance-with-linked-tables"></a>Ralentissement des performances avec les tables liées  
**Cause :** Certaines requêtes peuvent être lentes après le dimensionnement pour les raisons suivantes :  
  
-   L’application dépend de fonctions qui n’existent pas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure, ce qui amène jet à extraire les tables localement pour exécuter une requête SELECT.  
  
-   Les requêtes qui mettent à jour ou suppriment de nombreuses lignes sont envoyées par jet en tant que requête paramétrable pour chaque ligne.  
  
**Résolution :** Convertissez les requêtes qui s’exécutent lentement en requêtes directes, procédures stockées ou vues. La conversion en requêtes directes présente les problèmes suivants :  
  
-   Les requêtes directes ne peuvent pas être modifiées. La modification du résultat de la requête ou l’ajout de nouveaux enregistrements doit être effectuée d’une autre façon, par exemple en ayant des boutons **modifier** ou **Ajouter** explicites dans votre formulaire qui sont liés à la requête.  
  
-   Certaines requêtes nécessitent une entrée d’utilisateur, mais les requêtes directes ne prennent pas en charge l’entrée d’utilisateur. Les entrées utilisateur peuvent être obtenues par Visual Basic pour Applications Code (VBA) qui demande des paramètres ou par un formulaire utilisé comme contrôle d’entrée. Dans les deux cas, le code VBA soumet la requête avec l’entrée utilisateur au serveur.  
  
### <a name="auto-increment-columns-are-not-updated-until-the-record-is-updated"></a>Les colonnes à incrémentation automatique ne sont pas mises à jour tant que l’enregistrement n’est pas mis à jour  
**Cause :** Après avoir appelé Recordset. AddNew dans Jet, la colonne d’incrémentation automatique est disponible avant la mise à jour de l’enregistrement. Ce n’est pas le cas dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. La nouvelle valeur de la colonne d’identité nouvelle valeur n’est disponible qu’après l’enregistrement du nouvel enregistrement.  
  
**Résolution :** Exécutez le code de Visual Basic pour Applications (VBA) suivant avant d’accéder au champ d’identité :  
  
```  
Recordset.Update  
Recordset.Move 0,  
Recordset.LastModified  
```  
  
### <a name="new-records-are-not-available"></a>Les nouveaux enregistrements ne sont pas disponibles  
**Cause :** Lorsque vous ajoutez un enregistrement à une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] table ou SQL Azure à l’aide de VBA, si le champ d’index unique de la table a une valeur par défaut et que vous n’affectez pas de valeur à ce champ, le nouvel enregistrement n’apparaît pas tant que vous n’avez pas rouvert la table dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou SQL Azure. Si vous essayez d’obtenir une valeur à partir du nouvel enregistrement, vous recevez le message d’erreur suivant :  
  
`Run-time error '3167' Record is deleted.`  
  
**Résolution :** Lorsque vous ouvrez le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SQL Azure table à l’aide de code VBA, incluez l' `dbSeeChanges` option, comme dans l’exemple suivant :  
  
`Set rs = db.OpenRecordset("TestTable", dbOpenDynaset, dbSeeChanges)`  
  
### <a name="after-migration-some-queries-will-not-allow-the-user-to-add-a-new-record"></a>Après la migration, certaines requêtes n’autorisent pas l’utilisateur à ajouter un nouvel enregistrement  
**Cause :** Si une requête n’inclut pas toutes les colonnes incluses dans un index unique, vous ne pouvez pas ajouter de nouvelles valeurs à l’aide de la requête.  
  
**Résolution :** Veillez à ce que toutes les colonnes incluses dans au moins un index unique fassent partie de la requête.  
  
### <a name="you-cannot-modify-a-linked-table-schema-with-access"></a>Vous ne pouvez pas modifier un schéma de table lié avec accès  
**Cause :** Après la migration des données et la liaison des tables, l’utilisateur ne peut pas modifier le schéma d’une table dans Access.  
  
**Résolution :** Modifiez le schéma de la table à l’aide de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , puis mettez à jour le lien dans Access.  
  
### <a name="hyperlink-functionality-is-lost-after-migrating-data"></a>La fonctionnalité de lien hypertexte est perdue après la migration des données  
**Cause :** Après la migration des données, les liens hypertexte dans les colonnes perdent leur fonctionnalité et deviennent des colonnes **nvarchar (max)** simples.  
  
**Résolution :** Aucun.  
  
### <a name="some-sql-server-data-types-are-not-supported-by-access"></a>Certains types de données SQL Server ne sont pas pris en charge par Access  
**Cause :** Si, par la suite, vous mettez à jour vos [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] tables ou SQL Azure pour qu’elles contiennent des types de données qui ne sont pas pris en charge par Access, vous ne pouvez pas ouvrir la table dans Access.  
  
**Résolution :** Vous pouvez définir une requête Access qui ne retourne que les lignes avec des types de données pris en charge.  
  
## <a name="see-also"></a>Voir également  
[Migration de bases de données Access vers SQL Server](migrating-access-databases-to-sql-server-azure-sql-db-accesstosql.md)  
  
