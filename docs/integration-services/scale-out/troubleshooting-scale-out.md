---
title: Résoudre les problèmes de SSIS Scale Out | Microsoft Docs
description: Résolvez les problèmes courants liés à SSIS Scale Out, notamment les erreurs de configuration et l’absence d’autorisations d’accès.
ms.custom: performance
ms.date: 01/09/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 7d9c02b6912019304b37d3d536312c6a5707f769
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100337421"
---
# <a name="troubleshoot-scale-out"></a>Résoudre les problèmes de Scale-out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



SSIS Scale Out implique la communication entre la base de données de catalogues SSIS `SSISDB`, le service Scale Out Master et le service Scale Out Worker. Parfois, cette communication est interrompue en raison par exemple d’erreurs de configuration ou d’un défaut d’autorisations d’accès. Cet article vous aide à résoudre les problèmes liés à la configuration de Scale-out.

Pour étudier les problèmes que vous rencontrez, suivez les étapes ci-après une par une jusqu’à ce que votre problème soit résolu.

## <a name="scale-out-master-fails"></a>Scale Out Master échoue

### <a name="symptoms"></a>Symptômes 
-   Scale Out Master ne peut pas se connecter à SSISDB. 

-   Les propriétés Master n’apparaissent pas dans Scale Out Manager.

-   Les propriétés Master ne sont pas renseignées dans la vue `[catalog].[master_properties]`.

### <a name="solution"></a>Solution
1.  Vérifiez si Scale-out est activé.

    Dans l’Explorateur d’objets de SSMS, cliquez avec le bouton droit sur **SSISDB**, puis vérifiez que **la fonctionnalité Scale Out est activée**.

    ![Vérifier si Scale-out est activé](media/isenabled.PNG)

    Si la valeur de propriété est False, activez Scale-out en appelant la procédure stockée `[catalog].[enable_scaleout]`.

2.  Vérifiez si le nom SQL Server spécifié dans le fichier de configuration de Scale Out Master est correct et redémarrez le service Scale Out Master.

## <a name="scale-out-worker-fails"></a>Scale Out Worker échoue

### <a name="symptoms"></a>Symptômes 
-   Scale Out Worker ne peut pas se connecter à Scale Out Master.

-   Scale Out Worker n’apparaît pas une fois ajouté à Scale Out Manager.

-   Scale Out Worker n’apparaît pas dans la vue `[catalog].[worker_agents]`.

-   Le service Scale Out Worker est en cours d’exécution, mais Scale Out Worker est hors connexion.

### <a name="solution"></a>Solution
Vérifiez les messages d’erreur dans le journal du service Scale Out Worker sous `\<drive\>:\Users\\*[account running worker service]*\AppData\Local\SSIS\Cluster\Agent`.

## <a name="no-endpoint-listening"></a>Aucun point de terminaison à l’écoute

### <a name="symptoms"></a>Symptômes

*"System.ServiceModel.EndpointNotFoundException: Aucun point de terminaison à l’écoute sur https://* [NomMachine]:[Port] */ClusterManagement/ pouvant accepter le message. »*

### <a name="solution"></a>Solution

1.  Vérifiez si le numéro de port spécifié dans le fichier de configuration du service Scale Out Master est correct et redémarrez le service Scale Out Master. 

2.  Vérifiez si le point de terminaison maître spécifié dans la configuration du service Scale Out Worker est correct et redémarrez le service Scale Out Worker.

3.  Vérifiez si le port de pare-feu est ouvert sur le nœud Scale Out Master.

4.  Résolvez tout problème de connexion entre les nœuds Scale Out Master et Scale Out Worker.

## <a name="could-not-establish-trust-relationship"></a>Impossible d’établir une relation d’approbation

### <a name="symptoms"></a>Symptômes
*""System.ServiceModel.Security.SecurityNegotiationException: Impossible d’établir une relation d’approbation pour le canal sécurisé SSL/TLS avec l’autorité '[NomMachine]:[Port]'. »*

*"System.Net.WebException: Le serveur a clos la connexion sous-jacente : Impossible d’établir une relation d’approbation pour le canal sécurisé SSL/TLS. »*

*"System.Security.Authentication.AuthenticationException: Le certificat distant n’est pas valide selon la procédure de validation. »*

### <a name="solution"></a>Solution
1.  Installez le certificat Scale Out Master dans le magasin de certificats racine de l’ordinateur local sur le nœud Scale Out Worker si ce n’est déjà fait, puis redémarrez le service Scale Out Worker.

2.  Vérifiez si le nom d’hôte dans le point de terminaison maître est inclus dans les noms communs du certificat Scale Out Master. Si ce n’est pas le cas, réinitialisez le point de terminaison maître dans le fichier de configuration de Scale Out Worker et redémarrez le service Scale Out Worker. 

    > [!NOTE]
    > Si vous ne pouvez pas changer le nom d’hôte du point de terminaison maître en raison des paramètres DNS, vous devez changer le certificat Scale Out Master. Consultez [Gérer les certificats dans SQL Server Integration Services Scale Out](deal-with-certificates-in-ssis-scale-out.md).

3.  Vérifiez si l’empreinte numérique du master spécifiée dans la configuration de Scale Out Worker correspond à l’empreinte du certificat Scale Out Master. 

## <a name="could-not-establish-secure-channel"></a>Impossible d’établir un canal sécurisé

### <a name="symptoms"></a>Symptômes

*"System.ServiceModel.Security.SecurityNegotiationException: Impossible d’établir un canal sécurisé pour SSL/TLS avec l’autorité '[NomMachine]:[Port]'. »*

*"System.Net.WebException: La demande a été abandonnée : Impossible de créer un canal sécurisé SSL/TLS. »*

### <a name="solution"></a>Solution
Vérifiez si le compte exécutant le service Scale Out Worker a accès au certificat Scale Out Worker en exécutant la commande suivante :

```dos
winhttpcertcfg.exe -l -c LOCAL_MACHINE\MY -s {CN of the worker certificate}
```

Si ce n’est pas le cas, accordez l’accès en exécutant la commande suivante et redémarrez le service Scale Out Worker.

```dos
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the worker certificate} -a {the account running Scale Out Worker service}
```

## <a name="http-request-forbidden"></a>Requête HTTP interdite

### <a name="symptoms"></a>Symptômes

*"System.ServiceModel.Security.MessageSecurityException: La requête HTTP a été interdite avec le schéma d’authentification client 'Anonyme'. »*

*"System.Net.WebException: Le serveur distant a retourné une erreur : (403) Interdit. »*

### <a name="solution"></a>Solution
1.  Installez le certificat Scale Out Worker dans le magasin de certificats racine de l’ordinateur local sur le nœud Scale Out Master si ce n’est déjà fait, puis redémarrez le service Scale Out Worker.

2.  Nettoyez les certificats inutiles dans le magasin de certificats racine de l’ordinateur local sur le nœud Scale Out Master.

3.  Configurez Schannel pour ne plus envoyer la liste des autorités de certification racine de confiance pendant le processus de négociation TLS/SSL, en ajoutant l’entrée de Registre suivante au nœud Scale Out Master.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nom de la valeur : **SendTrustedIssuerList** 

    Type de valeur : **REG_DWORD** 

    Données de la valeur : **0 (False)**

4.  S’il n’est pas possible de nettoyer tous les certificats non signés comme décrit à l’étape 2, attribuez à la clé de Registre suivante la valeur 2.

    `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL`

    Nom de la valeur : **ClientAuthTrustMode** 

    Type de valeur : **REG_DWORD** 

    Données de la valeur : **2**

    > [!NOTE]
    > S’il existe des certificats non signés automatiquement dans le magasin de certificats racine, l’authentification par certificat client échoue. Pour plus d’informations, voir [Internet Information Services (IIS) 8 peut rejeter les demandes de certificat client avec une erreur HTTP 403.7 ou 403.16](https://support.microsoft.com/help/2802568/internet-information-services-iis-8-may-reject-client-certificate-requ).

## <a name="http-request-error"></a>Erreur de requête HTTP

### <a name="symptoms"></a>Symptômes

*"System.ServiceModel.CommunicationException: Une erreur s’est produite lors de la requête HTTP envoyée à https://[Machine]:[Port]/ClusterManagement/. Cela peut être dû au fait que le certificat de serveur n’est pas configuré correctement avec HTTP.SYS pour HTTPS. Une autre raison possible est une non-correspondance de la liaison de sécurité entre le client et le serveur. »*

### <a name="solution"></a>Solution
1.  Vérifiez si le certificat Scale Out Master est correctement lié au port dans le point de terminaison maître sur le nœud Master en exécutant la commande suivante :

    ```dos
    netsh http show sslcert ipport=0.0.0.0:{Master port}
    ```

    Vérifiez si le hachage de certificat affiché correspond à l’empreinte du certificat Scale Out Master. Si la liaison n’est pas correcte, réinitialisez-la en exécutant les commandes suivantes et redémarrez le service Scale Out Worker.

    ```dos
    netsh http delete sslcert ipport=0.0.0.0:{Master port}
    netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={Master certificate thumbprint} certstorename=Root  appid={random guid}
    ```

## <a name="cannot-open-certificate-store"></a>Impossible d’ouvrir le magasin de certificats

### <a name="symptoms"></a>Symptômes
La validation échoue au moment de la connexion de Scale Out Worker à Scale Out Master dans Scale Out Manager avec le message d’erreur *« Impossible d’ouvrir le magasin de certificats sur l’ordinateur »* .

### <a name="solution"></a>Solution

1.  Exécutez Scale Out Manager en tant qu’administrateur. Si vous l’ouvrez avec SSMS, vous devez exécuter SSMS en tant qu’administrateur.

2.  Démarrez le service Registre à distance sur l’ordinateur s’il n’est pas en cours d’exécution.

## <a name="execution-doesnt-start"></a>L’exécution ne démarre pas

### <a name="symptoms"></a>Symptômes
L’exécution dans Scale-out ne démarre pas.

### <a name="solution"></a>Solution

Vérifiez l’état des ordinateurs que vous avez sélectionnés pour exécuter le package dans la vue `[catalog].[worker_agents]`. Au moins un Worker doit être en ligne et activé.

## <a name="no-log"></a>Aucun journal

### <a name="symptoms"></a>Symptômes 
Les packages s’exécutent correctement, mais aucun message n’est journalisé.

### <a name="solution"></a>Solution

Vérifiez si l’authentification SQL Server est autorisée par l’instance de SQL Server qui héberge SSISDB.

> [!NOTE]  
> Si vous avez changé le compte pour la journalisation Scale-out, consultez [Changer le compte pour la journalisation Scale-out](change-logdb-account.md) et vérifiez la chaîne de connexion utilisée pour la journalisation.

## <a name="error-messages-arent-helpful"></a>Les messages d’erreur ne sont pas utiles

### <a name="symptoms"></a>Symptômes
Les messages d’erreur dans le rapport d’exécution des packages ne sont pas suffisants pour résoudre les problèmes.

### <a name="solution"></a>Solution
Des journaux d’exécution supplémentaires sont disponibles sous `TasksRootFolder` configuré dans `WorkerSettings.config`. Par défaut, ce dossier est `\<drive\>:\Users\\[account]\AppData\Local\SSIS\ScaleOut\Tasks`. *[compte]* fait référence au compte exécutant le service Scale Out Worker, avec la valeur par défaut `SSISScaleOutWorker140`.

Pour localiser le journal de l’exécution des packages avec *[ID_exécution]* , exécutez la commande Transact-SQL suivante pour obtenir *[ID_tâche]* . Ensuite, recherchez le nom de sous-dossier contenant *[ID_tâche]* sous `TasksRootFolder`.

```sql
SELECT [TaskId]
FROM [SSISDB].[internal].[tasks] tasks, [SSISDB].[internal].[executions] executions 
WHERE executions.execution_id = *Your Execution Id* AND tasks.JobId = executions.job_id
```

> [!WARNING]
> Cette requête n’est fournie qu’à des fins de résolution de problèmes. Les vues internes référencées dans la requête feront l’objet de modifications dans le futur. 

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants sur l’installation et la configuration de SSIS Scale Out :
-   [Bien démarrer avec SSIS (SQL Server Integration Services) Scale Out sur un seul ordinateur](get-started-with-ssis-scale-out-onebox.md)
-   [Procédure pas à pas : Configurer Integration Services (SSIS) Scale Out](walkthrough-set-up-integration-services-scale-out.md)
