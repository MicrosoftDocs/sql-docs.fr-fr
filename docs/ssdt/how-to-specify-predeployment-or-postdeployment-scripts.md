---
title: Spécifier les scripts de pré ou de post-déploiement
description: Découvrez comment utiliser les scripts de prédéploiement et de post-déploiement pour exécuter des instructions Transact-SQL avant et après l’exécution du script de déploiement principal.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 7f78f517-f13d-4f4b-84b9-e804cb490b2c
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: d4565e9b6ab4efc133aef84ff9d4d6dbbd6c501a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018109"
---
# <a name="how-to-specify-predeployment-or-postdeployment-scripts"></a>Procédure : Spécifier des scripts de prédéploiement et de post-déploiement

Les scripts de prédéploiement et de post-déploiement exécutent des instructions Transact\-SQL avant et après le script de déploiement principal, qui est généré à partir du projet de base de données. Le script de prédéploiement ne sera pas exécuté lors de la mise à jour des cibles à partir des résultats de comparaison des schémas dans Visual Studio. Un projet peut comporter un script de prédéploiement et un script de post-déploiement. Ces scripts peuvent être utilisés à de nombreuses fins. Par exemple :  
  
-   Un script de prédéploiement peut copier les données d'une table qui est modifiée dans une table temporaire avant de les remettre en forme et de les appliquer à la table modifiée dans un script de post-déploiement.  
  
-   Vous pouvez insérer les données de référence qui doivent exister dans une table dans un script de post-déploiement. Avant d'insérer les données, vérifiez si la table en contient déjà. Si la table n'est pas vide, vous devez supprimer les données existantes ou spécifier que vous souhaitez toujours recréer la base de données avant de la déployer. Ajoutez une instruction telle que la suivante à votre script de post-déploiement :  
  
```  
IF (EXISTS(SELECT * FROM [dbo].[MyReferenceTable]))  
BEGIN  
    DELETE FROM [dbo].[MyReferenceTable]  
END  
```  

## <a name="to-add-and-modify-a-pre--or-post-deployment-script"></a>Pour ajouter et modifier un script de prédéploiement ou de post-déploiement  
  
1.  Dans **l’Explorateur de solutions**, développez votre projet de base de données pour afficher le dossier Scripts.  
  
2.  Cliquez avec le bouton droit sur le dossier Scripts et sélectionnez Ajouter.  
  
3.  Sélectionnez Scripts dans le menu contextuel.  
  
4.  Sélectionnez un script de prédéploiement ou de post-déploiement. Vous pouvez également spécifier un nom qui n'est pas un nom par défaut. Pour terminer, cliquez sur Ajouter.  
  
5.  Double-cliquez sur le fichier dans le dossier Scripts.  
  
    L’Éditeur Transact\-SQL s’ouvre et présente le contenu du fichier.  
  
Utilisez la syntaxe et les variables SQLCMD dans vos scripts et définissez-les dans les propriétés du projet de base de données. Par exemple :  
  
-   Utilisez la syntaxe SQLCMD pour inclure le contenu d'un fichier dans un script de pré-déploiement ou de post-déploiement. Ces fichiers sont inclus et s'exécutent dans l'ordre dans lequel vous les définissez : `:r .\myfile.sql`  
  
-   Utilisez la syntaxe SQLCMD pour référencer une variable dans le script de post-déploiement. Vous définissez la variable SQLCMD dans les propriétés du projet ou dans un profil de publication :  
  
    ```  
    :setvar TableName MyTable  
    SELECT * FROM [$(TableName)]  
    ```  
  
Pour plus d’informations sur l’utilisation de SQLCMD dans des scripts, voir [Paramètres des projets de base de données](../ssdt/database-project-settings.md).  
  
## <a name="see-also"></a>Voir aussi  
[Développement de base de données hors connexion orienté projet](../ssdt/project-oriented-offline-database-development.md)  
  
