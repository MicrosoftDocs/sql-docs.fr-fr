---
title: Mettre à jour AZDATA_PASSWORD
description: Mettre à jour le `AZDATA_PASSWORD` manuellement
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/01/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 062e574772c2a44b78772da4a979c81ed3deb959
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836246"
---
# <a name="manually-update-azdata_password"></a>Mettre à jour manuellement `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Que `AZDATA_PASSWORD` fonctionne ou non avec l’intégration Active Directory, [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] est défini lors du déploiement. Il fournit une authentification de base au contrôleur de cluster et à l’instance maître. Ce document explique comment mettre à jour manuellement `AZDATA_PASSWORD`.

## <a name="change-azdata_password-for-controller"></a>Modifier `AZDATA_PASSWORD` pour le contrôleur

Si le cluster fonctionne en mode non Active Directory, mettez à jour le mot de passe de la passerelle Apache Knox en procédant comme suit :

1. Obtenez les informations d’identification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] du contrôleur en exécutant les commandes suivantes :

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

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>Modifiez `AZDATA_PASSWORD` dans l’instance maître SQL Server

1. Connectez-vous au point de terminaison SQL maître avec n’importe quel utilisateur administrateur.

1. Pour modifier le mot de passe des informations d’identification de connexion que vous avez définies pendant le déploiement dans le paramètre `AZDATA_USERNAME`, exécutez la commande TSQL suivante :

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>Mise à jour manuelle du mot de passe pour Grafana et Kibana

Après avoir effectué les étapes de mise à jour de AZDATA_PASSWORD, vous verrez que [Grafana](app-monitor.md) et [Kibana](cluster-logging-kibana.md) acceptent toujours l’ancien mot de passe. Cela est dû au fait que Grafana et Kibana n’ont pas de visibilité sur le nouveau secret Kubernetes. Vous devez mettre à jour manuellement le mot de passe pour Grafana et Kibana séparément.

## <a name="update-grafana-password"></a>Mettre à jour le mot de passe Grafana

Suivez ces options pour mettre à jour manuellement le mot de passe pour [Grafana](app-monitor.md).

1. L’utilitaire htpasswd est obligatoire. Vous pouvez l’installer sur n’importe quel ordinateur client.
    
    #### <a name="for-ubuntu"></a>[Pour Ubuntu](#tab/ubuntu) : 
    ```bash
    sudo apt install apache2-utils
    ```
    
    #### <a name="for-rhel"></a>[Pour RHEL](#tab/rhel) : 
    ```bash
    sudo yum install httpd-tools
    ```
    
    ---

2. Générer le nouveau mot de passe. 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    Remplacez les valeurs par /<username/>, /<password/>, /<secret/> selon les cas, par exemple :
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. À présent, encodez le mot de passe :
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    Conservez la chaîne base64 de sortie pour plus tard.
    
4. Modifiez ensuite le mgmtproxy-secret :
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. Mettez à jour le contrôleur-login-htpasswd avec la nouvelle chaîne encodée du mot de passe en base64 générée ci-dessus :
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. Identifiez et supprimez le pod mgmtproxy. 
     
    Si nécessaire, identifiez le nom de votre prod mgmtproxy.
    
    #### <a name="for-windows"></a>[Pour Windows](#tab/windows) : 
    Sur un serveur Windows, vous pouvez utiliser les éléments suivants :
    
    ```bash 
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    
    #### <a name="for-linux"></a>[Pour Linux](#tab/linux) : 
    Sur Linux, vous pouvez utiliser ce qui suit :
    
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    
    ---
    
    Supprimez le pod mgmtproxy :
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. Attendez que le pod mgmtproxy soit en ligne et que le tableau de bord Grafana démarre.  
 
    Le temps d’attente n’est pas trop long et le pod doit être en ligne en quelques secondes. Pour vérifier l’état du pod, vous pouvez utiliser la même commande `get pods` que celle utilisée à l’étape précédente. 
    Si vous voyez que le pod mgmtproxy ne retourne pas rapidement à l’état Prêt, utilisez kubectl pour décrire le pod :
    
    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```
    
    Pour la résolution des problèmes et la collecte de journaux supplémentaire, utilisez la commande Azure Data CLI `[azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md)`.
    
8. Connectez-vous maintenant à Grafana à l’aide du nouveau mot de passe. 


## <a name="update-the-kibana-password"></a>Mettre à jour le mot de passe Kibana

Suivez ces options pour mettre à jour manuellement le mot de passe pour [Kibana](cluster-logging-kibana.md).

> [!NOTE]
> L’ancien navigateur Microsoft Edge étant incompatible avec Kibana, vous devez utiliser le navigateur Chromium Edge pour que le tableau de bord s’affiche correctement. Une page vide s’affiche lors du chargement des tableaux de bord à l’aide d’un navigateur non pris en charge, consultez [navigateurs pris en charge pour Kibana](https://www.elastic.co/support/matrix#matrix_browsers).

1. Ouvrez l’URL Kibana.
    
    Vous pouvez trouver l’URL du point de terminaison du service Kibana à partir de [Azure Data Studio](manage-with-controller-dashboard#controller-dashboard) ou utilisez la commande **azdata** suivante :
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    Par exemple : https://11.111.111.111:30777/kibana/app/kibana#/discover

2. Dans le volet gauche, cliquez sur l’option **Sécurité**.
    
    ![Capture d’écran du menu dans le volet gauche de Kibana, avec l’option de Sécurité choisie](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. Sur la page Sécurité, sous le titre Serveurs principaux d’authentification, cliquez sur **Base de données utilisateur interne**.

    ![Capture d’écran de la page Sécurité, avec la boîte Base de données utilisateur interne choisie.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. À présent, vous verrez la liste des utilisateurs sous le titre Base de données des utilisateurs internes. Utilisez cette page pour ajouter, modifier et supprimer des utilisateurs pour l’accès au point de terminaison Kibana. Pour l’utilisateur qui a besoin du mot de passe mis à jour, cliquez sur le bouton **Modifier** situé à droite pour l’utilisateur.

    ![Capture d’écran de la page Base de données utilisateur interne. Dans la liste des utilisateurs, pour l’utilisateur KubeAdmin, le bouton Modifier est choisi.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. Entrez le nouveau mot de passe à deux reprises, puis cliquez sur **Envoyer** :

    ![Capture d’écran du formulaire de modification de l’utilisateur interne. Un nouveau mot de passe a été entré dans les champs Mot de passe et Répéter le mot de passe.](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. Fermez le navigateur et reconnectez-vous à l’URL Kibana à l’aide du mot de passe mis à jour.

> [!Note]
> Après vous être connecté avec un nouveau mot de passe, si vous voyez des pages vides dans Kibana, déconnectez-vous manuellement à l’aide de l’option de déconnexion dans le coin supérieur droit et connectez-vous de nouveau.

## <a name="see-also"></a>Voir aussi

* [azdata bdc (Azure Data CLI)](../../sql/azdata/reference/reference-azdata-bdc.md) 
* [Surveiller les applications avec azdata et le tableau de bord Grafana](app-monitor.md)  
* [Extraire des journaux de cluster avec le tableau de bord Kibana](cluster-logging-kibana.md)  