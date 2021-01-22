---
title: Déployer le Service Guardian hôte
description: Déployez le Service Guardian hôte pour Always Encrypted avec enclaves sécurisées.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 88e79166a8b44139f58192feece211bc3b3d2db3
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534788"
---
# <a name="deploy-the-host-guardian-service-for-ssnoversion-md"></a>Déployer le Service Guardian hôte pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Cet article décrit comment déployer le Service Guardian hôte (SGH) en tant que service d’attestation pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Avant de commencer, lisez l’article [Planifier l’attestation avec le Service Guardian hôte](./always-encrypted-enclaves-host-guardian-service-plan.md) pour accéder à la liste complète des prérequis et à de l’aide en matière d’architecture.

> [!NOTE]
> L’administrateur SGH est responsable de l’exécution de toutes les étapes décrites dans cet article. Consultez [Rôles et responsabilités lors de la configuration de l’attestation avec SGH](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

## <a name="step-1-set-up-the-first-hgs-computer"></a>Étape 1 : Configurer le premier ordinateur SGH

Le Service Guardian hôte (SGH) s’exécute en tant que service en cluster sur un ou plusieurs ordinateurs.
Dans cette étape, vous allez configurer un nouveau cluster SGH sur le premier ordinateur.
Si vous disposez déjà d’un cluster SGH et que vous souhaitez y ajouter des ordinateurs à des fins de haute disponibilité, passez à l’[Étape 2 : Ajouter des ordinateurs SGH au cluster](#step-2-add-more-hgs-computers-to-the-cluster).

Avant de commencer, vérifiez que l’ordinateur que vous utilisez exécute Windows Server 2019 Standard ou Datacenter, que vous disposez de privilèges d’administrateur local et que l’ordinateur n’est pas déjà joint à un domaine Active Directory.

1. Connectez-vous au premier ordinateur SGH en tant qu’administrateur local et ouvrez une console Windows PowerShell avec élévation de privilèges. Exécutez la commande suivante pour installer le rôle Service Guardian hôte. L’ordinateur redémarre automatiquement pour appliquer les changements.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Une fois l’ordinateur SGH redémarré, exécutez les commandes suivantes dans une console Windows PowerShell avec élévation de privilèges pour installer la nouvelle forêt Active Directory :

    ```powershell
    # Select the name for your new Active Directory root domain.
    # Make sure the name does not conflict with, and is not subordinate to, any existing domains on your network.
    $HGSDomainName = 'bastion.local'

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

    Votre ordinateur SGH redémarre une nouvelle fois pour terminer la configuration de la forêt Active Directory. La prochaine fois que vous vous connecterez, votre compte d’administrateur sera un compte d’administrateur de domaine. Pour plus d’informations sur la gestion et la sécurisation de votre nouvelle forêt, nous vous recommandons de passer en revue la [documentation sur les opérations dans Active Directory Domain Services](/windows-server/identity/ad-ds/manage/component-updates/ad-ds-operations).

3. Vous allez à présent configurer le cluster SGH et installer le service d’attestation en exécutant la commande suivante dans une console Windows PowerShell avec élévation de privilèges :

    ```powershell
    # Note: the name you provide here will be shared by all HGS nodes and used to point your SQL Server computers to the HGS cluster.
    # For example, if you provide "attsvc" here, a DNS record for "attsvc.yourdomain.com" will be created for every HGS computer.
    Initialize-HgsAttestation -HgsServiceName 'hgs'
    ```

## <a name="step-2-add-more-hgs-computers-to-the-cluster"></a>Étape 2 : Ajouter des ordinateurs SGH au cluster

Une fois votre premier ordinateur/cluster SGH configuré, vous pouvez ajouter des serveurs SGH à des fins de haute disponibilité.
Si vous ne configurez qu’un seul serveur SGH (dans un environnement de développement/test, par exemple), vous pouvez passer à l’étape 3.

Comme pour le premier ordinateur SGH, vérifiez que l’ordinateur que vous joignez au cluster exécute Windows Server 2019 Standard ou Datacenter, que vous disposez de privilèges d’administrateur local et que l’ordinateur n’est pas déjà joint à un domaine Active Directory.

1. Connectez-vous à l’ordinateur en tant qu’administrateur local et ouvrez une console Windows PowerShell avec élévation de privilèges. Exécutez la commande suivante pour installer le rôle Service Guardian hôte. L’ordinateur redémarre automatiquement pour appliquer les changements.

    ```powershell
    Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
    ```

2. Examinez la configuration du client DNS sur votre ordinateur pour vérifier qu’il peut résoudre le domaine SGH. La commande suivante doit retourner une adresse IP pour votre serveur SGH. Si vous ne parvenez pas à résoudre le domaine SGH, vous devrez peut-être mettre à jour les informations du serveur DNS sur votre carte réseau de manière à utiliser le serveur DNS SGH pour la résolution de noms.

    ```powershell
    # Change 'bastion.local' to the domain name you specified in Step 1.2
    nslookup bastion.local

    # If it fails, use sconfig.exe, option 8, to set the first HGS computer as your preferred DNS server.
    ```

3. Une fois l’ordinateur redémarré, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges pour joindre l’ordinateur au domaine Active Directory créé par le premier serveur SGH. Pour exécuter cette commande, vous devez disposer d’informations d’identification d’administrateur de domaine pour le domaine AD. À l’issue de la commande, l’ordinateur redémarre et devient un contrôleur de domaine Active Directory pour le domaine SGH.

    ```powershell
    # Provide the fully qualified HGS domain name
    $HGSDomainName = 'bastion.local'

    # Provide a domain administrator's credential for the HGS domain
    $DomainAdminCred = Get-Credential

    # Specify a Directory Services Restore Mode password that can be used to recover your domain in safe mode.
    # This password is not, and will not change, your admin account password.
    # Save this password somewhere safe and include it in your disaster recovery plan.
    $DSRMPassword = Read-Host -AsSecureString -Prompt "Directory Services Restore Mode Password"

    Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $DomainAdminCred -SafeModeAdministratorPassword $DSRMPassword -Restart
    ```

4. Après le redémarrage de l’ordinateur, connectez-vous avec des informations d’identification d’administrateur de domaine. Ouvrez une console Windows PowerShell avec élévation de privilèges et exécutez les commandes suivantes pour configurer le service d’attestation. Étant donné que le service d’attestation prend en charge les clusters, il réplique sa configuration à partir d’autres membres du cluster. Les modifications apportées aux stratégies d’attestation sur n’importe quel nœud SGH s’appliquent à tous les autres nœuds.

    ```powershell
    # Provide the IP address of an existing, initialized HGS server
    # If you are using separate networks for cluster and application traffic, choose an IP address on the cluster network.
    # You can find the IP address of your HGS server by signing in and running "ipconfig /all"
    Initialize-HgsAttestation -HgsServerIPAddress '172.16.10.20'
    ```

5. Répétez l’étape 2 pour chaque ordinateur que vous souhaitez ajouter à votre cluster SGH.

## <a name="step-3-configure-a-dns-forwarder-to-your-hgs-cluster"></a>Étape 3 : Configurer un redirecteur DNS vers votre cluster SGH

SGH exécute son propre serveur DNS qui contient les enregistrements de noms nécessaires pour résoudre le service d’attestation.
Pour que vos ordinateurs SQL Server puissent résoudre ces enregistrements, vous devez configurer le serveur DNS de votre réseau pour transférer les demandes aux serveurs DNS SGH.

Le processus de configuration des redirecteurs DNS étant spécifique au fournisseur, nous vous recommandons de contacter votre administrateur réseau pour obtenir des instructions adaptées à votre réseau.

Si vous utilisez le rôle Windows Server Serveur DNS pour votre réseau d’entreprise, votre administrateur DNS peut créer un redirecteur conditionnel vers le domaine SGH afin de transférer uniquement les demandes pour le domaine SGH.
Par exemple, si vos serveurs SGH utilisent le nom de domaine « bastion.local » et ont pour adresses IP 172.16.10.20, 172.16.10.21 et 172.16.10.22, vous pouvez exécuter la commande suivante sur le serveur DNS d’entreprise pour configurer un redirecteur conditionnel :

```powershell
# Tip: make sure to provide every HGS server's IP address
# If you use separate NICs for cluster and application traffic, use the application traffic NIC IP addresses here
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers "172.16.10.20", "172.16.10.21", "172.16.10.22"
```

## <a name="step-4-configure-the-attestation-service"></a>Étape 4 : Configurer le service d’attestation

SGH prend en charge deux modes d’attestation : l’attestation TPM (vérification du chiffrement pour valider l’intégrité et l’identité de chaque ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]) et l’attestation de clé d’hôte (simple vérification de l’identité de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]).
Si vous n’avez pas encore sélectionné de mode d’attestation, consultez les informations contenues dans le [guide de planification](./always-encrypted-enclaves-host-guardian-service-plan.md#attestation-modes) pour découvrir plus en détail les garanties de sécurité et les cas d’usage de chaque mode.

Les étapes de cette section configurent les stratégies d’attestation de base pour un mode d’attestation donné.
Vous inscrivez des informations spécifiques à l’hôte au cours de l’étape 4.
Si par la suite vous souhaitez changer de mode d’attestation, répétez les étapes 3 et 4.

### <a name="switch-to-tpm-attestation"></a>Passer à l’attestation TPM

Pour configurer l’attestation TPM sur SGH, vous devez disposer d’un ordinateur connecté à Internet et d’au moins un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] équipé d’une puce TPM 2.0 rév. 1.16.

Pour configurer SGH afin d’utiliser le mode TPM, ouvrez une console PowerShell avec élévation de privilèges et exécutez la commande suivante :

```powershell
Set-HgsServer -TrustTpm
```

Tous les ordinateurs SGH de votre cluster utilisent désormais le mode TPM quand un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tente une attestation.

Avant de pouvoir inscrire les informations TPM de vos ordinateurs SQL Server auprès de SGH, vous devez installer les certificats racines de la paire de clés de type EK (Endorsement Key) de votre ou vos fournisseurs TPM.
Chaque TPM physique est configuré en usine avec une paire unique de clés de type EK. Un certificat qui identifie le fabricant accompagne cette paire de clés.
Ce certificat garantit l’authenticité de votre TPM.
SGH vérifie le certificat de la paire de clés de type EK quand vous inscrivez un nouveau TPM auprès de SGH en comparant la chaîne de certificats à la liste de certificats racines approuvés.

Microsoft publie une liste de certificats racines de fournisseurs TPM valides connus que vous pouvez importer dans le magasin de certificats racines TPM approuvés par SGH.
Si vos ordinateurs SQL Server sont virtualisés, vous devez contacter votre fournisseur de services cloud ou votre fournisseur de plateforme de virtualisation afin de déterminer comment obtenir la chaîne de certificats pour la paire de clés de type EK de votre TPM virtuel.

Pour télécharger le package de certificats racines TPM approuvés pour les TPM physiques à partir du site web de Microsoft, effectuez les étapes suivantes :

1. Sur un ordinateur connecté à Internet, téléchargez le package de certificats racines TPM le plus récent à partir de la page [https://go.microsoft.com/fwlink/?linkid=2097925](https://go.microsoft.com/fwlink/?linkid=2097925).

2. Vérifiez que la signature du fichier CAB est authentique.

    ```powershell
    # Note: replace the path below with the correct one to the file you downloaded in step 1
    Get-AuthenticodeSignature ".\TrustedTpm.cab"
    ```

    > [!WARNING]
    > N’allez pas plus loin si la signature n’est pas valide et contactez le support Microsoft pour obtenir de l’aide.

3. Développez le fichier CAB dans un nouveau répertoire.

    ```powershell
    mkdir .\TrustedTpmCertificates
    expand.exe -F:* ".\TrustedTpm.cab" ".\TrustedTpmCertificates"
    ```

4. Le nouveau répertoire contient un répertoire pour chaque fournisseur de TPM. Vous pouvez supprimer les répertoires des fournisseurs que vous n’utilisez pas.

5. Copiez l’intégralité du répertoire « TrustedTpmCertificates » sur votre serveur SGH.

6. Ouvrez une console PowerShell avec élévation de privilèges sur le serveur SGH et exécutez la commande suivante pour importer tous les certificats racines et intermédiaires du TPM :

    ```powershell
    # Note: replace the path below with the correct location of the TrustedTpmCertificates folder on your HGS computer
    cd "C:\scratch\TrustedTpmCertificates"
    .\setup.cmd
    ```

7. Répétez les étapes 5 et 6 pour chaque ordinateur SGH.

Si vous avez obtenu des certificats d’autorité de certification racines et intermédiaires auprès de votre fabricant d’ordinateurs OEM, fournisseur de services cloud ou fournisseur de plateforme de virtualisation, vous pouvez importer directement les certificats dans le magasin de certificats correspondant de l’ordinateur local : `TrustedHgs_RootCA` ou `TrustedHgs_IntermediateCA`. Par exemple, dans PowerShell :

```powershell
# Imports MyCustomTpmVendor_Root.cer to the local machine's "TrustedHgs_RootCA" store
Import-Certificate -FilePath ".\MyCustomTpmVendor_Root.cer" -CertStoreLocation "Cert:\LocalMachine\TrustedHgs_RootCA"
```

### <a name="switch-to-host-key-attestation"></a>Passer à l’attestation de clé d’hôte

Pour utiliser l’attestation de clé d’hôte, exécutez la commande suivante sur un serveur SGH dans une console PowerShell avec élévation de privilèges :

```powershell
Set-HgsServer -TrustHostKey
```

Tous les ordinateurs SGH de votre cluster utilisent désormais le mode clé d’hôte quand un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tente une attestation.

## <a name="step-5-configure-the-hgs-https-binding"></a>Étape 5 : Configurer la liaison HTTPS SGH

Dans une installation par défaut, SGH expose uniquement une liaison HTTP (port 80).
Vous pouvez configurer une liaison HTTPS (port 443) pour chiffrer toutes les communications entre les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et SGH.
Il est recommandé que toutes les instances de production de SGH utilisent une liaison HTTPS.

1. Obtenez un certificat TLS auprès de votre autorité de certification en utilisant le nom de service SGH complet de l’étape 1.3 comme nom d’objet. Si vous ne connaissez pas le nom de votre service, exécutez `Get-HgsServer` sur n’importe quel ordinateur SGH pour le trouver. Vous pouvez ajouter d’autres noms DNS à la liste Autre nom de l’objet si vos ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] utilisent un nom DNS différent pour joindre votre cluster SGH (par exemple, si SGH se trouve derrière un équilibreur de charge réseau avec une adresse différente).

2. Sur l’ordinateur SGH, utilisez [Set-HgsServer](/powershell/module/hgsserver/set-hgsserver) pour activer la liaison HTTPS et spécifier le certificat TLS obtenu à l’étape précédente. Si votre certificat est déjà installé sur l’ordinateur dans le magasin de certificats local, utilisez la commande suivante pour l’inscrire auprès de SGH :

    ```powershell
    # Note: you'll need to know the thumbprint for your certificate to configure HGS this way
    Set-HgsServer -Http -Https -HttpsCertificateThumbprint "54A043386555EB5118DB367CFE38776F82F4A181"
    ```

    Si vous avez exporté votre certificat (avec une clé privée) dans un fichier PFX protégé par mot de passe, vous pouvez l’inscrire auprès de SGH en exécutant la commande suivante :

    ```powershell
    $PFXPassword = Read-Host -AsSecureString -Prompt "PFX Password"
    Set-HgsServer -Http -Https -HttpsCertificatePath "C:\path\to\hgs_tls.pfx" -HttpsCertificatePassword $PFXPassword
    ```

3. Répétez les étapes 1 et 2 pour chaque ordinateur SGH du cluster. Les certificats TLS ne sont pas automatiquement répliqués entre les nœuds SGH. Par ailleurs, chaque ordinateur SGH peut avoir son propre certificat TLS unique tant que l’objet correspond au nom du service SGH.

## <a name="step-6-determine-and-share-the-hgs-attestation-url"></a>Étape 6 : Déterminer et partager l’URL d’attestation SGH

En tant qu’administrateur SGH, vous devez partager l’URL d’attestation de SGH avec les administrateurs d’ordinateurs SQL Server et les administrateurs d’applications de votre organisation. Les administrateurs d’ordinateurs SQL Server ont besoin de l’URL d’attestation pour vérifier que ces ordinateurs peuvent effectuer une attestation avec SGH. Les administrateurs d’applications ont besoin de l’URL d’attestation pour configurer le mode de connexion de leurs applications à SQL Server.

Pour déterminer l’URL d’attestation, exécutez l’applet de commande suivante.

```powershell
Get-HGSServer
```
La sortie de la commande doit ressembler à ce qui suit :

```
Name                           Value                                                                         
----                           -----                                                                         
AttestationOperationMode       HostKey                                                                       
AttestationUrl                 {http://hgs.bastion.local/Attestation}                                        
KeyProtectionUrl               {}         
```

L’URL d’attestation pour votre SGH est la valeur de la propriété AttestationUrl.

## <a name="next-steps"></a>Étapes suivantes

- [Inscrire des ordinateurs auprès de SGH](./always-encrypted-enclaves-host-guardian-service-register.md)