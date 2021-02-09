---
title: Télécharger Microsoft JDBC Driver pour SQL Server
description: Télécharger le pilote Microsoft JDBC pour SQL Server pour développer des applications Java qui se connectent à SQL Server et Azure SQL Database.
ms.date: 01/29/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 451181b8-11e6-4d01-b547-9ac5aada8238
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 76ab3aac455b7fa230f7dcf850f78dcd0fcfc96d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99176280"
---
# <a name="download-microsoft-jdbc-driver-for-sql-server"></a>Télécharger Microsoft JDBC Driver pour SQL Server

Le pilote Microsoft JDBC pour SQL Server est un pilote JDBC de type 4 offrant une connectivité de base de données par le biais des API JDBC standard, disponibles sur la plateforme Java. Les pilotes sont téléchargeables gratuitement pour tous les utilisateurs. Ils permettent d’accéder à SQL Server à partir d’une application Java, d’un serveur d’applications ou d’une applet Java.

## <a name="download"></a>Téléchargement

La version 9.2 est la version en disponibilité générale la plus récente. Elle prend en charge Java 8, 11 et 15. Si vous avez besoin de l’exécuter sur un runtime Java antérieur à celui-ci, consultez la matrice [Java et prise en charge de la spécification JDBC](microsoft-jdbc-driver-for-sql-server-support-matrix.md#java-and-jdbc-specification-support) pour voir s’il existe une version du pilote prise en charge que vous pouvez utiliser. Nous améliorons constamment la prise en charge de la connectivité Java. Nous vous recommandons donc vivement d’utiliser la dernière version de Microsoft JDBC Driver.

**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2 pour SQL Server (zip)](https://go.microsoft.com/fwlink/?linkid=2153622)**  
**[![Télécharger](../../ssms/media/download-icon.png) Télécharger Microsoft JDBC Driver 9.2 pour SQL Server (tar.gz)](https://go.microsoft.com/fwlink/?linkid=2153521)**  

### <a name="version-information"></a>Informations sur la version

- Numéro de version : 9.2.0
- Publication : 29 janvier 2021

Lorsque vous téléchargez le pilote, il y a plusieurs fichiers JAR. Le nom du fichier JAR indique la version de Java qu’il prend en charge.

> [!Note]
> Si vous accédez à cette page à partir d’une version autre que l’anglais et que vous souhaitez voir le contenu le plus à jour, consultez la [version anglaise (États-Unis) du site](). Vous pouvez télécharger différentes langues à partir du site en version anglaise (États-Unis) en sélectionnant [Langues disponibles](#available-languages).

## <a name="available-languages"></a>Langues disponibles

Cette version du pilote Microsoft JDBC pour SQL Server est disponible dans les langues suivantes :

Microsoft JDBC Driver 9.2.0 pour SQL Server (zip) : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153622&clcid=0x40a)

Microsoft JDBC Driver 9.2.0 pour SQL Server (tar.gz) : [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2153521&clcid=0x40a)

### <a name="release-notes"></a>Notes de publication

Pour plus d’informations sur cette version, consultez les [notes de publication](release-notes-for-the-jdbc-driver.md) et la [configuration requise](system-requirements-for-the-jdbc-driver.md).

### <a name="previous-releases"></a>Versions précédentes

Pour télécharger les versions précédentes, consultez [Versions précédentes du pilote Microsoft JDBC pour SQL Server](release-notes-for-the-jdbc-driver.md#previous-releases).

## <a name="using-the-jdbc-driver-with-maven-central"></a>Utilisation du pilote JDBC avec Maven Central

Le pilote JDBC peut être ajouté à un projet Maven en tant que dépendance dans le fichier POM.xml avec le code suivant :

```xml
<dependency>
    <groupId>com.microsoft.sqlserver</groupId>
    <artifactId>mssql-jdbc</artifactId>
    <version>9.2.0.jre11</version>
</dependency>
```  

## <a name="unsupported-drivers"></a>Pilotes non pris en charge

Les versions du pilote non prises en charge ne sont pas proposées en téléchargement ici. Nous améliorons constamment la prise en charge de la connectivité Java. Nous vous recommandons donc vivement d’utiliser la dernière version de Microsoft JDBC Driver.  
  
## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le pilote Microsoft JDBC pour SQL Server, consultez [Vue d’ensemble du pilote JDBC](overview-of-the-jdbc-driver.md) et le [référentiel GitHub du pilote JDBC](https://github.com/microsoft/mssql-jdbc/blob/dev/README.md).