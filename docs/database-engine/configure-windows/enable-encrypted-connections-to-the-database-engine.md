---
title: Activation des connexions chiffrées | Microsoft Docs
ms.custom: contperf-fy20q4
ms.date: 08/29/2019
ms.prod: sql
ms.prod_service: security
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
description: Chiffrez les données entre les canaux de communication. Activez les connexions chiffrées pour une instance du moteur de base de données SQL Server à l’aide du Gestionnaire de configuration SQL Server.
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- TLS [SQL Server]
- Transport Layer Security (TLS)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
- TLS certificates
ms.assetid: e1e55519-97ec-4404-81ef-881da3b42006
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 1b5726aad103012b0ed7619749c1f6f669baa234
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98241831"
---
# <a name="enable-encrypted-connections-to-the-database-engine"></a>Activez les connexions chiffrées dans le moteur de base de données

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Découvrez comment chiffrer les données entre les canaux de communication.  Vous activez les connexions chiffrées d'une instance de [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] et utilisez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Configuration Manager pour spécifier un certificat.
 
 L’ordinateur serveur doit disposer d’un certificat alloué. Pour allouer le certificat sur l’ordinateur serveur, vous [l’importez dans Windows](#single-server). L’ordinateur client doit être configuré pour [approuver l’autorité racine du certificat](#about).  
  
> [!IMPORTANT]
> À compter de [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)], le protocole SSL (Secure Sockets Layer) n’est plus disponible. Utilisez TLS (Transport Layer Security) à la place.

## <a name="transport-layer-security-tls"></a>TLS (Transport Layer Security)

[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut utiliser le protocole TLS pour chiffrer des données transmises sur un réseau entre une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et une application cliente. Le chiffrement TLS est effectué au sein de la couche de protocole et il est disponible pour tous les clients [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pris en charge.

TLS peut être utilisé pour la validation de serveur quand une connexion cliente demande un chiffrement. Si l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute sur un ordinateur auquel un certificat a été attribué à partir d’une autorité de certification publique, son identité et l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont garanties par la chaîne des certificats menant jusqu’à l’autorité racine approuvée. Une telle validation de serveur requiert que l’ordinateur sur lequel l’application cliente s’exécute soit configuré de manière à approuver l’autorité racine du certificat utilisé par le serveur. 

Le chiffrement avec un certificat autosigné est possible (voir section suivante), mais ce type de certificat n’offre qu’une protection limitée.
Le niveau de chiffrement utilisé par TLS, sur 40 ou 128 bits, dépend de la version du système d’exploitation Microsoft Windows utilisée sur les ordinateurs d’application et de base de données.

> [!WARNING]
> L’utilisation du niveau de chiffrement 40 bits est considérée comme non sécurisée.

> [!WARNING]
> Les connexions TLS chiffrées à l’aide d’un certificat autosigné n’offrent pas de sécurité renforcée. Elles sont vulnérables aux attaques de l’intercepteur. Ne faites jamais confiance à une connexion TLS utilisant des certificats autosignés dans un environnement de production ou sur des serveurs connectés à Internet.

L’activation du chiffrement TLS améliore la sécurité des données transmises sur des réseaux entre des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et des applications. Toutefois, quand tout le trafic entre [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et une application cliente est chiffré au moyen de TLS, le traitement supplémentaire suivant est nécessaire :
-  Un aller-retour réseau supplémentaire est requis au moment de la connexion.
-  Les paquets envoyés de l’application à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doivent être chiffrés par la pile TLS du client et déchiffrés par la pile TLS du serveur.
-  Les paquets envoyés de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’application doivent être chiffrés par la pile TLS du serveur et déchiffrés par la pile TLS du client.

## <a name="about-certificates"></a><a name="about"></a> À propos des certificats

 Le certificat doit être émis pour l' **authentification serveur**. Le nom du certificat doit être le nom de domaine complet de l'ordinateur.  
  
 Les certificats sont stockés localement pour les utilisateurs de l'ordinateur. Pour installer un certificat afin que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’utilise, vous devez exécuter le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec un compte disposant des privilèges d’administrateur local.

 Le client doit être en mesure de vérifier la propriété du certificat employé par le serveur. Si le client dispose du certificat de clé publique de l'autorité de certification qui a signé le certificat de serveur, aucune configuration supplémentaire n'est requise. [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows comprend les certificats de clé publique d'un grand nombre d'autorités de certification. Si le certificat de serveur a été signé par une autorité de certification publique ou privée pour laquelle le client ne dispose pas de certificat de clé publique, vous devez installer le certificat de clé publique de l'autorité de certification ayant signé le certificat de serveur.  
  
> [!NOTE]  
> Pour utiliser le chiffrement à l'aide d'un cluster de basculement, installez le certificat du serveur avec le nom DNS complet du serveur virtuel sur tous les nœuds du cluster de basculement. Si, par exemple, vous disposez d’un cluster à deux nœuds nommés **_test1.\_\<your company>\*.com*** and **_test2.\_\<your company>\*.com***, ainsi que d’un serveur virtuel nommé **_virtsql_*_, vous devez installer un certificat pour _ *_virtsql.\_\<your company>\*.com*** sur les deux nœuds. Vous pouvez définir la valeur de l’option **Forcer le chiffrement** dans la zone de propriété **Protocoles pour virtsql** de **Configuration du réseau SQL Server** sur **Oui**.

> [!NOTE]
> Si vous créez des connexions chiffrées entre un indexeur de la Recherche Azure et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur une machine virtuelle Azure, consultez [Configurer une connexion à partir d’un indexeur de la Recherche Azure à SQL Server sur une machine virtuelle Azure](/azure/search/search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers). 

## <a name="certificate-requirements"></a>Configuration requise des certificats

Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], pour charger un certificat TLS, le certificat doit répondre aux conditions suivantes :

- Le certificat doit figurer soit dans le magasin de certificats de l'ordinateur local, soit dans le magasin de certificats de l'utilisateur actuel.

- Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit avoir l’autorisation nécessaire pour accéder au certificat TLS.

- L'heure actuelle du système doit être postérieure à la propriété **Valide du** du certificat et antérieure à la propriété **Valide jusqu’au** du certificat.

- Le certificat doit être destiné à une authentification serveur. Pour cela, la propriété **Utilisation améliorée de la clé** du certificat doit indiquer l’**authentification du serveur (1.3.6.1.5.5.7.3.1)** .

- Le certificat doit être créé avec l'option **KeySpec** de **AT_KEYEXCHANGE**. Habituellement, la propriété d'utilisation de la clé du certificat (**KEY_USAGE**) inclut également un chiffrement de clé (**CERT_KEY_ENCIPHERMENT_KEY_USAGE**).

- La propriété **Subject** du certificat doit indiquer que le nom courant (CN) est le même que le nom de domaine ou le nom de domaine complet (FQDN, Fully Qualified Domain Name) de l'ordinateur serveur. Quand vous utilisez le nom d’hôte, le suffixe DNS doit être spécifié dans le certificat. Si [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécute sur un cluster de basculement, le nom courant doit correspondre au nom d’hôte (ou FQDN) du serveur virtuel, et les certificats doivent être provisionnés sur tous les nœuds dans le cluster de basculement.

- [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] Native Client (SNAC) prennent en charge les certificats génériques. SNAC a depuis été déprécié et remplacé par [Microsoft OLE DB Driver pour SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) et [Microsoft ODBC Driver for SQL Server](../../connect/odbc/microsoft-odbc-driver-for-sql-server.md). Les autres clients ne prennent pas toujours en charge les certificats génériques.      
  Vous ne pouvez pas sélectionner un certificat générique à l’aide du Gestionnaire de configuration SQL Server. Pour utiliser un certificat générique, modifiez le Registre de clé `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\MSSQL12.MSSQLSERVER\MSSQLServer\SuperSocketNetLib`, puis entrez l’empreinte numérique du certificat, sans espaces, comme valeur **Certificat**.  

  > [!WARNING]  
  > [!INCLUDE[ssnoteregistry_md](../../includes/ssnoteregistry-md.md)]  

## <a name="install-on-single-server"></a><a name="single-server"></a>Installation sur un serveur unique

Avec [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], la gestion des certificats est intégrée au Gestionnaire de configuration SQL Server. Le Gestionnaire de configuration SQL Server pour [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] peut être utilisé avec des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Consultez [Gestion des certificats (Gestionnaire de configuration SQL Server)](../../database-engine/configure-windows/manage-certificates.md) pour ajouter un certificat sur une seule instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Si vous utilisez [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], et que le Gestionnaire de configuration SQL Server pour [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] n’est pas disponible, effectuez les étapes suivantes :

1. Dans le menu **Démarrer** , cliquez sur **Exécuter** puis, dans la zone **Ouvrir** , tapez **MMC** et cliquez sur **OK**.  
  
2. Dans la console MMC, dans le menu **Fichier** , cliquez sur **Ajouter/Supprimer un composant logiciel enfichable**.  
  
3. Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable** , cliquez sur **Ajouter**.  
  
4. Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Certificats**, puis sur **Ajouter**.  
  
5. Dans la boîte de dialogue **Composant logiciel enfichable Certificats** , cliquez sur **Compte d’ordinateur**, puis sur **Terminer**.  
  
6. Dans la boîte de dialogue **Ajout d’un composant logiciel enfichable autonome** , cliquez sur **Fermer**.  
  
7. Dans la boîte de dialogue **Ajouter/Supprimer un composant logiciel enfichable** , cliquez sur **OK**.  
  
8. Dans le composant logiciel enfichable **Certificats** , développez **Certificats**, **Personnel**, cliquez avec le bouton droit sur **Certificats**, pointez sur **Toutes les tâches**, puis cliquez sur **Importer**.  

9. Cliquez avec le bouton droit sur le certificat importé, pointez sur **Toutes les tâches**, puis cliquez sur **Gérer les clés privées**. Dans la boîte de dialogue **Sécurité**, ajoutez l’autorisation de lecture pour le compte d’utilisateur utilisé par le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
10. Remplissez les informations dans l' **Assistant Importation de certificat** afin d'ajouter un certificat à l'ordinateur, puis fermez la console MMC. Pour plus d'informations sur l'ajout d'un certificat à un ordinateur, consultez la documentation Windows.  

> [!IMPORTANT]
> Pour les environnements de production, nous vous recommandons de vous procurer un certificat approuvé auprès d’une autorité de certification.    
> À des fins de test, un certificat auto-signé peut aussi être utilisé. Pour créer un certificat auto-signé, voir l’[applet de commande PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate) ou la [commande certreq](/windows-server/administration/windows-commands/certreq_1).
  
## <a name="install-across-multiple-servers"></a>Installation sur plusieurs serveurs

Avec [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)], la gestion des certificats est intégrée au Gestionnaire de configuration SQL Server. Le Gestionnaire de configuration SQL Server pour [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] peut être utilisé avec des versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Reportez-vous à [Gestion des certificats (Gestionnaire de configuration SQL Server)](../../database-engine/configure-windows/manage-certificates.md) pour ajouter un certificat dans une configuration de cluster de basculement ou dans une configuration de groupe de disponibilité.

Si vous utilisez [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] à [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], et que le Gestionnaire de configuration SQL Server pour [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)] n’est pas disponible, suivez les étapes de la section [Pour provisionner (installer) un certificat sur un seul serveur](#single-server) pour chaque serveur.

## <a name="export-server-certificate"></a>Exportation du certificat de serveur  
  
1. Dans le composant logiciel enfichable **Certificats** , recherchez le certificat dans le dossier **Certificats** / **Personnel** , cliquez avec le bouton droit sur **Certificat**, pointez sur **Toutes les tâches**, puis cliquez sur **Exporter**.  
  
2. Suivez les étapes de l' **Assistant d'exportation de certificats** en stockant le fichier de certificat à un emplacement approprié.  
  
## <a name="configure-server"></a>Configuration du serveur

Configurez le serveur afin qu’il force les connexions chiffrées.

> [!IMPORTANT]
> Le compte de service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit disposer d’autorisations de lecture sur le certificat utilisé pour forcer le chiffrement sur le serveur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour un compte de service sans privilèges, les autorisations de lecture doivent être ajoutées au certificat. Si vous ne le faites pas, vous risquez de provoquer l’échec du redémarrage du service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].
  
1. Dans le **Gestionnaire de configuration SQL Server**, développez **Configuration du réseau SQL Server**, cliquez avec le bouton droit sur **Protocoles pour** _\<server instance>_ , puis sélectionnez **Propriétés**.  
  
2. Dans la boîte de dialogue **Propriétés** _\<instance name>_ **Protocoles pour**, cliquez sur l'onglet **Certificat**, sélectionnez le certificat voulu dans la liste déroulante de la zone **Certificat**, puis cliquez sur **OK**.  
  
3. Sur l'onglet **Indicateurs** , dans la zone **ForceEncryption** , sélectionnez **Oui**, puis cliquez sur **OK** pour fermer la boîte de dialogue.  
  
4. Redémarrez le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  

> [!NOTE]
> Pour garantir une connectivité sécurisée entre le client et le serveur, configurez le client pour qu’il demande des connexions chiffrées. Plus de détails figurent [plus loin dans cet article](#configure-client).

## <a name="configure-client"></a><a name="configure-client"></a>Configuration du client

Configurez le client afin qu'il demande l'établissement de connexions chiffrées.

1. Copiez le certificat d'origine ou le fichier de certificat exporté sur l'ordinateur client.  
  
2. Sur l’ordinateur client, utilisez le composant logiciel enfichable **Certificats** pour installer le certificat racine ou le fichier de certificat exporté.  
  
3. À l’aide du Gestionnaire de configuration SQL Server, cliquez avec le bouton droit sur **Configuration de SQL Server Native Client**, puis cliquez sur **Propriétés**.  
  
4. Sur la page **Indicateurs** , dans la zone **Forcer le chiffrement du protocole** , cliquez sur **Oui**.  
  
## <a name="use-sql-server-management-studio"></a>Utiliser SQL Server Management Studio
  
Pour chiffrer une connexion à partir de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] :  

1. Dans la barre d'outils de l'Explorateur d'objets, cliquez sur **Connecter**, puis sur **Moteur de base de données**.  
  
2. Dans la boîte de dialogue **Se connecter au serveur** , fournissez les informations de connexion, puis cliquez sur **Options**.  
  
3. Sous l'onglet **Propriétés de connexion** , cliquez sur **Chiffrer la connexion**.  

## <a name="internet-protocol-security-ipsec"></a>Internet Protocol Security (IPSec)
Les données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être chiffrées pendant la transmission à l’aide du protocole IPSec. IPSec est fourni par les systèmes d’exploitation client et serveur et ne nécessite aucune configuration de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur IPSec, consultez la documentation Windows ou de la mise en réseau.

## <a name="next-steps"></a>Étapes suivantes

+ [Prise en charge de TLS 1.2 pour Microsoft SQL Server](https://support.microsoft.com/kb/3135244)     
+ [Configurer le Pare-feu Windows pour autoriser l’accès à SQL Server](../../sql-server/install/configure-the-windows-firewall-to-allow-sql-server-access.md)     
+ [Applet de commande PowerShell New-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)
