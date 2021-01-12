---
title: Gérer SQL Server sur Linux
description: Cet article fournit des liens vers des tâches et des outils de gestion courants pour SQL Server s’exécutant sur Linux.
author: VanMSFT
ms.author: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 6bd8eb0b-593d-467e-87ea-ab1c4dbcd1ea
moniker: '>= sql-server-linux-2017 || >= sql-server-2017 '
ms.openlocfilehash: 04ece9daf696d73b9de96f48a08e32136fe6ab66
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094629"
---
# <a name="choose-the-right-tool-to-manage-sql-server-on-linux"></a>Choisissez l’outil approprié pour gérer SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Il existe plusieurs façons de gérer SQL Server sur Linux. La section suivante fournit un rapide aperçu des différents outils et techniques de gestion avec des pointeurs vers d’autres ressources.

## <a name="mssql-conf"></a>mssql-conf 

L'outil **mssql-conf** configure SQL Server sur Linux. Pour plus d’informations, consultez [Configurer SQL Server sur Linux avec mssql-conf](sql-server-linux-configure-mssql-conf.md).

## <a name="transact-sql"></a>Transact-SQL

Presque tout ce que vous pouvez faire dans un outil client peut également être accompli à l’aide d’instructions Transact-SQL. SQL Server fournit des [Vues de gestion dynamique (DMV)](../relational-databases/system-dynamic-management-views/system-dynamic-management-views.md) qui interrogent l’état et la configuration de SQL Server. Il existe également des [commandes Transact-SQL](../t-sql/language-reference.md) pour les tâches de gestion des bases de données. Vous pouvez exécuter ces commandes dans n’importe quel outil client qui prend en charge la connexion à SQL Server et l’exécution de requêtes Transact-SQL, par exemple [sqlcmd](sql-server-linux-setup-tools.md) ou [Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md).

## <a name="azure-data-studio"></a>Azure Data Studio

Le nouvel outil Azure Data Studio est un outil multiplateforme pour la gestion de SQL Server. Pour plus d’informations, consultez [Azure Data Studio](../azure-data-studio/what-is-azure-data-studio.md).

## <a name="sql-server-management-studio-on-windows"></a>SQL Server Management Studio sur Windows

SQL Server Management Studio (SSMS) est une application Windows qui fournit une interface utilisateur graphique pour la gestion de SQL Server. Bien qu’il s’exécute actuellement uniquement sur Windows, vous pouvez l’utiliser pour vous connecter à distance à vos instances Linux. Pour plus d’informations sur l’utilisation de SSMS pour gérer SQL Server, consultez [Utiliser SSMS pour gérer SQL Server sur Linux](sql-server-linux-manage-ssms.md).

## <a name="mssql-cli-preview"></a>mssql-cli (préversion)

Microsoft a mis en production un nouvel outil de script multiplateforme pour SQL Server, [mssql-cli](https://blogs.technet.microsoft.com/dataplatforminsider/2017/12/12/try-mssql-cli-a-new-interactive-command-line-tool-for-sql-server/). Cet outil est actuellement en préversion.

## <a name="powershell"></a>PowerShell

PowerShell fournit un environnement de ligne de commande riche pour gérer les SQL Server sur Linux. Pour plus d’informations, consultez [Utiliser PowerShell pour gérer SQL Server sur Linux](sql-server-linux-manage-powershell.md).

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur SQL Server sur Linux, consultez [SQL Server sur Linux](sql-server-linux-overview.md).