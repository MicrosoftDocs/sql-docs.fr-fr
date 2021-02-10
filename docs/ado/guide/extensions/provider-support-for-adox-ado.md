---
description: Prise en charge de fournisseur pour ADOX (ADO)
title: Prise en charge du fournisseur pour ADOX (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADOX provider support [ADO]
ms.assetid: 64234ce5-dc46-4c8a-a316-61956b6b9abb
author: rothja
ms.author: jroth
ms.openlocfilehash: 45842fd5035a8b52c68911d29a23a3414a776e02
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036689"
---
# <a name="provider-support-for-adox-ado"></a>Prise en charge de fournisseur pour ADOX (ADO)
Certaines fonctionnalités d’ADOX ne sont pas prises en charge, en fonction de votre fournisseur de données OLE DB. ADOX est entièrement pris en charge avec le [fournisseur de OLE DB pour Microsoft Jet](../appendixes/microsoft-ole-db-provider-for-microsoft-jet.md). Les fonctionnalités non prises en charge avec le [fournisseur microsoft OLE DB pour SQL Server](../appendixes/microsoft-ole-db-provider-for-sql-server.md), le [fournisseur Microsoft OLE DB pour ODBC](../appendixes/microsoft-ole-db-provider-for-odbc.md)ou le [fournisseur Microsoft OLE DB pour Oracle](../appendixes/microsoft-ole-db-provider-for-oracle.md) sont répertoriées dans les tableaux suivants. ADOX n’est pris en charge par aucun autre fournisseur Microsoft OLE DB.  
  
## <a name="microsoft-ole-db-provider-for-sql-server"></a>Fournisseur Microsoft OLE DB pour SQL Server  
  
|Objet ou collection|Restriction d’utilisation|  
|--------------------------|-----------------------|  
|**Tables** , collection|Les propriétés sont en lecture/écriture avant la création d’objets et en lecture seule lors du référencement d’un objet existant.|  
|**Views** , collection|Les **vues** ne sont pas prises en charge.|  
|**Procedures** , collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Objet de **procédure**|La propriété **Command** n’est pas prise en charge.|  
|Collection **Keys**|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Collection d' **utilisateurs**|**Les utilisateurs** ne sont pas pris en charge.|  
|Collection de **groupes**|Les **groupes** ne sont pas pris en charge.|  
  
## <a name="microsoft-ole-db-provider-for-odbc"></a>Fournisseur Microsoft OLE DB pour ODBC  
  
|Objet ou collection|Restriction d’utilisation|  
|--------------------------|-----------------------|  
|Objet **catalogue**|La méthode **Create** n’est pas prise en charge.|  
|**Tables** , collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge. Les propriétés sont en lecture/écriture avant la création d’objets et en lecture seule lors du référencement d’un objet existant.|  
|**Procedures** , collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Objet de **procédure**|La propriété **Command** n’est pas prise en charge.|  
|**Indexer** la collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Collection **Keys**|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Collection d' **utilisateurs**|**Les utilisateurs** ne sont pas pris en charge.|  
|Collection de **groupes**|Les **groupes** ne sont pas pris en charge.|  
  
## <a name="microsoft-ole-db-provider-for-oracle"></a>Fournisseur Microsoft OLE DB pour Oracle  
  
|Objet ou collection|Restriction d’utilisation|  
|--------------------------|-----------------------|  
|Objet **catalogue**|La méthode **Create** n’est pas prise en charge.|  
|**Tables** , collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge. Les propriétés sont en lecture/écriture avant la création d’objets et en lecture seule lors du référencement d’un objet existant.|  
|**Views** , collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|**Afficher** l’objet|La propriété **Command** n’est pas prise en charge.|  
|Objet **procedures**|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Objet de **procédure**|La propriété **Command** n’est pas prise en charge.|  
|**Indexer** la collection|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Collection **Keys**|Les méthodes **Append** et **Delete** ne sont pas prises en charge.|  
|Collection d' **utilisateurs**|**Les utilisateurs** ne sont pas pris en charge.|  
|Collection de **groupes**|Les **groupes** ne sont pas pris en charge.|