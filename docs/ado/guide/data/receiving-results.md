---
description: Réception de résultats
title: Réception des résultats | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- receiving results [ADO]
- Recordset object [ADO], receiving results
ms.assetid: 791aa26e-7aae-477e-9f05-5cd46e1de095
author: rothja
ms.author: jroth
ms.openlocfilehash: 1cdcf6acb2eb9de033f86b9f34739a55dc0dc021
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032581"
---
# <a name="receiving-results"></a>Réception de résultats
Dans ADO, la plupart des commandes génèrent des informations retournées à l’appelant. Pour les commandes qui retournent l’ensemble de lignes, les résultats sont reçus dans un objet **Recordset** , qui est probablement le plus utilisé pour les objets ADO.  
  
 Il existe plusieurs façons de recevoir des données dans un objet **Recordset** à partir d’une source de données, notamment en appelant les éléments suivants :  
  
-   [ Méthode deConnection.Exe](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [ Méthode deCommand.Exe](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Recordset. Open, méthode](../../../ado/guide/data/creating-and-executing-a-simple-command.md)  
  
-   [Connexion. NamedCommand](../../../ado/guide/data/named-commands.md)  
  
-   [Connexion. StoredProcedure](../../../ado/guide/data/calling-a-stored-procedure-as-a-method-on-a-connection-object.md)  
  
 La réception de données dans un objet **Recordset** conclut le processus d’obtention des données, avec la participation d’un objet **Connection** et d’un objet **Command** , implicitement ou explicitement. Dans un système d’applications client/serveur classique, l’ensemble du processus d’obtention des données nécessite un aller-retour sur le réseau pour chaque **jeu d’enregistrements** rempli.  
  
 Pour recevoir plusieurs jeux de résultats, vous devez effectuer plusieurs allers-retours sur le réseau, un pour chaque jeu de données encapsulé dans un objet **Recordset** . Pour les réseaux lents ou encombrés, la réduction du nombre d’allers-retours peut contribuer à améliorer les performances de l’application. Par conséquent, certains fournisseurs offrent la prise en charge de la réception de plusieurs **jeux d’enregistrements** en un seul aller-retour. Ce sujet est abordé dans la rubrique suivante :  
  
-   [Réception de plusieurs recordsets](../../../ado/guide/data/receiving-multiple-recordsets.md)
