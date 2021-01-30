---
description: ConnectionString, ConnectionTimeout et State, exemple de propriétés (VC + +)
title: Connection, exemple de propriétés (VC + +) | Microsoft Docs
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
- ConnectionString property [ADO], VC++ example
- ConnectionTimeout property [ADO], VC++ example
- State property [ADO], VC++ example
ms.assetid: c6bd2609-4c49-462f-a1aa-7bee0f615adb
author: rothja
ms.author: jroth
ms.openlocfilehash: 910c2cc35af9e13d89783602f2b63bf68b0af8c9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164596"
---
# <a name="connectionstring-connectiontimeout-and-state-properties-example-vc"></a>ConnectionString, ConnectionTimeout et State, exemple de propriétés (VC + +)
Cet exemple illustre différentes façons d’utiliser la propriété [ConnectionString](./connectionstring-property-ado.md) pour ouvrir un objet de [connexion](./connection-object-ado.md) . Elle utilise également la propriété [ConnectionTimeout](./connectiontimeout-property-ado.md) pour définir un délai d’attente de connexion et la propriété [State](./state-property-ado.md) pour vérifier l’état des connexions. La fonction GetState est requise pour l’exécution de cette procédure.  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.  
  
```  
// ConnectionStringSampleCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void ConnectionStringX();  
_bstr_t GetState(int intState);   
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
   if (FAILED(::CoInitialize(NULL)))  
      return 0;  
  
   ConnectionStringX();  
   ::CoUninitialize();  
}  
  
void ConnectionStringX() {  
   // Define Connection object pointers.  Initialize pointers on define.  These are in the ADODB::  namespace  
   _ConnectionPtr pConnection1 = NULL;  
   _ConnectionPtr pConnection2 = NULL;  
   _ConnectionPtr pConnection3 = NULL;  
   _ConnectionPtr pConnection4 = NULL;  
  
   // Define Other Variables  
   HRESULT hr = S_OK;  
  
   try {  
      // Open a connection using OLE DB syntax.  
      TESTHR(pConnection1.CreateInstance(__uuidof(Connection)));  
      pConnection1->ConnectionString = "Provider='sqloledb';Data Source='(local)';"  
         "Initial Catalog='Pubs';Integrated Security='SSPI';";  
      pConnection1->ConnectionTimeout = 30;  
      pConnection1->Open("", "", "",adConnectUnspecified);  
      printf("cnn1 state: %s\n", (LPCTSTR)GetState(pConnection1->State));  
  
      // Open a connection using a DSN and ODBC tags.  
      // It is assumed that you have create DSN 'DataPubs' with a user name as   
      // 'MyUserId' and password as 'MyPassword'.  
      TESTHR(pConnection2.CreateInstance(__uuidof(Connection)));  
      pConnection2->ConnectionString = "DSN=DataPubs;UID=MyUserId;PWD=MyPassword;";  
      pConnection2->Open("", "", "", adConnectUnspecified);  
      printf("cnn2 state: %s\n", (LPCTSTR)GetState(pConnection2->State));  
  
      // Open a connection using a DSN and OLE DB tags.  
      TESTHR(pConnection3.CreateInstance(__uuidof(Connection)));  
      pConnection3->ConnectionString = "Data Source=DataPubs;";  
      pConnection3->Open("", "", "", adConnectUnspecified);  
      printf("cnn3 state: %s\n", (LPCTSTR)GetState(pConnection3->State));  
  
      // Open a connection using a DSN and individual arguments instead of a connection string.  
      // It is assumed that you have create DSN 'DataPubs' with a user name as   
      // 'MyUserId' and password as 'MyPassword'.  
      TESTHR(pConnection4.CreateInstance(__uuidof(Connection)));  
      pConnection4->Open("DataPubs", "MyUserId", "MyPassword", adConnectUnspecified);  
      printf("cnn4 state: %s\n", (LPCTSTR)GetState(pConnection4->State));  
   }  
   catch(_com_error &e) {  
      // Notify user of any errors.  Pass a connection pointer accessed from the Connection.  
      PrintProviderError(pConnection1);  
      if (pConnection2)  
         PrintProviderError(pConnection2);  
  
      if (pConnection3)  
         PrintProviderError(pConnection3);  
  
      if (pConnection4)  
         PrintProviderError(pConnection4);  
  
      PrintComError(e);  
   }  
  
   // Cleanup objects before exit.  
   if (pConnection1)  
      if (pConnection1->State == adStateOpen)  
         pConnection1->Close();  
  
   if (pConnection2)  
      if (pConnection2->State == adStateOpen)  
         pConnection2->Close();  
  
   if (pConnection3)  
      if (pConnection3->State == adStateOpen)  
         pConnection3->Close();  
  
   if (pConnection4)  
      if (pConnection4->State == adStateOpen)  
         pConnection4->Close();  
}  
  
_bstr_t GetState(int intState) {  
   _bstr_t strState;   
   switch(intState) {  
   case adStateClosed:  
      strState = "adStateClosed";  
      break;  
   case adStateOpen:  
      strState = "adStateOpen";  
      break;  
   default:  
      ;  
   }  
   return strState;  
}  
  
void PrintProviderError(_ConnectionPtr pConnection) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr  pErr = NULL;  
  
   if ( (pConnection->Errors->Count) > 0) {  
      long nCount = pConnection->Errors->Count;  
  
      // Collection ranges from 0 to nCount -1.  
      for ( long i = 0 ; i < nCount ; i++ ) {  
         pErr = pConnection->Errors->GetItem(i);  
         printf("Error number: %x\t%s\n", pErr->Number, (LPCSTR)pErr->Description);  
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
 [Connection, objet (ADO)](./connection-object-ado.md)   
 [ConnectionString, propriété (ADO)](./connectionstring-property-ado.md)   
 [ConnectionTimeout, propriété (ADO)](./connectiontimeout-property-ado.md)   
 [State, propriété (ADO)](./state-property-ado.md)