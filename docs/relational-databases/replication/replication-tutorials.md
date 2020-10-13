---
title: Tutoriels sur la réplication | Microsoft Docs
description: Utilisez ces tutoriels qui vous aident à préparer votre serveur pour la réplication dans SQL Server, puis vous apprennent à configurer la réplication transactionnelle et la réplication de fusion.
ms.custom: ''
ms.date: 04/09/2018
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- tutorials [SQL Server replication]
- walkthroughs [SQL Server replication]
- replication [SQL Server], tutorials
ms.assetid: 19fbd10e-5b59-4cd0-a988-52d5d9206242
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||=sqlallproducts-allversions
ms.openlocfilehash: ab482e4818d7d60b9876fbf300a39de0b2f9ee40
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868385"
---
# <a name="replication-tutorials"></a>Tutoriels sur la réplication
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
La réplication est une solution efficace pour transférer des données, ou des sous-ensembles de données, entre des serveurs. Vous pouvez répliquer des données entre des serveurs intégralement connectés à l’aide de la réplication transactionnelle. Vous pouvez également répliquer des données entre des serveurs et des clients qui sont connectés par intermittence à l’aide de la réplication de fusion. Dans cet article, vous trouverez des tutoriels qui vous aident à préparer votre serveur pour la réplication, puis vous expliquent comment configurer la réplication transactionnelle et la réplication de fusion. 
  
Dans les tutoriels sur la réplication, « serveur de publication » fait référence au serveur qui contient les données sources en cours de réplication. « Abonné » fait référence au serveur de destination. Le serveur de publication et l’abonné peuvent partager la même instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], mais ce n’est pas une obligation. Pour plus d’informations, consultez la [présentation du modèle de publication de réplication](../../relational-databases/replication/publish/replication-publishing-model-overview.md).  

Ces tutoriels utilisent NODE1\SQL2016 comme serveur de publication et serveur de distribution. Ils utilisent NODE2\SQL2016 comme abonné. 
  
> [!NOTE]  
> La plupart des tâches expliquées dans ces didacticiels peuvent être effectuées par programme. Pour plus d’informations, consultez le [Guide du développeur (réplication)](../../relational-databases/replication/concepts/replication-developer-documentation.md).  
  
## <a name="replication-tutorials"></a>Tutoriels sur la réplication  
[Tutoriel : préparer SQL Server pour la réplication (serveur de publication, serveur de distribution, abonné)](../../relational-databases/replication/tutorial-preparing-the-server-for-replication.md) 
 
Apprenez à préparer les serveurs afin que la réplication puisse s'exécuter avec les privilèges minimaux. Ce didacticiel doit être effectué avant les autres didacticiels sur la réplication.  
  
[Tutoriel : configurer la réplication entre deux serveurs intégralement connectés (réplication transactionnelle)](../../relational-databases/replication/tutorial-replicating-data-between-continuously-connected-servers.md)

Découvrez comment configurer la réplication transactionnelle pour répliquer des données entre serveurs intégralement connectés. Ce tutoriel inclut également une méthodologie de résolution des erreurs de base. 

  
[Tutoriel : configurer la réplication entre un serveur et des clients mobiles (fusion)](../../relational-databases/replication/tutorial-replicating-data-with-mobile-clients.md)

Découvrez comment configurer la réplication de fusion pour échanger des données entre un serveur et un ou plusieurs clients connectés uniquement de façon occasionnelle.  
  
## <a name="see-also"></a>Voir aussi  
[Afficher et modifier les paramètres de sécurité de la réplication](../../relational-databases/replication/security/view-and-modify-replication-security-settings.md) 

[Présentation de la réplication transactionnelle](./transactional/transactional-replication.md) 

[Présentation de la réplication de fusion](./merge/merge-replication.md)

