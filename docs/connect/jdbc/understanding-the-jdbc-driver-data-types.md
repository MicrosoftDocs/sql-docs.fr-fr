---
title: Présentation des types de données du pilote JDBC
description: En savoir plus sur les types de données JDBC et sur la manière dont Microsoft JDBC Driver pour SQL Server convertit ces types en types de base de données.
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: 7802328d-4d23-4775-9573-4169b127d258
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 48090db2e671fdfb1fb076645b06c9315b32bdd5
ms.sourcegitcommit: 620a868e623134ad6ced6728ce9d03d7d0038fe0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87410915"
---
# <a name="understanding-the-jdbc-driver-data-types"></a>Présentation des types de données du pilote JDBC

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

Le [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] prend en charge les types de données JDBC de base et avancés dans une application Java utilisant [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] comme base de données.  
  
Le système de type JDBC modère la conversion entre les types de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les types et objets du langage Java. Les types JDBC sont modelés sur les types SQL-92 et SQL-99. Le pilote JDBC adhère à la spécification JDBC et est conçu pour fournir un bon compromis entre la prévisibilité et la flexibilité.  
  
Les rubriques de cette section décrivent comment utiliser les types de données de base et avancés et comment les types de données peuvent être convertis en d'autres types de données.  
  
## <a name="in-this-section"></a>Contenu de cette section  
  
| Rubrique                                                                                                                                            | Description                                                                                                                                                                                                                                                          |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Utilisation des types de données de base](../../connect/jdbc/using-basic-data-types.md)                                                                           | Décrit les types de données de base JDBC. Inclut des exemples illustrant comment travailler avec les types de données en utilisant des jeux de résultats, des requêtes paramétrables et des procédures stockées.                                                                                                        |
| [Configuration du mode d'envoi des valeurs java.sql.Time au serveur](../../connect/jdbc/configuring-how-java-sql-time-values-are-sent-to-the-server.md) | Décrit la manière dont le pilote JDBC génère les dates.                                                                                                                                                                                                                       |
| [Utilisation des types de données avancés](../../connect/jdbc/using-advanced-data-types.md)                                                                     | Décrit les types de données avancés JDBC.                                                                                                                                                                                                                              |
| [Présentation des différences entre les types de données](../../connect/jdbc/understanding-data-type-differences.md)                                                 | Décrit les différences entre les différents types de données de pilote JDBC.                                                                                                                                                                                                    |
| [Présentation des conversions de types de données](../../connect/jdbc/understanding-data-type-conversions.md)                                                 | Explique comment est gérée la conversion de type de données lors de l'utilisation de méthodes getter et setter.                                                                                                                                                                                  |
| [Prise en charge des jeux de caractères nationaux](../../connect/jdbc/national-character-set-support.md)                                                           | Décrit la prise en charge des types du jeu de caractères nationaux.                                                                                                                                                                                                          |
| [Prise en charge des données XML](../../connect/jdbc/supporting-xml-data.md)                                                                                 | Décrit l'interface SQLXML. Explique également comment lire et écrire des données XML dans la base de données relationnelle avec le type de données Java **SQLXML**.                                                                                                             |
| [Wrappers et interfaces](../../connect/jdbc/wrappers-and-interfaces.md)                                                                         | Présente les interfaces utilisant les méthodes et constantes spécifiques du [!INCLUDE[jdbcNoVersion](../../includes/jdbcnoversion_md.md)] qui permettent à un serveur d’applications de créer un proxy de la classe. Présente également la prise en charge de l’interface `java.sql.Wrapper`. |
  
## <a name="see-also"></a>Voir aussi

[Présentation du pilote JDBC](../../connect/jdbc/overview-of-the-jdbc-driver.md)  
