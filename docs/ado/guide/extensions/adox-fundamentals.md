---
description: Concepts de base d’ADOX
title: Notions de base d’ADOX | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADOX, fundamentals
ms.assetid: 954476fc-5f72-4ada-ace5-d9acb27d18f8
author: rothja
ms.author: jroth
ms.openlocfilehash: 6b03c71180a3d32ca3a592c063d5a267f187964c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032171"
---
# <a name="adox-fundamentals"></a>Concepts de base d’ADOX
Les extensions Microsoft® ActiveX® Data Objects pour la sécurité et le langage de définition de données (ADOX) sont une extension du modèle de programmation et des objets ADO. ADOX comprend des objets pour la création et la modification d’un schéma, ainsi que pour la sécurité. Étant donné qu’il s’agit d’une approche basée sur les objets de la manipulation de schéma, vous pouvez écrire du code qui fonctionnera sur diverses sources de données, quelles que soient les différences dans leurs syntaxes natives.  
  
 ADOX est une bibliothèque associée aux objets ADO de base. Il expose des objets supplémentaires pour la création, la modification et la suppression d’objets de schéma, tels que des tables et des procédures. Il comprend également des objets de sécurité pour gérer les utilisateurs et les groupes, ainsi que pour accorder et révoquer des autorisations sur des objets.  
  
 Pour utiliser ADOX avec votre outil de développement, vous devez établir une référence à la bibliothèque de types ADOX. La description de la bibliothèque ADOX est « Microsoft ADO ext. for DDL and Security ». Le nom du fichier de bibliothèque ADOX est Msadox.dll, et l’ID de programme (ProgID) est « ADOX ». Pour plus d’informations sur la création de références à des bibliothèques, consultez la documentation de votre outil de développement.  
  
 Le fournisseur Microsoft OLE DB pour Microsoft Jet Moteur de base de données prend entièrement en charge ADOX. Certaines fonctionnalités d’ADOX peuvent ne pas être prises en charge, en fonction de votre fournisseur de données.  
  
 Ce document suppose une connaissance du langage de programmation Microsoft® Visual Basic® et une connaissance générale d’ADO. Pour plus d’informations sur ADO, consultez le [Guide du programmeur ADO](../ado-programmer-s-guide.md). Pour plus d’informations générales sur ADOX, consultez les rubriques suivantes :  
  
-   [Modèle objet ADOX](../../reference/adox-api/adox-object-model.md)  
  
-   [Objets ADOX](../../reference/adox-api/adox-objects.md)  
  
-   [Collections ADOX](../../reference/adox-api/adox-collections.md)  
  
-   [Propriétés ADOX](../../reference/adox-api/adox-properties.md)  
  
-   [Méthodes ADOX](../../reference/adox-api/adox-methods.md)  
  
-   [Exemples ADOX](../../reference/adox-api/adox-code-examples.md)  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADOX](../../reference/adox-api/adox-object-model.md)   
 [Exemples de code ADOX](../../reference/adox-api/adox-code-examples.md)   
 [Collections ADOX](../../reference/adox-api/adox-collections.md)   
 [Constantes énumérées ADOX](../../reference/adox-api/adox-enumerated-constants.md)   
 [Méthodes ADOX](../../reference/adox-api/adox-methods.md)   
 [Modèle objet ADOX](../../reference/adox-api/adox-object-model.md)   
 [Objets ADOX](../../reference/adox-api/adox-objects.md)   
 [Propriétés ADOX](../../reference/adox-api/adox-properties.md)   
 [ADO (multidimensionnel) (ADO MD)](../multidimensional/ado-multidimensional-ado-md.md)   
 [Guide du programmeur ADO](../ado-programmer-s-guide.md)
