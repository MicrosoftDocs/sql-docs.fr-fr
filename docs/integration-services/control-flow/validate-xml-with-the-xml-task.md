---
description: Validate XML with the XML Task
title: Effectuer une validation XML avec la tâche XML | Microsoft Docs
ms.custom: ''
ms.date: 09/08/2020
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
helpviewer_keywords:
- XML validation
- XML, validating
ms.assetid: 224fc025-c21f-4d43-aa9d-5ffac337f9b0
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 97e0ea80dcc5cffefc99cbbc928eace94ade942a
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172591"
---
# <a name="validate-xml-with-the-xml-task"></a>Validate XML with the XML Task

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Validez des documents XML et obtenez une sortie d’erreur détaillée en activant la propriété **ValidationDetails** de la tâche XML.  
  
 La capture d’écran ci-après affiche l’**Éditeur de tâche XML** avec les paramètres requis pour la validation XML avec une sortie d’erreur détaillée.  
  
 ![Propriétés de la tâche XML dans l’Éditeur de tâche XML](../../integration-services/control-flow/media/xmltaskproperties.jpg "Propriétés de la tâche XML dans l’Éditeur de tâche XML")  
  
 Avant que la propriété **ValidationDetails** ne soit disponible, la validation XML par la tâche XML ne renvoyait qu’un résultat true ou false, sans aucune information sur les erreurs ou leur emplacement. À présent, quand vous définissez **ValidationDetails** sur True, le fichier de sortie contient des informations détaillées sur chaque erreur, notamment le numéro de ligne et la position. Vous pouvez utiliser ces informations pour comprendre, localiser et corriger les erreurs dans les documents XML.  
  
 La fonctionnalité de validation XML s’adapte aisément aux documents XML volumineux et aux nombres d’erreurs élevés. Étant donné que le fichier de sortie proprement dit présente le format XML, vous pouvez exécuter des requêtes sur la sortie et analyser cette dernière. Par exemple, si la sortie contient un grand nombre d’erreurs, vous pouvez regrouper les erreurs en exécutant une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] , comme décrit dans cette rubrique.  
  
> [!NOTE]
>  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ( [!INCLUDE[ssIS](../../includes/ssis-md.md)]) a introduit la propriété **ValidationDetails** dans [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] Service Pack 2. Cette propriété est également disponible dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] et dans [!INCLUDE[ssSQL15](../../includes/sssql16-md.md)].  
  
## <a name="sample-output-for-xml-thats-valid"></a>Exemple de sortie pour XML valide  
 Voici un exemple de fichier de sortie présentant les résultats de validation d’un fichier XML valide.  
  
```xml  
  
<root xmlns:ns="https://schemas.microsoft.com/xmltools/2002/xmlvalidation">  
    <metadata>  
        <result>true</result>  
        <errors>0</errors>  
        <warnings>0</warnings>  
        <startTime>2015-05-28T10:27:22.087</startTime>  
        <endTime>2015-05-28T10:29:07.007</endTime>  
        <xmlFile>d:\Temp\TestData.xml</xmlFile>  
        <xsdFile>d:\Temp\TestSchema.xsd</xsdFile>  
    </metadata>  
    <messages />  
</root>  
```  
  
## <a name="sample-output-for-xml-thats-not-valid"></a>Exemple de sortie pour XML non valide  
 Voici un exemple de fichier de sortie présentant les résultats de validation d’un fichier XML contenant un petit nombre d’erreurs. Le texte des éléments \<error> a été renvoyé à la ligne pour améliorer la lisibilité.  
  
```xml  
  
<root xmlns:ns="https://schemas.microsoft.com/xmltools/2002/xmlvalidation">  
    <metadata>  
        <result>false</result>  
        <errors>2</errors>  
        <warnings>0</warnings>  
        <startTime>2015-05-28T10:45:09.538</startTime>  
        <endTime>2015-05-28T10:45:09.558</endTime>  
        <xmlFile>C:\Temp\TestData.xml</xmlFile>  
        <xsdFile>C:\Temp\TestSchema.xsd</xsdFile>  
    </metadata>  
    <messages>  
        <error line="5" position="26">The 'ApplicantRole' element is invalid - The value 'wer3' is invalid  
    according to its datatype 'ApplicantRoleType' - The Enumeration constraint failed.</error>  
        <error line="16" position="28">The 'Phone' element is invalid - The value 'we3056666666' is invalid  
     according to its datatype 'phone' - The Pattern constraint failed.</error>  
    </messages>  
</root>  
```  
  
## <a name="analyze-xml-validation-output-with-a-transact-sql-query"></a>Analyser la sortie de validation XML avec une requête Transact-SQL  
 Si le résultat de la validation XML contient un grand nombre d’erreurs, vous pouvez utiliser une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] pour charger la sortie dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]. Vous pouvez ensuite analyser la liste d’erreurs avec toutes les fonctionnalités du langage T-SQL, notamment WHERE, GROUP BY, ORDER BY, JOIN, et ainsi de suite.  
  
```sql  
DECLARE @xml XML;  
  
SELECT @xml = XmlDoc     
FROM OPENROWSET (BULK N'C:\Temp\XMLValidation_2016-02-212T10-41-00.xml', SINGLE_BLOB) AS Tab(XmlDoc);  
  
-- Query # 1, flat list of errors  
-- convert to relational/rectangular  
;WITH XMLNAMESPACES ('https://schemas.microsoft.com/xmltools/2002/xmlvalidation' AS ns), rs AS  
(  
SELECT col.value('@line','INT') AS line  
     , col.value('@position','INT') AS position  
     , col.value('.','VARCHAR(1024)') AS error  
FROM @XML.nodes('/root/messages/error') AS tab(col)  
)  
SELECT * FROM rs;  
-- WHERE error LIKE '%whatever_string%'  
  
-- Query # 2, count of errors grouped by the error message  
-- convert to relational/rectangular  
;WITH XMLNAMESPACES ('https://schemas.microsoft.com/xmltools/2002/xmlvalidation' AS ns), rs AS  
(  
SELECT col.value('@line','INT') AS line  
     , col.value('@position','INT') AS position  
     , col.value('.','VARCHAR(1024)') AS error  
FROM @XML.nodes('/root/messages/error') AS tab(col)  
)  
SELECT COALESCE(error,'Total # of errors:') AS [error], COUNT(*) AS [counter]  
FROM rs  
GROUP BY GROUPING SETS ((error), ())  
ORDER BY 2 DESC, COALESCE(error, 'Z');  
  
```  
  
 Voici le résultat dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] du second exemple de requête figurant dans le texte précédent.  
  
 ![Erreurs XML de requête sur un groupe dans Management Studio](../../integration-services/control-flow/media/query-for-xml-errors.png "Erreurs XML de requête sur un groupe dans Management Studio")  
  
## <a name="see-also"></a>Voir aussi  
 [Tâche XML](../../integration-services/control-flow/xml-task.md)   
 [Éditeur de tâche XML &#40;page Général&#41;](./xml-task.md)  
  
