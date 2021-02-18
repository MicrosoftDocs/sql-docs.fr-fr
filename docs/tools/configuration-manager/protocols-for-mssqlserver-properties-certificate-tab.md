---
title: Propriétés de Protocoles pour MSSQLSERVER (onglet Certificat)
description: Sélectionnez un certificat pour SQL Server ou affichez des propriétés de certificats à l’aide de l’onglet Certificat de la boîte de dialogue Protocoles pour les propriétés MSSQLSERVER.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
f1_keywords:
- sql13.swb.computermgr.cert.general.f1
helpviewer_keywords:
- MSSQLSERVER property protocols
ms.assetid: 776addd6-25f3-4875-9a71-064035787090
author: markingmyname
ms.author: maghan
monikerRange: '>=sql-server-2016'
ms.openlocfilehash: de4ad06ad4cca2dcc346b82c4e4fb6e66337106e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349703"
---
# <a name="protocols-for-mssqlserver-properties-certificate-tab"></a>Propriétés de Protocoles pour MSSQLSERVER (onglet Certificat)
[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]
  L'onglet **Certificat** de la boîte de dialogue **Propriétés de Protocoles pour MSSQLSERVER** vous permet de sélectionner un certificat pour [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou d'afficher les propriétés d'un certificat. Tous les champs sont vides tant qu'aucun certificat n'est sélectionné.  
  
 Les certificats sont stockés localement pour les utilisateurs de l'ordinateur. Pour charger un certificat en vue de son utilisation par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous devez exécuter le Gestionnaire de configuration [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous le même compte d'utilisateur que le service [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="page-header"></a>En-tête de page  
 **Afficher**  
 Fournit des détails supplémentaires sur le certificat. Cette option n'est disponible qu'une fois qu'un certificat est sélectionné dans la zone **Certificat** . Pour plus d'informations sur les détails des certificats, consultez la documentation [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows.  
  
 **Clear**  
 Supprime la sélection de la zone **Certificat** .  
  
 **Certificate**  
 Nom de certificat tel que déterminé par le fournisseur de sécurité. Sélectionnez un certificat pour en afficher les détails dans la grille des propriétés.  
  
## <a name="options"></a>Options  
 Date d'expiration  
 Date de fin de la période de validité du certificat.  
  
 Nom convivial  
 Nom convivial ou courant représentant la personne ou l'Autorité de certification destinataire du certificat.  
  
 Émis par  
 Informations relatives à l'Autorité de certification qui a émis le certificat.  
  
 Délivré à  
 Informations relatives au destinataire du certificat.  
  
  
