---
description: Transitions de connexion
title: Transitions de connexion | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], connection
- connection transitions [ODBC]
- state transitions [ODBC], connection
ms.assetid: 6b6e1a47-4a52-41c8-bb9e-7ddeae09913e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 57135740af1c8b7a4b8637b33adacf280c025e6d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100071763"
---
# <a name="connection-transitions"></a>Transitions de connexion
Les connexions ODBC ont les États suivants.  
  
|State|Description|  
|-----------|-----------------|  
|C0|Environnement non alloué, connexion non allouée|  
|C1|Environnement alloué, connexion non allouée|  
|C2|Environnement alloué, connexion allouée|  
|C3|La fonction de connexion a besoin de données|  
|C4|Connexion connectée|  
|C5|Connexion connectée, instruction allouée|  
|C6|Connexion connectée, transaction en cours. Il est possible qu’une connexion soit dans l’État C6 sans qu’aucune instruction ne soit allouée sur la connexion. Par exemple, supposons que la connexion est en mode de validation manuelle et qu’elle soit dans l’État C4. Si une instruction est allouée, exécutée (en démarrant une transaction), puis libérée, la transaction reste active, mais il n’y a aucune instruction sur la connexion.|  
  
 Les tableaux suivants montrent comment chaque fonction ODBC affecte l’état de la connexion.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|C0<br /><br /> Pas de env.|C1 non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|--------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|C1 [1]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|IH 2|C2|--[5]|--[5]|--[5]|--[5]|--[5]|  
|IH 1,3|IH|(08003)|(08003)|C5|--[5]|--[5]|  
|IH 4|IH|(08003)|(08003)|--[5]|--[5]|--[5]|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [4] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DESC.  
  
 [5] l’appel de **SQLAllocHandle** avec *OutputHandlePtr* pointant vers un handle valide remplace ce handle sans tenir compte du contenu précédent ofthat handle et peut entraîner des problèmes pour les pilotes ODBC. Il s’agit d’une programmation d’application ODBC incorrecte pour appeler **SQLAllocHandle** deux fois avec la même variable d’application définie pour *\* OutputHandlePtr* sans appeler **SQLFreeHandle** pour libérer le descripteur avant de le réallouer. Le remplacement des handles ODBC de telle manière peut entraîner des erreurs ou des comportements incohérents dans la partie des pilotes ODBC.  
  
## <a name="sqlbrowseconnect"></a>SQLBrowseConnect  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C3 [d] C4 [s]|--[d] C2 [e] C4 [s]|(08002)|(08002)|(08002)|  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--|--[1] C5 [2]|  
  
 [1] la connexion était en mode de validation manuelle.  
  
 [2] la connexion était en mode de validation automatique.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges et SQLTables  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--|  
  
 [1] la connexion était en mode de validation automatique, ou la source de données n’a pas commencé de transaction.  
  
 [2] la connexion était en mode de validation manuelle et la source de données a commencé une transaction.  
  
## <a name="sqlconnect"></a>SQLConnect  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C4|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlcopydesc-sqlgetdescfield-sqlgetdescrec-sqlsetdescfield-and-sqlsetdescrec"></a>SQLCopyDesc, SQLGetDescField, SQLGetDescRec, SQLSetDescField et SQLSetDescRec  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|--[1]|--|--|  
  
 [1] dans cet État, les seuls descripteurs disponibles pour l’application sont des descripteurs alloués explicitement.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources et SQLDrivers  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|--|--|--|--|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|(08003)|C2|C2|C2|25000|  
  
## <a name="sqldriverconnect"></a>SQLDriverConnect  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|C4 s--n [f]|(08002)|(08002)|(08002)|(08002)|  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1,0|--[3]|--[3]|--[3]|--|--|--[4] ou ([5], [6], et [8]) C4 [5] et [7] C5 [5], [6] et [9]|  
|IH 2|IH|(08003)|(08003)|--|--|C5|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] étant donné que la connexion n’est pas dans un état connecté, elle n’est pas affectée par la transaction.  
  
 [4] échec de la validation ou de la restauration sur la connexion. La fonction retourne SQL_ERROR dans ce cas.  
  
 [5] la validation ou la restauration a réussi sur la connexion. La fonction retourne SQL_ERROR si la validation ou la restauration a échoué sur une autre connexion, ou si la fonction retourne SQL_SUCCESS si la validation ou la restauration a réussi sur toutes les connexions.  
  
 [6] au moins une instruction a été allouée sur la connexion.  
  
 [7] aucune instruction n’a été allouée sur la connexion.  
  
 [8] la connexion a au moins une instruction pour laquelle il existait un curseur ouvert, et la source de données préserve les curseurs lorsque les transactions sont validées ou restaurées, selon le cas (selon que *CompletionType* a été SQL_COMMIT ou SQL_ROLLBACK). Pour plus d’informations, consultez les attributs SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 [9] si la connexion avait des instructions pour lesquelles il existait des curseurs ouverts, les curseurs n’étaient pas conservés lors de la validation ou de la restauration de la transaction.  
  
## <a name="sqlexecdirect-and-sqlexecute"></a>SQLExecDirect et SQLExecute  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2] C6 [3]|--|  
  
 [1] la connexion était en mode de validation automatique et l’instruction exécutée n’était pas une *spécification* de *curseur* (telle qu’une instruction SELECT); ou la connexion était en mode de validation manuelle, et l’instruction exécutée n’a pas commencé une transaction.  
  
 [2] la connexion était en mode de validation automatique et l’instruction exécutée était une  *spécification* de curseur (telle qu’une instruction SELECT).  
  
 [3] la connexion était en mode de validation manuelle et la source de données a commencé une transaction.  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1,0|C0|HY010|HY010|HY010|HY010|HY010|  
|IH 2|IH|C1|HY010|HY010|HY010|HY010|  
|IH 1,3|IH|IH|IH|IH|C4 [5]--[6]|--[7] C4 [5] et [8] C5 [6] et [8]|  
|IH 4|IH|IH|IH|--|--|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [4] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DESC.  
  
 [5] une seule instruction a été allouée sur la connexion.  
  
 [6] plusieurs instructions ont été allouées sur la connexion.  
  
 [7] la connexion était en mode de validation manuelle.  
  
 [8] la connexion était en mode de validation automatique.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1,0|IH|IH|IH|IH|--|C5 [3]--[4]|  
|IH 2|IH|IH|IH|IH|--|--|  
  
 [1] cette ligne affiche les transactions lorsque l’argument de l' *option* est SQL_CLOSE.  
  
 [2] cette ligne affiche les transactions lorsque l’argument de l' *option* est SQL_UNBIND ou SQL_RESET_PARAMS.  
  
 [3] la connexion était en mode de validation automatique et aucun curseur n’était ouvert sur les instructions à l’exception de celle-ci.  
  
 [4] la connexion était en mode de validation manuelle, ou elle était en mode de validation automatique et un curseur était ouvert sur au moins une autre instruction.  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--|--|--|  
  
 [1] l’argument d' *attribut* a été SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE ou SQL_ATTR_TRACEFILE, ou une valeur a été définie pour l’attribut de connexion.  
  
 [2] l’argument d' *attribut* n’était pas SQL_ATTR_ACCESS_MODE, SQL_ATTR_AUTOCOMMIT, SQL_ATTR_LOGIN_TIMEOUT, SQL_ATTR_ODBC_CURSORS, SQL_ATTR_TRACE ou SQL_ATTR_TRACEFILE, et aucune valeur n’a été définie pour l’attribut de connexion.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField et SQLGetDiagRec  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH 1,0|--|--|--|--|--|--|  
|IH 2|IH|--|--|--|--|--|  
|IH 1,3|IH|IH|IH|IH|--|--|  
|IH 4|IH|IH|IH|--|--|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [4] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DESC.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|HY010|HY010|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|08003|--|--|--|  
  
 [1] l’argument *infotype* a été SQL_ODBC_VER.  
  
 [2] l’argument *infotype* n’a pas été SQL_ODBC_VER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--[3] C5 [1]|  
  
 [1] la connexion était en mode de validation automatique, et l’appel à **SQLMoreResults** n’a pas initialisé le traitement d’un jeu de résultats d’une spécification de curseur.  
  
 [2] la connexion était en mode de validation automatique, et l’appel à **SQLMoreResults** a initialisé le traitement d’un jeu de résultats d’une spécification de curseur.  
  
 [3] la connexion était en mode de validation manuelle.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|(08003)|(08003)|--|--|--|  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--[1] C6 [2]|--|  
  
 [1] la connexion était en mode de validation automatique, ou la source de données n’a pas commencé de transaction.  
  
 [2] la connexion était en mode de validation manuelle et la source de données a commencé une transaction.  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|--[1] 08003 [2]|HY010|--[3] 08002 [4] HY011 [5]|--[3] 08002 [4] HY011 [5]|--[3] et [6] C5 [8] 08002 [4] HY011 [5] ou [7]|  
  
 [1] l’argument d' *attribut* n’a pas été SQL_ATTR_TRANSLATE_LIB ou SQL_ATTR_TRANSLATE_OPTION.  
  
 [2] l’argument d' *attribut* était SQL_ATTR_TRANSLATE_LIB ou SQL_ATTR_TRANSLATE_OPTION.  
  
 [3] l’argument d' *attribut* n’a pas été SQL_ATTR_ODBC_CURSORS ou SQL_ATTR_PACKET_SIZE.  
  
 [4] l’argument d' *attribut* a été SQL_ATTR_ODBC_CURSORS.  
  
 [5] l’argument d' *attribut* a été SQL_ATTR_PACKET_SIZE.  
  
 [6] l’argument d' *attribut* n’a pas été SQL_ATTR_AUTOCOMMIT, ou l’argument d' *attribut* a été SQL_ATTR_AUTOCOMMIT et la définition de cet attribut n’a pas validé la transaction.  
  
 [7] l’argument d' *attribut* a été SQL_ATTR_TXN_ISOLATION.  
  
 [8] l’argument d' *attribut* a été SQL_ATTR_AUTOCOMMIT et la configuration de cet attribut a validé la transaction.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|--|--|HY010|--|--|--|  
  
## <a name="all-other-odbc-functions"></a>Toutes les autres fonctions ODBC  
  
|C0<br /><br /> Pas de env.|C1<br /><br /> Non alloué|C2<br /><br /> Allocated|C3<br /><br /> Données nécessaires|C4<br /><br /> Connecté|C5<br /><br /> .|C6<br /><br /> Transaction|  
|--------------------|------------------------|----------------------|----------------------|----------------------|----------------------|------------------------|  
|IH|IH|IH|IH|IH|--|--|
