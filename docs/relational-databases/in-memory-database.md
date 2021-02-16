---
description: Technologies des systèmes de base de données en mémoire
title: Fonctionnalités et technologies des systèmes de base de données en mémoire
ms.date: 10/30/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- in-memory systems
- in-memory technologies
- in-memory features
- database, in-memory database
- system, in-memory system
- features, in-memory features
- in-memory
ms.assetid: 11f8017e-5bc3-4bab-8060-c16282cfbac1
author: briancarrig
ms.author: brcarrig
ms.openlocfilehash: 96f175f4eb6f913c7870f573e65201e1124dffad
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351285"
---
# <a name="in-memory-database-systems-and-technologies"></a>Technologies des systèmes de base de données en mémoire

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Cette page constitue une référence pour les fonctionnalités et les technologies en mémoire disponibles dans SQL Server. Le concept de système de base de données en mémoire fait référence à un système qui a été conçu pour tirer parti des capacités de mémoire plus importantes qu’offrent les systèmes de base de données modernes. Une base de données en mémoire peut être de nature relationnelle ou non relationnelle.

Il est souvent supposé que les performances d’un système de base de données en mémoire sont principalement dues au fait qu’il est plus rapide d’accéder aux données résidant en mémoire qu’aux données qui se trouvent sur des sous-systèmes de disque, même les plus rapides (selon plusieurs ordres de grandeur). Toutefois, de nombreuses charges de travail SQL Server peuvent faire rentrer l’intégralité de leur ensemble de travail dans la mémoire disponible. De nombreux systèmes de base de données en mémoire peuvent conserver les données sur le disque et ne pas toujours être en mesure de faire rentrer l’ensemble des données dans la mémoire disponible.

Pour les charges de travail de bases de données relationnelles, c’est un cache volatile rapide qui est principalement utilisé sur les appareils beaucoup plus lents mais durables. Cela nécessite des approches particulières quant à la gestion des charges de travail. Des taux de transfert de mémoire plus rapides, une plus grande capacité ou même une mémoire persistante peuvent faciliter le développement de nouvelles fonctionnalités et technologies et favoriser l’apparition de nouvelles approches concernant la gestion des charges de travail de base de données relationnelle.

## <a name="hybrid-buffer-pool"></a>Pool de tampons hybride

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

Le [pool de mémoires tampons hybride](../database-engine/configure-windows/hybrid-buffer-pool.md) étend le pool de mémoires tampons des fichiers de base de données résidant sur des supports de stockage à mémoire persistante adressables par octets, aussi bien pour les plateformes Windows que pour les plateformes Linux avec [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

## <a name="memory-optimized-tempdb-metadata"></a>Métadonnée `tempdb` à mémoire optimisée

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

[!INCLUDE[sql-server-2019](../includes/sssql19-md.md)] introduit une nouvelle fonctionnalité, les [métadonnées tempdb à mémoire optimisée](./databases/tempdb-database.md#memory-optimized-tempdb-metadata), qui supprime efficacement certains goulots de contention et déverrouille un nouveau niveau de scalabilité pour les charges de travail de base de données tempdb lourdes.

## <a name="in-memory-oltp"></a>OLTP en mémoire

[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]

L’[OLTP en mémoire](./in-memory-oltp/in-memory-oltp-in-memory-optimization.md) est une technologie de base de données disponible dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et [!INCLUDE[ssSDS](../includes/sssds-md.md)] qui permet d’optimiser les performances du traitement transactionnel, de l’ingestion et du chargement des données, et des scénarios de données temporaires.

## <a name="configuring-persistent-memory-support-for-linux"></a>Configuration de la prise en charge de la mémoire persistante pour Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

[!INCLUDE[sqlv15](../includes/sssql19-md.md)] explique comment configurer la mémoire persistante (PMEM) à l’aide de la [mémoire persistante](../linux/sql-server-linux-configure-pmem.md) de l’utilitaire `ndctl`.

## <a name="persisted-log-buffer"></a>Mémoire tampon du journal persistant

Le Service Pack 1 de [!INCLUDE[ssSQL16](../includes/sssql16-md.md)] apporte une optimisation des performances pour les charges de travail gourmandes en écriture qui étaient liées par les attentes WRITELOG. La mémoire persistante est utilisée pour stocker la mémoire tampon du journal. Cette mémoire tampon, qui est de petite taille (20 Mo par base de données utilisateur), doit être vidée sur le disque afin que les transactions écrites dans le journal des transactions soient renforcées. Pour les charges de travail OLTP gourmandes en écriture, ce mécanisme de vidage peut devenir un goulot d’étranglement. Lorsque la mémoire tampon du journal se trouve dans la mémoire persistante, le nombre d’opérations nécessaires pour renforcer le journal est réduit, ce qui améliore les temps de transaction globaux et augmente les performances des charges de travail. Ce processus s’est d’abord appelé [mise en cache de la fin du journal]( https://blogs.msdn.microsoft.com/bobsql/2016/11/08/how-it-works-it-just-runs-faster-non-volatile-memory-sql-server-tail-of-log-caching-on-nvdimm/). Toutefois, une contradiction était perçue entre les [sauvegardes de fin de journal](./backup-restore/tail-log-backups-sql-server.md) et la compréhension traditionnelle selon laquelle la fin du journal correspond à la partie du journal des transactions qui est renforcée, mais pas encore sauvegardée. Étant donné que le nom officiel de cette fonctionnalité est Mémoire tampon du journal persistant, c’est lui que nous avons utilisé dans cet article.

Consultez [Ajouter une mémoire tampon de journal persistant à une base de données](./databases/add-persisted-log-buffer.md).
