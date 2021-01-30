---
description: Version, exemple de propriété (VC++)
title: Version, exemple de propriété (VC + +) | Microsoft Docs
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
- Version property [ADO], VC++ example
ms.assetid: 2440b6ff-2536-497c-a5f4-41db0cf1945e
author: rothja
ms.author: jroth
ms.openlocfilehash: 9d99778b2fd53fd75d38e8476022a4744192d251
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166285"
---
# <a name="version-property-example-vc"></a>Version, exemple de propriété (VC++)
Cet exemple utilise la propriété [version](./version-property-ado.md) d’un objet [Connection](./connection-object-ado.md) pour afficher la version ADO actuelle. Il utilise également plusieurs propriétés dynamiques pour afficher :  
  
-   Nom et version du SGBD actuel.  
  
-   Version de OLE DB.  
  
-   Nom et version du fournisseur.  
  
-   Version ODBC.  
  
-   Nom et version du pilote ODBC.  
  
> [!NOTE]
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier **Trusted_Connection = Yes** ou **Integrated Security = SSPI** à la place des informations d’ID d’utilisateur et de mot de passe dans la chaîne de connexion.  
  
```  
// BeginVersionCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void VersionX();  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
int main() {  
    if (FAILED(::CoInitialize(NULL)))  
        return -1;  
  
    VersionX();  
  
    ::CoUninitialize();  
}  
  
void VersionX() {  
    HRESULT hr = S_OK;  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
  
    // Define ADO object pointers.  Initialize pointers on define.  
    // These are in the ADODB::  namespace.  
    _ConnectionPtr pConnection = NULL;  
  
    try {  
        // Open connection.  
        TESTHR(pConnection.CreateInstance(__uuidof(Connection)));  
        pConnection->Open (strCnn, "", "", adConnectUnspecified);  
  
        printf("ADO Version   : %s\n\n",(LPCSTR) pConnection->Version);  
        printf("DBMS Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("DBMS Name")->Value);  
        printf("DBMS Version   : %s\n\n",(LPCSTR) (_bstr_t)  
            pConnection->Properties->GetItem("DBMS Version")->Value);  
        printf("OLE DB Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("OLE DB Version")->Value);  
        printf("Provider Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Provider Name")->Value);  
        printf("Provider Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Provider Version")->Value);  
        printf("Driver Name   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver Name")->Value);  
        printf("Driver Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver Version")->Value);  
        printf("Driver ODBC Version   : %s\n\n",(LPCSTR) (_bstr_t)   
            pConnection->Properties->GetItem("Driver ODBC Version")->Value);  
  
    }  
  
    catch (_com_error &e) {  
        // Notify the user of errors if any.  
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
  
    if ( (pConnection->Errors->Count) > 0) {  
        long nCount = pConnection->Errors->Count;  
        // Collection ranges from 0 to nCount -1.  
        for ( long i = 0 ; i < nCount ; i++) {  
            pErr = pConnection->Errors->GetItem(i);  
            printf("Error number: %x\t%s\n", pErr->Number, (LPCSTR) pErr->Description);  
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
 [Version, propriété (ADO)](./version-property-ado.md)