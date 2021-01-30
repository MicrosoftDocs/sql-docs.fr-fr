---
description: DeleteRule, exemple de propriété (VC++)
title: DeleteRule, exemple de propriété (VC + +) | Microsoft Docs
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
- DeleteRule property [ADOX], VC++ example
ms.assetid: 7a1def31-2b6f-4542-aac3-ec35b54c89ef
author: rothja
ms.author: jroth
ms.openlocfilehash: 52d22de963b204daeaba12a319c3a54d59f3789b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99169415"
---
# <a name="deleterule-property-example-vc"></a>DeleteRule, exemple de propriété (VC++)
Cet exemple illustre la propriété [DeleteRule](./deleterule-property-adox.md) d’un objet [Key](./key-object-adox.md) . Le code ajoute une nouvelle [table](./table-object-adox.md) , puis définit une nouvelle clé primaire, en affectant à **DeleteRule** la valeur **adRICascade**.  
  
```  
// BeginDeleteRuleCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   HRESULT hr = S_OK;  
  
   // Define ADOX object pointers, initialize pointers. These are in ADODB namespace.  
   _KeyPtr m_pKeyPrimary = NULL;  
   _CatalogPtr m_pCatalog = NULL;  
   _TablePtr m_pTblNew = NULL;  
  
   // Define string variables.  
   _bstr_t strcnn("Provider='Microsoft.JET.OLEDB.4.0';Data source = 'c:\\Northwind.mdb';");  
   try {  
      TESTHR(hr = m_pKeyPrimary.CreateInstance(__uuidof(Key)));  
      TESTHR(hr = m_pCatalog.CreateInstance(__uuidof (Catalog)));  
      TESTHR(hr = m_pTblNew.CreateInstance(__uuidof(Table)));  
  
      // Connect the catalog.  
      m_pCatalog->PutActiveConnection(strcnn);  
  
      // Name new table.  
      m_pTblNew->Name = "NewTable";  
  
      // Append a numeric and a text field to new table.  
      m_pTblNew->Columns->Append("NumField", adInteger, 20);  
      m_pTblNew->Columns->Append("TextField", adVarWChar, 20);  
  
      // Append the new table.  
      m_pCatalog->Tables->Append(_variant_t((IDispatch*)m_pTblNew));  
      printf("Table 'NewTable' is added.\n");  
  
      // Define the Primary key.  
      m_pKeyPrimary->Name = "NumField";  
      m_pKeyPrimary->Type = adKeyPrimary;  
      m_pKeyPrimary->RelatedTable = "Customers";  
      m_pKeyPrimary->Columns->Append("NumField", adInteger,20);  
      m_pKeyPrimary->Columns->GetItem("NumField")->RelatedColumn = "CustomerId";  
      m_pKeyPrimary->DeleteRule = adRICascade;  
  
      // to pass an optional column parameter to Key's Apppend method  
      _variant_t vOptional;  
      vOptional.vt = VT_ERROR;  
      vOptional.scode = DISP_E_PARAMNOTFOUND;  
  
      // Append the primary key.  
      m_pCatalog->Tables->GetItem("NewTable")->Keys->Append( _variant_t((IDispatch*)m_pKeyPrimary),   
         adKeyPrimary,vOptional,   
         L"",   
         L"");  
  
      // Delete the table as this is a demonstration.  
      m_pCatalog->Tables->Delete(m_pTblNew->Name);  
      printf("Table 'NewTable' is deleted.\n");  
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
 [DeleteRule, propriété (ADOX)](./deleterule-property-adox.md)   
 [Key, objet (ADOX)](./key-object-adox.md)