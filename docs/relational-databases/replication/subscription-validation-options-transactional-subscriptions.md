---
title: Options de validation d’abonnement, boîte de dialogue (Transactionnel)
description: Décrit la boîte de dialogue « Options de validation d’abonnement » pour la réplication transactionnelle dans SQL Server Management Studio (SSMS).
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
f1_keywords:
- sql13.rep.validate.options.f1
helpviewer_keywords:
- Subscription Validation Options dialog box
ms.assetid: fd66ad1f-df01-4240-9e89-8f41bff12c1e
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016
ms.openlocfilehash: 963c9873147dd81171a776f1cc9d2d6fd33a2864
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468730"
---
# <a name="subscription-validation-options-transactional-subscriptions"></a>Options de validation d'abonnement (abonnements transactionnels)
[!INCLUDE[sql-asdb](../../includes/applies-to-version/sql-asdb.md)]
  Utilisez la boîte de dialogue **Options de validation d'abonnement** pour spécifier si la validation doit utiliser uniquement un nombre de lignes ou un nombre de lignes et une somme de contrôle binaire. 

[!INCLUDE[azure-sql-db-replication-supportability-note](../../includes/azure-sql-db-replication-supportability-note.md)]
  
## <a name="options"></a>Options  
 **Vérifiez si l'abonné possède le même nombre de lignes de données répliquées que le serveur de publication**  
 Sélectionnez le type de validation du nombre de lignes à effectuer. Pour les publications Oracle, la seule option proposée est **Calculer le nombre réel de lignes en interrogeant directement les tables**.  
  
 **Comparer les sommes de contrôle pour vérifier les données de ligne**  
 Outre le comptage des lignes sur le serveur de publication et sur l'Abonné, une somme de contrôle de toutes les données est calculée à l'aide de l'algorithme de somme de contrôle binaire. Si le nombre de lignes est erroné, la somme de contrôle n'est pas effectuée.  
  
 **Arrêter l'Agent de distribution une fois la validation terminée**  
 Par défaut, l'Agent de distribution s'exécute en permanence. Sélectionnez cette option pour arrêter l'agent lorsque la validation est terminée. Cela permet de vérifier que la validation a réussi avant de continuer à répliquer des données vers l'Abonné.  
  
## <a name="see-also"></a>Voir aussi  
 [Valider des données sur l’abonné](../../relational-databases/replication/validate-data-at-the-subscriber.md)   
 [Valider des données répliquées](../../relational-databases/replication/validate-data-at-the-subscriber.md)  
  
  
