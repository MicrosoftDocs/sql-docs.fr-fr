---
description: MSSQLSERVER_33129
title: MSSQLSERVER_33129 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 33129 (Database Engine error)
ms.assetid: 83b5f368-f1a1-4a40-9bb6-c77e2dec690f
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: dd8ba9d3bce316b77db7fa6ae3ec01190607b858
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99190909"
---
# <a name="mssqlserver_33129"></a>MSSQLSERVER_33129
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|33129|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|SEC_CANNOT_DISABLE_WIN_GROUP|  
|Texte du message|Impossible d'utiliser ALTER_LOGIN avec l'argument DISABLE pour refuser l'accès à un groupe Windows.|  
  
## <a name="explanation"></a>Explication  
Ce message est généré lors d'une tentative de désactivation de la connexion d'un groupe Windows.  
  
## <a name="user-action"></a>Action de l'utilisateur  
La connexion d'un groupe Windows ne peut pas être désactivée. Pour supprimer temporairement l'autorisation d'accès accordée à un groupe Windows, révoquez avec REVOKE l'autorisation CONNECT de la connexion du groupe Windows. Les utilisateurs Windows peuvent encore bénéficier d'un accès par leur connexion individuelle ou via un autre groupe Windows. L'exemple suivant révoque l'autorisation de connexion au groupe de comptabilité Accounting du domaine WESTCOAST.  
  
```Transact-SQL  
REVOKE CONNECT TO [WESTCOAST\Accounting];  
```  
  
