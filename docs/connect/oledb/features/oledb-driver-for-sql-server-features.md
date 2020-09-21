---
title: Fonctionnalités OLE DB Driver pour SQL Server
description: Découvrez les fonctionnalités prises en charge par OLE DB Driver pour SQL Server telles que la mise en miroir de bases de données, les opérations asynchrones, Azure Active Directory et d’autres.
ms.custom: ''
ms.date: 02/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- MDAC [SQL Server]
- MSOLEDBSQL, about OLE DB Driver for SQL Server
- data access [OLE DB Driver for SQL Server], features
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8c93a622561a7c4384f4e7be3d0e95daf9d46abb
ms.sourcegitcommit: c95f3ef5734dec753de09e07752a5d15884125e2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88861452"
---
# <a name="ole-db-driver-for-sql-server-features"></a>Fonctionnalités OLE DB Driver pour SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  En plus de l’exposition des fonctionnalités Windows (anciennement Microsoft) Data Access Components (WDAC), OLE DB Driver pour SQL Server implémente plusieurs autres fonctionnalités pour tirer parti des fonctionnalités [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
## <a name="in-this-section"></a>Dans cette section    
 [Utilisation de la mise en miroir de bases de données](../../oledb/features/using-database-mirroring.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge l’utilisation de bases de données mises en miroir, à savoir la capacité de conserver une copie, ou un miroir, d’une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur un serveur de secours.  
  
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge les opérations asynchrones, à savoir la capacité d’un retour immédiat sans blocage sur le thread appelant.  

[Utilisation d’Azure Active Directory](using-azure-active-directory.md)  
Présente les nouvelles méthodes d’authentification introduites dans OLE DB Driver 18.2.1, dont les paramètres par défaut sont plus sécurisés et qui permettent la connexion à une instance Azure SQL Database à l’aide d’une identité fédérée.

 [Utilisation de MARS &#40;Multiple Active Result Sets&#41;](../../oledb/features/using-multiple-active-result-sets-mars.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge MARS (Multiple Active Result Set). MARS permet d'exécuter et de recevoir plusieurs jeux de résultats à l'aide d'une seule connexion de base de données.  
  
 [Utilisation de types de données XML](../../oledb/features/using-xml-data-types.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge le type de données XML, qui peut être utilisé comme type de colonne, type de variable, type de paramètre ou type de retour de fonction.  
  
 [Utilisation de types définis par l’utilisateur](../../oledb/features/using-user-defined-types.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge UDT, qui étend le système de types SQL en permettant de stocker les objets et les structures de données personnalisées dans une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Utilisation de types de valeur élevée](../../oledb/features/using-large-value-types.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge les types de données de valeur élevée, à savoir les types de données LOB.  
  
 [Changement des mots de passe par programmation](../../oledb/features/changing-passwords-programmatically.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge la gestion des mots de passe périmés afin que les mots de passe puissent désormais être modifiés sur le client sans intervention de l’administrateur.  
  
 [Utilisation du niveau d’isolement de capture instantanée](../../oledb/features/working-with-snapshot-isolation.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge l’amélioration apportée au contrôle de version de ligne qui optimise les performances de la base de données en évitant les scénarios de blocage du lecteur/enregistreur.  
  
 [Utilisation de notifications de requêtes](../../oledb/features/working-with-query-notifications.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge la notification du consommateur en cas de modification de l'ensemble de lignes.  
  
 [Exécution d'opérations de copie en bloc](../../oledb/features/performing-bulk-copy-operations.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge les opérations de copie en bloc qui autorisent le transfert d’importantes quantités de données vers ou depuis une table ou une vue [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
 [Utilisation du chiffrement sans validation](../../oledb/features/using-encryption-without-validation.md)  
 Explique comment utiliser OLE DB Driver pour SQL Server afin de chiffrer les données envoyées au serveur sans validation du certificat.  
  
 [Paramètres table &#40;OLE DB Driver pour SQL Server&#41;](../../oledb/features/table-valued-parameters-oledb-driver-for-sql-server.md)  
 Présente la prise en charge d’OLE DB Driver pour SQL Server pour les paramètres table.  
  
 [Types CLR volumineux définis par l’utilisateur](../../oledb/features/large-clr-user-defined-types.md)  
 Explique la prise en charge des types UDT volumineux du CLR.  
  
 [Prise en charge de FILESTREAM](../../oledb/features/filestream-support.md)  
 Présente la prise en charge d’OLE DB Driver pour SQL Server pour la fonctionnalité FILESTREAM améliorée.  
  
 [Prise en charge des noms de principal du service &#40;SPN&#41; dans les connexions clientes](../../oledb/features/service-principal-name-spn-support-in-client-connections.md)  
 Explique comment la prise en charge des noms de principaux du service a été étendue pour permettre l'authentification mutuelle à travers l'ensemble des protocoles.  
  
 [Prise en charge des colonnes éparses dans OLE DB Driver pour SQL Server](../../oledb/features/sparse-columns-support-in-oledb-driver-for-sql-server.md)  
 Présente la prise en charge d’OLE DB Driver pour SQL Server pour les colonnes éparses.  
  
 [Améliorations des types de données date et heure](../../oledb/features/date-and-time-improvements.md)  
 Décrit la prise en charge par OLE DB Driver pour SQL Server des nouveaux types de données date et time.  
  
 [Détection des métadonnées](../../oledb/features/metadata-discovery.md)  
 Décrit les améliorations apportées à la découverte des métadonnées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Prise en charge d’UTF-16 dans OLE DB Driver pour SQL Server](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)  
 Décrit un changement de comportement introduit dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. Si vous fournissez une mémoire tampon de longueur fixe lors de la liaison d’un résultat de colonne ou d’un paramètre de sortie et que le caractère **wchar** écrit dans la mémoire tampon avant le caractère de fin est un point de code de substitut étendu d’une paire de substitution, et si le caractère **wchar** suivant est un point de code de substitut faible, OLE DB Driver pour SQL Server n’ajoutera pas le point de code de substitut étendu à la mémoire tampon.  
 
 [Prise en charge d’UTF-8 dans OLE DB Driver pour SQL Server](../../oledb/features/utf-8-support-in-oledb-driver-for-sql-server.md)  
 Présente la prise en charge de l’encodage et de la configuration du serveur UTF-8 que les utilisateurs doivent effectuer lors de l’utilisation de données encodées en UTF-8.
  
 [Prise en charge de la récupération d’urgence et de la haute disponibilité par OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-support-for-high-availability-disaster-recovery.md)  
 Explique comment votre application peut être configurée pour tirer parti des fonctionnalités de récupération d'urgence haute disponibilité, ajoutées dans [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)].  
  
 [Accès aux informations de diagnostic dans le journal des événements étendus](../../oledb/features/accessing-diagnostic-information-in-the-extended-events-log.md)  
 Présente les améliorations apportées à OLE DB Driver pour SQL Server et au suivi de données qui vous donne accès aux informations de diagnostic dans la mémoire tampon en anneau et le journal XEvents.  
  
 [Prise en charge de la base de données locale par OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-support-for-localdb.md)  
 Présente la prise en charge d’OLE DB Driver pour SQL Server pour la fonctionnalité LocalDB.  
  
 [Utilisation de la résolution d’adresses IP réseau transparente](../../oledb/features/using-transparent-network-ip-resolution.md)  
 Explique comment OLE DB Driver pour SQL Server prend en charge la résolution d’adresses IP réseau transparente dans un cluster de basculement.  
  
## <a name="see-also"></a>Voir aussi  
 [OLE DB Driver for SQL Server](../../oledb/oledb-driver-for-sql-server.md)      
 [Rubriques de procédures OLE DB](../../oledb/ole-db-how-to/ole-db-how-to-topics.md)   
 [Installation d’OLE DB Driver pour SQL Server](../../oledb/applications/installing-oledb-driver-for-sql-server.md)  
  
  
