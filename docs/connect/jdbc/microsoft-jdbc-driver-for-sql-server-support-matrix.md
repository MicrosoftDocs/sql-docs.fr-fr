---
title: Matrice de support de Microsoft JDBC Driver pour SQL Server
description: Cette page contient la matrice de support et la stratégie de support de Microsoft JDBC Driver pour SQL Server.
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: c5769e67-99f7-4bc1-a4fa-8941dad33d35
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2fb84ce512cca3e87f0ec108698928caa2934122
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101835359"
---
# <a name="microsoft-jdbc-driver-for-sql-server-support-matrix"></a>Matrice de support de Microsoft JDBC Driver pour SQL Server

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette page contient la matrice de support et la politique de support de Microsoft JDBC Driver pour SQL Server.  
  
## <a name="microsoft-jdbc-driver-support-lifecycle-matrix-and-policy"></a>Matrice et politique de support de Microsoft JDBC Driver  

La politique de support de Microsoft fournit des informations transparentes et prévisibles concernant la politique de support des produits Microsoft. Les versions 4.x, 6.x, 7.x, 8.x et 9.x du pilote JDBC bénéficient de cinq ans de support standard à partir de la date de publication du pilote. Le support standard est défini sur le site web de la politique de support de Microsoft.  
  
Les options de support étendu et personnalisé ne sont pas disponibles pour le pilote Microsoft JDBC.  

Les pilotes Microsoft JDBC suivants bénéficient d’un support jusqu’à la date de fin de support indiquée.  
  
|Nom du pilote|Version de package du pilote|Fichier(s) JAR applicable(s)|Fin du support standard|
|-|-|-|-|  
|Microsoft JDBC Driver 9.2 pour SQL Server|9.2|mssql-jdbc-9.2.1.jre15.jar<br> mssql-jdbc-9.2.1.jre11.jar<br> mssql-jdbc-9.2.1.jre8.jar|29 janvier 2026|
|Microsoft JDBC Driver 8.4 pour SQL Server|8,4|mssql-jdbc-8.4.1.jre14.jar<br> mssql-jdbc-8.4.1.jre11.jar<br> mssql-jdbc-8.4.1.jre8.jar|31 juillet 2025|
|Microsoft JDBC Driver 8.2 pour SQL Server|8,2|mssql-jdbc-8.2.2.jre13.jar<br> mssql-jdbc-8.2.2.jre11.jar<br> mssql-jdbc-8.2.2.jre8.jar|31 janvier 2025|
|Microsoft JDBC Driver 7.4 pour SQL Server|7.4|mssql-jdbc-7.4.1.jre12.jar<br> mssql-jdbc-7.4.1.jre11.jar<br> mssql-jdbc-7.4.1.jre8.jar|31 juillet 2024|
|Microsoft JDBC Driver 7.2 pour SQL Server|7.2|mssql-jdbc-7.2.2.jre11.jar<br> mssql-jdbc-7.2.2.jre8.jar|31 janvier 2024|
|Microsoft JDBC Driver 7.0 pour SQL Server|7.0|mssql-jdbc-7.0.0.jre10.jar<br> mssql-jdbc-7.0.0.jre8.jar|31 juillet 2023|
|Microsoft JDBC Driver 6.4 pour SQL Server|6.4|mssql-jdbc-6.4.0.jre9.jar<br> mssql-jdbc-6.4.0.jre8.jar<br> mssql-jdbc-6.4.0.jre7.jar|27 février 2023|
|Microsoft JDBC Driver 6.2 pour SQL Server|6.2|mssql-jdbc-6.2.2.jre8.jar<br> mssql-jdbc-6.2.2.jre7.jar|30 juin 2022|
|Microsoft JDBC Driver 6.0 pour SQL Server|6.0|sqljdbc42.jar<br>sqljdbc41.jar|14 juillet 2021|
  
 Le support des pilotes Microsoft JDBC suivants est fini.  

|Nom du pilote|Version de package du pilote|Fin du support standard|  
|-|-|-|
|Microsoft JDBC Driver 4.2 pour SQL Server|4,2|24 août 2020|
|Microsoft JDBC Driver 4.1 pour SQL Server|4,1|12 décembre 2019|  
|Microsoft JDBC Driver 4.0 pour SQL Server|4.0|6 mars 2017|  
|Pilote JDBC Microsoft SQL Server 3.0|3.0|23 avril 2015|  
|Microsoft SQL Server JDBC Driver 2.0|2.0|31 décembre 2012|  
|Pilote JDBC Microsoft SQL Server 2005 1.2|1.2|25 juin 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.1|1.1|25 juin 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.0|1.0|25 juin 2011|  
|Microsoft SQL Server 2000 JDBC Driver|2000|9 juillet 2010|  
  
## <a name="sql-version-compatibility"></a>Compatibilité des versions de SQL  
  
|Version de la base de données&nbsp;&#8594;<br />Version du pilote &#8595;|Azure SQL Database|Azure Synapse Analytics|Azure SQL Managed Instance|SQL Server 2019|SQL Server 2017|SQL Server 2016|SQL Server 2014|SQL Server 2012|PDW 2008R2 AU3<sup>4</sup>|SQL Server 2008 R2|SQL Server 2008|
|---|---|---|---|---|---|---|---|---|---|---|---|
|9.2|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |   |   |
|8,4|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|8,2|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|7.4|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|Oui|   |   |
|7.2|Oui|Oui|Oui|   |Oui|Oui|Oui|Oui|Oui|Oui|   |
|7.0|Oui|Oui|Oui|   |Oui|Oui|Oui|Oui|Oui|Oui|   |
|6.4|Oui|Oui|Oui|   |Oui|Oui|Oui|Oui|Oui|Oui|   |
|6.2|Oui|Oui|   |   |Oui|Oui|Oui|Oui|Oui|Oui|Oui|
|6.1|Oui|   |   |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|6.0|Oui|   |   |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|4,2|Oui|   |   |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|4,1|Oui|   |   |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|4,0|Oui|   |   |   |   |Oui|Oui|Oui|Oui|Oui|Oui|
|3.0|Oui<sup>2</sup>|   |   |   |   |   |Oui<sup>5</sup>|Oui<sup>1</sup>|   |Oui|Oui|
|2,0|   |   |   |   |   |   |   |   |   |Oui<sup>3</sup>|Oui<sup>3</sup>|
|1.2|   |   |   |   |   |   |   |   |   |   |Oui<sup>3</sup>|

 <sup>1</sup> Microsoft SQL Server JDBC Driver version 3.0 peut se connecter à SQL Server 2012 comme client de bas niveau.  
  
 <sup>2</sup> Le support d’Azure SQL Database a été introduite dans le pilote 3.0 sous la forme d’un correctif. Nous recommandons aux clients d’Azure SQL Database d’utiliser la dernière version disponible du pilote.  
  
 <sup>3</sup> Microsoft SQL Server JDBC Driver version 2.0 et Microsoft SQL Server 2005 JDBC Driver version 1.2 peuvent se connecter à SQL Server 2008 comme client de bas niveau. Quand des conversions de bas niveau sont autorisées, les applications peuvent exécuter des requêtes et effectuer des mises à jour sur les nouveaux types de données SQL Server 2008, comme date, time, datetime2, datetimeoffset et FILESTREAM. Pour plus d’informations sur l’utilisation de ces nouveaux types de données avec le pilote JDBC, consultez  [Working with SQL Server 2008 Date/Time Data Types using JDBC Driver](/archive/blogs/jdbcteam/) et  [Working with SQL Server 2008 FileStream using JDBC Driver](/archive/blogs/jdbcteam/). Pour plus d’informations sur la compatibilité de bas niveau de ces nouveaux types de données, consultez les rubriques  [Utilisation des données de date et d’heure](/previous-versions/sql/sql-server-2008-r2/ms180878(v=sql.105))et  [FILESTREAM Support](../../relational-databases/native-client/features/filestream-support.md) dans la documentation en ligne de SQL Server.  
  
 <sup>4</sup> Support des connexions entre Microsoft JDBC Driver et Parallel Data Warehouse a été introduit dans Microsoft JDBC Driver 4.0 pour SQL Server et l’appliance Microsoft SQL Server 2008 R2 Parallel Data Warehouse mise à jour 3.  
  
 <sup>5</sup> Microsoft SQL Server JDBC Driver version 3.0 peut se connecter à SQL Server 2014 comme un client de bas niveau.  
  
## <a name="java-and-jdbc-specification-support"></a>Java et prise en charge de la spécification JDBC
  
|Version du pilote JDBC|Versions de JRE|Version de l’API JDBC|
|-|-|-|
|[9.2](release-notes-for-the-jdbc-driver.md#92)|1.8, 11, 15|4.2, 4.3 (partiellement)|
|[8,4](release-notes-for-the-jdbc-driver.md#84)|1.8, 11, 14|4.2, 4.3 (partiellement)|
|[8.2](release-notes-for-the-jdbc-driver.md#82)|1.8, 11, 13|4.2, 4.3 (partiellement)|
|[7.4](release-notes-for-the-jdbc-driver.md#74)|1.8, 11, 12|4.2, 4.3 (partiellement)|
|[7.2](release-notes-for-the-jdbc-driver.md#72)|1.8, 11|4.2, 4.3 (partiellement)|
|[7.0](release-notes-for-the-jdbc-driver.md#70)|1.8, 10|4.2, 4.3 (partiellement)|
|[6.4](release-notes-for-the-jdbc-driver.md#64)|1.7, 1.8, 9|4.1, 4.2, 4.3 (partiellement)|
|[6.2](release-notes-for-the-jdbc-driver.md#62)|1.7, 1.8|4.1, 4.2|
|[6.1](release-notes-for-the-jdbc-driver.md#61)|1.7, 1.8|4.1, 4.2|
|[6.0](release-notes-for-the-jdbc-driver.md#60)|1.7, 1.8|4.1, 4.2|
|4,2|1.7, 1.8|4.1, 4.2|
|4,1|1.7|4.0|
|4.0|1.5, 1.6, 1.7|3.0, 4.0|
|3.0|1.5, 1.6,|3.0, 4.0|
|2.0|1.5, 1.6|3.0, 4.0|
|1.2|1.4, 1.5, 1.6|3.0|
|1.1|1.4|3.0|
|1.0|1.4|3.0|
|2000|1.4|3.0|

## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge

Le pilote Microsoft JDBC est conçu pour fonctionner sur tout système d’exploitation prenant en charge l’utilisation d’une machine virtuelle Java (JVM). Les plateformes couramment utilisées sont notamment Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2008 R2, Linux, Unix, AIX, macOS et autres.  

L’équipe du produit JDBC teste notre pilote sur Windows, Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux et macOS.

## <a name="application-server-support"></a>Prise en charge des serveurs d’applications

Le pilote Microsoft JDBC pour SQL Server est testé avec différents serveurs d’applications.  Pour plus d’informations sur la version du pilote compatible avec leur produit, consultez le fournisseur de votre serveur d’applications.