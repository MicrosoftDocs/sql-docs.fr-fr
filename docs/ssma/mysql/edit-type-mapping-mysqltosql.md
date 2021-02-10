---
description: Modifier le mappage de type (MySQLToSQL)
title: Modifier le mappage de type (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 184f7ab2-725f-491e-a15b-b889f2fb6a68
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b1ddf264835cf0a9a008335787353d7a1051431f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016599"
---
# <a name="edit-type-mapping-mysqltosql"></a>Modifier le mappage de type (MySQLToSQL)
La boîte de dialogue **modifier le mappage de type** vous permet de spécifier la manière dont les types sont mappés entre les objets de base de données source et de destination.  
  
Vous pouvez accéder à cette boîte de dialogue à plusieurs endroits :  
  
-   Lorsque vous sélectionnez une base de données source ou un objet de base de données, l’onglet **mappage de type** s’affiche à droite de l’Explorateur de métadonnées. Cliquez sur **Ajouter** pour ajouter un nouveau mappage de type, ou cliquez sur **modifier** pour modifier un mappage de type existant.  
  
-   Dans le menu **Outils** , sélectionnez **paramètres du projet** ou paramètres du **projet par défaut**. Dans la boîte de dialogue qui en résulte, sélectionnez **mappage de type**. Cliquez sur **Ajouter** pour ajouter un nouveau mappage de type, ou cliquez sur **modifier** pour modifier un mappage de type existant.  
  
-   Les mappages de types spécifiques aux tables remplacent les mappages de type de projet et de base de données. Les mappages spécifiques à la base de données remplacent les mappages de projet.  
  
## <a name="options"></a>Options  
  
##### <a name="source-type"></a>Type de source  
Sélectionnez le type de données source à mapper à un type de données SQL Server.  
  
Si le type de données est de longueur variable, les champs suivants s’affichent sous **SourceType**:  
  
##### <a name="from"></a>Du  
Spécifiez la longueur minimale pour ce mappage. Par exemple, pour le type de données **nchar** , vous pouvez entrer 10 pour spécifier que ce mappage s’adresse à une plage commençant par **nchar (10).**  
  
##### <a name="to"></a>À  
Spécifiez la longueur maximale pour ce mappage. Par exemple, pour le type de données **nchar** , vous pouvez entrer 20 pour spécifier que ce mappage est destiné à une plage se terminant par **nchar (20).**  
  
##### <a name="target-type"></a>Type cible  
Sélectionnez le type de données SQL Server auquel le type de données source est mappé. Lorsque SSMA crée la table ou dans le SQL Server, le type de données source passe à ce type de données.  
  
Si le type de données est de longueur variable, le champ suivant s’affiche sous **type de cible**:  
  
##### <a name="replace-with"></a>Remplacer par  
Spécifiez la longueur cible pour ce mappage. Par exemple, pour le type de données **nvarchar** , vous pouvez entrer 20 pour spécifier que le type de données source spécifié doit être mappé à **nvarchar (20).**  
  
