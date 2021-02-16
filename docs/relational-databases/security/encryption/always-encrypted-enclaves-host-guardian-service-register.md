---
title: Inscrire un ordinateur auprès du Service Guardian hôte
description: Inscrivez l’ordinateur SQL Server auprès du Service Guardian hôte pour Always Encrypted avec enclaves sécurisées.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 64971ecd3ed3a821ca823ad3cc5d0d05d08669f4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031391"
---
# <a name="register-computer-with-host-guardian-service"></a>Inscrire un ordinateur auprès du Service Guardian hôte

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Cet article décrit comment inscrire des ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auprès du Service Guardian hôte (SGH) à des fins d’attestation.

> [!NOTE]
> Le processus d’inscription d’une instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auprès de SGH nécessite la collaboration de l’administrateur SGH et de l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Consultez [Rôles et responsabilités lors de la configuration de l’attestation avec SGH](always-encrypted-enclaves-host-guardian-service-plan.md#roles-and-responsibilities-when-configuring-attestation-with-hgs).

Avant de commencer, veillez à déployer au moins un ordinateur SGH et à configurer le service d’attestation SGH.
Pour plus d’informations, consultez [Déployer le Service Guardian hôte pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md).

## <a name="step-1-install-the-attestation-client-components"></a>Étape 1 : Installer les composants du client d’attestation

> [!NOTE]
> Cette étape doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Pour qu’un client SQL puisse vérifier qu’il communique avec un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] digne de confiance, le Service Guardian hôte doit attester l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Le processus d’attestation est géré par un composant Windows facultatif appelé « client SGH ».
Les étapes ci-dessous vous aideront à installer ce composant et à commencer l’attestation d’ordinateurs.

1. Vérifiez que l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] répond aux [prérequis listés dans la documentation de planification de SGH](./always-encrypted-enclaves-host-guardian-service-plan.md#prerequisites).

2. Exécutez la commande suivante dans une console PowerShell avec élévation de privilèges pour installer la fonctionnalité Prise en charge d’Hyper-V Guardian hôte, celle-ci contenant le client SGH et les composants d’attestation.

    ```powershell
    Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
    ```

3. Redémarrez pour terminer l’installation.

## <a name="step-2-verify-virtualization-based-security-is-running"></a>Étape 2 : Vérifier que la sécurité basée sur la virtualisation est en cours d’exécution

> [!NOTE]
> Cette étape doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Quand vous installez la fonctionnalité Prise en charge d’Hyper-V Guardian hôte, la sécurité basée sur la virtualisation (VBS) est automatiquement configurée et activée.
Les enclaves pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted s’exécutent dans l’environnement VBS qui assure leur protection.
VBS peut ne pas démarrer si aucun appareil IOMMU n’est installé et activé sur l’ordinateur.
Pour déterminer si VBS est en cours d’exécution, ouvrez l’outil Informations système en exécutant `msinfo32.exe` et recherchez les éléments `Virtualization-based security` dans la partie inférieure de Résumé système.

![Capture d’écran de l’outil Informations système montrant l’état et la configuration de la sécurité basée sur la virtualisation](./media/always-encrypted-enclaves/msinfo32-vbs-status.png)

Le premier élément à examiner est `Virtualization-based security`. Il peut avoir l’une des trois valeurs suivantes :

- `Running` signifie que la fonctionnalité VBS est correctement configurée et qu’elle a pu démarrer. Si l’ordinateur affiche cet état, vous pouvez passer à l’étape 3.
- `Enabled but not running` signifie que la fonctionnalité VBS est configurée pour s’exécuter, mais que le matériel ne répond pas aux exigences minimales de sécurité pour exécuter VBS. Vous devrez peut-être changer la configuration du matériel dans le BIOS ou l’UEFI pour activer des fonctionnalités de processeur facultatives comme une unité IOMMU. Ou, si le matériel ne prend vraiment pas en charge les fonctionnalités requises, vous devrez peut-être réduire les exigences de sécurité de VBS. Continuez à lire cette section pour en savoir plus.
- `Not enabled` signifie que la fonctionnalité VBS n’est pas configurée pour s’exécuter. Étant donné que la fonctionnalité Prise en charge d’Hyper-V Guardian hôte active automatiquement VBS, nous vous recommandons de répéter l’étape 1 si vous voyez cet état.

Si VBS n’est pas en cours d’exécution sur l’ordinateur, examinez les propriétés `Virtualization-based security`. Comparez les valeurs de l’élément `Required Security Properties` à celles de l’élément `Available Security Properties`.
Les propriétés obligatoires doivent être égales aux propriétés de sécurité disponibles ou à un sous-ensemble de celles-ci pour que VBS s’exécute.

Dans le contexte de l’attestation d’enclaves [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], les propriétés de sécurité varient en importance :

- La propriété `Base virtualization support` est toujours obligatoire, car elle représente les fonctionnalités matérielles minimales pour exécuter un hyperviseur.
- La propriété `Secure Boot` est recommandée, mais elle n’est pas obligatoire pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. Le démarrage sécurisé assure une protection contre les rootkits, car il exige l’exécution d’un chargeur de démarrage signé par Microsoft immédiatement après l’initialisation de l’UEFI. Si vous utilisez l’attestation du module de plateforme sécurisée (TPM), l’activation du démarrage sécurisé est mesurée et appliquée, que VBS soit configuré ou non pour exiger un démarrage sécurisé.
- La propriété `DMA Protection` est recommandée, mais elle n’est pas obligatoire pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] Always Encrypted. La protection DMA utilise une unité IOMMU pour protéger la mémoire de VBS et de l’enclave contre les attaques d’accès direct à la mémoire. Dans un environnement de production, vous devez toujours utiliser des ordinateurs avec une protection DMA. Dans un environnement de développement/test, vous pouvez supprimer l’obligation d’utiliser une protection DMA. Si l’instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est virtualisée, la protection DMA n’est probablement pas disponible et vous devrez supprimer l’obligation d’exécuter VBS. Pour plus d’informations sur la réduction des garanties de sécurité en cas d’exécution dans une machine virtuelle, passez en revue le [modèle d’approbation](./always-encrypted-enclaves-host-guardian-service-plan.md#trust-model).

Avant d’abaisser le niveau de sécurité exigé par VBS, contactez votre fabricant d’ordinateurs OEM ou votre fournisseur de services cloud afin de voir s’il existe un moyen d’activer les spécifications de plateforme manquantes dans l’UEFI ou le BIOS (par exemple, en activant le démarrage sécurisé, Intel VT-d ou AMD IOV).

Pour changer les fonctionnalités de sécurité de la plateforme obligatoires pour VBS, exécutez la commande suivante dans une console PowerShell avec élévation de privilèges :

```powershell
# Value 0 = No security features required (use this for Azure VMs)
# Value 1 = Only Secure Boot is required
# Value 2 = Only DMA protection is required (default configuration)
# Value 3 = Both Secure Boot and DMA protection are required
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
```

Après avoir modifié le Registre, redémarrez l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et vérifiez si VBS s’exécute à nouveau.

Si l’ordinateur est géré par votre entreprise, une stratégie de groupe ou le Gestionnaire de points de terminaison de Microsoft peut remplacer toute modification apportée à ces clés de Registre après le redémarrage.
Contactez votre support technique pour savoir si des stratégies gérant votre configuration VBS sont déployées.

## <a name="step-3-configure-the-attestation-url"></a>Étape 3 : Configurer l’URL d’attestation

> [!NOTE]
> Cette étape doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Ensuite, vous allez configurer l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] avec l’URL du service d’attestation SGH, que vous avez obtenue auprès de l’administrateur SGH.

Dans une console PowerShell avec élévation de privilèges, mettez à jour et exécutez la commande suivante pour configurer l’URL d’attestation.

- Remplacez `hgs.bastion.local` par le nom du cluster SGH.
- Vous pouvez exécuter `Get-HgsServer` sur n’importe quel ordinateur SGH pour récupérer le nom du cluster.
- L’URL d’attestation doit toujours se terminer par `/Attestation`.
- SQL Server ne tirant pas parti des fonctionnalités de protection de clé de SGH, indiquez une URL factice comme `http://localhost` pour `-KeyProtectionServerUrl`.

```powershell
Set-HgsClientConfiguration -AttestationServerUrl "https://hgs.bastion.local/Attestation" -KeyProtectionServerUrl "http://localhost"
```

À moins que vous n’ayez déjà inscrit cet ordinateur auprès de SGH, la commande signale un échec d’attestation. Ce résultat est normal.

Le champ `AttestationMode` dans la sortie de l’applet de commande indique le mode d’attestation utilisé par SGH.

Passez à l’[étape 4A](#step-4a-register-a-computer-in-tpm-mode) pour inscrire l’ordinateur en mode TPM ou à [l’étape 4B](#step-4b-register-a-computer-in-host-key-mode) pour l’inscrire en mode clé d’hôte.

## <a name="step-4a-register-a-computer-in-tpm-mode"></a>Étape 4A : Inscrire un ordinateur en mode TPM

> [!NOTE]
> Cette étape est effectuée conjointement par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et par l’administrateur SGH. Pour plus d’informations, consultez les notes ci-dessous.

### <a name="prepare"></a>Préparation

> [!NOTE]
> Cette action doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Dans cette étape, vous collectez des informations sur l’état du TPM de l’ordinateur et l’inscrivez auprès de SGH.

Si le service d’attestation SGH est configuré pour utiliser le mode de clé d’hôte, passez à l’[étape 4B](#step-4b-register-a-computer-in-host-key-mode).

Avant de commencer à collecter les mesures du module TPM, vérifiez que vous utilisez une configuration valide connue de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
L’ordinateur doit être équipé de tout le matériel nécessaire. Son microprogramme et ses logiciels doivent également être à jour.
SGH mesure les ordinateurs par rapport à cette base de référence au moment de l’attestation. Il est donc important d’être dans l’état souhaité le plus sécurisé possible lors de la collecte des mesures du TPM.

Trois fichiers de données sont collectés pour l’attestation TPM, dont certains peuvent être réutilisés si vos ordinateurs sont configurés de manière identique.

| Artefact d’attestation | Ce qu’il mesure | Unicité |
| -------------------- | ---------------- | ---------- |
| Identificateur de plateforme  | Paire de clés de type EK (Endorsement Key) publique dans le TPM de l’ordinateur et certificat de paire de clés de type EK du fabricant du TPM. | 1 pour chaque ordinateur |
| Base de référence TPM | Registres de contrôle de plateforme (PCR) dans le TPM qui mesurent la configuration du microprogramme et du système d’exploitation chargée pendant le processus de démarrage. Citons par exemple l’état du démarrage sécurisé et le chiffrement ou non des vidages sur incident. | 1 base de référence par configuration d’ordinateur (un ordinateur aux caractéristiques matérielles et logiciels identiques peut utiliser la même base de référence) |
| Stratégie d’intégrité du code | Stratégie de [contrôle d’application Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control) que vous approuvez pour protéger les ordinateurs | 1 par stratégie CI unique déployée sur les ordinateurs |

Vous pouvez configurer plusieurs artefacts d’attestation de chaque type sur SGH pour prendre en charge des configurations matérielles et logicielles diversifiées.
SGH exige uniquement qu’un ordinateur en attente d’attestation respecte une stratégie dans chaque catégorie de stratégie.
Par exemple, si vous avez trois bases de référence TPM inscrites sur SGH, il suffit que les mesures de l’ordinateur correspondent à l’une de ces bases de référence pour que les exigences de la stratégie soient satisfaites.

### <a name="configure-a-code-integrity-policy"></a>Configurer une stratégie d’intégrité du code

> [!NOTE]
> Les étapes ci-dessous doivent être effectuées par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

SGH exige qu’une stratégie de contrôle d’application Windows Defender (WDAC) soit appliquée à chaque ordinateur en attente d’attestation en mode TPM.
Les stratégies d’intégrité du code WDAC restreignent les logiciels qui peuvent s’exécuter sur un ordinateur. Pour cela, elles vérifient chaque processus qui tente d’exécuter du code par rapport à une liste d’éditeurs et de hachages de fichiers approuvés.
Pour le cas d’usage [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], les enclaves sont protégées par la sécurité basée sur la virtualisation et ne peuvent pas être modifiées à partir du système d’exploitation hôte. La rigueur de la stratégie WDAC n’affecte donc pas la sécurité des requêtes chiffrées.
Par conséquent, nous vous recommandons de déployer une stratégie de mode d’audit sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] afin de respecter les exigences d’attestation sans imposer de restrictions supplémentaires au système.

Si vous utilisez déjà une stratégie d’intégrité du code WDAC personnalisée sur les ordinateurs pour renforcer la configuration du système d’exploitation, vous pouvez passer à [Collecter les informations d’attestation du TPM](#collect-tpm-attestation-information).

1. Des exemples de stratégies prédéfinies sont disponibles sur Windows Server 2019, Windows 10 version 1809 et ultérieur. La stratégie `AllowAll` permet à n’importe quel logiciel de s’exécuter sur l’ordinateur sans aucune restriction. Convertissez la stratégie en une forme binaire comprise par le système d’exploitation et SGH pour pouvoir vous en servir. Dans une console PowerShell avec élévation de privilèges, exécutez les commandes suivantes pour compiler la stratégie `AllowAll` :

    ```powershell
    # We are changing the policy to disable enforcement and user mode code protection before compiling
    $temppolicy = "$HOME\Desktop\allowall_edited.xml"
    Copy-Item -Path "$env:SystemRoot\schemas\CodeIntegrity\ExamplePolicies\AllowAll.xml" -Destination $temppolicy
    Set-RuleOption -FilePath $temppolicy -Option 0 -Delete
    Set-RuleOption -FilePath $temppolicy -Option 3

    ConvertFrom-CIPolicy -XmlFilePath $temppolicy -BinaryFilePath "$HOME\Desktop\allowall_cipolicy.bin"
    ```

2. Suivez les instructions du [guide de déploiement du contrôle d’application Windows Defender](/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide) pour déployer le fichier `allowall_cipolicy.bin` sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] à l’aide de la [stratégie de groupe](/windows/security/threat-protection/windows-defender-application-control/deploy-windows-defender-application-control-policies-using-group-policy). Pour les ordinateurs de groupe de travail, suivez le même processus à l’aide de l’Éditeur de stratégie de groupe locale (`gpedit.msc`).

3. Exécutez `gpupdate /force` sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pour configurer la nouvelle stratégie d’intégrité du code, puis redémarrez les ordinateurs pour appliquer la stratégie.

### <a name="collect-tpm-attestation-information"></a>Collecter les informations d’attestation du TPM

> [!NOTE]
> Les étapes ci-dessous doivent être effectuées par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Répétez les étapes suivantes pour chaque ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] à attester avec SGH :

1. L’ordinateur étant dans un état valide connu, exécutez les commandes suivantes dans PowerShell pour collecter les informations d’attestation du TPM :

    ```powershell
    # Collects the TPM EKpub and EKcert
    $name = $env:computername
    $path = "$HOME\Desktop"
    (Get-PlatformIdentifier -Name $name).Save("$path\$name-EK.xml")

    # Collects the TPM baseline (current PCR values)
    Get-HgsAttestationBaselinePolicy -Path "$path\$name.tcglog" -SkipValidation

    # Collects the applied CI policy, if one exists
    Copy-Item -Path "$env:SystemRoot\System32\CodeIntegrity\SIPolicy.p7b" -Destination "$path\$name-CIpolicy.bin"
    ```

2. Partagez les trois fichiers d’attestation avec l’administrateur SGH. 

### <a name="register-the-sql-server-computer-with-hgs"></a>Inscrire l’ordinateur SQL Server auprès de SGH

> [!NOTE]
> Les étapes ci-dessous doivent être effectuées par l’administrateur SGH.

Répétez les étapes suivantes pour chaque ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] à attester avec SGH :

1. Copiez les fichiers d’attestation que vous avez obtenus de l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sur le serveur SGH. 
2. Sur le serveur SGH, exécutez les commandes suivantes dans une console PowerShell avec élévation de privilèges pour inscrire l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] :

    ```powershell
    # TIP: REMEMBER TO CHANGE THE FILENAMES
    # Registers the unique TPM with HGS (required for every computer)
    Add-HgsAttestationTpmHost -Path "C:\temp\SQL01-EK.xml"

    # Registers the TPM baseline (required ONCE for each unique hardware and software configuration)
    Add-HgsAttestationTpmPolicy -Name "MyHWSoftwareConfig" -Path "C:\temp\SQL01.tcglog"

    # Registers the CI policy (required ONCE for each unique CI policy)
    Add-HgsAttestationCiPolicy -Name "AllowAll" -Path "C:\temp\SQL01-CIpolicy.bin"
    ```

    > [!TIP]
    > Si vous rencontrez une erreur lors de l’inscription de l’identificateur TPM unique, vérifiez que vous avez [importé les certificats racines et intermédiaires du TPM](./always-encrypted-enclaves-host-guardian-service-deploy.md#switch-to-tpm-attestation) sur l’ordinateur SGH que vous utilisez.

Outre l’identificateur de plateforme, la base de référence TPM et la stratégie d’intégrité du code, vous devrez peut-être changer des stratégies intégrées qui sont configurées et appliquées par SGH.
Ces stratégies intégrées sont mesurées par rapport à la base de référence TPM que vous collectez à partir du serveur et représentent un large éventail de paramètres de sécurité qui doivent être activés pour protéger l’ordinateur.
Si vous avez des ordinateurs qui ne disposent pas d’une unité IOMMU pour vous protéger contre les attaques DMA (par exemple, une machine virtuelle), vous devez désactiver la stratégie IOMMU.

Pour désactiver l’exigence IOMMU, exécutez la commande suivante sur le serveur SGH :

```powershell
Disable-HgsAttestationPolicy Hgs_IommuEnabled
```

> [!NOTE]
> Si vous désactivez la stratégie IOMMU, aucune unité IOMMU ne sera exigée pour tout ordinateur en attente d’attestation par SGH.
> Vous ne pouvez pas désactiver la stratégie IOMMU pour un seul ordinateur.

Vous pouvez passer en revue la liste des stratégies et hôtes TPM inscrits à l’aide des commandes PowerShell suivantes :

```powershell
Get-HgsAttestationTpmHost
Get-HgsAttestationTpmPolicy
```

## <a name="step-4b-register-a-computer-in-host-key-mode"></a>Étape 4B : Inscrire un ordinateur en mode clé d’hôte

> [!NOTE]
> Cette étape est effectuée conjointement par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et par l’administrateur SGH. Pour plus d’informations, consultez les notes ci-dessous.

Cette étape vous guide tout au long du processus de génération d’une clé unique pour l’hôte et de son inscription auprès de SGH.
Si le service d’attestation SGH est configuré pour utiliser le mode TPM, suivez plutôt les instructions de l’[étape 4A](#step-4a-register-a-computer-in-tpm-mode).

### <a name="generate-a-key-for-a-ssnoversion-md-computer"></a>Générer une clé pour un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

> [!NOTE]
> Cette étape doit être effectuée en collaboration avec l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

L’attestation de clé d’hôte génère une paire de clés asymétriques sur l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et fournit à SGH la moitié publique de cette clé.

Répétez les étapes suivantes pour chaque ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] à attester avec SGH :

1. Pour générer la paire de clés, exécutez la commande suivante dans une console PowerShell avec élévation de privilèges :

    ```powershell
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

    Si vous avez déjà créé une clé d’hôte et que vous souhaitez générer une nouvelle paire de clés, utilisez les commandes suivantes à la place :

    ```powershell
    Remove-HgsClientHostKey
    Set-HgsClientHostKey
    Get-HgsClientHostKey -Path "$HOME\Desktop\$env:computername-key.cer"
    ```

2. Partagez le fichier de certificat avec l’administrateur SGH.

### <a name="register-the-sql-server-computer-with-hgs"></a>Inscrire l’ordinateur SQL Server auprès de SGH

> [!NOTE]
> Les étapes ci-dessous doivent être effectuées par l’administrateur SGH.

Répétez les étapes suivantes pour chaque ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] à attester avec SGH :

1. Copiez le fichier de certificat que vous avez obtenu de l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sur un serveur SGH.
2. Pour inscrire l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], exécutez les commandes suivantes dans une console PowerShell avec élévation de privilèges :

    ```powershell
    Add-HgsAttestationHostKey -Name "YourComputerName" -Path "C:\temp\yourcomputername.cer"
   ```

## <a name="step-5-confirm-the-host-can-attest-successfully"></a>Étape 5 : Confirmer que l’hôte peut effectuer correctement une attestation

> [!NOTE]
> Cette étape doit être effectuée par l’administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].

Une fois que vous avez inscrit l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auprès de SGH ([étape 4A](#step-4a-register-a-computer-in-tpm-mode) pour le mode TPM, [étape 4B](#step-4b-register-a-computer-in-host-key-mode) pour le mode clé d’hôte), vous devez confirmer qu’il peut effectuer une attestation.

Vous pouvez vérifier la configuration du client d’attestation SGH et effectuer une tentative d’attestation à tout moment avec [Get-HgsClientConfiguration](/powershell/module/hgsclient/get-hgsclientconfiguration?view=win10-ps&preserve-view=true).

La sortie de la commande doit ressembler à ce qui suit :

```
PS C:\> Get-HgsClientConfiguration


IsHostGuarded                  : True
Mode                           : HostGuardianService
KeyProtectionServerUrl         : http://localhost
AttestationServerUrl           : http://hgs.bastion.local/Attestation
AttestationOperationMode       : HostKey
AttestationStatus              : Passed
AttestationSubstatus           : NoInformation
FallbackKeyProtectionServerUrl :
FallbackAttestationServerUrl   :
IsFallbackInUse                : False
```

Les deux champs les plus importants de la sortie sont `AttestationStatus` (qui indique si l’ordinateur a obtenu l’attestation) et `AttestationSubStatus` (qui explique les stratégies que l’ordinateur n’a pas respectées en cas d’échec de l’attestation de l’ordinateur).

Les valeurs les plus courantes pouvant apparaître dans `AttestationStatus` sont expliquées ci-dessous :

| `AttestationStatus` | Explication |
| ----------------- | ----------- |
| Expiré | L’hôte a déjà obtenu l’attestation, mais le certificat d’intégrité qu’il a reçu a expiré. Vérifiez que l’heure de l’hôte et celle du SGH sont synchronisées. |
| `InsecureHostConfiguration` | L’ordinateur ne respecte pas une ou plusieurs des stratégies d’attestation configurées sur le serveur SGH. Pour plus d’informations, consultez `AttestationSubStatus`. |
| NotConfigured | L’ordinateur n’est pas configuré avec une URL d’attestation. [Configurer l’URL d’attestation](#step-3-configure-the-attestation-url) |
| Passed | L’ordinateur a obtenu l’attestation et est autorisé à exécuter des enclaves [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |
| `TransientError` | La tentative d’attestation a échoué en raison d’une erreur temporaire. Cette erreur signifie généralement qu’un problème s’est produit au moment de contacter SGH sur le réseau. Examinez la connexion réseau et vérifiez que l’ordinateur peut résoudre et acheminer le nom du service SGH. |
| `TpmError` | L’appareil TPM de l’ordinateur a signalé une erreur durant la tentative d’attestation. Consultez les journaux TPM pour plus d’informations. L’effacement du TPM peut résoudre le problème, mais veillez à suspendre BitLocker et d’autres services qui reposent sur le TPM avant de l’effacer. |
| `UnauthorizedHost` | La clé d’hôte n’est pas connue du SGH. Suivez les instructions de l’[étape 4B](#step-4b-register-a-computer-in-host-key-mode) pour inscrire l’ordinateur auprès de SGH. |

Si `AttestationStatus` indique `InsecureHostConfiguration`, le champ `AttestationSubStatus` contient un ou plusieurs noms de stratégie ayant échoué.
Le tableau ci-dessous liste les valeurs les plus courantes et explique comment corriger les erreurs.

| AttestationSubStatus | Ce que cela signifie et que faire |
| -------------------- | ---------------------------- |
| CodeIntegrityPolicy | La stratégie d’intégrité du code sur l’ordinateur n’est pas inscrite auprès du SGH *ou* l’ordinateur n’utilise pas actuellement de stratégie d’intégrité du code. Pour obtenir de l’aide, consultez [Configurer une stratégie d’intégrité du code](#configure-a-code-integrity-policy). |
| DumpsEnabled | L’ordinateur est configuré pour autoriser les vidages sur incident, mais la stratégie Hgs_DumpsEnabled interdit les vidages. Désactivez les vidages sur cet ordinateur ou désactivez la stratégie Hgs_DumpsEnabled pour continuer. |
| FullBoot | L’ordinateur est sorti d’un état de veille ou de veille prolongée, ce qui a entraîné des changements au niveau des mesures TPM. Redémarrez l’ordinateur pour générer des mesures TPM propres. |
| HibernationEnabled | L’ordinateur est configuré pour autoriser la mise en veille prolongée avec des fichiers de mise en veille prolongée non chiffrés. Pour résoudre ce problème, désactivez la mise en veille prolongée sur l’ordinateur. |
| HypervisorEnforcedCodeIntegrityPolicy | L’ordinateur n’est pas configuré pour utiliser une stratégie d’intégrité du code. Vérifiez la Stratégie de groupe ou la Stratégie de groupe locale > Configuration ordinateur > Modèles d’administration > Système > Device Guard > Activer la sécurité basée sur la virtualisation > Protection basée sur la virtualisation de l’intégrité du code. Cet élément de stratégie doit être « activé sans verrouillage de l’UEFI ». |
| Iommu | Aucune appareil IOMMU n’est activé sur cet ordinateur. S’il s’agit d’un ordinateur physique, activez l’unité IOMMU dans le menu Configuration UEFI. S’il s’agit d’une machine virtuelle et qu’aucune unité IOMMU n’est disponible, exécutez `Disable-HgsAttestationPolicy Hgs_IommuEnabled` sur le serveur SGH. |
| SecureBoot | Le démarrage sécurisé n’est pas activé sur cet ordinateur. Activez le démarrage sécurisé dans le menu de configuration UEFI pour résoudre cette erreur. |
| VirtualSecureMode | La sécurité basée sur la virtualisation n’est pas en cours d’exécution sur cet ordinateur. Suivez les instructions de l’[Étape 2 : Vérifier que VBS est en cours d’exécution sur l’ordinateur](#step-2-verify-virtualization-based-security-is-running). |

## <a name="next-steps"></a>Étapes suivantes

- [Configurer l’enclave sécurisée dans SQL Server](always-encrypted-enclaves-configure-enclave-type.md)