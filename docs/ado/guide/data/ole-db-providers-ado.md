---
description: Fournisseurs OLE DB (ADO)
title: Fournisseurs de OLE DB (ADO) | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 6e0488c3-934d-4976-99dc-65c580dc7a3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 7e70762a2cbaa6e2dd3d8d91b824591799605748
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032661"
---
# <a name="ole-db-providers-ado"></a>Fournisseurs OLE DB (ADO)
OLE DB définit un ensemble d’interfaces COM pour fournir aux applications un accès uniforme aux données stockées dans diverses sources d’informations. Cette approche permet à une source de données de partager ses données via les interfaces qui prennent en charge la quantité de fonctionnalités SGBD appropriées à la source de données. Par défaut, l’architecture hautes performances de OLE DB est basée sur l’utilisation d’un modèle de services flexible basé sur les composants. Au lieu d’avoir un nombre prescrit de couches intermédiaires entre l’application et les données, OLE DB ne nécessite que le nombre de composants nécessaires pour accomplir une tâche particulière.  
  
 Supposons, par exemple, qu’un utilisateur souhaite exécuter une requête. Examinez les scénarios suivants :  
  
-   Les données se trouvent dans une base de données relationnelle pour laquelle il existe actuellement un pilote ODBC mais aucun fournisseur de OLE DB natif : l’application utilise ADO pour communiquer avec le fournisseur OLE DB pour ODBC, qui charge ensuite le pilote ODBC approprié. Le pilote passe l’instruction SQL au SGBD, qui récupère les données.  
  
-   Les données se trouvent dans Microsoft SQL Server pour lesquelles il existe un fournisseur de OLE DB natif : l’application utilise ADO pour communiquer directement avec le fournisseur OLE DB pour Microsoft SQL Server. Aucun intermédiaire n’est requis.  
  
-   Les données résident dans Microsoft Exchange Server, pour lequel il existe un fournisseur de OLE DB mais qui n’expose pas de moteur pour traiter les requêtes SQL : l’application utilise ADO pour communiquer avec le fournisseur OLE DB pour Microsoft Exchange et appelle un composant de processeur de requêtes OLE DB pour gérer l’interrogation.  
  
-   Les données résident dans le système de fichiers Microsoft NTFS sous la forme de documents : les données sont accessibles à l’aide d’un fournisseur de OLE DB natif sur le service d’indexation Microsoft, qui indexe le contenu et les propriétés des documents dans le système de fichiers pour permettre des recherches de contenu efficaces.  
  
 Dans tous les exemples précédents, l’application peut interroger les données. Les besoins de l’utilisateur sont satisfaits avec un nombre minimal de composants. Dans chaque cas, les composants supplémentaires sont utilisés uniquement si nécessaire, et seuls les composants requis sont appelés. Ce chargement à la demande des composants réutilisables et partageables contribue fortement aux performances élevées lorsque OLE DB est utilisé.  
  
 Les fournisseurs se répartissent en deux catégories : celles fournissant des données et celles fournissant des services. Un fournisseur de données possède ses propres données et l’expose sous forme tabulaire à votre application. Un fournisseur de services encapsule un service en générant et en consommant des données, en augmentant les fonctionnalités de vos applications ADO. Un fournisseur de services peut également être défini comme un composant de service, qui doit fonctionner conjointement avec d’autres fournisseurs de services ou composants.  
  
 ADO offre une interface cohérente et de niveau supérieur aux différents fournisseurs de OLE DB.  
  
 Cette section contient les rubriques suivantes :  
  
-   [Fournisseurs de données](./data-providers.md)  
  
-   [Fournisseurs et composants de services](./service-providers-and-components.md)