---
title: IBCPSession2::BCPSetBulkMode (pilote OLE DB) | Microsoft Docs
description: Apprenez à utiliser la méthode IBCPSession2::BCPSetBulkMode pour créer une copie en bloc à partir d’une requête ou d’une table dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- BCPSetBulkMode function
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a29e4de3d4c0cdd3572b1ce9f8f257bad65e57e7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192065"
---
# <a name="ibcpsession2bcpsetbulkmode"></a>IBCPSession2::BCPSetBulkMode
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  IBCPSession2::BCPSetBulkMode fournit une alternative à [IBCPSession::BCPColFmt &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession-bcpcolfmt-ole-db.md) pour spécifier le format de colonne. Contrairement à IBCPSession::BCPColFmt, qui définit des attributs de format de colonne individuels, IBCPSession2::BCPSetBulkMode définit tous les attributs.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
HRESULT BCPSetBulkMode (  
      int property,  
   void * pField,  
   int cbField,  
   void * pRow,  
   int cbRow  
);  
```  
  
## <a name="arguments"></a>Arguments  
 *property*  
 Constante de type BYTE. Consultez le tableau dans la section Notes pour obtenir une liste des constantes.  
  
 *pField*  
 Pointeur vers la valeur de marque de fin de champ.  
  
 cbField  
 Longueur, en octets, de la valeur de marque de fin de champ.  
  
 pRow  
 Pointeur vers la valeur de marque de fin de ligne.  
  
 cbRow  
 Longueur, en octets, de la valeur de marque de fin de ligne.  
  
## <a name="returns"></a>Retours  
 IBCPSession2::BCPSetBulkMode peut retourner l’un des éléments suivants :  
  
|||  
|-|-|  
|**S_OK**|S_OK|  
|**E_FAIL**|Une erreur spécifique au fournisseur s’est produite. Pour obtenir des informations détaillées, utilisez l’interface ISQLServerErrorInfo.|  
|**E_UNEXPECTED**|L'appel à la méthode était inattendu. Par exemple, la méthode **IBCPSession2::BCPInit** n’a pas été appelée avant d’appeler IBCPSession2::BCPSetBulkMode.|  
|**E_INVALIDARG**|L'argument n'était pas valide.|  
|**E_OUTOFMEMORY**|Erreur de mémoire insuffisante.|  
  
## <a name="remarks"></a>Notes  
 IBCPSession2::BCPSetBulkMode peut être utilisé pour créer une copie en bloc à partir d’une requête ou d’une table. Lorsque IBCPSession2::BCPSetBulkMode est utilisé pour copier en bloc une instruction de requête, il doit être appelé avant d’appeler `IBCPSession::BCPControl(BCP_OPTIONS_HINTS, ...)` pour spécifier l’instruction de requête.  
  
 Vous devez éviter de combiner la syntaxe d'appel RPC avec la syntaxe de requête de lot (`{rpc func};SELECT * from Tbl`, par exemple) dans un texte de commande simple.  Cela entraînera le retour par ICommandPrepare::Prepare d'une erreur et vous empêchera de récupérer des métadonnées. Utilisez la syntaxe ODBC CALL (`{call func}; SELECT * from Tbl`, par exemple) si vous devez combiner l'exécution d'une procédure stockée et une requête de lot dans un texte de commande simple.  
  
 Le tableau suivant répertorie les constantes du paramètre *property* .  
  
|Propriété|Description|  
|--------------|-----------------|  
|BCP_OUT_CHARACTER_MODE|Spécifie le mode de sortie de caractères.<br /><br /> Correspond à l’option -c dans BCP.EXE et à IBCPSession::BCPColFmt avec la propriété *eUserDataType* définie sur **BCP_TYPE_SQLCHARACTER**.|  
|BCP_OUT_WIDE_CHARACTER_MODE|Spécifie le mode de sortie Unicode.<br /><br /> Correspond à l’option -w dans BCP.EXE et à IBCPSession::BCPColFmt avec la propriété *eUserDataType* définie sur **BCP_TYPE_SQLNCHAR**.|  
|BCP_OUT_NATIVE_TEXT_MODE|Spécifie des types natifs pour les types de non-caractères et Unicode pour les types de caractères.<br /><br /> Correspond à l’option -N dans BCP.EXE et IBCPSession::BCPColFmt avec la propriété *eUserDataType* définie sur **BCP_TYPE_SQLNCHAR** si le type de colonne est une chaîne ou **BCP_TYPE_DEFAULT** si ce n’est pas une chaîne.|  
|BCP_OUT_NATIVE_MODE|Spécifie les types de base de données natifs.<br /><br /> Correspond à l’option -n dans BCP.EXE et à IBCPSession::BCPColFmt avec la propriété *eUserDataType* définie sur **BCP_TYPE_DEFAULT**.|  
  
 Vous pouvez appeler IBCPSession::BCPControl et IBCPSession2::BCPSetBulkMode pour les options IBCPSession::BCPControl qui ne sont pas en conflit avec IBCPSession2::BCPSetBulkMode. Par exemple, vous pouvez appeler IBCPSession::BCPControl avec **BCP_OPTION_FIRST** et IBCPSession2::BCPSetBulkMode.  
  
 Vous ne pouvez pas appeler IBCPSession::BCPControl avec **BCP_OPTION_TEXTFILE** et IBCPSession2::BCPSetBulkMode.  
  
 Si vous tentez d’appeler IBCPSession2::BCPSetBulkMode avec une séquence d’appels de fonction qui comprend IBCPSession::BCPColFmt, IBCPSession::BCPControl et IBCPSession::BCPReadFmt, l’un des appels de fonction retourne un échec d’erreur de séquence. Si vous choisissez de corriger l’échec, appelez IBCPSession::BCPInit pour réinitialiser les paramètres et recommencer.  
  
 Vous trouverez ci-dessous quelques exemples d'appels de fonction qui provoquent une erreur de séquence de fonction :  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_IN);  
BCPSetBulkMode();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPReadFmt();  
```  
  
```cpp  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPSetBulkMode();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPSetBulkMode();  
BCPColFmt();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPReadFmt();  
BCPColFmt();  
```  
  
```cpp  
BCPInit(NULL, "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetBulkMode();  
BCPControl(BCP_OPTION_HINTS, "select ...");  
BCPReadFmt();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPColumns();  
```  
  
```cpp  
BCPInit("table", "dataFile", "errorFile", BCP_DIRECTION_OUT);  
BCPControl(BCP_OPTION_DELAYREADFMT, true);  
BCPSetColFmt();  
```  
  
## <a name="example"></a>Exemple  
 L'exemple suivant crée quatre fichiers à l'aide de paramètres différents d'IBCPSession2::BCPSetBulkMode.  
  
```cpp  
  
// compile with: oleaut32.lib ole32.lib  
  
#include <stdio.h>  
#include "msoledbsql.h"  
  
IDBInitialize*  g_pIDBInitialize = NULL;  
IBCPSession2 * g_pIBcpSession = NULL;  
class COLEDBPropSet : public DBPROPSET {  
public:  
   COLEDBPropSet() {  
      rgProperties = NULL;  
      cProperties = 0;  
   };  
   COLEDBPropSet(const GUID& guid) {  
      rgProperties = NULL;  
      cProperties = 0;  
      guidPropertySet = guid;  
   };  
   ~COLEDBPropSet() {  
      for ( ULONG i = 0 ; i < cProperties ; i++ )  
         VariantClear(&rgProperties[i].vValue);  
      CoTaskMemFree(rgProperties);  
   }  
   void SetGUID(const GUID& guid) {  
      guidPropertySet = guid;  
   };  
   bool AddProperty(DWORD dwPropertyID, bool bValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BOOL;  
      rgProperties[cProperties].vValue.boolVal = (bValue) ? VARIANT_TRUE : VARIANT_FALSE;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID, long nValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID  = dwPropertyID;  
      rgProperties[cProperties].vValue.vt     = VT_I4;  
      rgProperties[cProperties].vValue.lVal   = nValue;  
      cProperties++;  
      return true;  
   };  
   bool AddProperty(DWORD dwPropertyID,LPCWSTR szValue) {  
      if (!Add())  
         return false;  
      rgProperties[cProperties].dwPropertyID = dwPropertyID;  
      rgProperties[cProperties].vValue.vt = VT_BSTR;  
      rgProperties[cProperties].vValue.bstrVal = SysAllocString(szValue);  
      cProperties++;  
      return true;  
   };  
   bool Add() {  
      DBPROP* p = (DBPROP*)CoTaskMemRealloc(rgProperties, (cProperties + 1) * sizeof(DBPROP));  
      if (p != NULL) {  
         rgProperties = p;  
         rgProperties[cProperties].dwOptions = DBPROPOPTIONS_REQUIRED;  
         rgProperties[cProperties].colid = DB_NULLID;  
         rgProperties[cProperties].vValue.vt = VT_EMPTY;  
         return true;  
      }  
      else  
         return false;  
   };  
};  
  
void OLEDBCleanUp() {  
   if (g_pIDBInitialize) {  
      g_pIDBInitialize->Release();  
      g_pIDBInitialize = NULL;  
   }  
   if (g_pIBcpSession) {  
      g_pIBcpSession->Release();  
      g_pIBcpSession = NULL;  
   }  
}  
  
BOOL MakeOLEDBConnect(LPWSTR  pServer) {  
   BOOL ret = true;  
   IDBProperties * pIDBProperties = NULL;  
   IDBCreateSession * pIDBCreateSession = NULL;  
   COLEDBPropSet PropSet(DBPROPSET_DBINIT);  
   COLEDBPropSet BcpProperty(DBPROPSET_SQLSERVERDATASOURCE);  
   try {  
      HRESULT hr = CoInitializeEx(NULL,COINIT_MULTITHREADED);   
      hr = CoCreateInstance(MSOLEDBSQL_CLSID, NULL, CLSCTX_INPROC_SERVER, IID_IDBInitialize, (LPVOID *)&g_pIDBInitialize);  
      if (FAILED(hr)) {  
         printf("CoCreateInstance failed\n");  
         return false;  
      }  
      PropSet.AddProperty(DBPROP_INIT_DATASOURCE, (LPWSTR)pServer);  
      PropSet.AddProperty(DBPROP_AUTH_INTEGRATED, L"SSPI");  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBProperties, (void**) &pIDBProperties);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->QueryInterface(IID_IDBProperties...) failed\n");  
         throw false;  
      }  
      hr = pIDBProperties->SetProperties(1, &PropSet);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties(...) failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->Initialize();  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->Initialize() failed\n");  
         throw false;  
      }  
      BcpProperty.AddProperty(SSPROP_ENABLEFASTLOAD, true);  
      BcpProperty.AddProperty(SSPROP_ENABLEBULKCOPY, true);  
      hr = pIDBProperties->SetProperties(1, &BcpProperty);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->->SetProperties() for bcp failed\n");  
         throw false;  
      }  
      hr = g_pIDBInitialize->QueryInterface(IID_IDBCreateSession, (void**) &pIDBCreateSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBInitialize->QueryInterface(IID_IDBCreateSession..) failed\n");  
         throw false;  
      }  
  
      hr = pIDBCreateSession->CreateSession(NULL, IID_IBCPSession2, (IUnknown**) &g_pIBcpSession);  
      if (FAILED(hr)) {  
         printf("g_pIDBCreateSession->CreateSession() failed\n");  
         throw false;  
      }  
   }  
   catch(...) {  
      ret = false;  
   }  
   if (pIDBProperties)  
      pIDBProperties->Release();  
   if (pIDBCreateSession)  
      pIDBCreateSession->Release();  
   return ret;  
}  
  
BOOL BCPSetBulkMode(LPWSTR pszServer, LPTSTR pszQureryOut, char BCPType, LPWSTR pszDataFile) {  
   HRESULThr;  
   if (!MakeOLEDBConnect(pszServer))  
      return false;  
   hr = g_pIBcpSession->BCPInit(NULL, pszDataFile, NULL, BCP_DIRECTION_OUT );   // bcp init for queryout  
   if (FAILED(hr)) {  
      printf("BCP init failed\n");  
      OLEDBCleanUp();  
      return false;  
   }  
   // setbulkmode  
   char ColTerm[] = "\t";  
   char RowTerm[] = "\r\n";  
   wchar_t wColTerm[] = L"\t";  
   wchar_t wRowTerm[] = L"\r\n";  
   BYTE * pColTerm = NULL;  
   int cbColTerm = NULL;  
   BYTE * pRowTerm = 0;  
   int cbRowTerm = 0;  
   int bulkmode = -1;  
  
   if(BCPType == 'c') {   // bcp -c  
      pColTerm = (BYTE*)ColTerm;  
      pRowTerm = (BYTE*)RowTerm;  
      cbColTerm = 1;  
      cbRowTerm = 2;  
      bulkmode = BCP_OUT_CHARACTER_MODE;  
   }  
   else  
      if(BCPType == 'w') {   // bcp -w  
         pColTerm = (BYTE*)wColTerm;  
         pRowTerm = (BYTE*)wRowTerm;  
         cbColTerm = 2;  
         cbRowTerm = 4;  
         bulkmode = BCP_OUT_WIDE_CHARACTER_MODE;  
      }  
      else  
         if (BCPType == 'n')   // bcp -n  
            bulkmode = BCP_OUT_NATIVE_MODE;  
         else  
            if (BCPType == 'N')   // bcp -n  
               bulkmode = BCP_OUT_NATIVE_TEXT_MODE;  
            else {  
               printf("unknown bcp mode\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            hr = g_pIBcpSession->BCPSetBulkMode(bulkmode, pColTerm, cbColTerm, pRowTerm, cbRowTerm);  
            if (FAILED(hr)) {  
               printf("BCPSetBulkMode failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
  
            // set queryout TSQL statement  
            hr = g_pIBcpSession->BCPControl(BCP_OPTION_HINTS, pszQureryOut);  
            if (FAILED(hr)) {  
               printf("BCPControl failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            // bcp copy  
            DBROWCOUNT nRowsInserted = 0;  
            hr = g_pIBcpSession->BCPExec(&nRowsInserted);  
            if (FAILED(hr)) {  
               printf("BCPExec failed\n");  
               OLEDBCleanUp();  
               return false;  
            }  
            printf("bcp done\n");  
            OLEDBCleanUp();  
            return true;  
}  
  
int main() {  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'c', L"bcpc.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'w', L"bcpw.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -c test', 1,2") , 'n', L"bcpn.dat");  
   BCPSetBulkMode(L"localhost", TEXT("SELECT 'this is a bcp -w test', 1,2") , 'N', L"bcp_N.dat");  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [IBCPSession2 &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/ibcpsession2-ole-db.md)  
  
  
