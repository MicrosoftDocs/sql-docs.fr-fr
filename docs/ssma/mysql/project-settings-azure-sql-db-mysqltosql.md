---
description: Paramètres du projet (Azure SQL Database) (MySQLToSQL)
title: Paramètres du projet (Azure SQL Database) (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8c06420a-533b-4de0-948d-a0c6b368c544
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: a5c3075180e60737931adfb1f544ace8bb38b9b0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071863"
---
# <a name="project-settings-azure-sql-database-mysqltosql"></a>Paramètres du projet (Azure SQL Database) (MySQLToSQL)
Les paramètres du projet SQL Azure vous permettent de configurer l’ajout du suffixe Azure SQL Database dans la boîte de dialogue de connexion et d’autoriser également l’implémentation d’un mécanisme de pulsation dans SQL Azure connexion.  
  
Le volet SQL Azure est disponible dans les boîtes de dialogue **paramètres du projet** et **paramètres du projet par défaut** .  
  
-   Utilisez la boîte de dialogue Paramètres du projet pour définir les options de configuration du projet actif. Pour accéder aux paramètres de SQL Azure, dans le menu **Outils** , sélectionnez **paramètres du projet**, cliquez sur **général** en bas du volet gauche, puis sélectionnez **SQL Azure**.  
  
-   Utilisez la boîte de dialogue Paramètres du projet par défaut pour définir les options de configuration de tous les projets. Pour accéder aux paramètres de SQL Azure, dans le menu **Outils** , sélectionnez **paramètres DefaultProject**, sélectionnez type de projet de migration comme SQL Azure dans la liste déroulante **version cible** de la migration pour accéder aux paramètres dans SQL Azure volet, cliquez sur **général** en bas du volet gauche, puis sélectionnez **SQL Azure**.  
  
## <a name="options"></a>Options  
  
## <a name="connectivity"></a>Connectivité  
**Intervalle de pulsation**  
  
Spécifie un intervalle de temps à utiliser pour le mécanisme de pulsation afin que la connexion SQL Azure reste active en « minutes : secondes ».  
  
**Valeur par défaut**: « 4:45 »  
  
La valeur doit être spécifiée au format « m :SS » (par exemple, « 4:45 » ou « 0:50 »).  
  
**Suffixe du serveur SQL Azure**  
  
Spécifie le suffixe du serveur SQL Azure  
  
**Valeur par défaut**: 'Database.Windows.net'.  
  
