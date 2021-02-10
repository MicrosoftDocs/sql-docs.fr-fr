---
description: Enregistrements et champs fournis par le fournisseur
title: Enregistrements et champs de Provider-Supplied | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- records-provided fields [ADO]
- provider-supplied fields [ADO]
ms.assetid: 77f95e0a-0cf2-411a-a792-593f77330fbd
author: rothja
ms.author: jroth
ms.openlocfilehash: a64168a8a1fec81d47c337978ea75664fec0a10c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037109"
---
# <a name="records-and-provider-supplied-fields"></a>Enregistrements et champs fournis par le fournisseur
Lorsqu’un objet [Record](../../../ado/reference/ado-api/record-object-ado.md) est ouvert, sa source peut être la ligne actuelle d’un [Recordset](../../../ado/reference/ado-api/recordset-object-ado.md)ouvert, une URL absolue ou une URL relative avec un objet de [connexion](../../../ado/reference/ado-api/connection-object-ado.md) ouvert.  
  
 Si l' **enregistrement** est ouvert à partir d’un **jeu d’enregistrements**, la collection [Fields](../../../ado/reference/ado-api/fields-collection-ado.md) de l’objet **Record** contient tous les champs du **Recordset**, ainsi que tous les champs ajoutés par le fournisseur sous-jacent.  
  
 Le fournisseur peut insérer des champs supplémentaires qui servent de caractéristiques supplémentaires de l' **enregistrement**. Par conséquent, un **enregistrement** peut avoir des champs uniques qui ne se trouvent pas dans le **Recordset** dans son ensemble ou dans un **enregistrement** dérivé d’une autre ligne de l’ensemble d' **enregistrements**.  
  
 Par exemple, toutes les lignes d’un **jeu d’enregistrements** dérivé d’une source de données de messagerie électronique peuvent avoir des colonnes telles que from, to et subject. Un **enregistrement** dérivé de ce **Recordset** aura les mêmes champs. Toutefois, l' **enregistrement** peut également avoir d’autres champs propres au message spécifique représenté par cet **enregistrement**, par exemple, pièce jointe et CC (copie carbone).  
  
 Bien que l’objet **Record** et la ligne actuelle du **Recordset** aient les mêmes champs, ils sont différents, car les objets **Record** et **Recordset** ont des méthodes et des propriétés différentes.  
  
 Un champ qui est commun à l' **enregistrement** et au **Recordset** peut être modifié sur l’un ou l’autre des objets. Toutefois, le champ ne peut pas être supprimé de l’objet **Record** , même si le fournisseur sous-jacent peut prendre en charge l’affectation de la valeur null au champ.  
  
 Une fois l' **enregistrement** ouvert, vous pouvez ajouter des champs par programmation. Vous pouvez également supprimer des champs que vous avez ajoutés, mais vous ne pouvez pas supprimer des champs de l' **objet Recordset** d’origine.  
  
 Vous pouvez également ouvrir l’objet **Record** directement à partir d’une URL. Dans ce cas, les champs ajoutés à l' **enregistrement** dépendent du fournisseur sous-jacent. Actuellement, la plupart des fournisseurs ajoutent un ensemble de champs qui décrivent l’entité représentée par l' **enregistrement**. Si l’entité se compose d’un flux d’octets, tel qu’un fichier simple, un objet de [flux](../../../ado/reference/ado-api/stream-object-ado.md) peut généralement être ouvert à partir de l' **enregistrement**.  
  
## <a name="special-fields-for-document-source-providers"></a>Champs spéciaux pour les fournisseurs de source de document  
 Une classe spéciale de fournisseurs, appelée *fournisseurs de source de document*, gère les dossiers et les documents. Lorsqu’un objet **Record** représente un document ou un objet **Recordset** représente un dossier de documents, le fournisseur de source du document remplit ces objets avec un ensemble unique de champs qui décrivent les caractéristiques du document plutôt que le document lui-même. En règle générale, un champ contient une référence au **flux** qui représente le document.  
  
 Ces champs constituent un **enregistrement** de ressource ou un **jeu d’enregistrements** et sont répertoriés pour les fournisseurs spécifiques qui les prennent en charge dans [annexe a : fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md).  
  
 Deux constantes indexent la collection de **champs** d’un **enregistrement** de ressource ou d’un **jeu d’enregistrements** pour récupérer une paire de champs couramment utilisés. La propriété [valeur](../../../ado/reference/ado-api/value-property-ado.md) de l’objet de **champ** retourne le contenu souhaité.  
  
-   Le champ accédé avec la constante **adDefaultStream** contient un flux par défaut associé à l' **enregistrement** ou à l’objet **Recordset** . Le fournisseur assigne un flux par défaut à un objet.  
  
-   Le champ accessible avec la constante **adRecordURL** contient l’URL absolue qui identifie le document.  
  
 Un fournisseur de source de document ne prend pas en charge la collection [Properties](../../../ado/reference/ado-api/properties-collection-ado.md) d’objets **Record** et **Field** . Le contenu de la collection **Properties** a la valeur null pour ces objets.  
  
 Un fournisseur de source de document peut ajouter une propriété spécifique au fournisseur telle que le **type DataSource** pour déterminer s’il s’agit d’un fournisseur de source de document. Pour plus d’informations sur la façon de déterminer votre type de fournisseur, consultez la documentation de votre fournisseur.  
  
## <a name="resource-recordset-columns"></a>Colonnes de l’ensemble d’enregistrements de ressources  
 Un *jeu d’enregistrements de ressources* se compose des colonnes suivantes.  
  
|Nom de la colonne|Type|Description|  
|-----------------|----------|-----------------|  
|RESOURCE_PARSENAME|AdVarWChar|Lecture seule. Indique l’URL de la ressource.|  
|RESOURCE_PARENTNAME|AdVarWChar|Lecture seule. Indique l’URL absolue de l’enregistrement parent.|  
|RESOURCE_ABSOLUTEPARSENAME|AdVarWChar|Lecture seule. Indique l’URL absolue de la ressource, qui est la concaténation de PARENTNAME et PARSENAME.|  
|RESOURCE_ISHIDDEN|AdBoolean|True si la ressource est masquée. Aucune ligne n’est retournée, sauf si la commande qui crée l’ensemble de lignes sélectionne explicitement les lignes où RESOURCE_ISHIDDEN a la valeur true.|  
|RESOURCE_ISREADONLY|AdBoolean|True si la ressource est en lecture seule. Tente d’ouvrir cette ressource avec DBBINDFLAG_WRITE et échoue avec DB_E_READONLY. Cette propriété peut être modifiée même lorsque la ressource n’a été ouverte que pour la lecture.|  
|RESOURCE_CONTENTTYPE|AdVarWChar|Indique l’utilisation probable du document (par exemple, un résumé d’avocat). Cela peut correspondre au modèle Office utilisé pour créer le document.|  
|RESOURCE_CONTENTCLASS|AdVarWChar|Indique le type MIME du document, indiquant le format tel que « `text/html` ».|  
|RESOURCE_CONTENTLANGUAGE|AdVarWChar|Indique la langue dans laquelle le contenu est stocké.|  
|RESOURCE_CREATIONTIME|adFileTime|Lecture seule. Indique une structure FILETIME qui contient l’heure à laquelle la ressource a été créée. L’heure est indiquée au format de temps universel coordonné (UTC, Coordinated Universal Time).|  
|RESOURCE_LASTACCESSTIME|AdFileTime|Lecture seule. Indique une structure FILETIME qui contient l’heure du dernier accès à la ressource. L’heure est au format UTC. Les membres FILETIME sont nuls si le fournisseur ne prend pas en charge ce membre de temps.|  
|RESOURCE_LASTWRITETIME|AdFileTime|Lecture seule. Indique une structure FILETIME qui contient l’heure à laquelle la ressource a été écrite pour la dernière fois. L’heure est au format UTC. Les membres FILETIME sont nuls si le fournisseur ne prend pas en charge ce membre de temps.|  
|RESOURCE_STREAMSIZE|asUnsignedBigInt|Lecture seule. Indique la taille du flux par défaut de la ressource, en octets.|  
|RESOURCE_ISCOLLECTION|AdBoolean|Lecture seule. True si la ressource est une collection, telle qu’un répertoire. False si la ressource est un fichier simple.|  
|RESOURCE_ISSTRUCTUREDDOCUMENT|AdBoolean|True si la ressource est un document structuré. False si la ressource n’est pas un document structuré. Il peut s’agir d’une collection ou d’un fichier simple.|  
|DEFAULT_DOCUMENT|AdVarWChar|Lecture seule. Indique que cette ressource contient une URL vers le document simple par défaut d’un dossier ou d’un document structuré. Utilisé lorsque le flux par défaut est demandé à partir d’une ressource. Cette propriété est vide pour un fichier simple.|  
|CHAPTERED_CHILDREN|AdChapter|Lecture seule. facultatif. Indique le chapitre de l’ensemble de lignes qui contient les enfants de la ressource. (Le *fournisseur de OLE DB pour la publication Internet* n’utilise pas cette colonne.)|  
|RESOURCE_DISPLAYNAME|AdVarWChar|Lecture seule. Indique le nom complet de la ressource.|  
|RESOURCE_ISROOT|AdBoolean|Lecture seule. True si la ressource est la racine d’une collection ou d’un document structuré.|  
  
## <a name="see-also"></a>Voir aussi  
 [Record, objet (ADO)](../../../ado/reference/ado-api/record-object-ado.md)   
 [Annexe A : Fournisseurs](../../../ado/guide/appendixes/appendix-a-providers.md)
