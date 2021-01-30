---
description: Transitions d’environnement
title: Transitions d’environnement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- environment transitions [ODBC]
- transitioning states [ODBC], environment
- state transitions [ODBC], environment
ms.assetid: 9d11b1ab-f4c8-48ca-9812-8c04303f939d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: daef0bc276831358cd3d82dd10a1dd44082214af
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194839"
---
# <a name="environment-transitions"></a>Transitions d’environnement
Les environnements ODBC présentent les trois États suivants.  
  
|State|Description|  
|-----------|-----------------|  
|E0|Environnement non alloué|  
|E1|Environnement alloué, connexion non allouée|  
|E2|Environnement alloué, connexion allouée|  
  
 Les tableaux suivants montrent comment chaque fonction ODBC affecte l’état de l’environnement.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|E1 [1]|--[4]|--[4]|  
|IH 2|E2 [5]<br />HY010 6,3|--[4]|  
|IH 1,3|IH|--[4]|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
 [4] l’appel de **SQLAllocHandle** avec *OutputHandlePtr* pointant vers un handle valide remplace ce handle. Il peut s’agir d’une erreur de programmation d’application.  
  
 [5] l’attribut d’environnement SQL_ATTR_ODBC_VERSION avait été défini sur l’environnement.  
  
 [6] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources et SQLDrivers  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH|--[1]<br />HY010 2|--[1]<br />HY010 2|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH 1,0|--[3]<br />HY010 4|--[3]<br />HY010 4|  
|IH 2|IH|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [4] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH 1,0|E0|HY010|  
|IH 2|IH|--[4]<br />E1 [5]|  
|IH 1,3|IH|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
 [4] des descripteurs de connexion ont été alloués.  
  
 [5] le descripteur de connexion spécifié dans *handle* était le seul handle de connexion alloué.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField et SQLGetDiagRec  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH 1,0|--|--|  
|IH 2|IH|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH|--[1]<br />HY010 2|--|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH|--[1]<br />HY010 2|HY011|  
  
 [1] l’attribut d’environnement SQL_ATTR_ODBC_VERSION a été défini sur l’environnement.  
  
 [2] l’argument d' *attribut* n’a pas été SQL_ATTR_ODBC_VERSION, et l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’a pas été défini sur l’environnement.  
  
## <a name="all-other-odbc-functions"></a>Toutes les autres fonctions ODBC  
  
|E0<br /><br /> Non alloué|E1<br /><br /> Allocated|E2<br /><br /> Connexion|  
|------------------------|----------------------|-----------------------|  
|IH|IH|--|
