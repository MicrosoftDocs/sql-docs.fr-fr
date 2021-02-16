---
title: Installer des mises à jour de maintenance de SQL Server | Microsoft Docs
description: Cet article fournit des informations sur l’installation des mises à jour de SQL Server au cours d’une nouvelle installation ou après l’installation de SQL Server.
ms.custom: ''
ms.date: 09/05/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: install
ms.topic: conceptual
ms.assetid: 7d6c962b-c8d0-49f7-a2ac-00ad8dca930a
author: cawrites
ms.author: chadam
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: 6c9ff0ec46dd63f50bdacf0e3feec5df1fbd90f3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348368"
---
# <a name="install-sql-server-servicing-updates"></a>Installer des mises à jour de maintenance de SQL Server

[!INCLUDE [SQL Server -Windows Only](../../includes/applies-to-version/sql-windows-only.md)]

Cet article fournit des informations sur l’installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)]. Cette section aborde les sujets suivants :
  
- Installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] pendant une nouvelle installation  
  
- Installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] après qu'il a déjà été installé.  
  
Installez les dernières mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en temps voulu pour vous assurer que les systèmes sont à jour avec les mises à jour de sécurité les plus récentes.  
  
## <a name="installing-updates-for-noversion-during-a-new-installation"></a>Installation des mises à jour de [!INCLUDE[noVersion](../../includes/ssNoVersion-md.md)] pendant une nouvelle installation  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] intègre les dernières mises à jour du produit avec l'installation principale, de sorte que le produit principal et les mises à jour applicables sont installés en même temps. La mise à jour du produit peut rechercher les mises à jour applicables à partir de :  
  
- [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update  
  
- Windows Server Update Services (WSUS)  
  
- Un dossier local  
  
- Un partage de réseau  
  
Une fois que le programme d'installation a détecté les versions les plus récentes des mises à jour applicables, il les télécharge et les intègre dans le processus d'installation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en cours. La fonctionnalité de mise à jour du produit peut inclure une mise à jour, un Service Pack, ou un Service Pack et la mise à jour cumulative.  
  
## <a name="installing-updates-for-ssnoversion-after-it-has-already-been-installed"></a>Installation des mises à jour de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)] après qu'il a déjà été installé  
Sur une instance installée de [!INCLUDE[ssNoVersion](../../includes/ssNoVersion-md.md)], nous vous recommandons d’appliquer les dernières mises à jour de sécurité et mises à jour critique comprenant les versions générales de distribution (GDRs), les Services Pack (SP) et les mises à jour cumulatives. Pour plus d’informations, consultez [l’annonce de mars 2016 concernant le modèle de maintenance incrémentiel SQL Server (ISM)](/archive/blogs/sqlreleaseservices/announcing-updates-to-the-sql-server-incremental-servicing-model-ism).

> [!NOTE]
> À partir de [!INCLUDE[ssSQL17](../../includes/sssql17-md.md)], nous adoptons un cycle de vie de maintenance standard simplifié et prévisible, marquant la fin de la disponibilité des Services Pack. Le cas échéant, seules seront mises à disposition les mises à jour cumulatives et les versions de distribution générale.
> Pour plus d’informations, consultez [l’annonce de septembre 2017 concernant le modèle de maintenance moderne pour SQL Server (MSM)](/archive/blogs/sqlreleaseservices/announcing-the-modern-servicing-model-for-sql-server).
  
Les mises à jour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont disponibles via [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update (MU), Windows Server Update Services (WSUS) et le Centre de téléchargement Microsoft. Les mises à jour de sécurité et les mises à jour critiques de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sont disponibles via [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update, et pour pouvoir consulter ces mises à jour, vous devez choisir MU par l'applet Windows Update dans le panneau de configuration.  
  
Lorsque vous recevez une mise à jour par [!INCLUDE[msCoName](../../includes/msconame-md.md)] Update, il met à jour toutes les fonctionnalités de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers la version la plus récente en mode sans assistance. Si vous avez besoin de plus de souplesse ou n’avez pas de connexion Internet ni d’accès à WSUS, vous devez vous procurer les mises à jour à partir du Centre de téléchargement [!INCLUDE[msCoName](../../includes/msconame-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
[Installer SQL Server à partir de l'Assistant Installation &#40;programme d’installation&#41;](../../database-engine/install-windows/install-sql-server-from-the-installation-wizard-setup.md)        
[Ajouter des fonctionnalités à une instance de SQL Server &#40;programme d’installation&#41;](./add-features-to-an-instance-of-sql-server-setup.md)         
[Réparer une installation défectueuse de SQL Server](../../database-engine/install-windows/repair-a-failed-sql-server-installation.md)