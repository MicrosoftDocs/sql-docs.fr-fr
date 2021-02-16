---
title: 'Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server'
description: Ce tutoriel explique comment créer un environnement simple pour Always Encrypted avec enclaves sécurisées dans SQL Server, en utilisant des enclaves VBS et le service Guardian hôte (SGH) pour l’attestation. Vous verrez également comment chiffrer des données sur place et émettre des requêtes confidentielles complexes sur des colonnes chiffrées en utilisant SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 01/15/2021
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: vanto
ms.suite: sql
ms.technology: security
ms.tgt_pltfrm: ''
ms.topic: tutorial
author: jaszymas
ms.author: jaszymas
monikerRange: '>= sql-server-ver15'
ms.openlocfilehash: a33db9a451e8ccf01ecd2f50b832afe4565ebc8c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077820"
---
# <a name="tutorial-getting-started-with-always-encrypted-with-secure-enclaves-in-sql-server"></a>Tutoriel : Bien démarrer avec Always Encrypted avec enclaves sécurisées dans SQL Server
[!INCLUDE [sqlserver2019-windows-only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Ce tutoriel explique comment utiliser [Always Encrypted avec enclaves sécurisées](encryption/always-encrypted-enclaves.md) dans [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)]. Il vous montre comment :

> [!div class="checklist"]
> - Créer un environnement de base pour tester et évaluer Always Encrypted avec enclaves sécurisées.
> - Chiffrer des données sur place et émettre des requêtes confidentielles complexes sur des colonnes chiffrées en utilisant SQL Server Management Studio (SSMS).

## <a name="prerequisites"></a>Prérequis

Pour bien démarrer avec Always Encrypted avec enclaves sécurisées, vous avez besoin d’au moins deux ordinateurs (qui peuvent être des machines virtuelles) :

- L’ordinateur [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] pour héberger [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] et SSMS.
- L’ordinateur SGH pour exécuter le Service Guardian hôte, qui est nécessaire pour l’attestation d’enclave.

### <a name="sql-server-computer-requirements"></a>Configuration requise de l’ordinateur SQL Server

- [!INCLUDE [sssql19-md](../../includes/sssql19-md.md)] ou une version ultérieure.
- Windows 10 Entreprise version 1809 ou ultérieure , ou Windows Server 2019 Édition Datacenter. Les autres éditions de Windows 10 et de Windows Server ne prennent pas en charge l’attestation avec le Service Guardian hôte.
- Prise en charge du processeur pour les technologies de virtualisation :
  - Intel VT-x avec Extended Page Tables.
  - AMD-V avec Rapid Virtualization Indexing.
  - Si vous exécutez [!INCLUDE [ssnoversion-md](../../includes/ssnoversion-md.md)] sur une machine virtuelle, l’hyperviseur et le processeur physique doivent offrir des fonctionnalités de virtualisation imbriquées. 
    - Sur Hyper-V 2016 ou ultérieur, [activez les extensions de virtualisation imbriquée sur le processeur de la machine virtuelle](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - Dans Azure, sélectionnez une taille de machine virtuelle qui prend en charge la virtualisation imbriquée. Ceci comprend toutes les machines virtuelles de la série v3, par exemple Dv3 et Ev3. Voir [Créer une machine virtuelle Azure compatible avec l’imbrication](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - Sur VMware vSphere 6.7 ou ultérieur, activez la prise en charge de la sécurité basée sur la virtualisation pour la machine virtuelle, comme le décrit la [documentation VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html).
    - D’autres hyperviseurs et clouds publics peuvent prendre en charge les fonctionnalités de virtualisation imbriquées qui permettent aussi l’utilisation d’Always Encrypted avec enclaves VBS. Consultez les instructions relatives à la compatibilité et à la configuration dans la documentation de votre solution de virtualisation.
- [SQL Server Management Studio (SSMS) version 18.3 ou ultérieure](../../ssms/download-sql-server-management-studio-ssms.md).

Vous pouvez aussi installer SSMS sur un autre ordinateur.

> [!WARNING]
> Dans les environnements de production, vous ne devez jamais utiliser SSMS ou d’autres outils pour gérer les clés Always Encrypted ni exécuter des requêtes sur des données chiffrées sur l’ordinateur SQL Server, au risque d’atténuer voire de neutraliser complètement l’intérêt d’utiliser Always Encrypted. Consultez [Considérations relatives à la sécurité pour la gestion des clés](encryption/overview-of-key-management-for-always-encrypted.md#security-considerations-for-key-management) pour plus d’informations.

### <a name="hgs-computer-requirements"></a>Configuration requise de l’ordinateur SGH

- Windows Server 2019 Édition Standard ou Datacenter
- 2 UC
- 8 Go de RAM
- Stockage de 100 Go

> [!NOTE]
> L’ordinateur SGH ne doit pas être joint à un domaine avant de commencer.

## <a name="step-1-configure-the-hgs-computer"></a>Étape 1 : Configurer l’ordinateur SGH

Dans cette étape, vous allez configurer l’ordinateur SGH pour exécuter le Service Guardian hôte qui prend en charge l’attestation de clé d’hôte.

1. Connectez-vous à l’ordinateur SGH en tant qu’administrateur (administrateur local), ouvrez une console Windows PowerShell avec élévation de privilèges et ajoutez le rôle de Service Guardian hôte en exécutant la commande suivante :

   ```powershell
   Install-WindowsFeature -Name HostGuardianServiceRole -IncludeManagementTools -Restart
   ```

2. Une fois que l’ordinateur SGH a redémarré, reconnectez-vous avec votre compte d’administrateur, ouvrez une console Windows PowerShell avec élévation de privilèges, puis exécutez les commandes suivantes pour installer le Service Guardian hôte et configurer son domaine. Le mot de passe que vous spécifiez ici s’applique uniquement au mode de réparation des services d’annuaire pour Active Directory ; il ne modifie pas le mot de passe de connexion de votre compte d’administrateur. Vous pouvez spécifier le nom de domaine de votre choix pour - HgsDomainName.

   ```powershell
   $adminPassword = ConvertTo-SecureString -AsPlainText '<password>' -Force
   Install-HgsServer -HgsDomainName 'bastion.local' -SafeModeAdministratorPassword $adminPassword -Restart
   ```

3. Une fois que l’ordinateur a de nouveau redémarré, connectez-vous avec votre compte d’administrateur (qui est aussi désormais administrateur de domaine), ouvrez une console Windows PowerShell avec élévation de privilèges et configurez l’attestation de clé d’hôte pour votre instance SGH. 

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey  
   ```

4. Recherchez l’adresse IP de l’ordinateur SGH en exécutant la commande suivante. Enregistrez cette adresse IP pour les étapes suivantes.

   ```powershell
   Get-NetIPAddress  
   ```

> [!NOTE]
> Sinon, si vous préférez référencer votre ordinateur SGH avec un nom DNS, vous pouvez configurer un redirecteur sur les serveurs DNS de votre entreprise vers le nouveau contrôleur de domaine SGH.  

## <a name="step-2-configure-the-sql-server-computer-as-a-guarded-host"></a>Étape 2 : Configurer l’ordinateur SQL Server comme hôte Service Guardian
Dans cette étape, vous allez configurer l’ordinateur SQL Server comme hôte Service Guardian inscrit auprès de SGH à l’aide de l’attestation de clé d’hôte.

> [!WARNING]
> L’utilisation de l’attestation de clé d’hôte n’est recommandée que dans les environnements de test. Pour les environnements de production, vous devez utiliser l’attestation du module de plateforme sécurisée (TPM).

1. Connectez-vous à votre ordinateur SQL Server en tant qu’administrateur, ouvrez une console Windows PowerShell avec élévation de privilèges et récupérez le nom de votre ordinateur en accédant à la variable computername.

   ```powershell
   $env:computername 
   ```

2. Installez la fonctionnalité de l’hôte Service Guardian, qui va également installer Hyper-V (si ce n’est pas déjà fait).

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All
   ```

3. Quand vous y êtes invité, redémarrez votre ordinateur SQL Server pour terminer l’installation d’Hyper-V.

4. Si votre ordinateur SQL Server est une machine virtuelle, une machine physique qui ne prend pas en charge le démarrage sécurisé UEFI ou une machine physique non équipée d’IOMMU, vous devez supprimer la condition VBS pour les fonctionnalités de sécurité de la plateforme.
    1. Supprimez l’exigence de démarrage sécurisé et d’IOMMU en exécutant la commande suivante sur votre ordinateur SQL Server dans une console PowerShell avec élévation de privilèges :

        ```powershell
       Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Control\DeviceGuard -Name RequirePlatformSecurityFeatures -Value 0
       ```

    1. Redémarrez l’ordinateur SQL Server pour que VBS présente les conditions réduites en ligne.

        ```powershell
       Restart-Computer
       ```

5. Reconnectez-vous à l’ordinateur SQL Server en tant qu’administrateur, ouvrez une console Windows PowerShell avec élévation de privilèges, générez une clé d’hôte unique, puis exportez la clé publique qui en résulte dans un fichier.

   ```powershell
   Set-HgsClientHostKey 
   Get-HgsClientHostKey -Path $HOME\Desktop\hostkey.cer
   ```

6. Copiez manuellement le fichier de clé d’hôte généré à l’étape précédente sur l’ordinateur SGH. Les instructions ci-dessous considèrent que votre nom de fichier est hostkey.cer et que vous l’avez copié sur le bureau de l’ordinateur SGH.

7. Sur l’ordinateur SGH, ouvrez une console Windows PowerShell avec élévation de privilèges et inscrivez la clé d’hôte de votre ordinateur SQL Server auprès de SGH :

   ```powershell
   Add-HgsAttestationHostKey -Name <your SQL Server computer name> -Path $HOME\Desktop\hostkey.cer
   ```

8. Sur l’ordinateur SQL Server, exécutez la commande suivante dans une console Windows PowerShell avec élévation de privilèges pour indiquer à l’ordinateur SQL Server où attester. Veillez à spécifier l’adresse IP ou le nom DNS de votre ordinateur SGH aux deux emplacements d’adresse. 

   ```powershell
   # use http, and not https
   Set-HgsClientConfiguration -AttestationServerUrl http://<IP address or DNS name>/Attestation -KeyProtectionServerUrl http://<IP address or DNS name>/KeyProtection/  
   ```

Le résultat de la commande ci-dessus doit indiquer AttestationStatus = Passed.

Si vous obtenez l’erreur HostUnreachable, cela signifie que votre ordinateur SQL Server ne peut pas communiquer avec SGH. Vérifiez que vous pouvez effectuer un test ping sur l’ordinateur SGH.

L’erreur UnauthorizedHost indique que la clé publique n’a pas été inscrite sur le serveur SGH. Répétez les étapes 5 et 6 pour résoudre l’erreur.

Si le problème persiste, exécutez Remove-HgsClientHostKey et répétez les étapes 4 à 7.

## <a name="step-3-enable-always-encrypted-with-secure-enclaves-in-sql-server"></a>Étape 3 : Activer Always Encrypted avec enclaves sécurisées dans SQL Server

Dans cette étape, vous allez activer la fonctionnalité Always Encrypted avec enclaves dans votre instance SQL Server.

1. À l’aide de SSMS, connectez-vous à votre instance de SQL Server en tant que sysadmin **sans** Always Encrypted activé pour la connexion de base de données.
    1. Démarrer SSMS.
    1. Dans la boîte de dialogue **Se connecter au serveur**, spécifiez le nom de votre serveur, sélectionnez une méthode d’authentification et spécifiez vos informations d’identification.
    1. Cliquez sur **Options >>** et sélectionnez l’onglet **Always Encrypted**.
    1. Assurez-vous que la case **Activer Always Encrypted (chiffrement de colonne)** n’est **pas** cochée.

          ![Se connecter au serveur sans Always Encrypted](./encryption/media/always-encrypted-enclaves/connect-without-always-encrypted-ssms.png)

    1. Sélectionnez **Connecter**.

2. Ouvrez une nouvelle fenêtre de requête et exécutez l’instruction ci-dessous pour définir le type d’enclave sécurisée sur Sécurité basée sur la virtualisation (VBS).

   ```sql
   EXEC sys.sp_configure 'column encryption enclave type', 1;
   RECONFIGURE;
   ```

3. Redémarrez votre instance SQL Server pour que la modification précédente prenne effet. Vous pouvez redémarrer l’instance dans SSMS en cliquant dessus avec le bouton droit dans l’Explorateur d’objets et en sélectionnant Redémarrer. Une fois que l’instance a redémarré, connectez-vous à nouveau.

4. Vérifiez que l’enclave sécurisée est maintenant chargée en exécutant la requête suivante :

   ```sql
   SELECT [name], [value], [value_in_use] FROM sys.configurations
   WHERE [name] = 'column encryption enclave type';
   ```

    La requête doit retourner le résultat suivant :  

    | name                           | value | value_in_use |
    | ------------------------------ | ----- | -------------- |
    | column encryption enclave type | 1     | 1              |

## <a name="step-4-create-a-sample-database"></a>Étape 4 : Créer un exemple de base de données
Dans cette étape, vous allez créer une base de données avec des exemples de données, qui vous chiffrerez par la suite.

1. À l’aide de l’instance SSMS dans l’étape précédente, exécutez l’instruction ci-dessous dans une fenêtre de requête pour créer une nouvelle base de données, nommée **ContosoHR**.

    ```sql
    CREATE DATABASE [ContosoHR];
    ```

1. Créez une nouvelle table nommée **Employés**.

    ```sql
    USE [ContosoHR];
    GO

    CREATE SCHEMA [HR];
    GO
    
    CREATE TABLE [HR].[Employees]
    (
        [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
        [SSN] [char](11) NOT NULL,
        [FirstName] [nvarchar](50) NOT NULL,
        [LastName] [nvarchar](50) NOT NULL,
        [Salary] [money] NOT NULL
    ) ON [PRIMARY];
    ```

1. Ajoutez quelques enregistrements d’employés à la table **Employés**.

    ```sql
    USE [ContosoHR];
    GO

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('795-73-9838'
            , N'Catherine'
            , N'Abel'
            , $31692);

    INSERT INTO [HR].[Employees]
            ([SSN]
            ,[FirstName]
            ,[LastName]
            ,[Salary])
        VALUES
            ('990-00-6818'
            , N'Kim'
            , N'Abercrombie'
            , $55415);
    ```

## <a name="step-5-provision-enclave-enabled-keys"></a>Étape 5 : Approvisionner des clés prenant en charge les enclaves

Dans cette étape, vous allez créer une clé principale de colonne et une clé de chiffrement de colonne qui permettent les calculs d’enclave.

1. À l’aide de l’instance SSMS dans l’étape précédente, dans **Explorateur d’objets**, développez votre base de données et accédez à **Sécurité** > **Clés Always Encrypted**.
1. Provisionnez une nouvelle clé principale de colonne prenant en charge les enclaves :
    1. Cliquez avec le bouton droit sur **Clés Always Encrypted** et sélectionnez **Nouvelle clé principale de colonne...** .
    2. Sélectionnez le nom de votre clé principale de colonne : **CMK1**.
    3. Veillez à sélectionnez **Magasin de certificats Windows (utilisateur actuel ou ordinateur local)** ou **Azure Key Vault**.
    4. Sélectionnez **Autoriser les calculs d’enclave**.
    5. Si vous avez sélectionné Azure Key Vault, connectez-vous à Azure et sélectionnez votre coffre de clés. Pour plus d’informations sur la création d’un coffre de clés pour Always Encrypted, consultez [Gérer vos coffres de clés à partir du portail Azure](/archive/blogs/kv/manage-your-key-vaults-from-new-azure-portal).
    6. Sélectionnez votre certificat ou votre clé Valeur de clé Azure, ou cliquez sur le bouton **Générer un certificat** pour en créer.
    7. Sélectionnez **OK**.

        ![Autoriser les calculs d’enclave](encryption/media/always-encrypted-enclaves/allow-enclave-computations.png)

1. Créez une clé de chiffrement de colonne prenant en charge les enclaves :

    1. Cliquez avec le bouton droit sur **Clés Always Encrypted** et sélectionnez **Nouvelle clé de chiffrement de colonne**.
    2. Entrez un nom pour la nouvelle clé de chiffrement de colonne : **CEK1**.
    3. Dans le menu déroulant **Clé principale de colonne**, sélectionnez la clé principale de colonne que vous avez créée aux étapes précédentes.
    4. Sélectionnez **OK**.

## <a name="step-6-encrypt-some-columns-in-place"></a>Étape 6 : Chiffrer des colonnes sur place

Dans cette étape, vous allez chiffrer les données stockées dans les colonnes **SSN** et **Salaire** à l’intérieur de l’enclave côté serveur, puis vous testerez une requête SELECT sur les données.

1. Ouvrez une nouvelle instance SSMS et connectez-vous à votre instance SQL Server **avec** Always Encrypted activé pour la connexion de base de données.
    1. Démarrez une nouvelle instance SSMS.
    1. Dans la boîte de dialogue **Se connecter au serveur**, spécifiez le nom de votre serveur, sélectionnez une méthode d’authentification et spécifiez vos informations d’identification.
    1. Cliquez sur **Options >>** et sélectionnez l’onglet **Always Encrypted**.
    1. Cochez la case **Activer Always Encrypted (chiffrement de colonne)** et spécifiez l’URL de votre attestation d’enclave (par exemple ht <span>tp://</span>hgs.bastion.local/Attestation).

          ![Se connecter au serveur avec une attestation à l’aide de SSMS](./encryption/media/always-encrypted-enclaves/column-encryption-setting.png)

    1. Sélectionnez **Connecter**.
    1. Si vous êtes invité à activer les requêtes Paramétrage pour Always Encrypted, sélectionnez **Activer**.

1. En utilisant la même instance SSMS (avec Always Encrypted activé), ouvrez une nouvelle fenêtre de requête et chiffrez les colonnes **SSN** et **Salaire** en exécutant les requêtes ci-dessous.

    ```sql
    USE [ContosoHR];
    GO

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [SSN] [char] (11) COLLATE Latin1_General_BIN2
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER TABLE [HR].[Employees]
    ALTER COLUMN [Salary] [money]
    ENCRYPTED WITH (COLUMN_ENCRYPTION_KEY = [CEK1], ENCRYPTION_TYPE = Randomized, ALGORITHM = 'AEAD_AES_256_CBC_HMAC_SHA_256') NOT NULL
    WITH
    (ONLINE = ON);

    ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE;
    ```

    > [!NOTE]
    > Notez l’instruction ALTER DATABASE SCOPED CONFIGURATION CLEAR PROCEDURE_CACHE destinée à effacer le cache du plan de requête de la base de données dans le script ci-dessus. Une fois que vous avez modifié la table, vous devez effacer les plans pour l’ensemble des lots et des procédures stockées qui accèdent à la table, afin d’actualiser les informations de chiffrement des paramètres. 

1. Pour vérifier que les colonnes **SSN** et **Salaire** sont maintenant chiffrées, ouvrez une nouvelle fenêtre de requête dans l’instance SSMS **sans** Always Encrypted activé pour la connexion de base de données et exécutez l’instruction ci-dessous. La fenêtre de requête doit retourner des valeurs chiffrées dans les colonnes **SSN** et **Salaire**. Si vous exécutez la même requête à l’aide de l’instance SSMS avec Always Encrypted activé, vous devez voir les données déchiffrées.

    ```sql
    SELECT * FROM [HR].[Employees];
    ```

## <a name="step-7-run-rich-queries-against-encrypted-columns"></a>Étape 7 : Exécuter des requêtes complexes sur les colonnes chiffrées

À présent, vous pouvez exécuter des requêtes complexes sur les colonnes chiffrées. Un traitement de requête se produit à l’intérieur de votre enclave côté serveur. 

1. Dans l’instance SSMS **avec** Always Encrypted activé, vérifiez que Paramétrage pour Always Encrypted est également activé.
    1. Sélectionnez **Outils** dans le menu principal de SSMS.
    2. Sélectionnez **Options...** .
    3. Accédez à **Exécution de la requête** > **SQL Server** > **Avancé**.
    4. Vérifiez que la case **Activer Paramétrage pour Always Encrypted** est cochée.
    5. Sélectionnez **OK**.
2. Ouvrez une nouvelle fenêtre de requête, collez et exécutez la requête ci-dessous. La requête doit retourner des valeurs de texte en clair et des lignes correspondant aux critères de recherche spécifiés.

    ```sql
    DECLARE @SSNPattern [char](11) = '%6818';
    DECLARE @MinSalary [money] = $1000;
    SELECT * FROM [HR].[Employees]
    WHERE SSN LIKE @SSNPattern AND [Salary] >= @MinSalary;
    ```

3. Essayez à nouveau la même requête dans l’instance SSMS dont Always Encrypted n’est pas activé et notez l’échec survenu.

## <a name="next-steps"></a>Étapes suivantes

À l’issue de ce tutoriel, vous pouvez accéder à l’un des tutoriels suivants :

- [Tutoriel : Développer une application .NET en utilisant Always Encrypted avec enclaves sécurisées](../../connect/ado-net/sql/tutorial-always-encrypted-enclaves-develop-net-apps.md)
- [Tutoriel : Développer une application .NET Framework avec Always Encrypted avec enclaves sécurisées](tutorial-always-encrypted-enclaves-develop-net-framework-apps.md)
- [Tutoriel : Création et utilisation des index sur des colonnes prenant en charge les enclaves à l’aide d’un chiffrement aléatoire](./tutorial-creating-using-indexes-on-enclave-enabled-columns-using-randomized-encryption.md)

## <a name="see-also"></a>Voir aussi

- [Configurer et utiliser Always Encrypted avec enclaves sécurisées](encryption/configure-always-encrypted-enclaves.md)
- [Tutoriel : Always Encrypted avec enclaves sécurisées dans [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]](/azure/azure-sql/database/always-encrypted-enclaves-getting-started)
