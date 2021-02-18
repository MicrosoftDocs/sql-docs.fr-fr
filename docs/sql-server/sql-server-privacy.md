---
description: Avenant à la déclaration de confidentialité de SQL Server
title: Avenant à la déclaration de confidentialité de SQL Server | Microsoft Docs
ms.date: 11/11/2020
ms.prod: sql
ms.technology: release-landing
ms.reviewer: wopeter
ms.custom: ''
ms.topic: conceptual
f1_keywords: ''
helpviewer_keywords: ''
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 1821ebdd7c52d306a34be98f6e506de24c31539e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100341673"
---
# <a name="sql-server-privacy-supplement"></a>Avenant à la déclaration de confidentialité de SQL Server

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Cet article résume les fonctionnalités Internet qui peuvent collecter et envoyer à Microsoft des données anonymes de diagnostic et d’utilisation des fonctionnalités. SQL Server peut collecter des informations informatiques standard, et les données sur l’utilisation et les performances peuvent être transmises à Microsoft et analysées dans le but d’améliorer la qualité, la sécurité et la fiabilité du produit. Si vous installez SQL Server sur une machine virtuelle dans le service Microsoft Azure, des informations sur l’environnement peuvent être envoyées à Microsoft afin que Microsoft puisse installer l’extension de l’agent IaaS SQL Server sur votre machine virtuelle et inscrire votre ressource de machine virtuelle SQL auprès du fournisseur de ressources de machines virtuelles SQL (cf. [description](/azure/azure-sql/virtual-machines/windows/sql-vm-resource-provider-register)).

Cet article fait office d’addendum à la [déclaration de confidentialité Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839) générale. La classification des données dans cet article s’applique uniquement aux versions du produit SQL Server local. Elle ne s'applique pas aux éléments suivants :

- Azure SQL Database
- [SQL Server Management Studio (SSMS)](../ssms/sql-server-management-studio-telemetry-ssms.md)
- SQL Server Data Tools (SSDT)
- Azure Data Studio
- Assistant Migration de base de données
- Assistant Migration SQL Server
- Extension MS-SQL

Définition des *scénarios d’usages autorisés*. Dans le contexte de cet article, Microsoft définit les « Scénarios d’usages autorisés » comme des actions ou activités démarrées par Microsoft.

## <a name="access-control"></a>Contrôle d’accès

Détails des informations d’identification utilisées pour sécuriser les connexions, utilisateurs ou comptes au sein d’une installation de SQL Server.

### <a name="examples-of-access-control"></a>Exemples de contrôle d’accès

- Mots de passe
- Certificats

### <a name="permitted-usage-scenarios"></a>Scénarios d’usages autorisés

|Scénario |Restrictions d'accès |Conditions de rétention |
|---------|---------|---------|
|Ces informations d’identification ne quittent jamais l’ordinateur de l’utilisateur par le biais des données d’utilisation et de diagnostic. |- |- |
|Les images mémoire peuvent contenir des données de contrôle d’accès. |- |Images mémoire : 30 jours maximum. |
|Ces informations d’identification ne quittent jamais l’ordinateur de l’utilisateur via les Commentaires des utilisateurs, sauf si le client les indique manuellement |Limité à un usage interne Microsoft sans accès à des tiers. |Commentaires de l’utilisateur : 1 an maximum|
|&nbsp;|&nbsp;|&nbsp;|

## <a name="customer-data"></a>Données client

Les données client sont définies comme des données stockées dans les tables utilisateur, directement ou indirectement. Les données sont notamment des statistiques ou des littéraux d’utilisateur dans les textes de requête qui peuvent être stockés dans les tables utilisateur.

### <a name="examples-of-customer-data"></a>Exemples de données client

- Valeurs de données stockées dans les lignes d’une table utilisateur.
- Objets de statistiques contenant des copies des valeurs contenues dans les lignes d’une table utilisateur.
- Textes de requête contenant des valeurs littérales.

### <a name="permitted-usage-scenarios"></a>Scénarios d’usages autorisés

|Scénario  |Restrictions d'accès  |Conditions de rétention |
|---------|---------|---------|
|Ces données ne quittent jamais l’ordinateur de l’utilisateur par le biais des données d’utilisation et de diagnostic. |- |- |
|Les images mémoire peuvent contenir des données client et être envoyées à Microsoft. |- |Images mémoire : 30 jours maximum. |
|Les clients avec leur consentement peuvent envoyer via les Commentaires des utilisateurs des données client à Microsoft. |Limité à un usage interne Microsoft sans accès à des tiers. Microsoft peut exposer les données au client d’origine. |Commentaires de l’utilisateur : 1 an maximum |

## <a name="personal-data"></a>Données à caractère personnel

Données reçues d’un utilisateur ou générées par son utilisation du produit.
- Pouvant être liées à un utilisateur individuel.
- Ne contiennent pas de données client.

### <a name="examples-of-personal-data"></a>Exemples de données personnelles

- Identification de l’interface. Adresse IP complète
- Nom de l’ordinateur
- Noms de la connexion/de l’utilisateur
- Partie locale de l’adresse e-mail (joe@contoso.com)
- Informations de localisation
- Identification du client

### <a name="permitted-usage-scenarios"></a>Scénarios d’usages autorisés

|Scénario  |Restrictions d'accès  |Conditions de rétention|
|---------|---------|---------|
|Ces données ne quittent jamais l’ordinateur de l’utilisateur par le biais des données d’utilisation et de diagnostic. |- |- |
|Les images mémoire peuvent contenir des données personnelles et être envoyées à Microsoft. |- |Images mémoire : 30 jours maximum |
|L’ID d’identification du client peut être envoyé à Microsoft pour distribuer des nouvelles fonctionnalités hybrides et cloud auxquelles l’utilisateur s’est abonné. |- |Actuellement, ces fonctionnalités cloud ou hybrides n’existent pas.|
|Les clients avec leur consentement peuvent envoyer via les Commentaires des utilisateurs des données client à Microsoft.|Limité à un usage interne Microsoft sans accès à des tiers. Microsoft peut exposer les données au client d’origine. |Commentaires de l’utilisateur : 1 an maximum |

## <a name="internet-based-services-data"></a>Données des services Internet

Données nécessaires pour fournir des services Internet, d’après le CLUF de SQL Server.

### <a name="examples-of-internet-based-services-data"></a>Exemples de données de services Internet

- Informations sur les spécifications de l’ordinateur
- Nom/version du navigateur
- Version de SQL Server
- Code langue
- Adresse IP avec certains octets supprimés
- Données cartographiques

### <a name="permitted-usage-scenarios"></a>Scénarios d’usages autorisés

|Scénario  |Restrictions d'accès  |Conditions de rétention|
|---------|---------|---------| 
|Peut être utilisé par Microsoft pour améliorer les fonctionnalités et/ou corriger les bogues dans les fonctionnalités actuelles. |Limité à un usage interne Microsoft sans accès à des tiers. Microsoft peut exposer les données au client d’origine.  Par exemple, les tableaux de bord |90 jours minimum, 3 ans maximum |
|Les clients avec leur consentement peuvent envoyer via les Commentaires des utilisateurs des données client à Microsoft. |Limité à un usage interne Microsoft sans accès à des tiers. |Les clients avec leur consentement peuvent envoyer via les Commentaires des utilisateurs des données client à Microsoft. |
|Les éléments cartographiques Power View et SQL Reporting Services peuvent envoyer des données pour l’utilisation de Bing Maps. |Limité aux données de session |- |

## <a name="non-personal-data"></a>Données non personnelles

1. Données reçues d’une organisation ou générées par son utilisation du produit. Elles peuvent être liées à une organisation et ne contiennent pas de données client.

   - Exemple
     - Nom de l’organisation (par exemple : Microsoft Corp.)

   - Scénarios d’usages autorisés

     |Scénario  |Restrictions d'accès  |Conditions de rétention|
     |---------|---------|---------|
     | Microsoft peut collecter des données d’utilisation génériques des instances SQL Server s’exécutant dans Machines virtuelles Azure, dans le but exprès de fournir aux clients des avantages facultatifs dans Azure pour l’utilisation de SQL Server dans Machines virtuelles Azure. | Microsoft peut exposer les données au client, par exemple via le portail Azure, afin d’aider les clients qui exécutent SQL Server dans Machines virtuelles Azure à accéder aux avantages spécifiques à l’exécution de SQL Server dans Azure. </br></br>Microsoft n’utilisera pas ces données pour la gestion des licences d’audits sans le consentement préalable du client. | 90 jours minimum, 3 ans maximum |

2. Données qui sont utilisées pour décrire ou configurer des serveurs, des bases de données, des tables et autres ressources créées ou fournies par les clients. Elles comprennent notamment les noms des tables et des colonnes des bases de données, mais pas le contenu des lignes des bases de données ou autres données client. Les clients ne doivent pas placer de données personnelles dans ces champs ni créer d’applications conçues pour stocker des données personnelles dans ces champs. Pour le scénario d’usage autorisé ci-dessous, seul le formulaire de hachage est utilisé pour déterminer les modèles d’usage visant à améliorer le produit.

   - Exemple
      - Noms de base de données SQL Server
      - Noms de table et de colonne
      - Noms de statistiques

    - Scénarios d’usages autorisés

      > [!NOTE]
      > Toutes les valeurs de métadonnées sont hachées avant d’être collectées.
      >

      |Scénario  |Restrictions d'accès  |Conditions de rétention|
      |---------|---------|---------|
      |Peut être utilisé par Microsoft pour améliorer les fonctionnalités ou corriger les bogues dans les fonctionnalités actuelles. |Limité à un usage interne Microsoft sans accès à des tiers. |90 jours minimum, 3 ans maximum|

3. Données générées au cours de l’exécution du serveur. Elles ne contiennent pas de données client, de données non personnelles (listées dans les sections 1. ou 2. ci-dessus), de données de contrôle d’accès client ou de données personnelles.

   - Exemple
     - GUID de la base de données
     - Code de hachage du nom de l’ordinateur
     - Code de hachage du nom de l’instance
     - Nom de l'application
     - Données comportementales/d’utilisation
     - Données du programme d’amélioration du produit SQL (SQLCEIP)
     - Données de configuration du serveur, par exemple les paramètres de sp_configure
     - Données de configuration des fonctionnalités
     - Noms d’événement et codes d’erreur
     - Paramètres matériels et identification, tels que le fabricant OEM

   Microsoft examine les valeurs de nom d’application définies par d’autres programmes qui utilisent SQL Server (exemple : SharePoint ou des programmes packagés tiers) et inclut ces informations dans des champs de métadonnées envoyés à Microsoft quand les données d’utilisation sont activées. Les clients ne doivent pas placer de données personnelles dans ces champs de métadonnées ni créer d’applications conçues pour stocker des données personnelles dans ces champs.

   - Scénarios d’usages autorisés

     |Scénario  |Restrictions d'accès  |Conditions de rétention|
     |---------|---------|---------|
     |Peut être utilisé par Microsoft pour améliorer les fonctionnalités ou corriger les bogues dans les fonctionnalités actuelles.|Limité à un usage interne Microsoft sans accès à des tiers. |90 jours minimum, 3 ans maximum |
     |Peut être utilisé pour faire des suggestions au client.  Par exemple, « D’après votre utilisation du produit, utilisez la fonctionnalité *X* pour obtenir de meilleures performances ». |Microsoft peut exposer les données au client d’origine, par exemple, à travers des tableaux de bord. |Journaux de sécurité de données client : 3 ans minimum, 6 ans maximum |
     |Peut être utilisé par Microsoft pour la planification du produit futur. |Microsoft peut partager ces informations avec d’autres fournisseurs de matériel et de logiciel afin d’améliorer le fonctionnement de leurs produits exécutés avec les logiciels Microsoft. |90 jours minimum, 3 ans maximum|
     |Peut être utilisé par Microsoft pour fournir des services cloud basés sur les données d’utilisation et de diagnostic envoyées. Par exemple, le tableau de bord d’un client affichant l’utilisation des fonctionnalités sur toutes les installations de SQL Server dans une organisation. |Microsoft peut exposer les données au client d’origine, par exemple, à travers des tableaux de bord. |90 jours minimum, 3 ans maximum |
     |Les clients avec leur consentement peuvent envoyer via les Commentaires des utilisateurs des données client à Microsoft. |Limité à un usage interne Microsoft sans accès à des tiers. Microsoft peut exposer les données au client d’origine. |Commentaires de l’utilisateur : 1 an maximum |
     |Peut utiliser les noms de base de données et les noms d’application pour classer les bases de données et les applications dans des catégories connues, par exemple, celles susceptibles d’exécuter des logiciels fournis par Microsoft ou d’autres sociétés.|Limité à un usage interne Microsoft sans accès à des tiers.|90 jours minimum, 3 ans maximum |

## <a name="system-generated-logs-controls"></a>Contrôles des journaux générés par le système

Pour obtenir des instructions sur la façon dont les journaux générés par le système peuvent être activés ou désactivés dans le produit, consultez [Configurer les données d’utilisation et de diagnostic pour SQL Server (CEIP)](usage-and-diagnostic-data-configuration-for-sql-server.md).

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
