---
title: Résolution des problèmes et maintenance du connecteur SQL Server
description: Découvrez les instructions de maintenance et les étapes courantes de résolution des problèmes pour le connecteur SQL Server.
ms.custom: seo-lt-2019
ms.date: 10/08/2019
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Connector, appendix
ms.assetid: 7f5b73fc-e699-49ac-a22d-f4adcfae62b1
author: jaszymas
ms.author: jaszymas
ms.openlocfilehash: 7bbd8aee97ccd31649661032c76729ad89b7fe5a
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622745"
---
# <a name="sql-server-connector-maintenance--troubleshooting"></a>Résolution des problèmes et maintenance du connecteur SQL Server

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

  Des informations supplémentaires sur le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont fournies dans cette rubrique. Pour plus d’informations sur le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], consultez [Gestion de clés extensible à l’aide d’Azure Key Vault &#40;SQL Server&#41;](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md), [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault ](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md) et [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md).  
  
##  <a name="a-maintenance-instructions-for-ssnoversion-connector"></a><a name="AppendixA"></a> A. Instructions de maintenance du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
### <a name="key-rollover"></a>Substitution de clé  
  
> [!IMPORTANT]  
> Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] exige que le nom de clé utilise uniquement les caractères « a-z », « A-Z », « 0-9 » et « - », avec une limite de 26 caractères.
> Des versions de clés différentes sous le même nom de clé dans Azure Key Vault ne fonctionnent pas avec le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Pour permuter une clé Azure Key Vault utilisée par [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], une clé portant un nouveau nom de clé doit être créée.  
  
 En général, les clés asymétriques de serveur pour le chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] doivent faire l’objet d’une gestion des versions une fois par an ou une fois tous les deux ans. Bien que le coffre de clés prenne en charge la gestion des versions des clés, les clients ne doivent pas utiliser cette fonctionnalité pour implémenter la gestion des versions. Le connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas traiter les modifications apportées à la version des clés du coffre de clés. Pour implémenter la gestion des versions des clés, créez une clé dans le coffre de clés, puis rechiffrez la clé de chiffrement de données dans [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)].  
  
 Dans le cas du chiffrement transparent des données, voici comment procéder :  
  
- **Dans PowerShell :** Créez une clé asymétrique (avec un nom différent de celui de votre clé asymétrique TDE) dans le coffre de clés.  
  
    ```powershell  
    Add-AzKeyVaultKey -VaultName 'ContosoDevKeyVault' `  
      -Name 'Key2' -Destination 'Software'  
    ```  
  
- **À l’aide de [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] ou sqlcmd.exe :** Utilisez les instructions suivantes, comme indiqué à l’étape 3 de la section 3.  
  
     Importez la nouvelle clé asymétrique.  
  
    ```sql  
    USE master  
    CREATE ASYMMETRIC KEY [MASTER_KEY2]
    FROM PROVIDER [EKM]
    WITH PROVIDER_KEY_NAME = 'Key2',
    CREATION_DISPOSITION = OPEN_EXISTING
    GO  
    ```  
  
     Créez une connexion à associer à la nouvelle clé asymétrique (comme indiqué dans les instructions sur le chiffrement transparent des données).  
  
    ```sql  
    USE master  
    CREATE LOGIN TDE_Login2
    FROM ASYMMETRIC KEY [MASTER_KEY2]  
    GO  
    ```  
  
     Créez des informations d’identification à associer à la connexion.  
  
    ```sql  
    CREATE CREDENTIAL Azure_EKM_TDE_cred2  
        WITH IDENTITY = 'ContosoDevKeyVault',
       SECRET = 'EF5C8E094D2A4A769998D93440D8115DAADsecret123456789='
    FOR CRYPTOGRAPHIC PROVIDER EKM;  
  
    ALTER LOGIN TDE_Login2  
    ADD CREDENTIAL Azure_EKM_TDE_cred2;  
    GO  
    ```  
  
     Choisissez la base de données dont vous souhaitez chiffrer à nouveau la clé de chiffrement de base de données.  
  
    ```sql  
    USE [database]  
    GO  
    ```  
  
     Rechiffrez la clé de chiffrement de base de données.  
  
    ```sql  
    ALTER DATABASE ENCRYPTION KEY
    ENCRYPTION BY SERVER ASYMMETRIC KEY [MASTER_KEY2];  
    GO  
    ```  
  
### <a name="upgrade-of-ssnoversion-connector"></a>Mise à niveau du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

Les versions 1.0.0.440 et antérieures ont été remplacées et ne sont plus prises en charge dans les environnements de production. Les versions 1.0.1.0 et les versions antérieures ne sont pas prises en charge dans les environnements de production. Utilisez les instructions suivantes pour effectuer une mise à niveau vers la dernière version disponible sur le [Centre de téléchargement Microsoft](https://www.microsoft.com/download/details.aspx?id=45344).

### <a name="upgrade"></a>Mettre à niveau

1. Arrêter le service SQL Server à l’aide du Gestionnaire de configuration SQL Server
1. Désinstaller l’ancienne version à l’aide de Panneau de configuration\Programmes\Programmes et fonctionnalités
    1. Nom de l’application : Connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.300.96 (ou antérieure)
    1. Date du fichier DLL : 30/01/2018 15:00 (ou antérieure)
1. Installer (mettre à niveau) le nouveau connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.2000.367
    1. Date du fichier DLL : 11/09/2020 ‏‎5:17
1. Démarrer le service SQL Server
1. Vérifier si la ou les bases de données chiffrées sont accessibles

### <a name="rollback"></a>Restauration

1. Arrêter le service SQL Server à l’aide du Gestionnaire de configuration SQL Server

1. Désinstaller la nouvelle version à l’aide de Panneau de configuration\Programmes\Programmes et fonctionnalités
    1. Nom de l’application : Connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.2000.367
    1. Date du fichier DLL : 11/09/2020 ‏‎5:17

1. Installer l’ancienne version du connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.300.96
    1. Date du fichier DLL : 30/01/2018 15:00
1. Démarrer le service SQL Server

1. Vérifiez que les bases de données qui utilisent le chiffrement TDE sont accessibles.  
  
1. Après avoir vérifié que la mise à jour fonctionne, vous pouvez supprimer le dossier de l’ancien connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] si vous avez choisi de le renommer au lieu de le désinstaller à l’étape 3.

### <a name="older-versions-of-the-sql-server-connector"></a>Versions antérieures du connecteur SQL Server
  
Liens ciblés vers les versions antérieures du connecteur SQL Server

- Intensité : [1.0.5.0 (version 15.0.2000.367) – Date de fichier du 11 septembre 2020](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/1033_15.0.2000.367/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.5.0 (version 15.0.300.96) – Date de fichier du 30 janvier 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)
- [1.0.4.0 : (version 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi)

### <a name="rolling-the-ssnoversion-service-principal"></a>Modification du principal du service [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] utilise des principaux du service créés dans Azure Active Directory comme informations d’identification pour accéder au coffre de clés. Le principal du service a un ID client et une clé d’authentification. Des informations d’identification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sont configurées avec le **nom du coffre**, l’ **ID client** et la **clé d’authentification**. La **clé d’authentification** est valide un certain temps (un ou deux ans). Avant l’expiration de cette période, une nouvelle clé doit être générée pour le principal du service dans Azure AD. Ensuite, les informations d’identification doivent être modifiées dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] gère un cache pour les informations d’identification de la session en cours ; ainsi, si des informations d’identification sont modifiées, [!INCLUDE[ssManStudio](../../../includes/ssmanstudio-md.md)] doit être redémarré.  
  
### <a name="key-backup-and-recovery"></a>Sauvegarde et récupération des clés

Le coffre de clés doit être sauvegardé régulièrement. Si une clé asymétrique dans le coffre est perdue, elle peut être restaurée à partir d’une sauvegarde. La clé doit être restaurée à l’aide du même nom qu’avant, ce que fera la commande PowerShell Restore (voir les étapes ci-dessous).  
Si le coffre a été perdu, vous devez recréer un coffre et restaurer la clé asymétrique dans le coffre en utilisant le même nom qu’avant. Le nom du coffre peut être différent (ou le même qu’avant). Définissez les autorisations d’accès sur le nouveau coffre de manière à accorder au principal du service SQL Server l’accès nécessaire pour les scénarios de chiffrement SQL Server, puis paramétrez les informations d’identification SQL Server afin qu’elles reflètent le nom du nouveau coffre.

Voici un récapitulatif des étapes :  
  
- Sauvegarder la clé de coffre (à l’aide de l’applet de commande PowerShell Backup-AzureKeyVaultKey).  
- En cas de défaillance du coffre, créer un coffre dans la même région géographique. L’utilisateur qui crée le coffre doit se trouver dans le même annuaire par défaut que le programme d’installation du principal du service pour SQL Server.  
- Restaurer la clé dans le nouveau coffre à l’aide de l’applet de commande PowerShell Restore-AzureKeyVaultKey, opération qui restaure la clé en utilisant le même nom qu’avant. S’il existe déjà une clé portant le même nom, la restauration échoue.  
- Accorder des autorisations au principal du service SQL Server pour utiliser ce nouveau coffre.
- Modifier les informations d’identification SQL Server utilisées par le moteur de base de données afin qu’elles reflètent le nom du nouveau coffre (le cas échéant).  
  
Les sauvegardes des clés peuvent être restaurées dans les régions Azure, tant qu’elles restent dans la même région géographique ou le même cloud national : États-Unis, Canada, Japon, Australie, Inde, Asie-Pacifique, Europe, Brésil, Chine, Gouvernement des États-Unis ou Allemagne.  
  
##  <a name="b-frequently-asked-questions"></a><a name="AppendixB"></a> B. Forum Aux Questions (FAQ)

### <a name="on-azure-key-vault"></a>À propos d’Azure Key Vault
  
**Comment fonctionnent les opérations de clé avec Azure Key Vault ?**  
 La clé asymétrique dans le coffre de clés permet de protéger les clés de chiffrement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Seule la partie publique de la clé asymétrique sort du coffre ; la partie privée n’est jamais exportée par le coffre. Toutes les opérations de chiffrement effectuées à l’aide de la clé asymétrique ont lieu dans le service Azure Key Vault et sont protégées par la sécurité de ce service.  
  
 **Qu’est-ce qu’un URI de clé ?**  
 Dans Azure Key Vault, chaque clé a un URI, que vous pouvez utiliser pour référencer la clé dans votre application. Utilisez le format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey` pour obtenir la version actuelle, et le format `https://ContosoKeyVault.vault.azure.net/keys/ContosoFirstKey/cgacf4f763ar42ffb0a1gca546aygd87` pour obtenir une version spécifique.  
  
### <a name="on-configuring-ssnoversion"></a>À propos de la configuration de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  

**Quels sont les points de terminaison auxquels le connecteur SQL Server doit pouvoir accéder ?**
Le connecteur communique avec deux points de terminaison, qui doivent être autorisés. Le seul port requis pour la communication sortante à ces autres services est le port 443 pour Https :

- login.microsoftonline.com/*:443
- *.vault.azure.net/* :443

**Comment se connecter à Azure Key Vault par le biais d’un serveur proxy HTTP(S) ?**
Le connecteur utilise les paramètres de configuration de proxy d’Internet Explorer. Ces paramètres peuvent être contrôlés par le biais d’une [stratégie de groupe](/archive/blogs/askie/how-to-configure-proxy-settings-for-ie10-and-ie11-as-iem-is-not-available) ou du Registre, mais il est important de noter qu’ils ne s’appliquent pas à l’ensemble du système et qu’ils doivent cibler le compte de service exécutant l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Si un administrateur de base de données examine ou modifie les paramètres dans Internet Explorer, seul le compte de l’administrateur de base de données est affecté (le moteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne l’est pas). La connexion interactive au serveur à l’aide du compte de service n’est pas recommandée et est bloquée dans de nombreux environnements sécurisés. Les modifications apportées aux paramètres de proxy configurés peuvent nécessiter le redémarrage de l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour entrer en vigueur. En effet, elles sont mises en cache quand le connecteur tente pour la première fois de se connecter à un coffre de clés.

**Quelles tailles de clé Azure Key Vault le connecteur SQL Server prend-il en charge ?**
La dernière build du connecteur SQL Server prend en charge les tailles de clés Azure Key Vault 2048 et 3072.
  
 > [!NOTE] 
 > La vue « sys.asymmetric_keys » indique la taille de clé 2048 même si la taille de clé 3072 est utilisée. Il s’agit d’une lacune connue de cette vue que l’équipe produit SQL Server tâchera de résoudre dans une version ultérieure.

**Quels sont les niveaux d’autorisation minimaux exigés pour chaque étape de configuration dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]?**  
 Même si vous pouvez effectuer toutes les étapes de configuration comme membre du rôle serveur fixe sysadmin, [!INCLUDE[msCoName](../../../includes/msconame-md.md)] vous encourage à réduire les autorisations que vous utilisez. La liste suivante définit le niveau d’autorisation minimal pour chaque action.  
  
- Pour créer un fournisseur de services de chiffrement, l’autorisation `CONTROL SERVER` ou l’appartenance au rôle de serveur fixe **sysadmin** est exigée.  
  
- Pour modifier une option de configuration et exécuter l’instruction `RECONFIGURE` , vous devez disposer de l’autorisation de niveau serveur `ALTER SETTINGS` . L’autorisation `ALTER SETTINGS` est implicitement détenue par les rôles serveur fixes sysadmin et **serveradmin** .  
  
- Pour créer des informations d’identification, l’autorisation `ALTER ANY CREDENTIAL` est exigée.  
  
- Pour ajouter des informations d’identification à une connexion, l’autorisation `ALTER ANY LOGIN` est exigée.  
  
- Pour créer une clé asymétrique, l’autorisation `CREATE ASYMMETRIC KEY` est exigée.  

**Comment modifier mon Active Directory par défaut, de sorte que mon coffre de clés soit créé dans le même abonnement et Active Directory que le principal du service que j’ai créé pour le connecteur [!INCLUDE[ssNoVersion_md](../../../includes/ssnoversion-md.md)] ?**

![procédure pour changer d’annuaire aad par défaut](../../../relational-databases/security/encryption/media/aad-change-default-directory-helpsteps.png)

1. Accédez au portail Azure Classic : [https://manage.windowsazure.com](https://manage.windowsazure.com)  
2. Dans le menu de gauche, sélectionnez **Paramètres**.
3. Sélectionnez l’abonnement Azure que vous utilisez actuellement, puis cliquez sur **Modifier l’annuaire** à partir des commandes en bas de l’écran.
4. Dans la fenêtre contextuelle, utilisez la liste déroulante **Annuaire** pour sélectionner l’annuaire Active Directory que vous souhaitez utiliser. Celui-ci devient l’annuaire par défaut.
5. Assurez-vous que vous êtes l’administrateur global d’Active Directory qui vient d’être sélectionné. Si vous n’êtes pas l’administrateur général, vous risquez de perdre les autorisations de gestion parce que vous avez changé d’annuaires.
6. Une fois la fenêtre contextuelle fermée, si aucun de vos abonnements ne s’affiche, vous devez mettre à jour le filtre **Filtrer par annuaire** du filtre **Abonnements** du menu supérieur droit de l’écran pour afficher les abonnements à l’aide de l’annuaire Active Directory qui vient d’être mis à jour.

    > [!NOTE] 
    > Il se peut que vous ne disposiez pas des autorisations permettant de modifier réellement l’annuaire par défaut sur votre abonnement Azure. Dans ce cas, créez le principal du service AAD dans votre annuaire par défaut afin qu’il soit dans le même annuaire que le coffre de clés Azure utilisé ultérieurement.

Pour en savoir plus sur Active Directory, lisez [Association des abonnements Azure avec Azure Active Directory](/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory)
  
##  <a name="c-error-code-explanations-for-ssnoversion-connector"></a><a name="AppendixC"></a> C. Explications des codes d’erreur du connecteur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]

**Codes d’erreur du fournisseur :**  
  
Code d'erreur  |Symbole  |Description
---------|---------|---------  
0 | scp_err_Success | L'opération a réussi.
1 | scp_err_Failure | L’opération a échoué.
2 | scp_err_InsufficientBuffer | Cette erreur indique au moteur d’allouer davantage de mémoire pour la mémoire tampon.
3 | scp_err_NotSupported | L'opération n'est pas prise en charge. Par exemple, le type ou l’algorithme de clé spécifié n’est pas pris en charge par le fournisseur EKM.
4 | scp_err_NotFound | Le type ou l’algorithme de clé spécifié n’a pas pu être trouvé par le fournisseur EKM.
5 | scp_err_AuthFailure | L’authentification auprès du fournisseur EKM a échoué.
6 | scp_err_InvalidArgument | L’argument fourni n’est pas valide.
7 | scp_err_ProviderError | Une erreur non spécifiée interceptée par le moteur SQL s’est produite dans le fournisseur EKM.
401 | acquireToken | Le serveur a répondu 401 pour la requête. Vérifiez que l’ID client et le secret client sont corrects et que la chaîne d’informations d’identification est une concaténation de l’ID client et du secret client AAD sans trait d’union.
404 | getKeyByName | Le serveur a répondu par une erreur 404, car le nom de la clé est introuvable. Vérifiez que le nom de la clé existe dans le coffre.
2049 | scp_err_KeyNameDoesNotFitThumbprint | Le nom de la clé est trop long pour tenir dans l’empreinte numérique du moteur SQL. Le nom de la clé ne doit pas dépasser 26 caractères.
2050 | scp_err_PasswordTooShort | La chaîne secrète, qui est la concaténation de l’ID du client AAD et de la clé secrète, comporte moins de 32 caractères.
2051 | scp_err_OutOfMemory | Le moteur SQL ne dispose pas de suffisamment de mémoire et n’a pas pu allouer de mémoire pour le fournisseur EKM.
2052 | scp_err_ConvertKeyNameToThumbprint | Impossible de convertir le nom de la clé en empreinte numérique.
2053 | scp_err_ConvertThumbprintToKeyName|  Impossible de convertir l’empreinte numérique en nom de clé.
2058 | scp_err_FailureInRegistry|  Impossible d’effectuer l’opération dans le Registre. Le compte de service SQL Server n’a pas l’autorisation de créer la clé de Registre.
3000 | ErrorSuccess | L’opération AKV a réussi.
3001 | ErrorUnknown | L’opération AKV a échoué avec une erreur non spécifiée.
3002 | ErrorHttpCreateHttpClientOutOfMemory | Impossible de créer une opération HttpClient pour AKV en raison d’une insuffisance de mémoire.
3003 | ErrorHttpOpenSession | Impossible d’ouvrir une session http en raison d’une erreur réseau.
3004 | ErrorHttpConnectSession | Impossible de se connecter à une session http en raison d’une erreur réseau.
3005 | ErrorHttpAttemptConnect | Tentative de connexion impossible en raison d’une erreur réseau.
3006 | ErrorHttpOpenRequest | Impossible d’ouvrir une requête en raison d’une erreur réseau.
3007 | ErrorHttpAddRequestHeader | Impossible d’ajouter un en-tête de requête.
3008 | ErrorHttpSendRequest | Impossible d’envoyer une requête en raison d’une erreur réseau.
3009 | ErrorHttpGetResponseCode | Impossible d’obtenir un code de réponse en raison d’une erreur réseau.
3010 | ErrorHttpResponseCodeUnauthorized | Le serveur a répondu 401 pour la requête.
3011 | ErrorHttpResponseCodeThrottled | Le serveur a limité la requête.
3012 | ErrorHttpResponseCodeClientError | La demande envoyée à partir du connecteur n’est pas valide. Cela signifie généralement le nom de clé n’est pas valide ou contient des caractères non valides.
3013 | ErrorHttpResponseCodeServerError | Le serveur a renvoyé un code de réponse compris entre 500 et 600.
3014 | ErrorHttpQueryHeader | Impossible de rechercher l’en-tête de réponse.
3015 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader | Impossible de copier l’en-tête de réponse en raison d’une insuffisance de mémoire.
3016 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer | Impossible d’interroger l’en-tête de réponse en raison d’une insuffisance de mémoire lors de la réallocation d’une mémoire tampon.
3017 | ErrorHttpQueryHeaderNotFound | L’en-tête de requête est introuvable dans la réponse.
3018 | ErrorHttpQueryHeaderUpdateBufferLength | Impossible de mettre à jour la longueur de la mémoire tampon lors de l’interrogation de l’en-tête de réponse.
3019 | ErrorHttpReadData | Impossible de lire les données de la réponse en raison d’une erreur réseau.
3076 | ErrorHttpResourceNotFound | Le serveur a répondu par une erreur 404, car le nom de la clé est introuvable. Vérifiez que le nom de la clé existe dans le coffre.
3077 | ErrorHttpOperationForbidden | Le serveur a répondu par une erreur 403, car l’utilisateur n’a pas l’autorisation appropriée pour effectuer l’action. Vérifiez que vous disposez de l’autorisation pour l’opération spécifiée. Au minimum, le connecteur exige des autorisations « get, list, wrapKey, unwrapKey » pour fonctionner correctement.
3100 | ErrorHttpCreateHttpClientOutOfMemory               | Impossible de créer une opération HttpClient pour AKV en raison d’une insuffisance de mémoire.
3101 | ErrorHttpOpenSession                               | Impossible d’ouvrir une session HTTP en raison d’une erreur réseau.
3102 | ErrorHttpConnectSession                            | Impossible de connecter une session HTTP en raison d’une erreur réseau.
3103 | ErrorHttpAttemptConnect                            | Tentative de connexion impossible en raison d’une erreur réseau.
3104 | ErrorHttpOpenRequest                               | Impossible d’ouvrir une requête en raison d’une erreur réseau.
3105 | ErrorHttpAddRequestHeader                          | Impossible d’ajouter un en-tête de requête.
3106 | ErrorHttpSendRequest                               | Impossible d’envoyer une requête en raison d’une erreur réseau.
3107 | ErrorHttpGetResponseCode                           | Impossible d’obtenir un code de réponse en raison d’une erreur réseau.
3108 | ErrorHttpResponseCodeUnauthorized                  | Le serveur a répondu 401 pour la requête. Vérifiez que l’ID client et le secret client sont corrects et que la chaîne d’informations d’identification est une concaténation de l’ID client et du secret client AAD sans trait d’union.
3109 | ErrorHttpResponseCodeThrottled                     | Le serveur a limité la requête.
3110 | ErrorHttpResponseCodeClientError                    | La requête n’est pas valide. Cela signifie généralement le nom de clé n’est pas valide ou contient des caractères non valides.
3111 | ErrorHttpResponseCodeServerError                   | Le serveur a renvoyé un code de réponse compris entre 500 et 600.
3112 | ErrorHttpResourceNotFound                          | Le serveur a répondu par une erreur 404, car le nom de la clé est introuvable. Vérifiez que le nom de la clé existe dans le coffre.
3113 | ErrorHttpOperationForbidden                         | Le serveur a répondu 403, car l’utilisateur n’a pas l’autorisation appropriée pour effectuer l’action. Vérifiez que vous disposez des autorisations pour l’opération spécifiée. Au minimum, les autorisations « Get, WrapKey, UnwrapKey » sont requises.
3114 | ErrorHttpQueryHeader                               | Impossible de rechercher l’en-tête de réponse.
3115 | ErrorHttpQueryHeaderOutOfMemoryCopyHeader          | Impossible de copier l’en-tête de réponse en raison d’une insuffisance de mémoire.
3116 | ErrorHttpQueryHeaderOutOfMemoryReallocBuffer       | Impossible d’interroger l’en-tête de réponse en raison d’une insuffisance de mémoire lors de la réallocation d’une mémoire tampon.
3117 | ErrorHttpQueryHeaderNotFound                       | L’en-tête de requête est introuvable dans la réponse.
3118 | ErrorHttpQueryHeaderUpdateBufferLength             | Impossible de mettre à jour la longueur de la mémoire tampon lors de l’interrogation de l’en-tête de réponse.
3119 | ErrorHttpReadData                                  | Impossible de lire les données de la réponse en raison d’une erreur réseau.
3120 | ErrorHttpGetResponseOutOfMemoryCreateTempBuffer    | Impossible d’obtenir le corps de la réponse en raison d’une mémoire insuffisante lors de la création d’une mémoire tampon temporaire.
3121 | ErrorHttpGetResponseOutOfMemoryGetResultString     | Impossible d’obtenir le corps de la réponse en raison d’une mémoire insuffisante lors de l’obtention de la chaîne de résultats.
3122 | ErrorHttpGetResponseOutOfMemoryAppendResponse      | Impossible d’obtenir le corps de la réponse en raison d’une mémoire insuffisante lors de l’ajout de la réponse.
3200 | ErrorGetAADValuesOutOfMemoryConcatPath | Impossible d’obtenir les valeurs d’en-tête de stimulation Azure Active Directory en raison d’une mémoire insuffisante lors de la concaténation du chemin.
3201 | ErrorGetAADDomainUrlStartPosition | Impossible de trouver la position de départ de l’URL du domaine Azure Active Directory dans l’en-tête de stimulation de la réponse mal formée.
3202 | ErrorGetAADDomainUrlStopPosition | Impossible de trouver la position de fin de l’URL du domaine Azure Active Directory dans l’en-tête de stimulation de la réponse mal formée.
3203 | ErrorGetAADDomainUrlMalformatted | L’en-tête de stimulation de la réponse Azure Active Directory est mal formée et ne contient pas l’URL de domaine AAD.
3204 | ErrorGetAADDomainUrlOutOfMemoryAlloc | Mémoire insuffisante lors de l’allocation de la mémoire tampon pour l’URL de domaine Azure Active Directory.
3205 | ErrorGetAADTenantIdOutOfMemoryAlloc | Mémoire insuffisante lors de l’allocation de la mémoire tampon pour l’ID d’abonné Azure Active Directory.
3206 | ErrorGetAKVResourceUrlStartPosition | Impossible de trouver la position de départ de l’URL de ressource Azure Active Directory dans l’en-tête de stimulation de la réponse mal formée.
3207 | ErrorGetAKVResourceUrlStopPosition | Impossible de trouver la position de fin de l’URL de ressource Azure Active Directory dans l’en-tête de stimulation de la réponse mal formée.
3208 | ErrorGetAKVResourceUrlOutOfMemoryAlloc | Mémoire insuffisante lors de l’allocation de la mémoire tampon pour l’URL de ressource Azure Key Vault.
3300 | ErrorGetTokenOutOfMemoryConcatPath | Impossible d’obtenir le jeton en raison d’une mémoire insuffisante lors de la concaténation du chemin d’accès de la requête.
3301 | ErrorGetTokenOutOfMemoryConcatBody | Impossible d’obtenir le jeton en raison d’une mémoire insuffisante lors de la concaténation du corps de la réponse.
3302 | ErrorGetTokenOutOfMemoryConvertResponseString | Impossible d’obtenir le jeton en raison d’une mémoire insuffisante lors de la conversion de la chaîne de réponses.
3303 | ErrorGetTokenBadCredentials | Impossible d’obtenir un jeton en raison d’informations d’identification incorrectes. Vérifiez que la chaîne d’informations d’identification ou le certificat est valide.
3304 | ErrorGetTokenFailedToGetToken | Alors que les informations d’identification sont correctes, l’opération n’a toujours pas réussi à obtenir un jeton valide.
3305 | ErrorGetTokenRejected | Le jeton est valide, mais il est rejeté par le serveur.
3306 | ErrorGetTokenNotFound | Le jeton est introuvable dans la réponse.
3307 | ErrorGetTokenJsonParser | Impossible d’analyser la réponse JSON du serveur.
3308 | ErrorGetTokenExtractToken | Impossible d’extraire le jeton de la réponse JSON.
3400 | ErrorGetKeyByNameOutOfMemoryConvertResponseString | Impossible d’obtenir la clé par son nom en raison d’une mémoire insuffisante lors de la conversion de la chaîne de réponses.
3401 | ErrorGetKeyByNameOutOfMemoryConcatPath | Impossible d’obtenir la clé par son nom en raison d’une mémoire insuffisante lors de la concaténation du chemin d’accès.
3402 | ErrorGetKeyByNameOutOfMemoryConcatHeader | Impossible d’obtenir la clé par son nom en raison d’une mémoire insuffisante lors de la concaténation de l’en-tête.
3403 | ErrorGetKeyByNameNoResponse | Impossible d’obtenir la clé par son nom en raison de l’absence de réponse du serveur.
3404 | ErrorGetKeyByNameJsonParser | Impossible d’obtenir la clé par son nom en raison de l’échec de l’analyse de la réponse JSON.
3405 | ErrorGetKeyByNameExtractKeyNode | Impossible d’obtenir la clé par son nom en raison de l’échec de l’extraction du nœud de clé à partir de la réponse.
3406 | ErrorGetKeyByNameExtractKeyId | Impossible d’obtenir la clé par son nom en raison de l’échec de l’extraction de l’ID de clé à partir de la réponse.
3407 | ErrorGetKeyByNameExtractKeyType | Impossible d’obtenir la clé par son nom en raison de l’échec de l’extraction du type de clé à partir de la réponse.
3408 | ErrorGetKeyByNameExtractKeyN | Impossible d’obtenir la clé par son nom en raison de l’échec de l’extraction de la clé N à partir de la réponse.
3409 | ErrorGetKeyByNameBase64DecodeN | Impossible d’obtenir la clé par son nom en raison de l’échec du décodage en Base64 de N.
3410 | ErrorGetKeyByNameExtractKeyE | Impossible d’obtenir la clé par son nom en raison de l’échec de l’extraction de la clé E à partir de la réponse.
3411 | ErrorGetKeyByNameBase64DecodeE | Impossible d’obtenir la clé par son nom en raison de l’échec du décodage en Base64 de E.
3412 | ErrorGetKeyByNameExtractKeyUri | Impossible d’extraire l’URI de clé de la réponse.
3 500 | ErrorBackupKeyOutOfMemoryConvertResponseString | Impossible de sauvegarder la clé en raison d’une mémoire insuffisante lors de la conversion de la chaîne de réponses.
3501 | ErrorBackupKeyOutOfMemoryConcatPath | Impossible de sauvegarder la clé en raison d’une mémoire insuffisante lors de la concaténation du chemin d’accès.
3502 | ErrorBackupKeyOutOfMemoryConcatHeader | Impossible de sauvegarder la clé en raison d’une mémoire insuffisante lors de la concaténation de l’en-tête de demande.
3503 | ErrorBackupKeyNoResponse | Impossible de sauvegarder la clé en raison de l’absence de réponse du serveur.
3504 | ErrorBackupKeyJsonParser | Impossible de sauvegarder la clé en raison de l’échec de l’analyse de la réponse JSON.
3505 | ErrorBackupKeyExtractValue | Impossible de sauvegarder la clé en raison de l’échec de l’extraction de la valeur de la réponse JSON.
3506 | ErrorBackupKeyBase64DecodeValue | Impossible de sauvegarder la clé en raison de l’échec du décodage en Base64 du champ de valeur.
3600 | ErrorWrapKeyOutOfMemoryConvertResponseString | Impossible d’envelopper la clé en raison d’une mémoire insuffisante lors de la conversion de la chaîne de réponses.
3601 | ErrorWrapKeyOutOfMemoryConcatPath | Impossible d’envelopper la clé en raison d’une mémoire insuffisante lors de la concaténation du chemin d’accès.
3602 | ErrorWrapKeyOutOfMemoryConcatHeader | Impossible d’envelopper la clé en raison d’une mémoire insuffisante lors de la concaténation de l’en-tête.
3603 | ErrorWrapKeyOutOfMemoryConcatBody | Impossible d’envelopper la clé en raison d’une mémoire insuffisante lors de la concaténation du corps.
3604 | ErrorWrapKeyOutOfMemoryConvertEncodedBody | Impossible d’envelopper la clé en raison d’une mémoire insuffisante lors de la conversion du corps encodé.
3605 | ErrorWrapKeyBase64EncodeKey | Impossible d’envelopper la clé en raison de l’échec d’encodage en Base64 de la clé.
3606 | ErrorWrapKeyBase64DecodeValue | Impossible d’envelopper la clé en raison de l’échec du décodage en Base64 de la valeur de la réponse.
3607 | ErrorWrapKeyJsonParser | Impossible d’envelopper la clé en raison de l’échec de l’analyse de la réponse JSON.
3608 | ErrorWrapKeyExtractValue | Impossible d’encapsuler la clé en raison de l’échec d’extraction de la valeur de la réponse.
3609 | ErrorWrapKeyNoResponse | Impossible d’envelopper la clé en raison de l’absence de réponse du serveur.
3700 | ErrorUnwrapKeyOutOfMemoryConvertResponseString | Impossible de désenvelopper la clé en raison d’une mémoire insuffisante lors de la conversion de la chaîne de réponses.
3701 | ErrorUnwrapKeyOutOfMemoryConcatPath | Impossible de désenvelopper la clé en raison d’une mémoire insuffisante lors de la concaténation du chemin d’accès.
3702 | ErrorUnwrapKeyOutOfMemoryConcatHeader | Impossible de désenvelopper la clé en raison d’une mémoire insuffisante lors de la concaténation de l’en-tête.
3703 | ErrorUnwrapKeyOutOfMemoryConcatBody | Impossible de désenvelopper la clé en raison d’une mémoire insuffisante lors de la concaténation du corps.
3704 | ErrorUnwrapKeyOutOfMemoryConvertEncodedBody | Impossible de désenvelopper la clé en raison d’une mémoire insuffisante lors de la conversion du corps encodé.
3705 | ErrorUnwrapKeyBase64EncodeKey | Impossible de désenvelopper la clé en raison de l’échec d’encodage en Base64 de la clé.
3706 | ErrorUnwrapKeyBase64DecodeValue | Impossible de désenvelopper la clé en raison de l’échec du décodage en Base64 de la valeur de la réponse.
3707 | ErrorUnwrapKeyJsonParser | Impossible de désenvelopper la clé en raison de l’échec de l’extraction de la valeur de la réponse.
3708 | ErrorUnwrapKeyExtractValue | Impossible de désenvelopper la clé en raison de l’échec de l’extraction de la valeur de la réponse.
3709 | ErrorUnwrapKeyNoResponse | Impossible de désenvelopper la clé en raison de l’absence de réponse du serveur.
3 800 | ErrorSecretAuthParamsGetRequestBody | Erreur lors de la création du corps de la demande à l’aide de l’ID client et du secret AAD.
3801 | ErrorJWTTokenCreateHeader | Erreur lors de la création de l’en-tête de jeton JWT pour l’authentification avec AAD.
3802 | ErrorJWTTokenCreatePayloadGUID | Erreur lors de la création du GUID pour la charge utile du jeton JWT pour l’authentification avec AAD.
3803 | ErrorJWTTokenCreatePayload | Erreur lors de la création de la charge utile du jeton JWT pour l’authentification avec AAD.
3804 | ErrorJWTTokenCreateSignature | Erreur lors de la création de la signature du jeton JWT pour l’authentification avec AAD.
3805 | ErrorJWTTokenSignatureHashAlg | Erreur lors de l’obtention de l’algorithme de hachage SHA256 pour l’authentification avec AAD.
3806 | ErrorJWTTokenSignatureHash | Erreur lors de la création du code de hachage SHA256 pour l’authentification du jeton JWT avec AAD.
3807 | ErrorJWTTokenSignatureSignHash | Erreur lors de la signature du hachage de jeton JTW pour l’authentification avec AAD.
3808 | ErrorJWTTokenCreateToken | Erreur lors de la création du jeton JWT pour l’authentification avec AAD.
3809 | ErrorPfxCertAuthParamsImportPfx | Erreur lors de l’importation du certificat pfx pour l’authentification avec AAD.
3810 | ErrorPfxCertAuthParamsGetThumbprint | Erreur lors de l’obtention de l’empreinte du certificat Pfx pour l’authentification avec AAD.
3811 | ErrorPfxCertAuthParamsGetPrivateKey | Erreur lors de l’obtention de la clé privée du certificat Pfx pour l’authentification avec AAD.
3812 | ErrorPfxCertAuthParamsSignAlg | Erreur lors de l’obtention de l’algorithme de signature RSA pour l’authentification par certificat Pfx avec AAD.
3813 | ErrorPfxCertAuthParamsImportForSign | Erreur lors de l’importation de la clé privée Pfx pour la signature RSA pour l’authentification avec AAD.
3814 | ErrorPfxCertAuthParamsCreateRequestBody | Erreur lors de la création du corps de la demande du certificat Pfx pour l’authentification avec AAD.
3815 | ErrorPEMCertAuthParamsGetThumbprint | Erreur de décodage en Base64 de l’empreinte pour l’authentification avec AAD.
3816 | ErrorPEMCertAuthParamsGetPrivateKey | Erreur lors de l’obtention de la clé privée RSA du PEM pour l’authentification avec AAD.
3817 | ErrorPEMCertAuthParamsSignAlg | Erreur lors de l’obtention de l’algorithme de signature RSA pour l’authentification par clé privée PEM avec AAD.
3818 | ErrorPEMCertAuthParamsImportForSign | Erreur lors de l’importation de la clé privée PEM pour la signature RSA pour l’authentification avec AAD.
3819 | ErrorPEMCertAuthParamsCreateRequestBody | Erreur lors de la création du corps de la demande de la clé privée PEM pour l’authentification avec AAD.
3820 | ErrorLegacyPrivateKeyAuthParamsSignAlg | Erreur lors de l’obtention de l’algorithme de signature RSA pour l’authentification par clé privée héritée avec AAD.
3821 | ErrorLegacyPrivateKeyAuthParamsImportForSign | Erreur lors de l’importation de la clé privée héritée pour la signature RSA pour l’authentification avec AAD.
3822 | ErrorLegacyPrivateKeyAuthParamsCreateRequestBody        | Erreur lors de la création du corps de la demande de la clé privée héritée pour l’authentification avec AAD.
3900 | ErrorAKVDoesNotExist | Erreur du nom Internet non résolu. Cela indique généralement que Azure Key Vault est supprimée.
4000 | ErrorCreateKeyVaultRetryManagerOutOfMemory | Impossible de créer une opération RetryManager pour AKV en raison d’une insuffisance de mémoire.

Si votre code d’erreur ne figure pas dans ce tableau, voici d’autres raisons pouvant expliquer pourquoi l’erreur se produit :
  
- Vous ne disposez peut-être pas d’un accès Internet et vous ne pouvez pas accéder à votre service Azure Key Vault. Vérifiez votre connexion Internet.  
  
- Le service Azure Key Vault n’est peut-être pas disponible. Réessayez ultérieurement.  
  
- Vous avez peut-être supprimé la clé asymétrique d’Azure Key Vault ou de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Restaurez la clé.  
  
- Si vous obtenez l’erreur « Impossible de charger la bibliothèque », vérifiez que la version de Visual Studio C++ Redistribuable dont vous disposez convient à la version de SQL Server que vous exécutez. Le tableau ci-dessous indique la version à installer à partir du Centre de téléchargement Microsoft.

Le journal des événements Windows consigne également les erreurs associées au connecteur SQL Server, ce qui peut vous donner des informations supplémentaires sur la cause de l’erreur. La source dans le journal des événements de l’application Windows sera « Connecteur SQL Server pour Microsoft Azure Key Vault ».
  
Version de SQL Server  |Lien d’installation du package redistribuable
---------|---------
2008, 2008 R2, 2012, 2014 | [Packages redistribuables Visual C++ pour Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=40784)
2016 | [Package redistribuable Visual C++ pour Visual Studio 2015](https://www.microsoft.com/download/details.aspx?id=48145)
  
## <a name="additional-references"></a>Références supplémentaires

 Informations supplémentaires sur la gestion de clés extensible :  
  
- [Gestion de clés extensible &#40;EKM&#41;](../../../relational-databases/security/encryption/extensible-key-management-ekm.md)  
  
 Chiffrements SQL prenant en charge la gestion de clés extensible :  
  
- [Activer le chiffrement transparent des données à l’aide de la gestion de clés extensible (EKM)](../../../relational-databases/security/encryption/enable-tde-on-sql-server-using-ekm.md)  
  
- [Chiffrement de sauvegarde](../../../relational-databases/backup-restore/backup-encryption.md)  
  
- [Créer une sauvegarde chiffrée](../../../relational-databases/backup-restore/create-an-encrypted-backup.md)  
  
 Commandes [!INCLUDE[tsql](../../../includes/tsql-md.md)] associées :  
  
- [sp_configure &#40;Transact-SQL&#41;](../../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
- [CREATE CRYPTOGRAPHIC PROVIDER &#40;Transact-SQL&#41;](../../../t-sql/statements/create-cryptographic-provider-transact-sql.md)  
  
- [CREATE CREDENTIAL &#40;Transact-SQL&#41;](../../../t-sql/statements/create-credential-transact-sql.md)  
  
- [CREATE ASYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-asymmetric-key-transact-sql.md)  
  
- [CREATE SYMMETRIC KEY &#40;Transact-SQL&#41;](../../../t-sql/statements/create-symmetric-key-transact-sql.md)  
  
- [CREATE LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/create-login-transact-sql.md)  
  
- [ALTER LOGIN &#40;Transact-SQL&#41;](../../../t-sql/statements/alter-login-transact-sql.md)  
  
 Documentation relative à Azure Key Vault  
  
- [Qu’est-ce qu’Azure Key Vault ?](/azure/key-vault/general/basic-concepts)  
  
- [Prise en main d'Azure Key Vault](/azure/key-vault/general/overview)  
  
- Informations de référence sur les [applets de commande Azure Key Vault](/powershell/module/azurerm.keyvault/) de PowerShell  
  
## <a name="see-also"></a>Voir aussi

 [Gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/extensible-key-management-using-azure-key-vault-sql-server.md)  
 [Utiliser le connecteur SQL Server avec les fonctionnalités de chiffrement SQL](../../../relational-databases/security/encryption/use-sql-server-connector-with-sql-encryption-features.md)  
 [Fournisseur EKM activé (option de configuration de serveur)](../../../database-engine/configure-windows/ekm-provider-enabled-server-configuration-option.md)  
 [Étapes de la configuration de la gestion de clés extensible à l’aide d’Azure Key Vault](../../../relational-databases/security/encryption/setup-steps-for-extensible-key-management-using-the-azure-key-vault.md)

Pour obtenir des exemples de scripts supplémentaires, consultez le blog sur [Chiffrement des données transparentes SQL Server et gestion de clés extensible avec Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549)
