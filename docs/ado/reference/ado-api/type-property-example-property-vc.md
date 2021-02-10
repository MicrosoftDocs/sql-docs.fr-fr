---
description: Type, exemple de propriété (objet Property) (VC++)
title: Type, exemple de propriété (Property) (VC + +) | Microsoft Docs
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
- Type property [property] [ADO], VC++ example
ms.assetid: a4e23508-fbf3-4468-be55-212e7238802b
author: rothja
ms.author: jroth
ms.openlocfilehash: 39941627af5bdbcbcacac123ee3543f8963eb6d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100056404"
---
# <a name="type-property-example-property-vc"></a>Type, exemple de propriété (objet Property) (VC++)
Cet exemple illustre la propriété [type](./type-property-ado.md) . Il s’agit d’un modèle d’utilitaire permettant de répertorier les noms et les types d’une collection, tels que les [Propriétés](./properties-collection-ado.md), les [champs](./fields-collection-ado.md), etc.  
  
 Nous n’avons pas besoin d’ouvrir le [jeu d’enregistrements](./recordset-object-ado.md) pour accéder à sa collection **Properties** . ils sont mis en existence lorsque l’objet **Recordset** est instancié. Toutefois, l’affectation de la valeur **adUseClient** à la propriété [CursorLocation](./cursorlocation-property-ado.md) ajoute plusieurs propriétés dynamiques à la collection **Properties** de l’objet **Recordset** , ce qui rend l’exemple un peu plus intéressant. À des fins d’illustration, nous utilisons explicitement la propriété [Item](./item-property-ado.md) pour accéder à chaque objet [Property](./property-object-ado.md) .  
  
```  
// BeginTypePropertyCpp.cpp  
// compile with: /EHsc  
#import "msado15.dll" no_namespace rename("EOF", "EndOfFile")  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void TypeX();  
void PrintComError(_com_error &e);  
  
int main() {  
   if (FAILED(::CoInitialize(NULL)))  
      return -1;  
  
   TypeX();  
   ::CoUninitialize();  
}  
  
void TypeX() {  
   // Define ADO object pointers, initialize pointers. These are in the ADODB:: namespace  
   _RecordsetPtr pRst = NULL;  
   PropertyPtr pProperty = NULL;  
  
   // Define Other Variables  
   _bstr_t strMsg;  
   _variant_t vIndex;  
   int intLineCnt = 0;     
  
   try {  
      TESTHR(pRst.CreateInstance (__uuidof(Recordset)));  
  
      // Set the Recordset Cursor Location  
      pRst->CursorLocation = adUseClient;  
  
      for (short iIndex = 0; iIndex <= (pRst->Properties->GetCount() - 1);iIndex++) {  
         vIndex = iIndex;  
         pProperty = pRst->Properties->GetItem(vIndex);  
  
         int propType = (int)pProperty->GetType();  
  
         switch(propType) {  
         case adBigInt:  
            strMsg = "adBigInt";  
            break;  
         case adBinary:  
            strMsg = "adBinary";  
            break;  
         case adBoolean:  
            strMsg = "adBoolean";  
            break;  
         case adBSTR:  
            strMsg = "adBSTR";  
            break;  
         case adChapter:  
            strMsg = "adChapter";  
            break;  
         case adChar:  
            strMsg = "adChar";  
            break;  
         case adCurrency:  
            strMsg = "adCurrency";  
            break;  
         case adDate:  
            strMsg = "adDate";  
            break;  
         case adDBDate:  
            strMsg = "adDBDate";  
            break;  
         case adDBTime:  
            strMsg = "adDBTime";  
            break;  
         case adDBTimeStamp:  
            strMsg = "adDBTimeStamp";  
            break;  
         case adDecimal:  
            strMsg = "adDecimal";  
            break;  
         case adDouble:  
            strMsg = "adDouble";  
            break;  
         case adEmpty:  
            strMsg = "adEmpty";  
            break;  
         case adError:  
            strMsg = "adError";  
            break;  
         case adFileTime:  
            strMsg = "adFileTime";  
            break;  
         case adGUID:  
            strMsg = "adGUID";  
            break;  
         case adIDispatch:  
            strMsg = "adIDispatch";  
            break;  
         case adInteger:  
            strMsg = "adInteger";  
            break;  
         case adIUnknown:  
            strMsg = "adIUnknown";  
            break;  
         case adLongVarBinary:  
            strMsg = "adLongVarBinary";  
            break;  
         case adLongVarChar:  
            strMsg = "adLongVarChar";  
            break;  
         case adLongVarWChar:  
            strMsg = "adLongVarWChar";  
            break;  
         case adNumeric:  
            strMsg = "adNumeric";  
            break;  
         case adPropVariant:  
            strMsg = "adPropVariant";  
            break;  
         case adSingle:  
            strMsg = "adSingle";  
            break;  
         case adSmallInt:  
            strMsg = "adSmallInt";  
            break;  
         case adTinyInt:  
            strMsg = "adTinyInt";  
            break;  
         case adUnsignedBigInt:  
            strMsg = "adUnsignedBigInt";  
            break;  
         case adUnsignedInt:  
            strMsg = "adUnsignedInt";  
            break;  
         case adUnsignedSmallInt:  
            strMsg = "adUnsignedSmallInt";  
            break;  
         case adUnsignedTinyInt:  
            strMsg = "adUnsignedTinyInt";  
            break;  
         case adUserDefined:  
            strMsg = "adUserDefined";  
            break;  
         case adVarBinary:  
            strMsg = "adVarBinary";  
            break;  
         case adVarChar:  
            strMsg = "adVarChar";  
            break;  
         case adVariant:  
            strMsg = "adVariant";  
            break;  
         case adVarNumeric:  
            strMsg = "adVarNumeric";  
            break;  
         case adVarWChar:  
            strMsg = "adVarWChar";  
            break;  
         case adWChar:  
            strMsg = "adWChar";  
            break;  
         default:  
            strMsg = "*UNKNOWN*";  
            break;  
         }  
  
         intLineCnt++;  
  
         printf ("Property %d : %s,Type = %s\n",iIndex, (LPCSTR)pProperty->GetName(),  
            (LPCSTR)strMsg);  
      }  
   }  
   catch(_com_error &e) {  
      // Notify the user of errors if any.  
      PrintComError(e);  
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
 [Property, objet (ADO)](./property-object-ado.md)   
 [Type, propriété (ADO)](./type-property-ado.md)