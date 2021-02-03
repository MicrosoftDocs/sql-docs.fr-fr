---
description: MSSQLSERVER_33028
title: MSSQLSERVER_33028 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33028 (Database Engine error)
ms.assetid: c5cec0e4-0bcd-4907-826f-e7d835cfcb37
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 039df95618dd8d6f5402fc6c996a2495ee9991b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190915"
---
# <a name="mssqlserver_33028"></a>MSSQLSERVER_33028
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|33028|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SEC_CRYPTOPROV_CANTOPENSESSION|  
|Texte du message|Impossible d'ouvrir la session pour %S_MSG '%.*ls'. Code d'erreur du fournisseur : %d.|  
  
## <a name="explanation"></a>Explication  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] n’a pas pu ouvrir le fournisseur de services de chiffrement répertorié dans le message d’erreur. Le fournisseur de services de chiffrement a fourni le code d'erreur répertorié. Il peut s'avérer nécessaire de contacter votre fournisseur de services de chiffrement pour plus d'informations sur l'erreur.  
  
|Code d'erreur|Description|  
|--------------|---------------|  
|0|Réussite. Aucune erreur.|  
|1|Échec. Une erreur non spécifiée ou inattendue s'est produite. Aucune information supplémentaire n'est disponible.|  
|2|Mémoire tampon insuffisante. Impossible d'allouer de l'espace au fournisseur de services de chiffrement.|  
|3|Non pris en charge. Le fournisseur de services de chiffrement n'est pas pris en charge par cette version. Sélectionnez-en un autre.|  
|4|Introuvable. Le fournisseur de services de chiffrement spécifié est absent ou vous n'avez pas l'autorisation d'accéder aux fichiers.|  
|5|Échec d'autorisation. Peut provenir de l'indication d'un mot de passe ou nom d'utilisateur incorrect lors de la création des informations d'identification. Peut être généré si les informations d'identification n'existent pas.|  
|6|Argument non valide. Un argument non valide a été transmis au fournisseur de services de chiffrement.|  
  
## <a name="user-action"></a>Action de l'utilisateur  
Corrigez l'erreur ou contactez le fournisseur de services de chiffrement pour plus d'informations.  
  
