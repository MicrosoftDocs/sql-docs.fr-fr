---
title: Boîte de dialogue Ajouter une référence de base de données (boîte de dialogue )
description: En savoir plus sur les références de base de données et leur utilisation. Affichez les procédures que vous pouvez effectuer dans la boîte de dialogue Ajouter une référence de base de données.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.adddatabasereference.dialog
- sql.data.tools.newdatabase.dialog
- sql.data.tools.criticalerror.dialog
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: feb7df193ea54f4c7184066bb38cc5cc700f9bf9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079660"
---
# <a name="add-database-reference-dialog-box"></a>Boîte de dialogue Ajouter une référence de base de données (boîte de dialogue )

Cette rubrique décrit les procédures que vous pouvez effectuer dans la boîte de dialogue **Ajouter une référence de base de données**.  
  
Les références de base de données vous permettent de :  
  
Accéder aux objets dans d'autres bases de données.  
Un projet peut référencer une autre base de données sur n'importe quel serveur à l'aide d'une résolution de noms en trois ou quatre parties. Lorsque vous utilisez un nom en trois ou quatre parties pour une référence, vous pouvez utiliser les variables SQLCMD pour autoriser l'utilisation des références pour travailler sur plusieurs serveurs et bases de données.  
  
Créer une solution composite pour plusieurs projets de base de données.  
Dans un projet composite, les références de bases de données partitionnent une base de données de grande taille en plusieurs projets distincts. Vous créez une référence qui ne contient pas de variables ni de valeurs pour la base de données ou le serveur (en utilisant uniquement des noms en une ou deux parties).  
  
Les références de bases de données peuvent être créées dans un projet de base de données au sein de la solution actuelle ou bien dans un fichier DACPAC. L'ajout d'une référence de base de données à un projet modifie les dépendances du projet et l'ordre de création.  
  
## <a name="selecting-the-database-to-reference"></a>Sélection de la base de données à référencer

Vous pouvez référencer un autre projet de base de données dans la même solution, dans une base de données système ou dans un fichier DACPAC.  
  
S'il existe plus d'un projet de base de données dans votre solution, l'option **Projets de base de données dans la solution actuelle** est activée. Vous pouvez référencer une autre base de données dans la solution.  
  
Sélectionnez **Base de données système** si vous souhaitez sélectionner l'une des bases de données système comme référence de base de données.  
  
Sélectionnez **Application de la couche Données (fichier .dacpac)** si vous référencez une base de données dans un fichier DACPAC, puis accédez au répertoire qui contient le fichier DACPAC.  
  
## <a name="selecting-the-databases-relative-location"></a>Sélection de l’emplacement relatif de la base de données

Après avoir sélectionné la base de données que vous souhaitez référencer, vous pouvez spécifier l'emplacement attendu de l'objet de base de données, par rapport au projet de référencement.  
  
Les références peuvent être résolues pour des objets dans l'un des emplacements suivants :  
  
- Dans la base de données de référencement.  
  
- Dans une autre base de données que la base de données de référencement, mais sur le même serveur.  
  
- Dans une autre base de données que la base de données de référencement, mais sur un serveur différent.  
  
Spécifiez un nom de base de données. Si vous avez choisi **Base de données système**, vous ne devez modifier aucune option du littéral de la base de données système. Si vous avez choisi **Projets de base de données dans la solution actuelle**, le nom de la base de données par défaut est basé sur le nom de la base de données du projet.  
  
Si vous avez sélectionné **Autre base de données, autre serveur**, vous devez spécifier un nom de serveur.  
  
Vous pouvez une variable de base de données (SQLCMD). Si vous souhaitez référencer une base de données avec une variable, au lieu du nom littéral, acceptez ou modifiez le nom de variable de base de données par défaut. Une variable de base de données est utile si vous publiez le projet de base de données sur plusieurs serveurs et bases de données. Dans ce cas de figure, un développeur peut accéder aux **Variables SQLCMD** dans la page des propriétés du projet et spécifier le nom local de la base de données. Si vous ne renseignez pas la **Variable de base de données**, vous pouvez uniquement référencer la base de données par son nom littéral. Si vous spécifiez un nom de variable de base de données, vous ne pouvez pas référencer la base de données par son nom littéral.  
  
Si vous avez sélectionné **Autre base de données, autre serveur**, une variable serveur (SQLCMD) est requise. Une variable de serveur est utile pour publier le projet de base de données sur plusieurs serveurs et bases de données. Dans ce cas de figure, un développeur peut accéder aux **Variables SQLCMD** dans la page des propriétés du projet et spécifier le nom local du serveur. Vous pouvez uniquement référencer le serveur avec la variable, et non avec le nom du serveur.  
  
> [!IMPORTANT]  
> Dans certains cas, vous pouvez créer une référence de base de données qui a le même nom qu'une référence de base de données existante. Deux références de bases de données avec le même nom peuvent aboutir à un comportement inattendu. Dans ce cas, supprimez les deux références de bases de données.  
  
## <a name="common-procedures"></a>Procédures courantes

Voici les procédures courantes :  
  
### <a name="to-create-a-reference-to-a-database-on-the-same-server"></a>Pour créer une référence à une base de données sur le même serveur  
  
1.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence de base de données**.  
  
2.  Dans la boîte de dialogue **Ajouter une référence de base de données**, spécifiez l'**Emplacement de la base de données** comme **Autre base de données, même serveur**.  
  
3.  Spécifiez le nom de la base de données.  
  
4.  Décidez si vous souhaitez utiliser une variable de base de données.  
  
5.  Copiez l'exemple d'utilisation et collez-le dans votre fichier .sql. Dans l'exemple d'utilisation, remplacez [Schema1] par le nom de votre schéma (par exemple, [dbo]). Modifiez également le nom de l'objet de la base de données comme il convient pour votre projet.  
  
6.  Générez la solution.  
  
### <a name="to-create-a-reference-to-a-database-on-another-server"></a>Pour créer une référence à une base de données sur un autre serveur  
  
1.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence de base de données**.  
  
2.  Dans la boîte de dialogue **Ajouter une référence de base de données**, spécifiez l'**Emplacement de la base de données** comme **Autre base de données, autre serveur**.  
  
3.  Vérifiez que le nom de la base de données est correct.  
  
4.  Décidez si vous souhaitez utiliser une variable de base de données.  
  
5.  Spécifiez le nom du serveur et la variable du serveur.  
  
6.  Copiez l'exemple d'utilisation et collez-le dans votre fichier .sql. Dans l'exemple d'utilisation, remplacez [Schema1] par le nom de votre schéma (par exemple, [dbo]). Modifiez également le nom de l'objet de la base de données comme il convient pour votre projet.  
  
7.  Générez la solution.  
  
### <a name="to-create-a-composite-project"></a>Pour créer un projet composite  
  
1.  Dans l'Explorateur de solutions, cliquez avec le bouton droit sur **Références**, puis cliquez sur **Ajouter une référence de base de données**.  
  
2.  Sélectionnez la source de la base de données à la quelle vous faites référence (un projet dans la solution ou un fichier DACPAC).  
  
3.  Dans la boîte de dialogue **Ajouter une référence de base de données**, spécifiez l'**Emplacement de la base de données** comme **Même base de données**.  
  
4.  Copiez l'exemple d'utilisation et collez-le dans votre fichier .sql. Dans l'exemple d'utilisation, remplacez [Schema1] par le nom de votre schéma. Modifiez également le nom de l'objet de la base de données comme il convient pour votre projet.  
  
5.  Générez la solution.  
  
Lorsque vous publiez ce projet, vous pouvez déployer de projets composites dans la même solution en cible unique :  
  
1.  Cliquez avec le bouton droit sur le nom du projet dans l’**Explorateur de solutions**, pointez sur **Publier** pour afficher le boîte de dialogue **Publication de base de données**.  
  
2.  Dans la boîte de dialogue **Publier la base de données**, cliquez sur **Paramètres avancés**.  
  
3.  Dans la boîte de dialogue **Options de publication avancées**, assurez-vous que **Inclure des objets composite** est activé dans la liste **Options de déploiement avancées**.  
  
## <a name="see-also"></a>Voir aussi

[Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md)