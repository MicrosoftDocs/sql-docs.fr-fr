---
description: Transitions d’instruction
title: Transitions d’instruction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- transitioning states [ODBC], statement
- state transitions [ODBC], statement
- statement transitions [ODBC]
ms.assetid: 3d70e0e3-fe83-4b4d-beac-42c82495a05b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 904b1116be5e0daec2fce61e4a988c9b957322ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016239"
---
# <a name="statement-transitions"></a>Transitions d’instruction
Les instructions ODBC ont les États suivants.  
  
|State|Description|  
|-----------|-----------------|  
|S0|Instruction non allouée. (L’état de la connexion doit être C4, C5 ou C6. Pour plus d’informations, consultez [transitions de connexion](../../../odbc/reference/appendixes/connection-transitions.md).)|  
|S1|Instruction allouée.|  
|S2|Instruction préparée. Aucun jeu de résultats ne sera créé.|  
|S3|Instruction préparée. Un jeu de résultats (peut-être vide) sera créé.|  
|S4|Instruction exécutée et aucun jeu de résultats n’a été créé.|  
|S5|L’instruction exécutée et un jeu de résultats (éventuellement vide) ont été créés. Le curseur est ouvert et positionné avant la première ligne du jeu de résultats.|  
|S6|Curseur positionné avec **SQLFetch** ou **SQLFetchScroll**.|  
|S7|Curseur positionné avec **SQLExtendedFetch**.|  
|S8|La fonction a besoin de données. **SQLParamData** n’a pas été appelé.|  
|S9|La fonction a besoin de données. **SQLPutData** n’a pas été appelé.|  
|S10|La fonction a besoin de données. **SQLPutData** a été appelé.|  
|S11|Toujours en cours d’exécution. Une instruction reste dans cet État après qu’une fonction exécutée de façon asynchrone retourne SQL_STILL_EXECUTING. Une instruction est momentanément dans cet État alors qu’une fonction qui accepte un descripteur d’instruction est en cours d’exécution. Le séjour temporaire en état S11 n’est pas affiché dans les tables d’État, à l’exception de la table d’État pour **SQLCancel**. Lorsqu’une instruction est momentanément dans l’État S11, la fonction peut être annulée en appelant **SQLCancel** à partir d’un autre thread.|  
|S12|Exécution asynchrone annulée. Dans S12, une application doit appeler la fonction Canceled jusqu’à ce qu’elle retourne une valeur autre que SQL_STILL_EXECUTING. La fonction a été annulée avec succès uniquement si la fonction retourne SQL_ERROR et SQLSTATE HY008 (opération annulée). Si une autre valeur est retournée, par exemple SQL_SUCCESS, l’opération d’annulation a échoué et la fonction s’est exécutée normalement.|  
  
 Les États S2 et S3 sont appelés États préparés, les États S5 à S7 comme États de curseur, les États S8 à S10 en tant qu’États de données requis et les États S11 et S12 comme États asynchrones. Dans chacun de ces groupes, les transitions sont affichées séparément uniquement lorsqu’elles sont différentes pour chaque État du groupe ; dans la plupart des cas, les transitions pour chaque État de chaque groupe sont les mêmes.  
  
 Les tableaux suivants montrent comment chaque fonction ODBC affecte l’état de l’instruction.  
  
## <a name="sqlallochandle"></a>SQLAllocHandle  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1], [5], [6]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[2], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|S1 [3]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
|--[4], [5]|--[5]|--[5]|--[5]|--[5]|--[5]|--[5]|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DBC.  
  
 [3] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [4] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_DESC.  
  
 [5] l’appel de **SQLAllocHandle** avec *OutputHandlePtr* pointant vers un handle valide remplace ce handle sans tenir compte du contenu précédent dans ce handle et peut entraîner des problèmes pour les pilotes ODBC. Il s’agit d’une programmation d’application ODBC incorrecte pour appeler **SQLAllocHandle** deux fois avec la même variable d’application définie pour *\* OutputHandlePtr* sans appeler **SQLFreeHandle** pour libérer le descripteur avant de le réallouer. Le remplacement des handles ODBC de telle manière peut entraîner des erreurs ou des comportements incohérents dans la partie des pilotes ODBC.  
  
## <a name="sqlbindcol"></a>SQLBindCol  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbindparameter"></a>SQLBindParameter  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlbrowseconnect-sqlconnect-and-sqldriverconnect"></a>SQLBrowseConnect, SQLConnect et SQLDriverConnect  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|08002|08002|08002|08002|08002|08002|08002|  
  
## <a name="sqlbulkoperations"></a>SQLBulkOperations  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24 000|Voir le tableau suivant|HY010|NS [c] HY010 o|  
  
## <a name="sqlbulkoperations-cursor-states"></a>SQLBulkOperations (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[s] S8 [d] S11 [x]|--[s] S8 [d] S11 [x]|HY010|  
  
## <a name="sqlcancel"></a>SQLCancel  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|S1 [1] S2 [Nr] et [2] S3 [r] et [2] S5 [3] et [5] S6 ([3] ou [4]) et [6] S7 [4] et [7]|Voir le tableau suivant|  
  
 [1]   **SQLExecDirect** a retourné SQL_NEED_DATA.  
  
 [2]   **SQLExecute** a retourné SQL_NEED_DATA.  
  
 [3]   **SQLBulkOperations** a retourné SQL_NEED_DATA.  
  
 [4]   **SQLSetPos** a retourné SQL_NEED_DATA.  
  
 [5]   **SQLFetch**, **SQLFetchScroll** ou **SQLExtendedFetch** n’a pas été appelé.  
  
 [6]   **SQLFetch** ou **SQLFetchScroll** a été appelé.  
  
 [7]   **SQLExtendedFetch** a été appelé.  
  
## <a name="sqlcancel-asynchronous-states"></a>SQLCancel (États asynchrones)  
  
|S11<br /><br /> Toujours en cours d’exécution|S12<br /><br /> Asynchrone annulé|  
|-----------------------------|-----------------------------|  
|NS [1] S12 [2]|S12|  
  
 [1] l’instruction était temporairement dans l’État S11 lors de l’exécution d’une fonction. **SQLCancel** a été appelé à partir d’un thread différent.  
  
 [2] l’instruction était dans l’État S11, car une fonction appelée de manière asynchrone retournait SQL_STILL_EXECUTING.  
  
## <a name="sqlclosecursor"></a>SQLCloseCursor  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|24 000|24 000|24 000|S1 [NP] S3 [p]|HY010|HY010|  
  
## <a name="sqlcolattribute"></a>SQLColAttribute  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Voir le tableau suivant|24 000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqlcolattribute-prepared-states"></a>SQLColAttribute (États préparés)  
  
|S2<br /><br /> Aucun résultat|S3<br /><br /> Résultats|  
|-----------------------|--------------------|  
|--[1] 07005 [2]|--[s] S11 x|  
  
 [1]   *FieldIdentifier* a été SQL_DESC_COUNT.  
  
 [2]   *FieldIdentifier* n’a pas été SQL_DESC_COUNT.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges et SQLTables  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S5 [s] S11 [x]|S1 [e] S5 [s] S11 [x]|S1 [e] et [1] S5 [s] et [1] S11 [x] et [1] 24000 [2]|Voir le tableau suivant|HY010|NS [c] HY010 o|  
  
 [1] le résultat actuel est le dernier ou le seul résultat, ou il n’y a aucun résultat actuel. Pour plus d’informations sur les résultats multiples, consultez [résultats multiples](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] le résultat actuel n’est pas le dernier résultat.  
  
## <a name="sqlcolumnprivileges-sqlcolumns-sqlforeignkeys-sqlgettypeinfo-sqlprimarykeys-sqlprocedurecolumns-sqlprocedures-sqlspecialcolumns-sqlstatistics-sqltableprivileges-and-sqltables-cursor-states"></a>SQLColumnPrivileges, SQLColumns, SQLForeignKeys, SQLGetTypeInfo, SQLPrimaryKeys, SQLProcedureColumns, SQLProcedures, SQLSpecialColumns, SQLStatistics, SQLTablePrivileges et SQLTables (États de curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24 000|24000 [1]|24 000|  
  
 [1] cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.  
  
## <a name="sqlcopydesc"></a>SQLCopyDesc  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|AU-DESSUS DE 1 [1]|--|--|--|--|HY010|NS [c] et [3] HY010 [o] ou [4]|  
|AU-DESSUS DE 1 [2]|HY010|Voir le tableau suivant|24 000|--[s] S11 x|HY010|NS [c] et [3] HY010 [o] ou [4]|  
  
 [1] cette ligne montre les transitions lorsque l’argument *SourceDescHandle* était un ARD, un APD ou un IPD.  
  
 [2] cette ligne affiche des transitions lorsque l’argument *SourceDescHandle* était un IRD.  
  
 [3] les arguments *SourceDescHandle* et *TargetDescHandle* étaient les mêmes que dans la fonction **SQLCopyDesc** qui s’exécute de façon asynchrone.  
  
 [4] l’argument *SourceDescHandle* ou l’argument *TargetDescHandle* (ou les deux) étaient différents de ceux de la fonction **SQLCopyDesc** qui s’exécute de façon asynchrone.  
  
## <a name="sqlcopydesc-prepared-states"></a>SQLCopyDesc (États préparés)  
  
|S2<br /><br /> Aucun résultat|S3<br /><br /> Résultats|  
|-----------------------|--------------------|  
|24000 [1]|--[s] S11 [x]|  
  
 1,0 Cette ligne affiche des transitions lorsque l’argument *SourceDescHandle* était un IRD.  
  
## <a name="sqldatasources-and-sqldrivers"></a>SQLDataSources et SQLDrivers  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqldescribecol"></a>SQLDescribeCol  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Voir le tableau suivant|24 000|--[s] S11 [x]|HY010|NS [c] HY010 o|  
  
## <a name="sqldescribecol-prepared-states"></a>SQLDescribeCol (États préparés)  
  
|S2<br /><br /> Aucun résultat|S3<br /><br /> Résultats|  
|-----------------------|--------------------|  
|07005|--[s] S11 [x]|  
  
## <a name="sqldescribeparam"></a>SQLDescribeParam  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|HY010|HY010|HY010|NS [c] HY010 [o]|  
  
## <a name="sqldisconnect"></a>SQLDisconnect  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|S0 [1]|S0 [1]|S0 [1]|S0 [1]|HY010|HY010|  
  
 [1] l’appel de **SQLDisconnect** libère toutes les instructions associées à la connexion. En outre, cela retourne l’état de la connexion à C2 ; l’état de la connexion doit être C4 avant que l’état de l’instruction soit S0.  
  
## <a name="sqlendtran"></a>SQLEndTran  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--[2] ou [3] S1 [1]|--[3] S1 [NP] et ([1] ou [2]) S1 [p] et [1] S2 [p] et [2]|--[3] S1 [NP] et ([1] ou [2]) S1 [p] et [1] S3 [p] et [2]|HY010|HY010|  
  
 [1] l’argument *CompletionType* est SQL_COMMIT et **SQLGetInfo** retourne SQL_CB_DELETE pour le type d’informations SQL_CURSOR_COMMIT_BEHAVIOR, ou l’argument *CompletionType* est SQL_ROLLBACK et **SQLGetInfo** retourne SQL_CB_DELETE pour le type d’informations SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [2] l’argument *CompletionType* est SQL_COMMIT et **SQLGetInfo** retourne SQL_CB_CLOSE pour le type d’informations SQL_CURSOR_COMMIT_BEHAVIOR, ou l’argument *CompletionType* est SQL_ROLLBACK et **SQLGetInfo** retourne SQL_CB_CLOSE pour le type d’informations SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
 [3] l’argument *CompletionType* est SQL_COMMIT et **SQLGetInfo** retourne SQL_CB_PRESERVE pour le type d’informations SQL_CURSOR_COMMIT_BEHAVIOR, ou l’argument *CompletionType* est SQL_ROLLBACK et **SQLGetInfo** retourne SQL_CB_PRESERVE pour le type d’informations SQL_CURSOR_ROLLBACK_BEHAVIOR.  
  
## <a name="sqlexecdirect"></a>SQLExecDirect  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S4 [s] et [Nr] S5 [s] et [r] S8 [d] S11 [x]|--[e] et [1] S1 [e] et [2] S4 [s] et [Nr] S5 [s] et [r] S8 [d] S11 [x]|--[e], [1] et [3] S1 [e], [2], et [3] S4 [s], [Nr] et [3] S5 [s], [r] et [3] S8 [d] et [3] S11 [x] et [3] 24000 [4]|Voir le tableau suivant|HY010|NS [c] HY010 [o]|  
  
 [1] l’erreur a été retournée par le gestionnaire de pilotes.  
  
 [2] l’erreur n’a pas été retournée par le gestionnaire de pilotes.  
  
 [3] le résultat actuel est le dernier ou le seul résultat, ou il n’y a aucun résultat actuel. Pour plus d’informations sur les résultats multiples, consultez [résultats multiples](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] le résultat actuel n’est pas le dernier résultat.  
  
## <a name="sqlexecdirect-cursor-states"></a>SQLExecDirect (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24 000|24000 [1]|24 000|  
  
 [1] cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.  
  
## <a name="sqlexecute"></a>SQLExecute  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|Voir le tableau suivant|S2 [e], p, et [1] S4 [s], [p], [Nr] et [1] S5 [s], [p], [r] et [1] S8 [d], [p] et [1] S11 [x], [p] et [1] 24000 [p] et [2] HY010 [NP]|Voir table des États de curseur|HY010|NS [c] HY010 [o]|  
  
 [1] le résultat actuel est le dernier ou le seul résultat, ou il n’y a aucun résultat actuel. Pour plus d’informations sur les résultats multiples, consultez [résultats multiples](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [2] le résultat actuel n’est pas le dernier résultat.  
  
## <a name="sqlexecute-prepared-states"></a>SQLExecute (États préparés)  
  
|S2<br /><br /> Aucun résultat|S3<br /><br /> Résultats|  
|-----------------------|--------------------|  
|S4 [s] S8 [d] S11 [x]|S5 [s] S8 [d] S11 [x]|  
  
## <a name="sqlexecute-cursor-states"></a>SQLExecute (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24000 [p] HY010 [NP]|24000 [p], [1] HY010 [NP]|24000 [p] HY010 [NP]|  
  
 [1] cette erreur est retournée par le gestionnaire de pilotes si **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_NO_DATA, et est retourné par le pilote si **sqlfetch** ou **SQLFetchScroll** a retourné SQL_NO_DATA.  
  
## <a name="sqlextendedfetch"></a>SQLExtendedFetch  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S1010|S1010|24 000|Voir le tableau suivant|S1010|NS [c] S1010 [o]|  
  
## <a name="sqlextendedfetch-cursor-states"></a>SQLExtendedFetch (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S7 [s] ou [NF] S11 [x]|S1010|--[s] ou [NF] S11 [x]|  
  
## <a name="sqlfetch-and-sqlfetchscroll"></a>SQLFetch et SQLFetchScroll  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24 000|Voir le tableau suivant|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlfetch-and-sqlfetchscroll-cursor-states"></a>SQLFetch et SQLFetchScroll (États de curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|S6 [s] ou [NF] S11 [x]|--[s] ou [NF] S11 [x]|HY010|  
  
## <a name="sqlfreehandle"></a>SQLFreeHandle  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|HY010|HY010|HY010|HY010|HY010|HY010|  
|AU-DESSUS DE 1 [2]|S0|S0|S0|S0|HY010|HY010|  
|--[3]|--|--|--|--|--|--|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV ou SQL_HANDLE_DBC.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [3] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_DESC et le descripteur a été explicitement alloué.  
  
## <a name="sqlfreestmt"></a>SQLFreeStmt  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|AU-DESSUS DE 1 [1]|--|--|S1 [NP] S2 [p]|S1 [NP] S3 [p]|HY010|HY010|  
|AU-DESSUS DE 1 [2]|--|--|--|--|HY010|HY010|  
  
 [1] cette ligne affiche les transitions lorsque l' *option* a été SQL_CLOSE.  
  
 [2] cette ligne affiche les transitions lorsque l' *option* a été SQL_UNBIND ou SQL_RESET_PARAMS. Si l’argument *option* a été SQL_DROP et que le pilote sous-jacent est un pilote ODBC 3 *. x* , le gestionnaire de pilotes mappe ce à un appel à **SQLFreeHandle** avec *comme HandleType* défini sur SQL_HANDLE_STMT. Pour plus d’informations, consultez la table de transition pour [SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md).  
  
## <a name="sqlgetconnectattr"></a>SQLGetConnectAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetcursorname"></a>SQLGetCursorName  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|--|--|HY010|HY010|  
  
## <a name="sqlgetdata"></a>SQLGetData  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24 000|Voir le tableau suivant|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlgetdata-cursor-states"></a>SQLGetData (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24 000|--[s] ou [NF] S11 [x] 24000 [b] HY109 [i]|--[s] ou [NF] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec"></a>SQLGetDescField et SQLGetDescRec  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] ou [2] HY010 [3]|Voir le tableau suivant|--[1] ou [2] 24000 [3]|--[1], [2], ou [3] S11 [3] et [x]|HY010|NS [c] ou [4] HY010 [o] et [5]|  
  
 [1] l’argument *DescriptorHandle* était un APD ou un ARD.  
  
 [2] l’argument *DescriptorHandle* était un IPD.  
  
 [3] l’argument *DescriptorHandle* était un IRD.  
  
 [4] l’argument *DescriptorHandle* était le même que l’argument *DescriptorHandle* dans la fonction **SQLGetDescField** ou **SQLGetDescRec** qui s’exécute de façon asynchrone.  
  
 [5] l’argument *DescriptorHandle* était différent de l’argument *DescriptorHandle* dans la fonction **SQLGetDescField** ou **SQLGetDescRec** qui s’exécute de façon asynchrone.  
  
## <a name="sqlgetdescfield-and-sqlgetdescrec-prepared-states"></a>SQLGetDescField et SQLGetDescRec (États préparés)  
  
|S2<br /><br /> Aucun résultat|S3<br /><br /> Résultats|  
|-----------------------|--------------------|  
|--[1], [2], ou [3] S11 [2] et [x]|--[1], [2] ou [3] S11 [x]|  
  
 [1] l’argument *DescriptorHandle* était un APD ou un ARD.  
  
 [2] l’argument *DescriptorHandle* était un IPD.  
  
 [3] l’argument *DescriptorHandle* était un IRD. Notez que ces fonctions retournent toujours SQL_NO_DATA dans l’État S2 quand *DescriptorHandle* était un IRD.  
  
## <a name="sqlgetdiagfield-and-sqlgetdiagrec"></a>SQLGetDiagField et SQLGetDiagRec  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--|--|--|  
|AU-DESSUS DE 1 [2]|--[3]|--[3]|--|--|--[3]|--[3]|  
  
 [1] cette ligne montre les transitions lorsque *comme HandleType* a été SQL_HANDLE_ENV, SQL_HANDLE_DBC ou SQL_HANDLE_DESC.  
  
 [2] cette ligne affiche les transitions lorsque *comme HandleType* a été SQL_HANDLE_STMT.  
  
 [3]   **SQLGetDiagField** retourne toujours une erreur dans cet état quand *DiagIdentifier* est SQL_DIAG_ROW_COUNT.  
  
## <a name="sqlgetenvattr"></a>SQLGetEnvAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetfunctions"></a>SQLGetFunctions  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetinfo"></a>SQLGetInfo  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlgetstmtattr"></a>SQLGetStmtAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1] 24000 [2]|--[1] 24000 [2]|--[1] 24000 [2]|Voir le tableau suivant|HY010|HY010|  
  
 [1] l’attribut d’instruction n’a pas été SQL_ATTR_ROW_NUMBER.  
  
 [2] l’attribut d’instruction a été SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlgetstmtattr-cursor-states"></a>SQLGetStmtAttr (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|--[1] 24000 [2]|--[1] ou ([v] et [2]) 24000 [b] et [2] HY109 [i] et [2]|--[i] ou ([v] et [2]) 24000 [b] et [2] HY109 [1] et [2]|  
  
 [1] l’argument d' *attribut* n’a pas été SQL_ATTR_ROW_NUMBER.  
  
 [2] l’argument d' *attribut* a été SQL_ATTR_ROW_NUMBER.  
  
## <a name="sqlmoreresults"></a>SQLMoreResults  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--[1]|--[1]|--[s] et [2] S1 [NF], [NP] et [4] S2 [NF], [p] et [4] S5 [s] et [3] S11 [x]|S1 [NF], [NP] et [4] S3 [NF], [p] et [4] S4 [s] et [2] S5 [s] et [3] S11 [x]|HY010|NS [c] HY010 [o]|  
  
 [1] la fonction retourne toujours SQL_NO_DATA dans cet État.  
  
 [2] le résultat suivant est un nombre de lignes.  
  
 [3] le résultat suivant est un jeu de résultats.  
  
 [4] le résultat actuel est le dernier résultat.  
  
## <a name="sqlnativesql"></a>SQLNativeSql  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--|--|--|--|--|--|--|  
  
## <a name="sqlnumparams"></a>SQLNumParams  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlnumresultcols"></a>SQLNumResultCols  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|--[s] S11 [x]|--[s] S11 [x]|--[s] S11 [x]|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata"></a>SQLParamData  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|Voir le tableau suivant|NS [c] HY010 [o]|  
  
## <a name="sqlparamdata-need-data-states"></a>SQLParamData (nécessite des États de données)  
  
|S8<br /><br /> Données nécessaires|S9<br /><br /> Doit placer|S10<br /><br /> Peut mettre|  
|----------------------|---------------------|---------------------|  
|S1 [e] et [1] S2 [e], [Nr] et [2] S3 [e], [r] et [2] S5 [e] et [4] S6 [e] et [5] S7 [e] et [3] S9 [d] S11 [x]|HY010|S1 [e] et [1] S2 [e], [Nr], et [2] S3 [e], [r] et [2] S4 [s], [Nr] et ([1] ou [2]) S5 [s], [r] et ([1] ou [2]) S5 ([s] ou [e]) et [4] S6 ([s] ou [e]) et [5] S7 ([s] ou [e]) et [3] S9 [d] S11 [x]|  
  
 [1]   **SQLExecDirect** a retourné SQL_NEED_DATA.  
  
 [2]   **SQLExecute** a retourné SQL_NEED_DATA.  
  
 [3]   **SQLSetPos** a été appelé à partir de l’État S7 et a retourné SQL_NEED_DATA.  
  
 [4]   **SQLBulkOperations** a été appelé à partir de l’État S5 et a retourné SQL_NEED_DATA.  
  
 [5]   **SQLSetPos** ou **SQLBulkOperations** a été appelé à partir de l’État S6 et a retourné SQL_NEED_DATA.  
  
## <a name="sqlprepare"></a>SQLPrepare  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|S2 [s] et [Nr] S3 [s] et [r] S11 [x]|--[s] ou ([e] et [1]) S1 [e] et [2] S11 [x]|S1 [e] et [3] S2 [s], [Nr] et [3] S3 [s], [r] et [3] S11 [x] et [3] 24000 [4]|Voir le tableau suivant|HY010|NS [c] HY010 [o]|  
  
 [1] la préparation échoue pour une raison autre que la validation de l’instruction (la valeur SQLSTATE était HY009 [valeur d’argument non valide] ou HY090 [longueur de chaîne ou de mémoire tampon non valide]).  
  
 [2] la préparation échoue lors de la validation de l’instruction (le SQLSTATE n’est pas HY009 [valeur d’argument non valide] ou HY090 [longueur de chaîne ou de mémoire tampon non valide]).  
  
 [3] le résultat actuel est le dernier ou le seul résultat, ou il n’y a aucun résultat actuel. Pour plus d’informations sur les résultats multiples, consultez [résultats multiples](../../../odbc/reference/develop-app/multiple-results.md).  
  
 [4] le résultat actuel n’est pas le dernier résultat.  
  
## <a name="sqlprepare-cursor-states"></a>SQLPrepare (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24 000|24 000|24 000|  
  
## <a name="sqlputdata"></a>SQLPutData  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|HY010|HY010|Voir le tableau suivant|NS [c] HY010 [o]|  
  
## <a name="sqlputdata-need-data-states"></a>SQLPutData (États requis des données)  
  
|S8<br /><br /> Données nécessaires|S9<br /><br /> Doit placer|S10<br /><br /> Peut mettre|  
|----------------------|---------------------|---------------------|  
|HY010|S1 [e] et [1] S2 [e], [Nr] et [2] S3 [e], [r] et [2] S5 [e] et [4] S6 [e] et [5] S7 [e] et [3] S10 [s] S11 [x]|--[s] S1 [e] et [1] S2 [e], [Nr] et [2] S3 [e], [r] et [2] S5 [e] et [4] S6 [e] et [5] S7 [e] et [3] S11 [x] HY011 [6]|  
  
 [1]   **SQLExecDirect** a retourné SQL_NEED_DATA.  
  
 [2]   **SQLExecute** a retourné SQL_NEED_DATA.  
  
 [3]   **SQLSetPos** a été appelé à partir de l’État S7 et a retourné SQL_NEED_DATA.  
  
 [4]   **SQLBulkOperations** a été appelé à partir de l’État S5 et a retourné SQL_NEED_DATA.  
  
 [5]   **SQLSetPos** ou **SQLBulkOperations** a été appelé à partir de l’État S6 et a retourné SQL_NEED_DATA.  
  
 [6] un ou plusieurs appels à **SQLPutData** pour un seul paramètre retourné SQL_SUCCESS, puis un appel à **SQLPutData** a été effectué pour le même paramètre avec *StrLen_Or_Ind* défini sur SQL_NULL_DATA.  
  
## <a name="sqlrowcount"></a>SQLRowCount  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|--|--|HY010|HY010|  
  
## <a name="sqlsetconnectattr"></a>SQLSetConnectAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|--[1]|--|--|--|--[2] 24000 [3]|HY010|HY010|  
  
 [1] cette ligne montre les transitions quand l' *attribut* était un attribut de connexion. Pour les transitions lorsque l' *attribut* est un attribut d’instruction, consultez la table de transition d’instruction pour **SQLSetStmtAttr**.  
  
 [2] l’argument d' *attribut* n’a pas été SQL_ATTR_CURRENT_CATALOG.  
  
 [3] l’argument d' *attribut* a été SQL_ATTR_CURRENT_CATALOG.  
  
## <a name="sqlsetcursorname"></a>SQLSetCursorName  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--|24 000|24 000|HY010|HY010|  
  
## <a name="sqlsetdescfield-and-sqlsetdescrec"></a>SQLSetDescField et SQLSetDescRec  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|AU-DESSUS DE 1 [1]|--|--|--|--|HY010|HY010|  
  
 [1] cette ligne montre les transitions où l’argument *DescriptorHandle* est un ARD, APD, IPD ou (pour **SQLSETDESCFIELD**) un IRD lorsque l’argument *FieldIdentifier* est SQL_DESC_ARRAY_STATUS_PTR ou SQL_DESC_ROWS_PROCESSED_PTR. L’appel de **SQLSetDescField** pour un IRD lorsque *FieldIdentifier* est une autre valeur est une erreur.  
  
## <a name="sqlsetenvattr"></a>SQLSetEnvAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|HY011|HY011|HY011|HY011|Y011|HY01|HY011|  
  
## <a name="sqlsetpos"></a>SQLSetPos  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|HY010|HY010|24 000|Voir le tableau suivant|HY010|NS [c] HY010 [o]|  
  
## <a name="sqlsetpos-cursor-states"></a>SQLSetPos (États du curseur)  
  
|S5<br /><br /> Ouvert|S6<br /><br /> SQLFetch ou SQLFetchScroll|S7<br /><br /> SQLExtendedFetch|  
|-------------------|---------------------------------------|-----------------------------|  
|24 000|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|--[s] S8 [d] S11 [x] 24000 [b] HY109 [i]|  
  
## <a name="sqlsetstmtattr"></a>SQLSetStmtAttr  
  
|S0<br /><br /> Non alloué|S1<br /><br /> Allocated|S2-S3<br /><br /> Prepared|S4<br /><br /> Effectuées|S5-S7<br /><br /> Curseur|S8-S10<br /><br /> Données nécessaires|S11-S12<br /><br /> Async|  
|------------------------|----------------------|------------------------|---------------------|----------------------|--------------------------|-----------------------|  
|IH|--|--[1] HY011 [2]|--[1] 24000 [2]|--[1] 24000 [2]|HY010 [NP] ou [1] HY011 [p] et [2]|HY010 [NP] ou [1] HY011 [p] et [2]|  
  
 [1] l’argument d' *attribut* n’était pas SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE ou SQL_ATTR_CURSOR_SENSITIVITY.  
  
 [2] l’argument d' *attribut* était SQL_ATTR_CONCURRENCY, SQL_ATTR_CURSOR_TYPE, SQL_ATTR_SIMULATE_CURSOR, SQL_ATTR_USE_BOOKMARKS, SQL_ATTR_CURSOR_SCROLLABLE ou SQL_ATTR_CURSOR_SENSITIVITY.
