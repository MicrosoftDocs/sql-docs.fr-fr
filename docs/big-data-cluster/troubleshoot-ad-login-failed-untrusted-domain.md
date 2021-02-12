---
title: Échec de la connexion en mode AD – Domaine non approuvé
titleSuffix: SQL Server Big Data Cluster
description: Corriger le comportement – Les clients ne parviennent pas à s’authentifier quand les entrées DNS des points de terminaison sont configurés en tant qu’enregistrements CNAME pointant vers un nom d’alias.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 05/01/2020
ms.topic: how-to
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 41145555b16dc437a4cb8f47ec490077f8aa8db3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100045549"
---
# <a name="symptom-ad-mode-login-fails---untrusted-domain-big-data-clusters"></a>Symptôme : Échec de la connexion en mode AD – Domaine non approuvé (Clusters Big Data)

Sur un cluster Big Data SQL Server en mode Active Directory, une tentative de connexion peut échouer et retourner l’erreur suivante :

`Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.`

Cela peut se produire quand des entrées DNS sont configurées en tant qu’enregistrements CNAME pointant vers un nom d’alias de proxy inverse qui répartit le trafic entre des nœuds Kubernetes.

## <a name="root-cause"></a>Cause racine

Quand les points de terminaison sont configurés avec des entrées DNS avec CNAME pointant vers un nom d’alias de proxy inverse qui répartit le trafic entre des nœuds Kubernetes :

- Le processus d’authentification Kerberos recherche un nom de principal du service (SPN) qui correspond à l’entrée de l’enregistrement CNAME ; il ne s’agit pas du véritable SPN inscrit par le cluster Big Data dans Active Directory
- Échec de l’authentification 

## <a name="confirm-root-cause"></a>Vérifier la cause racine

Après l’échec de l’authentification, vérifiez le cache des tickets Kerberos. 

Pour vérifier le cache des tickets, utilisez la commande `klist`. 

Recherchez un ticket avec un nom de principal du service (SPN) correspondant au point de terminaison auquel vous avez essayé de vous connecter.

Le ticket attendu n’est pas présent.

Dans cet exemple de point de terminaison principal, l’entrée DNS `bdc-sql` définie en tant qu’enregistrement CNAME pointe vers le proxy inverse nommé `ServerReverseProxy`

```PowerShell
Resolve-DnsName bdc-sql
```

La section suivante montre les résultats de la commande précédente.

```
Name                           Type   TTL   Section    NameHost
----                           ----   ---   -------    --------
bdc-sql.mydomain.com           CNAME  3600  Answer     ReverseProxyServer.mydomain.com

Name       : ReverseProxyServer.mydomain.com
QueryType  : A
TTL        : 3600
Section    : Answer
IP4Address : 193.168.5.10
```

>[!NOTE]
>La section suivante fait référence à [`tshark`](https://www.wireshark.org/docs/man-pages/tshark.html). `tshark` est un utilitaire de ligne de commande installé avec l’utilitaire de traçage réseau [Wireshark](https://www.wireshark.org/docs/).

Pour voir le SPN demandé à partir d’Active Directory, utilisez `tshark`. La commande suivante limite la capture du traçage réseau à la communication avec le protocole Kerberos et montre uniquement les messages `krb-error (30)`. Ces messages doivent contenir des messages de demande SPN en échec.

```bash
tshark -Y "kerberos && kerberos.msg_type == 30" -T fields -e kerberos.error_code -e kerberos.SNameString
```

À partir d’un interpréteur de commandes différent, essayez de vous connecter au pod principal :

```bash
klist purge

sqlcmd -S bdc-sql.mydomain.com,31433 -E
```

Examinez l’exemple de sortie suivant.

```bash
klist purge

Current LogonId is 0:0xf6b58
        Deleting all tickets:
        Ticket(s) purged!

sqlcmd -S bdc-sql.mydomain.com,31433 -E
sqlcmd: Error: Microsoft ODBC Driver 17 for SQL Server : Login failed. The login is from an untrusted domain and cannot be used with Integrated authentication.
```

Vérifiez la sortie de `tshark`. 

```bash
Capturing on 'Ethernet 3'
25      krbtgt,RLAZURE.COM
7       MSSQLSvc,ReverseProxyServer.mydomain.com:31433
2 packets captured
```

Notez que le client demande `SPN MSSQLSvc,ReverseProxyServer.mydomain.com:31433` qui n’existe pas. La tentative de connexion échoue finalement avec l’erreur 7. L’erreur 7 signifie `KRB5KDC_ERR_S_PRINCIPAL_UNKNOWN Server not found in Kerberos database`.

Dans la configuration appropriée, le client demande le SPN inscrit par le cluster Big Data. Dans l’exemple, le SPN approprié aurait été `MSSQLSvc,bdc-sql.mydomain.com:31433`.

>[!NOTE]
>L’erreur 25 signifie `KDC_ERR_PREAUTH_REQUIRED` – Pré-authentification supplémentaire nécessaire. Elle peut être ignorée sans risque. `KDC_ERR_PREAUTH_REQUIRED` est retourné lors de la demande AD Kerberos initiale. Par défaut, le client Kerberos Windows n’inclut pas les informations de pré-authentification dans cette première demande.

Pour voir la liste des SPN inscrits par le cluster Big Data pour le point de terminaison principal, exécutez `setspn -L mssql-master`. 

Examinez l’exemple de sortie suivant :

```bash
Registered ServicePrincipalNames for CN=mssql-master,OU=bdc,DC=mydomain,DC=com:
        MSSQLSvc/bdc-sqlread.mydomain.com:31436
        MSSQLSvc/-sqlread:31436
        MSSQLSvc/bdc-sqlread.mydomain.com
        MSSQLSvc/bdc-sqlread
        MSSQLSvc/bdc-sql.mydomain.com:31433
        MSSQLSvc/bdc-sql:31433
        MSSQLSvc/bdc-sql.mydomain.com
        MSSQLSvc/bdc-sql
        MSSQLSvc/master-p-svc.mydomain.com:1533
        MSSQLSvc/master-p-svc:1533
        MSSQLSvc/master-p-svc.mydomain.com:1433
        MSSQLSvc/master-p-svc:1433
        MSSQLSvc/master-p-svc.mydomain.com
        MSSQLSvc/master-p-svc
        MSSQLSvc/master-svc.mydomain.com:1533
        MSSQLSvc/master-svc:1533
        MSSQLSvc/master-svc.mydomain.com:1433
        MSSQLSvc/master-svc:1433
        MSSQLSvc/master-svc.mydomain.com
        MSSQLSvc/master-svc
```

Dans les résultats ci-dessus, l’adresse du proxy inverse ne doit pas être inscrite.

## <a name="resolve"></a>Résoudre

Cette section propose deux façons de résoudre le problème. Après avoir apporté les modifications appropriées, exécutez `ipconfig -flushdns` et `klist purge` dans votre client. Réessayez ensuite de vous connecter.

### <a name="option-1"></a>Option 1 :

Supprimez l’enregistrement CNAME pour chaque point de terminaison de cluster Big Data dans DNS et remplacez ces enregistrements par plusieurs enregistrements `A` qui pointent vers chaque nœud Kubernetes ou chaque maître Kubernetes s’il en existe plusieurs.

>[!TIP]
>Le script décrit ci-dessous utilise PowerShell. Pour plus d’informations, consultez [Installation de PowerShell sur Linux](/powershell/scripting/install/installing-powershell-core-on-linux).

Vous pouvez utiliser le script PowerShell suivant pour mettre à jour les enregistrements de points de terminaison DNS. Exécutez le script à partir d’un ordinateur connecté au même domaine :

```powershell
#Specify the DNS server, example contoso.local
$Domain_DNS_name=mydomain.com'

#DNS records for bdc endpoints
$Controller_DNS_name = 'bdc-control'
$Managment_proxy_DNS_name= 'bdc-proxy'
$Master_Primary_DNS_name = 'bdc-sql'
$Master_Secondary_DNS_name = 'bdc-sqlread'
$Gateway_DNS_name = 'bdc-gateway'
$AppProxy_DNS_name = 'bdc-appproxy'

#Performing Endpoint DNS records Checks..

#Build array of endpoints 
$BdcEndpointsDns = New-Object System.Collections.ArrayList

[void]$BdcEndpointsDns.Add($Controller_DNS_name)
[void]$BdcEndpointsDns.Add($Managment_proxy_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Primary_DNS_name)
[void]$BdcEndpointsDns.Add($Master_Secondary_DNS_name)
[void]$BdcEndpointsDns.Add($Gateway_DNS_name)
[void]$BdcEndpointsDns.Add($AppProxy_DNS_name)

#Build arrary for results 
$BdcEndpointsDns_Result = New-Object System.Collections.ArrayList

foreach ($DnsName in $BdcEndpointsDns) {
    try {
        $endpoint_DNS_record = Resolve-DnsName $DnsName -Type A -Server $Domain_DNS_IP_address -ErrorAction Stop 
        foreach ($ip in $endpoint_DNS_record.IPAddress) {
            [void]$BdcEndpointsDns_Result.Add("OK - $DnsName is an A record with an IP $ip")
        }
    }
    catch {
        [void]$BdcEndpointsDns_Result.Add("MisConfiguration - $DnsName is not an A record or does not exists")
    }  
}

#show the results 
$BdcEndpointsDns_Result
```

### <a name="option-2"></a>Option 2 :

Il est possible aussi de contourner le problème en faisant pointer l’enregistrement CNAME vers l’adresse IP du proxy inverse plutôt que vers son nom.

## <a name="confirm-resolution"></a>Vérifier la résolution

Après avoir corrigé le problème avec l’une des options précédentes, vérifiez que tout est solutionné en vous connectant au cluster Big Data avec Active Directory.

## <a name="next-steps"></a>Étapes suivantes

[Vérifier l’entrée de DNS inversé (enregistrement PTR) pour le contrôleur de domaine](active-directory-deploy.md#verify-reverse-dns-entry-for-domain-controller).
