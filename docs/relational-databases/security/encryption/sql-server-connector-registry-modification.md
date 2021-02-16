---
title: Journalisation des erreurs et des informations du connecteur SQL Server
description: Cet article explique comment activer les erreurs et la journalisation pour le connecteur SQL Server en modifiant les entrées du Registre
ms.date: 10/08/2020
ms.localizationpriority: medium
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: how-to
author: rupp29
ms.author: arupp
ms.openlocfilehash: 3a674994d7028a8f4455c26d6f705b9da7b5ee08
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018682"
---
# <a name="sql-server-connector-error-and-information-logging"></a>Journalisation des erreurs et des informations du connecteur SQL Server

Cet article explique comment modifier les entrées du Registre pour activer la journalisation des erreurs et des informations du connecteur SQL Server.

## <a name="sql-server-connector-for-microsoft-azure-key-vault"></a>Connecteur SQL Server pour Microsoft Azure Key Vault

Le [connecteur SQL Server pour Microsoft Azure Key Vault](https://www.microsoft.com/download/details.aspx?id=45344) permet au chiffrement SQL Server d’utiliser Microsoft Azure Key Vault en tant que fournisseur de gestion de clés extensible (EKM) pour protéger ses clés de chiffrement.

Le [téléchargement](https://www.microsoft.com/download/details.aspx?id=45344) comprend le connecteur SQL Server ainsi que des exemples de scripts permettant à un administrateur SQL Server d’apprendre à configurer le connecteur SQL Server et à activer les scénarios de chiffrement SQL Server. Pour plus d’informations, consultez [Gestion de clés extensible à l’aide de Key Vault (SQL Server)](./extensible-key-management-using-azure-key-vault-sql-server.md).

Utilisez le [forum Azure Key Vault](https://social.msdn.microsoft.com/Forums/AzureKeyVault) pour poser des questions, partager des insights et discuter du connecteur SQL Server.

> [!NOTE]
> Pendant l’exécution normale, la DLL du connecteur SQL Server crée dynamiquement des entrées de Registre pour établir la connectivité à Azure Key Vault, afin de créer la clé `[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\SQL Server Cryptographic Provider]`. Le compte de démarrage de SQL Server doit être administrateur local ou son compte de service doit être **NT SERVICE\MSSQLSERVER**.

## <a name="upgrade-sql-server-connector-to-the-latest-version"></a>Mettre à niveau le connecteur SQL Server vers la dernière version

Pour mettre à niveau le connecteur SQL Server (version : 1.0.5.0 avec la date de septembre 2020) vers la version la plus récente du fichier DLL du fournisseur de chiffrement.

### <a name="upgrade"></a>Mettre à niveau

1. Arrêter le service SQL Server à l’aide du Gestionnaire de configuration SQL Server
1. Désinstaller l’ancienne version à l’aide de **Panneau de configuration\Programmes\Programmes et fonctionnalités**
    1. Nom de l’application : Connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.300.96
    1. Date du fichier DLL : 30/01/2018 15:00
1. Installer (mettre à niveau) le nouveau connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.2000.367
    1. Date du fichier DLL : 11/09/2020 ‏‎5:17
1. Démarrer le service SQL Server
1. Vérifier si la ou les bases de données chiffrées sont accessibles

### <a name="rollback"></a>Restauration

1. Arrêter le service SQL Server à l’aide du Gestionnaire de configuration SQL Server
1. Désinstaller la nouvelle version à l’aide de **Panneau de configuration\Programmes\Programmes et fonctionnalités**
    1. Nom de l’application : Connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.2000.367
    1. Date du fichier DLL : 11/09/2020 ‏‎5:17
1. Installer l’ancienne version du connecteur SQL Server pour Microsoft Azure Key Vault
    1. Version : 15.0.300.96
    1. Date du fichier DLL : 30/01/2018 15:00
1. Démarrer le service SQL Server
1. Vérifier si la ou les bases de données chiffrées sont accessibles

> [!NOTE]
> - Les versions 1.0.0.440 et antérieures du connecteur SQL Server ont été remplacées et ne sont plus prises en charge dans les environnements de production. Pour plus d’informations sur la résolution des problèmes du connecteur SQL Server, consultez [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md).
> - À partir de la version 1.0.3.0, le connecteur SQL Server signale les messages d’erreur pertinents dans les journaux des événements Windows à des fins de résolution des problèmes.
> - À partir de la version [1.0.4.0 (version 13.0.811.168)](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/SQLServerConnectorforMicrosoftAzureKeyVault.msi), il existe une prise en charge des clouds privés Azure, y compris Azure Chine, Azure Allemagne et Azure Government.
> - Un changement cassant figure dans la version 1.0.5.0, lié à l’algorithme d’empreinte. Vous pouvez rencontrer un échec de restauration des bases de données après la mise à niveau vers la version 1.0.5.0. Pour plus d’informations, consultez [l’article 447099 de la Base de connaissances](https://support.microsoft.com/help/4470999/db-backup-problems-to-sql-server-connector-for-azure-1-0-5-0).
> - **À partir de la version 1.0.5.0 (date de fichier de septembre 2020), le connecteur SQL Server prend en charge le filtrage des messages et la logique de nouvelle tentative de requête réseau.**
> - *L’ancienne version du connecteur SQL Server est également la version : [1.0.5.0 (version 15.0.300.96) – Date de fichier de janvier 2018](https://download.microsoft.com/download/8/0/9/809494F2-BAC9-4388-AD07-7EAF9745D77B/ENU/SQLServerConnectorforMicrosoftAzureKeyVault.msi)* . Effectuez une mise à niveau vers le dernier connecteur SQL Server si vous rencontrez des problèmes.

**Configuration requise** - Versions SQL Server prises en charge :

- SQL Server 2019 RTM Enterprise 64 bits
- SQL Server 2017 RTM Enterprise 64 bits
- SQL Server 2016 RTM Enterprise 64 bits
- SQL Server 2014 RTM Enterprise 64 bits
- SQL Server 2012 SP2 Enterprise 64 bits
- SQL Server 2012 SP1 CU6 Enterprise 64 bits
- SQL Server 2008 R2 SP2 CU8 Enterprise 64 bits

Dans les versions de SQL Server 2008 et 2012 antérieures à celles indiquées ci-dessus, le correctif spécifié dans l’article de la Base de connaissances suivant doit être installé : [https://support2.microsoft.com/kb/2859713](https://support2.microsoft.com/kb/2859713).

Le connecteur SQL Server pour Microsoft Azure Key Vault nécessite également .NET version 4.5.1 sur la machine virtuelle Microsoft SQL Server sur Azure. Cette bibliothèque doit être installée avant que vous n’installiez le connecteur SQL Server.

La version de Visual Studio C++ Redistribuable dont vous disposez doit convenir à la version de SQL Server que vous exécutez :

- Pour les versions SQL Server 2008, 2008 R2, 2012 et 2014, installez le package Visual C++ Redistributable 2013.

- Pour SQL Server 2016, installez le package Visual C++ Redistributable 2015.

## <a name="modify-windows-registry-steps"></a>Étapes de la modification du Registre Windows

Modifiez les entrées du Registre pour activer la journalisation des erreurs et des événements du connecteur SQL Server dans le journal des événements des applications Windows.

> [!CAUTION]
> Suivez les étapes de cette section attentivement et à vos propres risques. En effet, toute erreur de modification peut être à l’origine de problèmes graves. Avant de modifier le Registre, [sauvegardez-le en vue de sa restauration](https://support.microsoft.com/help/322756) en cas de problème.

1. Il existe deux façons d’ouvrir l’Éditeur du Registre dans Windows 10 :
    - Dans la zone de recherche dans la barre des tâches, tapez **regedit**. Ensuite, sélectionnez le premier résultat correspondant à Éditeur du Registre (Application).

    ![Ouverture de regedit pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-open.png "Ouverture de regedit pour EKM")
    - Appuyez longuement ou cliquez avec le bouton droit sur le bouton Démarrer, puis sélectionnez Exécuter. Entrez **regedit** dans la boîte de dialogue, puis sélectionnez **OK**.

   ![Démarrage de regedit pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-start.png "Démarrage de regedit pour EKM")

1. Accédez à la clé de Registre suivante :

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\**

    ![Accès à la clé AKV pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv.png "Accès à la clé AKV pour EKM")  

1. Ajoutez une nouvelle clé sous **Azure Key Vault** nommée `Log` :

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![Ajout de la clé Log d’AKV dans regedit pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log.png "Ajout de la clé Log d’AKV dans regedit pour EKM")  

1. Sous la clé **Log**, ajoutez une valeur DWORD (32 bits) nommée `Level` :

    **HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\\Log\\**

    ![Ajout de la valeur dword à la clé Log d’AKV dans regedit pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-dword.png "Ajout de la valeur dword à la clé Log d’AKV dans regedit pour EKM")  

1. Définissez la valeur de DWORD sur un niveau de journalisation approprié (0, 1, 2) :
   1. 0 (information) - **Par défaut**
   1. 1 (erreur)
   1. 2 (aucun journal)

   ![Définition du niveau de journalisation associé à la clé d’AKV dans regedit pour EKM](../../../relational-databases/security/encryption/media/ekm-registry/ekm-regedit-akv-log-level.png "Définition du niveau de journalisation associé à la clé d’AKV dans regedit pour EKM")  

Les entrées de Registre décrites dans cet article se trouvent sous la clé suivante :

```console
\Computer
    \HKEY_LOCAL_MACHINE
       \SOFTWARE
          \Microsoft
             \SQL Server Cryptographic Provider
                \Azure Key Vault
                   \Log\
                      <Level>
```

Si vous le souhaitez, vous pouvez utiliser la ligne de commande pour générer la clé :

```cmd
--Create the logging parameter using (Administrator) Command Line:
REG ADD "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level /t REG_DWORD /d 1 

--Validate the new registry entry
REG QUERY "HKLM\SOFTWARE\Microsoft\SQL Server Cryptographic Provider\Azure Key Vault\Log" /v Level
```

Les entrées du journal des événements d’application qui ne contiennent pas de messages peuvent également être corrigées à l’aide d’une entrée de Registre. Le journal des événements peut contenir le message `The description for Event ID 0 from source SQL Server Connector for Microsoft Azure Key Vault cannot be found...`.  

```cmd
--Create the registry entry to enable missing messages (this works with any version)
REG ADD "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile /t REG_EXPAND_SZ /d "C:\Program Files\SQL Server Connector for Microsoft Azure Key Vault\Microsoft.AzureKeyVaultService.EKM.dll"

--Validate the new registry entry
REG QUERY "HKLM\SYSTEM\ControlSet001\Services\EventLog\Application\SQL Server Connector for Microsoft Azure Key Vault" /v EventMessageFile
```

## <a name="related-articles"></a>Articles connexes

- Pour obtenir des exemples de scripts supplémentaires, consultez le blog sur [Chiffrement des données transparentes SQL Server et gestion de clés extensible avec Azure Key Vault](https://techcommunity.microsoft.com/t5/sql-server/intro-sql-server-transparent-data-encryption-and-extensible-key/ba-p/1427549)
- [Gestion de clés extensible (EKM)](extensible-key-management-ekm.md)  
- [Gestion de clés extensible à l’aide d’Azure Key Vault](extensible-key-management-using-azure-key-vault-sql-server.md)
- [Résolution des problèmes et maintenance du connecteur SQL Server](../../../relational-databases/security/encryption/sql-server-connector-maintenance-troubleshooting.md)