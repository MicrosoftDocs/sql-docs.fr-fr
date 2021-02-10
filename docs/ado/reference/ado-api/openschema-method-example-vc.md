---
description: OpenSchema, exemple de méthode (VC++)
title: OpenSchema, exemple de méthode (VC + +) | Microsoft Docs
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
- OpenSchema method [ADO], VC++ example
ms.assetid: 6f3da460-0f49-41e0-999d-a754ec1d887e
author: rothja
ms.author: jroth
ms.openlocfilehash: 14e33283ac05de6f8ad1864e87a6c66a5bfbafd8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100041329"
---
# <a name="openschema-method-example-vc"></a>OpenSchema, exemple de méthode (VC++)
Cet exemple utilise la méthode [OpenSchema](./openschema-method.md) pour afficher le nom et le type de chaque table dans la base de données ***pubs*** .  
  
```  
// OpenSchemaMethodExample.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <oleauto.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void OpenSchemaX();  
void OpenSchemaX2();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   OpenSchemaX();  
   OpenSchemaX2();  
   ::CoUninitialize();  
}  
  
void OpenSchemaX() {  
   // Define ADO object pointers.  Initialize pointers on define.  
   // These are in the ADODB::  namespace.  
   _ConnectionPtr pConnection = NULL;  
   _RecordsetPtr pRstSchema = NULL;  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   try {  
      // Open connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      pConnection->Open (strCnn, "", "", adConnectUnspecified);  
  
      pRstSchema = pConnection->OpenSchema(adSchemaTables);  
  
      while ( !(pRstSchema->EndOfFile) ) {  
         _bstr_t table_name = pRstSchema->Fields->GetItem("TABLE_NAME")->Value;  
  
         printf("Table Name: %s\n",(LPCSTR) table_name);  
  
         _bstr_t table_type = pRstSchema->Fields->GetItem("TABLE_TYPE")->Value;  
  
         printf("Table type: %s\n\n",(LPCSTR) table_type);  
  
         pRstSchema->MoveNext();           
      }  
   }  
   catch (_com_error &e) {  
      // Display errors, if any. Pass connection pointer accessed from the Connection.          
      PrintProviderError(pConnection);  
      PrintComError(e);  
   }  
  
   // Clean up objects before exit.  
   if (pRstSchema)  
      if (pRstSchema->State == adStateOpen)  
         pRstSchema->Close();  
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void OpenSchemaX2() {  
   // Define ADO object pointers. Initialize pointers on define.  
   // These are in the ADODB::  namespace.  
   _ConnectionPtr pConnection2 = NULL;  
   _RecordsetPtr pRstSchema = NULL;  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   try {  
      // Open connection.  
      TESTHR(pConnection2.CreateInstance(__uuidof(Connection)));  
      pConnection2->Open (strCnn, "", "", adConnectUnspecified);  
  
      // Create a safearray which takes four elements,and pass it as   
      // 2nd parameter in OpenSchema method.  
      SAFEARRAY FAR* psa = NULL;  
      SAFEARRAYBOUND rgsabound;  
      _variant_t  var;  
      _variant_t  Array;  
      rgsabound.lLbound = 0;  
      rgsabound.cElements = 4;  
      psa = SafeArrayCreate(VT_VARIANT, 1, &rgsabound);  
      var.vt = VT_EMPTY;  
  
      long ix;  
      ix = 0;  
      SafeArrayPutElement(psa, &ix, &var);  
  
      ix= 1;  
      SafeArrayPutElement(psa, &ix, &var);  
  
      ix = 2;  
      SafeArrayPutElement(psa, &ix, &var);  
  
      var.vt = VT_BSTR;  
      char * s1 = "VIEW";  
      _bstr_t str = s1;  
      var.bstrVal = str;  
  
      ix = 3;  
      SafeArrayPutElement(psa, &ix, &var);  
  
      Array.vt = VT_ARRAY|VT_VARIANT;  
      Array.parray = psa;    
  
      pRstSchema = pConnection2->OpenSchema(adSchemaTables,&Array);  
  
      while(!(pRstSchema->EndOfFile))  
      {  
         printf("Table Name: %s\n",   
            (LPCSTR) (_bstr_t) pRstSchema->Fields->GetItem("TABLE_NAME")->Value);  
  
         printf("Table type: %s\n\n",  
            (LPCSTR) (_bstr_t) pRstSchema->Fields->GetItem("TABLE_TYPE")->Value);  
  
         pRstSchema->MoveNext();           
      }  
   }    // End Try statement.  
   catch (_com_error &e) {  
      // Display errors, if any. Pass connection pointer accessed from the Connection.  
      PrintProviderError(pConnection2);  
      PrintComError(e);  
   }  
  
   // Clean up objects before exit.  
   if (pRstSchema)  
      if (pRstSchema->State == adStateOpen)  
         pRstSchema->Close();  
   if (pConnection2)  
      if (pConnection2->State == adStateOpen)  
         pConnection2->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0) {  
      long nCount = pConnection->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("\t Error number: %x\t%s", pErr->Number, pErr->Description);  
      }  
   }  
}  
  
void PrintComError(_com_error &e) {  
   _bstr_t bstrSource(e.Source());  
   _bstr_t bstrDescription(e.Description());  
  
   // Print COM errors.   
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [OpenSchema, méthode](./openschema-method.md)