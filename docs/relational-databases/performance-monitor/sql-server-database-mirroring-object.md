---
title: SQL Server, objet Database Mirroring | Microsoft Docs
description: Découvrez l’objet de performance SQLServer:Database Mirroring, qui contient des compteurs de performances faisant état d’informations sur la mise en miroir de bases de données SQL Server.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- SQLServer:Database Mirroring
- database mirroring [SQL Server], performance counters
- performance counters [SQL Server], database mirroring
- Database Mirroring object
ms.assetid: a27b51ee-7637-4525-9424-bcc16947dc13
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f88935ba5ef8cf7f42c844e2d9a02c21d68aee97
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505763"
---
# <a name="sql-server-database-mirroring-object"></a>SQL Server, objet Database Mirroring
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  L’objet de performance **SQLServer:Database Mirroring** contient des compteurs de performances qui font état d’informations sur la mise en miroir de bases de données [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le tableau ci-dessous répertorie les compteurs inclus dans cet objet.  
  
|Nom|Description|  
|----------|-----------------|  
|**Octets reçus/s**|Nombre d'octets reçus par seconde.|  
|**Octets envoyés/s**|Nombre d'octets envoyés par seconde.|  
|**Octets de journal reçus/s**|Nombre d'octets du journal reçus par seconde.|  
|**Octets du journal restaurés à partir du cache/s**|Nombre d'octets du journal restaurés qui ont été obtenus à partir du cache du journal de mise en miroir au cours de la dernière seconde.<br /><br /> Ce compteur est uniquement utilisé sur le serveur miroir. Sur le serveur principal, la valeur est toujours 0.|  
|**Octets du journal envoyés du cache/s**|Nombre d'octets du journal envoyés qui ont été obtenus à partir du cache du journal de mise en miroir au cours de la dernière seconde.<br /><br /> Ce compteur est uniquement utilisé sur le serveur principal. Sur le serveur miroir, la valeur est toujours 0.|  
|**Octets du journal envoyés/s**|Nombre d'octets du journal envoyés par seconde.|  
|**Octets compressés du journal reçus/s**|Nombre d'octets compressés du journal reçus au cours de la dernière seconde.|  
|**Octets compressés du journal envoyés/s**|Nombre d'octets compressés du journal envoyés au cours de la dernière seconde.|  
|**Temps de renforcement de journal (ms)**|Temps en millisecondes passé par les blocs du journal en attente de renforcement sur disque au cours de la dernière seconde.|  
|**Enregistrer les Ko restants pour l'annulation**|Nombre total de kilo-octets du journal qui restent à analyser par le nouveau serveur miroir après le basculement.<br /><br /> Ce compteur est uniquement utilisé sur le serveur miroir pendant la phase de restauration. Lorsque la phase de restauration est terminée, le compteur est réinitialisé à 0. Sur le serveur principal, la valeur est toujours 0.|  
|**Enregistrer les Ko analysés pour l'annulation**|Nombre total de kilo-octets de journal qui ont été analysés par le nouveau serveur miroir depuis le basculement.<br /><br /> Ce compteur est uniquement utilisé sur le serveur miroir pendant la phase de restauration. Lorsque la phase de restauration est terminée, le compteur est réinitialisé à 0. Sur le serveur principal, la valeur est toujours 0.|  
|**Temps de contrôle du flux d'envoi du journal (ms)**|Temps en millisecondes passé par les messages de flux du journal en attente de contrôle du flux d'envoi au cours de la dernière seconde.<br /><br /> L'envoi de données et de métadonnées du journal au partenaire de mise en miroir est l'opération qui nécessite le plus de données dans la mise en miroir de bases de données et peut monopoliser les mémoires tampons d'envoi de la mise en miroir de bases de données et de Service Broker. Utilisez ce compteur pour surveiller l'utilisation de cette mémoire tampon par la session de mise en miroir de bases de données.|  
|**Ko de la file d'attente d'envoi du journal**|Nombre total de kilo-octets du journal qui n'ont pas été envoyés au serveur miroir.|  
|**Transactions d'écriture en miroir/s**|Nombre de transactions qui ont écrit dans la base de données mise en miroir et ont attendu que le journal soit envoyé au miroir pour être validées au cours de la dernière seconde.<br /><br /> Ce compteur est incrémenté uniquement lorsque le serveur principal envoie activement des enregistrements du journal au serveur miroir.|  
|**Pages envoyées/s**|Nombre de pages envoyées par seconde.|  
|**Réceptions/s**|Nombre de réceptions de messages de mise en miroir par seconde.|  
|**Octets restaurés par progression/s**|Nombre d'octets du journal restaurés par progression par seconde sur la base de données miroir.|  
|**Ko de la file d'attente de restauration par progression**|Nombre total de kilo-octets du journal renforcé qui doivent encore être appliqués à la base de données miroir pour la restaurer par progression. Envoi à la base de données principale à partir de la base de données miroir.|  
|**Temps d'accusé de réception/envoi**|Temps en millisecondes pendant lequel les messages ont attendu l’accusé de réception du serveur partenaire au cours de la dernière seconde.<br /><br /> Ce compteur est utile pour résoudre un problème susceptible d'être provoqué par un goulot d'étranglement du réseau, tel que des basculements inexpliqués, une longue file d'attente d'envoi ou une latence élevée des transactions. Dans ce type de situation, vous pouvez analyser la valeur de ce compteur pour déterminer si le réseau est à l'origine du problème.|  
|**Envois/s**|Nombre d'envois de messages de mise en miroir par seconde.|  
|**Délai de transaction**|Délai d’attente d’accusé de réception non terminé.|  
  
> [!NOTE]  
>  Sur chaque partenaire, certains des compteurs affichent une valeur de zéro en fonction du rôle que le partenaire détient actuellement.  
  
## <a name="remarks"></a>Notes  
 Les compteurs de performances vous permettent d'analyser les performances de la mise en miroir de bases de données. Par exemple, vous pouvez examiner le compteur **Délai de transaction** pour savoir si la mise en miroir de bases de données a une incidence sur les performances du serveur principal, et vous pouvez examiner les compteurs **File d'attente de restauration par progression** et **File d'attente d'envoi du journal** pour savoir si la mise en miroir de la base de données est synchronisée avec la base de données principale. Vous pouvez examiner le compteur **Octets du journal envoyés/s** pour analyser la quantité de journal envoyée par seconde.  
  
## <a name="see-also"></a>Voir aussi  
 [Analyser l’utilisation des ressources &#40;Moniteur système&#41;](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
