---
title: Tâche Azure Data Lake Analytics | Microsoft Docs
description: Vous pouvez soumettre des travaux U-SQL au service Azure Data Lake Analytics avec la tâche Data Lake Analytics.
ms.custom: ''
ms.date: 06/27/2019
ms.prod: sql
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- SQL13.DTS.DESIGNER.AFPADLSTASK.F1
- SQL14.DTS.DESIGNER.AFPADLSTASK.F1
author: yanancai
ms.author: yanacai
ms.reviewer: maghan
ms.openlocfilehash: 2bc717a0dc59c0d880e7095e18a0c97432519e56
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346572"
---
# <a name="azure-data-lake-analytics-task"></a>Tâche Azure Data Lake Analytics

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Vous pouvez soumettre des travaux U-SQL au service Azure Data Lake Analytics avec la tâche Data Lake Analytics. Cette tâche est un composant du [pack de fonctionnalités SQL Server Integration Services (SSIS) pour Azure](../../integration-services/azure-feature-pack-for-integration-services-ssis.md).

Pour obtenir des informations générales, consultez [Azure Data Lake Analytics](https://azure.microsoft.com/services/data-lake-analytics/).

## <a name="configure-the-task"></a>Configurer la tâche

Pour ajouter une tâche Data Lake Analytics à un package, faites-la glisser de la boîte à outils SSIS à la zone de conception. Ensuite, double-cliquez sur la tâche, ou cliquez avec le bouton droit sur la tâche et sélectionnez **Modifier**. La boîte de dialogue **Éditeur du gestionnaire de connexions Azure Data Lake Analytics** s’ouvre. Vous pouvez définir les propriétés par le biais du concepteur SSIS ou par programmation.

## <a name="general-page-configuration"></a>Configuration de la page Général

Sur la page **Général**, configurez la tâche et fournissez le script U-SQL soumis par la tâche. Pour plus d’informations sur le langage U-SQL, consultez [Référence du langage U-SQL](/u-sql/).

### <a name="basic-configuration"></a>Configuration de base

Vous pouvez spécifier le nom et la description de la tâche.

### <a name="u-sql-configuration"></a>Configuration U-SQL

La configuration U-SQL comporte deux paramètres : **SourceType** et des options dynamiques qui dépendent de la valeur **SourceType**. 

**SourceType** spécifie la source du script U-SQL. Le script est soumis à un compte Data Lake Analytics pendant l’exécution du package SSIS. Les options de cette propriété sont :

|Valeur|Description|  
|-----------|-----------------|  
|**DirectInput**|Spécifie le script U-SQL par le biais de l’éditeur inline. Sélectionnez cette valeur pour afficher l’option dynamique **USQLStatement**.|  
|**FileConnection**|Spécifie un fichier .usql local contenant le script U-SQL. Sélectionnez cette option pour afficher l’option dynamique **FileConnection**.|  
|**Variable**|Spécifie une variable SSIS contenant le script U-SQL. Sélectionnez cette valeur pour afficher l'option dynamique **SourceVariable**.|
| &nbsp; | &nbsp; |

**Options dynamiques SourceType** : spécifient le contenu du script pour la requête U-SQL. 

|SourceType|Options dynamiques|  
|-----------|-----------------|  
|**SourceType = DirectInput**|Tapez directement la requête U-SQL à envoyer dans la zone des options, ou sélectionnez le bouton Parcourir (…) pour la taper dans la boîte de dialogue **Entrer une requête U-SQL**.|  
|**SourceType = FileConnection**|Sélectionnez un gestionnaire de connexions de fichiers existant ou <**Nouvelle connexion…** > pour créer une connexion de fichiers. Pour des informations connexes, consultez : [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md) et [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md).|  
|**SourceType = Variable**|Sélectionnez une variable existante, ou sélectionnez \<**New variable...**> pour en créer une. Pour des informations connexes, consultez [Integration Services &#40;SSIS&#41; Variables](../../integration-services/integration-services-ssis-variables.md) et [Ajouter une Variable](../integration-services-ssis-variables.md).|
| &nbsp; | &nbsp; |


### <a name="job-configuration"></a>Configuration des travaux
La configuration des travaux spécifie les propriétés de soumission des travaux U-SQL.

- **AzureDataLakeAnalyticsConnection :** compte Data Lake Analytics auquel le script U-SQL est soumis. Choisissez la connexion dans la liste des gestionnaires de connexions définis. Pour créer une connexion, sélectionnez <**Nouvelle connexion**>. Pour des informations connexes, consultez [Gestionnaire de connexions Azure Data Lake Analytics](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md).

- **JobName :** nom du travail U-SQL. 
- **AnalyticsUnits :** nombre d’unités analytiques du travail U-SQL.
- **Priority :** priorité du travail U-SQL. Vous pouvez le définir entre 0 et 1000. Plus le nombre est faible, plus la priorité est haute.
- **RuntimeVersion :** version du runtime Data Lake Analytics du travail U-SQL. Valeur par défaut : « default ». En général, il n’est pas nécessaire de modifier cette propriété.
- **Synchronous :** valeur booléenne indiquant si la tâche attend ou non la fin de l’exécution du travail. Si la valeur est définie sur true, la tâche est marquée en tant que **réussie** une fois le travail terminé. Si la valeur est définie sur false, la tâche est marquée en tant que **réussie** une fois la phase de préparation du travail terminée.

  |Valeur|Description|
  |-----------|-----------------|
  |True|Le résultat de la tâche dépend du résultat de l’exécution du travail U-SQL. Réussite du travail > la tâche réussit. Échec du travail > la tâche échoue. La tâche réussit ou échoue > la tâche se termine.|
  |False|Le résultat de la tâche dépend du résultat de la soumission et de la préparation du travail U-SQL. L’envoi du travail réussit et la phase de préparation est franchie > la tâche réussit. L’envoi du travail échoue ou le travail échoue lors de la phase de préparation > la tâche échoue. La tâche réussit ou échoue > la tâche se termine.|
  | &nbsp; | &nbsp; |

- **TimeOut :** délai d’attente en secondes de l’exécution du travail. Si le travail expire, il est annulé et marqué comme ayant échoué. Cette propriété n’est pas disponible si **Synchronous** est défini sur false.

## <a name="parameter-mapping-page-configuration"></a>Configuration de la page Mappage des paramètres

Sur la page **Mappage des paramètres** de la boîte de dialogue **Éditeur de tâches Azure Data Lake Analytics**, mappez les variables avec les paramètres (variables U-SQL) dans le script U-SQL.

- **Nom de la variable :** après avoir ajouté un mappage des paramètres en sélectionnant **Ajouter**, sélectionnez un système ou une variable définie par l’utilisateur dans la liste. Vous pouvez également sélectionner <**Nouvelle variable...** > pour ajouter une nouvelle variable avec la boîte de dialogue **Ajouter une Variable**. Pour des informations connexes, consultez [Integration Services &#40;SSIS&#41; Variables](../../integration-services/integration-services-ssis-variables.md).  

- **Nom du paramètre :** nom de paramètre/variable dans le script U-SQL. Vérifiez que le nom du paramètre commence par le signe \@, comme \@Param1. 

Voici un exemple montrant comment passer des paramètres au script U-SQL.

**Exemple de script U-SQL**
```
@searchlog =
    EXTRACT UserId          int,
            Start           DateTime,
            Region          string,
            Query           string,
            Duration        int,
            Urls            string,
            ClickedUrls     string
    FROM @in
    USING Extractors.Tsv(nullEscape:"#NULL#");

@rs1 =
    SELECT Start, Region, Duration
    FROM @searchlog
WHERE Region == "en-gb";

@rs1 =
    SELECT Start, Region, Duration
    FROM @rs1
    WHERE Start <= DateTime.Parse("2012/02/19");

OUTPUT @rs1   
    TO @out
      USING Outputters.Tsv(quoting:false, dateTimeFormat:null);
```

Notez que les chemins d’accès d’entrée et de sortie sont définis dans les paramètres **\@entrée** et **\@sortie**. Les valeurs des paramètres **\@entrée** et **\@sortie** dans le script U-SQL sont passées dynamiquement par la configuration du mappage des paramètres.

|Nom de la variable|Nom du paramètre|
|-------------|--------------|
|User: Variable1|\@entrée|
|User: Variable2|\@sortie| 
| &nbsp; | &nbsp; |

## <a name="expression-page-configuration"></a>Configuration de la page Expression

Vous pouvez attribuer toutes les propriétés dans la configuration de la page Général comme expression de propriété pour permettre une mise à jour dynamique de la propriété lors de l’exécution. Pour des informations connexes, consultez [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md).

## <a name="see-also"></a>Voir aussi
- [Gestionnaire de connexions Azure Data Lake Analytics](../../integration-services/connection-manager/azure-data-lake-analytics-connection-manager.md)
- [Tâche du système de fichiers Azure Data Lake Store](../../integration-services/control-flow/azure-data-lake-store-file-system-task.md)
- [Gestionnaire de connexions Azure Data Lake Store](../../integration-services/connection-manager/azure-data-lake-store-connection-manager.md)