---
title: Gérer les certificats dans SQL Server Integration Services Scale Out | Microsoft Docs
description: Cet article décrit la gestion des certificats pour sécuriser les communications entre le maître de montée en puissance parallèle SSIS et les travailleurs de montée en puissance parallèle.
ms.date: 12/19/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.custom: performance
ms.topic: conceptual
author: haoqian
ms.author: haoqian
ms.openlocfilehash: 0577e6f0e0d1bd78b71b2945977c313ff6a36afc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100347429"
---
# <a name="manage-certificates-for-sql-server-integration-services-scale-out"></a>Gérer les certificats dans SQL Server Integration Services Scale Out

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Pour sécuriser la communication entre Scale Out Master et les Scale Out Workers, SSIS Scale Out utilise deux certificats : un pour le Master et un pour les Workers. 

## <a name="scale-out-master-certificate"></a>Certificat Scale Out Master

Dans la plupart des cas, le certificat Scale Out Master est configuré pendant l’installation de Scale Out Master.

Dans la page **Configuration d’Integration Services Scale Out - Nœud Master** de l’Assistant Installation de SQL Server, vous pouvez créer un certificat TLS/SSL auto-signé ou utiliser un certificat TLS/SSL existant.

![Configuration de Master](media/master-config.PNG)

**Nouveau certificat**. Si vous n’avez pas d’exigences particulières pour les certificats, vous pouvez choisir de créer un certificat TLS/SSL auto-signé. Vous pouvez ensuite spécifier les noms communs dans le certificat. Vérifiez que le nom d’hôte du point de terminaison maître utilisé plus tard par les Scale Out Workers est inclus dans les noms communs. Par défaut, le nom de l’ordinateur et l’adresse IP du nœud Master sont inclus. 

**Certificat existant**. Si vous choisissez d’utiliser un certificat existant, cliquez sur **Parcourir** pour sélectionner un certificat TLS/SSL à partir du magasin de certificats **racine** de l’ordinateur local.

### <a name="change-the-scale-out-master-certificate"></a>Changer le certificat Scale Out Master

Vous pouvez changer votre certificat Scale Out Master s’il expire ou pour d’autres raisons. Pour changer le certificat Scale Out Master, effectuez les opérations suivantes :

#### <a name="1-create-a-tlsssl-certificate"></a>1. Obtenez un certificat TLS/SSL.
Créez et installez un certificat TLS/SSL sur le nœud Master à l’aide de la commande suivante :

```dos
MakeCert.exe -n CN={master endpoint host} SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```
Par exemple :

```dos
MakeCert.exe -n CN=MasterMachine SSISScaleOutMaster.cer -r -ss Root -sr LocalMachine -a sha1
```

#### <a name="2-bind-the-certificate-to-the-master-port"></a>2. Lier le certificat au port Master
Vérifiez la liaison d’origine à l’aide de la commande suivante :

```dos
netsh http show sslcert ipport=0.0.0.0:{Master port}
```

Par exemple :

```dos
netsh http show sslcert ipport=0.0.0.0:8391
```

Supprimez la liaison d’origine et configurez la nouvelle liaison à l’aide des commandes suivantes :

```dos
netsh http delete sslcert ipport=0.0.0.0:{Master port}
netsh http add sslcert ipport=0.0.0.0:{Master port} certhash={TLS/SSL Certificate Thumbprint} certstorename=Root appid={original appid}
```

Par exemple :

```dos
netsh http delete sslcert ipport=0.0.0.0:8391
netsh http add sslcert ipport=0.0.0.0:8391 certhash=01d207b300ca662f479beb884efe6ce328f77d53 certstorename=Root appid={a1f96506-93e0-4c91-9171-05a2f6739e13}
```

#### <a name="3-update-the-scale-out-master-service-configuration-file"></a>3. Mettre à jour le fichier de configuration du service Scale Out Master
Mettez à jour le fichier de configuration du service Scale Out Master, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\MasterSettings.config`, sur le nœud Master. Mettez à jour **SSLCertThumbprint** vers l’empreinte du nouveau certificat TLS/SSL.

#### <a name="4-restart-the-scale-out-master-service"></a>4. Redémarrer le service Scale Out Master

#### <a name="5-reconnect-scale-out-workers-to-scale-out-master"></a>5. Reconnecter les Scale Out Workers au Scale Out Master
Pour chaque Scale Out Worker, supprimez le Worker et rajoutez-le avec [Scale Out Manager](integration-services-ssis-scale-out-manager.md) ou effectuez les opérations suivantes :

a.  Installez le certificat TLS/SSL client dans le magasin racine de l’ordinateur local sur le nœud Worker.

b.  Mettez à jour le fichier de configuration du service Scale Out Worker.

Mettez à jour le fichier de configuration du service Scale Out Worker, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`, sur le nœud Worker. Mettez à jour **MasterHttpsCertThumbprint** vers l’empreinte du nouveau certificat TLS/SSL.

c.  Redémarrez le service Scale Out Worker.

## <a name="scale-out-worker-certificate"></a>Certificat Scale Out Worker

Le certificat Scale Out Worker est généré automatiquement durant l’installation de Scale Out Worker. 

### <a name="change-the-scale-out-worker-certificate"></a>Changer le certificat Scale Out Worker

Si vous souhaitez changer le certificat Scale Out Worker, effectuez les opérations suivantes :

#### <a name="1-create-a-certificate"></a>1. Créer un certificat
Créez et installez un certificat à l’aide de la commande suivante :

```dos
MakeCert.exe -n CN={worker machine name};CN={worker machine ip} SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

Par exemple :

```dos
MakeCert.exe -n CN=WorkerMachine;CN=10.0.2.8 SSISScaleOutWorker.cer -r -ss My -sr LocalMachine
```

#### <a name="2-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-worker-node"></a>2. Installer le certificat client dans le magasin racine de l’ordinateur local sur le nœud Worker

#### <a name="3-grant-service-access-to-the-certificate"></a>3. Donner au service l’accès au certificat
Supprimez l’ancien certificat et donnez au service Scale Out Worker l’accès au nouveau certificat à l’aide de la commande suivante :

```dos
certmgr.exe /del /c /s /r localmachine My /n {CN of the old certificate}
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s {CN of the new certificate} -a {the account running Scale Out Worker service}
```

Par exemple :

```dos
certmgr.exe /del /c /s /r localmachine My /n WorkerMachine
winhttpcertcfg.exe -g -c LOCAL_MACHINE\My -s WorkerMachine -a SSISScaleOutWorker140
```

#### <a name="4-update-the-scale-out-worker-service-configuration-file"></a>4. Mettre à jour le fichier de configuration du service Scale Out Worker
Mettez à jour le fichier de configuration du service Scale Out Worker, `\<drive\>:\Program Files\Microsoft SQL Server\140\DTS\Binn\WorkerSettings.config`, sur le nœud Worker. Mettez à jour **WorkerHttpsCertThumbprint** vers l’empreinte du nouveau certificat.

#### <a name="5-install-the-client-certificate-to-the-root-store-of-the-local-computer-on-the-master-node"></a>5. Installer le certificat client dans le magasin racine de l’ordinateur local sur le nœud master

#### <a name="6-restart-the-scale-out-worker-service"></a>6. Redémarrer le service Scale Out Worker

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations, consultez les articles suivants :
-   [SSIS (SQL Server Integration Services) Scale Out Master](integration-services-ssis-scale-out-master.md)
-   [SSIS (SQL Server Integration Services) Scale Out Worker](integration-services-ssis-scale-out-worker.md)
