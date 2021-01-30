---
description: BOF, EOF et Bookmark, exemple de propriétés (VC + +)
title: BOF, EOF et Bookmark, exemple de propriétés (VC + +) | Microsoft Docs
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
- Bookmark property [ADO], VC++ example
- EOF property [ADO], VC++ example
- BOF property [ADO], VC++ example
ms.assetid: bd2b9d85-e75e-4fc8-a392-076582019caa
author: rothja
ms.author: jroth
ms.openlocfilehash: 7cfc7d8ffa447beccf4597beb63f54d5ba958f29
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164778"
---
# <a name="bof-eof-and-bookmark-properties-example-vc"></a>BOF, EOF et Bookmark, exemple de propriétés (VC + +)
La première fonction de cet exemple utilise les propriétés [BOF](./bof-eof-properties-ado.md) et [EOF](./bof-eof-properties-ado.md) pour afficher un message si un utilisateur tente de se déplacer après le premier ou le dernier enregistrement d’un [jeu d’enregistrements](./recordset-object-ado.md). Elle utilise la propriété [Bookmark](./bookmark-property-ado.md) pour permettre à l’utilisateur d’indicateur un enregistrement dans un **jeu d’enregistrements** et de revenir ultérieurement.  
  
 La deuxième fonction utilise la propriété Bookmark pour placer le **signet** de chaque autre enregistrement d’un **jeu d’enregistrements** dans un tableau, puis filtre le recordset à l’aide du tableau.  
  
## <a name="example"></a>Exemple  
  
```  
// BOF_EOF_Bookmark.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include "conio.h"  
#include "icrsint.h"  
  
// class extracts only author id,fname,lastname  
class CEmployeeRs : public CADORecordBinding {  
   BEGIN_ADO_BINDING(CEmployeeRs)  
  
   // Column au_id is the 1st field in the recordset     
   ADO_VARIABLE_LENGTH_ENTRY2(1, adVarChar, m_szau_id, sizeof(m_szau_id), lau_idStatus, TRUE)  
  
    ADO_VARIABLE_LENGTH_ENTRY2(2, adVarChar, m_szau_lname, sizeof(m_szau_lname), lau_lnameStatus, TRUE)  
  
    ADO_VARIABLE_LENGTH_ENTRY2(3, adVarChar, m_szau_fname, sizeof(m_szau_fname), lau_fnameStatus, TRUE)  
  
END_ADO_BINDING()  
  
public:  
   CHAR m_szau_id[20];  
   ULONG lau_idStatus;  
   CHAR m_szau_fname[40];  
   ULONG lau_fnameStatus;  
   CHAR   m_szau_lname[40];  
   ULONG  lau_lnameStatus;  
};  
  
// Function declaration  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void AppendX();  
void PrintProviderError(_ConnectionPtr pConnection);  
  
int main() {  
   HRESULT hr = S_OK;  
  
   if (FAILED(::CoInitialize(NULL)))  
        return -1;  
  
    AppendX();  
    ::CoUninitialize();   
}  
  
void AppendX() {  
   HRESULT hr = S_OK;  
  
   // Define ADO object pointers. Initialize pointers on define. These are in the ADODB::  namespace.  
   _RecordsetPtr pRstByRoyalty = NULL;  
   _RecordsetPtr pRstAuthors = NULL;    
   _CommandPtr pcmdByRoyalty = NULL;  
   _ParameterPtr pprmByRoyalty = NULL;  
   _ConnectionPtr pConnection = NULL;  
  
   //Define Other variables  
   IADORecordBinding *picRs = NULL;   // Interface Pointer declared.(VC++ Extensions)     
   CEmployeeRs emprs;   // C++ class object      
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   _bstr_t strMessage, strAuthorID;  
  
   int intRoyalty;  
   VARIANT vtRoyalty;  
  
   try {  
      // Open a Connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      hr = pConnection->Open(strCnn, "", "", adConnectUnspecified);  
      pConnection->CursorLocation = adUseClient;  
  
      // Open Command Object with one Parameter  
      TESTHR(pcmdByRoyalty.CreateInstance(__uuidof(Command)));  
      pcmdByRoyalty->CommandText = "byroyalty";  
      pcmdByRoyalty->CommandType = adCmdStoredProc;  
  
      // Get parameter value and append parameter  
      printf("Enter Royalty: ");  
      scanf_s("%d", &intRoyalty);  
  
      // Define Integer/variant.  
      vtRoyalty.vt = VT_I2;  
      vtRoyalty.iVal = intRoyalty;  
      pprmByRoyalty = pcmdByRoyalty->CreateParameter("percentage", adInteger, adParamInput, sizeof(int), vtRoyalty);  
      pcmdByRoyalty->Parameters->Append(pprmByRoyalty);  
  
      pprmByRoyalty->Value = vtRoyalty;  
  
      // Create Recordset by executing the command  
      pcmdByRoyalty->ActiveConnection = pConnection;  
      pRstByRoyalty = pcmdByRoyalty->Execute(NULL, NULL, adCmdStoredProc);  
  
      // Open the authors table to get author names for display  
      TESTHR(pRstAuthors.CreateInstance(__uuidof(Recordset)));  
  
      // You have to explicitly pass the default Cursor type and LockType to the Recordset here  
      hr = pRstAuthors->Open("authors", _variant_t((IDispatch*)pConnection, true), adOpenForwardOnly, adLockReadOnly, adCmdTable);   
  
      // Open an IADORecordBinding interface pointer which we'll use for Binding Recordset to a class      
      TESTHR(pRstAuthors->QueryInterface(__uuidof(IADORecordBinding), (LPVOID*)&picRs));  
  
      // Bind the Recordset to a C++ Class here      
      TESTHR(picRs->BindToRecordset(&emprs));  
  
      // Print current data in the recordset, adding author names from author table.  
      printf("Authors with %d percent royalty ", intRoyalty);  
  
      while(!(pRstByRoyalty->EndOfFile)) {  
         strAuthorID = pRstByRoyalty->Fields->Item["au_id"]->Value;  
         pRstAuthors->Filter = "au_id = '"+strAuthorID+"'";  
  
         printf("\n"  "%s, %s  %s",emprs.lau_idStatus == adFldOK ? emprs.m_szau_id : "<NULL>",\  
            emprs.lau_fnameStatus == adFldOK ? emprs.m_szau_fname : "<NULL>",\  
            emprs.lau_lnameStatus == adFldOK ? emprs.m_szau_lname : "<NULL>");  
  
         pRstByRoyalty->MoveNext();   
      }  
   }  
   catch(_com_error &e) {  
      _bstr_t bstrSource(e.Source());  
      _bstr_t bstrDescription(e.Description());  
      PrintProviderError(pConnection);  
      printf("\n Source : %s \n Description : %s \n", (LPCSTR)bstrSource, (LPCSTR)bstrDescription);  
   }  
  
   // Clean up objects before exit.  Release the IADORecordset Interface here     
   if (picRs)  
      picRs->Release();  
  
   if (pRstByRoyalty)  
      if (pRstByRoyalty->State == adStateOpen)  
         pRstByRoyalty->Close();  
   if (pRstAuthors)  
      if (pRstAuthors->State == adStateOpen)  
         pRstAuthors->Close();  
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
    // Print Provider Errors from Connection object.  
    // pErr is a record object in the Connection's Error collection.  
    ErrorPtr pErr = NULL;  
    long nCount = 0;  
    long i = 0;  
  
    if ( (pConnection->Errors->Count) > 0) {  
        nCount = pConnection->Errors->Count;  
        // Collection ranges from 0 to nCount -1.  
        for (i = 0 ; i < nCount ; i++) {  
            pErr = pConnection->Errors->GetItem(i);  
            printf("Error number: %x\n Error Description: %s\n", pErr->Number, (LPCSTR) pErr->Description);  
        }  
    }  
}  
```  
  
## <a name="input"></a>Entrée  
  
```  
25  
```  
  
## <a name="sample-output"></a>Exemple de sortie  
  
```  
Authors with 25 percent royalty  
724-80-9391, Stearns  MacFeather  
899-46-2035, Anne  Ringer  
```  
  
## <a name="see-also"></a>Voir aussi  
 [BOF, EOF, propriétés (ADO)](./bof-eof-properties-ado.md)   
 [Bookmark, propriété (ADO)](./bookmark-property-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)