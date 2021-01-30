---
description: Récapitulatif des fonctions de la DLL d’installation
title: Résumé de la fonction DLL du programme d’installation | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], installer DLL functions
- installer DLL [ODBC]
ms.assetid: 666c09d3-1e10-4d89-9b42-eda2957a87f0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f5cce294142a716180b66544ebe12638e83f792e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174669"
---
# <a name="installer-dll-function-summary"></a>Récapitulatif des fonctions de la DLL d’installation
Le tableau suivant décrit les fonctions de la DLL du programme d’installation. Pour plus d’informations sur la syntaxe et la sémantique de chaque fonction, consultez Référence de l' [API dll du programme d’installation](../../../odbc/reference/syntax/installer-dll-api-reference-function.md).  
  
|Tâche|Nom de la fonction|Objectif|  
|----------|-------------------|-------------|  
|Installation d’ODBC|[SQLConfigDriver](../../../odbc/reference/syntax/sqlconfigdriver-function.md)|Charge la DLL d’installation spécifique au pilote.|  
||[SQLGetInstalledDrivers](../../../odbc/reference/syntax/sqlgetinstalleddrivers-function.md)|Retourne la liste des pilotes installés.|  
||[SQLInstallDriverEx](../../../odbc/reference/syntax/sqlinstalldriverex-function.md)|Ajoute un pilote aux informations système.|  
||[SQLInstallDriverManager](../../../odbc/reference/syntax/sqlinstalldrivermanager-function.md)|Retourne le répertoire cible pour le gestionnaire de pilotes.|  
||[SQLInstallerError](../../../odbc/reference/syntax/sqlinstallererror-function.md)|Retourne des informations d’erreur ou d’État pour les fonctions du programme d’installation.|  
||[SQLInstallTranslatorEx](../../../odbc/reference/syntax/sqlinstalltranslatorex-function.md)|Ajoute un convertisseur aux informations système.|  
||[SQLPostInstallerError](../../../odbc/reference/syntax/sqlpostinstallererror-function.md)|Permet à une bibliothèque de configuration de pilote ou de convertisseur de signaler des erreurs.|  
||[SQLRemoveDriver](../../../odbc/reference/syntax/sqlremovedriver-function.md)|Supprime un pilote des informations système.|  
||[SQLRemoveDriverManager](../../../odbc/reference/syntax/sqlremovedrivermanager-function.md)|Supprime les composants ODBC Core des informations système.|  
||[SQLRemoveTranslator](../../../odbc/reference/syntax/sqlremovetranslator-function.md)|Supprime le traducteur des informations système.|  
|Configuration des sources de données|[SQLConfigDataSource](../../../odbc/reference/syntax/sqlconfigdatasource-function.md)|Appelle la DLL d’installation spécifique au pilote.|  
||[SQLCreateDataSource](../../../odbc/reference/syntax/sqlcreatedatasource-function.md)|Affiche une boîte de dialogue pour ajouter une source de données.|  
||[SQLGetConfigMode](../../../odbc/reference/syntax/sqlgetconfigmode-function.md)|Récupère le mode de configuration qui indique où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système.|  
||[SQLGetPrivateProfileString](../../../odbc/reference/syntax/sqlgetprivateprofilestring-function.md)|Écrit une valeur dans les informations système.|  
||[SQLGetTranslator](../../../odbc/reference/syntax/sqlgettranslator-function.md)|Affiche une boîte de dialogue pour sélectionner un traducteur.|  
||[SQLManageDataSources](../../../odbc/reference/syntax/sqlmanagedatasources.md)|Affiche une boîte de dialogue pour configurer les sources de données et les pilotes.|  
||[SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md)|Lit les informations à partir de sources de données de fichier.|  
||[SQLRemoveDefaultDataSource](../../../odbc/reference/syntax/sqlremovedefaultdatasource-function.md)|Supprime la source de données par défaut.|  
||[SQLRemoveDSNFromIni](../../../odbc/reference/syntax/sqlremovedsnfromini-function.md)|Supprime une source de données.|  
||[SQLSetConfigMode](../../../odbc/reference/syntax/sqlsetconfigmode-function.md)|Définit le mode de configuration qui indique où l’entrée de Odbc.ini répertoriant les valeurs DSN se trouve dans les informations système.|  
||[SQLValidDSN](../../../odbc/reference/syntax/sqlvaliddsn-function.md)|Vérifie la longueur et la validité du nom de la source de données.|  
||[SQLWriteDSNToIni](../../../odbc/reference/syntax/sqlwritedsntoini-function.md)|Ajoute une source de données.|  
||[SQLWriteFileDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md)|Écrit des informations dans les sources de données de fichier.|  
||[SQLWritePrivateProfileString](../../../odbc/reference/syntax/sqlwriteprivateprofilestring-function.md)|Obtient une valeur à partir des informations système.|
