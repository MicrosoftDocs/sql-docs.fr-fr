---
title: Définir une taille maximale de fichier de trace
titleSuffix: SQL Server Profiler
description: Découvrez comment limiter la taille du fichier de trace dans SQL Server Profiler et comment spécifier si le fichier est substitué lorsqu’il atteint la taille maximale.
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: profiler
ms.topic: conceptual
ms.assetid: e86dc4ce-5aa3-4c0d-acb5-c9e8871ed963
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: 094df80b40338af09e7c4d9084ec3715a91a61c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345802"
---
# <a name="set-a-maximum-file-size-for-a-trace-file-sql-server-profiler"></a>Définir la taille maximale d'un fichier de trace (SQL Server Profiler)

 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Procédez comme suit pour définir la taille maximale d'un fichier de trace.  
  
### <a name="to-set-a-maximum-file-size-for-a-trace-file"></a>Pour définir une taille maximale de fichier de trace  
  
1.  Dans le menu **Fichier** , cliquez sur **Nouvelle trace**, puis connectez-vous à une instance de Microsoft [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
     La boîte de dialogue **Propriétés de la trace** apparaît.  
  
    > [!NOTE]  
    >  Si la case **Démarrer le suivi juste après avoir établi la connexion** est cochée, la boîte de dialogue **Propriétés de la trace** ne s’affiche pas et le suivi se lance. Pour désactiver ce paramètre, accédez au menu **Outils**, cliquez sur **Options** et désactivez la case à cocher **Démarrer le suivi juste après avoir établi la connexion** .  
  
2.  Dans la zone **Nom de la trace** , tapez un nom pour la trace.  
  
3.  Dans la liste **Nom du modèle**, sélectionnez un modèle de trace.  
  
4.  Sélectionnez **Enregistrer dans le fichier**, puis spécifiez un fichier dans lequel stocker les informations de trace.  
  
5.  Dans la case à cocher **Définir la taille de fichier maximale** , spécifiez la taille de fichier maximale pour la trace. Quand la taille du fichier atteint ce maximum, les événements de la trace ne sont plus enregistrés. Si vous sélectionnez **Activer la substitution de fichier** (option par défaut), voici ce qu’il se produit :  
  
     L'option de substitution de fichier entraîne la fermeture par [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] du fichier actif et la création d'un nouveau fichier lorsque la taille de fichier maximale est atteinte. Le nouveau fichier possède le même nom que le fichier d'origine, mais un entier est ajouté au nom pour indiquer son numéro séquentiel. Par exemple, si le premier fichier de trace s'appelle nom_fichier_1.trc, le deuxième se nommera nom_fichier_2.trc. Si le nom attribué au nouveau fichier de substitution est celui d'un fichier existant, ce dernier est écrasé sauf s'il est en lecture seule. L’option de substitution de fichier est activée par défaut lors de l’enregistrement de données de trace dans un fichier.  
  
     Lorsque l’option de substitution de fichier est activée, la trace se poursuit jusqu'à ce qu'elle soit arrêtée par un autre moyen. Pour arrêter la trace lorsque la taille de fichier maximale est atteinte, vous devez désactiver l’option de substitution de fichier.  
  
    > [!NOTE]  
    >  Le système de fichiers FAT32 impose une limite maximale légèrement inférieure à 4 gigaoctets (Go) à la taille de fichier. Lorsque la taille du fichier de trace atteint ce maximum, la trace échoue et le message d'erreur « Espace disque insuffisant » s'affiche. Pour que des fichiers plus volumineux puissent être créés, vous devez opter pour le système de fichiers NTFS.  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Profiler](../../tools/sql-server-profiler/sql-server-profiler.md)  
  
  
