---
description: Objets et interfaces ADO
title: Objets et interfaces ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- ADO, objects and interfaces
- objects [ADO]
ms.assetid: d0b7e254-c89f-4406-b846-a060ef038c30
author: rothja
ms.author: jroth
ms.openlocfilehash: c694a5051729a6ce9b925014ae93b5dbde11f5d9
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99164898"
---
# <a name="ado-objects-and-interfaces"></a>Objets et interfaces ADO
Les relations entre ces objets sont représentées dans le [modèle objet ADO](./ado-object-model.md).  
  
 Chaque objet peut être contenu dans sa collection correspondante. Par exemple, un objet d' [erreur](./error-object.md) peut être contenu dans une collection d' [Erreurs](./errors-collection-ado.md) . Pour plus d’informations, consultez [Collections ADO](./ado-collections.md) ou une rubrique de collection spécifique.  
  
|Objet ou interface|Description|  
|-|-|  
|[IADOCommandConstruction](/previous-versions/windows/desktop/aa965677(v=vs.85))|Utilisé pour récupérer la commande OLEDB sous-jacente à partir d’un objet ADOCommand.|  
|[ADORecordConstruction](./adorecordconstruction-interface.md)|Construit un objet **enregistrement** ADO à partir d’un objet OLE DB **Row** dans une application C/C++.|  
|[ADORecordsetConstruction](./adorecordsetconstruction-interface.md)|Construit un objet **Recordset** ADO à partir d’un objet d' **ensemble de lignes** OLE DB dans une application C/C++.|  
|[ADOStreamConstruction, interface](./adostreamconstruction-interface.md)|Construit un objet de **flux** ADO à partir d’un objet OLE DB **IStream** dans une application C/C++.|  
|[Commande](./command-object-ado.md)|Définit une commande spécifique que vous avez l’intention d’exécuter sur une source de données.<br /><br /> L’objet **Command** n’est pas sûr pour l’écriture de scripts.|  
|[Connection](./connection-object-ado.md)|Représente une connexion ouverte à une source de données.<br /><br /> L’objet de **connexion** est sécurisé pour l’écriture de scripts.|  
|[IDSOShapeExtensions, interface](./idsoshapeextensions-interface.md)|Obtient l’objet source de données OLEDB sous-jacent pour le fournisseur de formes.|  
|[Error](./error-object.md)|Contient des détails sur les erreurs d’accès aux données qui se rapportent à une seule opération impliquant le fournisseur.<br /><br /> L’objet d' **erreur** n’est pas sûr pour l’écriture de scripts.|  
|[Champ](./field-object.md)|Représente une colonne de données avec un type de données commun.|  
|[Paramètre](./parameter-object.md)|Représente un paramètre ou un argument associé à un objet de **commande** basé sur une requête paramétrable ou une procédure stockée.<br /><br /> L’objet de **paramètre** n’est pas sûr pour l’écriture de scripts.|  
|[Propriété](./property-object-ado.md)|Représente une caractéristique dynamique d’un objet ADO qui est défini par le fournisseur.|  
|[Enregistrement](./record-object-ado.md)|Représente une ligne d’un **jeu d’enregistrements**, ou un répertoire ou un fichier dans un système de fichiers. L’objet **Record** est sécurisé pour l’écriture de scripts.|  
|[Recordset](./recordset-object-ado.md)|Représente le jeu d’enregistrements d’une table de base ou les résultats d’une commande exécutée. À tout moment, l’objet **Recordset** fait référence à un seul enregistrement dans le jeu en tant qu’enregistrement actif.<br /><br /> L’objet **Recordset** est sécurisé pour l’écriture de scripts.|  
|[STREAM](./stream-object-ado.md)|Représente un flux de données binaire.<br /><br /> L’objet de **flux** est sécurisé pour l’écriture de scripts.|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ADO](./ado-api-reference.md)   
 [Collections ADO](./ado-collections.md)   
 [Propriétés dynamiques ADO](./ado-dynamic-properties.md)   
 [Constantes énumérées ADO](./ado-enumerated-constants.md)   
 [Annexe B : erreurs ADO](../../guide/appendixes/appendix-b-ado-errors.md)   
 [Événements ADO](./ado-events.md)   
 [Méthodes ADO](./ado-methods.md)   
 [Modèle objet ADO](./ado-object-model.md)   
 [Propriétés ADO](./ado-properties.md)