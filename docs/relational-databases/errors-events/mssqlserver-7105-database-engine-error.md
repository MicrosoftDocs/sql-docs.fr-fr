---
description: MSSQLSERVER_7105
title: MSSQLSERVER_7105
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 7105 (Database Engine error)
ms.assetid: ''
author: rgward
ms.author: ramakoni
ms.openlocfilehash: 7aa3b39996f8dc282e11d92706743a7ba42998da
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099265"
---
# <a name="mssqlserver_7105"></a>MSSQLSERVER_7105
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|7105|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|TXT_PGNOTEXIST|
|Texte du message|L'ID de base de données %d, page %S_PGID, emplacement %d du nœud de type de données LOB n'existe pas. Ceci est généralement causé par des transactions pouvant lire des données non validées sur une page de données. Exécutez DBCC CHECKTABLE|
||

## <a name="explanation"></a>Explication

Une requête peut rencontrer le message 7105 quand des données LOB (Large Object) référencées par une ligne de page de base de données ne sont pas accessibles.

Étant donné que cette erreur présente le niveau de gravité 22, la connexion est arrêtée par le serveur. Ce message d’erreur est également écrit dans le fichier SQL ERRORLOG et dans le journal des événements des applications Windows avec EventID = 7105.

## <a name="possible-causes"></a>Causes possibles

Cette erreur peut se produire pour l’une des raisons suivantes :

- Un problème d’endommagement de base de données existe dans une page de base de données ou dans les structures de page LOB auxquelles la page de base de données fait référence.
- La requête en situation d’échec utilise `READ UNCOMMITTED ISOLATION LEVEL` ou l’indicateur de requête `NOLOCK`.
- Il existe un problème au niveau du moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui provoque l’échec de la requête avec cette erreur.

Consultez les sections Résolution et [Informations complémentaires](#more-information) pour identifier la cause de votre problème spécifique et la solution appropriée.

## <a name="user-action"></a>Action requise

1. Comme indiqué dans le message, la première étape consiste à exécuter `DBCC CHECKDB` sur la base de données ou `DBCC CHECKTABLE` sur la table où le problème s’est posé.

    - L’ID de base de données est fourni dans le message.
    - Pour connaître la table exacte affectée sans exécuter `DBCC CHECKDB`, vous devez déterminer quelles tables ont été consultées par la requête qui a rencontré l’erreur. Une méthode consiste à utiliser le Générateur de profils SQL pour suivre la requête. Toutefois, dans [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] et [!INCLUDE[sskatmai](../../includes/sskatmai-md.md)] R2, vous pourrez peut-être trouver la requête à l’aide de la session des événements étendus system_health. Pour plus d’informations sur l’utilisation de la session system_health, consultez le lien suivant : [Utiliser la session system_health](../extended-events/use-the-system-health-session.md).

    - Comme avec tout problème de cohérence de base de données, vous pouvez résoudre ces erreurs en procédant à une restauration à partir d’une sauvegarde reconnue fiable qui ne contient pas ce problème.

    - Toutefois, si vous ne pouvez pas effectuer une restauration à partir d’une sauvegarde, suivez les recommandations relatives à `DBCC CHECKDB` ou `DBCC CHECKTABLE` pour corriger ces erreurs. Il est possible que cela entraîne une perte de données. Pour plus d’informations sur l’utilisation de CHECKDB et les causes des problèmes d’endommagement de base de données, consultez l’article : [Guide pratique pour résoudre les erreurs de cohérence de base de données signalées par DBCC CHECKDB](https://support.microsoft.com/kb/2015748).
  
1. Cette erreur s’est peut-être produite parce que la requête qui accède à la table utilisait le niveau d’isolation `READ UNCOMMITTED` ou l’indicateur de requête `NOLOCK` (également connu sous le nom de lecture erronée).

   - Si `DBCC CHECKDB` ou `DBCC CHECKTABLE` n’affichent pas d’erreurs associées à cette table et aux données LOB, la cause la plus probable est l’utilisation d’une lecture erronée. Si c’est le cas pour votre application, vous devrez soit éviter d’utiliser une lecture erronée, soit retenter la requête.
  
   - Si vous découvrez qu’il s’agit de la cause de l’erreur, il n’existe aucun problème réel de cohérence de la base de données.

## <a name="more-information"></a>Informations complémentaires

Si l’endommagement de la base de données est la cause de ce problème, `DBCC CHECKDB` et/ou `DBCC CHECKTABLE` doivent signaler des erreurs. Toutefois, ces commandes ne signalent pas le message 7105. Les erreurs que vous rencontrez à partir de CHECKDB dépendent des éléments endommagés dans la référence aux structures LOB ou des structures LOB elles-mêmes.

- Si la ligne de page de base de données ne référence pas correctement une page LOB valide, vous pouvez voir des erreurs telles que :

    > Message 8929, niveau 16, état 1, ligne 1  
    ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039828480 (type de données dans la ligne) : erreurs détectées dans les données hors ligne avec l’ID 131203072 détenu par l’enregistrement de données identifié par RID = (1:179:1)  
    Message 8964, niveau 16, état 1, ligne 1  
    Erreur de table, ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039894016 (type de données LOB). Le nœud de données hors ligne à la page (1:177), emplacement 1, ID de texte 131203072 n’est pas référencé.  
    Message 8965, niveau 16, état 1, ligne 1  
    Erreur de table, ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039894016 (type de données LOB). Le nœud de données hors ligne à la page (255:177), emplacement 1, ID de texte 131203072 est référencé par la page (1:179), emplacement 1, mais n’a pas été détecté lors de l’analyse  

- Différents scénarios pour le problème peuvent entraîner une combinaison d’erreurs différente. Dans cet exemple :  

    > La page de base de données 1:179, emplacement 1 fait référence à une page LOB qui n’est pas une page valide dans la base de données (page 255:177). La page (1:177) est une page LOB valide, mais n’a jamais été référencée par une page de base de données. Dans ce cas, le problème est que la ligne de l’emplacement 1 de la page 1:179 fait référence à la page 255:177 au lieu de 1:177.

- La clé permettant de déterminer si les erreurs `DBCC CHECKDB` sont liées aux problèmes de page LOB consiste à rechercher les expressions « données hors ligne » et « type de données LOB ».

    > Le message 8929 est une erreur liée à la page de base de données qui référence les pages LOB.  
Le message 8964 est une erreur indiquant qu’une page LOB n’a été référencée par aucune page de base de données.  
Le message 8965 est une erreur indiquant qu’une page LOB a été référencée par une page de base de données, mais qu’elle n’existe pas en tant que page valide

    Dans de nombreuses situations impliquant ces types d’erreurs, la réparation entraînera la suppression des lignes qui pointent vers les données LOB et des données LOB elles-mêmes. L’algorithme de réparation tentera de supprimer uniquement les fragments LOB qui affectent les lignes de base de données en question, mais cela ne peut pas être garanti dans toutes les situations, en fonction des éléments endommagés dans l’arborescence LOB.

- Dans l’exemple ci-dessous, les messages retournés par CHECKTABLE avec `REPAIR_ALLOW_DATA_LOSS` ressemblent à ceci :

    > Réparation : enregistrement supprimé pour l’ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039828480 (type de données dans la ligne), à la page (1:179), emplacement 1. Les index seront reconstruits.  
    Réparation : colonne de données hors ligne supprimée avec l’ID 131203072, pour l’ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039894016 (type de données LOB), à la page (1:177), emplacement 1.  
    Message 8929, niveau 16, état 1, ligne 1  
    ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039828480 (type de données dans la ligne) : erreurs détectées dans les données hors ligne avec l’ID 131203072 détenu par l’enregistrement de données identifié par RID = (1:179:1)  
            L'erreur a été résolue.  
    Message 8964, niveau 16, état 1, ligne 1  
    Erreur de table, ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039894016 (type de données LOB). Le nœud de données hors ligne à la page (1:177), emplacement 1, ID de texte 131203072 n’est pas référencé.  
            L'erreur a été résolue.  
    Message 8965, niveau 16, état 1, ligne 1  
    Erreur de table, ID d’objet 2137058649, ID d’index 0, ID de partition 72057594038910976, ID d’unité d’allocation 72057594039894016 (type de données LOB). Le nœud de données hors ligne à la page (255:177), emplacement 1, ID de texte 131203072 est référencé par la page (1:179), emplacement 1, mais n’a pas été détecté lors de l’analyse.  
            Impossible de résoudre cette erreur

    Le dernier message qui indique `Could not repair this error` est trompeur. L’erreur a été réparée, car la ligne de page de base de données qui pointait vers la page non valide (255:177) a été supprimée.