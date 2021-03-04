---
title: Découverte et classification des données dans SqlClient
description: Explique comment vérifier si une base de données SQL Server prend en charge la classification des données et comment accéder aux informations de classification des données au moyen d’un objet SqlDataReader.
ms.date: 11/23/2020
dev_langs:
- csharp
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: johnnypham
ms.author: v-jopha
ms.reviewer: v-daenge
ms.openlocfilehash: bac8c09b862e43549ade3a6d98e34fdc97596131
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835766"
---
# <a name="data-discovery-and-classification-in-sqlclient"></a>Découverte et classification des données dans SqlClient

[!INCLUDE[appliesto-netfx-netcore-netst-md](../../../includes/appliesto-netfx-netcore-netst-md.md)]

[!INCLUDE[Driver_ADONET_Download](../../../includes/driver_adonet_download.md)]

[Découverte et classification des données](../../../relational-databases/security/sql-data-discovery-and-classification.md) est un ensemble de services avancés de découverte, de classification, d’étiquetage des données sensibles dans les bases de données et de création de rapports associés. SqlClient fournit une API permettant de mettre au jour les informations de découverte et de classification des données en lecture seule lorsque la source sous-jacente prend en charge la fonctionnalité. Ces informations sont accessibles par le biais de SqlDataReader.

Microsoft.Data.SqlClient v2.1.0 introduit le support des informations de `Sensitivity Rank` de la classification des données. `Sensitivity Rank` est un identificateur basé sur un ensemble prédéfini de valeurs qui définissent le rang de sensibilité. Il est utilisé par d’autres services tels que le service Advanced Threat Protection pour détecter les anomalies en fonction de leur rang. Les API de classification des données suivantes sont maintenant disponibles dans l’espace de noms Microsoft.Data.SqlClient.DataClassification :

```csharp
// New in Microsoft.Data.SqlClient v2.1.0
public enum SensitivityRank
{
    NOT_DEFINED = -1,
    NONE = 0,
    LOW = 10,
    MEDIUM = 20,
    HIGH = 30,
    CRITICAL = 40
}

public sealed class SensitivityClassification
{
  // Returns the sensitivity rank for the query associated with the active 'SqlDataReader'.
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the labels collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<Label> Labels;

  // Returns the information types collection for this 'SensitivityClassification' Object
  public ReadOnlyCollection<InformationType> InformationTypes;

  // Returns the column sensitivity for this 'SensitivityClassification' Object
  public ReadOnlyCollection<ColumnSensitivity> ColumnSensitivities;
}

public sealed class SensitivityProperty
{
  // Returns the sensitivity rank for this 'SensitivityProperty' Object
  // New in Microsoft.Data.SqlClient v2.1.0
  public SensitivityRank SensitivityRank;

  // Returns the label for this 'SensitivityProperty' Object
  public Label Label;

  // Returns the information type for this 'SensitivityProperty' Object
  public InformationType InformationType;
}

public sealed class Label
{
  // Gets the name for this 'Label' object
  public string Name;

  // Gets the ID for this 'Label' object
  public string Id;
}

public sealed class InformationType
{
  // Gets the name for this 'InformationType' object
  public string Name;

  // Gets the ID for this 'InformationType' object
  public string Id;
}

public sealed class ColumnSensitivity
{
  // Returns the list of sensitivity properties as received from Server for this 'ColumnSensitivity' information      
  public ReadOnlyCollection<SensitivityProperty> SensitivityProperties;
}
```

> [!NOTE]
> Microsoft.Data.SqlClient lit des informations `Sensitivity Rank` uniquement si SQL Server prend en charge la classification des données par ordre de priorité. Pour les serveurs qui utilisent l’ancienne version de la classification des données sans classement, la valeur de classement des requêtes est « NON DÉFINI ».

Cet exemple d’application montre comment accéder aux propriétés de classification des données de SqlDataReader.

[!code-csharp [SqlDataReader_DataDiscoveryAndClassification#1](~/../sqlclient/doc/samples/SqlDataReader_DataDiscoveryAndClassification.cs#1)]


**Voir aussi**  

 - [Fonctionnalités de SQL Server et ADO.NET](sql-server-features-adonet.md)
 - [sys.sensitivity_classifications (Transact-SQL)](../../../relational-databases/system-catalog-views/sys-sensitivity-classifications-transact-sql.md)
 - [ADD SENSITIVITY CLASSIFICATION](../../../t-sql/statements/add-sensitivity-classification-transact-sql.md)