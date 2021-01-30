---
description: SQLSetCursorName, fonction
title: SQLSetCursorName fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetCursorName
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetCursorName
helpviewer_keywords:
- SQLSetCursorName function [ODBC]
ms.assetid: 4e055946-12d4-4589-9891-41617a50f34e
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 2141a6ee259e51080d018af32bd0c433a1725e1e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208099"
---
# <a name="sqlsetcursorname-function"></a>SQLSetCursorName, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLSetCursorName** associe un nom de curseur à une instruction active. Si une application n’appelle pas **SQLSetCursorName**, le pilote génère des noms de curseur en fonction des besoins pour le traitement des instructions SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSetCursorName(  
     SQLHSTMT      StatementHandle,  
     SQLCHAR *     CursorName,  
     SQLSMALLINT   NameLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *CursorName*  
 Entrée Nom du curseur. Pour un traitement efficace, le nom du curseur ne doit pas inclure d’espaces de début ou de fin dans le nom du curseur, et si le nom du curseur inclut un identificateur délimité, le délimiteur doit être positionné en tant que premier caractère dans le nom du curseur.  
  
 *NameLength*  
 Entrée Longueur en caractères de **CursorName*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSetCursorName** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLSetCursorName** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le nom du curseur dépasse la limite maximale, donc seul le nombre maximal autorisé de caractères a été utilisé.|  
|24 000|État de curseur non valide|L’instruction correspondant à *StatementHandle* se trouvait déjà dans un état d’exécution ou positionné sur un curseur.|  
|34000|Nom de curseur non valide|Le nom de curseur spécifié dans **CursorName* n’était pas valide, car il a dépassé la longueur maximale définie par le pilote, ou il a démarré avec « SQLCUR » ou « SQL_CUR ».|  
|3C000|Nom de curseur dupliqué|Le nom de curseur spécifié dans **CursorName* existe déjà.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *CursorName* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction aynchronous était toujours en cours d’exécution lors de l’appel de la fonction **SQLSetCursorName** .<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) l’argument *NameLength* est inférieur à 0 mais n’est pas égal à SQL_NTS.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Les noms de curseurs sont utilisés uniquement dans les instructions Update et DELETE positionnées (par exemple, **Update** _table-Name_ ... **Où Current de** _Cursor-Name_). Pour plus d’informations, consultez [instructions Update et DELETE positionnées](../../../odbc/reference/develop-app/positioned-update-and-delete-statements.md). Si l’application n’appelle pas **SQLSetCursorName** pour définir un nom de curseur, lors de l’exécution d’une instruction de requête, le pilote génère un nom qui commence par les lettres SQL_CUR et ne dépasse pas 18 caractères.  
  
 Tous les noms de curseurs dans la connexion doivent être uniques. La longueur maximale d’un nom de curseur est définie par le pilote. Pour une interopérabilité maximale, il est recommandé que les applications limitent les noms de curseur à 18 caractères. Dans ODBC 3 *. x*, si un nom de curseur est un identificateur entre guillemets, il est traité en respectant la casse et peut contenir des caractères que la syntaxe de SQL n’autorise pas ou traitera de manière spéciale, comme des espaces ou des mots clés réservés. Si un nom de curseur doit être traité de façon sensible à la casse, il doit être passé comme identificateur entre guillemets.  
  
 Un nom de curseur défini de manière explicite ou implicite reste défini jusqu’à ce que l’instruction à laquelle il est associé soit supprimée à l’aide de **SQLFreeHandle**. **SQLSetCursorName** peut être appelé pour renommer un curseur sur une instruction tant que le curseur se trouve dans un État alloué ou préparé.  
  
## <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application utilise **SQLSetCursorName** pour définir un nom de curseur pour une instruction. Il utilise ensuite cette instruction pour récupérer les résultats de la table CUSTOMers. Enfin, il effectue une mise à jour positionnée pour modifier le numéro de téléphone de John Smith. Notez que l’application utilise différents descripteurs d’instruction pour les instructions **Select** et **Update** .  
  
 Pour obtenir un autre exemple de code, consultez [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp  
#define NAME_LEN 50  
#define PHONE_LEN 10  
  
SQLHSTMT     hstmtSelect,  
SQLHSTMT     hstmtUpdate;  
SQLRETURN    retcode;  
SQLHDBC      hdbc;  
SQLCHAR      szName[NAME_LEN], szPhone[PHONE_LEN];  
SQLINTEGER   cbName, cbPhone;  
  
/* Allocate the statements and set the cursor name. */  
  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtSelect);  
SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmtUpdate);  
SQLSetCursorName(hstmtSelect, "C1", SQL_NTS);  
  
/* SELECT the result set and bind its columns to local buffers. */  
  
SQLExecDirect(hstmtSelect,  
      "SELECT NAME, PHONE FROM CUSTOMERS",  
      SQL_NTS);  
SQLBindCol(hstmtSelect, 1, SQL_C_CHAR, szName, NAME_LEN, &cbName);  
SQLBindCol(hstmtSelect, 2, SQL_C_CHAR, szPhone, PHONE_LEN, &cbPhone);  
  
/* Read through the result set until the cursor is */  
/* positioned on the row for John Smith. */  
  
do  
 retcode = SQLFetch(hstmtSelect);  
while ((retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) &&  
   (strcmp(szName, "Smith, John") != 0));  
  
/* Perform a positioned update of John Smith's name. */  
  
if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
   SQLExecDirect(hstmtUpdate,  
   "UPDATE EMPLOYEE SET PHONE=\"2064890154\" WHERE CURRENT OF C1",  
   SQL_NTS);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Renvoi d’un nom de curseur|[SQLGetCursorName Function](../../../odbc/reference/syntax/sqlgetcursorname-function.md)|  
|Définition des options de défilement du curseur|[SQLSetScrollOptions, fonction](../../../odbc/reference/syntax/sqlsetscrolloptions-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
