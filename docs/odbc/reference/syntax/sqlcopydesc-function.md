---
description: SQLCopyDesc, fonction
title: SQLCopyDesc fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLCopyDesc
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLCopyDesc
helpviewer_keywords:
- SQLCopyDesc function [ODBC]
ms.assetid: d5450895-3824-44c4-8aa4-d4f9752a9602
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ab2ef18e777d433daf4ab93b73d249e62507ea3c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206306"
---
# <a name="sqlcopydesc-function"></a>SQLCopyDesc, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLCopyDesc** copie les informations de descripteur d’un handle de descripteur vers un autre.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLCopyDesc(  
     SQLHDESC     SourceDescHandle,  
     SQLHDESC     TargetDescHandle);  
```  
  
## <a name="arguments"></a>Arguments  
 *SourceDescHandle*  
 Entrée Handle du descripteur source.  
  
 *TargetDescHandle*  
 Entrée Handle du descripteur cible. L’argument *TargetDescHandle* peut être un handle vers un descripteur d’application ou un IPD. *TargetDescHandle* ne peut pas être défini sur un handle d’IRD, ou **SQLCOPYDESC** retourne SQLState HY016 (impossible de modifier un descripteur de ligne d’implémentation).  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLCopyDesc** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DESC et un *handle* de *TargetDescHandle*. Si un *SourceDescHandle* non valide a été passé dans l’appel, SQL_INVALID_HANDLE est retourné, mais aucun SQLSTATE n’est retourné. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLCopyDesc** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
 Lorsqu’une erreur est retournée, l’appel à **SQLCopyDesc** est immédiatement abandonné et le contenu des champs du descripteur *TargetDescHandle* n’est pas défini.  
  
 Étant donné que **SQLCopyDesc** peut être implémenté en appelant **SQLGetDescField** et **SQLSetDescField**, **SQLCopyDesc** peut retourner des sqlstates retournés par **SQLGetDescField** ou **SQLSetDescField**.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY007|L’instruction associée n’est pas préparée|*SourceDescHandle* a été associé à un IRD et le descripteur d’instruction associé n’était pas dans l’État prepared ou Executed.|  
|HY010|Erreur de séquence de fonction|(DM) le handle de descripteur dans *SourceDescHandle* ou *TargetDescHandle* a été associé à un *StatementHandle* pour lequel une fonction d’exécution asynchrone (pas celui-ci) a été appelée et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) le handle de descripteur dans *SourceDescHandle* ou *TargetDescHandle* a été associé à un *StatementHandle* pour lequel **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *SourceDescHandle* ou *TargetDescHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLCopyDesc** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des descripteurs d’instruction associés à *SourceDescHandle* ou *TargetDescHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY016|Impossible de modifier un descripteur de ligne d’implémentation|*TargetDescHandle* a été associé à un IRD.|  
|HY021|Informations de descripteur incohérentes|Les informations de descripteur vérifiées pendant une vérification de cohérence n’étaient pas cohérentes. Pour plus d’informations, consultez « vérifications de cohérence » dans **SQLSetDescField**.|  
|HY092|Identificateur d’attribut/option non valide|L’appel à **SQLCopyDesc** invite un appel à **SQLSetDescField**, mais *\* ValuePtr* n’était pas valide pour l’argument *FieldIdentifier* sur *TargetDescHandle*.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé à *SourceDescHandle* ou *TargetDescHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Un appel à **SQLCopyDesc** copie les champs du handle du descripteur source vers le handle du descripteur cible. Les champs peuvent être copiés uniquement vers un descripteur d’application ou un IPD, mais pas vers un IRD. Les champs peuvent être copiés à partir d’une application ou d’un descripteur d’implémentation.  
  
 Les champs ne peuvent être copiés à partir d’un IRD que si le descripteur d’instruction est à l’état préparé ou exécuté ; dans le cas contraire, la fonction retourne SQLSTATE HY007 (l’instruction associée n’est pas préparée).  
  
 Les champs peuvent être copiés à partir d’une IPD, qu’une instruction ait ou non été préparée. Si une instruction SQL avec des paramètres dynamiques a été préparée et que le remplissage automatique de l’IPD est pris en charge et activé, l’IPD est remplie par le pilote. Lorsque **SQLCopyDesc** est appelé avec l’IPD comme *SourceDescHandle*, les champs remplis sont copiés. Si l’IPD n’est pas remplie par le pilote, le contenu des champs à l’origine dans l’IPD est copié.  
  
 Tous les champs du descripteur, à l’exception de SQL_DESC_ALLOC_TYPE (qui spécifie si le handle de descripteur a été alloué automatiquement ou explicitement), sont copiés, que le champ soit défini ou non pour le descripteur de destination. Les champs copiés remplacent les champs existants.  
  
 Le pilote copie tous les champs de descripteur si les arguments *SourceDescHandle* et *TargetDescHandle* sont associés au même pilote, même si les pilotes se trouvent sur deux connexions ou environnements différents. Si les arguments *SourceDescHandle* et *TargetDescHandle* sont associés à différents pilotes, le gestionnaire de pilotes copie les champs définis par ODBC, mais ne copie pas les champs ou les champs définis par le pilote qui ne sont pas définis par ODBC pour le type de descripteur.  
  
 L’appel à **SQLCopyDesc** est immédiatement abandonné si une erreur se produit.  
  
 Lorsque le champ SQL_DESC_DATA_PTR est copié, une vérification de cohérence est effectuée sur le descripteur cible. Si la vérification de cohérence échoue, les informations SQLSTATE HY021 (informations de descripteur incohérentes) sont retournées et l’appel à **SQLCopyDesc** est immédiatement abandonné. Pour plus d’informations sur les vérifications de cohérence, consultez « vérifications de cohérence » dans la [fonction SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 Les handles de descripteur peuvent être copiés sur les connexions même si les connexions se trouvent dans des environnements différents. Si le gestionnaire de pilotes détecte que les handles de descripteur source et de destination n’appartiennent pas à la même connexion et que les deux connexions appartiennent à des pilotes distincts, il implémente **SQLCopyDesc** en effectuant une copie champ par champ à l’aide de **SQLGetDescField** et **SQLSetDescField**.  
  
 Lorsque **SQLCopyDesc** est appelé avec un *SourceDescHandle* sur un pilote et un *TargetDescHandle* sur un autre pilote, la file d’attente des erreurs du *SourceDescHandle* est effacée. Cela est dû au fait que **SQLCopyDesc** dans ce cas est implémenté par les appels à **SQLGetDescField** et **SQLSetDescField**.  
  
> [!NOTE]  
>  Une application peut être en mesure d’associer un handle de descripteur explicitement alloué à un *StatementHandle*, plutôt que d’appeler **SQLCopyDesc** pour copier des champs d’un descripteur vers un autre. Un descripteur alloué explicitement peut être associé à un autre *StatementHandle* sur le même *ConnectionHandle* en affectant au SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC attribut d’instruction le descripteur du descripteur alloué explicitement. Dans ce cas, **SQLCopyDesc** ne doit pas être appelé pour copier des valeurs de champ de descripteur d’un descripteur à un autre. Toutefois, un handle de descripteur ne peut pas être associé à un *StatementHandle* sur un autre *ConnectionHandle*. pour utiliser les mêmes valeurs de champ de descripteur sur *StatementHandles* sur différents *ConnectionHandles*, **SQLCopyDesc** doit être appelé.  
  
 Pour obtenir une description des champs dans un en-tête ou un enregistrement de descripteur, consultez [fonction SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="copying-rows-between-tables"></a>Copie de lignes entre des tables  
 Une application peut copier des données d’une table vers une autre sans copier les données au niveau de l’application. Pour ce faire, l’application lie les mêmes mémoires tampons de données et informations de descripteur à une instruction qui extrait les données et l’instruction qui insère les données dans une copie. Pour ce faire, vous pouvez partager un descripteur d’application (en liant un descripteur explicitement alloué comme ARD à une instruction et APD dans un autre) ou en utilisant **SQLCopyDesc** pour copier les liaisons entre les ARD et les APD des deux instructions. Si les instructions se trouvent sur des connexions différentes, **SQLCopyDesc** doit être utilisé. En outre, **SQLCopyDesc** doit être appelé pour copier les liaisons entre les IRD et la IPD des deux instructions. Lors de la copie à travers des instructions sur la même connexion, le type d’informations SQL_ACTIVE_STATEMENTS retourné par le pilote pour un appel à **SQLGetInfo** doit être supérieur à 1 pour que cette opération aboutisse. (Ce n’est pas le cas lors de la copie sur des connexions.)  
  
### <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, les opérations de descripteur sont utilisées pour copier les champs de la table PartsSource dans la table PartsCopy. Le contenu de la table PartsSource est extrait dans les tampons d’ensemble de lignes dans *hstmt0*. Ces valeurs sont utilisées comme paramètres d’une instruction INSERT sur *hstmt1* pour remplir les colonnes de la table PartsCopy. Pour ce faire, les champs de la IRD de *hstmt0* sont copiés dans les champs de l’IPD de *hstmt1* et les champs de la ARD de *hstmt0* sont copiés dans les champs de la APD de *hstmt1*. Utilisez **SQLSetDescField** pour définir l’attribut SQL_DESC_PARAMETER_TYPE de l’IPD sur SQL_PARAM_INPUT lorsque vous copiez des champs IRD à partir d’une instruction avec des paramètres de sortie vers des champs IPD qui doivent être des paramètres d’entrée.  
  
```cpp  
#define ROWS 100  
#define DESC_LEN 50  
#define SQL_SUCCEEDED(rc) (rc == SQL_SUCCESS || rc == SQL_SUCCESS_WITH_INFO)  
  
// Template for a row  
typedef struct {  
   SQLINTEGER   sPartID;  
   SQLINTEGER   cbPartID;  
   SQLUCHAR     szDescription[DESC_LENGTH];  
   SQLINTEGER   cbDescription;  
   REAL         sPrice;  
   SQLINTEGER   cbPrice;  
} PartsSource;  
  
PartsSource    rget[ROWS];          // rowset buffer  
SQLUSMALLINT   sts_ptr[ROWS];       // status pointer  
SQLHSTMT       hstmt0, hstmt1;  
SQLHDESC       hArd0, hIrd0, hApd1, hIpd1;  
  
// ARD and IRD of hstmt0  
SQLGetStmtAttr(hstmt0, SQL_ATTR_APP_ROW_DESC, &hArd0, 0, NULL);  
SQLGetStmtAttr(hstmt0, SQL_ATTR_IMP_ROW_DESC, &hIrd0, 0, NULL);  
  
// APD and IPD of hstmt1  
SQLGetStmtAttr(hstmt1, SQL_ATTR_APP_PARAM_DESC, &hApd1, 0, NULL);  
SQLGetStmtAttr(hstmt1, SQL_ATTR_IMP_PARAM_DESC, &hIpd1, 0, NULL);  
  
// Use row-wise binding on hstmt0 to fetch rows  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_BIND_TYPE, (SQLPOINTER) sizeof(PartsSource), 0);  
  
// Set rowset size for hstmt0  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_ARRAY_SIZE, (SQLPOINTER) ROWS, 0);  
  
// Execute a select statement  
SQLExecDirect(hstmt0, "SELECT PARTID, DESCRIPTION, PRICE FROM PARTS ORDER BY 3, 1, 2"",  
               SQL_NTS);  
  
// Bind  
SQLBindCol(hstmt0, 1, SQL_C_SLONG, rget[0].sPartID, 0,   
   &rget[0].cbPartID);  
SQLBindCol(hstmt0, 2, SQL_C_CHAR, &rget[0].szDescription, DESC_LEN,   
   &rget[0].cbDescription);  
SQLBindCol(hstmt0, 3, SQL_C_FLOAT, rget[0].sPrice,   
   0, &rget[0].cbPrice);  
  
// Perform parameter bindings on hstmt1.   
SQLCopyDesc(hArd0, hApd1);  
SQLCopyDesc(hIrd0, hIpd1);  
  
// Set the array status pointer of IRD  
SQLSetStmtAttr(hstmt0, SQL_ATTR_ROW_STATUS_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the ARRAY_STATUS_PTR field of APD to be the same  
// as that in IRD.  
SQLSetStmtAttr(hstmt1, SQL_ATTR_PARAM_OPERATION_PTR, sts_ptr, SQL_IS_POINTER);  
  
// Set the hIpd1 records as input parameters  
rc = SQLSetDescField(hIpd1, 1, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 2, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
rc = SQLSetDescField(hIpd1, 3, SQL_DESC_PARAMETER_TYPE, (SQLPOINTER)SQL_PARAM_INPUT, SQL_IS_INTEGER);  
  
// Prepare an insert statement on hstmt1. PartsCopy is a copy of  
// PartsSource  
SQLPrepare(hstmt1, "INSERT INTO PARTS_COPY VALUES (?, ?, ?)", SQL_NTS);  
  
// In a loop, fetch a rowset, and copy the fetched rowset to PARTS_COPY  
  
rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
while (SQL_SUCCEEDED(rc)) {  
  
   // After the call to SQLFetchScroll, the status array has row   
   // statuses. This array is used as input status in the APD  
   // and hence determines which elements of the rowset buffer  
   // are inserted.  
   SQLExecute(hstmt1);  
  
   rc = SQLFetchScroll(hstmt0, SQL_FETCH_NEXT, 0);  
} // while  
```  
  
### <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition d’un champ de descripteur unique|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition de plusieurs champs de descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
