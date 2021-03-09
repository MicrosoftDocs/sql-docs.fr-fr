---
title: Cycle de vie de prise en charge du pilote SqlClient
description: Page contenant des informations sur le cycle de vie du support technique.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 998fc5eecfa0e8840111b1ee9bf1d9e653ac5687
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464730"
---
# <a name="sqlclient-driver-support-lifecycle"></a>Cycle de vie de prise en charge du pilote SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

La bibliothèque Microsoft.Data.SqlClient suit la dernière stratégie de prise en charge de .NET Core pour toutes les versions.

[Afficher la stratégie de prise en charge de .NET Core](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)

## <a name="microsoftdatasqlclient-release-cadence"></a>Cadence de publication de Microsoft.Data.SqlClient

Les nouvelles versions stables (GA) seront publiées tous les six mois sur une cadence normale à partir de la version 1.2, avec en plus de 2 à 3 préversions entre elles. Les versions avec prise en charge à long terme (LTS) seront choisies par les parties prenantes et les responsables de maintenance en fonction de quelques qualifications et de la réponse des clients.

### <a name="actively-supported-releases"></a>Mises en production activement prises en charge

| Version | Date de publication officielle | Dernière version du correctif | Date de publication du correctif | Niveau de prise en charge  | Fin du support |
| -- | -- | -- | -- | -- | -- |
| 2.1 | 19 novembre 2020 | 2.1.2 | 3 mars 2021 | LTS | 20 novembre 2023 |
| 1.1 | 20 novembre 2019 | 1.1.3 | 15 mai 2020 | LTS | 21 novembre 2022 |

### <a name="out-of-support-releases"></a>Mises en production non prises en charge

| Version | Date de mise en production du dernier patch | Dernière version du correctif | Prise en charge terminée |
| -- | -- | -- | -- |
| 2.0 | 16 juin 2020 | 2.0.1 | 25 août 2020 |
| 1.0 | 26 septembre 2019 | 1.0.19269.1 | 20 février 2020 |


## <a name="azure-key-vault-provider-release-cadence"></a>Cadence des versions release d’Azure Key Vault Provider

De nouvelles versions en disponibilité générale stables de `Microsoft.Data.SqlClient.AlwaysEncrypted.AzureKeyVaultProvider` sont publiées à la demande quand de nouvelles fonctionnalités sont ajoutées. Les versions avec prise en charge à long terme (LTS) seront choisies par les parties prenantes et les responsables de maintenance en fonction de quelques qualifications et de la réponse des clients.

### <a name="actively-supported-releases"></a>Mises en production activement prises en charge

| Version | Date de publication officielle | Dernière version du correctif | Date de publication du correctif | Niveau de prise en charge  | Fin du support |
| -- | -- | -- | -- | -- | -- |
| 2.x | 3 mars 2021 | 2.0.0 | 3 mars 2021 | LTS | 4 mars 2024 |
| 1.x | 19 novembre 2019 | 1.2.0 | 1er décembre 2020 | LTS | 21 novembre 2022 |


## <a name="long-term-support-lts-releases"></a>Mises en production de la prise en charge à long terme (LTS)

Les versions LTS sont prises en charge pendant trois ans après la version initiale.

## <a name="current-releases"></a>Versions actuelles

Les versions actuelles sont prises en charge pendant trois mois après la publication d’une version actuelle ou LTS postérieure.


## <a name="sql-version-compatibility-with-microsoftdatasqlclient"></a>Compatibilité des versions SQL avec Microsoft.Data.SqlClient

|Version de la base de données&nbsp;&#8594;<br />Version du pilote &#8595;|Azure SQL Database|Azure Synapse Analytics|Azure SQL Managed Instance|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|
|---|---|---|---|---|---|---|---|---|
|2.1|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|2.0|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|1.1|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|1.0|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|

## <a name="supported-os-versions"></a>Versions du système d'exploitation prises en charge

### <a name="support-for-net-framework-applications"></a>Support pour les applications .NET Framework

Microsoft.Data.SqlClient prend en charge tous les systèmes d’exploitation pris en charge par .NET Framework v4.6 et versions ultérieures.

[Configuration requise de .NET Framework](/dotnet/framework/get-started/system-requirements).

### <a name="support-for-net-core-applications"></a>Support pour les applications .NET Core

Microsoft.Data.SqlClient prend en charge tous les systèmes d’exploitation pris en charge par .NET Core v2.1 et versions ultérieures.

[Stratégie de cycle de vie du système d’exploitation .NET Core pris en charge](https://github.com/dotnet/core/blob/master/os-lifecycle-policy.md).

> [!NOTE]
> Le mode de globalisation invariant n’est pas pris en charge actuellement.
