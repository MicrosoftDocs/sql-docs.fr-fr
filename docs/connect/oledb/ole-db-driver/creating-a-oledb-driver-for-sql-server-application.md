---
title: Création d’une application de pilote OLE DB pour SQL Server | Microsoft Docs
description: Création d’une application de pilote OLE DB pour SQL Server
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- OLE DB Driver for SQL Server, application creation
- applications [OLE DB Driver for SQL Server]
- OLE DB, creating applications
author: pmasl
ms.author: pelopes
ms.openlocfilehash: e6b1ef5d4c4cbf486bd041c3bc39668fb9595e4b
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86004432"
---
# <a name="creating-an-ole-db-driver-for-sql-server-application"></a>Création d’une application de pilote OLE DB pour SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  La création d’une application de pilote OLE DB pour SQL Server implique les étapes suivantes :  
  
1.  Établissement d'une connexion à une source de données.  
  
2.  Exécution d'une commande.  
  
3.  Traitement des résultats.  
  
> [!NOTE]  
>  Lorsque c'est possible, utilisez l'authentification Windows. Si l'authentification Windows n'est pas disponible, invitez les utilisateurs à entrer leurs informations d'identification au moment de l'exécution. Évitez de stocker ces informations dans un fichier. Si vous devez rendre les informations d’identification persistantes, chiffrez-les avec [l’API de chiffrement Win32 cryptoAPI](https://go.microsoft.com/fwlink/?LinkId=9504).  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Établissement d’une connexion à une source de données](../../oledb/ole-db-driver/establishing-a-connection-to-a-data-source.md)  
  
-   [Exécution d’une commande](../../oledb/ole-db-driver/executing-a-command.md)  
  
-   [Traitement des résultats](../../oledb/ole-db-driver/processing-results.md)  
  
-   [Présentation des propriétés OLE DB](../../oledb/ole-db-driver/about-ole-db-properties.md)  
  
-   [Utilisation de la clause OUTPUT avec OLE DB dans OLE DB Driver pour SQL Server](../../oledb/ole-db-driver/using-the-output-clause-with-ole-db-in-oledb-driver-for-sql-server.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation OLE DB Driver pour SQL Server](../../oledb/ole-db/oledb-driver-for-sql-server-programming.md)  
  
  
