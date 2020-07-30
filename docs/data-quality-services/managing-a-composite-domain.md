---
title: Gestion d'un domaine composite
ms.date: 07/31/2012
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
ms.assetid: 47821eff-800b-4053-8d36-e42bbc267f54
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 2d463b65247dc5606605f1760ef6a259b5f534e9
ms.sourcegitcommit: 99f61724de5edf6640efd99916d464172eb23f92
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87363053"
---
# <a name="managing-a-composite-domain"></a>Gestion d'un domaine composite

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique décrit l'utilisation des domaines composites dans [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS). Parfois un seul domaine ne représente pas les données d'un domaine de manière satisfaisante, et vous pouvez représenter les données uniquement en regroupant les domaines. Pour cela, vous créez un domaine composite. Un domaine composite comprend deux ou plusieurs domaines individuels et est mappé à un champ de données composé de plusieurs termes connexes qui ne sont pas analysés, mais sont inclus dans une même valeur composite. Chaque terme de la valeur est représenté par un seul domaine différent. Une fois que vous avez inclus les domaines individuels dans les domaines composites, et que vous avez ensuite mappé le domaine composite au champ de données, vous pouvez générer la connaissance dans la base de connaissances sur les données de ce champ lors de la génération de la connaissance dans les domaines individuels. Un domaine composite, comme un seul domaine, est une représentation sémantique des données d'un seul champ de données.  
  
 Les domaines uniques d'un domaine composite doivent avoir une zone commune de connaissance. Un exemple est une zone adresse qui a une rue, une ville, un état, un pays et un code postal. Les différents termes de ce champ peuvent avoir différents types de données. Pour gérer cela, vous mappez ces termes aux différents domaines individuels. Un autre exemple est un champ de nom complet qui contient le prénom, le deuxième prénom et le nom. Pour utiliser un domaine composite, vous devez pouvoir analyser les données du champ dans différents domaines individuels, en créant un domaine composite pour le champ et un domaine individuel pour une partie du champ.  
  
 Les domaines composites ont des fonctions différentes des domaines individuels. Vous ne pouvez pas modifier les valeurs du domaine composite. Vous ne devez le faire que dans un domaine simple. Avec les domaines composites, vous pouvez utiliser les règles inter-domaines pour tester les valeurs des domaines simples du domaine composite. Vous pouvez également afficher les combinaisons de valeurs trouvées dans les domaines composites.  
  
## <a name="in-this-section"></a>Dans cette section  
 L'utilisation d'un domaine composite permet d'effectuer les opérations suivantes :  
  
|Description de l’opération|Rubrique|  
|-|-|  
|Créer une représentation sémantique d'un champ de données composé de plusieurs termes connexes qui ne sont pas analysés|[Créer un domaine composite](../data-quality-services/create-a-composite-domain.md)|  
|Lorsque vous mappez les données complexes à un domaine composite, vous pouvez analyser les données selon la connaissance, en plus de l'analyse sur un séparateur. DQS essaie d'abord d'utiliser sa connaissance sur les domaines simples pour déterminer comment les parties de la chaîne complexe appartiennent aux domaines simples.|[Créer un domaine composite](../data-quality-services/create-a-composite-domain.md)|  
|Attachez un service de données de référence, tel que celui gérant les adresses, à un domaine composite.|[Attacher un domaine ou un domaine composite à des données de référence](../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md)|  
|Créez une règle inter-domaines lorsque la valeur d'un domaine d'un domaine composite affecte la valeur des autres.|[Créer une règle inter-domaines](../data-quality-services/create-a-cross-domain-rule.md)|  
|Identifiez les combinaisons de valeur afin que DQS puisse rapporter leur fréquence.|[Utiliser les relations de valeur dans un domaine composite](../data-quality-services/use-value-relations-in-a-composite-domain.md)|  
  
## <a name="related-tasks"></a>Tâches associées  
  
|Description de la tâche|Rubrique|  
|----------------------|-----------|  
|Construction d'une base de connaissances par la découverte de la connaissance et la gestion interactive de la connaissance|[Construction d'une base de connaissances](../data-quality-services/building-a-knowledge-base.md)|  
|Importation ou exportation de la connaissance depuis ou vers une base de connaissances.|[Importation et exportation de connaissances](../data-quality-services/importing-and-exporting-knowledge.md)|  
|Création d'un seul domaine et ajout de connaissances au domaine.|[Gestion d'un domaine](../data-quality-services/managing-a-domain.md)|  
  
  
