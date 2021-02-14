---
title: Bibliothèques de connexions pour Microsoft SQL Database | Microsoft Docs
description: Fournit des liens de téléchargement de modules qui permettent de se connecter à Microsoft SQL Server et Azure SQL Database à partir d’un large éventail de langages de programmation côté client.
author: David-Engel
ms.prod: sql
ms.technology: connectivity
ms.custom: ''
ms.topic: article
ms.date: 03/06/2020
ms.author: v-daenge
ms.openlocfilehash: b64d5d6d20e7d83385b040a49516cb9728b5f048
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058144"
---
# <a name="connection-modules-for-microsoft-sql-database"></a>Modules de connexion pour les bases de données SQL Microsoft

Cet article fournit des liens de téléchargement de modules de connexion ou de *pilotes* permettant aux programmes clients d’interagir avec [Microsoft SQL Server](../relational-databases/databases/databases.md) et son équivalent dans le cloud, [Azure SQL Database](/azure/sql-database/). Les pilotes sont disponibles pour un large éventail de langages de programmation, sur les systèmes d’exploitation suivants :

- Linux
- macOS
- Windows

**Incompatibilité POO-relationnel :**

*Relationnel* : les programmes clients écrits dans un langage de programmation orientée objet utilisent souvent des pilotes SQL qui retournent les données interrogées dans un format plus relationnel qu’orienté objet. C# avec ADO.NET en est un exemple. L’incompatibilité de format POO-relationnel rend parfois le code POO plus difficile à écrire et à comprendre.

*ORM* : d’autres pilotes ou frameworks renvoient les données interrogées au format POO, ce qui évite l’incompatibilité. Ces pilotes attendent que des classes soient définies pour correspondre aux colonnes de données de certaines tables SQL. Ils effectuent ensuite le *mappage objet-relationnel* (ORM) pour retourner les données interrogées en tant qu’instance d’une classe. Microsoft Entity Framework (EF) pour C#, et Hibernate pour Java, en sont deux exemples.

Le présent article consacre des sections distinctes à ces deux types de pilotes de connexion.

<a name="anchor-20-drivers-relational-access" />

## <a name="drivers-for-relational-access"></a>Pilotes pour l’accès relationnel

| Langage | Téléchargement du pilote SQL |
| :------- | :---------------------- |
| C# | [ADO.NET](https://www.microsoft.com/net/download/)<br />[Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/)<br /><br />[.NET Core pour : Linux-Ubuntu, macOS, Windows](https://dotnet.microsoft.com/download) |
| C++ | [ODBC](./odbc/download-odbc-driver-for-sql-server.md)<br /><br />[OLE DB](./oledb/download-oledb-driver-for-sql-server.md) |
| Java | [JDBC](./jdbc/download-microsoft-jdbc-driver-for-sql-server.md) |
| Node.js | [Pilote Node.js, instructions d’installation](./node-js/step-1-configure-development-environment-for-node-js-development.md) |
| PHP | [PHP](./php/download-drivers-php-sql-server.md) |
| Python | [pyodbc, instructions d’installation](./python/pyodbc/step-1-configure-development-environment-for-pyodbc-python-development.md)<br />[Télécharger ODBC](./odbc/download-odbc-driver-for-sql-server.md) |
| Ruby | [Pilote Ruby, instructions d’installation](./ruby/step-1-configure-development-environment-for-ruby-development.md)<br />[Ruby, page d’installation](https://rubyinstaller.org/downloads/) |
| &nbsp; | &nbsp; |

<a name="anchor-40-drivers-orm-access" />

## <a name="drivers-for-orm-access"></a>Pilotes pour l’accès ORM

Le tableau suivant présente des exemples d’infrastructures de Mappage Objet Relationnel (ORM) utilisés par les applications clientes pour se connecter à Microsoft SQL Database.

| Langage | Téléchargement du pilote ORM |
| :------- | :------------------ |
| C# | [Entity Framework Core](/ef/core/)<br />[Entity Framework (6.x ou version ultérieure)](/ef/) |
| Java | [Mettre en veille prolongée](https://hibernate.org/orm)|
| PHP | [Eloquent ORM, inclus dans l’installation de Laravel](https://laravel.com/docs/) |
| Node.js | [Sequelize ORM](https://sequelize.org/) |
| Python | [Django](https://www.djangoproject.com/) |
| Ruby | [Ruby on rails](https://rubyonrails.org/) |
| &nbsp; | &nbsp; |

<a name="anchor-60-build-an-app-webpages" />

## <a name="build-an-app-webpages"></a>Pages web Créer une application

**[https://aka.ms/sqldev](https://aka.ms/sqldev)** vous permet d’accéder à un ensemble de pages *Build-an-app*, qui donnent des informations sur les nombreuses combinaisons possibles de langage de programmation, système d’exploitation et pilote de connexion SQL :

- Premiers pas, pour chaque combinaison de langue, système d’exploitation et pilote.
  - Instructions d’installation des derniers pilotes de connexion SQL.
- Exemples de code pour chacun des éléments suivants :
  - Exemples de code objet-relationnel.
  - Exemples de code ORM.
  - Démonstrations d’index columnstore pour des performances beaucoup plus rapides.

**Pages Build-an-app – Première page :**  
![Pages Build-an-app, capture d’écran de la première page](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png)

**Pages Build-an-app – Menu pour Java-Ubuntu**  
![Pages Build-an-app, menu Java Ubuntu](media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png)

&nbsp;

## <a name="related-links"></a>Liens connexes

- [Exemples de code de connexion à Azure SQL Database dans le cloud, avec Java et d’autres langages](/azure/sql-database/sql-database-connect-query-java).

<!--
Image references, **obsolete** markdown syntax alternative:

![Build-an-app webpages, first page screenshot][image-ref-163-buildanapp-webpages-first-page]
![Build-an-app webpages, menu Java Ubuntu][image-ref-167-buildanapp-webpages-menu-java-ubuntu]

[image-ref-163-buildanapp-webpages-first-page]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-choose-language-g21.png
[image-ref-167-buildanapp-webpages-menu-java-ubuntu]: ./media/homepage-sql-connection-drivers/gm-aka-ms-sqldev-java-ubuntu-c31.png
-->