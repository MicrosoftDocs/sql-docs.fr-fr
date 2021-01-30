---
description: Fonction SQLPutData
title: SQLPutData, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLPutData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLPutData
helpviewer_keywords:
- SQLPutData function [ODBC]
ms.assetid: 9a60f004-1477-4c54-a20c-7378e1116713
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8f76a3624222968aabf91f9aff012e9370aa8ac8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192555"
---
# <a name="sqlputdata-function"></a>Fonction SQLPutData
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLPutData** permet à une application d’envoyer des données pour un paramètre ou une colonne au pilote au moment de l’exécution de l’instruction. Cette fonction peut être utilisée pour envoyer des valeurs de données de type caractère ou binaire en parties à une colonne avec un type de données caractère, binaire ou spécifique à la source de données (par exemple, les paramètres des types de SQL_LONGVARBINARY ou de SQL_LONGVARCHAR). **SQLPutData** prend en charge la liaison à un type de données C Unicode, même si le pilote sous-jacent ne prend pas en charge les données Unicode.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLPutData(  
      SQLHSTMT     StatementHandle,  
      SQLPOINTER   DataPtr,  
      SQLLEN       StrLen_or_Ind);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *DataPtr*  
 Entrée Pointeur vers une mémoire tampon contenant les données réelles du paramètre ou de la colonne. Les données doivent être dans le type de données C spécifié dans l’argument *ValueType* de **SQLBindParameter** (pour les données de paramètre) ou l’argument *TargetType* de **SQLBindCol** (pour les données de colonne).  
  
 *StrLen_or_Ind*  
 Entrée Longueur de \* *DataPtr*. Spécifie la quantité de données envoyées dans un appel à **SQLPutData**. La quantité de données peut varier avec chaque appel pour un paramètre ou une colonne donné (e). *StrLen_Or_Ind* est ignoré, sauf si l’une des conditions suivantes est satisfaite :  
  
-   *StrLen_Or_Ind* est SQL_NTS, SQL_NULL_DATA ou SQL_DEFAULT_PARAM.  
  
-   Le type de données C spécifié dans **SQLBindParameter** ou **SQLBindCol** est SQL_C_CHAR ou SQL_C_BINARY.  
  
-   Le type de données C est SQL_C_DEFAULT, et le type de données C par défaut pour le type de données SQL spécifié est SQL_C_CHAR ou SQL_C_BINARY.  
  
 Pour tous les autres types de données C, *si StrLen_Or_Ind* n’est pas SQL_NULL_DATA ou SQL_DEFAULT_PARAM, le pilote suppose que la taille de \* la mémoire tampon *DataPtr* est la taille du type de données C spécifié avec *ValueType* ou *TargetType* et envoie la valeur de données entière. Pour plus d’informations, consultez [conversion de données de C en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) dans l’annexe D : types de données.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLPutData** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLPutData** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données binaires ou de chaîne retournées pour un paramètre de sortie ont entraîné la troncation d’un caractère non vide ou de données binaires non NULL. S’il s’agit d’une valeur de chaîne, elle a été tronquée à droite. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur de données identifiée par l’argument *ValueType* dans **SQLBindParameter** pour le paramètre lié n’a pas pu être convertie dans le type de données identifié par l’argument *ParameterType* dans **SQLBindParameter**.|  
|07S01|Utilisation non valide du paramètre par défaut|Une valeur de paramètre, définie avec **SQLBindParameter**, a été SQL_DEFAULT_PARAM, et le paramètre correspondant n’a pas de valeur par défaut.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22001|Chaîne de données, troncation à droite|L’affectation d’une valeur de type caractère ou binaire à une colonne a entraîné la troncation de caractères ou d’octets non vides (caractères) ou non null (binaires).<br /><br /> Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y » et davantage de données ont été envoyées pour un paramètre long (le type de données était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un long type de données spécifique à la source de données) que celui spécifié avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**.<br /><br /> Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y » et d’autres données ont été envoyées pour une longue colonne (le type de données était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données long spécifique à la source de données) que celui spécifié dans la mémoire tampon de longueur correspondant à une colonne dans une ligne de données qui a été ajoutée ou mise à jour avec **SQLBulkOperations** ou mise à jour avec **SQLSetPos**.|  
|22003|Valeur numérique hors limites|Les données envoyées pour un paramètre numérique ou une colonne lié ont provoqué la troncature de la partie entière (par opposition à la fraction) du nombre à tronquer lorsqu’elle est assignée à la colonne de table associée.<br /><br /> Le retour d’une valeur numérique (sous forme de valeur numérique ou de chaîne) pour un ou plusieurs paramètres d’entrée/sortie ou de sortie a entraîné la troncation de la partie entière (par opposition à la fraction) du nombre à tronquer.|  
|22007|Format de date/heure non valide|Les données envoyées pour un paramètre ou une colonne qui était liée à une structure de date, d’heure ou d’horodatage était, respectivement, une date, une heure ou un horodateur non valide.<br /><br /> Un paramètre d’entrée/sortie ou de sortie a été lié à une structure de date, d’heure ou d’horodatage C, et une valeur dans le paramètre retourné était, respectivement, une date, une heure ou un horodateur non valide. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|22008|Dépassement du champ DateTime|Une expression DateTime calculée pour un paramètre d’entrée/sortie ou de sortie a entraîné la non-validité d’une valeur de date, d’heure ou d’horodatage.|  
|22012|Division par zéro|Une expression arithmétique calculée pour un paramètre d’entrée/sortie ou de sortie a entraîné une division par zéro.|  
|22015|Dépassement du champ d’intervalle|Les données envoyées pour une colonne ou un paramètre de type numeric ou Interval exact à un type de données SQL Interval ont provoqué une perte de chiffres significatifs.<br /><br /> Des données ont été envoyées pour une colonne ou un paramètre d’intervalle contenant plusieurs champs, a été converti en type de données numérique et n’a pas de représentation dans le type de données numérique.<br /><br /> Les données envoyées pour les données de colonne ou de paramètre ont été affectées à un type SQL Interval, et il n’existait aucune représentation de la valeur du type C dans le type SQL Interval.<br /><br /> Les données envoyées pour une colonne ou un paramètre d’un type ou d’un paramètre de type c exact à un type d’intervalle C ont provoqué une perte de chiffres significatifs.<br /><br /> Les données envoyées pour les données de colonne ou de paramètre ont été affectées à une structure d’intervalle C, et il n’existait aucune représentation des données dans la structure des données d’intervalle.|  
|22018|Valeur de caractère non valide pour la spécification de cast|Le type C était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type SQL de la colonne était un type de données caractère ; et la valeur dans la colonne ou le paramètre n’était pas un littéral valide du type C lié.<br /><br /> Le type SQL était un type de données numérique exact ou approximatif, DateTime ou Interval ; le type C a été SQL_C_CHAR ; et la valeur de la colonne ou du paramètre n’est pas un littéral valide du type SQL lié.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *DataPtr* était un pointeur null, et l’argument *StrLen_Or_Ind* n’était pas 0, SQL_DEFAULT_PARAM ou SQL_NULL_DATA.|  
|HY010|Erreur de séquence de fonction|(DM) l’appel de fonction précédent n’était pas un appel à **SQLPutData** ou **SQLParamData**.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction SQLPutData.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY019|Données non-caractères et non binaires envoyées en plusieurs parties|**SQLPutData** a été appelé plusieurs fois pour un paramètre ou une colonne, et n’est pas utilisé pour envoyer des données de caractères c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données, ou pour envoyer des données c binaires à une colonne avec un type de données caractère, binaire ou spécifique à la source de données.|  
|HY020|Tentative de concaténation d’une valeur null|**SQLPutData** a été appelé plusieurs fois depuis l’appel retourné SQL_NEED_DATA, et dans l’un de ces appels, l’argument *StrLen_Or_Ind* contient SQL_NULL_DATA ou SQL_DEFAULT_PARAM.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’argument *DataPtr* n’était pas un pointeur null, et l’argument *StrLen_Or_Ind* était inférieur à 0, mais n’est pas égal à SQL_NTS ou SQL_NULL_DATA.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
 Si **SQLPutData** est appelé lors de l’envoi de données pour un paramètre dans une instruction SQL, il peut retourner tout SQLSTATE pouvant être retourné par la fonction appelée pour exécuter l’instruction (**SQLExecute** ou **SQLExecDirect**). Si elle est appelée lors de l’envoi de données pour une colonne en cours de mise à jour ou d’ajout avec **SQLBulkOperations** ou si elle est mise à jour avec **SQLSetPos**, elle peut renvoyer n’importe quel SQLSTATE pouvant être retourné par **SQLBulkOperations** ou **SQLSetPos**.  
  
## <a name="comments"></a>Commentaires  
 **SQLPutData** peut être appelé pour fournir des données en cours d’exécution pour deux utilisations : les données de paramètre à utiliser dans un appel à **SQLExecute** ou **SQLExecDirect**, ou les données de colonne à utiliser lorsqu’une ligne est mise à jour ou ajoutée par un appel à **SQLBulkOperations** ou est mise à jour par un appel à **SQLSetPos**.  
  
 Quand une application appelle **SQLParamData** pour déterminer les données à envoyer, le pilote retourne un indicateur que l’application peut utiliser pour déterminer les données de paramètre à envoyer ou l’emplacement où les données de colonne peuvent être trouvées. Elle retourne également SQL_NEED_DATA, qui est un indicateur pour l’application qu’elle doit appeler **SQLPutData** pour envoyer les données. Dans l’argument *DataPtr* de **SQLPutData**, l’application passe un pointeur vers la mémoire tampon contenant les données réelles du paramètre ou de la colonne.  
  
 Lorsque le pilote retourne SQL_SUCCESS pour **SQLPutData**, l’application appelle **SQLParamData** à nouveau. **SQLParamData** retourne SQL_NEED_DATA si davantage de données doivent être envoyées, auquel cas l’application appelle **SQLPutData** à nouveau. Elle retourne SQL_SUCCESS si toutes les données en cours d’exécution ont été envoyées. L’application appelle ensuite **SQLParamData** à nouveau. Si le pilote retourne SQL_NEED_DATA et un autre indicateur dans *\* ValuePtrPtr*, il a besoin de données pour un autre paramètre ou une autre colonne et **SQLPutData** est à nouveau appelé. Si le pilote retourne SQL_SUCCESS, toutes les données en cours d’exécution ont été envoyées et l’instruction SQL peut être exécutée ou l’appel **SQLBulkOperations** ou **SQLSetPos** peut être traité.  
  
 Pour plus d’informations sur la façon dont les données de paramètre de données en cours d’exécution sont transmises au moment de l’exécution de l’instruction, consultez « transmission des valeurs de paramètres » dans [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) et [envoi de données de type long](../../../odbc/reference/develop-app/sending-long-data.md). Pour plus d’informations sur la mise à jour ou l’ajout des données de colonne de données en cours d’exécution, consultez la section « utilisation de SQLSetPos » dans [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), « exécution de mises à jour en bloc à l’aide de signets » dans [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)et [long Data, SQLSetPos et SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
> [!NOTE]  
>  Une application peut utiliser **SQLPutData** pour envoyer des données dans des parties uniquement lors de l’envoi de données de caractères c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données, ou lors de l’envoi de données binaires c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données. Si **SQLPutData** est appelé plus d’une fois dans toutes les autres conditions, il retourne SQL_ERROR et SQLSTATE HY019 (données non-caractères et non binaires envoyées en plusieurs parties).  
  
## <a name="example"></a>Exemple  
 L’exemple suivant suppose un nom de source de données nommé test. La base de données associée doit avoir une table que vous pouvez créer, comme suit :  
  
```sql  
CREATE TABLE emp4 (NAME char(30), AGE int, BIRTHDAY datetime, Memo1 text)  
```  
  
```cpp  
// SQLPutData.cpp  
// compile with: odbc32.lib user32.lib  
#include <stdio.h>  
#include <windows.h>  
#include <sqlext.h>  
#include <odbcss.h>  
  
#define TEXTSIZE  12000  
#define MAXBUFLEN 256  
  
SQLHENV henv = SQL_NULL_HENV;  
SQLHDBC hdbc1 = SQL_NULL_HDBC;       
SQLHSTMT hstmt1 = SQL_NULL_HSTMT;  
  
void Cleanup() {  
   if (hstmt1 != SQL_NULL_HSTMT)  
      SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
  
   if (hdbc1 != SQL_NULL_HDBC) {  
      SQLDisconnect(hdbc1);  
      SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   }  
  
   if (henv != SQL_NULL_HENV)  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
  
int main() {  
   RETCODE retcode;  
  
   // SQLBindParameter variables.  
   SQLLEN cbTextSize, lbytes;  
  
   // SQLParamData variable.  
   PTR pParmID;  
  
   // SQLPutData variables.  
   UCHAR  Data[] =   
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyzabcdefghijklmnopqrstuvwxyz"  
      "abcdefghijklmnopqrstuvwxyz";  
  
   SDWORD cbBatch = (SDWORD)sizeof(Data) - 1;  
  
   // Allocate the ODBC environment and save handle.  
   retcode = SQLAllocHandle (SQL_HANDLE_ENV, NULL, &henv);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(Env) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Notify ODBC that this is an ODBC 3.0 app.  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER) SQL_OV_ODBC3, SQL_IS_INTEGER);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLSetEnvAttr(ODBC version) Failed\n\n");  
      Cleanup();  
      return(9);      
   }  
  
   // Allocate ODBC connection handle and connect.  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc1);  
   if ( (retcode != SQL_SUCCESS_WITH_INFO) && (retcode != SQL_SUCCESS)) {  
      printf("SQLAllocHandle(hdbc1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Sample uses Integrated Security, create SQL Server DSN using Windows NT authentication.   
   retcode = SQLConnect(hdbc1, (UCHAR*)"Test", SQL_NTS, (UCHAR*)"",SQL_NTS, (UCHAR*)"", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLConnect() Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Allocate statement handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc1, &hstmt1);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLAllocHandle(hstmt1) Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Set parameters based on total data to send.  
   lbytes = (SDWORD)TEXTSIZE;  
   cbTextSize = SQL_LEN_DATA_AT_EXEC(lbytes);  
  
   // Bind the parameter marker.  
   retcode = SQLBindParameter (hstmt1,           // hstmt  
                               1,                // ipar  
                               SQL_PARAM_INPUT,  // fParamType  
                               SQL_C_CHAR,       // fCType  
                               SQL_LONGVARCHAR,  // FSqlType  
                               lbytes,           // cbColDef  
                               0,                // ibScale  
                               (VOID *)1,        // rgbValue  
                               0,                // cbValueMax  
                               &cbTextSize);     // pcbValue  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLBindParameter Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Execute the command.  
   retcode =   
      SQLExecDirect(hstmt1, (UCHAR*)"INSERT INTO emp4 VALUES('Paul Borm', 46,'1950-11-12 00:00:00', ?)", SQL_NTS);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_NEED_DATA) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLExecDirect Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Check to see if NEED_DATA; if yes, use SQLPutData.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if (retcode == SQL_NEED_DATA) {  
      while (lbytes > cbBatch) {  
         SQLPutData(hstmt1, Data, cbBatch);  
         lbytes -= cbBatch;  
      }  
      // Put final batch.  
      retcode = SQLPutData(hstmt1, Data, lbytes);   
   }  
  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Make final SQLParamData call.  
   retcode = SQLParamData(hstmt1, &pParmID);  
   if ( (retcode != SQL_SUCCESS) && (retcode != SQL_SUCCESS_WITH_INFO) ) {  
      printf("Final SQLParamData Failed\n\n");  
      Cleanup();  
      return(9);  
   }  
  
   // Clean up.  
   SQLFreeHandle(SQL_HANDLE_STMT, hstmt1);  
   SQLDisconnect(hdbc1);  
   SQLFreeHandle(SQL_HANDLE_DBC, hdbc1);  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Retour du paramètre suivant pour l’envoi de données|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
