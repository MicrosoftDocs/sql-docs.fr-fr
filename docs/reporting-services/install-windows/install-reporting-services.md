---
description: Installation de SQL Server Reporting Services
title: Installer SQL Server Reporting Services | Microsoft Docs
ms.date: 12/11/2020
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
monikerRange: '>= sql-server-2016'
ms.openlocfilehash: a6c2dc6ae1a8711c3e78403b539a603c0cb36dcf
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596994"
---
# <a name="install-sql-server-reporting-services"></a>Installation de SQL Server Reporting Services

[!INCLUDE[ssrs-appliesto](../../includes/ssrs-appliesto.md)] [!INCLUDE[ssrs-appliesto-2017-and-later](../../includes/ssrs-appliesto-2017-and-later.md)] [!INCLUDE[ssrs-appliesto-not-pbirsi](../../includes/ssrs-appliesto-not-pbirs.md)]

L’installation de SQL Server Reporting Services comprend des composants serveur pour le stockage des éléments de rapport, le rendu des rapports et le traitement des services d’abonnement et autres services de rapport. 

::: moniker range=">=sql-server-ver15"
Téléchargez [SQL Server 2019 Reporting Services](https://www.microsoft.com/download/details.aspx?id=100122) à partir du Centre de téléchargement Microsoft.

::: moniker-end

::: moniker range="=sql-server-2017"
Téléchargez [SQL Server 2017 Reporting Services](https://www.microsoft.com/download/details.aspx?id=55252) à partir du Centre de téléchargement Microsoft.

::: moniker-end

> [!NOTE]
> Vous recherchez Power BI Report Server ? Consultez [Installer Power BI Report Server](https://powerbi.microsoft.com/documentation/reportserver-install-report-server/).
> 
> Vous effectuez une mise à niveau ou une migration à partir de SQL Server 2016 Reporting Services ou version antérieure ? Consultez [Mettre à niveau et migrer Reporting Services](upgrade-and-migrate-reporting-services.md).

## <a name="before-you-begin"></a>Avant de commencer

Avant d’installer Reporting Services, prenez connaissance des [Configurations matérielle et logicielle requises pour l’installation de SQL Server](../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md).

## <a name="install-your-report-server"></a>Installer votre serveur de rapports

L’installation d’un serveur de rapports est simple. Quelques étapes suffisent à l’installation des fichiers.

> [!NOTE]
> Vous n’avez pas besoin d’un serveur Moteur de base de données SQL Server disponible au moment de l’installation. Vous en aurez besoin pour configurer Reporting Services après l’installation.

1. Recherchez l’emplacement de SQLServerReportingServices.exe et lancez le programme d’installation.

2. Sélectionnez **Installer Reporting Services**.

3. Choisissez une édition à installer, puis sélectionnez **Suivant**.

    Pour installer une édition gratuite, choisissez l’édition d’évaluation ou Developer dans la liste déroulante.

    ![Éditions d’évaluation ou Développeur](media/install-reporting-services/report-server-install-edition-select.png)

    Sinon, entrez une clé de produit. [Rechercher la clé de produit de SQL Server Reporting Services](find-reporting-services-product-key-ssrs.md).

4. Lisez et acceptez les termes du contrat de licence, puis sélectionnez **Suivant**.

5. Un moteur de base de données doit être disponible pour stocker la base de données du serveur de rapports. Sélectionnez **Suivant** pour installer uniquement le serveur de rapports.

6. Indiquez l’emplacement d’installation du serveur de rapports. Sélectionnez **Installer** pour continuer.

    > [!NOTE]
    > Le chemin par défaut est C:\Program Files\Microsoft SQL Server Reporting Services.

7. Après une installation réussie, sélectionnez **Configurer le serveur de rapports** pour lancer le Gestionnaire de configuration du serveur de rapports.

## <a name="configure-your-report-server"></a>Configurer votre serveur de rapports

Après avoir sélectionné **Configurer le serveur de rapports** dans le programme d’installation, le **Gestionnaire de configuration du serveur de rapports** s’affiche. Pour plus d’informations, consultez [Gestionnaire de configuration du serveur de rapports](reporting-services-configuration-manager-native-mode.md).

Vous devez [créer une base de données du serveur de rapports](ssrs-report-server-create-a-report-server-database.md) pour réaliser la configuration initiale de Reporting Services. Un serveur de base de données SQL Server est nécessaire pour effectuer cette étape.

### <a name="creating-a-database-on-a-different-server"></a>Création d’une base de données sur un serveur distinct

Si vous créez la base de données du serveur de rapports sur un serveur de base de données hébergé sur un autre ordinateur, vous devrez modifier le compte de service du serveur de rapports de manière à indiquer des informations d’identification reconnues sur le serveur de base de données.

Par défaut, le serveur de rapports utilise le compte de service virtuel. Si vous essayez de créer une base de données sur un autre serveur, vous risquez de recevoir l’erreur suivante au cours de l’étape Application des droits de connexion :

`System.Data.SqlClient.SqlException (0x80131904): Windows NT user or group '(null)' not found. Check the name again.`

Pour contourner cette erreur, vous pouvez modifier le compte de service et indiquer Service réseau ou un compte de domaine. Si vous choisissez le compte de service Service réseau, des droits sont appliqués dans le contexte du compte d’ordinateur du serveur de rapports.

Pour plus d’informations, consultez [Configurer le compte de service du serveur de rapports](configure-the-report-server-service-account-ssrs-configuration-manager.md).

## <a name="windows-service"></a>Service Windows

Un service Windows est créé dans le cadre de l’installation. Il est affiché sous la forme **SQL Server Reporting Services** et porte le nom **SQLServerReportingServices**.

## <a name="default-url-reservations"></a>Réservations d’URL par défaut

Les réservations d'URL se composent d'un préfixe, d'un nom d'hôte, d'un port et d'un répertoire virtuel :

|Élément|Description|
|----------|-----------------|
|Préfixe|Le préfixe par défaut est HTTP. Si vous avez préalablement installé un certificat TLS (Transport Layer Security), précédemment appelé SSL (Secure Sockets Layer), le programme d’installation tente de créer des réservations d’URL qui utilisent le préfixe HTTPS.|
|Nom de l’hôte|Le nom d'hôte par défaut est un caractère générique fort (+). Il indique que le serveur de rapports accepte toute requête HTTP sur le port désigné pour tout nom d’hôte qui correspond à l’ordinateur, notamment `https://<computername>/reportserver`, `https://localhost/reportserver` ou`https://<IPAddress>/reportserver.`|
|Port|Le port par défaut est 80. Si vous utilisez un port autre que le port 80, vous devez l’ajouter explicitement à l’URL lorsque vous ouvrez un portail web dans une fenêtre de navigateur.|
|Répertoire virtuel|Par défaut, les répertoires virtuels sont créés au format ReportServer pour le service web Report Server et au format Reports pour le portail web. Pour le service Web Report Server, le répertoire virtuel par défaut est **reportserver**. Pour le portail web, le répertoire virtuel par défaut est **reports**.|

Voici un exemple de chaîne URL complète :

- `https://+:80/reportserver` fournit l’accès au serveur de rapports.

- `https://+:80/reports` fournit l’accès au portail web.

## <a name="firewall"></a>Pare-feu

Si vous accédez au serveur de rapports à partir d’un ordinateur distant, vous souhaitez vous assurer que vous avez configuré les règles de pare-feu si un pare-feu est présent.

Vous devez ouvrir le port TCP que vous avez configuré pour l’URL de votre service web et l’URL de votre portail web. Par défaut, il s’agit du port TCP 80.

## <a name="additional-configuration"></a>Configuration supplémentaire

- Pour configurer l’intégration avec le service Power BI de manière à pouvoir épingler des éléments de rapport à un tableau de bord Power BI, consultez [Intégration avec le service Power BI](power-bi-report-server-integration-configuration-manager.md).

- Pour configurer l’e-mail pour le traitement des abonnements, consultez [Paramètres d’e-mail](e-mail-settings-reporting-services-native-mode-configuration-manager.md) et [Remise d’e-mail dans un serveur de rapports](../subscriptions/e-mail-delivery-in-reporting-services.md).

- Pour configurer le portail web et le rendre accessible sur un ordinateur distant afin de consulter et de gérer des rapports, consultez [Configurer un pare-feu pour accéder au serveur de rapports](../report-server/configure-a-firewall-for-report-server-access.md) et [Configurer un serveur de rapports pour l’administration à distance](../report-server/configure-a-report-server-for-remote-administration.md).

## <a name="related-information"></a>Informations connexes

Pour plus d’informations sur l’installation de SQL Server Reporting Services en mode natif, consultez [Installer le serveur de rapports Reporting Services en mode natif](install-reporting-services-native-mode-report-server.md). 

::: moniker range="=sql-server-2016"

Pour plus d’informations sur l’installation de SQL Server 2016 Reporting Services (et versions antérieures) en mode intégré SharePoint, consultez [Installer le premier serveur de rapports en mode SharePoint](install-the-first-report-server-in-sharepoint-mode.md).

::: moniker-end

## <a name="next-steps"></a>Étapes suivantes

Une fois votre serveur de rapports installé, commencez à créer des rapports et déployez-les sur votre serveur de rapports. Pour plus d’informations sur le démarrage du Générateur de rapports, consultez [Installer le Générateur de rapports](../../reporting-services/install-windows/install-report-builder.md).

Pour créer des rapports à l’aide SQL Server Data Tools, [téléchargez SQL Server Data Tools](../../ssdt/download-sql-server-data-tools-ssdt.md).

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)