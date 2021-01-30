---
description: Fonction SQLMoreResults
title: Fonction SQLMoreResults | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLMoreResults
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLMoreResults
helpviewer_keywords:
- SQLMoreResults function [ODBC]
ms.assetid: bf169ed5-4d55-412c-b184-12065a726e89
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: d894cc8cf87a3db794bed763c9bffc596910299a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160794"
---
# <a name="sqlmoreresults-function"></a>Fonction SQLMoreResults
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLMoreResults** détermine si davantage de résultats sont disponibles sur une instruction contenant des instructions **Select**, **Update**, **Insert** ou **Delete** et, le cas échéant, initialise le traitement de ces résultats.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLMoreResults(  
     SQLHSTMT     StatementHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE OU SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLMoreResults** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLMoreResults** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|La valeur de l’option a changé|La valeur d’un attribut d’instruction a changé lors du traitement du lot. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction **SQLMoreResults** a été appelée et, avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. La fonction **SQLMoreResults** a été appelée à nouveau sur *StatementHandle*.<br /><br /> La fonction **SQLMoreResults** a été appelée et, avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLMoreResults** .<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
## <a name="comments"></a>Commentaires  
 Les instructions **Select** retournent des jeux de résultats. Les instructions **Update**, **Insert** et **Delete** retournent un nombre de lignes affectées. Si l’une de ces instructions est regroupée, soumise avec des tableaux de paramètres (numérotés dans l’ordre des paramètres d’incrémentation, dans l’ordre dans lequel ils apparaissent dans le traitement), ou dans les procédures, ils peuvent retourner plusieurs jeux de résultats ou nombres de lignes. Pour plus d’informations sur les lots d’instructions et les tableaux de paramètres, consultez [lots d’instructions SQL](../../../odbc/reference/develop-app/batches-of-sql-statements.md) et de [tableaux de valeurs de paramètre](../../../odbc/reference/develop-app/arrays-of-parameter-values.md).  
  
 Après l’exécution du traitement, l’application est positionnée sur le premier jeu de résultats. L’application peut appeler **SQLBindCol**, **SQLBulkOperations**, **SQLFetch**, **SQLGetData**, **SQLFetchScroll**, **SQLSetPos** et toutes les fonctions de métadonnées, sur le premier ou dans tous les jeux de résultats suivants, exactement comme s’il s’agissait d’un seul jeu de résultats. Une fois le premier jeu de résultats terminé, l’application appelle **SQLMoreResults** pour passer au jeu de résultats suivant. Si un autre jeu de résultats ou nombre est disponible, **SQLMoreResults** retourne SQL_SUCCESS et initialise le jeu de résultats ou le nombre de traitements supplémentaires. Si des instructions de génération de nombre de lignes s’affichent entre les instructions de génération de jeu de résultats, elles peuvent être exécutées en escalier en appelant **SQLMoreResults**. Après l’appel de **SQLMoreResults** pour les instructions **Update**, **Insert** ou **Delete** , une application peut appeler **SQLRowCount**.  
  
 En présence d’un jeu de résultats avec des lignes non récupérées, **SQLMoreResults** ignore ce jeu de résultats et rend le jeu de résultats suivant ou le nombre disponible. Si tous les résultats ont été traités, **SQLMoreResults** retourne SQL_NO_DATA. Pour certains pilotes, les paramètres de sortie et les valeurs de retour ne sont pas disponibles tant que tous les jeux de résultats et nombre de lignes n’ont pas été traités. Pour ces pilotes, les paramètres de sortie et les valeurs de retour deviennent disponibles lorsque **SQLMoreResults** retourne SQL_NO_DATA.  
  
 Toutes les liaisons établies pour le jeu de résultats précédent restent valides. Si les structures de colonne sont différentes pour ce jeu de résultats, l’appel de **SQLFetch** ou **SQLFetchScroll** peut entraîner une erreur ou une troncation. Pour éviter cela, l’application doit appeler **SQLBindCol** pour se reconnecter explicitement comme il convient (ou le faire en définissant des champs de descripteur). L’application peut également appeler **SQLFreeStmt** avec une *option* de SQL_UNBIND pour dissocier toutes les mémoires tampons de colonne.  
  
 Les valeurs des attributs d’instruction, telles que le type de curseur, l’accès concurrentiel du curseur, la taille du jeu de clés ou la longueur maximale, peuvent changer à mesure que l’application parcourt le lot par le biais d’appels à **SQLMoreResults**. Dans ce cas, **SQLMoreResults** retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01s02 ne (la valeur d’option a changé).  
  
 L’appel de **SQLCloseCursor** ou **SQLFreeStmt** avec l' *option* SQL_CLOSE, ignore tous les jeux de résultats et nombre de lignes qui étaient disponibles suite à l’exécution du traitement. Le descripteur d’instruction retourne à l’État alloué ou préparé. L’appel de **SQLCancel** pour annuler une fonction s’exécutant de façon asynchrone lorsqu’un lot a été exécuté et que le descripteur d’instruction se trouve dans l’État exécuté, positionné sur un curseur ou asynchrone entraîne la suppression de tous les jeux de résultats et de tous les nombres de lignes générés par le lot si l’appel d’annulation a réussi. L’instruction retourne alors à l’État prepared ou allocated.  
  
 Si un lot d’instructions ou une procédure mélange d’autres instructions SQL avec des instructions **Select**, **Update**, **Insert** et **Delete** , ces autres instructions n’affectent pas **SQLMoreResults**.  
  
 Pour plus d’informations, consultez [plusieurs résultats](../../../odbc/reference/develop-app/multiple-results.md).  
  
 Si une instruction Update, INSERT ou DELETE recherchée dans un lot d’instructions n’affecte aucune ligne de la source de données, **SQLMoreResults** retourne SQL_SUCCESS. Cela est différent du cas d’une instruction Update, INSERT ou DELETE recherchée qui est exécutée par le biais de **SQLExecDirect**, **SQLExecute** ou **SQLParamData**, qui retourne SQL_NO_DATA s’il n’affecte pas les lignes de la source de données. Si une application appelle **SQLRowCount** pour récupérer le nombre de lignes après qu’un appel à **SQLMoreResults** n’a affecté aucune ligne, **SQLRowCount** retourne SQL_NO_DATA.  
  
 Pour plus d’informations sur le séquencement valide des fonctions de traitement des résultats, consultez [annexe B : tables de transition d’État ODBC](../../../odbc/reference/appendixes/appendix-b-odbc-state-transition-tables.md).  
  
 Pour plus d’informations sur les SQL_PARAM_DATA_AVAILABLE et les paramètres de sortie diffusés en continu, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="availability-of-row-counts"></a>Disponibilité des nombres de lignes  
 Lorsqu’un lot contient plusieurs instructions consécutives de génération du nombre de lignes, il est possible que ces nombres de lignes soient cumulés dans un seul nombre de lignes. Par exemple, si un lot contient cinq instructions INSERT, certaines sources de données peuvent retourner cinq nombres de lignes individuels. Certaines autres sources de données ne retournent qu’un seul nombre de lignes qui représente la somme des cinq nombres de lignes individuels.  
  
 Lorsqu’un lot contient une combinaison d’instructions de génération de jeu de résultats et de génération de nombre de lignes, le nombre de lignes peut être ou ne pas être disponible du tout. Le comportement du pilote en ce qui concerne la disponibilité des nombres de lignes est énuméré dans le type d’informations SQL_BATCH_ROW_COUNT disponible via un appel à **SQLGetInfo**. Supposons, par exemple, que le lot contient une **instruction SELECT**, suivie de deux **instructions INSERT** et d’une autre instruction **Select**. Les cas suivants sont possibles :  
  
-   Les nombres de lignes correspondant aux deux instructions **Insert** ne sont pas disponibles du tout. Le premier appel à **SQLMoreResults** se positionnera sur le jeu de résultats de la deuxième instruction **Select** .  
  
-   Les nombres de lignes correspondant aux deux instructions **Insert** sont disponibles individuellement. (Un appel à **SQLGetInfo** ne retourne pas le bit SQL_BRC_ROLLED_UP pour le type d’informations SQL_BATCH_ROW_COUNT.) Le premier appel à **SQLMoreResults** se positionnera sur le nombre de lignes de la première **insertion**, et le deuxième appel se positionnera sur le nombre de lignes de la deuxième **insertion**. Le troisième appel à **SQLMoreResults** se positionnera sur le jeu de résultats de la deuxième instruction **Select** .  
  
-   Les nombres de lignes correspondant aux deux **insertions** sont reportés dans un seul nombre de lignes disponible. (Un appel à **SQLGetInfo** retourne le bit SQL_BRC_ROLLED_UP pour le type d’informations SQL_BATCH_ROW_COUNT.) Le premier appel à **SQLMoreResults** se positionnera sur le nombre de lignes cumulées, et le deuxième appel à **SQLMoreResults** se positionnera sur le jeu de résultats de la deuxième **sélection**.  
  
 Certains pilotes rendent les nombres de lignes disponibles uniquement pour les lots explicites, et non pour les procédures stockées.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction d’une seule ligne ou d’un bloc de données dans une direction vers l’avant uniquement|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
|Extraction d’une partie ou de la totalité d’une colonne de données|[Fonction SQLGetData](../../../odbc/reference/syntax/sqlgetdata-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
