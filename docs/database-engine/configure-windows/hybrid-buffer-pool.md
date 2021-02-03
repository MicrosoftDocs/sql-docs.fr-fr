---
title: Pool de mémoires tampons hybride | Microsoft Docs
description: Découvrez comment le pool de mémoires tampons hybride rend les appareils persistants accessibles via le bus de mémoire. Activez ou désactivez cette fonctionnalité SQL Server 2019 et affichez les meilleures pratiques.
ms.custom: ''
ms.date: 10/31/2019
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
ms.assetid: ''
author: briancarrig
ms.author: brcarrig
manager: amitban
ms.openlocfilehash: ccf508d0415bc6baabc9c054a95ec26cf7989880
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99236520"
---
# <a name="hybrid-buffer-pool"></a>Pool de mémoires tampons hybride
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

Le pool de mémoires tampons hybride permet aux objets de pool de mémoires tampons de référencer des pages de données dans des fichiers de base de données résidant sur des appareils de mémoire persistante (PMEM), au lieu des copies des pages de données mises en cache dans la DRAM volatile. Cette fonctionnalité est introduite dans [!INCLUDE[sqlv15](../../includes/sssql19-md.md)].

![Pool de mémoires tampons hybride](./media/hybrid-buffer-pool.png)

Les appareils de mémoire persistante (PMEM) sont adressables par octet et si un système de fichiers prenant en charge la mémoire persistante (DAX) d’accès direct (par exemple, XFS, EXT4 ou NTFS) est utilisé, les fichiers du système de fichiers sont accessibles en utilisant les API du système de fichiers habituelles du système d’exploitation. Il peut aussi effectuer ce qui est connu sous le nom d’opérations de chargement et de stockage sur les cartes de mémoire des fichiers sur l’appareil. Cela permet aux applications prenant en charge PMEM, telles que SQL Server, d’accéder aux fichiers sur l’appareil sans traverser la pile de stockage traditionnelle.

Le pool de mémoires tampons hybride utilise cette capacité pour effectuer des opérations de chargement et de stockage sur les fichiers mappés en mémoire, afin de tirer parti de l’appareil PMEM comme cache pour le pool de mémoires tampons et comme stockage des fichiers de base de données. Cela crée une situation unique où une lecture logique et une lecture physique sont fondamentalement la même opération. Les appareils de mémoire persistante sont accessibles via le bus de mémoire tout comme la DRAM volatile normale.

Seules les pages de données propres sont mises en cache sur l’appareil pour le pool de mémoires tampons hybride. Quand une page est marquée comme « sale », elle est copiée dans le pool de mémoires tampons DRAM avant d’être réécrite sur l’appareil PMEM et marquée comme propre de nouveau. Cela se produit pendant les opérations de point de contrôle normales d’une manière similaire à celle exécutée sur un appareil de bloc standard.

La fonctionnalité de pool de mémoires tampons hybride est disponible pour Windows et Linux. L’appareil PMEM doit être formaté avec un système de fichiers qui prend en charge DAX (DirectAccess). Les systèmes de fichiers XFS, EXT4 et NTFS prennent tous en charge DAX. SQL Server détermine automatiquement si des fichiers de données résident sur un appareil PMEM correctement formaté et effectue un mappage de mémoire des fichiers de base de données au démarrage, quand une base de données est attachée, restaurée ou créée.

Pour plus d'informations, consultez les pages suivantes :

* [Comprendre et déployer la mémoire persistante (Windows)](/windows-server/storage/storage-spaces/deploy-pmem/)
* [Configurer la mémoire persistante (PMEM) pour SQL Server sur Linux](../../linux/sql-server-linux-configure-pmem.md)


## <a name="enable-hybrid-buffer-pool"></a>Activer le pool de mémoires tampons hybride

[!INCLUDE[sqlv15](../../includes/sssql19-md.md)] introduit un langage dynamique de données (DDL) permettant de contrôler le pool de mémoires tampons hybride.

L’exemple suivant active le pool de mémoires tampons hybride pour une instance de SQL Server :

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = ON;
```

Par défaut, le pool de mémoires tampons hybride est désactivé dans l’étendue de l’instance. Notez que pour que la modification du paramètre prenne effet, l’instance de SQL Server doit être redémarrée. Un redémarrage est nécessaire afin de faciliter l’allocation de pages de hachage suffisantes pour prendre en compte la capacité PMEM totale sur le serveur.

L’exemple suivant active le pool de mémoires tampons hybride pour une base de données spécifique.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = ON;
```

Par défaut, le pool de mémoires tampons hybride est activé dans l’étendue de la base de données.

## <a name="disable-hybrid-buffer-pool"></a>Désactiver le pool de mémoires tampons hybride

L’exemple suivant désactive le pool de mémoires tampons hybride au niveau de l’instance :

```sql
ALTER SERVER CONFIGURATION SET MEMORY_OPTIMIZED HYBRID_BUFFER_POOL = OFF;
```

Par défaut, le pool de mémoires tampons hybride est désactivé au niveau de l’instance. Pour que ce changement prenne effet, l’instance doit être redémarrée. Cela garantit que suffisamment de pages de hachage sont allouées pour le pool de mémoires tampons, puisque la capacité PMEM sur le serveur doit maintenant être prise en compte.

L’exemple suivant désactive le pool de mémoires tampons hybride pour une base de données spécifique.

```sql
ALTER DATABASE <databaseName> SET MEMORY_OPTIMIZED = OFF;
```

Par défaut, le pool de mémoires tampons hybride est activé dans l’étendue de la base de données.

## <a name="view-hybrid-buffer-pool-configuration"></a>Afficher la configuration du pool de mémoires tampons hybride

L’exemple suivant retourne l’état actuel de la configuration du pool de mémoires tampons hybride de l’instance.

```sql
SELECT * FROM
sys.server_memory_optimized_hybrid_buffer_pool_configuration;
```

Le deuxième exemple liste les bases de données et le paramètre de niveau de base de données pour le pool de mémoires tampons hybride (`is_memory_optimized_enabled`).

```sql
SELECT name, is_memory_optimized_enabled FROM sys.databases;
```

## <a name="best-practices-for-hybrid-buffer-pool"></a>Bonnes pratiques pour le pool de mémoires tampons hybride

 - Quand vous formatez votre appareil PMEM sur Windows, utilisez la plus grande taille d’unité d’allocation disponible pour NTFS (2 Mo dans Windows Server 2019) et vérifiez que l’appareil a été formaté pour DAX (Direct Access).

 - Activez [Verrouiller les pages en mémoire](./enable-the-lock-pages-in-memory-option-windows.md) sur Windows.

 - Les tailles de fichiers doivent être un multiple de 2 Mo (modulo 2 Mo doit être égal à zéro).

 - Si le paramètre délimité au serveur pour le pool de mémoires tampons hybride est désactivé, la fonctionnalité n’est utilisée par aucune base de données utilisateur.

 - Si le paramètre délimité au serveur pour le pool de mémoires tampons hybride est activé, vous pouvez utiliser le paramètre délimité à la base de données pour désactiver la fonctionnalité pour les bases de données utilisateur individuelles.
