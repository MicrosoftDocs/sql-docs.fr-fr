---
description: Fonction SQLEndTran
title: SQLEndTran fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLEndTran
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLEndTran
helpviewer_keywords:
- SQLEndTran function [ODBC]
ms.assetid: ff375ce1-eb50-4693-b1e6-70181a6dbf9f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2961cab7c061583f68387e29ed564efc16916870
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195597"
---
# <a name="sqlendtran-function"></a>Fonction SQLEndTran
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLEndTran** demande une opération de validation ou de restauration pour toutes les opérations actives sur toutes les instructions associées à une connexion. **SQLEndTran** peut également demander qu’une opération de validation ou de restauration soit effectuée pour toutes les connexions associées à un environnement.  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon dont le gestionnaire de pilotes mappe cette fonction quand ODBC 3. l’application *x* fonctionne avec ODBC 2. *x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLEndTran(  
     SQLSMALLINT   HandleType,  
     SQLHANDLE     Handle,  
     SQLSMALLINT   CompletionType);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Identificateur du type de handle. Contient soit SQL_HANDLE_ENV *(si le handle est* un handle d’environnement), soit SQL_HANDLE_DBC (si le *handle* est un handle de connexion).  
  
 *Handle*  
 Entrée Handle, du type indiqué par *comme HandleType*, indiquant la portée de la transaction. Pour plus d’informations, consultez « commentaires ».  
  
 *CompletionType*  
 Entrée Une des deux valeurs suivantes :  
  
 SQL_COMMIT SQL_ROLLBACK  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLEndTran** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec le *comme HandleType* et le *handle* appropriés. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLEndTran** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08003|Connexion non ouverte|(DM) le *comme HandleType* a été SQL_HANDLE_DBC et le *handle* n’était pas dans un état connecté.|  
|08007|Échec de connexion pendant la transaction|Le *comme HandleType* a été SQL_HANDLE_DBC, et la connexion associée au *descripteur* a échoué pendant l’exécution de la fonction, et il ne peut pas être déterminé si la **validation** ou la **restauration** demandée se sont produites avant la défaillance.|  
|25S01|État de transaction inconnu|Une ou plusieurs des connexions dans *handle* n’ont pas pu terminer la transaction avec le résultat spécifié, et le résultat est inconnu.|  
|25S02|La transaction est toujours active|Le pilote n’a pas pu garantir que tout le travail dans la transaction globale pouvait être effectué de manière atomique et que la transaction est toujours active.|  
|25S03|La transaction est restaurée|Le pilote n’a pas pu garantir que tout le travail dans la transaction globale pouvait être effectué de manière atomique, et tout le travail de la transaction actif dans le *descripteur* a été annulé.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40002|Violation de contrainte d’intégrité|Le *CompletionType* a été SQL_COMMIT et l’engagement des modifications a entraîné une violation de contrainte d’intégrité. Par conséquent, la transaction a été restaurée.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* szMessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *ConnectionHandle*. La fonction a été appelée et avant la fin de l’exécution de la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) a été appelée sur *ConnectionHandle*. Ensuite, la fonction a été appelée à nouveau sur le *ConnectionHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution de **SQLCancelHandle** a été appelée sur le *ConnectionHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour un descripteur d’instruction associé à *ConnectionHandle* et était toujours en cours d’exécution lors de l’appel de **SQLEndTran** .<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour un descripteur d’instruction associé à *ConnectionHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *handle* avec *comme HandleType* ayant la valeur SQL_HANDLE_DBC et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des handles d’instruction associés au *handle* et retournés SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY012|Code d’opération de transaction non valide|(DM) la valeur spécifiée pour l’argument *CompletionType* n’était ni SQL_COMMIT ni SQL_ROLLBACK.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY092|Identificateur d’attribut/option non valide|(DM) la valeur spécifiée pour l’argument *comme HandleType* n’était ni SQL_HANDLE_ENV ni SQL_HANDLE_DBC.|  
|HY115|SQLEndTran n’est pas autorisé pour un environnement qui contient une connexion avec l’exécution de fonctions asynchrones activée|(DM) *comme HandleType* ne peut pas avoir la valeur SQL_HANDLE_ENV si l’exécution asynchrone des fonctions de connexion est activée pour une connexion dans l’environnement.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, reportez-vous à la section commentaires de cette rubrique.|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge l’opération de **restauration** .|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *ConnectionHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Pour ODBC 3. *x* , si *comme HandleType* est SQL_HANDLE_ENV et *handle* est un handle d’environnement valide, le gestionnaire de pilote appellera **SQLEndTran** dans chaque pilote associé à l’environnement. L’argument de *handle* pour l’appel à un pilote est le descripteur d’environnement du pilote. Pour ODBC 2. *x* , si *comme HandleType* est SQL_HANDLE_ENV et *handle* est un handle d’environnement valide et qu’il existe plusieurs connexions dans un état connecté dans cet environnement, le gestionnaire de pilotes appellera **SQLTransact** dans le pilote une fois pour chaque connexion dans un état connecté dans cet environnement. L’argument *handle* dans chaque appel sera le handle de la connexion. Dans les deux cas, le pilote tente de valider ou de restaurer les transactions, en fonction de la valeur de *CompletionType*, sur toutes les connexions qui sont dans un état connecté sur cet environnement. Les connexions qui ne sont pas actives n’affectent pas la transaction.  
  
> [!NOTE]  
>  **SQLEndTran** ne peut pas être utilisé pour valider ou restaurer des transactions dans un environnement partagé. SQLSTATE HY092 (identificateur d’attribut/option non valide) est retourné si **SQLEndTran** est appelé avec un *handle* défini sur le handle d’un environnement partagé ou sur le handle d’une connexion sur un environnement partagé.  
  
 Le gestionnaire de pilotes renverra SQL_SUCCESS uniquement s’il reçoit des SQL_SUCCESS pour chaque connexion. Si le gestionnaire de pilotes reçoit des SQL_ERROR sur une ou plusieurs connexions, il retourne SQL_ERROR à l’application, et les informations de diagnostic sont placées dans la structure de données de diagnostic de l’environnement. Pour déterminer la connexion ou les connexions qui ont échoué pendant l’opération de validation ou de restauration, l’application peut appeler **SQLGetDiagRec** pour chaque connexion.  
  
> [!NOTE]  
>  Le gestionnaire de pilotes ne simule pas une transaction globale pour toutes les connexions et n’utilise donc pas les protocoles de validation en deux phases.  
  
 Si *CompletionType* est SQL_COMMIT, **SQLEndTran** émet une demande de validation pour toutes les opérations actives sur toute instruction associée à une connexion affectée. Si *CompletionType* est SQL_ROLLBACK, **SQLEndTran** émet une demande de restauration pour toutes les opérations actives sur toute instruction associée à une connexion affectée. Si aucune transaction n’est active, **SQLEndTran** retourne SQL_SUCCESS sans effet sur les sources de données. Pour plus d’informations, consultez [validation et annulation de transactions](../../../odbc/reference/develop-app/committing-and-rolling-back-transactions.md).  
  
 Si le pilote est en mode de validation manuelle (en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_AUTOCOMMIT défini sur SQL_AUTOCOMMIT_OFF), une nouvelle transaction est démarrée implicitement quand une instruction SQL qui peut être contenue dans une transaction est exécutée sur la source de données actuelle. Pour plus d’informations, consultez [mode de validation](../../../odbc/reference/develop-app/commit-mode.md).  
  
 Pour déterminer comment les opérations de transaction affectent les curseurs, une application appelle **SQLGetInfo** avec les options SQL_CURSOR_ROLLBACK_BEHAVIOR et SQL_CURSOR_COMMIT_BEHAVIOR. Pour plus d’informations, consultez les paragraphes suivants et observez également les [effets des transactions sur les curseurs et les instructions préparées](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md).  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égale à SQL_CB_DELETE, **SQLEndTran** ferme et supprime tous les curseurs ouverts sur toutes les instructions associées à la connexion et ignore tous les résultats en attente. **SQLEndTran** laisse toute instruction présente dans un État alloué (non préparé); l’application peut les réutiliser pour des demandes SQL ultérieures ou appeler **SQLFreeStmt** ou **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_STMT pour les libérer.  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égale à SQL_CB_CLOSE, **SQLEndTran** ferme tous les curseurs ouverts sur toutes les instructions associées à la connexion. **SQLEndTran** laisse toute instruction présente dans un état préparé ; l’application peut appeler **SQLExecute** pour une instruction associée à la connexion sans appeler d’abord **SQLPrepare**.  
  
 Si la valeur SQL_CURSOR_ROLLBACK_BEHAVIOR ou SQL_CURSOR_COMMIT_BEHAVIOR est égale à SQL_CB_PRESERVE, **SQLEndTran** n’affecte pas les curseurs ouverts associés à la connexion. Les curseurs restent à la ligne vers laquelle ils sont dirigés avant l’appel à **SQLEndTran**.  
  
 Pour les pilotes et les sources de données qui prennent en charge les transactions, l’appel de **SQLEndTran** avec SQL_COMMIT ou SQL_ROLLBACK quand aucune transaction n’est active retourne SQL_SUCCESS (indiquant qu’il n’y a aucun travail à engager ou à restaurer) et n’a aucun effet sur la source de données.  
  
 Lorsqu’un pilote est en mode de validation automatique, le gestionnaire de pilotes n’appelle pas **SQLEndTran** dans le pilote. **SQLEndTran** retourne toujours SQL_SUCCESS qu’il soit appelé avec un *CompletionType* de SQL_COMMIT ou SQL_ROLLBACK.  
  
 Les pilotes ou les sources de données qui ne prennent pas en charge les transactions ( *option* **SQLGetInfo** SQL_TXN_CAPABLE est SQL_TC_NONE) sont en fait toujours en mode de validation automatique et, par conséquent, retournent toujours SQL_SUCCESS pour **SQLEndTran** , qu’elles soient ou non appelées avec un *CompletionType* de SQL_COMMIT ou SQL_ROLLBACK. Ces pilotes et sources de données n’annulent pas réellement les transactions lorsqu’ils sont invités à le faire.  
  
## <a name="suspended-state"></a>État suspendu  
 Dans les gestionnaires de pilotes qui ont été publiés avant Windows 7, une transaction était active si **SQLEndTran** retournait SQL_ERROR à partir du pilote. Toutefois, il était possible que la transaction ait été validée avec succès sur le serveur, mais que le pilote sur le client n’ait pas été notifié (par exemple, une erreur réseau s’est produite). Cela laisse la connexion dans un état incorrect. À compter de Windows 7, quand **SQLEndTran** retourne SQL_ERROR, la connexion peut être dans un état suspendu. Dans un état suspendu, il est possible d’appeler des fonctions en lecture seule. Finalement, l’application doit appeler **SQLDisconnect** sur une connexion interrompue pour libérer des ressources.  
  
 Si toutes les conditions suivantes sont remplies, la connexion est mise dans un état suspendu :  
  
-   Le pilote retourne SQL_ERROR à partir de **SQLEndTran**.  
  
-   Le pilote est ODBC version 3,8 ou ultérieure.  
  
-   La version de l’application est 3,8 ou une version ultérieure ; ou l’application ODBC 2. x ou 3. x recompilée annule avec succès la fonction **SQLEndTran** via **SQLCancelHandle**.  
  
-   Le pilote n’a pas retourné l’un des messages suivants, qui confirment que la transaction n’a pas été effectuée :  
  
    -   25S03 : la transaction est restaurée  
  
    -   40001 : échec de la sérialisation  
  
    -   40002 : contrainte d’intégrité  
  
    -   HYC00 : fonctionnalité facultative non implémentée  
  
 Si **SQLEndTran** a été appelé sur un handle d’environnement et que l’une de ses connexions répond aux conditions ci-dessus, toutes les connexions qui se connectent au même pilote sont mises dans l’État Suspended.  
  
 Une fois qu’une application a appelé **SQLDisconnect** sur une connexion interrompue, la connexion peut être utilisée pour se reconnecter à une autre source de données ou à la même source de données.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Annulation d’une fonction qui s’exécute de façon asynchrone sur un handle de connexion.|[SQLCancelHandle, fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md)|  
|Retour d’informations sur un pilote ou une source de données|[Fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md)|  
|Libération d’un descripteur|[Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Libération d’un descripteur d’instruction|[Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md)
