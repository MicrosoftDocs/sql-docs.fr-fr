---
title: Objets système liés à XEvents
description: Ces ressources sont liées aux événements étendus, notamment la façon dont les objets système les prennent en charge, la manière dont SQL Server les utilise et les aspects spécifiques à Azure SQL Database.
ms.date: 03/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: jukoesma
ms.technology: xevents
ms.topic: reference
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ef31edf367eacd41135bb4db9a58cd8ba4950de8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346718"
---
# <a name="system-objects-that-support-extended-events"></a>Objets système qui prennent en charge les événements étendus

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

Cet article fournit des liens vers d’autres articles relatifs aux événements étendus. Il s’agit d’articles qui décrivent les éléments suivants :

- Objets système qui prennent en charge la fonctionnalité d’événements étendus.
- Parties de SQL Server qui utilisent les événements étendus.
- Aspects des événements étendus qui sont spécifiques à Azure SQL Database dans le cloud.

Les listes ne sont pas nécessairement complètes.

## <a name="system-tables"></a>Tables système

- [Tables d’événements étendus - trace_xe_action_map](../system-tables/extended-events-tables-trace-xe-action-map.md)

- [Tables d’événements étendus - trace_xe_event_map](../system-tables/extended-events-tables-trace-xe-event-map.md)

## <a name="system-catalog-views"></a>Vues de catalogue système

- [Affichages catalogue des événements étendus (Transact-SQL)](../system-catalog-views/extended-events-catalog-views-transact-sql.md)

- [sys.server_event_sessions (Transact-SQL)](../system-catalog-views/sys-server-event-sessions-transact-sql.md)

- [sys.server_event_session_actions (Transact-SQL)](../system-catalog-views/sys-server-event-session-actions-transact-sql.md)

- [sys.server_event_session_events (Transact-SQL)](../system-catalog-views/sys-server-event-session-events-transact-sql.md)

- [sys.server_event_session_fields (Transact-SQL)](../system-catalog-views/sys-server-event-session-fields-transact-sql.md)

- [sys.server_event_session_targets (Transact-SQL)](../system-catalog-views/sys-server-event-session-targets-transact-sql.md)

## <a name="other-system-objects"></a>Autres objets système

- [Vues de gestion dynamique des Événements étendus](../system-dynamic-management-views/extended-events-dynamic-management-views.md)

## <a name="uses-of-extended-events-by-sql-server-itself"></a>Utilisations des événements étendus par SQL Server lui-même

Cette liste n’est pas censée être complète.

- [Accès aux informations de diagnostic dans le journal des événements étendus](../native-client/features/accessing-diagnostic-information-in-the-extended-events-log.md)

- [Configurer les événements étendus pour les groupes de disponibilité Always On](../../database-engine/availability-groups/windows/always-on-extended-events.md)

- [Événements étendus pour Stretch Database](../../sql-server/stretch-database/extended-events-for-stretch-database.md)

## <a name="azure-sql-database-and-extended-events"></a>Azure SQL Database et les événements étendus

- [Événements étendus dans Azure SQL Database](/azure/sql-database/sql-database-xevent-db-diff-from-svr)

- [sys.database_event_session_targets (Azure SQL Database)](../system-catalog-views/sys-database-event-session-targets-azure-sql-database.md)

- [sys.database_event_session_fields (Azure SQL Database)](../system-catalog-views/sys-database-event-session-fields-azure-sql-database.md)

- [sys.database_event_session_events (Azure SQL Database)](../system-catalog-views/sys-database-event-session-events-azure-sql-database.md)

- [sys.database_event_session_actions (Azure SQL Database)](../system-catalog-views/sys-database-event-session-actions-azure-sql-database.md)
