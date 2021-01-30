---
description: Cancel, exemple de méthode (VC++)
title: Cancel, exemple de méthode (VC + +) | Microsoft Docs
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
- Cancel method [ADO], VC++ example
ms.assetid: 7e0eaa39-0c24-4d8c-87e8-f9c4fd3455e7
author: rothja
ms.author: jroth
ms.openlocfilehash: 32f67f8b6208dd6cb45fdc51434520c66f44bf32
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99167807"
---
# <a name="cancel-method-example-vc"></a>Cancel, exemple de méthode (VC++)
Cet exemple utilise la méthode [Cancel](./cancel-method-ado.md) pour annuler une commande s’exécutant sur un objet [Connection](./connection-object-ado.md) si la connexion est occupée.  
  
```  
// CancelMethodExample.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include<conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void CancelX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   CancelX();  
   ::CoUninitialize();  
}  
  
void CancelX() {  
   // Define ADO object pointers.  Initialize pointers on define.  
   // These are in the ADODB::  namespace  
   _ConnectionPtr pConnection = NULL;  
  
   // Define Other Variables  
   _bstr_t strCmdChange;  
   _bstr_t strCmdRestore;  
   BOOL booChanged = FALSE;  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   try {  
      // open a connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      pConnection->Open(strCnn, "", "", adConnectUnspecified);  
  
      // Define command strings.  
      strCmdChange = "UPDATE titles SET type = 'self_help' WHERE type = 'psychology'";  
  
      strCmdRestore = "UPDATE titles SET type = 'psychology' WHERE type = 'self_help'";  
  
      // Begin a transaction, then execute a command asynchronously.  
      pConnection->BeginTrans();  
      pConnection->Execute(strCmdChange, NULL, adAsyncExecute);  
  
      // do something else for a little while - this could be changed  
      for ( int i = 1 ; i <= 100000 ; i++ )  
         printf("%d\n", i);        
  
      // If the command has NOT completed, cancel the execute and roll back the   
      // transaction. Otherwise, commit the transaction.  
      if ( (pConnection->GetState()) ) {  
         pConnection->Cancel();  
         pConnection->RollbackTrans();  
         booChanged = FALSE;  
         printf("Update canceled.\n");  
         printf("GetState = %d\n", pConnection->GetState());  
      }  
      else {  
         pConnection->CommitTrans();  
         booChanged = TRUE;  
         printf("Update complete.\n");  
      }  
  
      // If the change was made, restore the data because this is a demonstration.  
      if (booChanged) {  
         pConnection->Execute(strCmdRestore, NULL, 0);  
         printf("Data restored.\n");  
      }  
   }  
   catch(_com_error &e) {  
      // Notify user of any errors that result from executing the query.  
      // Pass a connection pointer accessed from the Connection.  
      PrintProviderError(pConnection);  
      PrintComError(e);  
   }  
  
   // Cleanup object before exit      
   if (pConnection)  
      if (pConnection->State == adStateOpen)  
         pConnection->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0 ) {  
      long nCount = pConnection->Errors->Count;  
  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("Error number: %x\t%s", pErr->Number, pErr->Description);  
      }  
   }  
}  
  
void PrintComError(_com_error &e) {  
   _bstr_t bstrSource(e.Source());  
   _bstr_t bstrDescription(e.Description());  
  
   // Print Com errors.    
   printf("Error\n");  
   printf("\tCode = %08lx\n", e.Error());  
   printf("\tCode meaning = %s\n", e.ErrorMessage());  
   printf("\tSource = %s\n", (LPCSTR) bstrSource);  
   printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Cancel, méthode (ADO)](./cancel-method-ado.md)   
 [Connection, objet (ADO)](./connection-object-ado.md)