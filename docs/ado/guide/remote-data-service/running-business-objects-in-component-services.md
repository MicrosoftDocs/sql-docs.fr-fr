---
description: Exécution d’objets métier dans les services de composants
title: Exécution d’objets métier dans les services de composants | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- component services in RDS [ADO]
ms.assetid: 3077d0b6-42d6-4f10-8e5d-42e6204f1109
author: rothja
ms.author: jroth
ms.openlocfilehash: 7129254d609e1bd80853ccf5008527bfbf938c30
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036379"
---
# <a name="running-business-objects-in-component-services"></a>Exécution d’objets métier dans les services de composants
> [!IMPORTANT]
>  À compter de Windows 8 et de Windows Server 2012, les composants serveur RDS ne sont plus inclus dans le système d’exploitation Windows (pour plus d’informations, consultez le livre de recettes sur la compatibilité avec Windows 8 et [Windows server 2012](https://www.microsoft.com/download/details.aspx?id=27416) ). Les composants clients RDS seront supprimés dans une prochaine version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Les applications qui utilisent RDS doivent migrer vers le [service de données WCF](/dotnet/framework/wcf/).  
  
 Les objets métier peuvent être des fichiers exécutables (. exe) ou des bibliothèques de liens dynamiques (. dll). La configuration que vous utilisez pour exécuter l’objet métier varie selon que l’objet est un fichier. dll ou. exe :  
  
-   Les objets métier créés en tant que fichiers. exe peuvent être appelés via DCOM. Si ces objets métier sont utilisés par le biais de Internet Information Services (IIS), ils sont soumis à un marshaling de données supplémentaire, ce qui ralentit les performances du client.  
  
-   Les objets métier créés en tant que fichiers. dll peuvent être utilisés par le biais d’IIS, et par conséquent également par HTTP. Ils peuvent également être utilisés sur DCOM uniquement par le biais des services de composants ou via Microsoft Transaction Server, si vous utilisez Windows NT. Les dll de l’objet métier devront être inscrites sur l’ordinateur serveur IIS pour y accéder via IIS. Pour plus d’informations sur la configuration d’une DLL à exécuter sur DCOM, consultez la section [activation d’une dll à exécuter sur DCOM](./enabling-a-dll-to-run-on-dcom.md).  
  
> [!NOTE]
>  Lorsque des objets métier sur le niveau intermédiaire sont implémentés en tant que composants de services de composants à l’aide de **GetObjectContext**, **SetComplete** et **SetAbort**, les objets métier peuvent utiliser les services de composants (ou MTS, si vous utilisez Windows NT) pour maintenir leur état entre plusieurs appels clients. Ce scénario est possible avec DCOM, qui est généralement implémenté entre les clients et les serveurs approuvés dans un intranet. Dans ce cas, le [RDS.](../../reference/rds-api/dataspace-object-rds.md) L’objet DataSpace et la méthode [CreateObject](../../reference/rds-api/createobject-method-rds.md) côté client sont remplacés par l’objet de contexte de transaction et la méthode **CreateInstance** , qui sont fournis par l’interface **ITransactionContext** et implémentés par les services de composants.  
  
## <a name="see-also"></a>Voir aussi  
 [Concepts de base de RDS](./rds-fundamentals.md)