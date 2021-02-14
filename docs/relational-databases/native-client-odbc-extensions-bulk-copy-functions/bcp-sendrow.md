---
description: bcp_sendrow
title: bcp_sendrow | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
apiname:
- bcp_sendrow
apilocation:
- sqlncli11.dll
apitype: DLLExport
helpviewer_keywords:
- bcp_sendrow function
ms.assetid: ddbdb4bd-ad4e-4bf1-9a75-656aa26ce10a
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 2a97c59e9606b0ad5d08d8ed04a41f50f8924e6c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350662"
---
# <a name="bcp_sendrow"></a>bcp_sendrow
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Envoie une ligne de données à partir de variables de programme à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
RETCODE bcp_sendrow (  
    HDBC hdbc);  
```  
  
## <a name="arguments"></a>Arguments  
 *hdbc*  
 Handle de connexion ODBC compatible avec la copie en bloc.  
  
## <a name="returns"></a>Retours  
 SUCCEED ou FAIL.  
  
## <a name="remarks"></a>Notes  
 La fonction **bcp_sendrow** génère une ligne à partir de variables de programme et l'envoie à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Avant d'appeler **bcp_sendrow**, vous devez effectuer des appels à [bcp_bind](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-bind.md) pour spécifier les variables de programme qui contiennent des données de ligne.  
  
 Si **bcp_bind** est appelée en spécifiant un type de données long, de longueur variable, par exemple, un paramètre *eDataType* de SQLTEXT et un paramètre *pData* non null, **bcp_sendrow** envoie la valeur de données entière, comme c’est le cas pour tout autre type de données. Toutefois, si **bcp_bind** possède un paramètre *pData* NULL, **bcp_sendrow** redonne le contrôle à l'application immédiatement après que tous les colonnes avec des données spécifiées ont été envoyées à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. L'application peut ensuite appeler [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md) à plusieurs reprises pour envoyer les données longues de longueur variable à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], segment par segment. Pour plus d'informations, consultez [bcp_moretext](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-moretext.md).  
  
 Lorsque **bcp_sendrow** est utilisé pour copier en bloc des lignes à partir de variables de programme dans des tables [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les lignes sont validées uniquement lorsque l'utilisateur appelle [bcp_batch](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-batch.md) ou [bcp_done](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/bcp-done.md). L'utilisateur peut choisir d'appeler **bcp_batch** une fois chaque *n* lignes ou lors de creux entre deux périodes de données entrantes. Si **bcp_batch** n'est jamais appelé, les lignes sont validées lorsque **bcp_done** est appelé.  
  
 Pour plus d’informations sur la modification avec rupture dans la copie en bloc à compter de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] , consultez [exécution d’opérations de copie en bloc &#40;ODBC&#41;](../../relational-databases/native-client-odbc-bulk-copy-operations/performing-bulk-copy-operations-odbc.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Bulk Copy Functions](../../relational-databases/native-client-odbc-extensions-bulk-copy-functions/sql-server-driver-extensions-bulk-copy-functions.md)  
  
  
