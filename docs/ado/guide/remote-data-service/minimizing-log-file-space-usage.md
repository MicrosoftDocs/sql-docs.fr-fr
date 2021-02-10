---
description: Minimisation de l’espace utilisé par un fichier journal
title: Minimisation de l’utilisation de l’espace du fichier journal | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- log file space in RDS [ADO]
ms.assetid: 669662a0-e20f-483e-ab28-53f66c524c98
author: rothja
ms.author: jroth
ms.openlocfilehash: c0be8adfc9fdcf85df12768571567ce832c2f69a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036469"
---
# <a name="minimizing-log-file-space-usage"></a>Minimisation de l’espace utilisé par un fichier journal
Un fichier journal peut être rempli rapidement (ce qui entraîne l’arrêt du serveur) s’il existe un volume important d’activités sur une base de données SQL Server. Vous pouvez définir le fichier journal à **tronquer au point de contrôle** afin d’étendre considérablement la durée de vie du fichier journal pour une base de données.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-65"></a>Pour activer TRUNCATE au point de contrôle dans Microsoft SQL Server 6,5  
  
1.  Démarrez Microsoft SQL Server Entreprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence unités de base de données.  
  
2.  Double-cliquez sur le nom de la base de données sur laquelle cette fonctionnalité sera activée.  
  
3.  Dans l’onglet **base de données** , sélectionnez **tronquer**.  
  
4.  Dans l’onglet **options** , sélectionnez **tronquer le journal au point de contrôle**, puis cliquez sur **OK**.  
  
### <a name="to-enable-truncate-on-checkpoint-in-microsoft-sql-server-70"></a>Pour activer TRUNCATE au point de contrôle dans Microsoft SQL Server 7,0  
  
1.  Démarrez Microsoft SQL Server Entreprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence bases de données.  
  
2.  Cliquez avec le bouton droit sur le nom de la base de données sur laquelle cette fonctionnalité sera activée, puis choisissez **Propriétés**.  
  
3.  Dans l’onglet **options** , sélectionnez **tronquer le journal au point de contrôle**, puis cliquez sur **OK**.  
  
 Pour plus d’informations sur la fonctionnalité **tronquer au point de contrôle** , consultez la documentation Microsoft SQL Server.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)