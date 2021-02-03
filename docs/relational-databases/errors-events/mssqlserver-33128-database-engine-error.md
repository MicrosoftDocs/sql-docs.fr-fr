---
description: MSSQLSERVER_33128
title: MSSQLSERVER_33128 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33128 (Database Engine error)
ms.assetid: 12c1096f-d120-439b-85f3-f794859503c9
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: a1cb053c0922fde5ed87c6a666c55c950457e358
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208401"
---
# <a name="mssqlserver_33128"></a>MSSQLSERVER_33128
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|33128|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SEC_DEPRECATED_ALGO|  
|Texte du message|Échec du chiffrement. La clé utilise l'algorithme déconseillé « %.*ls » qui n'est plus pris en charge.|  
  
## <a name="explanation"></a>Explication  
Ce message s'affiche pour faire référence à l'algorithme de chiffrement RC4 (ou RC4_128). Les algorithmes RC4 et RC4_128 sont des algorithmes de chiffrement faible et sont déconseillés. Utilisez à la place un algorithme plus fort, tel qu'un des algorithmes AES.  
  
Si le niveau de compatibilité de la base de données est 90 ou 100, l'opération réussit, l'événement de dépréciation est déclenché et le message s'affiche uniquement dans la mémoire tampon en anneau.  
  
Si le niveau de compatibilité de la base de données est 110 ou supérieur, les opérations de déchiffrement réussissent, l'événement de dépréciation est déclenché et le message s'affiche uniquement dans la mémoire tampon en anneau. Les opérations de chiffrement échouent, l'événement de dépréciation est déclenché, et le message est affiché pour l'utilisateur, et le message apparaît dans la mémoire tampon en anneau.  
  
> [!NOTE]  
> La mémoire tampon en anneau est un composant interne qui n'est pas entièrement documenté et qui n'est pas conçu pour être utilisé par les clients. Les messages de la mémoire tampon en anneau sont utiles lorsque vous contactez le support technique [!INCLUDE[msCoName](../../includes/msconame-md.md)]. Pour afficher la mémoire tampon en anneau, interrogez la vue de gestion dynamique sys.dm_os_ring_buffers.  
  
|State|Description|  
|---------|---------------|  
|1|Une clé RC4 est utilisée dans la fonction intégrée encryptbykey(). La fonction intégrée retourne NULL. Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
|2|Une clé RC4 est utilisée par la fonction intégrée decryptbykey(). Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
|3|Une clé RC4 déconseillée est chiffrée par une clé symétrique. Message visible pour les utilisateurs et dans la mémoire tampon en anneau. Impossible de modifier les clés symétriques RC4 déconseillées avec un niveau de compatibilité de 110. Essayez d'utiliser des clés non-RC4 pour les opérations de chiffrement. Si nécessaire, définissez le niveau de compatibilité descendante à 90 ou 100.|  
|4|Une clé non-RC4 est chiffrée par une clé symétrique RC4 déconseillée. Message visible pour les utilisateurs et dans la mémoire tampon en anneau. Modifiez l'application de façon à utiliser des clés symétriques non-RC4 ou définissez le niveau de compatibilité descendante à 90 ou 100.|  
|5|Une clé RC4 déconseillée est déchiffrée par une clé symétrique. Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
|6|Une clé non-RC4 est déchiffrée par une clé symétrique RC4. Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
|7|Une clé RC4 symétrique est chiffrée par le certificat. Message visible pour les utilisateurs et dans la mémoire tampon en anneau.|  
|8|Une clé RC4 symétrique est déchiffrée par le certificat. Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
|9|Une clé RC4 symétrique est chiffrée par une clé EKM.|  
|10|Une clé RC4 symétrique est déchiffrée par une clé EKM. Ce message apparaît uniquement dans la mémoire tampon en anneau.|  
  
## <a name="user-action"></a>Action de l'utilisateur  
Utilisez à la place un algorithme plus fort, tel qu'un des algorithmes AES. (Recommandé)  
  
Utilisez ALTER DATABASE SET COMPATIBILITY_LEVEL pour définir le niveau de compatibilité de la base de données à 100. (Non recommandé.)  
  
