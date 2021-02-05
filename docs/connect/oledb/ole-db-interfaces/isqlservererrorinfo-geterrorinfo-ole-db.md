---
title: ISQLServerErrorInfo::GetErrorInfo (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode ISQLServerErrorInfo::GetErrorInfo retourne un pointeur vers une structure SSERRORINFO OLE DB Driver pour SQL Server avec des détails d’erreurs SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISQLServerErrorInfo::GetErrorInfo (OLE DB)
apitype: COM
helpviewer_keywords:
- GetErrorInfo method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c23db8a2472f236e0a9733b46fdc0d338d6b6f70
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183762"
---
# <a name="isqlservererrorinfogeterrorinfo-ole-db"></a>ISQLServerErrorInfo::GetErrorInfo (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Retourne un pointeur vers une structure SSERRORINFO OLE DB Driver pour SQL Server qui contient les détails de l'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 OLE DB Driver pour SQL Server définit l'interface d'erreur **ISQLServerErrorInfo**. Cette interface retourne les détails d’une erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)], notamment sa gravité et son état.  

  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT GetErrorInfo(  
   SSERRORINFO**ppSSErrorInfo,  
   OLECHAR**ppErrorStrings);  
```  
  
## <a name="arguments"></a>Arguments  
 *ppSSErrorInfo*[out]  
 Pointeur vers une structure SSERRORINFO. Si la méthode échoue ou qu’il n’existe pas d’informations [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associées à l’erreur, le fournisseur n’alloue pas de mémoire et vérifie que l’argument *ppSSErrorInfo* est un pointeur null en sortie.  
  
 *ppErrorStrings*[out]  
 Pointeur vers un pointeur de chaîne de caractère Unicode. Si la méthode échoue ou qu’il n’existe pas d’informations [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] associées à l’erreur, le fournisseur n’alloue pas de mémoire et vérifie que l’argument *ppErrorStrings* est un pointeur null en sortie. La libération de l’argument *ppErrorStrings* avec la méthode **IMalloc::Free** libère les trois membres de type chaîne individuels de la structure SSERRORINFO retournée, la mémoire étant allouée dans un bloc.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 S_OK  
  
 E_INVALIDARG  
 L'argument *ppSSErrorInfo* ou *ppErrorStrings* était NULL.  
  
 E_OUTOFMEMORY  
 OLE DB Driver pour SQL Server n'a pas pu allouer une mémoire suffisante pour compléter la requête.  
  
## <a name="remarks"></a>Notes  
 Le pilote OLE DB pour SQL Server alloue de la mémoire pour les chaînes SSERRORINFO et OLECHAR retournées via les pointeurs passés par le consommateur. Le consommateur doit désallouer cette mémoire avec la méthode **IMalloc::Free** quand il n’est plus nécessaire d’accéder aux données d’erreur.  
  
 La structure SSERRORINFO est définie comme suit :  
  
```  
typedef struct tagSSErrorInfo  
   {  
   LPOLESTR pwszMessage;  
   LPOLESTR pwszServer;  
   LPOLESTR pwszProcedure;  
   LONG lNative;  
   BYTE bState;  
   BYTE bClass;  
   WORD wLineNumber;  
   }  
SSERRORINFO;  
```  
  
|Membre|Description|  
|------------|-----------------|  
|*pwszMessage*|Message d'erreur de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le message est retourné via la méthode **IErrorInfo::GetDescription**.|  
|*pwszServer*|Nom de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur laquelle l'erreur s'est produite.|  
|*pwszProcedure*|Le nom de la procédure stockée qui génère l'erreur si l'erreur s'est produite dans une procédure stockée ; sinon, une chaîne vide.|  
|*lNative*|Numéro d'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Le numéro d’erreur est identique à celui retourné dans le paramètre *plNativeError* de la méthode **ISQLErrorInfo::GetSQLInfo**.|  
|*bState*|État de l'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*bClass*|Gravité de l'erreur [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].|  
|*wLineNumber*|Le cas échéant, la ligne d'une procédure stockée [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] qui a généré le message d'erreur. Si aucune procédure n'est impliquée, la valeur par défaut est 1.|  
  
 Pointeurs dans les adresses de type référence de la structure de la chaîne retournée dans l’argument *ppErrorStrings*.  
  
## <a name="see-also"></a>Voir aussi  
 [RAISERROR &#40;Transact-SQL&#41;](../../../t-sql/language-elements/raiserror-transact-sql.md)  
  
