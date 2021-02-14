---
title: Mettre à jour AZDATA_PASSWORD
description: Mettre à jour le `AZDATA_PASSWORD` manuellement
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 12/19/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 7fe856ae92c198d44ed0b11d1d2ce808e87fb4eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100039679"
---
# <a name="manually-update-azdata_password"></a>Mettre à jour manuellement `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Que le cluster fonctionne ou non avec l’intégration Active Directory, `AZDATA_PASSWORD` est défini lors du déploiement. Il fournit une authentification de base au contrôleur de cluster et à l’instance maître. Ce document explique comment mettre à jour manuellement `AZDATA_PASSWORD`.

## <a name="change-azdata_password-for-controller"></a>Modifier `AZDATA_PASSWORD` pour le contrôleur

Si le cluster fonctionne en mode non Active Directory, mettez à jour le mot de passe de la passerelle Apache Knox en procédant comme suit :

1. Obtenez les informations d’identification SQL Server du contrôleur en exécutant les commandes suivantes :

   a. Exécutez cette commande en tant qu’administrateur Kubernetes :

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. Décoder le secret au format Base64 :
   
   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. Dans une fenêtre de commande distincte, exposez le port du serveur de base de données du contrôleur :

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. Utilisez le mot de passe de l’administrateur système, que vous venez d’obtenir, pour vous connecter au serveur de base de données du contrôleur à partir d’un outil client SQL.

1. Générez un nouveau mot de passe complexe pour `AZDATA_USERNAME` pour remplacer le `AZDATA_PASSWORD` existant.

   Pour simplifier l’exemple, les étapes suivantes utilisent « newPassword », car le mot de passe généré est « newPassword ». 

1. Obtenez `hexsalt` à partir de la table utilisateurs :

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` renvoie une chaîne hexadécimale aléatoire (par exemple, `64FC59DF31244FFEE02F457BC0750226`).

1. Chiffrez le nouveau mot de passe complexe à l’aide de `hexsalt` :

   Pour plus de commodité, nous fournissons un outil prédéfini `pbkdf2` pour chiffrer le mot de passe. Téléchargez l’application .NET Core appropriée à la plateforme pour [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries).

   L’application est autonome et ne nécessite pas de composants requis, tels que les runtimes .NET. Chiffrer le mot de passe, exécutez :

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. Mettez à jour le mot de passe dans la table utilisateurs :

   ```SQL
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Modifiez `AZDATA_PASSWORD` dans l’instance maître SQL Server

1. Connectez-vous au point de terminaison SQL maître avec n’importe quel utilisateur administrateur.

1. Pour modifier le mot de passe des informations d’identification de connexion que vous avez définies pendant le déploiement dans le paramètre `AZDATA_USERNAME`, exécutez la commande TSQL suivante :

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```
