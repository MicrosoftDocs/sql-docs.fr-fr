---
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 02/08/2021
ms.topic: include
author: dphansen
ms.author: davidph
ms.openlocfilehash: 5356e01f91e0df7e653c626403546e27e3697a87
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072875"
---
## <a name="prerequisites"></a>Prérequis

Avant d’installer un runtime Python personnalisé, installez ce qui suit :

+ Installez SQL Server 2019 pour Linux. Vous pouvez installer SQL Server sur Red Hat Enterprise Linux (RHEL), SUSE Linux Enterprise Server (SLES) et Ubuntu. Pour plus d’informations, voir [Conseils d’installation pour SQL Server sur Linux](../../../linux/sql-server-linux-setup.md).

+ Effectuez une mise à niveau vers la mise à jour cumulative (CU) 3 ou version ultérieure pour SQL Server 2019. Effectuez les étapes suivantes :
    1. Configurez les référentiels pour les mises à jour cumulatives. Pour plus d’informations, consultez [Configurer les référentiels pour l’installation et la mise à niveau de SQL Server sur Linux](../../../linux/sql-server-linux-change-repo.md).

    1. Mettez à jour le package **mssql-server** vers la dernière mise à jour cumulative. Pour plus d’informations, consultez [la section Mettre à jour ou mettre à niveau SQL Server dans le Guide d’installation de SQL Server sur Linux](../../../linux/sql-server-linux-setup.md#upgrade).
