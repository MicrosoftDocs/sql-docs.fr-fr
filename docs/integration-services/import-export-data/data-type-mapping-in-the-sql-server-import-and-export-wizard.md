---
description: Mappage de type de données dans l’Assistant Importation et Exportation SQL Server
title: Mappage de type de données dans l’Assistant Importation et Exportation SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 01/11/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 669be403-cb17-4b12-bbbf-e7a74003c4b6
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 946bb57a3d821186ebcca132539713cf515ab20f
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88484086"
---
# <a name="data-type-mapping-in-the-sql-server-import-and-export-wizard"></a>Mappage de type de données dans l’Assistant Importation et Exportation SQL Server

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


 Dans l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , vous pouvez définir le nom, le type de données et les propriétés de type de données des colonnes des nouveaux fichiers et tables de destination, mais vous ne pouvez pas spécifier de conversions personnalisées pour les valeurs de colonnes. Le mappage intégré des types de données à partir de la source à la destination est donc important.  
  
##  <a name="how-does-the-wizard-map-data-types-between-source-and-destination"></a><a name="wizardMapping"></a> Comment l’Assistant mappe-t-il les types de données entre la source et de destination ?
L’Assistant utilise les fichiers de mappage installés par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour mapper les types de données à partir d’une système ou d’une version de base de données à un autre. Par exemple, il peut mapper les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] aux types de données Oracle. Par défaut, les fichiers de mappage au format XML sont installés dans les dossiers suivants.
-   **C:\Program Files\Microsoft SQL Server\130\DTSMappingFiles\\** (64 bits)
-   **C:\Program Files (x86)\Microsoft SQL Server\130\DTSMappingFiles\\** (32 bits).  
  
 Si vous modifiez un fichier de mappage existant ou ajoutez un nouveau fichier de mappage au dossier, vous devez fermer et rouvrir l’Assistant Importation et Exportation [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)] pour charger le fichier de mappage (nouveau ou modifié).  
 
## <a name="you-can-change-an-existing-mapping-file"></a>Vous pouvez modifier un fichier de mappage existant
Si votre entreprise nécessite différents mappages entre types de données, vous pouvez mettre à jour les fichiers de mappage pour modifier les mappages utilisés par l’Assistant. Par exemple, si vous voulez que le type de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **nchar** soit mappé au type de données DB2 **GRAPHIC** et non au type de données DB2 **VARGRAPHIC** pendant le transfert de données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers DB2, vous devez modifier le mappage **nchar** dans le fichier de mappage **qlClientToIBMDB2.xml** pour utiliser **GRAPHIC** à la place de **VARGRAPHIC**.  
  
## <a name="you-can-add-a-new-mapping-file"></a>Vous pouvez ajouter un nouveau fichier de mappage
[!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] installe les mappages entre de nombreuses combinaisons de source et de destination couramment utilisées. Vous pouvez également ajouter les nouveaux fichiers de mappage au répertoire **MappingFiles** pour prendre en charge d’autres sources et destinations. Les nouveaux fichiers de mappage doivent se conformer au schéma XSD publié et mapper une combinaison unique de source et de destination. Le schéma des fichiers de mappage, **DataTypeMapping.xsd**, est publié [ici](https://schemas.microsoft.com/sqlserver/2008/07/IntegrationServices/DataTypeMapping/DataTypeMapping.xsd).
 
## <a name="sample-mapping-file"></a>Exemple de fichier de mappage
Voici une partie du fichier de mappage XML qui mappe les types de données SQL Server (ou, plus spécifiquement, les types de données utilisés par le fournisseur de données .NET Framework pour SQL Server) aux types de données Oracle. Par exemple, vous pouvez voir qu’un type de données **int** SQL Server est mappé à un type de données **INTEGER** Oracle.
  
```xml  
  
<dtm:DataTypeMappings  
    xmlns:dtm="https://www.microsoft.com/SqlServer/Dts/DataTypeMapping.xsd"   
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"  
    SourceType="System.Data.SqlClient.SqlConnection"   
    MinSourceVersion="*"   
    MaxSourceVersion="*"   
    DestinationType="MSDAORA;OraOLEDB.Oracle;System.Data.OracleClient.OracleConnection"   
    MinDestinationVersion="08.*"   
    MaxDestinationVersion="*">  
  
    <!-- smallint -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>smallint</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
    <!-- int -->  
    <dtm:DataTypeMapping >  
        <dtm:SourceDataType>  
            <dtm:DataTypeName>int</dtm:DataTypeName>  
        </dtm:SourceDataType>  
        <dtm:DestinationDataType>  
            <dtm:SimpleType>  
                <dtm:DataTypeName>INTEGER</dtm:DataTypeName>  
            </dtm:SimpleType>  
        </dtm:DestinationDataType>  
    </dtm:DataTypeMapping>    
  
        ...  
  
</dtm:DataTypeMappings>  
  
```  

