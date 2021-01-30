---
description: Récapitulatif des fonctions ODBC
title: Résumé de la fonction ODBC | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- functions [ODBC], listed by task
ms.assetid: 7aa635da-e6b7-439f-8e9b-c3860e24de5e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d5d27a6093947ba4e29e43f4970848780cbf4e8e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174647"
---
# <a name="odbc-function-summary"></a>Récapitulatif des fonctions ODBC
Le tableau suivant répertorie les fonctions ODBC, regroupées par type de tâche, et comprend la désignation de conformité, ainsi qu’une brève description de l’objectif de chaque fonction. Pour plus d’informations sur les désignations de conformité, consultez [ODBC et l’interface de commande standard](../../../odbc/reference/odbc-and-the-standard-cli.md). Pour plus d’informations sur la syntaxe et la sémantique de chaque fonction, consultez [ODBC API Reference](../../../odbc/reference/syntax/odbc-api-reference.md).  
  
 Une application peut appeler la fonction **SQLGetInfo** pour obtenir des informations de conformité sur un pilote. Pour obtenir des informations sur la prise en charge d’une fonction spécifique dans un pilote, une application peut appeler **SQLGetFunctions**.  
  
|Tâche|Nom de la fonction|Conformité|Objectif|  
|----------|-------------------|-----------------|-------------|  
|Connexion à une source de données|[SQLAllocHandle](../../../odbc/reference/syntax/sqlallochandle-function.md)|ISO 92|Obtient un handle d’environnement, de connexion, d’instruction ou de descripteur.|  
||[SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)|ISO 92|Établit une connexion à un pilote spécifique par le nom de la source de données, l’ID d’utilisateur et le mot de passe.|  
||[SQLDriverConnect](../../../odbc/reference/syntax/sqldriverconnect-function.md)|ODBC|Établit une connexion à un pilote spécifique par chaîne de connexion ou demande que le gestionnaire de pilotes et le pilote affichent des boîtes de dialogue de connexion pour l’utilisateur.|  
||[SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|ODBC|Retourne des niveaux successifs d’attributs de connexion et des valeurs d’attribut valides. Lorsqu’une valeur a été spécifiée pour chaque attribut de connexion, se connecte à la source de données.|  
|Obtention d’informations sur un pilote et une source de données|[SQLDataSources](../../../odbc/reference/syntax/sqldatasources-function.md)<br /><br /> [SQLDrivers](../../../odbc/reference/syntax/sqldrivers-function.md)|ISO 92<br /><br /> ODBC|Retourne la liste des sources de données disponibles.<br /><br /> Retourne la liste des pilotes installés et leurs attributs.|  
||[SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|ISO 92|Retourne des informations sur un pilote et une source de données spécifiques.|  
||[SQLGetFunctions](../../../odbc/reference/syntax/sqlgetfunctions-function.md)|ISO 92|Retourne les fonctions de pilote prises en charge.|  
||[SQLGetTypeInfo](../../../odbc/reference/syntax/sqlgettypeinfo-function.md)|ISO 92|Retourne des informations sur les types de données pris en charge.|  
|Définition et récupération des attributs de pilote|[SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)<br /><br /> [SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|ISO 92<br /><br /> ISO 92|Définit un attribut de connexion.<br /><br /> Retourne la valeur d’un attribut de connexion.|  
||[SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|ISO 92|Définit un attribut d’environnement.|  
||[SQLGetEnvAttr](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|ISO 92|Retourne la valeur d’un attribut d’environnement.|  
||[SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|ISO 92|Définit un attribut d’instruction.|  
||[SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|ISO 92|Retourne la valeur d’un attribut d’instruction.|  
|Définition et récupération des champs de descripteur|[SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)<br /><br /> [SQLGetDescRec](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|ISO 92<br /><br /> ISO 92|Retourne la valeur d’un champ de descripteur unique.<br /><br /> Retourne les valeurs de plusieurs champs de descripteur.|  
||[SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|ISO 92|Définit un champ de descripteur unique.|  
||[SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|ISO 92|Définit plusieurs champs de descripteur.|  
||[SQLCopyDesc](../../../odbc/reference/syntax/sqlcopydesc-function.md)|ISO 92|Copie les informations de descripteur d’un handle de descripteur vers un autre.|  
|Préparation des requêtes SQL|[SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|ISO 92|Prépare une instruction SQL en vue d’une exécution ultérieure.|  
||[SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|ODBC|Affecte le stockage d’un paramètre dans une instruction SQL.|  
||[SQLGetCursorName](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|ISO 92|Retourne le nom du curseur associé à un descripteur d’instruction.|  
||[SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md)|ISO 92|Spécifie un nom de curseur.|  
||[SQLSetScrollOptions](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|ODBC|Définit des options qui contrôlent le comportement du curseur.|  
|Envoi des demandes|[SQLExecute](../../../odbc/reference/syntax/sqlexecute-function.md)<br /><br /> [SQLExecDirect](../../../odbc/reference/syntax/sqlexecdirect-function.md)|ISO 92<br /><br /> ISO 92|Exécute une instruction préparée.<br /><br /> Exécute une instruction.|  
||[SQLNativeSql](../../../odbc/reference/syntax/sqlnativesql-function.md)|ODBC|Retourne le texte d’une instruction SQL traduit par le pilote.|  
||[SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|ODBC|Retourne la description d’un paramètre spécifique dans une instruction.|  
||[SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|ISO 92|Retourne le nombre de paramètres dans une instruction.|  
||[SQLParamData](../../../odbc/reference/syntax/sqlparamdata-function.md)|ISO 92|Utilisé conjointement avec **SQLPutData** pour fournir des données de paramètre au moment de l’exécution. (Utile pour les valeurs de données de type long.)|  
||[SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|ISO 92|Envoie tout ou partie d’une valeur de données pour un paramètre. (Utile pour les valeurs de données de type long.)|  
|Récupération des résultats et des informations sur les résultats|[SQLRowCount](../../../odbc/reference/syntax/sqlrowcount-function.md)<br /><br /> [SQLNumResultCols](../../../odbc/reference/syntax/sqlnumresultcols-function.md)|ISO 92<br /><br /> ISO 92|Retourne le nombre de lignes affectées par une demande d’insertion, de mise à jour ou de suppression.<br /><br /> Retourne le nombre de colonnes dans le jeu de résultats.|  
||[SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|ISO 92|Décrit une colonne dans le jeu de résultats.|  
||[SQLColAttribute](../../../odbc/reference/syntax/sqlcolattribute-function.md)|ISO 92|Décrit les attributs d’une colonne dans le jeu de résultats.|  
||[SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|ISO 92|Affecte le stockage pour une colonne de résultats et spécifie le type de données.|  
||[SQLFetch](../../../odbc/reference/syntax/sqlfetch-function.md)|ISO 92|Retourne plusieurs lignes de résultats.|  
||[SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|ISO 92|Retourne des lignes de résultat défilantes.|  
||[SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|ISO 92|Retourne une partie ou l’intégralité d’une colonne d’une ligne d’un jeu de résultats. (Utile pour les valeurs de données de type long.)|  
||[SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md)|ODBC|Positionne un curseur dans un bloc de données extrait et permet à une application d’actualiser des données dans l’ensemble de lignes ou de mettre à jour ou de supprimer des données dans le jeu de résultats.|  
||[SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)|ODBC|Effectue des insertions en bloc et des opérations de signet en bloc, y compris Update, DELETE et FETCH par signet.|  
||[SQLMoreResults](../../../odbc/reference/syntax/sqlmoreresults-function.md)|ODBC|Détermine si d’autres jeux de résultats sont disponibles et, le cas échéant, initialise le traitement pour le prochain jeu de résultats.|  
||[SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|ISO 92|Retourne des informations de diagnostic supplémentaires (un champ unique de la structure de données de diagnostic).|  
||[SQLGetDiagRec](../../../odbc/reference/syntax/sqlgetdiagrec-function.md)|ISO 92|Retourne des informations de diagnostic supplémentaires (plusieurs champs de la structure de données de diagnostic).|  
|Obtention d’informations sur les tables système de la source de données (fonctions de catalogue)|[SQLColumnPrivileges](../../../odbc/reference/syntax/sqlcolumnprivileges-function.md)<br /><br /> [SQLColumns](../../../odbc/reference/syntax/sqlcolumns-function.md)|ODBC<br /><br /> Ouvrir le groupe|Retourne une liste de colonnes et les privilèges associés pour une ou plusieurs tables.<br /><br /> Retourne la liste des noms de colonnes dans les tables spécifiées.|  
||[SQLForeignKeys](../../../odbc/reference/syntax/sqlforeignkeys-function.md)|ODBC|Retourne une liste des noms de colonnes qui composent les clés étrangères, s’ils existent pour une table spécifiée.|  
||[SQLPrimaryKeys](../../../odbc/reference/syntax/sqlprimarykeys-function.md)|ODBC|Retourne la liste des noms de colonnes qui composent la clé primaire d’une table.|  
||[SQLProcedureColumns](../../../odbc/reference/syntax/sqlprocedurecolumns-function.md)|ODBC|Retourne la liste des paramètres d’entrée et de sortie, ainsi que les colonnes qui composent le jeu de résultats pour les procédures spécifiées.|  
||[SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md)|ODBC|Retourne la liste des noms de procédures stockés dans une source de données spécifique.|  
||[SQLSpecialColumns](../../../odbc/reference/syntax/sqlspecialcolumns-function.md)|Ouvrir le groupe|Retourne des informations sur l’ensemble optimal de colonnes qui identifient de façon unique une ligne dans une table spécifiée, ou sur les colonnes qui sont mises à jour automatiquement lorsqu’une valeur de la ligne est mise à jour par une transaction.|  
||[SQLStatistics](../../../odbc/reference/syntax/sqlstatistics-function.md)|ISO 92|Retourne des statistiques sur une table unique et la liste des index associés à la table.|  
||[SQLTablePrivileges](../../../odbc/reference/syntax/sqltableprivileges-function.md)|ODBC|Retourne une liste de tables et les privilèges associés à chaque table.|  
||[SQLTables](../../../odbc/reference/syntax/sqltables-function.md)|Ouvrir le groupe|Retourne la liste des noms de tables stockés dans une source de données spécifique.|  
|Fin d’une instruction|[SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|ISO 92|Termine le traitement de l’instruction, ignore les résultats en attente et, éventuellement, libère toutes les ressources associées au descripteur d’instruction.|  
||[SQLCloseCursor](../../../odbc/reference/syntax/sqlclosecursor-function.md)|ISO 92|Ferme un curseur qui a été ouvert sur un handle d’instruction.|  
||[SQLCancel](../../../odbc/reference/syntax/sqlcancel-function.md)|ISO 92|Annule le traitement sur une instruction.|  
||[SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|ODBC|Annule le traitement sur une instruction ou une connexion.|  
||[SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md)|ISO 92|Valide ou restaure une transaction.|  
|Arrêt d’une connexion|[SQLDisconnect](../../../odbc/reference/syntax/sqldisconnect-function.md)<br /><br /> [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|ISO 92<br /><br /> ISO 92|Ferme la connexion.<br /><br /> Libère un handle d’environnement, de connexion, d’instruction ou de descripteur.|
