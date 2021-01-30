---
description: sys.syslockinfo (Transact-SQL)
title: sys.sysLockInfo (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 03/15/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- syslockinfo_TSQL
- sys.syslockinfo_TSQL
- sys.syslockinfo
- syslockinfo
dev_langs:
- TSQL
helpviewer_keywords:
- syslockinfo system table
- sys.syslockinfo compatibility view
ms.assetid: d8cae434-807a-473e-b94f-f7a0e1b2daf0
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8fb36287697a68044f8118461501dd94eb86ebc7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99149381"
---
# <a name="syssyslockinfo-transact-sql"></a>sys.syslockinfo (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur toutes les demandes de verrou accordées, en conversion et en attente.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
> [!IMPORTANT]  
>  Cette fonction a été modifiée par rapport aux versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [modifications critiques apportées aux fonctionnalités de moteur de base de données dans SQL Server 2016](../../database-engine/breaking-changes-to-database-engine-features-in-sql-server-2016.md).  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**rsc_text**|**nchar(32)**|Description textuelle d’une ressource de verrou. Contient une partie du nom de la ressource.|  
|**rsc_bin**|**Binary(16**|Ressource de verrouillage binaire. Contient la ressource de verrouillage réelle contenue dans le gestionnaire de verrouillage. Cette colonne est incluse pour les outils qui connaissent le format des ressources de verrou pour générer leur propre ressource de verrouillage mise en forme et pour effectuer des jointures réflexives sur **syslockinfo**.|  
|**rsc_valblk**|**Binary(16**|Bloc de valeur de verrouillage. Certains types de ressources peuvent inclure des données supplémentaires dans la ressource de verrouillage qui n'est pas hachée par le gestionnaire de verrouillage, afin de déterminer le propriétaire d'une ressource de verrouillage particulière. Par exemple, les verrous de page ne sont pas détenus par un ID d'objet particulier. Pour l'escalade de verrous et d'autres utilisations. Toutefois, l'ID d'objet d'un verrou de page peut être inclus dans le bloc de valeurs de verrouillage.|  
|**rsc_dbid**|**smallint**|ID de la base de données associé à la ressource.|  
|**rsc_indid**|**smallint**|ID de l'index associé à la ressource (le cas échéant).|  
|**rsc_objid**|**int**|ID de l'objet associé à la ressource (le cas échéant).|  
|**rsc_type**|**tinyint**|Type de ressource :<br /><br /> 1 = Ressource NULL (inutilisée)<br /><br /> 2 = base de données<br /><br /> 3 = Fichier<br /><br /> 4 = Index<br /><br /> 5 = Table<br /><br /> 6 = Page<br /><br /> 7 = Clé<br /><br /> 8 = Extension<br /><br /> 9 = RID (ID de ligne)<br /><br /> 10 = Application|  
|**rsc_flag**|**tinyint**|Indicateurs de ressource interne.|  
|**req_mode**|**tinyint**|Mode de requête de verrouillage. Cette colonne correspond au mode de verrouillage du demandeur et représente le mode Accordé, le mode En conversion ou le mode En attente.<br /><br /> 0 = NULL. Aucun accès n'est accordé à la ressource. Sert d'espace réservé.<br /><br /> 1 = Sch-S (Stabilité du schéma). Garantit que l'élément d'un schéma, tel qu'une table ou un index, n'est pas supprimé alors qu'une session contient un verrou de stabilité du schéma sur l'élément du schéma.<br /><br /> 2 = Sch-M (Modification du schéma). Doit être détenu par toute session destinée à modifier le schéma de la ressource spécifiée. Garantit qu'aucune autre session ne fait référence à l'objet indiqué.<br /><br /> 3 = S (Partagé). La session détenant le verrou peut disposer d'un accès partagé à la ressource.<br /><br /> 4 = U (Mise à jour). Indique qu'un verrouillage de mise à jour a été posé sur des ressources qui peuvent finalement être mises à jour. Utilisé pour empêcher l'occurrence d'une forme de blocage courante qui apparaît lorsque plusieurs sessions verrouillent les ressources pour une mise à jour potentielle ultérieure.<br /><br /> 5 = X (Exclusif). La session détenant le verrou peut disposer d'un accès exclusif à la ressource.<br /><br /> 6 = IS (Partage intentionnel). Indique l'intention de placer des verrous S sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 7 = IU (Mise à jour intentionnelle). Indique l'intention de placer des verrous U sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 8 = IX (Exclusion intentionnelle). Indique l'intention de placer des verrous X sur certaines ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 9 = SIU (Mise à jour intentionnelle partagée). Signale des accès partagés à une ressource dans le but de poser des verrous de mise à jour sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 10 = SIX (Partage intentionnel exclusif). Signale des accès partagés à une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 11 = UIX (Mise à jour intentionnelle exclusive). Signale un verrou de mise à jour sur une ressource dans le but de poser des verrous exclusifs sur les ressources subordonnées dans la hiérarchie de verrouillage.<br /><br /> 12 = BU. Utilisé par les opérations en bloc.<br /><br /> 13 = RangeS_S (Verrou de groupes de clés partagés et de ressources partagées). Indique une analyse de plage sérialisable.<br /><br /> 14 = RangeS_U (Verrou de groupes de clés partagés et de ressources de mise à jour). Indique une analyse de mise à jour sérialisable.<br /><br /> 15 = RangeI_N (Verrou d'insertion de groupe de clé et de ressources NULL). Utilisé pour tester les étendues avant l'insertion d'une nouvelle clé dans un index.<br /><br /> 16 = RangeI_S. Verrou de conversion de groupes de clés, créé par un chevauchement de verrous RangeI_N et S.<br /><br /> 17 = RangeI_U. Verrou de conversion de groupes de clés, créé par un chevauchement de verrous RangeI_N et U.<br /><br /> 18 = RangeI_X. Verrou de conversion de groupes de clés, créé par un chevauchement de verrous RangeI_N et X.<br /><br /> 19 = RangeX_S. Verrou de conversion de groupes de clés, créé par un chevauchement de verrous RangeI_N et RangeS_S.<br /><br /> 20 = RangeX_U. Verrou de conversion de groupes de clés, créé par un chevauchement de verrous RangeI_N et RangeS_U.<br /><br /> 21 = RangeX_X (Verrou de groupes de clés exclusifs et de ressources exclusives). Verrou de conversion utilisé lors de la mise à jour d'une clé dans une étendue.|  
|**req_status**|**tinyint**|État de la demande de verrou :<br /><br /> 1 = Accordée<br /><br /> 2 = En conversion<br /><br /> 3 = En attente|  
|**req_refcnt**|**smallint**|Nombre de références du verrou. Chaque fois qu'une transaction demande un verrou sur une ressource, un nombre de références est incrémenté. Le verrou ne peut pas être libéré tant que le nombre de références n'est pas égal à 0.|  
|**req_cryrefcnt**|**smallint**|Réservé pour un usage ultérieur. Toujours défini à 0.|  
|**req_lifetime**|**int**|Bitmap de durée de vie du verrou. Au cours de certaines stratégies d'exécution de requêtes, les verrous doivent être maintenus sur les ressources jusqu'à ce que le processeur de requêtes ait terminé une phase précise de la requête. Le bitmap de durée de vie de verrou est utilisé par le processeur de requêtes et le gestionnaire des transactions pour indiquer les verrous qui peuvent être levés lorsqu'une certaine phase de la requête est terminée. Certains bits du bitmap sont utilisés pour indiquer les verrous maintenus jusqu'à la fin d'une transaction, même si leur nombre de références est égal à 0.|  
|**req_spid**|**int**|[!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] ID de processus interne de la session qui demande le verrou.|  
|**req_ecid**|**int**|ID de contexte d'exécution (ECID). Indique le thread qui détient un verrou dans une opération parallèle.|  
|**req_ownertype**|**smallint**|Type d'objet associé au verrou :<br /><br /> 1 = Transaction<br /><br /> 2 = Curseur<br /><br /> 3 = Session<br /><br /> 4 = ExSession<br /><br /> Notez que les valeurs 3 et 4 représentent une version spéciale des verrous de session, respectivement pour le suivi des verrous de base de données et de groupe de fichiers.|  
|**req_transactionID**|**bigint**|ID de transaction unique utilisé dans **syslockinfo** et dans l’événement du profileur|  
|**req_transactionUOW**|**uniqueidentifier**|Identifie l'ID de l'UOW (Unit of Work) de la transaction DTC. Pour les transactions non-MS DTC, UOW a la valeur 0.|  
  
## <a name="permissions"></a>Autorisations  
 requièrent l'autorisation VIEW SERVER STATE sur le serveur.  
  
## <a name="see-also"></a>Voir aussi  
 [Mappage de tables système à des vues système &#40;Transact-SQL&#41;](../../relational-databases/system-tables/mapping-system-tables-to-system-views-transact-sql.md)   
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  
