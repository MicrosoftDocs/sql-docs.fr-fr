---
description: À propos des propriétés de OLE DB SQL Server Native Client
title: À propos des propriétés OLE DB | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- OLE DB, properties
- SQL Server Native Client OLE DB provider, properties
- properties [OLE DB]
- property values [SQL Server Native Client]
ms.assetid: 0b36a61e-b542-400d-a3d2-e6f643caf2c6
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: b443a9389e45cdaa8abd8bb64f6845fbb47c7106
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91864934"
---
# <a name="about-sql-server-native-client-ole-db-properties"></a>À propos des propriétés de OLE DB SQL Server Native Client
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  Les consommateurs définissent des valeurs de propriétés afin de demander un comportement d'objet spécifique. Par exemple, ils utilisent des propriétés pour spécifier les interfaces à exposer par un ensemble de lignes. Ils obtiennent les valeurs de propriétés afin de déterminer les capacités d'un objet, tel qu'un ensemble de lignes, une session ou un objet source de données.  
  
 Chaque propriété a une valeur, un type, une description et un attribut de lecture/écriture et, pour les propriétés d'ensemble de lignes, un indicateur signalant s'il peut être appliqué sur la base de chaque colonne.  
  
 Une propriété est identifiée par un GUID et un entier représentant l'ID de propriété. Un jeu de propriétés est un jeu de toutes les propriétés qui partagent le même GUID. Outre les jeux de propriétés OLE DB prédéfinis, le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] fournisseur de OLE DB Native Client implémente des jeux de propriétés et des propriétés spécifiques au fournisseur. Chaque propriété appartient à un ou plusieurs groupes de propriétés. Un groupe de propriétés est le groupe de toutes les propriétés qui s'appliquent à un objet particulier. Parmi les groupes de propriétés, on peut citer le groupe de propriétés d'initialisation, le groupe de propriétés de source de données, le groupe de propriétés de session, le groupe de propriétés d'ensemble de lignes, le groupe de propriétés de table et le groupe des propriétés de colonnes. Il y a des propriétés dans chacun de ces groupes de propriétés.  
  
 La définition de valeurs de propriétés implique les opérations suivantes  
  
1.  Détermination des propriétés pour lesquelles il faut définir des valeurs.  
  
2.  Détermination des jeux de propriétés qui contiennent les propriétés identifiées.  
  
3.  Allocation d'un tableau de structures DBPROPSET, une pour chaque jeu de propriétés identifié.  
  
4.  Allocation d'un tableau de structures DBPROP pour chaque jeu de propriétés. Le nombre d'éléments dans chaque tableau est le nombre de propriétés (identifiées à l'Étape 1) qui appartiennent à ce jeu de propriétés.  
  
5.  Remplissage de la structure DBPROP pour chaque propriété.  
  
6.  Remplissage des informations (GUID de jeu de propriétés, nombre d'éléments et un pointeur vers le tableau DBPROP correspondant) dans la structure DBPROPSET pour chaque jeu de propriétés.  
  
7.  Appel d'une méthode pour définir des propriétés et passer le nombre et le tableau de structures DBPROPSET.  
  
## <a name="see-also"></a>Voir aussi  
 [Création d’une application SQL Server Native Client OLE DB Provider](../../relational-databases/native-client-ole-db-provider/creating-a-sql-server-native-client-ole-db-provider-application.md)   
 [Propriétés (OLE DB)](/previous-versions/windows/desktop/ms722734(v=vs.85))  
  
