---
title: Matériel pour l’OLTP en mémoire SQL | Microsoft Docs
description: Découvrez les considérations matérielles relatives aux performances de l’OLTP en mémoire dans SQL Server. L’OLTP en mémoire n’utilise pas la mémoire et le disque de la même façon que les tables basées sur des disques.
ms.custom: ''
ms.date: 03/28/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: ba34a1b550d9726c3de479c56f92e40adf404214
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063705"
---
# <a name="hardware-considerations-for-in-memory-oltp-in-sql-server"></a>Considérations matérielles pour l’OLTP en mémoire dans SQL Server

L’OLTP en mémoire n’utilise pas la mémoire et le disque de la même façon que les tables basées sur des disques classiques. L’amélioration des performances que vous verrez avec l’OLTP en mémoire dépend du matériel utilisé. Dans ce billet de blog, nous abordons un certain nombre de considérations matérielles d’ordre général et fournissons des instructions génériques relatives au matériel à utiliser avec l’OLTP en mémoire.

> [!NOTE]
> Cet article était un blog publié le 1er août 2013 par l’équipe Microsoft SQL Server 2014. La page web du blog est supprimée.
>
> [OLTP en mémoire SQL Server](./in-memory-oltp-in-memory-optimization.md)

<!--
    Here was the link to the blog. This blog was captured into this new article on 2018/11/30, by GeneMi (MightyPen).
    https://cloudblogs.microsoft.com/sqlserver/2013/08/01/hardware-considerations-for-in-memory-oltp-in-sql-server-2014/
    At least one pre-existing article that contained the obsolete blog link was:
        relational-databases\in-memory-oltp\sample-database-for-in-memory-oltp.md
-->

## <a name="cpu"></a>UC

L’OLTP en mémoire ne nécessite pas un serveur haut de gamme pour prendre en charge une charge de travail OLTP à débit élevé. Nous recommandons d’utiliser un serveur de milieu de gamme avec 2 sockets CPU. En raison du débit plus élevé produit par l’OLTP en mémoire, 2 sockets sont probablement suffisants pour les besoins de votre entreprise.

Nous recommandons d’activer la technologie Hyper-Threading avec l’OLTP en mémoire. Avec certaines charges de travail OLTP, nous avons constaté des gains de performances pouvant atteindre 40 % en utilisant Hyper-Threading.

## <a name="memory"></a>Mémoire

Toutes les tables à mémoire optimisée résident entièrement en mémoire. Par conséquent, vous devez disposer de suffisamment de mémoire physique pour les tables elles-mêmes et pour supporter la charge de travail exécutée sur la base de données : la quantité de mémoire dont vous avez réellement besoin dépend de la charge de travail, mais comme point de départ vous souhaiterez probablement une quantité de mémoire disponible égale à 2 fois la taille des données. Vous avez également besoin de suffisamment de mémoire pour le pool de mémoires tampons en cas d’utilisation de la charge de travail sur des tables basées sur des disques classiques.

Pour déterminer la quantité de mémoire utilisée par une table à mémoire optimisée donnée, exécutez la requête suivante :

```sql
select object_name(object_id), * from sys.dm_db_xtp_table_memory_stats;
```

Les résultats affichent la mémoire utilisée pour les tables à mémoire optimisée et leurs index. Les données de table incluent les données utilisateur ainsi que toutes les anciennes versions de ligne qui sont toujours requises par les transactions en cours d’exécution ou qui n’ont pas encore été nettoyées par le système. La mémoire utilisée par les index de hachage est constante et ne repose pas sur le nombre de lignes dans la table.

Il est important de garder à l’esprit quand vous utilisez l’OLTP en mémoire que votre base de données entière n’a pas besoin de tenir dans la mémoire. Vous pouvez avoir une base de données de plusieurs téraoctets et bénéficier néanmoins de l’OLTP en mémoire, tant que la taille de vos données chaudes (autrement dit, les tables à mémoire optimisée) ne dépasse pas 256 Go. Le nombre maximal de fichiers de données de point de contrôle que peut gérer SQL Server pour une base de données est de 4 000, chaque fichier ayant une taille de 128 Mo. Même si nous obtenons ainsi une valeur théorique maximale de 512 Go, afin de garantir que SQL Server peut suivre le rythme de la fusion des fichiers de point de contrôle et ne pas atteindre la limite de 4 000 fichiers, la prise en charge peut s’élever à 256 Go. Notez que cette limite s’applique uniquement aux tables à mémoire optimisée ; il n’existe aucune limitation de taille sur les tables basées sur des disques classiques dans la même base de données SQL Server.

Les tables à mémoire optimisée non durables, autrement dit les tables à mémoire optimisée avec DURABILITY=SCHEMA_ONLY, ne sont pas conservées sur le disque. Même si ces tables ne sont pas limitées par le nombre de fichiers de point de contrôle, une taille de 256 Go uniquement est prise en charge. Les considérations pour les lecteurs de journaux et de données dans le reste de ce billet ne s’appliquent pas aux tables non durables, car les données existent uniquement en mémoire.

## <a name="log-drive"></a>Lecteur de journal

Les enregistrements de journal se rapportant aux tables à mémoire optimisée sont écrits dans le journal des transactions de base de données avec les autres enregistrements de journal SQL Server.

Il est toujours important de placer le fichier journal sur un lecteur avec une latence faible afin que les transactions n’attendent pas trop longtemps et pour éviter toute contention sur les E/S de journal. Votre système s’exécute aussi rapidement que votre composant le plus lent (loi d’Amdahl). Vous devez vous assurer que votre périphérique d’E/S de journal ne devient pas un goulot d’étranglement lors de l’exécution de l’OLTP en mémoire. Nous recommandons l’utilisation d’un dispositif de stockage avec une faible latence, au moins SSD.

Notez que les tables à mémoire optimisée utilisent moins de bande passante journal que les tables basées sur des disques, car elles ne journalisent pas les opérations d’index ni les enregistrements UNDO. Cela peut aider à alléger la contention sur les E/S de journal.

## <a name="data-drive"></a>Lecteur de données

La persistance des tables à mémoire optimisée à l’aide de fichiers de point de contrôle utilise les E/S de streaming. Par conséquent, ces fichiers n’ont pas besoin d’un lecteur avec une faible latence ni d’E/S aléatoires rapides. Au lieu de cela, le principal facteur pour ces lecteurs est la vitesse des E/S séquentielles et la bande passante de l’adaptateur de bus hôte (HBA). Par conséquent, vous n’avez pas besoin de SSD pour les fichiers de point de contrôle ; vous pouvez les placer sur des broches hautes performances (par exemple, SAP) tant que leur vitesse d’E/S séquentielles répond à vos besoins.

Le facteur le plus important qui intervient dans la détermination des besoins en vitesse est votre objectif de délai de récupération [RTO, Recovery Time Objective] lors du redémarrage du serveur. Lors de la récupération de la base de données, toutes les données dans les tables à mémoire optimisée doivent être lues à partir du disque, en mémoire. La récupération de la base de données se produit à la vitesse de lecture séquentielle de votre sous-système d’E/S ; le disque est le goulot d’étranglement.

Pour répondre aux exigences strictes de l’objectif RTO, nous vous recommandons de répartir les fichiers de point de contrôle sur plusieurs disques, en ajoutant plusieurs conteneurs au groupe de fichiers MEMORY_OPTIMIZED_DATA. SQL Server prend en charge la charge parallèle de fichiers de point de contrôle à partir de plusieurs lecteurs ; la récupération se produit à la vitesse globale des lecteurs.

En termes de capacité de disque, nous vous recommandons de disposer de 2 à 3 fois la taille des tables à mémoire optimisée.

## <a name="see-also"></a>Voir aussi

[Exemple de base de données pour l’OLTP en mémoire](sample-database-for-in-memory-oltp.md)