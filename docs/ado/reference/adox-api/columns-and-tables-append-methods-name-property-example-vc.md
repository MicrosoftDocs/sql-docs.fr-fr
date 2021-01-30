---
description: Append, méthode des collections Columns et Tables, Name (exemple de propriété) (VC++)
title: Columns et tables Append, méthodes, Name, exemple de propriété (VC + +) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
dev_langs:
- C++
helpviewer_keywords:
- Name property [ADOX], VC++ example
- Append method [ADOX], VC++ example
ms.assetid: 2b6dfef9-bcdf-483d-a164-2fa3ec81a43f
author: rothja
ms.author: jroth
ms.openlocfilehash: f5ee45e4bccfc82e5a4e1017aa2ebe24f80520d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99172243"
---
# <a name="columns-and-tables-append-methods-name-property-example-vc"></a>Append, méthode des collections Columns et Tables, Name (exemple de propriété) (VC++)
Le code suivant montre comment créer une nouvelle table.  
  
```  
// BeginCreateTableCpp.cpp  
// compile with: /EHsc  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL) ) )  
      return -1;  
  
   HRESULT hr = S_OK;  
  
   // Define ADOX object pointers, initialize pointers.  These are in ADOX namespace.  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTable = NULL;  
  
   try {  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
  
      // Open the catalog  
      m_pCatalog->PutActiveConnection("Provider='Microsoft.JET.OLEDB.4.0';data source='c:\\Northwind.mdb';");  
  
      TESTHR(hr = m_pTable.CreateInstance(__uuidof(Table)));  
      m_pTable->PutName("MyTable");  
      m_pTable->Columns->Append("Column1",adInteger,0);  
      m_pTable->Columns->Append("Column2",adInteger,0);  
      m_pTable->Columns->Append("Column3",adVarWChar,50);  
      m_pCatalog->Tables->Append(_variant_t((IDispatch *)m_pTable));  
      printf("Table 'MyTable' is added.\n");  
  
      // Delete the table as this is a demonstration.  
      m_pCatalog->Tables->Delete("MyTable");  
      printf("Table 'MyTable' is deleted.\n");  
   }  
  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
  
      printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
  
   catch(...) {  
      cout << "Error occurred in include files...."<< endl;  
   }  
  
   ::CoUninitialize();  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Append, méthode (colonnes ADOX)](./append-method-adox-columns.md)   
 [Append, méthode (Tables ADOX)](./append-method-adox-tables.md)   
 [Name, propriété (ADOX)](./name-property-adox.md)