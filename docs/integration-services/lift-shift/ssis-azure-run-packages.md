---
title: Exécuter des packages SSIS dans Azure | Microsoft Docs
description: Fournit une vue d’ensemble des méthodes disponibles pour l’exécution de packages SSIS déployés sur Azure SQL Database.
ms.date: 05/29/2018
ms.topic: conceptual
ms.prod: sql
ms.prod_service: integration-services
ms.custom: ''
ms.technology: integration-services
author: swinarko
ms.author: sawinark
ms.reviewer: maghan
ms.openlocfilehash: 2c7209d43628296169e973012ea459296acc3549
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351976"
---
# <a name="run-sql-server-integration-services-ssis-packages-deployed-in-azure"></a>Exécuter des packages SSIS (SQL Server Integration Services) déployés sur Azure

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]



Choisissez une des méthodes décrites dans cet article pour exécuter des packages SSIS déployés sur le catalogue SSISDB sur un serveur Azure SQL Database. Vous pouvez exécuter un package directement ou dans le cadre d’un pipeline Azure Data Factory. Pour une vue d’ensemble de SSIS sur Azure, consultez [Déployer et exécuter des packages SSIS dans Azure](ssis-azure-lift-shift-ssis-packages-overview.md).

- Exécuter un package directement

  - [Exécuter avec SSMS](#ssms)

  - [Exécuter avec des procédures stockées](#sproc)

  - [Exécuter avec un script ou du code](#script)

- Exécuter un package dans le cadre d’un pipeline Azure Data Factory

  - [Exécuter avec l’activité Exécuter le package SSIS](#exec_activity)

  - [Exécuter avec l’activité Procédure stockée](#sproc_activity)

> [!NOTE]
> L’exécution d’un package avec `dtexec.exe` n’a pas été testée avec les packages déployés sur Azure.

## <a name="run-a-package-with-ssms"></a><a name="ssms"></a> Exécuter un package avec SSMS

Dans SSMS (SQL Server Management Studio), vous pouvez cliquer avec le bouton droit sur un package déployé sur la base de données du catalogue SSIS, SSISDB, puis sélectionner **Exécuter** pour ouvrir la boîte de dialogue **Exécuter le package**. Pour plus d’informations, consultez [Exécuter un package SSIS avec SQL Server Management Studio (SSMS)](../ssis-quickstart-run-ssms.md).

## <a name="run-a-package-with-stored-procedures"></a><a name="sproc"></a> Exécuter des packages avec des procédures stockées

Dans tout environnement à partir duquel vous pouvez vous connecter à Azure SQL Database et exécuter du code Transact-SQL, vous pouvez exécuter un package en appelant les procédures stockées suivantes :

1. **[catalog].[create_execution]** . Pour plus d’informations, consultez [catalog.create_execution](../system-stored-procedures/catalog-create-execution-ssisdb-database.md).

2. **[catalog].[set_execution_parameter_value]** . Pour plus d’informations, consultez [catalog.set_execution_parameter_value](../system-stored-procedures/catalog-set-execution-parameter-value-ssisdb-database.md).

3. **[catalog].[start_execution]** . Pour plus d’informations, consultez [catalog.start_execution](../system-stored-procedures/catalog-start-execution-ssisdb-database.md).

Pour plus d’informations, consultez les exemples suivants :

- [Exécuter un package SSIS à partir de SSMS avec Transact-SQL](../ssis-quickstart-run-tsql-ssms.md)

- [Exécuter un package SSIS à partir de Visual Studio Code avec Transact-SQL](../ssis-quickstart-run-tsql-vscode.md)

## <a name="run-a-package-with-script-or-code"></a><a name="script"></a> Exécuter un package avec un script ou du code

Dans tout environnement de développement à partir duquel vous pouvez appeler une API managée, vous pouvez exécuter un package en appelant la méthode `Execute` de l’objet `Package` dans l’espace de noms `Microsoft.SQLServer.Management.IntegrationServices`.

Pour plus d’informations, consultez les exemples suivants :

- [Exécuter un package SSIS avec PowerShell](../ssis-quickstart-run-powershell.md)

- [Exécuter un package SSIS avec du code C# dans une application .NET](../ssis-quickstart-run-dotnet.md)

## <a name="run-a-package-with-the-execute-ssis-package-activity"></a><a name="exec_activity"></a> Exécuter un package avec l’activité Exécuter le package SSIS

Pour plus d’informations, consultez [Exécuter un package SSIS à l’aide de l’activité Exécuter le package SSIS dans Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-ssis-activity).

## <a name="run-a-package-with-the-stored-procedure-activity"></a><a name="sproc_activity"></a> Exécuter un package avec l’activité Procédure stockée

Pour plus d’informations, consultez [Exécuter un package SSIS à l’aide de l’activité Procédure stockée dans Azure Data Factory](/azure/data-factory/how-to-invoke-ssis-package-stored-procedure-activity).

## <a name="next-steps"></a>Étapes suivantes

Découvrez les options disponibles pour planifier des packages SSIS déployés sur Azure. Pour plus d’informations, consultez [Planifier des packages SSIS dans Azure](ssis-azure-schedule-packages.md).