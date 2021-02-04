---
description: Configuration requise pour SQL Server Native Client
title: Configuration requise
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- system requirements [SQL Server Native Client]
- data access [SQL Server Native Client], system requirements
- SQL Server Native Client, system requirements
- SQLNCLI, system requirements
ms.assetid: 1c8e2f8a-a440-44da-8e3a-af632d34c52c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 27e6a98231e66a24a66233092e3e7e245a4e3d74
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536394"
---
# <a name="system-requirements-for-sql-server-native-client"></a>Configuration requise pour SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Pour utiliser les fonctionnalités d'accès aux données de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], par exemple MARS, les logiciels suivants doivent être installés :  
  
-   [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sur votre client.  
  
-   Une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur votre serveur.  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client requiert Windows Installer 3,1. Windows Installer 3,1 est déjà installé sur les [!INCLUDE[msCoName](../../includes/msconame-md.md)] systèmes d’exploitation Windows. Pour toutes les autres plateformes, vous devez l'installer explicitement. Pour plus d’informations, consultez [Windows Installer 3,1 Redistributable (v2)](https://www.microsoft.com/download/details.aspx?id=25).  
  
> [!NOTE]  
>  Assurez-vous que vous vous connectez avec les privilèges d'administrateur avant d'installer ce logiciel.  
  
## <a name="operating-system-requirements"></a>Systèmes d'exploitation requis  
 Pour obtenir la liste des systèmes d’exploitation qui prennent en charge [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client, consultez [stratégies de support pour SQL Server Native Client](../../relational-databases/native-client/applications/support-policies-for-sql-server-native-client.md).  
  
## <a name="sql-server-requirements"></a>Configuration SQL Server requise  
 Pour utiliser [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client afin d'accéder aux données de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez avoir installé une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] prend en charge les connexions à partir de toutes les versions de MDAC, Windows Data Access Components, et toutes les versions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Lorsqu'une version cliente plus ancienne se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], les types de données de serveur inconnus du client sont mappés à des types compatibles avec la version du client. Pour plus d'informations, consultez Compatibilité des types de données pour les versions du client, plus loin dans cette rubrique.  
  
## <a name="cross-language-requirements"></a>Configuration multilingue  
 La version anglaise de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client est prise en charge sur toutes les versions localisées des systèmes d'exploitation pris en charge. Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sont prises en charge sur les systèmes d'exploitation localisés dans la même langue que la version de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client localisée. Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client sont également prises en charge sur les versions en anglais des systèmes d'exploitation pris en charge, sous réserve que les paramètres de langue correspondants soient installés.  
  
 Pour les mises à niveau :  
  
-   Les versions en anglais de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client peuvent être mises à niveau vers n'importe quelle version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client peuvent être mises à niveau vers des versions localisées dans la même langue de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   La version localisée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client peut être mise à niveau vers la version en anglais de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   Les versions localisées de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client ne peuvent pas être mises à niveau vers des versions localisées dans une autre langue de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
## <a name="data-type-compatibility-for-client-versions"></a>Compatibilité des types de données pour les versions du client  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client mappent les nouveaux types de données aux types de données plus anciens qui sont compatibles avec les clients de bas niveau, comme indiqué dans le tableau ci-dessous.  
  
 Les applications OLE DB et ADO peuvent utiliser le mot clé de chaîne de connexion **DataTypeCompatibility** avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client pour fonctionner avec des types de données plus anciens. Quand **DataTypeCompatibility=80**, les clients OLE DB se connectent à l’aide de la version TDS (Tabular Data Stream) de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] plutôt que de la version TDS. Cela signifie que pour [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] et les types de données ultérieurs, la conversion de bas niveau sera effectuée par le serveur et non par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client. Cela signifie également que les fonctionnalités disponibles sur la connexion seront limitées à l'ensemble de fonctionnalités de [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. Plutôt que d'essayer de transmettre des requêtes non valides au serveur, une détection des tentatives d'utilisation de nouveaux types de données ou fonctionnalités intervient dès que possible sur les appels d'API et les erreurs sont retournées à l'application appelante.  
  
 Il n’existe aucun contrôle **DataTypeCompatibility** pour ODBC.  
  
 IDBInfo::GetKeywords retourne toujours une liste de mots clés qui correspondent à la version du serveur sur la connexion et n'est pas affectée par **DataTypeCompatibility**.  
  
|Type de données|SQL Server Native Client<br /><br /> SQL Server 2005|SQL Server Native Client 11.0<br /><br /> [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)]|Windows Data Access Components, MDAC et<br /><br /> applications OLE DB SQL Server Native Client avec DataTypeCompatibility=80|  
|---------------|--------------------------------------------------|-------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------|  
|CLR UDT (\<= 8 Ko)|udt|Udt|Varbinary|  
|varbinary(max)|varbinary|varbinary|Image|  
|varchar(max)|varchar|varchar|Texte|  
|nvarchar(max)|NVARCHAR|NVARCHAR|Ntext|  
|Xml|Xml|Xml|Ntext|  
|CLR UDT (> 8 Ko)|udt|varbinary|Image|  
|Date|Date|varchar|Varchar|  
|datetime2|datetime2|varchar|Varchar|  
|datetimeoffset|datetimeoffset|varchar|Varchar|  
|time|time|varchar|Varchar|  
  
## <a name="see-also"></a>Voir aussi  
 [Programmation SQL Server Native Client](../../relational-databases/native-client/sql-server-native-client-programming.md)   
 [Installation de SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)  
  
  
