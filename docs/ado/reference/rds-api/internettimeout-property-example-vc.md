---
description: InternetTimeout, exemple de propriété (VC++)
title: InternetTimeout, exemple de propriété (VC + +) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/20/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- C++
helpviewer_keywords:
- InternetTimeout property [ADO], VC++ example
ms.assetid: 88b6d05c-d4eb-4ab1-bbe2-95d146237f94
author: rothja
ms.author: jroth
ms.openlocfilehash: 74fc1cb8504e027db7181657cbcc74452cd5dfe4
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88768088"
---
# <a name="internettimeout-property-example-vc"></a>InternetTimeout, exemple de propriété (VC++)
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](https://go.microsoft.com/fwlink/?LinkId=199565).  
  
 Cet exemple illustre la propriété [InternetTimeout](./internettimeout-property-rds.md) , qui existe sur les objets [DataControl](./datacontrol-object-rds.md) et [DataSpace](./dataspace-object-rds.md) . Dans ce cas, la propriété **InternetTimeout** est illustrée sur l’objet **DataControl** et le délai d’attente est défini sur 20 secondes.  
  
```cpp
// BeginInternetTimeoutCpp  
#import "c:\Program Files\Common Files\System\ADO\msado15.dll" \  
    no_namespace rename("EOF", "EndOfFile")  
#import "C:\Program Files\Common Files\System\MSADC\msadco.dll"  
  
#include <ole2.h>  
#include <stdio.h>  
#include <conio.h>  
  
// Function declarations  
inline void TESTHR(HRESULT x) {if FAILED(x) _com_issue_error(x);};  
void InternetTimeOutX(void);  
void PrintProviderError(_ConnectionPtr pConnection);  
void PrintComError(_com_error &e);  
  
//////////////////////////////////////////////////////////  
//                                                      //  
//    Main Function                                     //  
//                                                      //  
//////////////////////////////////////////////////////////  
  
void main()  
{  
    if(FAILED(::CoInitialize(NULL)))  
        return;  
  
    InternetTimeOutX();  
  
    ::CoUninitialize();  
}  
  
//////////////////////////////////////////////////////////  
//                                                      //  
//         InternetTimeOutX Function                    //  
//                                                      //  
//////////////////////////////////////////////////////////  
  
void InternetTimeOutX(void)   
{  
    HRESULT hr = S_OK;  
  
    // Define ADO object pointers.  
    // Initialize pointers on define.  
    // These are in the ADODB::  namespace.  
    _RecordsetPtr  pRst = NULL;  
  
    //Define RDS object pointers  
    RDS::IBindMgrPtr dc ;  
  
    try  
    {  
        TESTHR(dc.CreateInstance(__uuidof(RDS::DataControl)));  
        dc->Server = "https://MyServer";  
        dc->Connect = "Data Source='AuthorDatabase'";  
        dc->SQL = "SELECT * FROM Authors";  
  
        // Wait at least 20 seconds.  
        dc->InternetTimeout = 20000;  
        dc->Refresh();  
  
        // Use another Recordset as a convenience  
        pRst = dc->GetRecordset();  
        while(!(pRst->EndOfFile))  
        {  
            printf("%s %s",(LPSTR) (_bstr_t) pRst->Fields->  
                GetItem("au_fname")->Value,  
               (LPSTR) (_bstr_t) pRst->Fields->  
                GetItem("au_lname")->Value);  
  
            pRst->MoveNext();  
        }  
        pRst->Close();  
    }  
  
    catch (_com_error &e)  
    {  
        PrintProviderError(pRst->GetActiveConnection());  
        PrintComError(e);  
    }  
}  
  
//////////////////////////////////////////////////////////  
//                                                      //  
//     PrintProviderError Function                      //  
//                                                      //  
//////////////////////////////////////////////////////////  
  
void PrintProviderError(_ConnectionPtr pConnection)  
{  
    // Print Provider Errors from Connection object.  
    // pErr is a record object in the Connection's Error collection.  
    ErrorPtr  pErr  = NULL;  
  
    if( (pConnection->Errors->Count) > 0)  
    {  
        long nCount = pConnection->Errors->Count;  
        // Collection ranges from 0 to nCount -1.  
        for(long i = 0; i < nCount; i++)  
        {  
            pErr = pConnection->Errors->GetItem(i);  
            printf("\t Error number: %x\t%s", pErr->Number,   
                pErr->Description);  
        }  
    }  
}  
  
//////////////////////////////////////////////////////////  
//                                                      //  
//     PrintComError Function                           //  
//                                                      //  
//////////////////////////////////////////////////////////  
  
void PrintComError(_com_error &e)  
{  
    _bstr_t bstrSource(e.Source());  
    _bstr_t bstrDescription(e.Description());  
  
    // Print Com errors.    
    printf("Error\n");  
    printf("\tCode = %08lx\n", e.Error());  
    printf("\tCode meaning = %s\n", e.ErrorMessage());  
    printf("\tSource = %s\n", (LPCSTR) bstrSource);  
    printf("\tDescription = %s\n", (LPCSTR) bstrDescription);  
}  
// EndInternetTimeoutCpp  
```  
  
## <a name="see-also"></a>Voir aussi  
 [InternetTimeout, propriété (RDS)](./internettimeout-property-rds.md)