---
description: MSSQLSERVER_17883
title: MSSQLSERVER_17883 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17883 (Database Engine error)
ms.assetid: adaf1c04-e397-4a69-90b8-9353a37277ea
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: e6b7e4e7321277c47ccab2adf6e4431fea71e58c
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88456355"
---
# <a name="mssqlserver_17883"></a>MSSQLSERVER_17883
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|17883|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SRV_SCHEDULER_NONYIELDING|  
|Texte du message|Le processus %ld:%ld:%ld (0x%lx) travail 0x%p semble être improductif dans le Planificateur %ld. Heure de création du thread : %I64d. Utilisation approximative de l'UC pour ce thread : noyau %I64d ms, utilisateur %I64d ms. Utilisation du processus %d%%. Système inactif %d%%. Intervalle : %I64d ms.|  
  
## <a name="explanation"></a>Explication  
Indique un problème possible avec un thread improductif dans un Planificateur.  Il peut s'agir d'un bogue dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou si le nombre de cycles à exécuter par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] est insuffisant.  Cette erreur peut se résoudre si le thread finit par être productif.  
  
## <a name="user-action"></a>Action de l'utilisateur  
Si la charge excessive sur le système réduit la charge en général, contactez le service de support technique Microsoft.  
  
