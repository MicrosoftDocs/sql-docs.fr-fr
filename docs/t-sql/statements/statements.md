---
title: instructions Transact-SQL
description: instructions Transact-SQL
ms.prod: sql
ms.prod_service: sql-data-warehouse, database-engine, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- Alter_TSQL
dev_langs:
- TSQL
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.custom: ''
ms.date: 04/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 65f48884fff2fc5ce9fc018f8d79b3c2da5a4770
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98079558"
---
# <a name="transact-sql-statements"></a>instructions Transact-SQL

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Une instruction SQL est une unité de travail atomique qui réussit complètement ou échoue entièrement. Une instruction SQL est un ensemble d’instructions comprenant des identificateurs, des paramètres, des variables, des noms, des types de données et des mots réservés SQL qui compilent correctement. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] crée une transaction *implicite* pour une instruction SQL si la commande `BeginTransaction` ne spécifie pas le démarrage d'une transaction. [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] valide toujours une transaction implicite si l’instruction aboutit et l'annule si la commande échoue.  

Il existe de nombreux types d’instructions. Le plus important est peut-être que le [SELECT](../queries/select-transact-sql.md) récupère des lignes de la base de données et permet de sélectionner une ou plusieurs lignes ou colonnes d’une ou de plusieurs tables dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Cet article résume les catégories d’instructions à utiliser avec Transact-SQL (T-SQL) en plus de l’instruction `SELECT`. Vous trouverez toutes les instructions répertoriées dans le volet de navigation gauche.

## <a name="backup-and-restore"></a>Sauvegarde et restauration

Les instructions de sauvegarde et restauration permettent de créer des sauvegardes et des restauration à partir de sauvegardes.  Pour plus d’informations, consultez la [Vue d’ensemble de la sauvegarde et de la restauration](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md).

## <a name="data-definition-language"></a>Data Definition Language

Les instructions DDL (Data Definition Language) définissent des structures de données. Utilisez ces instructions pour créer, modifier ou supprimer des structures de données dans une base de données. Ces instructions incluent :

- ALTER
- Classements
- CREATE
- DROP
- DISABLE TRIGGER
- ENABLE TRIGGER
- RENAME
- UPDATE STATISTICS
- TRUNCATE TABLE

## <a name="data-manipulation-language"></a>Data Manipulation Language

DML (Data Manipulation Language) affecte les informations stockées dans la base de données. Utilisez ces instructions pour insérer, mettre à jour et modifier les lignes dans la base de données.

- BULK INSERT
- Suppression
- INSERT
- SELECT
- UPDATE
- MERGE

## <a name="permissions-statements"></a>Instructions d’autorisations

Les instructions d’autorisations déterminent les connexions et les utilisateurs qui peuvent accéder aux données et effectuer des opérations. Pour plus d’informations sur l’authentification et l’accès, consultez le [Centre de sécurité](../../relational-databases/security/security-center-for-sql-server-database-engine-and-azure-sql-database.md).

## <a name="service-broker-statements"></a>Instructions de Service Broker

Service Broker est une fonctionnalité qui fournit la prise en charge native des applications de messagerie et de mise en file d’attente. Pour plus d’informations, consultez [Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md).

## <a name="session-settings"></a>Paramètres de session

Les instructions SET déterminent comment la session active gère les paramètres au moment de l’exécution. Pour obtenir une vue d’ensemble, consultez [Instructions SET](set-statements-transact-sql.md).
