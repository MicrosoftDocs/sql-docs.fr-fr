---
description: Configuration d’un espace TempDB suffisant
title: Garantir un espace TempDB suffisant | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- TempDB space in RDS [ADO]
ms.assetid: 09130db1-6248-4234-a1e5-a9c8e1622c06
author: rothja
ms.author: jroth
ms.openlocfilehash: 3f2ea2da9100b117a1526d7a36c6d1cc51a7a636
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036489"
---
# <a name="ensuring-sufficient-tempdb-space"></a>Configuration d’un espace TempDB suffisant
Si des erreurs se produisent lors de la gestion des objets [Recordset](../../reference/ado-api/recordset-object-ado.md) qui nécessitent un espace de traitement sur Microsoft SQL Server 6,5, vous devrez peut-être augmenter la taille de tempdb. (Certaines requêtes nécessitent un espace de traitement temporaire ; par exemple, une requête avec une clause ORDER BY requiert un tri de l’ensemble d' **enregistrements**, ce qui nécessite un espace temporaire.)  
  
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
> [!IMPORTANT]
>  Lisez cette procédure avant d’effectuer les actions, car il n’est pas aussi facile de réduire un appareil une fois qu’il est développé.  
  
> [!NOTE]
>  Par défaut, Microsoft SQL Server 7,0 et versions ultérieures, TempDB est configurée pour croître automatiquement en fonction des besoins. Par conséquent, cette procédure ne peut être nécessaire que sur les serveurs exécutant des versions antérieures à 7,0.  
  
### <a name="to-increase-the-tempdb-space-on-sql-server-65"></a>Pour augmenter l’espace TempDB sur SQL Server 6,5  
  
1.  Démarrez Microsoft SQL Server Entreprise Manager, ouvrez l’arborescence du serveur, puis ouvrez l’arborescence unités de base de données.  
  
2.  Sélectionnez un appareil (physique) à développer, tel que maître, puis double-cliquez sur l’appareil pour ouvrir la boîte de dialogue **modifier l’unité de base de données** .  
  
     Cette boîte de dialogue affiche l’espace utilisé par les bases de données actuelles.  
  
3.  Dans la zone **taille** , augmentez le volume de l’appareil à la quantité souhaitée (par exemple, 50 mégaoctets (Mo) d’espace disque).  
  
4.  Cliquez sur **modifier maintenant** pour augmenter la quantité d’espace que la base de données tempdb (logique) peut développer.  
  
5.  Ouvrez l’arborescence bases de données sur le serveur, puis double-cliquez sur **tempdb** pour ouvrir la boîte de dialogue **modifier la base de données** . L’onglet **base de données** répertorie la quantité d’espace actuellement allouée à tempdb (**taille des données**). Par défaut, il s’agit de 2 Mo.  
  
6.  Sous le groupe **taille** , cliquez sur **développer**. Les graphiques indiquent l’espace disponible et alloué sur chacun des périphériques physiques. Les barres de couleur marron représentent l’espace disponible.  
  
7.  Sélectionnez un **périphérique de journalisation**, tel que maître, pour afficher la taille disponible dans la zone **taille (Mo)** .  
  
8.  Cliquez sur **développer maintenant** pour allouer cet espace à la base de données tempdb.  
  
     La boîte de dialogue **modifier la base de données** affiche la nouvelle taille allouée pour tempdb.  
  
 Pour plus d’informations sur cette rubrique, recherchez dans le fichier d’aide de Microsoft SQL Server Entreprise Manager la boîte de dialogue développer la base de données.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)