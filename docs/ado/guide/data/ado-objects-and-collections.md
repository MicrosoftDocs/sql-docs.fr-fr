---
description: Objets et collections ADO
title: Objets et collections ADO | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- ADO, objects and collections
ms.assetid: 7a745aae-9372-49b6-8dae-b9c93e5f3216
author: rothja
ms.author: jroth
ms.openlocfilehash: c03e1b792e23552252eb84fae9a71e693b3781c8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028064"
---
# <a name="ado-objects-and-collections"></a>Objets et collections ADO
ADO est constitué des neuf objets et quatre collections suivants.  
  
|Objet ou collection|Description|  
|--------------------------|-----------------|  
|Objet **Connection**|Représente une session unique avec une source de données. Dans le cas d’un système de base de données client/serveur, il peut être équivalent à une connexion réseau réelle au serveur. Selon les fonctionnalités prises en charge par le fournisseur, certaines collections, méthodes ou propriétés d’un objet de **connexion** peuvent ne pas être disponibles.|  
|Objet **Command**|Utilisé pour définir une commande spécifique, telle qu’une requête SQL, destinée à s’exécuter sur une source de données.|  
|**Recordset** , objet|Représente l’ensemble des enregistrements d’une table de base ou les résultats d’une commande exécutée. Tous les objets **Recordset** se composent d’enregistrements (lignes) et de champs (colonnes).|  
|Objet **Record**|Représente une ligne de données unique à partir d’un **jeu d’enregistrements** ou du fournisseur. Cet enregistrement peut représenter un enregistrement de base de données ou un autre type d’objet, tel qu’un fichier ou un répertoire, en fonction de votre fournisseur.|  
|Objet de **flux**|Représente un flux de données binaires ou de texte. Par exemple, un document XML peut être chargé dans un flux pour une entrée de commande ou retournée à partir de certains fournisseurs en tant que résultats d’une requête. Un objet de **flux** peut être utilisé pour manipuler des champs ou des enregistrements contenant ces flux de données.|  
|**Parameter** (objet)|Représente un paramètre ou un argument associé à un objet de **commande** , en fonction d’une requête paramétrable ou d’une procédure stockée.|  
|**Field** , objet|Représente une colonne de données avec un type de données commun. Chaque objet **Field** correspond à une colonne dans le **Recordset**.|  
|Objet **Property**|Représente une caractéristique d’un objet ADO défini par le fournisseur. Les objets ADO ont deux types de propriétés : intégré et dynamique. Les propriétés intégrées sont les propriétés implémentées dans ADO et immédiatement disponibles pour tout nouvel objet. L’objet **Property** est un conteneur de propriétés dynamiques, défini par le fournisseur sous-jacent.|  
|Objet **Error**|Contient des détails sur les erreurs d’accès aux données qui se rapportent à une seule opération impliquant le fournisseur.|  
|**Fields** , collection|Contient tous les objets **Field** d’un objet **Recordset** ou **Record** .|  
|**Propriétés** , collection|Contient tous les objets de **propriété** pour une instance spécifique d’un objet.|  
|Collection **Parameters**|Contient tous les objets **Parameter** d’un objet **Command** .|  
|Collection d' **Erreurs**|Contient tous les objets d' **erreur** créés en réponse à un échec lié à un fournisseur unique.|  
  
## <a name="see-also"></a>Voir aussi  
 [Modèle objet ADO](../../reference/ado-api/ado-object-model.md)