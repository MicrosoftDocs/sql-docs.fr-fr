---
description: MSSQLSERVER_1785
title: MSSQLSERVER_1785
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 1785 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 7f300583da4c034da2963590c81e0aedbed86beb
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596261"
---
# <a name="mssqlserver_1785"></a>MSSQLSERVER_1785
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|1785|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|CRTFKINVTOPO|
|Texte du message|L'introduction de la contrainte FOREIGN KEY '%.ls' sur la table '%. ls' peut générer des cycles ou des chemins de cascade multiples. Spécifiez ON DELETE NO ACTION ou ON UPDATE NO ACTION, ou modifiez d'autres contraintes FOREIGN KEY.|
||

## <a name="explanation"></a>Explication

Vous recevez ce message d’erreur car, dans SQL Server, une table ne peut pas apparaître plus d’une fois dans une liste de toutes les actions référentielles en cascade démarrées par une instruction `DELETE` ou `UPDATE`. L’arborescence des actions référentielles en cascade ne doit avoir qu’un seul chemin d’accès à une table particulière dans l’arborescence des actions référentielles en cascade.

Un message d’erreur semblable au suivant s’affiche :

> Serveur :  MSG 1785, niveau 16, état 1, ligne 1 - L’introduction de la contrainte FOREIGN KEY 'fk_two' sur la table 'table2' peut générer des cycles ou des chemins de cascade multiples. Spécifiez ON DELETE NO ACTION ou ON UPDATE NO ACTION, ou modifiez d'autres contraintes FOREIGN KEY. Serveur :  MSG 1750, niveau 16, état 1, ligne 1 - Impossible de créer la contrainte. Consulter les erreurs précédentes

## <a name="user-action"></a>Action requise

Pour résoudre ce problème, créez une clé étrangère qui crée un chemin d’accès unique à une table dans une liste d’actions référentielles en cascade.

Vous pouvez appliquer l’intégrité référentielle de plusieurs façons. L’intégrité référentielle déclarative (DRI) est la méthode la plus simple, mais également la moins flexible. Si vous avez besoin de davantage de flexibilité, mais que vous souhaitez toujours un degré élevé d’intégrité, vous pouvez utiliser des déclencheurs à la place.

## <a name="more-information"></a>Informations complémentaires

L’exemple de code suivant est un exemple de tentative de création de clé étrangère qui génère le message d’erreur :

```sql
USE tempdb
GO

CREATE TABLE table1 (user_ID INTEGER NOT NULL PRIMARY KEY, user_name
CHAR(50) NOT NULL)
GO

CREATE TABLE table2 (author_ID INTEGER NOT NULL PRIMARY KEY, author_name
CHAR(50) NOT NULL, lastModifiedBy INTEGER NOT NULL, addedby INTEGER NOT NULL)
GO

ALTER TABLE table2 ADD CONSTRAINT fk_one FOREIGN KEY (lastModifiedby)
REFERENCES table1 (user_ID) ON DELETE CASCADE ON UPDATE cascade
GO

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1(user_ID) ON DELETE NO ACTION ON UPDATE cascade
GO
--this fails with the error because it provides a second cascading path to table2.

ALTER TABLE table2 ADD CONSTRAINT fk_two FOREIGN KEY (addedby)
REFERENCES table1 (user_ID) ON DELETE NO ACTION ON UPDATE NO ACTION
GO
-- this works.
```

### <a name="see-also"></a>Voir aussi

[Intégrité référentielle en cascade](../tables/primary-and-foreign-key-constraints.md#referential-integrity)