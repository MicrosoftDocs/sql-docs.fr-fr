---
title: Exécution d’opérations de copie en bloc (ODBC) | Microsoft Docs
description: Découvrez comment le pilote ODBC SQL Server Native Client prend en charge les fonctions DB-Library qui effectuent SQL Server opérations de copie en bloc.
ms.custom: ''
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client ODBC driver, bulk copy
- bulk copy [ODBC]
- ODBC, bulk copy operations
- minimally logged operations [SQL Server Native Client]
- bulk copy [ODBC], about bulk copy
ms.assetid: 5c793405-487c-4f52-88b8-0091d529afb3
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 0261b223d3a7a8e775b23ab37c264efa8a1e835f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465000"
---
# <a name="performing-bulk-copy-operations-odbc"></a>Exécution d'opérations de copie en bloc (ODBC)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Par défaut, ODBC ne prend pas directement en charge les opérations de copie en bloc [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. En cas de connexion à une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] version 7.0 ou ultérieure, le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client prend en charge les fonctions de la bibliothèque de bases de données qui effectuent les opérations de copie en bloc [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cette extension spécifique au pilote fournit un chemin d'accès de mise à niveau simple pour les applications de bibliothèque de bases de données existantes qui utilisent les fonctions de copie en bloc. La copie en bloc spécialisée est prise en charge dans les fichiers suivants :  
  
-   sqlncli.h  
  
     Inclut les prototypes de fonction et les définitions de constante pour les fonctions de copie en bloc. sqlncli.h doit être inclus dans l'application ODBC exécutant les opérations de copie en bloc et se trouver dans le chemin d'accès Include de l'application lorsqu'il est compilé.  
  
-   sqlncli11.lib  
  
     Doit être dans le chemin d'accès de la bibliothèque de l'éditeur de liens et spécifié comme fichier à lier. sqlncli11.lib est distribué avec le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
-   sqlncli11.dll  
  
     Doit être présent lors de l'exécution. sqlncli11.dll est distribué avec le pilote ODBC [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client.  
  
> [!NOTE]  
>  La fonction ODBC **SQLBulkOperations** n’a aucune relation avec les [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fonctions de copie en bloc. Les applications doivent utiliser les fonctions de copie en bloc spécifiques à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour effectuer les opérations de copie en bloc.  
  
## <a name="minimally-logging-bulk-copies"></a>Enregistrement minimal des copies en bloc dans le journal  
 Avec le mode de récupération complète, toutes les opérations d'insertion de lignes effectuées par le chargement en masse sont intégralement enregistrées dans le journal des transactions. Pour les chargements de données volumineux, le journal des transactions peut se remplir rapidement. Sous certaines conditions, l'enregistrement minimal est possible. L'enregistrement minimal réduit le risque qu'une opération de chargement en masse ne remplisse l'espace du journal et se révèle plus efficace que l'enregistrement complet.  
  
 Pour plus d’informations sur l’utilisation de la journalisation minimale, consultez [Configuration requise pour la journalisation minimale dans l’importation en bloc](../../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).  
  
## <a name="remarks"></a>Remarks  
 Lors de l'utilisation de bcp.exe dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] ou version ultérieure, il se peut que vous rencontriez des erreurs là où il n'en existait pas avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]. La raison en est que dans les versions ultérieures, bcp.exe n'effectue plus la conversion implicite des types de données. Avant [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], bcp.exe convertissait les données numériques en type de données money, si la table cible avait un tel type. Toutefois, dans ce cas, bcp.exe tronquait simplement les champs supplémentaires. Dans [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)], quand les types de données ne correspondent pas entre le fichier et la table cible, bcp.exe déclenche une erreur s'il existe des données qui devraient être tronquées pour contenir dans la table cible. Pour résoudre cette erreur, corrigez les données de sorte qu'elles correspondent au type de données cible. Le cas échéant, utilisez bcp.exe à partir d'une version antérieure à [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section  
  
-   [Utilisation de fichiers de données et de format](../../relational-databases/native-client-odbc-bulk-copy-operations/using-data-files-and-format-files.md)  
  
-   [Copie en bloc à partir de variables de programme](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-from-program-variables.md)  
  
-   [Gestion des tailles de lot de copie en bloc](../../relational-databases/native-client-odbc-bulk-copy-operations/managing-bulk-copy-batch-sizes.md)  
  
-   [Copier en bloc des données texte et image](../../relational-databases/native-client-odbc-bulk-copy-operations/bulk-copying-text-and-image-data.md)  
  
-   [Conversion à partir de la bibliothèque de bases de données vers une copie en bloc ODBC](../../relational-databases/native-client-odbc-bulk-copy-operations/converting-from-db-library-to-odbc-bulk-copy.md)  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Native Client &#40;&#41;ODBC ](../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)   
 [Importation et exportation en bloc de données &#40;SQL Server&#41;](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)  
  
  
