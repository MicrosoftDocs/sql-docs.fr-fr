---
title: Supprimer une instance de cluster de basculement
description: Suivez cette procédure pour désinstaller une instance de cluster de basculement SQL Server. Cet article contient des points importants que vous devez connaître avant de poursuivre.
ms.custom: seo-lt-2019
ms.date: 12/13/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: failover-cluster-instance
ms.topic: how-to
helpviewer_keywords:
- clusters [SQL Server], removing failover cluster instance
- failover clustering [SQL Server], removing failover cluster instance
- uninstalling failover cluster instances
- removing failover cluster instances
ms.assetid: bf63353b-69cf-4c5c-98ea-7b151e36537f
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a095fdf70e8190073079944fbbcfa57e8e0ecac
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354069"
---
# <a name="remove-a-failover-cluster-instance-setup"></a>Supprimer une instance de cluster de basculement (Configuration)

[!INCLUDE [SQL Server](../../../includes/applies-to-version/sqlserver.md)]

Suivez cette procédure pour désinstaller une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Always On.  
  
> [!IMPORTANT]  
>  Pour mettre à jour ou supprimer un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , vous devez être un administrateur local autorisé à se connecter en tant que service sur tous les nœuds du cluster de basculement Windows Server.  
  
 **Avant de commencer**  
  
 Prenez en compte les points importants suivants avant de désinstaller une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :  
  
-   Si [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client est désinstallé par accident, les ressources [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne pourront pas démarrer. Pour réinstaller [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client, exécutez le programme d'installation [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour installer les éléments requis de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] .  
  
-   Si vous désinstallez un cluster de basculement comportant plusieurs ressources de cluster IP SQL, vous devez supprimer les ressources IP SQL supplémentaires à l'aide du Gestionnaire du cluster de basculement ou PowerShell.  
  
 Pour plus d’informations sur la syntaxe de l’invite de commandes, consultez [Installer SQL Server 2016 à partir de l’invite de commandes](../../../database-engine/install-windows/install-sql-server-from-the-command-prompt.md).  
  
### <a name="to-uninstall-a-ssnoversion-failover-cluster-instance"></a>Pour désinstaller une instance de cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]
  
1.  Pour désinstaller un cluster de basculement [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] , utilisez la fonctionnalité Supprimer un nœud fournie par le programme d'installation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] pour supprimer chaque nœud individuellement. Pour plus d’informations, consultez [Ajouter ou supprimer des nœuds dans un cluster de basculement ](../../../sql-server/failover-clusters/install/add-or-remove-nodes-in-a-sql-server-failover-cluster-setup.md) &#40;Configuration&#41; Always On.  
  
## <a name="see-also"></a>Voir aussi  
 [Afficher et lire les fichiers journaux d'installation de SQL Server](../../../database-engine/install-windows/view-and-read-sql-server-setup-log-files.md)  
  
