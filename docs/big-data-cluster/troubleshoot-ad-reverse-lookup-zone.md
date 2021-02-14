---
title: 'Déploiement en mode AD arrêté : entrée de zone de recherche inversée manquante pour le contrôleur de domaine'
titleSuffix: SQL Server Big Data Cluster
description: Le déploiement du BDC en mode AD est bloqué en raison de l’absence d’une entrée de zone de recherche inversée pour le contrôleur de domaine dans le serveur DNS du contrôleur de domaine.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 04/21/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 02276cee62edd604df5e018bc22cf86cddaaec03
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100043949"
---
# <a name="ad-mode-deployment-stopped---missing-reverse-lookup-zone-entry-for-dc"></a>Déploiement en mode AD arrêté : entrée de zone de recherche inversée manquante pour le contrôleur de domaine

Le déploiement en mode Active Directory (AD) se fige. Vérifiez les symptômes pour voir si l’entrée de la zone de recherche inversée est manquante sur le serveur DNS du contrôleur de domaine. 

## <a name="symptom"></a>Symptôme

Vous avez démarré le déploiement du BDC avec le mode AD, mais le déploiement est bloqué et ne progresse pas.

L’exemple suivant montre le résultat du déploiement dans un interpréteur de commandes bash.

```
The privacy statement can be viewed at:
https://go.microsoft.com/fwlink/?LinkId=853010
 
The license terms for SQL Server Big Data Cluster can be viewed at:
Enterprise: https://go.microsoft.com/fwlink/?linkid=2104292
Standard: https://go.microsoft.com/fwlink/?linkid=2104294
Developer: https://go.microsoft.com/fwlink/?linkid=2104079
 
Cluster deployment documentation can be viewed at:
https://aka.ms/bdc-deploy
 
NOTE: Cluster creation can take a significant amount of time depending on
configuration, network speed, and the number of nodes in the cluster.
 
Starting cluster deployment.
Cluster controller endpoint is available at bdc-control.contoso.com:30080, 193.168.5.14:30080.
Waiting for control plane to be ready after 5 minutes.
Waiting for control plane to be ready after 10 minutes.
Waiting for control plane to be ready after 15 minutes.
Waiting for control plane to be ready after 20 minutes.
Waiting for control plane to be ready after 25 minutes.
```

Vérifiez les pods actuellement déployés.

```bash
kubectl get pods -n mssql-cluster
```

Les résultats ci-dessous indiquent que seuls les pods appartenant au contrôleur ont été déployés. Les pods pour le calcul, les données ou le stockage ne sont pas créés.

```
NAME              READY   STATUS    RESTARTS   AGE
control-rts5t     3/3     Running   0          18m
controldb-0       2/2     Running   0          18m
controlwd-csgst   1/1     Running   0          16m
dns-7kfnz         2/2     Running   0          16m
logsdb-0          1/1     Running   0          16m
logsui-2pc29      1/1     Running   0          16m
metricsdb-0       1/1     Running   0          16m
metricsdc-4rtm4   1/1     Running   0          16m
metricsdc-6lr2t   1/1     Running   0          16m
metricsdc-ftx9m   1/1     Running   0          16m
metricsdc-h59jb   1/1     Running   0          16m
metricsui-lvdpt   1/1     Running   0          16m
mgmtproxy-mkmxp   2/2     Running   0          16m
```

Inspectez les journaux du conteneur security-support. Recherchez les erreurs LDAP. 

## <a name="check-security-support-container"></a>Vérifier le conteneur security-support 

Examinez les journaux du conteneur security-support.

La commande suivante collecte les journaux de security-support dans un cluster de l’espace de noms `mssql-cluster`.

```bash
azdata bdc debug copy-logs -n mssql-cluster -c security-support
```

Extrayez les journaux et recherchez `\mssql-cluster\control-<identifier>\controller\control-rts5t-controller-stdout.log`.

> [!TIP]
> Il existe plusieurs façons de collecter les journaux. Au lieu de copier les journaux avec [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)], vous pouvez utiliser un notebook dans Azure Data Studio.
> Dans Azure Data Studio, connectez-vous au cluster Kubernetes et exécutez un notebook de dépannage approprié. Voici quelques exemples de notebooks.
>
> - TSG027 - Observer le déploiement du cluster
> - TSG061 - Obtenir la fin de tous les journaux de conteneurs pour les pods dans l’espace de noms BDC
> - TSG001 - Exécuter `azdata copy-logs`
>

## <a name="inspect-the-logs"></a>Inspection des données

Localisez le journal. L’exemple suivant pointe vers un journal de déploiement du contrôleur. 

`<folderOfDebugCopyLog>\debuglogs-mssql-cluster-YYYYMMDD-HHMMSS\<namespace>\control-<identifier>\controller\control-<identifier>-controller-stdout.log`"

```
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'cntrl-controller'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=cntrl-controller,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'ldap-user'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=ldap-user,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
YYYY-MM-DD HH:MM:SS.ms | ERROR | Failed to create AD user account 'nginx-mgmtproxy'. Error code: 53. Message: Failed to create user object: Failed to add object 'CN=nginx-mgmtproxy,OU=bdc, DC=CONTOSO, DC=com' to 'CONTOSO.COM': Server is unwilling to perform. 
```

## <a name="cause"></a>Cause

L’entrée de la zone de recherche inversée pour le contrôleur de domaine dans l’entrée DNS du contrôleur de domaine est manquante. 

## <a name="resolution"></a>Résolution

Exécutez le script PowerShell suivant pour vérifier que l’entrée de DNS inversé (enregistrement PTR) est configurée.

```powershell
#Domain Controller FQDN 'DCserver01.contoso.local'
$Domain_controller_FQDN = 'DCserver01.contoso.local'

#Performing Domain Controller DNS record, reverse PTR Checks...
$DcControllerDnsPtr_Result = New-Object System.Collections.ArrayList
try {
    $Domain_controller_DNS_Record = Resolve-DnsName $Domain_controller_FQDN -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop
    foreach ($ip in $Domain_controller_DNS_Record.IPAddress) {
        #resolving hostname by IP address to make sure we have reverse PTR record 
        if ((Resolve-DnsName $ip).NameHost -eq $Domain_controller_FQDN) {
            [void]$DcControllerDnsPtr_Result.add("OK - $Domain_controller_FQDN has an A record with an IP $ip, Reverse PTR record is in place") 
        }
        else {
            [void]$DcControllerDnsPtr_Result.add("Missing - $Domain_controller_FQDN has an A record with an IP $ip, But no reverse PTR record was found for the host")
        }
    }
}
catch {
    [void]$DcControllerDnsPtr_Result.add("Error - " + $_.exception.message)
}

#show the results 
$DcControllerDnsPtr_Result
```

## <a name="next-steps"></a>Étapes suivantes

[Vérifier l’entrée de DNS inversé (enregistrement PTR) pour le contrôleur de domaine](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
