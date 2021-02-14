---
description: Événements étendus pour Stretch Database
title: Événements étendus pour Stretch Database
ms.date: 06/14/2016
ms.service: sql-server-stretch-database
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 70485e74-2e25-4e7e-be6c-9dd1780a42e3
author: rothja
ms.author: jroth
ms.custom: seo-dt-2019
ms.openlocfilehash: f965b68279398720ff7b1b9bcb76b5ec73410466
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080030"
---
# <a name="extended-events-for-stretch-database"></a>Événements étendus pour Stretch Database
[!INCLUDE [sqlserver2016-windows-only](../../includes/applies-to-version/sqlserver2016-windows-only.md)]


Stretch Database fournit un ensemble d’événements étendus à des fins de résolution des problèmes.  
  
Pour plus d’informations, consultez [Événements étendus](../../relational-databases/extended-events/extended-events.md). Pour plus d’informations sur la façon de démarrer une session d’événements étendus à des fins de résolution des problèmes, consultez [Créer une session d’événements étendus](/previous-versions/sql/sql-server-2016/hh213147(v=sql.130)).  
  
## <a name="list-of-extended-events-for-stretch-database"></a>Liste des événements étendus pour Stretch Database  
  
Nom d'événement|Description de l'événement   
---------|---------  
remote_data_archive_db_ddl|Se produit quand le ddl T-SQL de la base de données est traitée pour l’extension des données.  
remote_data_archive_provision_operation|Se produit quand une opération d’approvisionnement démarre ou se termine.  
remote_data_archive_query_rewrite|Se produit quand RelOp_Get est remplacé au cours d’une réécriture de requête pour Stretch.  
remote_data_archive_table_ddl|Se produit quand le ddl T-SQL de la table est traitée pour l’extension des données.  
remote_data_archive_telemetry|Se produit chaque fois qu’un système local transmet un événement de télémétrie à la base de données Azure.  
remote_data_archive_telemetry_rejected|Se produit chaque fois qu’un événement de télémétrie AzureDB Stretch est rejeté.  
repopulate_stretch_schema_task_queue_complete|Signale la fin du nouveau remplissage de la file d’attente des tâches du schéma d’extension.  
repopulate_stretch_schema_task_queue_start|Signale le début du nouveau remplissage de la file d’attente des tâches du schéma d’extension.  
stretch_codegen_errorlog|Signale la sortie du générateur de code.  
stretch_codegen_start|Signale le début de la génération du code d’extension.  
stretch_create_remote_table_start|Signale le début de la création de table distante.  
stretch_database_disable_completed|Indique la fin d’une commande ALTER DATABASE SET REMOTE_DATA_ARCHIVE OFF.  
stretch_database_enable_completed|Indique la fin d’une commande ALTER DATABASE SET REMOTE_DATA_ARCHIVE ON.  
stretch_database_reauthorize_completed|Signale la fin d’une procédure spécifiée sp_rda_reauthorize_db.  
stretch_index_reconciliation_codegen_completed|Signale la fin de la génération de code pour l’opération d’index distant d’extension.  
stretch_index_update_step_completed|Signale la durée d’une opération de mise à jour d’un index étendu.  
stretch_migration_debug_trace|Trace de débogage des actions de migration d’extension.  
stretch_migration_dequeue_migration|Événement déclenché quand une tâche de migration d’extension est retirée de la file d’attente pour une base de données.  
stretch_migration_queue_migration|Met en file d’attente un paquet pour le démarrage de la migration de la base de données et de l’objet.  
stretch_migration_requeue_migration|Événement déclenché quand un paquet de la tâche de migration d’extension est replacé en file d’attente.  
stretch_migration_start_migration|Démarre la migration de la base de données et de l’objet.  
stretch_migration_start_unmigration|Démarre l’annulation de la migration de la base de données et de l’objet.  
stretch_remote_column_execution_completed|Signale la fin de l’exécution à distance du code généré pour une colonne étendue.  
stretch_remote_column_reconciliation_codegen_completed|Signale la fin de la génération de code pour un rapprochement de colonnes distantes d’extension.  
stretch_remote_index_execution_completed|Signale la fin de l’exécution distante du code généré pour un index étendu.  
stretch_schema_queue_task|Signale le moment où un paquet est sur le point d’être placé en file d’attente pour le traitement d’une tâche de schéma pour la base de données et l’objet.  
stretch_schema_script_execution_completed|Signale la fin de l’exécution d’un script d’extension pendant le traitement de la tâche de schéma d’extension.  
stretch_schema_script_execution_skipped|Signale que l’exécution d’un script d’extension est ignorée pendant le traitement de la tâche de schéma d’extension.  
stretch_schema_script_execution_start|Signale le début de l’exécution d’un script d’extension pendant le traitement de la tâche de schéma d’extension.  
stretch_schema_task_failed|Signale l’échec d’une fonction de schéma d’extension pendant la tâche de schéma d’extension.  
stretch_schema_task_skipped|Signale que la tâche de schéma d’extension est ignorée pendant la fonction de schéma d’extension.  
stretch_schema_task_start|Signale le démarrage de la fonction de schéma d’extension pendant la tâche de schéma d’extension.  
stretch_schema_task_succeeded|Signale la fin correcte d’une fonction de schéma d’extension pendant la tâche de schéma d’extension.  
stretch_sp_migration_get_batch_id|Appelle sp_stretch_get_batch_id.  
stretch_sync_metadata_start|Indique le début des vérifications de métadonnées pendant la tâche de migration.  
stretch_table_codegen_completed|Indique la fin de la génération de code pour une table étendue.  
stretch_table_complete_data_reconciliation|Effectue le rapprochement de la base de données et de l’objet.  
stretch_table_data_reconciliation_event|Signale la fin du rapprochement des données d’un lot de lignes.  
stretch_table_data_reconciliation_results_event|Signale une erreur ou la fin d’un rapprochement correct des données de plusieurs lots de lignes.  
stretch_table_hinted_admin_delete_event|Signale l’exécution d’une opération DML de suppression Stretch qui utilise un indicateur admin.  
stretch_table_hinted_admin_update_event|Signale l’exécution d’une opération DML de mise à jour Stretch qui utilise un indicateur admin.  
stretch_table_provisioning_step_completed|Indique la durée d’une opération d’approvisionnement de table étendue.  
stretch_table_query_error|Signale une erreur survenue pendant la réécriture d’une requête Stretch.  
stretch_table_remote_creation_completed|Indique la fin de l’exécution distante du code généré pour une table étendue.  
stretch_table_row_migration_event|Indique la fin de la migration d’un lot de lignes.  
stretch_table_row_migration_results_event|Indique une erreur ou la fin d’une migration réussie de plusieurs lots de lignes.  
stretch_table_row_unmigration_event|Signale la fin de l’annulation de la migration d’un lot de lignes.  
stretch_table_row_unmigration_results_event|Signale une erreur ou la fin d’une annulation de migration réussie de plusieurs lots de lignes.  
stretch_table_start_data_reconciliation|Démarre le rapprochement des données de la base de données et de l’objet.  
stretch_table_unprovision_completed|Indique la fin de la suppression de ressources locales pour une table dont l’extension a été annulée.  
stretch_table_validation_error|Indique la fin de la validation d’une table quand l’utilisateur active l’extension.  
stretch_unprovision_table_start|Indique le début de l’annulation de l’approvisionnement de la table d’extension.  
  
## <a name="see-also"></a>Voir aussi  
[Gérer et dépanner Stretch Database](../../sql-server/stretch-database/manage-and-troubleshoot-stretch-database.md)