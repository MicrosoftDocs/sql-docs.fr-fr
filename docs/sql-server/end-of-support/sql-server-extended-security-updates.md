---
title: Que sont les correctifs de sécurité étendus ?
description: Découvrez l’utilisation du registre SQL Server pour obtenir des correctifs de sécurité étendus pour vos produits de fin de support et de fin de vie SQL Server, tels que SQL Server 2008 et SQL Server 2008 R2.
ms.custom: ''
ms.date: 11/24/2020
ms.prod: sql
ms.technology: install
ms.topic: conceptual
author: cawrites
ms.author: chadam
ms.reviewer: pmasl
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 0f97fcf001db6fa561d54b3a9df6642f2aa148ea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346169"
---
# <a name="what-are-extended-security-updates-for-sql-server"></a>Que sont les correctifs de sécurité étendus pour SQL Server ?
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

Cet article fournit des informations sur l’utilisation du service de registre SQL Server pour recevoir des correctifs de sécurité étendus pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)]. Pour plus d’informations sur d’autres options, consultez [options Fin du support](sql-server-end-of-life-overview.md). 

## <a name="overview"></a>Vue d’ensemble
Une fois que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a atteint la fin de son cycle de vie de support, vous avez la possibilité de souscrire un abonnement de Correctif de sécurité étendu (ESU) pour vos serveurs et de rester protégé pendant trois ans maximum, jusqu’à ce que vous soyez prêt à effectuer une mise à niveau vers une version plus récente de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou à migrer vers [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)]. Cet abonnement peut être obtenu de deux manières :
-  Il peut être acheté pour vos serveurs d’environnement locaux ou hébergés.
-  Il peut être obtenu gratuitement et activé par défaut quand vous migrez des serveurs locaux vers des machines virtuelles Azure. Vous pouvez ensuite utiliser le service du **Registre SQL Server** dans le Portail Azure pour enregistrer votre instance de fin de support [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et télécharger les mises à jour dès qu’elles sont disponibles. 

Microsoft recommande l’application des correctifs ESU dès qu’ils sont disponibles pour protéger votre instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations sur les correctifs ESU, consultez la [page FAQ ESU](https://www.microsoft.com/cloud-platform/extended-security-updates).

> [!IMPORTANT]
> [Le support étendu de SQL Server 2008 et SQL Server 2008 R2 a pris fin le 10 juillet 2019](https://www.microsoft.com/cloud-platform/windows-sql-server-2008). Pour ces versions, envisagez d’utiliser des correctifs de sécurité étendus décrits dans cet article ou d’autres options de migration. Pour plus d’informations, consultez [Options de fin du support](sql-server-end-of-life-overview.md).

## <a name="what-are-extended-security-updates"></a>Que sont les correctifs de sécurité étendus
Les correctifs de sécurité étendus (ESU) pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] incluent la fourniture de correctifs de sécurité pour les clients qui ont acheté un abonnement de mise à jour de support étendu.

Les ESU sont mis à disposition **si nécessaire** quand une vulnérabilité de la sécurité est découverte et évaluée comme **Critique** par le [Centre de réponse aux problèmes de sécurité Microsoft (MSRC)](https://portal.msrc.microsoft.com). Par conséquent, il n’existe pas de cadence de publication régulière pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ESU.

Les ESU n’incluent pas :
- Nouvelles fonctionnalités
- Améliorations des fonctions
- Correctifs demandés par le client

### <a name="support"></a>Support
Les ESU n’incluent pas de support technique, mais vous pouvez utiliser un contrat de support actif, tel que [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default?activetab=software-assurance-default-pivot%3aprimaryr3) ou un support premier/unifié sur [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] / [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] pour obtenir un support technique sur les charges de travail couvertes par les ESU si vous choisissez de rester en local. Si vous hébergez sur Azure, vous pouvez également utiliser un plan de support Azure pour obtenir un support technique. 

  > [!NOTE]
  > Microsoft ne peut pas fournir de support technique pour les instances [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] (à la fois sur site et dans les environnements d’hébergement) qui ne sont pas couvertes par un abonnement ESU. 

## <a name="esu-availability-and-deployment"></a>Disponibilité et déploiement des ESU
Les ESU sont disponibles pour les clients qui exécutent leur charge de travail dans Azure, localement ou dans des environnements hébergés.

### <a name="azure-virtual-machines"></a>Machines virtuelles Azure
Si vous migrez vos charges de travail vers des machines Virtuelles Azure (IaaS), vous aurez accès à des correctifs de sécurité étendus pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] pendant trois ans au plus après la fin du support, avec **aucuns frais supplémentaires** au-delà du coût d’exécution de la machine virtuelle. Les clients n’ont pas besoin de la Software Assurance pour recevoir des correctifs de sécurité étendus dans Azure. 

Les machines virtuelles Azure qui exécutent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur **Windows Server 2008 R2 et versions ultérieures** reçoivent automatiquement des ESU par le biais de canaux de mise à jour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] existants quand la machine virtuelle est configurée pour utiliser la [mise à jour corrective automatisée](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching).

Pour les machines virtuelles Azure qui exécutent [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur **Windows Server 2008** et les machines virtuelles qui n’ont **_pas_ été configurées pour la [mise à jour corrective automatisée](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)** , vous devez télécharger et déployer manuellement les correctifs ESU comme décrit dans la section [Environnements locaux ou hébergés](#on-premises-or-hosted-environments).

### <a name="on-premises-or-hosted-environments"></a>Environnements locaux ou hébergés
Si vous disposez de Software Assurance, vous pouvez acheter un abonnement aux correctifs de sécurité étendus (ESU, Extended Security Update) pour une durée allant jusqu’à trois ans après la date de la fin du support, dans le cadre d’un Accord Entreprise (EA), d’un contrat d’abonnement entreprise (EAS), d’un accord de mise en œuvre serveur et cloud (SCE) ou d’un accord de mise en œuvre pour des solutions Education (EES). Vous pouvez acheter des ESU uniquement pour les serveurs que vous devez couvrir. Vous pouvez acheter des ESU directement auprès de Microsoft ou d’un partenaire de licence Microsoft. 

Les clients couverts par des accords ESU doivent suivre les étapes suivantes pour télécharger et déployer un correctif ESU :
-  [Inscrire les instances éligibles](#register-instances-for-esus) auprès du **[registre SQL Server](#create-sql-server-registry)** . 
-  Une fois l’inscription effectuée, chaque fois que des correctifs ESU sont publiés, un lien est mis à disposition dans le portail Azure pour le téléchargement du package. 
-  Le package téléchargé peut être déployé sur vos environnements locaux ou hébergés manuellement ou à l’aide de toute solution d’orchestration de mise à jour utilisée dans votre organisation, par exemple Microsoft Endpoint Configuration Manager (anciennement System Center Configuration Manager). 

> [!NOTE]
> Il s’agit également du processus que les clients devront suivre pour Azure Stack et les machines virtuelles Azure qui ne sont pas configurés pour recevoir des mises à jour automatiques.

Pour plus d’informations, consultez la section [Forum aux questions sur les correctifs de sécurité étendus](https://www.microsoft.com/cloud-platform/extended-security-updates). 

## <a name="create-sql-server-registry"></a>Créer un registre SQL Server
Pour enregistrer vos instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] compatibles ESU, vous devez d’abord créer le registre SQL Server dans le Portail Azure. 

> [!IMPORTANT]
> Il n’est pas nécessaire d’enregistrer des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour des ESU lors de l’exécution d’une machine virtuelle Azure configurée pour [des mises à jour automatiques](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching). 

Pour créer le registre SQL Server, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com). 
1. Sélectionnez l'option pour **Créer une ressource**. 
1. Tapez `SQL Server registry` dans la zone de recherche.  
1. Choisissez l’option **Registre SQL Server** publiée par [!INCLUDE[msCoName](../../includes/msconame-md.md)] puis, sélectionnez **Créer**. 

   ![Capture d’écran du portail Azure montrant comment créer un registre SQL Server.](media/sql-server-extended-security-updates/sql-server-registry-service.png)

1. Sous **Détails du projet**, choisissez votre abonnement dans la liste déroulante. Ensuite, choisissez un **Groupe de ressources** existant ou sélectionnez **Créer un nouveau** pour créer un nouveau groupe de ressources pour votre nouveau service de registres SQL Server. 
1. Sous **Détails du service**, indiquez un nom et une région pour votre nouvelle ressource **Registre SQL Server** : 

   ![Capture d’écran du registre SQL Server montrant l’onglet De base.](media/sql-server-extended-security-updates/create-new-sql-server-registry.png)

1. Sélectionnez **Évaluer et Créer** pour évaluer les détails de votre **Registre SQL Server**. Sélectionnez **Créer** une fois la validation passée. 

## <a name="register-instances-for-esus"></a>Enregistrer des instances pour des ESU

Une fois que la ressource **Registre SQL Server** est déployée, vous pouvez choisir d’enregistrer une instance [unique](#single-sql-server-instance)[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou vous pouvez enregistrer plusieurs instances des instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [en bloc](#multiple-sql-server-instances-in-bulk). Au moins une instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] doit obligatoirement être inscrite dans l’étendue de votre registre SQL Server afin de télécharger des packages ESU. 

### <a name="single-sql-server-instance"></a>Instance SQL Server unique

Pour enregistrer une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] unique, procédez comme suit :

1. Connectez-vous au [portail Azure](https://portal.azure.com). 
1. Accédez à votre ressource **Registre SQL Server**. 
1. Sélectionnez **+ Enregistrer** dans le volet **Vue d’ensemble** : 

   ![Choisissez enregistrer pour enregistrer une seule instance de SQL Server](media/sql-server-extended-security-updates/register-single-sql-server-instance.png)

1. Fournissez les informations requises comme détaillées dans ce tableau et sélectionnez **Enregistrer** : 

   |**Valeur**| **Description**|
   | :-------| :------------- |
   | **Instance** | Entrez la sortie de la commande `SELECT @@SERVERNAME`, par exemple `MyServer\Instance01`. | 
   | **Version SQL** | Sélectionnez 2008 ou 2008 R2 dans la liste déroulante. | 
   | **Édition** | Dans la liste déroulante, sélectionnez l'édition applicable : Centre de données, Développeur (déploiement gratuit en cas d’ESU achetés), Enterprise, Standard, Web, Groupe de travail. | 
   | **Cœurs** | Saisissez le nombre de cœurs pour cette instance | 
   | **Type d'hôte** | Dans la liste déroulante, sélectionnez le type d’hôte applicable : Machine virtuelle (locale), serveur physique (local), machine virtuelle Azure, Amazon EC2, moteur de calcul Google, autre. |
   | **SubscriptionID**<sup>1</sup> | Saisissez le SubscriptionID dans lequel la machine virtuelle est créée.  |
   | **Groupe de ressources**<sup>1</sup> | Saisissez le groupe de ressources dans lequel la machine virtuelle est créée.  | 
   | **Nom de la machine virtuelle Azure**<sup>1</sup>  | Saisissez le nom de ressource de la machine virtuelle.  | 
   | **Système d'exploitation de la machine virtuelle Azure**<sup>1</sup> | Sélectionnez la version du système d’exploitation Windows Server applicable dans la liste déroulante. | 

   <sup>1</sup> Uniquement nécessaire pour les machines virtuelles Azure. 

L’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nouvellement enregistrée est désormais visible dans la section **Enregistrer des instances** du volet **Vue d’ensemble** : 

![Instances enregistrées](media/sql-server-extended-security-updates/registered-sql-instance.png)

Une fois qu’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] a été enregistrée, la section **Correctifs de sécurité** est disponible. Tous les ESU disponibles seront publiés ici. 

### <a name="multiple-sql-server-instances-in-bulk"></a>Plusieurs Instances en bloc

Plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peuvent être enregistrées en téléchargeant un fichier .CSV. Une fois votre [fichier .CSV correctement mis en forme](#formatting-requirements-for-csv-file), vous pouvez suivre ces étapes pour enregistrer en bloc vos instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec la ressource de Registre SQL Server : 

1. Connectez-vous au [portail Azure](https://portal.azure.com). 
1. Accédez à votre ressource **Registre SQL Server**. 
1. Sélectionnez **Enregistrer en bloc** dans le volet **Vue d’ensemble** :  

   ![Choisissez Enregistrer en bloc pour enregistrer plusieurs instances de SQL Server](media/sql-server-extended-security-updates/bulk-register-sql-server-instances.png)

1. Sélectionnez l’icône de fichier pour accéder à l’emplacement de votre fichier .CSV. Sélectionnez le fichier .CSV. Sélectionnez ensuite **Enregistrer** pour télécharger le fichier et enregistrer plusieurs instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. 

   ![Téléchargez le fichier CSV pour enregistrer plusieurs instances de SQL Server](media/sql-server-extended-security-updates/upload-csv-file-for-bulk-registration.png)

### <a name="formatting-requirements-for-csv-file"></a>Exigences de mise en forme pour un fichier CSV
- Les valeurs sont séparées par des virgules
- Les valeurs ne sont pas uniques ou à deux guillemets
- Les noms de colonnes ne respectent pas la casse, mais ils doivent être **nommés** comme indiqué ci-dessous : 
  - name
  - version
  - edition
  - cœurs
  - hostType
  - subscriptionID<sup>1</sup>
  - resourceGroup<sup>1</sup>
  - azureVmName<sup>1</sup>
  - AzureVmOS<sup>1</sup>

<sup>1</sup> Uniquement nécessaire pour les machines virtuelles Azure. 

#### <a name="csv-example-1---on-premises"></a>Exemple CSV 1- local

Pour les instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] locales, le fichier CVS doit ressembler à ce qui suit : 

```csv
name,version,edition,cores,hostType
Server1\SQL2008,2008,Enterprise,12,Physical Server
Server1\SQL2008R2,2008 R2,Enterprise,12,Physical Server
Server2\SQL2008R2,2008 R2,Enterprise,24,Physical Server
Server3\SQL2008R2,2008 R2,Enterprise,12,Virtual Machine
Server4\SQL2008,2008,Developer,8,Physical Server  
```

Reportez-vous à [MyPhysicalServers.csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyPhysicalServers.csv) pour obtenir un exemple de fichier CSV.

#### <a name="csv-example-2---azure-vm"></a>Exemple CSV 2 - machine virtuelle Azure

Pour les instances de machine virtuelle Azure [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], le fichier CVS doit ressembler à ce qui suit : 

```csv
name,version,edition,cores,hostType,subscriptionId,resourceGroup,azureVmName,azureVmOS    
ProdServerUS1\SQL01,2008 R2,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ProdServerUS1\SQL02,2008 R2,Enterprise,24,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM1,2012    
ServerUS2\SQL01,2008,Enterprise,12,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
ServerUS2\SQL02,2008,Enterprise,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM2,2012 R2    
SalesServer\SQLProdSales,2008 R2,Developer,8,Azure Virtual Machine,61868ab8-16d4-44ec-a9ff-f35d05922847,RG,VM3,2008 R2  
```

Reportez-vous à [MyAzureVMs. csv](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts/MyAzureVMs.csv) pour un exemple de fichier CSV ciblé de machine virtuelle Azure. 


> [!TIP]
> Pour obtenir des exemples de scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] et PowerShell qui peuvent générer les informations d’inscription de l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] requises dans un fichier .CSV, consultez [Exemples de script d’inscription ESU](https://github.com/microsoft/sql-server-samples/blob/master/samples/manage/sql-server-extended-security-updates/scripts.md). 

## <a name="download-esus"></a>Télécharger des ESU

Une fois vos instances [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] enregistrées avec le service de registre SQL Server, vous pouvez télécharger les packages de correctifs de sécurité étendus à l’aide du lien qui se trouve dans le Portail Azure, selon leur disponibilité. 

Pour télécharger des ESU, procédez comme suit : 

1. Connectez-vous au [portail Azure](https://portal.azure.com). 
1. Accédez à votre ressource **Registre SQL Server**. 
1. Sélectionnez **Correctifs de sécurité** dans le volet de navigation. 

   ![Vérifier les mises à jour disponibles dans le volet des correctifs de sécurité](media/sql-server-extended-security-updates/security-updates-sql-registry.png)

1. Téléchargez des correctifs de sécurité à partir d’ici, selon leur disponibilité. 

## <a name="supported-regions-and-data-residency"></a>Régions et résidence des données prises en charge

Le service de **Registre SQL Server** (préversion) est disponible dans un sous-ensemble des régions Azure. La table suivante indique les régions prises en charge et le type de résidence des données de chacune.

| **Région** | **Résidence des données** |
|:--|:--|
|Australie Est|Zone géographique|
|Sud-Australie Est|Zone géographique|
|Centre du Canada|Zone géographique|
|France Centre|Zone géographique|
|Japon Est|Zone géographique|
|OuJapon Est|Zone géographique|
|Centre de la Corée|Zone géographique|
|Corée du Sud|Zone géographique|
|Centre-Nord des États-Unis|Zone géographique|
|Europe Nord|Zone géographique|
|États-Unis - partie centrale méridionale|Zone géographique|
|Asie Sud-Est|Région unique|
|Inde Sud|Zone géographique|
|Afrique du Sud Nord|Zone géographique|
|Sud du Royaume-Uni|Zone géographique|
|Ouest du Royaume-Uni|Zone géographique|
|USA Ouest|Zone géographique|
|USA Est|Zone géographique|
|USA Centre|Zone géographique|
|Asie Est|Zone géographique|
|Europe Ouest|Zone géographique|
|Centre-USA Ouest|Zone géographique|
|USA Ouest 2|Zone géographique|
|USA Est 2|Zone géographique|

Dans les régions avec résidence géographique, le service de Registre SQL gère les sauvegardes de données dans un compte de stockage géoredondant (GRS).  Dans les régions avec résidence de région unique, le service de Registre SQL gère les sauvegardes de données dans un compte de stockage redondant interzone (ZRS). Pour plus d’informations, consultez le [Centre de gestion de la confidentialité](https://azuredatacentermap.azurewebsites.net/).

## <a name="configure-regional-redundancy"></a>Configurer la redondance régionale 

Les clients qui ont besoin d’une redondance régionale pour leur **Registre SQL Server** peuvent créer des données d’inscription dans deux régions distinctes. Les clients peuvent ensuite télécharger les mises à jour de sécurité à partir de l’une ou l’autre région, en fonction de la disponibilité du service **Registre SQL Server**. 

Pour la redondance régionale, le service **Registre SQL Server** doit être créé dans deux régions différentes, et votre inventaire SQL Server doit être réparti entre ces deux services. De cette façon, la moitié de vos serveurs SQL sont inscrits auprès du service de registre dans une région, puis l’autre moitié de vos serveurs SQL Server sont inscrits auprès du service de registre dans l’autre région. 

Pour configurer la redondance régionale, procédez comme suit :

1. Fractionnez votre inventaire SQL Server 2008 ou 2008 R2 en deux fichiers, par exemple upload1.csv et upload2.csv. 
  
   :::image type="content" source="media/sql-server-extended-security-updates/two-upload-files-for-regional-redundancy.png" alt-text="Exemple de chargement de fichiers":::

1. Créez le premier service **Registre SQL Server** dans une région, puis inscrivez-y en bloc un des fichiers CSV. Par exemple, créez le premier service **Registre SQL Server** dans la région **USA Ouest** et enregistrez en bloc vos serveurs SQL à l’aide du fichier upload1.csv. 
1. Créez le deuxième service **Registre SQL Server** dans la deuxième région, puis inscrivez-y en bloc l’autre fichier CSV. Par exemple, créez le deuxième service **Registre SQL Server** dans la région **USA Est** et inscrivez en bloc vos serveurs SQL à l’aide du fichier upload2.csv. 


Une fois que vos données ont été enregistrées avec les deux ressources **Registre SQL Server** différentes, vous pouvez télécharger les mises à jour de sécurité à partir de l’une ou l’autre région, en fonction de la disponibilité du service. 


## <a name="faq"></a>Questions fréquentes (FAQ)

Vous trouverez les questions générales fréquemment posées sur les correctifs de sécurité étendus dans la section [Forum aux questions (FAQ) sur les correctifs de sécurité étendus](https://www.microsoft.com/cloud-platform/extended-security-updates). Les questions fréquemment posées spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont répertoriées ci-dessous. 

**Quand se trouve la fin du support de SQL Server 2008 et 2008 R2 ?**

La date de la fin du support pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] était le 9 juillet 2019. 

**Que signifie la fin du support ?**

La politique de cycle de vie Microsoft offre un support de 10 ans (5 ans pour le support standard et 5 ans de support étendu) pour les produits Entreprise et Développeur (tels que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows Server). Conformément à la politique, après la fin de la période de support étendu, il n’y aura aucun correctif ni aucun correctif de sécurité, ce qui peut entraîner des problèmes de sécurité et de conformité et exposer les applications des clients et l’entreprise à des risques de sécurité sérieux.

**Quelles éditions de SQL Server sont éligibles pour les correctifs de sécurité étendues ?**

Les éditions Enterprise, Centre de données, standard, Web et Groupe de travail de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] peuvent bénéficier de correctifs de sécurité étendus pour les versions x86 et x64. 

**Quand l’offre de correctifs de sécurité étendus sera-t-elle disponible ?**

Les correctifs de sécurité étendus sont désormais disponibles à l’achat et peuvent être commandés à partir de [!INCLUDE[msCoName](../../includes/msconame-md.md)] ou d’un partenaire de licence [!INCLUDE[msCoName](../../includes/msconame-md.md)]. La livraison des correctifs de sécurité étendus commencera après les dates de fin du support, selon leur disponibilité. Les clients intéressés par la migration vers Azure peuvent le faire immédiatement. 

**Que comprennent les correctifs de sécurité étendus ?** 

Les correctifs de sécurité étendus comprennent l’approvisionnement de correctifs et de bulletins de sécurité évalués comme **critiques** par le [Centre de réponse aux problèmes de sécurité Microsoft (MSRC)](https://portal.msrc.microsoft.com/) pendant une durée maximale de trois ans à compter du 9 juillet 2019. Les correctifs de sécurité étendus seront distribués selon leur disponibilité. Les correctifs de sécurité étendus n’incluent pas de support technique, mais vous pouvez utiliser d’autres plans de support [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour obtenir des réponses à vos questions [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] sur les charges de travail couvertes par les correctifs de sécurité étendus. Les correctifs de sécurité étendus n’incluent pas les nouvelles fonctionnalités, les améliorations de fonctions ni les correctifs demandés par le client. Toutefois, [!INCLUDE[msCoName](../../includes/msconame-md.md)] peut inclure des correctifs hors sécurité jugés nécessaires.

**Pourquoi les correctifs de sécurité étendus pour SQL Server 2008 et 2008 R2 offrent-ils uniquement des mises à jour « critiques » ?**

Pour les derniers événements de fin du support, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] offrait uniquement des correctifs de sécurité critiques, répondant aux critères de conformité des clients de nos entreprises. [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’expédie pas de correctif de sécurité mensuel général. [!INCLUDE[msCoName](../../includes/msconame-md.md)] ne fournit que des correctifs de sécurité [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (GDR) pour des bulletins MSRC, lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est identifié comme un produit affecté.
S’il existe des situations pour lesquelles de nouvelles mises à jour importantes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne seront pas fournies tout en étant considérées comme critiques par le client, mais pas par le MSRC, nous travaillons avec le client au cas par cas pour suggérer une atténuation appropriée.

**Quels sont les programmes de licences éligibles pour les correctifs de sécurité étendus ?**

Si vous disposez de Software Assurance, vous pouvez acheter des correctifs de sécurité étendus jusqu’à trois ans après la fin du support, dans le cadre d’un Accord Entreprise (EA), d’un contrat d’abonnement entreprise (EAS), d’un accord de mise en œuvre serveur et cloud (SCE) ou d’un accord de mise en œuvre pour des solutions Education (EES). La Software assurance n’a pas besoin d’être sur le même accord de mise en œuvre.

**Les clients SQL Server doivent-ils exécuter le Service Pack le plus récent pour tirer parti des correctifs de sécurité étendus ?**

Oui, les clients doivent exécuter [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou Windows Server 2008 et 2008 R2 avec le dernier Service Pack pour appliquer les correctifs de sécurité étendus. [!INCLUDE[msCoName](../../includes/msconame-md.md)] produira uniquement des mises à jour qui peuvent être appliquées au Service Pack le plus récent.

**Quelles sont les options pour les clients SQL Server sans Software Assurance ?** 

Pour les clients qui ne disposent pas de Software Assurance, l’autre option pour accéder aux correctifs de sécurité étendus consiste à migrer vers Azure. Pour les charges de travail variables, nous recommandons aux clients de migrer sur Azure via le Paiement à l’utilisation, ce qui permet une mise à l’échelle à tout moment. Pour les charges de travail prévisibles, nous recommandons aux clients de migrer vers Azure via un abonnement serveur et des instances réservées.
  
**Cette offre s’applique-t-elle également à SQL Server 2005 ?**

Non. Pour ces anciennes versions, nous vous recommandons de procéder à une mise à niveau vers les versions les plus récentes, mais les clients peuvent effectuer une mise à niveau vers les versions [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] pour tirer parti de cette offre.

**Puis-je déployer une nouvelle instance SQL Server 2008 ou 2008 R2 sur Azure tout en obtenant des correctifs de sécurité étendus ?**

Oui, les clients peuvent démarrer une nouvelle instance de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] sur une machine virtuelle Azure et avoir accès aux correctifs de sécurité étendus.

**Puis-je obtenir un support technique local pour SQL Server 2008 ou 2008 R2 après la date de la fin du support, sans acheter de correctifs de sécurité étendus ?**

Non. Si un client a [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] ou [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] et qu’il choisit de rester en local au cours d’une migration sans correctif de sécurité étendu, il ne peut pas enregistrer de ticket de support, même s’il dispose d’un plan de support. Toutefois, en migrant vers Azure, ils peuvent bénéficier d’un support à l’aide de leur plan de support Azure.

**Si un client SQL Server 2008 et 2008 R2 souhaite apporter sa propre licence (BYOL), est-il tenu de disposer de la couverture Software Assurance ?**

Oui, les clients doivent disposer de Software Assurance pour tirer parti du programme BYOL pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur les machines virtuelles Azure dans le cadre du programme mobilité de licence. Pour les clients sans Software Assurance, nous recommandons aux clients de passer à Azure SQL Managed Instance pour leurs environnements [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)]. Les clients peuvent également migrer vers des machines virtuelles Azure de paiement à l’utilisation. Les clients Software assurance qui possèdent une licence SQL par cœur ont également la possibilité de migrer vers Azure à l’aide de Azure Hybrid Benefit (AHB).

Azure SQL Managed Instance est un service Azure qui fournit une compatibilité d’environ 100 % avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en local. Managed Instance fournit des fonctionnalités de haute disponibilité et de récupération d’urgence intégrées, ainsi que des fonctionnalités de performances intelligentes et la possibilité de mettre à l’échelle à la volée. Managed Instance offre également une expérience sans version qui élimine le besoin de mises à niveau et de mises à jour correctives de sécurité manuelles. Pour plus d’informations sur le programme BYOL, consultez la page de conseils sur la tarification Azure.

**Quelles sont les options dont les clients ont besoin pour s’exécuter SQL Server dans Azure ?**

Les clients peuvent déplacer des environnements hérités [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers Azure SQL Managed Instance, un service de plateforme de données (PaaS) entièrement managé qui offre une option « sans version » pour éliminer les problèmes liés aux dates de la fin du support ou aux machines virtuelles Azure pour accéder aux correctifs de sécurité. Les bases de données migrées conserveront leur compatibilité avec le système hérité. Pour plus d’informations, consultez [Certification de compatibilité](../../database-engine/install-windows/compatibility-certification.md).

Les correctifs de sécurité étendus seront disponibles pour [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] dans des machines virtuelles Azure après la date de la fin du support le 9 juillet 2019, pour les trois années à venir. Pour les clients cherchant à effectuer une mise à niveau à partir de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)], toutes les versions ultérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] seront prises en charge. Pour [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] via [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], les clients doivent se trouver sur le dernier Service Pack pris en charge. À partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], il est recommandé aux clients d’utiliser la dernière mise à jour cumulative. Notez que les Service Packs ne seront pas disponibles à partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], mais uniquement des mises à jour cumulatives et des versions de distribution générale (GDR).

Azure SQL Managed Instance est une option de déploiement étend à l’instance dans [!INCLUDE[ssSDS](../../includes/sssds-md.md)] qui offre la compatibilité la plus étendue avec le moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et le support du réseau virtuel (VNET) natif, vous pouvez ainsi migrer des bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers Managed Instance sans modifier les applications. Il combine la surface d’exposition riche [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] avec les avantages opérationnels et financiers d’un service intelligent et entièrement géré. Tirez parti du nouveau Azure Database Migration Service pour déplacer [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] vers Azure SQL Managed Instance avec peu ou pas de modifications de code d’application.

**Les clients peuvent-ils tirer parti de Azure Hybrid Benefit pour les versions SQL Server 2008 et 2008 R2 ?**

Oui, les clients disposant d’un contrat Software Assurance actif ou d’abonnements serveur équivalents peuvent tirer parti de Azure Hybrid Benefit à l’aide d’investissements de licences locaux existants pour bénéficier d’une tarification à prix réduit sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutant sur [!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] et machines Virtuelles Azure.

**Les clients peuvent-ils bénéficier de correctifs de sécurité étendus gratuits sur les régions Azure Government ?**

Oui, les correctifs de sécurité étendus seront disponibles sur les machines virtuelles Azure sur les régions Azure Government.

**Les clients peuvent-ils bénéficier de correctifs de sécurité étendus gratuits sur Azure Stack ?**

Oui, les clients peuvent migrer [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et Windows Server 2008 et [!INCLUDE[ssKilimanjaro](../../includes/ssKilimanjaro-md.md)] vers Azure Stack et recevoir des correctifs de sécurité étendus sans coût supplémentaire après les dates de la fin du support.

**Pour les clients disposant d’un cluster SQL 2008 et 2008 R2 utilisant un stockage partagé, quels sont les conseils pour migrer vers Azure ?**

Actuellement, Azure ne prend pas en charge le clustering de stockage partagé du support. Pour obtenir des conseils sur la configuration d’une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à haut niveau de disponibilité sur Azure, reportez-vous au [Guide Haute disponibilité SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-high-availability-dr).

**Les clients peuvent-ils tirer parti des correctifs de sécurité étendus pour SQL Server avec un hébergeur tiers ?**

Les clients ne peuvent pas tirer parti des correctifs de sécurité étendus s’ils déplacent leur environnement [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] vers une implémentation PaaS sur d’autres fournisseurs de cloud. Si les clients cherchent à migrer vers des machines virtuelles (IaaS), ils peuvent tirer parti de mobilité de licence pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] via Software Assurance pour effectuer le déplacement et acheter des correctifs de sécurité étendus à partir de [!INCLUDE[msCoName](../../includes/msconame-md.md)] pour appliquer manuellement les correctifs aux instances de [!INCLUDE[ssKatmai](../../includes/ssKatmai-md.md)] en cours d’exécution dans une machine virtuelle (IaaS) sur le serveur de l’hébergeur SPLA autorisé. Toutefois, les mises à jour gratuites dans Azure sont l’offre la plus intéressante.

**Quelles sont les meilleures pratiques pour optimiser les performances de SQL Server dans les machines virtuelles Azure ?** 

Pour obtenir des conseils sur la façon d’optimiser les performances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur des machines virtuelles Azure, consultez le [Guide d’optimisation SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance). 

## <a name="see-also"></a>Voir aussi

- [Page de cycle de vie de SQL Server 2008 / 2008 R2](https://support.microsoft.com/lifecycle/search?alpha=sql%20server%202008)
- [Page de fin du support SQL Server 2008 / 2008 R2 ](./sql-server-end-of-life-overview.md?WT.mc_id=akamseos)
- [Forum aux questions (FAQ) sur les correctifs de sécurité étendus](https://aka.ms/sqleosfaq)
- [Centre de réponse aux problèmes de sécurité Microsoft (MSRC)](https://portal.msrc.microsoft.com/security-guidance/summary)
- [Gérer les mises à jour Windows à l’aide d’Azure Automation](/azure/automation/update-management/overview)
- [Mise à jour corrective automatisée de SQL Server](/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-automated-patching)
- [Guide de migration de données Microsoft](https://datamigration.microsoft.com/)
- [Migration Azure : options lift-and-shift pour déplacer votre SQL Server 2008 / 2008 R2 actuel vers une machine virtuelle Azure](https://azure.microsoft.com/services/azure-migrate/)
- [Infrastructure d’adoption du cloud pour la migration SQL](/azure/cloud-adoption-framework/migrate/expanded-scope/sql-migration)
- [Scripts relatifs aux ESU sur GitHub](https://github.com/microsoft/sql-server-samples/tree/master/samples/manage/sql-server-extended-security-updates/scripts)