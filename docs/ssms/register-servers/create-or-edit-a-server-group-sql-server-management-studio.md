---
description: Créer ou modifier un groupe de serveurs (SQL Server Management Studio)
title: Créer ou modifier un groupe de serveurs
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.swb.editgroup.f1
- sql13.swb.newgroup.f1
helpviewer_keywords:
- Registered Servers [SQL Server], server groups
- server groups [SQL Server]
- groups [SQL Server], server
ms.assetid: d4a942bd-2dd1-42db-ad0e-e9a9ae5b856d
author: markingmyname
ms.author: maghan
ms.reviewer: mikeray
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.openlocfilehash: 81212d0e55c322467abcb47ca96770bc1ae0f5ce
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350526"
---
# <a name="create-or-edit-a-server-group-sql-server-management-studio"></a>Créer ou modifier un groupe de serveurs (SQL Server Management Studio)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Cette rubrique explique comment organiser les serveurs dans des serveurs inscrits dans [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] en créant des groupes de serveurs et en plaçant les serveurs dans les groupes. Vous pouvez créer des groupes de serveurs dans Serveurs inscrits à tout moment ou lors de l'inscription des serveurs.  

## <a name="SSMSProcedure"></a>

### <a name="to-create-a-server-group-in-registered-servers"></a>Pour créer un groupe de serveurs dans Serveurs inscrits  

1. Dans Serveurs inscrits, cliquez sur le type de serveur dans la barre d'outils Serveurs inscrits. Si Serveurs inscrits n'est pas visible, cliquez sur **Serveurs inscrits** dans le menu **Affichage** .  

2. Cliquez avec le bouton droit sur un serveur ou un groupe de serveurs, pointez sur **Nouveau**, puis cliquez sur **Groupe de serveurs**.  

3. Dans la boîte de dialogue **Nouveau groupe de serveurs** , dans la zone de liste **Nom du groupe** , tapez un nom unique pour le groupe de serveurs. Le nom du groupe de serveurs doit être unique dans l'emplacement actuel de l'arborescence Serveurs inscrits.

4. Dans la zone de liste **Description du groupe** , vous pouvez taper un nom convivial qui décrit le groupe de serveurs, par exemple « Serveurs de finance pour l'Amérique latine ».  

5. Dans la zone **Sélectionnez un emplacement pour le nouveau groupe de serveurs** , cliquez sur un emplacement approprié, puis sur **Enregistrer**.  

   > [!NOTE]
   >  Vous pouvez également créer un nouveau groupe de serveurs pendant l'inscription d'un serveur, en cliquant sur **Nouveau groupe**, puis en renseignant la boîte de dialogue **Nouveau groupe** .  

## <a name="see-also"></a>Voir aussi

[Inscrire des serveurs](./register-servers.md)