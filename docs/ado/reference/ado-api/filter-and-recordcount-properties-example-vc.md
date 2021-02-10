---
description: Filter et RecordCount, exemples de propriétés (VC + +)
title: Filter et RecordCount, exemples de propriétés (VC + +) | Microsoft Docs
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
- RecordCount property [ADO], VC++ example
- Filter property [ADO], VC++ example
ms.assetid: b71346cb-3b09-4b8c-a600-976171a1c336
author: rothja
ms.author: jroth
ms.openlocfilehash: 3771ecab9a598c0cd3e40f316ce00ba95cbce2f6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100024605"
---
# <a name="filter-and-recordcount-properties-example-vc"></a>Filter et RecordCount, exemples de propriétés (VC + +)
Cet exemple utilise la propriété [Filter](./filter-property.md) pour ouvrir un nouveau [jeu d’enregistrements](./recordset-object-ado.md) en fonction d’une condition spécifiée appliquée à un **jeu d’enregistrements** existant. Elle utilise la propriété [RecordCount](./recordcount-property-ado.md) pour afficher le nombre d’enregistrements dans les deux **jeux d’enregistrements**. La fonction FilterField est requise pour l’exécution de cette procédure.  
  
## <a name="example"></a>Exemple  
  
```  
// BeginFilterCpp.cpp  
// compile with: /EHsc /c  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
#include <stdio.h>  
#include <ole2.h>  
#include <conio.h>  
#include "icrsint.h"  
  
// Class extracts only Pub Name and Country Name.  
class CPublishers : public CADORecordBinding {  
   BEGIN_ADO_BINDING(CPublishers)  
  
   // Column Pub Name is the 2nd field in the recordset  
   ADO_VARIABLE_LENGTH_ENTRY2(2, adVarChar, m_szP_pubname, sizeof(m_szP_pubname),   
                              lP_pubnameStatus, TRUE)  
  
   // Column Country Name is the 5th field in the recordset  
   ADO_VARIABLE_LENGTH_ENTRY2(5, adVarChar, m_szP_country, sizeof(m_szP_country),   
                              lP_countryStatus, TRUE)  
  
   END_ADO_BINDING()  
  
public:  
   CHAR m_szP_pubname[50];  
   ULONG lP_pubnameStatus;  
   CHAR m_szP_country[50];  
   ULONG lP_countryStatus;  
};  
  
// Function declarations  
inline void TESTHR(HRESULT x) { if FAILED(x) _com_issue_error(x); };  
void FilterX();  
_RecordsetPtr FilterField(_RecordsetPtr rstTemp, _bstr_t strField, _bstr_t strFilter);  
void FilterX2();  
void PrintProviderError(_ConnectionPtr pCnn1);  
void PrintComError(_com_error &e);  
  
inline char* mygets(char* strDest, int n) {  
   char strExBuff[10];  
   char* pstrRet = fgets(strDest, n, stdin);  
  
   if (pstrRet == NULL)  
      return NULL;  
  
   if (!strrchr(strDest, '\n'))  
      // Exhaust the input buffer.  
      do {  
         fgets(strExBuff, sizeof(strExBuff), stdin);  
      } while (!strrchr(strExBuff, '\n'));  
   else  
      // Replace '\n' with '\0'  
      strDest[strrchr(strDest, '\n') - strDest] = '\0';  
  
   return pstrRet;  
}  
  
int main() {  
   HRESULT hr = S_OK;  
  
   if ( FAILED(::CoInitialize(NULL)) )  
      return -1;  
  
   FilterX();  
   FilterX2();  
  
   ::CoUninitialize();  
}  
  
void FilterX() {  
   // Define ADO object pointers, initialize pointers on define.  These are in the ADODB::  namespace.  
   _RecordsetPtr rstPublishers = NULL;  
   _RecordsetPtr rstPublishersCountry = NULL;  
  
   // Define Other Variables  
   HRESULT hr = S_OK;  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
   int intPublisherCount = 0;  
   long recCount = 0;  
   _bstr_t strCountry;   
   _bstr_t strMessage;  
   char *tempStr;  
   CHAR sz_CountryName[50];  
   bool boolFlag = TRUE;  
   char * token1;  
  
   try {  
      // Open recordset with data from Publishers table.  
      rstPublishers.CreateInstance(__uuidof(Recordset));  
      rstPublishersCountry.CreateInstance(__uuidof(Recordset));  
  
      rstPublishers->CursorType = adOpenStatic;  
  
      TESTHR(rstPublishers->Open("publishers", strCnn, adOpenStatic , adLockReadOnly, adCmdTable));  
  
      // Populate the Recordset.  
      intPublisherCount = rstPublishers->RecordCount;  
  
      // Get user input.  
      printf("Enter a country to filter on:");  
      mygets(sz_CountryName, 50);  
  
      // Trim the string  
      tempStr = strtok_s(sz_CountryName, "  \t", &token1);  
      strCountry = tempStr;  
      if (tempStr == NULL)  
         boolFlag = FALSE;  
  
      if (boolFlag) {  
         if (strcmp(sz_CountryName,"")) {  
            // Open a filtered Recordset object.  
            rstPublishersCountry = FilterField(rstPublishers, "Country", strCountry);  
            recCount = rstPublishersCountry->GetRecordCount();  
            if (recCount == 0)  
               printf("\nNo publishers from that country.\n");  
            else {  
               // Print number of records for the original recordset object and the   
               // filtered Recordset object.  
               printf("\nOrders in original recordset:\n%d", intPublisherCount);  
               printf("\nOrders in filtered recordset (Country = '%s'): \n%d\n\n",   
                      (LPCSTR)strCountry, rstPublishersCountry->RecordCount);  
            }  
         }  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      _variant_t vtConnect = rstPublishers->GetActiveConnection();  
  
      // GetActiveConnection returns connect string if connection  
      // is not open, else returns Connection object.  
      switch(vtConnect.vt) {  
         case VT_BSTR:  
            PrintComError(e);  
            break;  
         case VT_DISPATCH:  
            PrintProviderError(vtConnect);  
            break;  
         default:  
            printf("Errors occured.");  
            break;  
      }  
   }  
  
   // Clean up objects before exit.  
   if (rstPublishers)  
      if (rstPublishers->State == adStateOpen)  
         rstPublishers->Close();  
   if (rstPublishersCountry)  
      if (rstPublishersCountry->State == adStateOpen)  
         rstPublishersCountry->Close();  
}  
  
_RecordsetPtr FilterField(_RecordsetPtr rstTemp,_bstr_t strField, _bstr_t strFilter) {  
   // Set a filter on the specified Recordset object and then open a new Recordset object.  
   rstTemp->Filter  = strField + " = '" + strFilter + "'";  
   return rstTemp;  
}  
  
void FilterX2() {  
   _RecordsetPtr rstPublishers;  
   CPublishers publishers;  
  
   // Define Other Variables  
   HRESULT hr = S_OK;  
   IADORecordBinding *picRs = NULL;   // Interface Pointer declared  
  
   _bstr_t strCnn("Provider='sqloledb'; Data Source='My_Data_Source'; Initial Catalog='pubs'; Integrated Security='SSPI';");  
   try {  
      // Open recordset with data from Publishers table.  
      rstPublishers.CreateInstance(__uuidof(Recordset));   
      rstPublishers->CursorType = adOpenStatic;  
  
      TESTHR(rstPublishers->Open("SELECT * FROM publishers WHERE "  
         "Country='USA'", strCnn, adOpenStatic, adLockReadOnly, adCmdText));  
  
      // Open an IADORecordBinding interface pointer   
      // which we'll use for Binding Recordset to a class  
      TESTHR(rstPublishers->QueryInterface(__uuidof(IADORecordBinding), (LPVOID*)&picRs));  
  
      // Bind the Recordset to a C++ Class here   
      TESTHR(picRs->BindToRecordset(&publishers));  
  
      // Print current data in recordset.  
      rstPublishers->MoveFirst();  
  
      while (!rstPublishers->EndOfFile) {  
         printf("%s, %s\n",   
            publishers.lP_pubnameStatus == adFldOK ?   
            publishers.m_szP_pubname: "<NULL>",  
            publishers.lP_countryStatus == adFldOK ?   
            publishers.m_szP_country: "<NULL>");  
  
         rstPublishers->MoveNext();  
      }  
   }  
   catch (_com_error &e) {  
      // Notify the user of errors if any.  
      _variant_t vtConnect = rstPublishers->GetActiveConnection();  
  
      // GetActiveConnection returns connect string if connection  
      // is not open, else returns Connection object.  
      switch(vtConnect.vt) {  
         case VT_BSTR:  
            PrintComError(e);  
            break;  
         case VT_DISPATCH:  
            PrintProviderError(vtConnect);  
            break;  
         default:  
            printf("Errors occured.");  
            break;  
      }  
   }  
  
   // Clean up objects before exit.  Release the IADORecordset Interface here     
   if (picRs)  
      picRs->Release();  
  
   if (rstPublishers)  
      if (rstPublishers->State == adStateOpen)  
         rstPublishers->Close();  
}  
  
void PrintProviderError(_ConnectionPtr pCnn1) {  
   // Print Provider Errors from Connection object.  
   // pErr is a record object in the Connection's Error collection.  
   ErrorPtr pErr  = NULL;  
  
   if ( (pCnn1->Errors->Count) > 0) {  
      long nCount = pCnn1->Errors->Count;  
      // Collection ranges from 0 to nCount -1.  
      for (long i = 0 ; i < nCount ; i++) {  
         pErr = pCnn1->Errors->GetItem(i);  
         printf("\t Error number: %x\t%s", pErr->Number, (LPCSTR) pErr->Description);  
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
  
## <a name="sample-input"></a>Exemple d’entrée  
  
```  
USA  
```  
  
## <a name="sample-output"></a>Exemple de sortie  
  
```  
Orders in original recordset:  
8  
Orders in filtered recordset (Country = 'USA'):  
6  
  
New Moon Books, USA  
Binnet & Hardley, USA  
Algodata Infosystems, USA  
Five Lakes Publishing, USA  
Ramona Publishers, USA  
Scootney Books, USA  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Filter (propriété)](./filter-property.md)   
 [RecordCount, propriété (ADO)](./recordcount-property-ado.md)   
 [Recordset, objet (ADO)](./recordset-object-ado.md)