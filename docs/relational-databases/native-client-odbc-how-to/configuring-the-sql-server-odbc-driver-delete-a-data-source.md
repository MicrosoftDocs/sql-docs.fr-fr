---
title: Supprimer une source de données (ODBC) | Microsoft Docs
description: Découvrez comment supprimer une source de données à l’aide de l’administrateur ODBC, par programmation ou à l’aide d’un fichier, avant d’utiliser des applications ODBC avec SQL Server 2005 ou version ultérieure.
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- data sources [ODBC]
ms.assetid: 910e3e16-7b91-49d8-80bb-b4243926afaa
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 56b013873ef8060101e28d9b2c4609fa41c0c798
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467810"
---
# <a name="configuring-the-sql-server-odbc-driver---delete-a-data-source"></a>Configuration du pilote ODBC SQL Server - Supprimer une source de données
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Avant d'utiliser des applications ODBC avec [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, vous devez savoir comment mettre à niveau la version des procédures stockées du catalogue sur les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et savoir comment ajouter, supprimer et tester des sources de données.  
  
  Vous pouvez supprimer une source de données à l’aide de l’administrateur ODBC, par programmation (à l’aide de [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md)) ou en supprimant un fichier (si un nom de source de données de fichier).  
  
### <a name="to-delete-a-data-source-by-using-odbc-administrator"></a>Pour supprimer une source de données à l'aide de l'Administrateur ODBC  
  
1.  Dans **le panneau de configuration**, ouvrez **Outils d’administration**, puis double-cliquez sur **sources de données ODBC (64 bits)** ou sur sources de **données ODBC (32 bits)**. Vous pouvez également exécuter odbcad32.exe à partir de l'invite de commandes.  
  
2.  Cliquez sur l’onglet **DSN utilisateur**, **système DSN** ou **fichier DSN** .  
  
3.  Sélectionnez la source de données à supprimer.  
  
4.  Cliquez sur **supprimer**, puis confirmez la suppression.  

## <a name="example"></a>Exemple  
 Pour supprimer par programmation une source de données, appelez [SQLConfigDataSource](../../relational-databases/native-client-odbc-api/sqlconfigdatasource.md) en utilisant ODBC_REMOVE_DSN ou ODBC_REMOVE_SYS_DSN comme deuxième paramètre.  
  
 L'exemple suivant vous montre comment supprimer une source de données par programme.  
  
```  
// remove_odbc_data_source.cpp  
// compile with: ODBCCP32.lib user32.lib  
#include <iostream>  
#include \<windows.h>  
#include \<odbcinst.h>  
  
int main() {   
   LPCSTR provider = "SQL Server";   // Windows SQL Server Driver  
   LPCSTR provider = "SQL Server";   // Windows SQL Server driver  
   LPCSTR provider2 = "SQL Server Native Client 11.0";   // SQL Server 2012 Native Client driver  
   LPCSTR dsnname = "DSN=data2";  
   BOOL retval = SQLConfigDataSource(NULL, ODBC_REMOVE_DSN, provider, dsnname);  
   std::cout << retval;   // 1 if successful  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter une source de données &#40;ODBC&#41;](../../relational-databases/native-client-odbc-how-to/configuring-the-sql-server-odbc-driver-add-a-data-source.md)  
  
  
