---
description: Parameters (collection), Command (exemple de propriété) (VC++)
title: Parameters (collection), Command, exemple de propriété (VC + +) | Microsoft Docs
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
- Parameters collection [ADOX], VC++ example
- Command property [ADOX], VC++ example
ms.assetid: 8636fa08-b3db-4e9a-a918-585e76dd59c8
author: rothja
ms.author: jroth
ms.openlocfilehash: 159b5ed5da68a0ac9fe92b2c696bc3084066b27d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049879"
---
# <a name="parameters-collection-command-property-example-vc"></a>Parameters (collection), Command (exemple de propriété) (VC++)
L’exemple de code suivant montre comment utiliser la propriété [Command](./command-property-adox.md) avec l’objet [Command](../ado-api/command-object-ado.md) pour récupérer des informations sur les paramètres de la procédure.  
  
```  
// BeginProcedureParametersCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" rename("EOF", "EndOfFile")  
#import "msadox.dll" no_namespace  
  
#include "iostream"  
using namespace std;  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void ProcedureParametersX(void);  
  
int main() {  
    if ( FAILED(::CoInitialize(NULL)) )  
        return -1;  
  
    ProcedureParametersX();  
  
    ::CoUninitialize();  
}  
  
void ProcedureParametersX() {  
    HRESULT hr = S_OK;  
  
    // Define and initialize ADOX object pointers. These are in ADODB namespace.  
    _CatalogPtr m_pCatalog = NULL;  
  
    //Define ADODB object pointers.  
    ADODB::_ConnectionPtr m_pCnn = NULL;  
    ADODB::_CommandPtr m_pCommand = NULL;  
    ADODB::_ParameterPtr m_pParameter = NULL;  
  
    try {  
        TESTHR(hr = m_pCnn.CreateInstance(__uuidof(ADODB::Connection)));  
  
        // Open the Connection  
        m_pCnn->Open("Provider='Microsoft.JET.OLEDB.4.0';Data Source='c:\\Northwind.mdb';", "", "", NULL);  
  
        TESTHR(hr = m_pCatalog.CreateInstance(__uuidof(Catalog)));  
  
        // Open the catalog  
        m_pCatalog->PutActiveConnection(_variant_t((IDispatch *)m_pCnn));  
  
        // Get the command object  
        m_pCommand = m_pCatalog->Procedures->GetItem("CustomerById")->GetCommand();  
  
        _variant_t vIndex;  
  
        // Retrieve Parameter information  
        m_pCommand->Parameters->Refresh();  
        for ( long lIndex = 0 ; lIndex < m_pCommand->Parameters->Count ; lIndex ++ ) {  
            vIndex = lIndex;  
            m_pParameter = m_pCommand->Parameters->GetItem(vIndex);  
            cout << m_pParameter->Name << ":" << m_pParameter->Type << "\n" << endl;  
        }  
    }  
    catch(_com_error &e) {  
        // Notify the user of errors if any.  
        _bstr_t bstrSource(e.Source());  
        _bstr_t bstrDescription(e.Description());  
  
        printf("\n\tSource :  %s \n\tdescription : %s \n ", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
    }  
    catch(...) {  
        cout << "Error occurred in ProcedureParametersX...." << endl;  
    }  
}  
```