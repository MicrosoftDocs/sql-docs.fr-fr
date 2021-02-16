---
title: Installer la recherche en texte intégral SQL Server sur Linux
description: Découvrez comment installer la recherche en texte intégral SQL Server sur Linux. La recherche en texte intégral vous permet d'exécuter des requêtes en texte intégral sur des données de caractères dans des tables SQL Server.
author: VanMSFT
ms.author: vanto
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: bb42076f-e823-4cee-9281-cd3f83ae42f5
ms.openlocfilehash: 406c1a5eb384db2d03514193e0e252ca89c94bd4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346376"
---
# <a name="install-sql-server-full-text-search-on-linux"></a>Installer la recherche en texte intégral SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Les étapes suivantes installent la [recherche en texte intégral SQL Server](../relational-databases/search/full-text-search.md) (**mssql-server-fts**) sur Linux. La recherche en texte intégral vous permet d'exécuter des requêtes en texte intégral sur des données de caractères dans des tables SQL Server. Pour les problèmes connus de cette version, consultez les [Notes de publication](sql-server-linux-release-notes.md).

> [!NOTE]
> Avant d’installer la recherche en texte intégral SQL Server, commencez par [installer SQL Server](sql-server-linux-setup.md#platforms). Cela permet de configurer les clés et les référentiels que vous utilisez pour installer le package **mssql-server-fts**.

Installer la recherche en texte intégral SQL Server pour votre plateforme :

- [Red Hat Enterprise Linux](#RHEL)
- [Ubuntu](#ubuntu)
- [SUSE Linux Enterprise Server](#SLES)

## <a name=""></a><a name="RHEL">Installer sur RHEL</a>

Utilisez les commandes suivantes pour installer **mssql-server-fts** sur Red Hat Enterprise Linux. 

```bash
sudo yum install -y mssql-server-fts
```

Si vous avez déjà installé **mssql-server-fts**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo yum check-update
sudo yum update mssql-server-fts
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de recherche en texte intégral dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

## <a name=""></a><a name="ubuntu">Installer sur Ubuntu</a>

Utilisez les commandes suivantes pour installer **mssql-server-fts** sur Ubuntu. 

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts
```

Si vous avez déjà installé **mssql-server-fts**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo apt-get update 
sudo apt-get install -y mssql-server-fts 
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de recherche en texte intégral dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

## <a name=""></a><a name="SLES">Installer sur SLES</a>

Utilisez les commandes suivantes pour installer **mssql-server-fts** sur SUSE Linux Enterprise Server. 

```bash
sudo zypper install mssql-server-fts
```

Si vous avez déjà installé **mssql-server-fts**, vous pouvez mettre à jour vers la dernière version avec les commandes suivantes :

```bash
sudo zypper refresh
sudo zypper update mssql-server-fts
```

Si vous avez besoin d’une installation hors connexion, recherchez le téléchargement du package de recherche en texte intégral dans les [Notes de publication](sql-server-linux-release-notes.md). Puis utilisez les mêmes étapes d’installation hors connexion décrites dans l’article [Installer SQL Server](sql-server-linux-setup.md#offline).

## <a name="supported-languages"></a>Langues prises en charge

La recherche en texte intégral utilise des [séparateurs de mots](../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md) qui déterminent comment identifier des mots individuels en fonction de la langue. Vous pouvez obtenir la liste des séparateurs de mots inscrits en interrogeant l'affichage du catalogue **sys.fulltext_languages**. Les séparateurs de mots pour les langues suivantes sont installés avec SQL Server :

| Language | ID de langue |
|---|---|
| Neutre | 0 |
| Arabe | 1025 |
| Bengali (India) | 1093 |
| Bokmål | 1044 |
| Portugais (Brésil) | 1046 |
| British English | 2057 |
| Bulgare | 1026 |
| Catalan | 1027 |
| Chinois (Hong Kong R.A.S., RPC) | 3076 |
| Chinese (Macao (R.A.S.)) | 5124 |
| Chinese (Singapore) | 4100 |
| Croate | 1050 |
| Tchèque | 1029 |
| Danois | 1030 |
| Néerlandais | 1043 |
| Anglais | 1033 |
| Français | 1036 |
| Allemand | 1031 |
| Grec | 1032 |
| Goudjrati | 1095 |
| Hébreu | 1037 |
| Hindi | 1081 |
| Islandais | 1039 |
| Indonésien | 1057 |
| Italien | 1040 |
| Japonais | 1041 |
| Kannada | 1099 |
| Coréen | 1042 |
| Letton | 1062 |
| Lituanien | 1063 |
| Malay - Malaysia | 1086 |
| Malayalam | 1100 |
| Marathi | 1102 |
| Polonais | 1045 |
| Portugais | 2070 |
| Pendjabi | 1094 |
| Roumain | 1048 |
| Russe | 1049 |
| Serbe (cyrillique) | 3098 |
| Serbe (latin) | 2074 |
| Chinois simplifié | 2052 |
| Slovaque | 1051 |
| Slovène | 1060 |
| Espagnol | 3082 |
| Suédois | 1053 |
| Tamoul | 1097 |
| Télougou | 1098 |
| Thaï | 1054 |
| Chinois traditionnel | 1028 |
| Turc | 1055 |
| Ukrainien | 1058 |
| Ourdou | 1056 |
| Vietnamien | 1066 |

## <a name="filters"></a><a id="filters"></a> Filtres

La recherche en texte intégral fonctionne également avec du texte stocké dans des fichiers binaires. Mais dans ce cas, un filtre installé est requis pour traiter le fichier. Pour plus d’informations sur les filtres, consultez [Configurer et gérer les filtres pour la recherche](../relational-databases/search/configure-and-manage-filters-for-search.md).

Vous pouvez consulter une liste des filtres installés en appelant **sp_help_fulltext_system_components ’Filter'**. Pour SQL Server, les filtres suivants sont installés :

| Nom du composant | ID de classe | Version |
|---|---|---|
|.a | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ans | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.asc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ascx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asm | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.asp | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.aspx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.asx | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bas | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.bat | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.bcp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.c | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cls | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cmd | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.cs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.csa | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.css | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.csv | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.cxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dbs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.def | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|. dic | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.dos | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsp | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.dsw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ext | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.faq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.fky | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.h | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hhc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hpp | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.hta | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.html | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htt | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htw | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.htx | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.hxx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.i | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.ibq | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ics | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.idl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.idq | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.ini | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.inl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.inx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.jav | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.java | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.js | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.kci | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.lgn | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.log | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.lst | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.m3u | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.mak | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.mk | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odc | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.odh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.odl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgdef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pkgundef | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.pl | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.prc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rc | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rc2 | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.reg | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rgs | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.rtf | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.rul | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.s | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.scc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.shtm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.shtml | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.snippet | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sol | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.sor | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.srf | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.stm | E0CA5340-4534-11CF-B952-00AA0051FE20 | 12.0.6828.0 |
|.tab | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tdl | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tlh | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.tli | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.trg | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.txt | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.udf | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.udt | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.url | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.usr | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vbs | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.viw | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsct | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixlangpack | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsixmanifest | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vspscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vsscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.vssscc | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wri | C1243CA0-BF96-11CD-B579-08002B30BFEB | 12.0.6828.0 |
|.wtx | C7310720-AC80-11D1-8DF3-00C04FB6EF4F | 12.0.6828.0 |
|.xml | 41B9BE05-B3AF-460C-BF0B-2CDD44A093B1 | 12.0.9735.0 |

## <a name="semantic-search"></a>Recherche sémantique
La [Recherche sémantique](../relational-databases/search/semantic-search-sql-server.md) s’appuie sur la fonctionnalité de recherche en texte intégral pour extraire et indexer des *phrases clés* statistiquement pertinentes. Cela vous permet d’interroger la signification dans des documents de votre base de données. Il permet également d’identifier les documents qui sont similaires.

Pour pouvoir utiliser la Recherche sémantique, vous devez d’abord restaurer la base de données Base de langages statistiques pour la recherche sémantique sur votre machine.

1. Utilisez un outil, tel que [sqlcmd](sql-server-linux-setup-tools.md), pour exécuter la commande Transact-SQL suivante sur votre instance Linux. Cette commande restaure la base de données de statistiques linguistiques.

   ```sql
   RESTORE DATABASE [semanticsdb] FROM
   DISK = N'/opt/mssql/misc/semanticsdb.bak' WITH FILE = 1,
   MOVE N'semanticsdb' TO N'/var/opt/mssql/data/semanticsDB.mdf',
   MOVE N'semanticsdb_log' TO N'/var/opt/mssql/data/semanticsdb_log.ldf', NOUNLOAD, STATS = 5
   GO
   ```

   > [!NOTE]
   > Si nécessaire, mettez à jour les chemins d’accès dans la commande RESTORE précédente pour les adapter à votre configuration.

1. Exécutez la commande Transact-SQL suivante pour inscrire la base de données de statistiques linguistiques sémantique.

    ```sql
    EXEC sp_fulltext_semantic_register_language_statistics_db @dbname = N'semanticsdb';  
    GO
    ```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d'informations sur la recherche en texte intégral, consultez [Recherche en texte intégral SQL Server](../relational-databases/search/full-text-search.md). 
