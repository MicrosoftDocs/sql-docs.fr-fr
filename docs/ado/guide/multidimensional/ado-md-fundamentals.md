---
description: Concepts de base d’ADO MD
title: Notions de base de ADO MD | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO MD, fundamentals
ms.assetid: f6a20d9f-c1ab-474c-b9f3-82277e2a126d
author: rothja
ms.author: jroth
ms.openlocfilehash: 7634d32b35eef7d0289ef778bee9646a050e8755
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032191"
---
# <a name="ado-md-fundamentals"></a>Concepts de base d’ADO MD
Microsoft® ActiveX® Data Objects (multidimensionnel) (ADO MD) permet d’accéder facilement aux données multidimensionnelles à partir de langages tels que Microsoft Visual Basic®, Microsoft Visual C++®. ADO MD étend Microsoft ActiveX® Data Objects (ADO) pour inclure des objets spécifiques aux données multidimensionnelles, tels que les objets [CubeDef](../../reference/ado-md-api/cubedef-object-ado-md.md) et [Cellset](../../reference/ado-md-api/cellset-object-ado-md.md) . Avec ADO MD vous pouvez parcourir le schéma multidimensionnel, interroger un cube et récupérer les résultats.  
  
 Comme ADO, ADO MD utilise un fournisseur de OLE DB sous-jacent pour accéder aux données. Pour utiliser ADO MD, le fournisseur doit être un fournisseur de données multidimensionnelles (MDP) comme défini par le OLE DB pour la spécification OLAP. Un MDP présente des données dans des vues multidimensionnelles au lieu de vues tabulaires, ce qui explique comment un fournisseur de données tabulaires (TDP) présente des données. Pour plus d’informations sur la syntaxe et le comportement spécifiques pris en charge par votre fournisseur, reportez-vous à la documentation de votre fournisseur de OLE DB OLAP.  
  
 Ce document suppose une connaissance du langage de programmation Visual Basic et une connaissance générale de ADO et OLAP. Pour plus d’informations, consultez le [Guide du programmeur ADO](../ado-programmer-s-guide.md) et [OLE DB pour le traitement analytique en ligne (OLAP)](/previous-versions/windows/desktop/ms717005(v=vs.85)).  
  
 Cette section contient les rubriques suivantes :  
  
-   [Vue d’ensemble des données et des schémas multidimensionnels](./overview-of-multidimensional-schemas-and-data.md)  
  
-   [Utilisation de données multidimensionnelles](./working-with-multidimensional-data.md)  
  
-   [Utilisation d’ADO avec ADO MD](./using-ado-with-ado-md.md)  
  
-   [Programmation avec ADO MD](./programming-with-ado-md.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO MD](../../reference/ado-md-api/ado-md-object-model.md)   
 [Guide du programmeur ADO](../ado-programmer-s-guide.md)   
 [Extensions ADO pour la sécurité et le langage de définition de données (ADOX)](../extensions/ado-extensions-for-data-definition-language-and-security-adox.md)   
 [Vue d’ensemble des schémas et des données multidimensionnels](./overview-of-multidimensional-schemas-and-data.md)   
 [Programmation avec ADO MD](./programming-with-ado-md.md)   
 [Utilisation d’ADO avec ADO MD](./using-ado-with-ado-md.md)   
 [Utilisation de données multidimensionnelles](./working-with-multidimensional-data.md)