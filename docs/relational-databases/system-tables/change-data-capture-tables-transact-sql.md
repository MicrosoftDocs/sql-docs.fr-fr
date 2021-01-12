---
description: Tables de capture des données modifiées (Transact-SQL)
title: Tables de capture de données modifiées (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: a4372d0b-50ca-4e58-80f6-2ed3cb52a84a
author: cawrites
ms.author: chadam
ms.openlocfilehash: 6ef80682d8d95188aa764c6e04e768c57298715a
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98094926"
---
# <a name="change-data-capture-tables-transact-sql"></a>Tables de capture des données modifiées (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  La capture de données modifiées active le suivi des modifications sur les tables afin que les modifications du langage de manipulation de données (DML) et du langage de définition de données (DDL) apportées aux tables puissent être chargées de façon incrémentielle dans un entrepôt de données. Les rubriques de cette section décrivent les tables système qui stockent les informations utilisées par les opérations de capture de données modifiées.  
  
## <a name="in-this-section"></a>Dans cette section  
 [cdc.<capture_instance>_CT](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)  
 Retourne une ligne pour chaque modification apportée à une colonne capturée dans la table source associée.  
  
 [cdc.captured_columns](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
 Retourne une ligne pour chaque colonne suivie dans une instance de capture.  
  
 [cdc.change_tables](../../relational-databases/system-tables/cdc-change-tables-transact-sql.md)  
 Retourne une ligne pour chaque table de modifications de la base de données.  
  
 [cdc.ddl_history](../../relational-databases/system-tables/cdc-ddl-history-transact-sql.md)  
 Retourne une ligne pour chaque modification du langage de définition de données (DDL) apportée aux tables qui sont activées pour la capture des données modifiées.  
  
 [cdc.lsn_time_mapping](../../relational-databases/system-tables/cdc-lsn-time-mapping-transact-sql.md)  
 Retourne une ligne pour chaque transaction qui a des lignes dans une table de modifications. Cette table est utilisée pour le mappage entre les valeurs de validation du numéro séquentiel dans le journal et l'heure de validation de la transaction.  
  
 [cdc.index_columns](../../relational-databases/system-tables/cdc-index-columns-transact-sql.md)  
 Retourne une ligne pour chaque colonne d'index associée à une table de modifications.  
  
 [dbo.cdc_jobs &#40;Transact-SQL&#41;](../../relational-databases/system-tables/dbo-cdc-jobs-transact-sql.md)  
 Retourne les paramètres de configuration pour les travaux de l'agent de capture des données modifiées.  
  
## <a name="see-also"></a>Voir aussi  
 [Procédures stockées de capture des données modifiées &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/change-data-capture-stored-procedures-transact-sql.md)   
 [Fonctions de capture de données modifiées &#40;Transact-SQL&#41;](../../relational-databases/system-functions/change-data-capture-functions-transact-sql.md)  
  
  
