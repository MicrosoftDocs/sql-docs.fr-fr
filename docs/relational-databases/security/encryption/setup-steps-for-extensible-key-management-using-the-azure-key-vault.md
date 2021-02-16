---
title: Configuration de la gestion de clés extensibles Transparent Data Encryption (TDE) avec Azure Key Vault
description: Installer et configurer le connecteur SQL Server pour Azure Key Vault.
ms.custom: seo-lt-2019
ms.date: 11/25/2020
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- Extensible Key Management
- EKM, with key vault setup
- SQL Server Connector, setup
- SQL Server Connector
- TDE, AKV, EKM
ms.assetid: c1f29c27-5168-48cb-b649-7029e4816906
author: Rupp29
ms.author: arupp
ms.openlocfilehash: 83a1b1c7e923362678a013de24b211a9da70be0e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345380"
---
# <a name="set-up-sql-server-tde-extensible-key-management-by-using-azure-key-vault"></a>Configuration de la Gestion de clés extensible de SQL Server TDE avec Azure Key Vault

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Dans cet article, vous découvrirez comment Installer et configurer le [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Connecteur pour Azure Key Vault.  
  
## <a name="prerequisites"></a>Prérequis

Avant de commencer à utiliser Azure Key Vault avec votre instance de SQL Server, assurez-vous que vous respectez les conditions préalables suivantes :  
  
- Vous devez avoir un abonnement Azure.
  
- Installez [Azure PowerShell, version 5.2.0 ou ultérieure](/powershell/azure/).  

- Créez une instance Azure Active Directory (Azure AD).

- Familiarisez-vous avec les principaux du stockage de la gestion de clés extensible à l’aide d’Azure Key Vault en consultant la rubrique [Gestion de clés extensible à l’aide d’Azure Key Vault (SQL Server)](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md).  

- Installez la version de Visual Studio C++ Redistribuable basée sur la version de SQL Server que vous exécutez :
  
  Version de SQL Server  | Visual C++ Version Redistributable
  ---------|---------
  2008, 2008 R2, 2012, 2014 | [Packages redistribuables Visual C++ pour Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
  2016 | [Package redistribuable Visual C++ pour Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)

## <a name="step-1-set-up-an-azure-ad-service-principal"></a>Étape 1 : Paramétrer un principal de service Azure AD

Pour accorder à votre instance SQL Server des autorisations d’accès à votre coffre de clés Azure, vous avez besoin d’un compte de principal de service dans AzureAD.  
  
1. Connectez-vous au [portail Azure](https://ms.portal.azure.com/) et effectuez l’une des actions suivantes :

    - Sélectionnez le bouton **Azure Active Directory**.

      ![Capture d’écran du volet « services Azure »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-login-portal.png)

    - Sélectionnez **Plus de services**, puis dans la boîte de dialogue **Tous les services**, saisissez **Azure Active Directory**.

      ![Capture d’écran du volet « Tous les services Azure »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-select-aad.png)  

1. Inscrivez une application auprès d’Azure Active Directory en suivant les étapes suivantes. (Pour obtenir des instructions pas à pas, consultez la section « Get an identity for the application » (Obtenir une identité pour l’application) du [billet de blog Azure Key Vault](/archive/blogs/kv/azure-key-vault-step-by-step).)

    a. Dans le volet **Vue d’ensemble d’Azure Active Directory**, sélectionnez **Inscriptions d’applications**.

    ![Capture d’écran du volet « Vue d’ensemble d’Azure Active Directory ».](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-app-register.png)

    b. Dans le volet **Inscriptions d’applications**, sélectionnez **Nouvelle inscription**.

    ![Capture d’écran du volet « Inscriptions d’applications »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-registration.png)  

    c. Dans le volet **Inscrire une application**, entrez le nom de l'application pour l'utilisateur, puis sélectionnez **Inscrire**.

    ![Capture d’écran du volet « Inscrire une application »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-register.png)

    d. Dans le volet gauche, sélectionnez **Certificats et secrets**, puis sélectionnez **Nouveau secret client**.

    ![Capture d’écran du volet « Certificats et secrets »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-certs-secrets.png)  

    e. Sous **Ajoutez une clé secrète client**, entrez une description et une date d’expiration appropriée, puis sélectionnez **Ajouter**.

    ![Capture d’écran de la section « Ajouter une clé secrète client »](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-add-secret.png)  

    f. Dans le volet **Certificats et secrets**, sous **« Valeur »** , sélectionnez le bouton **Copier** en regard de la valeur de la clé secrète client à utiliser pour créer une clé asymétrique dans SQL Server.

    ![Capture d’écran de la valeur du secret](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-new-secret.png)  

    g. Dans le volet gauche, sélectionnez **Vue d’ensemble** puis, dans la boîte de dialogue **l’ID de l’application (client)** , copiez la valeur à utiliser pour créer une clé asymétrique dans SQL Server.

    ![Capture d’écran de « ID d’application (client) » dans le volet Vue d’ensemble](../../../relational-databases/security/encryption/media/ekm/ekm-part1-aad-appid.png)  

## <a name="step-2-create-a-key-vault"></a>Étape 2 : Création d’un coffre de clés

Sélectionnez la méthode que vous souhaitez utiliser pour créer un coffre de clés.

## <a name="azure-portal"></a>[Azure portal](#tab/portal)

### <a name="create-a-key-vault-by-using-the-azure-portal"></a>Créez un coffre de clés à l’aide du portail Azure

Vous pouvez utiliser le Portail Azure pour créer le coffre de clés et y ajouter un principal Azure AD.

1. Créez un groupe de ressources.

   Toutes les ressources Azure que vous créez via le Portail Azure doivent être contenues dans un groupe de ressources que vous créez pour héberger votre coffre de clés. Dans cet exemple, le nom de la ressource est *ContosoDevRG*. Choisissez vos propres noms de groupe de ressources et de coffre de clés, car tous les noms de coffre de clés doivent être uniques au monde.

   Dans le volet **Créer un groupe de ressources**, sous **Détails du projet**, entrez les valeurs, puis sélectionnez **Évaluer + Créer**.

      ![Capture d’écran du volet « Créer un groupe de ressources »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-resource-group.png)  

1. Création d’un coffre de clés

    Dans le volet **Créer un coffre de clés**, sélectionnez l’onglet **De base**, entrez les valeurs appropriées, puis sélectionnez **Évaluer + Créer**.

    ![Capture d’écran du volet « Créer un coffre de clés »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-create-key-vault.png)  

1. Sur le volet **Stratégies d’accès**, sélectionnez **Ajouter une stratégie d’accès**.

    ![Capture d’écran du lien « Ajouter une stratégie d’accès » dans le volet « Stratégies d’accès »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-access-policy.png)  

1. Dans le volet **Ajouter une stratégie d'accès**, effectuez les étapes suivantes :
  
    a. Dans la liste déroulante **Configurer à partir du modèle (facultatif)** , sélectionnez **Gestion de clés**.

    b. Dans le volet gauche, sélectionnez l’onglet **Autorisations de clés**, puis vérifiez que les cases à cocher **Get**, **Liste**, **Unwrap Key** et **Wrap Key** sont activées.

    c. Sélectionnez **Ajouter**.

    ![Capture d’écran du volet « Ajouter une stratégie d’accès »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-access-policy-permission.png)

1. Dans le volet de gauche, sélectionnez l'onglet **Sélectionner le principal**, puis procédez comme suit :

    a. Dans le volet **Principal**, sous **Sélectionner**, commencez à taper le nom de votre application Azure AD, puis, dans la liste des résultats, sélectionnez l’application que vous souhaitez ajouter.

    ![Capture d’écran de la zone de recherche d’application sur le volet Principal](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal.png)  

    b. Sélectionnez le bouton **Sélectionner** pour ajouter le principal à votre coffre de clés.

    ![Capture d’écran du bouton Sélectionner du volet Principal](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-principal.png)

    c. En bas à gauche, sélectionnez **Ajouter** pour enregistrer vos modifications.

    ![Capture d’écran du bouton Ajouter dans le volet « Ajouter une stratégie d’accès »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-select-principal-new.png)

1. Dans le volet **Key Vault**, sélectionnez **Clés** et entrez un nom de coffre de clés. Utilisez le type de clé **RSA** et la taille de clé RSA **2048**. Définissez les dates d’activation et d’expiration comm eil convient et définissez **Activé?** sur **Oui**.

   ![Capture d’écran du volet « Créer une clé »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-add-key-vault-key.png)  

1. Sur le volet **Stratégies d’accès**, sélectionnez **Enregistrer**.
  
   ![Capture d’écran du bouton Enregistrer dans le volet « Ajouter une stratégie d’accès »](../../../relational-databases/security/encryption/media/ekm/ekm-part2-save-access-policy.png)  

## <a name="powershell"></a>[PowerShell](#tab/powershell)

### <a name="create-a-key-vault-and-key-by-using-powershell"></a>Créer un coffre de clés et une clé à l’aide de PowerShell

Le coffre de clés et la clé créés ici seront utilisés par le moteur de base de données SQL Server pour la protection des clés de chiffrement.  
  
> [!IMPORTANT]
> L’abonnement dans lequel le coffre de clés est créé doit se trouver dans la même instance Azure AD par défaut que celui où le principal du service Azure AD a été créé. Si vous souhaitez utiliser une instance Active Directory autre que votre instance par défaut pour la création d’un principal du service pour le connecteur SQL Server, vous devez modifier l’instance Active Directory par défaut dans votre compte Azure avant de créer votre coffre de clés. Pour savoir comment remplacer l’instance Azure AD par défaut par celle que vous souhaitez utiliser, consultez la section « Forum aux questions » dans [Maintenance et résolution des problèmes du Connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB).  
  
1. Installez [Azure PowerShell 5.2.0 ou version ultérieure](/powershell/azure/)et connectez-vous à l’aide de la commande suivante :  
  
    ```powershell  
    Connect-AzAccount  
    ```  
  
    L’instruction retourne ceci :  
  
    ```console  
    Environment           : AzureCloud  
    Account               : <account_name>  
    TenantId              : <tenant_id>  
    SubscriptionId        : <subscription_id>  
    CurrentStorageAccount :  
    ```  
  
    > [!NOTE]  
    > Si vous avez plusieurs abonnements et que vous voulez en spécifier un en vue de l’utiliser pour le coffre, exécutez `Get-AzSubscription` pour afficher les abonnements et `Select-AzSubscription` pour choisir l’abonnement approprié. Dans le cas contraire, PowerShell en sélectionne automatiquement un par défaut.  
  
1. Créez un groupe de ressources.

    Toutes les ressources Azure que vous créez via PowerShell doivent être contenues dans un groupe de ressources que vous créez pour héberger votre coffre de clés. Dans cet exemple, le nom de la ressource est *ContosoDevRG*. Choisissez vos propres noms de groupe de ressources et de coffre de clés, car tous les noms de coffre de clés doivent être uniques au monde.  
  
    ```powershell  
    New-AzResourceGroup -Name ContosoDevRG -Location 'East Asia'  
    ```  
  
    L’instruction retourne ceci :  
  
    ```console
    ResourceGroupName: ContosoDevRG  
    Location         : eastasia  
    ProvisioningState: Succeeded  
    Tags             :
    ResourceId       : /subscriptions/<subscription_id>/  
                        resourceGroups/ContosoDevRG  
    ```  
  
    > [!NOTE]
    > Pour le paramètre `-Location` , utilisez la commande `Get-AzureLocation` pour savoir comment spécifier un emplacement autre que celui fourni dans cet exemple. Si vous avez besoin de plus d’informations, tapez **Get-Help Get-AzureLocation**.  
  
1. Création d’un coffre de clés

    L’applet de commande `New-AzKeyVault` nécessite un nom de groupe de ressources, un nom de coffre de clés et un emplacement géographique. Par exemple, pour un coffre de clés nommé `ContosoEKMKeyVault`, exécutez :  
  
    ```powershell  
    New-AzKeyVault -VaultName 'ContosoEKMKeyVault' `  
       -ResourceGroupName 'ContosoDevRG' -Location 'East Asia'  
    ```  
  
    Notez le nom de votre coffre de clés.  
  
    L’instruction retourne ceci :

    ```console
    Vault Name                       : ContosoEKMKeyVault  
    Resource Group Name              : ContosoDevRG  
    Location                         : East Asia  
    ResourceId                       : /subscriptions/<subscription_id>/  
                                        resourceGroups/ContosoDevRG/providers/  
                                        Microsoft/KeyVault/vaults/ContosoEKMKeyVault  
    Vault URI: https://ContosoEKMKeyVault.vault.azure.net  
    Tenant ID                        : <tenant_id>  
    SKU                              : Standard  
    Enabled For Deployment?          : False  
    Enabled For Template Deployment? : False  
    Enabled For Disk Encryption?     : False  
    Access Policies                  :  
             Tenant ID              : <tenant_id>  
             Object ID              : <object_id>  
             Application ID         :
             Display Name           : <display_name>  
             Permissions to Keys    : get, create, delete, list, update, import,
                                      backup, restore  
             Permissions to Secrets : all  
    Tags                             :  
    ```  
  
1. Accorder au principal du service Azure AD des autorisations d’accès au coffre de clés.  
  
    Vous pouvez autoriser d’autres utilisateurs et applications à utiliser votre coffre de clés.  Pour notre exemple, nous allons utiliser le principal de service que vous avez créé au cours de [l’étape 1 : Configurez un principal de service Azure AD](#step-1-set-up-an-azure-ad-service-principal) pour autoriser l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    > [!IMPORTANT]
    > Le principal de service Azure AD doit avoir au moins les autorisations *get*, *list*, *wrapKey*, et *unwrapKey* pour le coffre de clés.  
  
    Comme indiqué dans la commande suivante, vous utilisez **l’ID d’application (client)** à partir de [l’étape 1 : Configurer un principal de service Azure AD](#step-1-set-up-an-azure-ad-service-principal)pour le`ServicePrincipalName` paramètre. La commande `Set-AzKeyVaultAccessPolicy` s’exécute en mode silencieux sans aucune sortie si son exécution réussit.  
  
    ```powershell  
    Set-AzKeyVaultAccessPolicy -VaultName 'ContosoEKMKeyVault' `  
      -ServicePrincipalName 9A57CBC5-4C4C-40E2-B517-EA677 `  
      -PermissionsToKeys get, list, wrapKey, unwrapKey  
    ```  
  
    Appelez l’applet de commande `Get-AzKeyVault` pour vérifier les autorisations. Dans la sortie de l’instruction, sous `Access Policies`, vous devez voir le nom de votre application Azure AD listé comme autre locataire ayant accès à ce coffre de clés.  
  
1. Générer une clé asymétrique dans le coffre de clés. Vous pouvez le faire de deux manières : importer une clé existante ou créer une nouvelle clé.  

     > [!NOTE]
     > SQL Server prend uniquement en charge les clés RSA 2 048 bits et 3 072 bits, ainsi que les clés RSA-HSM 2 048 bits et 3 072 bits.

### <a name="best-practices"></a>Meilleures pratiques

Pour garantir la récupération rapide de clé et être en mesure d’accéder à vos données en dehors d’Azure, nous recommandons les meilleures pratiques suivantes :

- Créez votre clé de chiffrement localement sur un appareil HSM (hardware security module) local. Vérifiez qu’il s’agit d’une clé RSA 2048 ou 3072 asymétrique, donc prise en charge par SQL Server.
- Importez la clé de chiffrement dans votre coffre de clés Azure. Ce processus est décrit dans les prochaines sections.
- Avant d’utiliser la clé dans le coffre de clés Azure pour la première fois, sauvegardez la clé Azure Key Vault. Pour en savoir plus, voir la commande [Backup-AzureKeyVaultKey]().
- Chaque fois que vous modifiez la clé (par exemple, ajout de listes de contrôle d’accès, ajout d’étiquettes, ajout d’attributs de clé), veillez à faire une autre sauvegarde de la clé Azure Key Vault.

  > [!NOTE]
  > La sauvegarde d’une clé est une opération de clé Azure Key Vault qui retourne un fichier que vous pouvez enregistrer à l’emplacement de votre choix.

### <a name="types-of-keys"></a>Types de clés

Vous pouvez générer quatre types de clés dans un coffre de clés Azure qui fonctionnent avec SQL Server. Les clés RSA 2 048 bits et 3 072 bits asymétriques, ainsi que les clés RSA-HSM 2 048 bits et 3 072 bits.
  
- **À protection logicielle** : Traitées dans le logiciel et chiffrées au repos. Les opérations sur les clés à protection logicielle ont lieu sur les machines virtuelles Azure. Nous recommandons ce type pour les clés qui ne sont pas utilisées dans un déploiement de production.  

- **Protégées par HSM** : Créées et protégées par un module de sécurité matériel (HSM) pour une sécurité renforcée. Le coût est d’environ 1 USD par version de clé.  
  
    > [!IMPORTANT]
    > Pour le connecteur SQL Server, le nom de clé doit utiliser uniquement les caractères « a-z », « A-Z », « 0-9 » et « - », avec une limite de 26 caractères.
    > Des versions de clés différentes sous le même nom de clé dans Azure Key Vault ne fonctionnent pas avec le Connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour faire pivoter une clé Azure Key Vault qui est utilisée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], reportez-vous aux étapes relatives à la substitution des clés dans l’annexe. Instructions de maintenance pour la section [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]« Connecteur » dans [Maintenance et résolution des problèmes du Connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).  

### <a name="import-an-existing-key"></a>Importer une clé existante
  
Si vous avez une clé à protection logicielle RSA 2 048 bits existante, vous pouvez charger la clé dans votre coffre de clés Azure. Par exemple, si vous disposez d’un fichier .PFX enregistré sur votre lecteur `C:\\` dans un fichier nommé *softkey.pfx* et que vous souhaitez charger cette clé dans le coffre de clés Azure, exécutez la commande suivante pour définir la variable `securepfxpwd` avec un mot de passe `12987553` pour le fichier .PFX :  
  
``` powershell  
$securepfxpwd = ConvertTo-SecureString -String '12987553' `  
  -AsPlainText -Force  
```  

Ensuite, vous pouvez exécuter la commande suivante pour importer la clé à partir du fichier .PFX, afin de protéger la clé par matériel (recommandé) dans le service Key Vault :  
  
``` powershell  
    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVault' `  
      -Name 'ContosoFirstKey' -KeyFilePath 'c:\softkey.pfx' `  
      -KeyFilePassword $securepfxpwd $securepfxpwd  -Destination 'HSM'  
```  

> [!CAUTION]
> L'importation de la clé asymétrique est fortement recommandée dans les scénarios de production, car elle permet à l'administrateur de déposer la clé dans un système de dépôt de clés (key escrow). Si la clé asymétrique est créée dans le coffre, elle ne peut pas être déposée, car la clé privée ne peut jamais sortir du coffre. Les clés utilisées pour protéger les données critiques doivent être déposées. La perte d’une clé asymétrique entraîne une perte définitive des données.  

### <a name="create-a-new-key"></a>Créer une clé

Vous pouvez aussi créer une nouvelle clé de chiffrement directement dans votre coffre de clés, et la protéger par logiciel ou par HSM.  Dans cet exemple, nous allons créer une clé à protection logicielle à l’aide du `Add-AzureKeyVaultKey` cmdlet :  

``` powershell  
Add-AzureKeyVaultKey -VaultName 'ContosoEKMKeyVault' `  
  -Name 'ContosoRSAKey0' -Destination 'Software'  
```  
  
L’instruction retourne ceci :  
  
```console
Attributes : Microsoft.Azure.Commands.KeyVault.Models.KeyAttributes  
Key        :  {"kid":"https:contosoekmkeyvault.azure.net/keys/  
                ContosoRSAKey0/<guid>","dty":"RSA:,"key_ops": ...  
VaultName  : contosodevkeyvault  
Name       : contosoRSAKey0  
Version    : <guid>  
Id         : https://contosoekmkeyvault.vault.azure.net:443/  
              keys/ContosoRSAKey0/<guid>  
```  

> [!IMPORTANT]
> Le coffre de clés prend en charge plusieurs versions de la même clé nommée, mais les clés que le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doit utiliser ne doivent pas avoir une version ou être restaurées. Si l'administrateur veut modifier la clé utilisée pour le chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], une nouvelle clé avec un autre nom doit être créée dans le coffre de clés et utilisée pour chiffrer la clé de chiffrement.  

---

## <a name="step-3-install-the-ssnoversion-connector"></a>Étape 3 : Installer le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

Téléchargez le connecteur SQL Server à partir du [Centre de téléchargement Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=521700). Ce téléchargement doit être effectué par l’administrateur de l’ordinateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  

> [!NOTE]
> - Les versions de connecteur SQL Server 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production et à l’aide des instructions de la page [Maintenance et résolution des problèmes du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md) sous [Mise à niveau du connecteur SQL Server](sql-server-connector-maintenance-troubleshooting.md#upgrade-of--connector).
> - À partir de la version 1.0.3.0, le connecteur SQL Server signale les messages d’erreur pertinents dans les journaux des événements Windows à des fins de résolution des problèmes.
> - À partir de la version 1.0.4.0, il existe une prise en charge des clouds privés Azure, y compris Azure Chine, Azure Allemagne et Azure Government.
> - Un changement cassant figure dans la version 1.0.5.0, lié à l’algorithme d’empreinte. Vous pouvez rencontrer un échec de restauration des bases de données après la mise à niveau vers la version 1.0.5.0. Pour plus d’informations, consultez [l’article 447099 de la Base de connaissances](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - À partir de la version 1.0.5.0 (horodatage : septembre 2020), le connecteur SQL Server prend en charge le filtrage des messages et la logique de nouvelle tentative de requête réseau.
> - **À compter de la version 1.0.5.0 (horodatage : novembre 2020), le connecteur SQL Server prend en charge les clés RSA 2048, RSA 3072, RSA-HSM 2048 et RSA-HSM 3072.**
  
  ![Capture d’écran de l’Assistant d’installation du Connecteur SQL Server](../../../relational-databases/security/encryption/media/ekm/ekm-connector-install.png)  
  
Par défaut, le Connecteur est installé sur C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault. Cet emplacement peut être changé lors de l'installation. Si vous le modifiez, ajustez les scripts dans la section suivante.  
  
Il n’existe aucune interface pour le Connecteur, mais s’il est correctement installé, le fichier *Microsoft.AzureKeyVaultService.EKM.dll* est installé sur l’ordinateur. Cet assembly est la DLL du fournisseur EKM de services chiffrement qui doit être inscrite auprès de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à l’aide de l’instruction `CREATE CRYPTOGRAPHIC PROVIDER`.  
  
L’installation du connecteur SQL Server vous permet également de télécharger des exemples de scripts pour le chiffrement SQL Server.  
  
Pour consulter des explications des codes d’erreur, les paramètres de configuration ou les tâches de maintenance pour le connecteur SQL Server, reportez-vous à :  
  
- [A. Instructions de maintenance du Connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixA)  
- [C. Explications des codes d’erreur du Connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixC)  
  
## <a name="step-4-configure-ssnoversion"></a>Étape 4 : Configurer [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Reportez-vous à [B. Forum aux questions](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md#AppendixB) pour lire une remarque concernant les niveaux d’autorisation minimaux nécessaires pour chaque action de cette section.  
  
1. Exécutez *sqlcmd.exe* ou ouvrez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Management Studio.  
  
1. Configurez [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour utiliser EKM en exécutant le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant :  
  
    ```sql  
    -- Enable advanced options.  
    USE master;  
    GO  

    EXEC sp_configure 'show advanced options', 1;  
    GO  
    RECONFIGURE;  
    GO  

    -- Enable EKM provider  
    EXEC sp_configure 'EKM provider enabled', 1;  
    GO  
    RECONFIGURE;  
    ```  
  
1. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]Inscrire le connecteur en tant que fournisseur EKM avec [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Créez un fournisseur de services de chiffrement à l’aide du Connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], qui est un fournisseur EKM pour le coffre de clés Azure.
    Dans cet exemple, le nom du fournisseur est `AzureKeyVault_EKM`.  
  
    ```sql  
    CREATE CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM
    FROM FILE = 'C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll';  
    GO  
    ```  

    > [!NOTE]
    > Le chemin d’accès ne peut pas dépasser 256 caractères.  
  
1. Configurer des informations d’identification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]pour un [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] de connexion afin d’utiliser le coffre de clés.  
  
    Des informations d’identification doivent être ajoutées à chaque connexion appelée à effectuer un chiffrement à l’aide d’une clé à partir du coffre de clés. Cela peut inclure :  
  
    - Une connexion administrateur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui utilisera le coffre de clés afin de configurer et de gérer des scénarios de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
    - D’autres connexions [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui peuvent activer TDE ou d’autres fonctionnalités de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
    Il existe un mappage un-à-un entre les informations d’identification et les connexions. Autrement dit, chaque connexion doit disposer de ses propres informations d’identification.  
  
    Modifiez le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] comme suit :  
  
    - Modifiez l’argument `IDENTITY` (`ContosoEKMKeyVault`) de sorte qu’il pointe vers votre coffre de clés Azure.
      - Si vous utilisez *Azure global*, remplacez l’argument `IDENTITY` par le nom de votre coffre de clés Azure de l’étape 2 [ : Créer un coffre de clés](#step-2-create-a-key-vault).
      - Si vous utilisez un *Cloud Azure privé* (par exemple, Azure Government, Azure China 21Vianet ou Azure Allemagne), remplacez l’argument `IDENTITY` par l’URI du coffre renvoyé à l’étape 3 de la section [Créer un coffre de clés et une clé à l’aide de PowerShell](#create-a-key-vault-and-key-by-using-powershell). N’incluez pas « https:/  » dans l’URI du coffre.
    - Remplacez la première partie de l’argument `SECRET` par l’ID client Azure Active Directory mentionné dans l’Étape 1[ : Paramétrer un principal de service Azure AD](#step-1-set-up-an-azure-ad-service-principal). Dans cet exemple, l’**ID client** est `9A57CBC54C4C40E2B517EA677E0EFA00`.  
  
      > [!IMPORTANT]
      > Veillez à bien supprimer les tirets de l’ID de l’application (client).  
  
    - Complétez la deuxième partie de l’argument `SECRET` avec **La clé secrète du client** à partir de [l’Étape 1 : Paramétrer un principal de service Azure AD](#step-1-set-up-an-azure-ad-service-principal).  Dans cet exemple, la clé secrète client est `08:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m`. La chaîne finale pour l’argument `SECRET` est une longue séquence de lettres et de chiffres sans tirets.  
  
    ```sql  
    USE master;  
    CREATE CREDENTIAL sysadmin_ekm_cred
        WITH IDENTITY = 'ContosoEKMKeyVault',                            -- for public Azure
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.usgovcloudapi.net', -- for Azure Government
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.azure.cn',          -- for Azure China 21Vianet
        -- WITH IDENTITY = 'ContosoEKMKeyVault.vault.microsoftazure.de', -- for Azure Germany
               --<----Application (Client) ID ---><--Azure AD app (Client) ID secret-->
        SECRET = '9A57CBC54C4C40E2B517EA677E0EFA0008:k?[:XEZFxcwIPvVVZhTjHWXm7w1?m'
    FOR CRYPTOGRAPHIC PROVIDER AzureKeyVault_EKM;  
  
    -- Add the credential to the SQL Server administrator's domain login
    ALTER LOGIN [<domain>\<login>]  
    ADD CREDENTIAL sysadmin_ekm_cred;  
    ```  
  
    Pour obtenir un exemple d’utilisation de variables pour les arguments `CREATE CREDENTIAL` et la suppression par programmation des tirets de l’ID client, consultez [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md).  
  
1. Ouvrez votre clé Azure Key Vault dans votre instance SQL Server.  

    Que vous ayez créé une clé ou importé une clé asymétrique, comme décrit dans l’Étape 2[ : Créez un coffre de clés](#step-2-create-a-key-vault), vous devrez ouvrir la clé. Ouvrez la clé en spécifiant son nom dans le script [!INCLUDE[tsql](../../../includes/tsql-md.md)] suivant.  
  
    - Remplacez `EKMSampleASYKey` par le nom souhaité pour la clé dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    - Remplacez `ContosoRSAKey0` par le nom de votre clé dans Azure Key Vault.  
  
    ```sql  
    CREATE ASYMMETRIC KEY EKMSampleASYKey
    FROM PROVIDER [AzureKeyVault_EKM]  
    WITH PROVIDER_KEY_NAME = 'ContosoRSAKey0',  
    CREATION_DISPOSITION = OPEN_EXISTING;  
    ```  

1. Créez une nouvelle connexion à l’aide de la clé asymétrique dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que vous avez créée à l’étape précédente.

     ```sql  
    --Create a Login that will associate the asymmetric key to this login
    CREATE LOGIN TDE_Login
    FROM ASYMMETRIC KEY EKMSampleASYKey;
    ```  

1. Créez une connexion à partir de la clé asymétrique dans SQL Server. Supprimez le mappage des informations d’identification de l’étape 4 : Configurez SQL Server pour que les informations d’identification puissent être mappées à la nouvelle connexion.

     ```sql  
    --Now drop the credential mapping from the original association
    ALTER LOGIN [<domain>\<login>]
    DROP CREDENTIAL sysadmin_ekm_cred;
    ```

1. Modifiez la nouvelle connexion et mappez les informations d’identification EKM à la nouvelle connexion.

     ```sql  
    --Now add the credential mapping to the new Login
    ALTER LOGIN TDE_Login
    ADD CREDENTIAL sysadmin_ekm_cred;
    ```  

1. Créez une base de données de test qui sera chiffrée avec la clé Azure Key Vault.

     ```sql
    --Create a test database that will be encrypted with the Azure key vault key
    CREATE DATABASE TestTDE
    ```  

1. Créez une clé de chiffrement de base de données à l’aide de ASYMMETRIC KEY (EKMSampleASYKey).

    ```sql  
    --Create an ENCRYPTION KEY using the ASYMMETRIC KEY (EKMSampleASYKey)
    CREATE DATABASE ENCRYPTION KEY
    WITH ALGORITHM = AES_256
    ENCRYPTION BY SERVER ASYMMETRIC KEY EKMSampleASYKey;
    ```
  
1. Chiffrez la base de données de test. Activez TDE en définissant ENCRYPTION ON.

     ```sql  
    --Enable TDE by setting ENCRYPTION ON
    ALTER DATABASE TestTDE
    SET ENCRYPTION ON;  
     ```  

1. Nettoyez les objets de test. Supprimez tous les objets qui ont été créés dans ce script de test.

    ```sql  
    -- CLEAN UP
    USE Master
    ALTER DATABASE [TestTDE] SET SINGLE_USER WITH ROLLBACK IMMEDIATE
    DROP DATABASE [TestTDE]

    ALTER LOGIN [TDE_Login] DROP CREDENTIAL [sysadmin_ekm_cred]
    DROP LOGIN [TDE_Login]

    DROP CREDENTIAL [sysadmin_ekm_cred]  

    USE MASTER
    DROP ASYMMETRIC KEY [EKMSampleASYKey]
    DROP CRYPTOGRAPHIC PROVIDER [AzureKeyVault_EKM]
    ```  

Pour obtenir des exemples de scripts, consultez le blog sur [Chiffrement des données transparentes SQL Server et la gestion de clés extensible avec Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549).

## <a name="next-steps"></a>Étapes suivantes  
  
Maintenant que vous avez terminé la configuration de base, découvrez comment [Utiliser le Connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).
  
## <a name="see-also"></a>Voir aussi  

- [Gestion de clés extensible TDE avec Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)
- [Maintenance et résolution des problèmes du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)
