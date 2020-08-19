---
description: Se connecter à une source de données PostgreSQL (Assistant Importation et Exportation SQL Server)
title: Se connecter à une source de données PostgreSQL (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: b7a75a72-b267-444f-9eb8-d23eb333fc35
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 023c791e27fba3c26ac3ccd9778f0beee44536b9
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88495592"
---
# <a name="connect-to-a-postgresql-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données PostgreSQL (Assistant Importation et Exportation SQL Server)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Cette rubrique vous montre comment se connecter à une source de données **PostgreSQL** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server. 

> [!IMPORTANT]
> Les exigences et prérequis détaillés pour la connexion à une base de données PostgreSQL n’entrent pas dans le cadre de cet article Microsoft. Cet article suppose que vous avez déjà installé le logiciel client PostgreSQL et que vous pouvez aussi vous connecter à la base de données PostgreSQL cible. Pour plus d’informations, consultez votre administrateur de base de données PostgreSQL ou la documentation PostgreSQL.

## <a name="get-the-postgresql-odbc-driver"></a>Se procurer le pilote ODBC PostgreSQL

### <a name="install-the-odbc-driver-with-stack-builder"></a>Installer le pilote ODBC à l’aide de Stack Builder
Exécutez Stack Builder pour ajouter le pilote ODBC PostgreSQL (psqlODBC) à votre installation de PostgreSQL.

![Installation du pilote ODBC PostgreSQL à l’aide de Stack Builder](../../integration-services/import-export-data/media/install-postgresql-odbc-with-stack-builder.png)

### <a name="or-download-the-latest-odbc-driver"></a>Ou télécharger le dernier pilote ODBC
Vous pouvez aussi télécharger le Windows Installer pour la dernière version du pilote ODBC PostgreSQL (psqlODBC) directement à partir de ce site FTP : [https://www.postgresql.org/ftp/odbc/versions/msi/](https://www.postgresql.org/ftp/odbc/versions/msi/). Extrayez les fichiers du fichier .zip et exécutez le fichier .msi.

## <a name="connect-to-postgresql-with-the-postgresql-odbc-driver-psqlodbc"></a>Se connecter à PostgreSQL avec le pilote ODBC PostgreSQL (psqlODBC)
Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Pour vous connecter avec un pilote ODBC, commencez par sélectionner le **Fournisseur de données .NET Framework pour ODBC** comme source de données dans la page **Choisir une source de données** ou **Choisir une destination**. Ce fournisseur agit comme un wrapper autour du pilote ODBC.

Voici l’écran générique que vous voyez immédiatement après avoir sélectionné le fournisseur de données .NET Framework pour ODBC.

![Connexion à PostgreSQL avec ODBC avant](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-before.jpg)

### <a name="options-to-specify-postgresql-odbc-driver"></a>Options à spécifier (pilote ODBC PostgreSQL)

> [!NOTE]
> Les options de connexion de ce fournisseur de données et pilote ODBC sont les mêmes, que PostgreSQL soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

Pour vous connecter à PostgreSQL au moyen du pilote ODBC PostgreSQL, assemblez une chaîne de connexion qui inclut les paramètres suivants accompagnés de leur valeur. Le format d’une chaîne de connexion complète est donné immédiatement après la liste des paramètres.

> [!TIP]
> Obtenez de l’aide pour l’assemblage d’une chaîne de connexion correcte. Au lieu de produire une chaîne de connexion, vous pouvez fournir un nom de source de données (DSN, data source name) existant ou en créer un. Pour obtenir plus d’informations sur ces options, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

**Driver**  
Nom du pilote ODBC, soit **PostgreSQL ODBC Driver(UNICODE)** ou **PostgreSQL ODBC Driver(ANSI)**.

**Serveur**  
Nom du serveur PostgreSQL. 

**Port**  
Port à utiliser pour se connecter au serveur PostgreSQL.

**Sauvegarde de la base de données**  
Nom de la base de données PostgreSQL.

**UID** et **Pwd**   
**Uid** (id d’utilisateur) et **Pwd** (mot de passe) pour se connecter.

### <a name="connection-string-format"></a>Format de la chaîne de connexion
Voici le format d’une chaîne de connexion standard. 

```console
Driver={PostgreSQL ODBC Driver(UNICODE)};Server=<server>;Port=<port>;Database=<database>;UID=<user id>;PWD=<password>
```

### <a name="enter-the-connection-string"></a>Entrer la chaîne de connexion
Indiquez la chaîne de connexion dans le champ **ConnectionString** ou entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une Destination**. Une fois que vous avez entré la chaîne de connexion, l’Assistant analyse cette chaîne et affiche les propriétés individuelles avec leur valeur dans la liste.

L’exemple suivant utilise cette chaîne de connexion.

```console
Driver={PostgreSQL ODBC Driver(UNICODE)};Server=127.0.0.1;Port=5432;Database=postgres;UID=postgres;PWD=********
```

Voici l’écran que vous voyez après avoir entré la chaîne de connexion.

![Connexion à PostgreSQL avec ODBC](../../integration-services/import-export-data/media/connect-to-postgresql-with-odbc.png)

## <a name="other-data-providers-and-more-info"></a>Autres fournisseurs de données et renseignements complémentaires
Pour plus d’informations sur la façon de se connecter à PostgreSQL au moyen d’un fournisseur de données qui n’est pas répertorié ici, consultez [Chaînes de connexion PostgreSQL](https://www.connectionstrings.com/postgresql/). Ce site tiers contient également des informations complémentaires sur les fournisseurs de données et les paramètres de connexion décrits dans cette page.

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)

