---
title: Utilisation d’un jeu de données externe avec Reporting Services | Microsoft Docs
description: Créez des extensions pour le traitement des données personnalisées dans Reporting Services afin de pouvoir intégrer des objets de jeux de données personnalisés créés par des applications externes.
ms.date: 03/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: extensions
ms.topic: reference
helpviewer_keywords:
- DataSet objects [Reporting Services]
- data processing extensions [Reporting Services], custom DataSet objects
- custom DataSet objects [Reporting Services]
- external DataSet objects [Reporting Services]
ms.assetid: 11daa013-ec17-4760-80e3-6d84cd8d5722
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: f34dee113abaf59f54db5983d39353bb45fc0641
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100060944"
---
# <a name="using-an-external-dataset-with-reporting-services"></a>Utilisation d'un dataset externe avec Reporting Services
  L’objet **DataSet** joue un rôle essentiel dans la prise en charge de scénarios de données déconnectés et distribués avec [!INCLUDE[vstecado](../../../includes/vstecado-md.md)]. L’objet **DataSet** est une représentation résidant en mémoire de données qui fournit un modèle de programmation relationnel cohérent indépendamment de la source de données. Il peut être utilisé avec plusieurs sources de données différentes, avec des données XML ou pour gérer des données locales à l'application. L’objet **DataSet** représente un jeu complet de données, y compris des tables associées, des contraintes et des relations entre les tables. En raison de la polyvalence de l’objet **DataSet** concernant le stockage et l’exposition des données, il est probable que vos données soient traitées et transformées en objet **DataSet** avant toute création de rapport sur ces données.  
  
 Avec les extensions pour le traitement des données [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)], vous pouvez intégrer n’importe quel objet **DataSet** personnalisé ayant été créé par des applications externes. Pour ce faire, vous créez une extension pour le traitement des données personnalisée dans [!INCLUDE[ssRSnoversion](../../../includes/ssrsnoversion-md.md)] qui agit comme un pont entre votre objet **DataSet** et le serveur de rapports. La majeure partie du code nécessaire pour traiter cet objet **DataSet** est contenue dans la classe **DataReader** que vous créez.  
  
 La première étape à suivre pour exposer votre objet **DataSet** au serveur de rapports consiste à implémenter une méthode spécifique au fournisseur dans votre classe **DataReader** capable de remplir un objet **DataSet**. L’exemple suivant indique comment charger des données statiques dans un objet **DataSet** en utilisant une méthode spécifique du fournisseur dans votre classe **DataReader**.  
  
```vb  
'Private members of the DataReader class  
Private m_dataSet As System.Data.DataSet  
Private m_currentRow As Integer  
  
'Method to create a dataset  
Friend Sub CreateDataSet()  
   ' Create a dataset.  
   Dim ds As New System.Data.DataSet("myDataSet")  
   ' Create a data table.   
   Dim dt As New System.Data.DataTable("myTable")  
   ' Create a data column and set various properties.   
   Dim dc As New System.Data.DataColumn()  
   dc.DataType = System.Type.GetType("System.Decimal")  
   dc.AllowDBNull = False  
   dc.Caption = "Number"  
   dc.ColumnName = "Number"  
   dc.DefaultValue = 25  
   ' Add the column to the table.   
   dt.Columns.Add(dc)  
   ' Add 10 rows and set values.   
   Dim dr As System.Data.DataRow  
   Dim i As Integer  
   For i = 0 To 9  
      dr = dt.NewRow()  
      dr("Number") = i + 1  
      ' Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr)  
   Next i  
  
   ' Fill the dataset.  
   ds.Tables.Add(dt)  
  
   ' Use a private variable to store the dataset in your  
   ' DataReader.  
   m_dataSet = ds  
  
   ' Set the current row to -1.  
   m_currentRow = - 1  
End Sub 'CreateDataSet  
```  
  
```csharp  
// Private members of the DataReader class  
private System.Data.DataSet m_dataSet;  
private int m_currentRow;  
  
// Method to create a dataset  
internal void CreateDataSet()  
{  
   // Create a dataset.  
   System.Data.DataSet ds = new System.Data.DataSet("myDataSet");  
   // Create a data table.   
   System.Data.DataTable dt = new System.Data.DataTable("myTable");  
   // Create a data column and set various properties.   
   System.Data.DataColumn dc = new System.Data.DataColumn();   
   dc.DataType = System.Type.GetType("System.Decimal");   
   dc.AllowDBNull = false;   
   dc.Caption = "Number";   
   dc.ColumnName = "Number";   
   dc.DefaultValue = 25;   
   // Add the column to the table.   
   dt.Columns.Add(dc);   
   // Add 10 rows and set values.   
   System.Data.DataRow dr;   
   for(int i = 0; i < 10; i++)  
   {   
      dr = dt.NewRow();   
      dr["Number"] = i + 1;   
      // Be sure to add the new row to the DataRowCollection.   
      dt.Rows.Add(dr);  
   }  
  
   // Fill the dataset.  
   ds.Tables.Add(dt);  
  
   // Use a private variable to store the dataset in your  
   // DataReader.  
   m_dataSet = ds;  
  
   // Set the current row to -1.  
   m_currentRow = -1;  
}  
```  
  
```csharp  
public bool Read()  
{  
   m_currentRow++;  
   if (m_currentRow >= m_dataSet.Tables[0].Rows.Count)   
   {  
      return (false);  
   }   
   else   
   {  
      return (true);  
   }  
}  
  
public int FieldCount  
{  
   // Return the count of the number of columns, which in  
   // this case is the size of the column metadata  
   // array.  
   get { return m_dataSet.Tables[0].Columns.Count; }  
}  
  
public string GetName(int i)  
{  
   return m_dataSet.Tables[0].Columns[i].ColumnName;  
}  
  
public Type GetFieldType(int i)  
{  
   // Return the actual Type class for the data type.  
   return m_dataSet.Tables[0].Columns[i].DataType;  
}  
  
public Object GetValue(int i)  
{  
   return m_dataSet.Tables[0].Rows[m_currentRow][i];  
}  
  
public int GetOrdinal(string name)  
{  
   // Look for the ordinal of the column with the same name and return it.  
   // Returns -1 if not found.  
   return m_dataSet.Tables[0].Columns[name].Ordinal;  
}  
```  
  
 Après avoir créé ou récupéré votre dataset, vous pouvez utiliser l’objet **DataSet** dans vos implémentations des membres **Read**, **GetValue**, **GetName**, **GetOrdinal**, **GetFieldType**, et **FieldCount** de la classe **DataReader**.  
  
## <a name="see-also"></a>Voir aussi  
 [Extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extensions.md)   
 [Implémentation d’une extension pour le traitement des données](../../../reporting-services/extensions/data-processing/implementing-a-data-processing-extension.md)   
 [Bibliothèque d'extensions Reporting Services](../../../reporting-services/extensions/reporting-services-extension-library.md)  
  
  
