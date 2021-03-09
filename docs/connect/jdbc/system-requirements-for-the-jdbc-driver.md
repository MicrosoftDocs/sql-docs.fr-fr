---
description: Configuration requise pour le pilote JDBC
title: Configuration requise pour le pilote JDBC | Microsoft Docs
ms.custom: ''
ms.date: 02/26/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 447792bb-f39b-49b4-9fd0-1ef4154c74ab
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 56ce0af2be925244e4ca2013b40d2b98060bc974
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464797"
---
# <a name="system-requirements-for-the-jdbc-driver"></a>Configuration requise pour le pilote JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Pour accéder aux données d’une base de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] à l’aide du [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)], vous devez installer les composants suivants sur votre ordinateur :

- [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] ([Télécharger](download-microsoft-jdbc-driver-for-sql-server.md))
- Java Runtime Environment

## <a name="java-runtime-environment-requirements"></a>Configuration requise pour Java Runtime Environment  

 À compter de Microsoft JDBC Driver 9.2 pour SQL Server, le Kit JDK (Java Development Kit) version 15.0 et l’environnement JRE (Java Runtime Environment) version 15.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 8.4 pour SQL Server, le Kit JDK (Java Development Kit) version 14.0 et l’environnement JRE (Java Runtime Environment) version 14.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 8.2 pour SQL Server, le Kit JDK (Java Development Kit) version 13.0 et l’environnement JRE (Java Runtime Environment) version 13.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 7.4 pour SQL Server, le Kit JDK (Java Development Kit) version 12.0 et l’environnement JRE (Java Runtime Environment) version 12.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 7.2 pour SQL Server, le Kit JDK (Java Development Kit) version 11.0 et l’environnement JRE (Java Runtime Environment) version 11.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 7.0 pour SQL Server, le Kit JDK (Java Development Kit) version 10.0 et l’environnement JRE (Java Runtime Environment) version 10.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 6.4 pour SQL Server, le Kit JDK (Java Development Kit) version 9.0 et l’environnement JRE (Java Runtime Environment) version 9.0 sont pris en charge.

 À compter de Microsoft JDBC Driver 4.2 pour SQL Server, le Kit JDK (Java Development Kit) version 8.0 et l’environnement JRE (Java Runtime Environment) version 8.0 sont pris en charge. La prise en charge de l’API Spec JDBC a été étendue pour inclure l’API JDBC 4.1 et 4.2.
  
 À compter de Microsoft JDBC Driver 4.1 pour SQL Server, le Kit JDK (Java Development Kit) version 7.0 et l’environnement JRE (Java Runtime Environment) version 7.0 sont pris en charge.
  
 À compter de [!INCLUDE[jdbc_40](../../includes/jdbc_40_md.md)], la prise en charge de l’API Spec JDBC par le pilote JDBC a été étendue pour inclure l’API JDBC 4.0. L’API JDBC 4.0 a été introduite dans le cadre du kit JDK (Java Development Kit) version 6.0 et de l’environnement JRE (Java Runtime Environment) version 6.0. JDBC 4.0 est un surensemble de l'API JDBC 3.0.
  
 Quand vous déployez le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] sur les systèmes d’exploitation Windows et UNIX, vous devez utiliser les packages d’installation, respectivement *sqljdbc_\<version>_enu.exe* et *sqljdbc_\<version>_enu.tar.gz*. Pour plus d'informations sur le déploiement du pilote JDBC, consultez la rubrique [Déployer le pilote JDBC](../../connect/jdbc/deploying-the-jdbc-driver.md).  

**Microsoft JDBC Driver 9.2 pour SQL Server :**  

  JDBC Driver 9.2 inclut trois bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-9.2.1.jre8.jar**, **mssql-jdbc-9.2.1.jre11.jar** et **mssql-jdbc-9.2.1.jre15.jar**.

  JDBC Driver 9.2 est conçu pour fonctionner et être pris en charge par toutes les machines virtuelles Java principales ; il est cependant testé seulement sur OpenJDK 1.8, OpenJDK 11.0, OpenJDK 15.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 et Azul Zulu JRE 15.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les pilotes Microsoft JDBC 9.2 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-9.2.1.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 1.8. JRE 1.7 et les versions antérieures lèvent une exception.<br /><br /> Les nouvelles fonctionnalités de la version 9.2 incluent : La prise en charge de JDK 15, la prise en charge de l’authentification interactive Azure Active Directory, la prise en charge de l’authentification par principal de service Azure Active Directory et la prise en charge de useBulkCopyForBatchInsert pour les serveurs non-Azure Synapse Analytics. |
|mssql-jdbc-9.2.1.jre11.jar|4.3|11|Nécessite un environnement JRE (Java Runtime Environment) 11.0. JRE 10.0 et les versions antérieures lèvent une exception.<br /><br /> Les nouvelles fonctionnalités de la version 9.2 incluent : La prise en charge de JDK 15, la prise en charge de l’authentification interactive Azure Active Directory, la prise en charge de l’authentification par principal de service Azure Active Directory et la prise en charge de useBulkCopyForBatchInsert pour les serveurs non-Azure Synapse Analytics. |
|mssql-jdbc-9.2.1.jre15.jar|4.3|15|Nécessite Java Runtime Environment (JRE) 15.0. L’utilisation de JRE 14.0 ou antérieur lève une exception.<br /><br /> Les nouvelles fonctionnalités de la version 9.2 incluent : La prise en charge de JDK 15, la prise en charge de l’authentification interactive Azure Active Directory, la prise en charge de l’authentification par principal de service Azure Active Directory et la prise en charge de useBulkCopyForBatchInsert pour les serveurs non-Azure Synapse Analytics. |

  JDBC Driver 9.2 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, ajoutez le code suivant dans le fichier POM.XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.4 pour SQL Server :**  

  JDBC Driver 8.4 comporte trois bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-8.4.1.jre8.jar**, **mssql-jdbc-8.4.1.jre11.jar** et **mssql-jdbc-8.4.1.jre14.jar**.

  JDBC Driver 8.4 est conçu pour fonctionner et être pris en charge par toutes les principales machines virtuelles Java. Toutefois, il est testé uniquement sur OpenJDK 1.8, OpenJDK 11.0, OpenJDK 14.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 et Azul Zulu JRE 14.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 8.4 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.4.1.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 1.8. JRE 1.7 et les versions antérieures lèvent une exception.<br /><br /> Les nouvelles fonctionnalités de la version 8.4 incluent : le support du kit JDK 14, le support de l’authentification pour Azure Key Vault à l’aide de l’identité managée, la prise en charge étendue de la copie en bloc pour Azure Data Warehouse, la mise en cache DNS SQL Azure, le support de la compatibilité descendante pour la diffusion en continu d’objets LOB et l’authentification du certificat client pour des scénarios de bouclage. |
|mssql-jdbc-8.4.1.jre11.jar|4.3|11|Nécessite un environnement JRE (Java Runtime Environment) 11.0. JRE 10.0 et les versions antérieures lèvent une exception.<br /><br /> Les nouvelles fonctionnalités de la version 8.4 incluent : le support du kit JDK 14, le support de l’authentification pour Azure Key Vault à l’aide de l’identité managée, la prise en charge étendue de la copie en bloc pour Azure Data Warehouse, la mise en cache DNS SQL Azure, le support de la compatibilité descendante pour la diffusion en continu d’objets LOB et l’authentification du certificat client pour des scénarios de bouclage. |
|mssql-jdbc-8.4.1.jre13.jar|4.3|14|Nécessite Java Runtime Environment (JRE) 14.0. L’utilisation de JRE 13.0 ou versions antérieures lèvent une exception.<br /><br /> Les nouvelles fonctionnalités de la version 8.4 incluent : le support du kit JDK 14, le support de l’authentification pour Azure Key Vault à l’aide de l’identité managée, la prise en charge étendue de la copie en bloc pour Azure Data Warehouse, la mise en cache DNS SQL Azure, le support de la compatibilité descendante pour la diffusion en continu d’objets LOB et l’authentification du certificat client pour des scénarios de bouclage. |

  JDBC Driver 8.4 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, ajoutez le code suivant dans le fichier POM. XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 8.2 pour SQL Server** :  

  Le pilote JDBC 8.2 comporte trois bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-8.2.2.jre8.jar**, **mssql-jdbc-8.2.2.jre11.jar** et **mssql-jdbc-8.2.2.jre13.jar**.

  JDBC Driver 8.2 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur OpenJDK 1.8, OpenJDK 11.0, OpenJDK 13.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 et Azul Zulu JRE 13.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 8.2 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-8.2.2.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 1.8. JRE 1.7 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 8.2 : prise en charge de JDK 13, Always Encrypted avec enclaves sécurisées et améliorations des performances avec des données temporelles. |
|mssql-jdbc-8.2.2.jre11.jar|4.3|11|Nécessite un environnement JRE (Java Runtime Environment) 11.0. JRE 10.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 8.2 : prise en charge de JDK 13, Always Encrypted avec enclaves sécurisées et améliorations des performances avec des données temporelles. |
|mssql-jdbc-8.2.2.jre13.jar|4.3|13|Nécessite Java Runtime Environment (JRE) 13.0. JRE 11.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 8.2 : prise en charge de JDK 13, Always Encrypted avec enclaves sécurisées et améliorations des performances avec des données temporelles. |

  JDBC Driver 8.2 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, ajoutez le code suivant dans le fichier POM. XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>8.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.4 pour SQL Server :**  

  JDBC Driver 7.4 comporte trois bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-7.4.1.jre8.jar**, **mssql-jdbc-7.4.1.jre11.jar** et **mssql-jdbc-7.4.1.jre12.jar**.

  JDBC Driver 7.4 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur OpenJDK 1.8, OpenJDK 11.0, OpenJDK 12.0, Azul Zulu JRE 1.8, Azul Zulu JRE 11.0 et Azul Zulu JRE 12.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 7.4 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.4.1.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 1.8. JRE 1.7 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.4 : prise en charge de JDK 12, authentification NTLM et useFmtOnly. |  
|mssql-jdbc-7.4.1.jre11.jar|4.3|11|Nécessite un environnement JRE (Java Runtime Environment) 11.0. JRE 10.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.4 : prise en charge de JDK 12, authentification NTLM et useFmtOnly. |  
|mssql-jdbc-7.4.1.jre12.jar|4.3|12|Nécessite Java Runtime Environment (JRE) 12.0. JRE 11.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.4 : prise en charge de JDK 12, authentification NTLM et useFmtOnly. |  

  JDBC Driver 7.4 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, insérez le code suivant dans le fichier POM. XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.4.1.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.2 pour SQL Server :**  

  JDBC Driver 7.2 comporte deux bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-7.2.2.jre8.jar** et **mssql-jdbc-7.2.2.jre11.jar**.

  JDBC Driver 7.2 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci ; cependant, il est testé seulement sur OpenJDK 8.0, OpenJDK 11.0, Azul Zulu JRE 8.0 et Azul Zulu JRE 11.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 7.2 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.2.2.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 8.0. JRE 7.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.2 : support du kit JDK 11, authentification de l’identité managée Active Directory, support d’OSGi, API SQLServerError. |  
|mssql-jdbc-7.2.2.jre11.jar|4.3|10|Nécessite un environnement JRE (Java Runtime Environment) 11.0. JRE 10.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.2 : support du kit JDK 11, authentification de l’identité managée Active Directory, support d’OSGi, API SQLServerError. |  

  JDBC Driver 7.2 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, insérez le code suivant dans le fichier POM. XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.2.2.jre11</version>
</dependency>
```

**Microsoft JDBC Driver 7.0 pour SQL Server :**  

  JDBC Driver 7.0 comporte deux bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-7.0.0.jre8.jar** et **mssql-jdbc-7.0.0.jre10.jar**.

  JDBC Driver 7.0 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur OpenJDK 8.0 et 10.0.
  
  Le graphique suivant récapitule le support fourni par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 7.0 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-7.0.0.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 8.0. JRE 7.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.0 : prise en charge de JDK 10, mise à jour du niveau de conformité par défaut pour les spécifications de JDBC 4.2, prise en charge des types de données spatiaux, propriété de connexion cancelQueryTimeout, méthodes Request Boundary, propriété de connexion useBulkCopyForBatchInsert, informations de découverte et de classification de données, extension de la fonctionnalité UTF-8 et prise en charge de CityHash. |  
|mssql-jdbc-7.0.0.jre10.jar|4.3|10|Nécessite Java Runtime Environment (JRE) 10.0. JRE 9.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 7.0 : prise en charge de JDK 10, mise à jour du niveau de conformité par défaut pour les spécifications de JDBC 4.2, prise en charge des types de données spatiaux, propriété de connexion cancelQueryTimeout, méthodes Request Boundary, propriété de connexion useBulkCopyForBatchInsert, informations de découverte et de classification de données, extension de la fonctionnalité UTF-8 et prise en charge de CityHash. |  

  JDBC Driver 7.0 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, insérez le code suivant dans le fichier POM. XML :  
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>7.0.0.jre10</version>
</dependency>
```
  
**Microsoft JDBC Driver 6.4 pour SQL Server :**  

  JDBC Driver 6.4 comporte trois bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-6.4.0.jre7.jar**, **mssql-jdbc-6.4.0.jre8.jar** et **mssql-jdbc-6.4.0.jre9.jar**.

  JDBC Driver 6.4 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur OpenJDK 7.0, 8.0 et 9.0.
  
  Le graphique suivant récapitule les versions prises en charge par les trois fichiers JAR inclus dans les Pilotes Microsoft JDBC 6.4 pour SQL Server :  
  
  |JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|mssql-jdbc-6.4.0.jre7.jar|4,1|7|Nécessite Java Runtime Environment (JRE) 7.0. JRE 6.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 6.4 : authentification Azure AD pour Linux, méthode principal/mot de passe pour Kerberos, détection automatique du domaine dans le SPN pour l’authentification entre domaines, délégation Kerberos contrainte, délai d’expiration de requête, délai d’expiration de socket et réutilisation de la gestion des instructions préparées. |  
|mssql-jdbc-6.4.0.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 8.0. JRE 7.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 6.4 : authentification Azure AD pour Linux, méthode principal/mot de passe pour Kerberos, détection automatique du domaine dans le SPN pour l’authentification entre domaines, délégation Kerberos contrainte, délai d’expiration de requête, délai d’expiration de socket et réutilisation de la gestion des instructions préparées. |  
|mssql-jdbc-6.4.0.jre9.jar|4.3|9|Nécessite Java Runtime Environment (JRE) 9.0. JRE 8.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 6.4 : authentification Azure AD pour Linux, méthode principal/mot de passe pour Kerberos, détection automatique du domaine dans le SPN pour l’authentification entre domaines, délégation Kerberos contrainte, délai d’expiration de requête, délai d’expiration de socket et réutilisation de la gestion des instructions préparées. |

JDBC Driver 6.4 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, insérez le code suivant dans le fichier POM. XML :

 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.4.0.jre9</version>
</dependency>
```

**Microsoft JDBC Driver 6.2 pour SQL Server :**  
  
  JDBC Driver 6.2 inclut deux bibliothèques de classes JAR dans chaque package d’installation : **mssql-jdbc-6.2.2.jre7.jar** et **mssql-jdbc-6.2.2.jre8.jar**.
  
 JDBC Driver 6.2 est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur Sun JRE 5.0, 6.0, 7.0 et 8.0.
  
 Le graphique suivant récapitule les versions prises en charge par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 6.0 et 4.2 pour SQL Server :  
  
|JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|
|mssql-jdbc-6.2.2.jre7.jar|4,1|7|Nécessite Java Runtime Environment (JRE) 7.0. JRE 6.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 6.2 : authentification Azure AD pour Linux, méthode principal/mot de passe pour Kerberos, détection automatique du domaine dans le SPN pour l’authentification entre domaines, délégation Kerberos contrainte, délai d’expiration de requête, délai d’expiration de socket et réutilisation de la gestion des instructions préparées. |  
|mssql-jdbc-6.2.3.jre8.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 8.0. JRE 7.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités de la version 6.2 : authentification Azure AD pour Linux, méthode principal/mot de passe pour Kerberos, détection automatique du domaine dans le SPN pour l’authentification entre domaines, délégation Kerberos contrainte, délai d’expiration de requête, délai d’expiration de socket et réutilisation de la gestion des instructions préparées|  

  JDBC Driver 6.2 est également disponible sur le référentiel central Maven ; pour l’ajouter à un projet Maven, insérez le code suivant dans le fichier POM. XML :
  
 ```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>6.2.2.jre8</version>
</dependency>
```

 **Microsoft JDBC Driver 6.0 et 4.2 pour SQL Server :**  
  
  JDBC Driver 6.0 et 4.2 comportent deux bibliothèques de classes JAR dans chaque package d’installation : **sqljdbc41.jar** et **sqljdbc42.jar**.
  
 Les pilotes JDBC Driver 6.0 et 4.2 sont conçus pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé uniquement sur Sun JRE 5.0, 6.0, 7.0 et 8.0.
  
 Le graphique suivant récapitule les versions prises en charge par les deux fichiers JAR inclus dans les Pilotes Microsoft JDBC 6.0 et 4.2 pour SQL Server :  
  
|JAR|Compatibilité avec la version de JDBC|Version de Java recommandée|Description|  
|---------|-----------------------------|----------------------|-----------------|  
|sqljdbc41.jar|4,1|7|Nécessite Java Runtime Environment (JRE) 7.0. JRE 6.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités des packages 6.0 et 4.2 : conformité JDBC 4.1 et copie en bloc.<br /><br /> Nouvelles fonctionnalités du package 6.0 uniquement : Always Encrypted, paramètres table, authentification Azure Active Directory, connexions transparentes aux groupes de disponibilité AlwaysOn, amélioration de la récupération des métadonnées de paramètre pour les requêtes préparées et nom de domaine international (IDN).|  
|sqljdbc42.jar|4,2|8|Nécessite Java Runtime Environment (JRE) 8.0. JRE 7.0 et les versions antérieures lèvent une exception.<br /><br /> Nouvelles fonctionnalités des packages 6.0 et 4.2 : conformité JDBC 4.1 et 4.2 et copie en bloc.<br /><br /> Nouvelles fonctionnalités du package 6.0 uniquement : Always Encrypted, paramètres table, authentification Azure Active Directory, connexions transparentes aux groupes de disponibilité AlwaysOn, amélioration de la récupération des métadonnées de paramètre pour les requêtes préparées et nom de domaine international (IDN).|  
  
 **Microsoft JDBC Driver 4.1 pour SQL Server :**  
  
 JDBC Driver 4.1 comporte deux bibliothèques de classes JAR dans chaque package d’installation : **sqljdbc41.jar**.  

|JAR|Description|  
|---------|-----------------|  
|sqljdbc41.jar|La bibliothèque de classes **sqljdbc41.jar** prend en charge l’API JDBC 4.0. Elle inclut toutes les fonctionnalités du pilote JDBC 4.0 et les méthodes de l’API JDBC 4.0. JDBC 4.1 n’est pas pris en charge (il lève une exception « SQLFeatureNotSupportedException »).<br /><br /> La bibliothèque de classes **sqljdbc41.jar** nécessite la version 7.0 de l’environnement d’exécution Java (JRE, Java Runtime Environment). **sqljdbc41.jar** sur JRE 6.0 et 5.0 lève une exception.<br /><br />
  
 Le pilote JDBC est conçu pour fonctionner avec toutes les principales machines virtuelles Java et être pris en charge par celles-ci. Toutefois, il est testé sur Sun JRE 5.0, 6.0 et 7.0.
  
 Le graphique suivant récapitule les versions prises en charge par le fichier JAR inclus dans Microsoft JDBC Driver 4.1 pour SQL Server.  
  
|JAR|Version JDBC|JRE (exécution possible)|JDK (compilation possible)|  
|---------|------------------|---------------------|-------------------------|  
|sqljdbc41.jar|4|7|7 6 5|  
  
## <a name="sql-server-requirements"></a>Configuration requise pour SQL Server  

 Le pilote JDBC prend en charge les connexions à Azure SQL Database et à SQL Server. Pour les pilotes Microsoft JDBC 4.2 et 4.1 pour SQL Server, la prise en charge commence avec SQL Server 2008.
  
## <a name="operating-system-requirements"></a>Système d'exploitation requis  

 Le pilote JDBC est conçu pour fonctionner sur tout système d'exploitation prenant en charge l'utilisation d'une machine virtuelle Java (JVM). Cependant, seuls les systèmes d’exploitation Sun Solaris, SUSE Linux, Ubuntu Linux, CentOS Linux, macOS et Windows ont été officiellement testés.  
  
## <a name="supported-languages"></a>Langues prises en charge  

 Le pilote JDBC prend en charge tous les classements de colonnes [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d'informations sur les classements pris en charge par le pilote JDBC, consultez [Fonctionnalités internationales du pilote JDBC](../../connect/jdbc/international-features-of-the-jdbc-driver.md).  
  
 Pour plus d’informations sur les classements, consultez la rubrique « Utilisation des classements » dans la documentation en ligne de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  

 [Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
