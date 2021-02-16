---
title: Configuration du stockage des tables optimisées en mémoire | Microsoft Docs
description: Découvrez comment configurer la capacité de stockage et les opérations d’E/S par seconde pour les tables à mémoire optimisée dans SQL Server.
ms.custom: ''
ms.date: 1/15/2020
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 6e005de0-3a77-4b91-b497-14cc0f9f6605
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 892ce217acbc07bdd98ca7166aeb0a9211b2adea
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350673"
---
# <a name="configuring-storage-for-memory-optimized-tables"></a>Configuration du stockage des tables optimisées en mémoire
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Vous devez configurer des opérations d'entrée/sortie par seconde (IOPS) et la capacité de stockage.  
  
## <a name="storage-capacity"></a>Capacité de stockage  

Utilisez les informations contenues dans [Estimer les besoins en mémoire des tables optimisées en mémoire](../../relational-databases/in-memory-oltp/estimate-memory-requirements-for-memory-optimized-tables.md) pour estimer la taille en mémoire des tables durables optimisées en mémoire de la base de données. Les index n’étant pas conservés pour les tables à mémoire optimisée, n’incluez pas la taille des index. 
 
Une fois que vous avez déterminé la taille, vous devez fournir un espace disque suffisant pour contenir les fichiers de point de contrôle, qui sont utilisés pour stocker les données récemment modifiées. Les données stockées comprennent non seulement le contenu des nouvelles lignes qui sont ajoutées aux tables en mémoire, mais également les nouvelles versions des lignes existantes. Ce stockage augmente lorsque les lignes sont insérées ou mises à jour. Les versions de ligne sont fusionnées et le stockage est récupéré lorsque la troncation du journal se produit. Si la troncation du journal est retardée pour une raison quelconque, le magasin de l’OLTP en mémoire augmente.

Un bon point de départ pour le dimensionnement du stockage pour cette zone consiste à réserver quatre fois la taille des tables en mémoire durables. Surveillez l’utilisation de l’espace et préparez-vous à développer le stockage qui y est disponible si nécessaire.
  
## <a name="storage-iops"></a>E/S par seconde dédiées au stockage  
 [!INCLUDE[hek_2](../../includes/hek-2-md.md)] peut augmenter considérablement le débit de la charge de travail. Par conséquent, il est important de s'assurer que les E/S ne forment pas un goulet d'étranglement.  
  
-   Lorsque vous migrez des tables sur disque vers des tables mémoire optimisées, vérifiez que le journal des transactions se trouve sur un support de stockage capable de prendre en charge le surcroît d'activité du journal des transactions. Par exemple, si votre support de stockage prend en charge des opérations du journal des transactions à 100 Mo/s et que les tables mémoire optimisées offrent des performances cinq fois supérieures, le support de stockage du journal des transactions doit être en mesure de prendre en charge une amélioration des performances cinq fois supérieure, pour empêcher l'activité du journal des transactions de devenir un goulot d'étranglement des performances.  
  
-   Les tables à mémoire optimisée sont conservées dans des fichiers de point de contrôle, qui sont distribués sur un ou plusieurs conteneurs. Chaque conteneur doit généralement être mappé à son propre dispositif de stockage et est utilisé pour augmenter la capacité de stockage et améliorer les performances d’E/S par seconde (IOPS). Vous devez vous assurer que les IOPS séquentielles du support de stockage peuvent prendre en charge jusqu’à 3 fois le débit soutenu du journal des transactions. Les opérations d’écriture dans les fichiers de point de contrôle représentent 256 Ko pour les fichiers de données et 4 Ko pour les fichiers delta.
  
     - Par exemple, si les tables à mémoire optimisée génèrent 500 Mo/s d’activité soutenue dans le journal des transactions, le stockage pour ces tables doit prendre en charge 1,5 Go/s d’IOPS. La nécessité de prendre en charge le triple du débit soutenu du journal des transactions s’explique par le fait que les paires de fichiers de données et delta sont d’abord écrites avec les données initiales, puis doivent être lues/réécrites dans le cadre d’une opération de fusion.  
  
- Le temps de récupération des tables optimisées en mémoire est un autre facteur entrant en compte dans l’estimation des IOPS pour le stockage. Les données des tables durables doivent être lues en mémoire avant qu'une base de données puisse être mise à la disposition des applications. Généralement, le chargement de données dans les tables mémoire optimisées peut être effectué à la vitesse des opérations d’E/S par seconde (IOPS). Si le stockage total des tables durables, mémoire optimisées, s'élève à 60 Go et que vous souhaitez pouvoir charger ces données en une minute, les IOPS du stockage doivent être définies à 1 Go/sec.  
  
-   Les fichiers de point de contrôle sont généralement distribués uniformément entre tous les conteneurs, si l’espace disponible le permet. Avec SQL Server 2014, vous devez provisionner un nombre impair de conteneurs pour obtenir une distribution uniforme ; à partir de la version 2016, les nombres de conteneurs impairs comme pairs assurent une distribution uniforme.
  
## <a name="encryption"></a>Chiffrement  
 Dans [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] et versions ultérieures, le stockage des tables à mémoire optimisée est chiffré au repos dans le cadre de l’activation de Transparent Data Encryption (TDE) sur la base de données. Pour plus d’informations, consultez [Transparent Data Encryption](../../relational-databases/security/encryption/transparent-data-encryption.md). Dans [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)], les fichiers de point de contrôle ne sont pas chiffrés, même si TDE est activé sur la base de données.

 Les données contenues dans les tables à mémoire optimisée [non durables](../../relational-databases/in-memory-oltp/defining-durability-for-memory-optimized-objects.md) (SCHEMA_ONLY) ne sont pas écrites sur disque à tout moment. Par conséquent, TDE ne s’applique pas à ces tables.
  
## <a name="see-also"></a>Voir aussi  
 [Création et gestion du stockage des objets à mémoire optimisée](../../relational-databases/in-memory-oltp/creating-and-managing-storage-for-memory-optimized-objects.md)  
  
  
