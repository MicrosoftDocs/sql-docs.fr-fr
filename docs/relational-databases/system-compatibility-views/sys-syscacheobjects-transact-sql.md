---
description: sys.syscacheobjects (Transact-SQL)
title: sys.syscacheobjects (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.syscacheobjects_TSQL
- sys.syscacheobjects
- syscacheobjects
- syscacheobjects_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- syscacheobjects system table
- sys.syscacheobjects compatibility view
ms.assetid: 9b14f37c-b7f5-4f71-b070-cce89a83f69e
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 4e3374c67917f3468bb9b31d73acd47544d07405
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180724"
---
# <a name="syssyscacheobjects-transact-sql"></a>sys.syscacheobjects (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  Contient des informations sur l'utilisation du cache.  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssnoteCompView](../../includes/ssnotecompview-md.md)]  
  
|Nom de la colonne|Type de données|Description|  
|-----------------|---------------|-----------------|  
|**bucketid**|**int**|ID du compartiment. La valeur est comprise entre 0 et (taille du répertoire -1). La taille du répertoire est la taille de la table de hachage.|  
|**cacheobjtype**|**nvarchar(17)**|Type de l'objet dans le cache :<br /><br /> Plan compilé<br /><br /> Plan exécutable<br /><br /> Arborescence d'analyse<br /><br /> Curseur<br /><br /> Procédure stockée étendue|  
|**déclaré**|**nvarchar(8)**|Type d’objet :<br /><br /> Procédure stockée<br /><br /> Instruction préparée<br /><br /> Requête ad hoc ( [!INCLUDE[tsql](../../includes/tsql-md.md)] soumise en tant qu’événements de langage à partir des utilitaires **sqlcmd** ou **osql** , au lieu d’appels de procédure distante)<br /><br /> ReplProc (procédure de réplication)<br /><br /> Déclencheur<br /><br /> Affichage<br /><br /> Default<br /><br /> Table utilisateur<br /><br /> Table système<br /><br /> Vérification<br /><br /> Règle|  
|**objid**|**int**|Une des clés principales servant à rechercher un objet dans le cache. Il s’agit de l’ID d’objet stocké dans **sysobjects** pour les objets de base de données (procédures, vues, déclencheurs, etc.). Pour les objets de cache tels que le SQL ad hoc ou préparé, **objID** est une valeur générée en interne.|  
|**dbid**|**smallint**|ID de la base de données dans laquelle a été compilé l'objet contenu dans le cache|  
|**dbidexec**|**smallint**|ID de la base de données à partir de laquelle la requête est exécutée.<br /><br /> Pour la plupart des objets, **dbidexec** a la même valeur que **dbid**.<br /><br /> Pour les vues système, **dbidexec** est l’ID de base de données à partir duquel la requête est exécutée.<br /><br /> Pour les requêtes ad hoc, **dbidexec** est égal à 0. Cela signifie que **dbidexec** a la même valeur que **dbid**.|  
|**codé**|**smallint**|Indique le créateur du plan pour les plans de requête ad hoc et les plans préparés.<br /><br /> –2 = le traitement soumis ne dépend pas de la résolution implicite des noms et peut être partagé entre différents utilisateurs. Ceci est la méthode privilégiée. Toute autre valeur représente l'ID de l'utilisateur soumettant la requête dans la base de données.<br /><br /> Déborde ou retourne la valeur NULL si le nombre d'utilisateurs et de rôles dépasse 32 767.|  
|**refcounts**|**int**|Nombre d'autres objets dans le cache faisant référence à cet objet. Un nombre de 1 est la base.|  
|**usecounts**|**int**|Nombre d'utilisations de l'objet dans le cache depuis le début|  
|**pagesused**|**int**|Nombre de pages consommées par l'objet dans le cache.|  
|**setopts**|**int**|Valeurs de l'option SET qui affectent un plan compilé. Elles font partie de la clé du cache. Des modifications de cette colonne indiquent que des utilisateurs ont modifié les options SET. Il s'agit des options suivantes :<br /><br /> **ANSI_PADDING**<br /><br /> **FORCEPLAN**<br /><br /> **CONCAT_NULL_YIELDS_NULL**<br /><br /> **ANSI_WARNINGS**<br /><br /> **ANSI_NULLS**<br /><br /> **QUOTED_IDENTIFIER**<br /><br /> **ANSI_NULL_DFLT_ON**<br /><br /> **ANSI_NULL_DFLT_OFF**|  
|**langid**|**smallint**|ID de langue. ID de la langue de la connexion qui a créé l'objet dans le cache.|  
|**DateFormat**|**smallint**|Format de date de la connexion qui a créé l'objet dans le cache|  
|**statut**|**int**|Indique si l'objet dans le cache est un plan de curseur ou non. Seul le bit de poids faible est actuellement utilisé.|  
|**lasttime**|**bigint**|Pour compatibilité descendante uniquement. Retourne toujours 0.|  
|**maxexectime**|**bigint**|Pour compatibilité descendante uniquement. Retourne toujours 0.|  
|**avgexectime**|**bigint**|Pour compatibilité descendante uniquement. Retourne toujours 0.|  
|**lastreads**|**bigint**|Pour compatibilité descendante uniquement. Retourne toujours 0.|  
|**lastwrites**|**bigint**|Pour compatibilité descendante uniquement. Retourne toujours 0.|  
|**sqlbytes**|**int**|Longueur en octets de la définition de procédure ou du traitement soumis.|  
|**sql**|**nvarchar(3900)**|Définition du module ou les 3 900 premiers caractères du traitement soumis.|  
  
## <a name="see-also"></a>Voir aussi  
 [Affichages de compatibilité &#40;Transact-SQL&#41;](~/relational-databases/system-compatibility-views/system-compatibility-views-transact-sql.md)  
  
  

