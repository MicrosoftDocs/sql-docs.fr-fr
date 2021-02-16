---
title: SQL Server Integration Services (SSIS) Scale Out | Microsoft Docs
description: Cet article fournit une vue d’ensemble de la fonctionnalité SQL Server Integration Services (SSIS) Scale Out, qui permet une exécution à hautes performances de packages SSIS.
ms.custom: performance
ms.date: 12/13/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: dcfbd1c5-c001-4fb7-b9ae-916e49ab6a96
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 9e75be10cff0f34d132ecfe57ddff10538091a77
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338366"
---
# <a name="integration-services-ssis-scale-out"></a>Scale-out Integration Services (SSIS)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


SSIS (SQL Server [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]) Scale Out constitue un moyen très performant d’exécuter des packages SSIS en distribuant les exécutions sur plusieurs ordinateurs. Après avoir configuré Scale-out, vous pouvez effectuer plusieurs exécutions de packages en parallèle, en mode scale-out, à partir de SSMS (SQL Server Management Studio).

## <a name="components"></a>Components
[!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale-out se compose d’un [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Master et d’un ou de plusieurs [!INCLUDE[ssIS_md](../../includes/ssis-md.md)] Scale Out Workers.

-   Le Scale Out Master est responsable de la gestion de Scale Out et reçoit les demandes d’exécution de package des utilisateurs. Pour plus d’informations, consultez [Scale Out Master](integration-services-ssis-scale-out-master.md).

-   Les Scale Out Workers extraient les tâches d’exécution du Scale Out Master et exécutent les packages. Pour plus d’informations, consultez [Scale Out Worker](integration-services-ssis-scale-out-worker.md).

## <a name="configuration-options"></a>Options de configuration
Vous pouvez définir Scale-out dans les configurations suivantes :

-   **Sur un seul ordinateur**, où un Scale Out Master et un Scale Out Worker sont exécutés côte à côte sur le même ordinateur.

-   **Sur plusieurs ordinateurs**, auquel cas chaque Scale Out Worker est sur un ordinateur différent.

## <a name="what-you-can-do"></a>Ce que vous pouvez faire
Après avoir configuré Scale-out, vous pouvez effectuer les opérations suivantes :

-   Exécuter plusieurs packages déployés sur le catalogue SSISDB en parallèle. Pour plus d’informations, consultez [Exécuter des packages dans Scale-out](run-packages-in-integration-services-ssis-scale-out.md).

-   Gérer la topologie Scale-out dans l’application Scale Out Manager. Pour plus d’informations, consultez [Integration Services Scale Out Manager](integration-services-ssis-scale-out-manager.md).

## <a name="next-steps"></a>Étapes suivantes
-   [Bien démarrer avec SSIS (SQL Server Integration Services) Scale Out sur un seul ordinateur](get-started-with-ssis-scale-out-onebox.md)

-   [Procédure pas à pas : Configurer Integration Services Scale Out](walkthrough-set-up-integration-services-scale-out.md)
