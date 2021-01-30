---
description: Prepared, exemple de propriété (VC++)
title: Prepared, exemple de propriété (VC + +) | Microsoft Docs
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
- Prepared property [ADO], VC++ example
ms.assetid: f697ac1a-f125-42b5-bbf6-762a7fa30ae3
author: rothja
ms.author: jroth
ms.openlocfilehash: 8dbdff171e7c1052e84e080aa5a11d37ef9055e6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170536"
---
# <a name="prepared-property-example-vc"></a>Prepared, exemple de propriété (VC++)
Cet exemple illustre la propriété [préparée](./prepared-property-ado.md) en ouvrant deux objets de [commande](./command-object-ado.md) : l’un préparé et l’autre non préparé.  
  
## <a name="example"></a>Exemple  
  
```  
// Prepared_Property_Sample.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
#include <winbase.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void PreparedX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   PreparedX();  
   ::CoUninitialize();  
}  
  
void PreparedX() {  
   // Define ADO object pointers.  Initialize pointers on define.  
   // These are in the ADODB:: namespace.  
   _ConnectionPtr pConnection = NULL;  
   _CommandPtr pCmd1 = NULL;  
   _CommandPtr pCmd2 = NULL;  
  
   // Define string variables.    
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
   try {  
      // Open a connection.  
      TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
      pConnection->Open (strCnn, "", "", adConnectUnspecified);  
  
      _bstr_t strCmd ("SELECT title,type FROM titles ORDER BY type");  
  
      // Create two command objects for the same command; one prepared and one not.  
      TESTHR(pCmd1.CreateInstance(__uuidof(Command)));  
      pCmd1->ActiveConnection = pConnection;  
      pCmd1->CommandText = strCmd;  
  
      TESTHR(pCmd2.CreateInstance(__uuidof(Command)));  
      pCmd2->ActiveConnection = pConnection;  
      pCmd2->CommandText = strCmd;  
      pCmd2->PutPrepared(true);  
  
      // Set a timer,then execute the unprepared command 20 times.  
      DWORD sngStart = GetTickCount();   
  
      for ( int intLoop = 1 ; intLoop <= 20 ; intLoop++ )  
         pCmd1->Execute(NULL, NULL, adCmdText);  
  
      DWORD sngEnd=GetTickCount();   
      float sngNotPrepared = (float)(sngEnd - sngStart) / (float)1000;  
  
      // Reset the timer,then execute the prepared command 20 times  
      sngStart = GetTickCount();   
      for ( int intLoop = 1 ; intLoop <= 20 ; intLoop++ )  
         pCmd2->Execute(NULL, NULL, adCmdText);  
  
      sngEnd=GetTickCount();   
  
      float sngPrepared = (float)(sngEnd - sngStart) / (float)1000;  
  
      // Display performance results  
      printf("Performance Results:");  
      printf("\n\nNot Prepared: %6.3f seconds", sngNotPrepared);  
      printf("\nPrepared:     %6.3f seconds", sngPrepared);  
   }  
   catch (_com_error &e) {  
      // Display errors, if any. Pass a connection pointer accessed from the Connection.  
      PrintProviderError(pConnection);  
      PrintComError(e);  
   }  
  
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
  
 **Résultats des performances :**  
**Non préparé : 0,016 secondes**  
**Préparé : 0,016 secondes**   
## <a name="see-also"></a>Voir aussi  
 [Command, objet (ADO)](./command-object-ado.md)   
 [Prepared, propriété (ADO)](./prepared-property-ado.md)