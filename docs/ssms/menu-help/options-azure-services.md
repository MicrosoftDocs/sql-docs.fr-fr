---
title: Page Options de SQL Server - Services Azure
description: Options (services Azure)
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Azure_Services.Azure_Cloud
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/15/2021
ms.openlocfilehash: 0983317d1d5c59e486764532708c566bb740000a
ms.sourcegitcommit: 23649428528346930d7d5b8be7da3dcf1a2b3190
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/15/2021
ms.locfileid: "98245702"
---
# <a name="options-azure-services"></a>Options (services Azure)

Utilisez cette page pour spécifier les options relatives aux services cloud Azure. Pour accéder à cette boîte de dialogue, accédez à **Outils > Options > Services Azure** à partir de la barre de menus supérieure.

## <a name="miscellaneous"></a>Divers

| Option | Information | Description |
|--------|-------------|-------------|
| Niveau de trace de Fenêtre Sortie ADAL | **Information** <br> **Aucun** <br> **Verbose** <br> **Avertissement** | Le niveau de traçage de connexion Azure Active Directory (AAD) à envoyer à la fenêtre Sortie. |
| URL du Portail Azure Data Factory | `https://adf.azure.com` | Spécifie l’URL du Portail Azure Data Factory. |
| Activer l’accès conditionnel à Azure SQL Database (EXPÉRIMENTAL) | **True** <br> **False** | EXPÉRIMENTAL : Si la valeur est true, les requêtes des jetons d’accès pour Azure SQL Database qui incluent une revendication pour l’accès au serveur sélectionné. Le paramètre peut nécessiter un redémarrage de SSMS pour prendre effet. |
| Point de terminaison de la galerie | `https://gallery.azure.com` | Spécifie le point de terminaison de la galerie Resource Manager des modèles de déploiement. |
| Audience Graph | `https://graph.windows.net` | ID des ressources du point de terminaison Graph. |
| Point de terminaison Graph | `https://graph.windows.net` | Spécifie l’URL pour les requêtes Azure Active Directory Graph. |
| URL du Portail de gestion | `https://portal.azure.com` | Spécifie l’URL du Portail de gestion. |
| Publier l’URL du fichier de paramètres | `https://go.microsoft.com/fwlink/?LinkID=335839` | Spécifie l’URL à partir de laquelle le fichier `.publishsettings` peut être téléchargé. |
| Nom de principal du service SQL Database | `https://database.windows.net/` | Nom de principal du service (SPN) Azure SQL Database pour obtenir un jeton lors de l’utilisation de l’authentification AAD. Il s’agit également de l’audience du JSON Web Token (JWT) pour l’analyse/validation du JSON Web Token côté serveur (JWT). |

## <a name="resource-management"></a>Gestion des ressources

| Option | Information | Description |
|--------|-------------|-------------|
| Autorité Active Directory | `https://login.microsoftonline.com` | Spécifie l’autorité de base pour l’authentification Azure Active Directory (AAD). |
| ID de ressources du point de terminaison de service Active Directory | `https://management.core.windows.net` | Spécifie l’audience pour les jetons qui authentifient les requêtes Azure Resource Manager (ARM) ou les points de terminaison de management des services (RDFE). |
| Point de terminaison Resource Manager | `https://management.azure.com` | Spécifie l’URL pour les requêtes de gestion des ressources. |
| Point de terminaison de service | `https://management.core.windows.net` | Spécifie le point de terminaison des requêtes de management des services. |

## <a name="select-an-azure-cloud"></a>Sélectionnez un cloud Azure

| Option | Information | Description |
|--------|-------------|-------------|
| Nom | **Cloud Azure Chine** <br><br> **Cloud Azure** <br><br> **Cloud Azure allemand** <br><br> **Azure US Government** <br><br> **Personnalisée** | Sélectionnez le cloud Azure auquel Management Studio se connecte pour découvrir et gérer les ressources. Sélectionnez **Personnaliser** pour fournir vos propres URL et suffixes DNS. |

## <a name="service-addresses"></a>Adresses de service

| Option | Information | Description |
|--------|-------------|-------------|
| Point de terminaison de services Azure Data Lake | `azuredatalakeanalytics.net` | Spécifie le catalogue Azure Data Lake Analytics le catalogue et le suffixe du point de terminaison du travail. |
| Point de terminaison du système de fichiers Azure Data Lake Store | `azuredatalakestore.net` | Spécifie le suffixe du point de terminaison du système de fichiers Azure Data Lake Store. | 
| Audience Azure Key Vault | `https://vault.azure.net` | Spécifie l’audience pour les jetons d’accès qui autorisent les requêtes pour les services Key Vault. |
| Suffixe DNS Azure Key Vault | `vault.azure.net` | Spécifie le suffixe du nom de domaine pour les serveurs Azure Key Vault. |
| Suffixe DNS SQL Database | `database.windows.net` | Spécifie le suffixe du nom de domaine pour les serveurs Azure SQL Database. |
| Point de terminaison de stockage | `core.windows.net` | Spécifie le point de terminaison pour l’accès au stockage. L’option comprend, blobs, tables, files d’attente et stockages de fichiers. |
| Suffixe DNS Traffic Manager | `trafficmanager.net` | Spécifie le suffixe du nom de domaine pour les services Azure Traffic Manager. |