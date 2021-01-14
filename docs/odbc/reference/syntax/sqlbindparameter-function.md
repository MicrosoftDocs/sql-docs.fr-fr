---
description: Fonction SQLBindParameter
title: SQLBindParameter, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLBindParameter
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBindParameter
helpviewer_keywords:
- SQLBindParameter function [ODBC]
ms.assetid: 38349d4b-be03-46f9-9d6a-e50dd144e225
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 44b8b59722b790685b7fad47300039913d7294ae
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170151"
---
# <a name="sqlbindparameter-function"></a>Fonction SQLBindParameter

**Conformité**  
 Version introduite : ODBC 2,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLBindParameter** lie une mémoire tampon à un marqueur de paramètre dans une instruction SQL. **SQLBindParameter** prend en charge la liaison à un type de données C Unicode, même si le pilote sous-jacent ne prend pas en charge les données Unicode.  
  
> [!NOTE]  
>  Cette fonction remplace la fonction ODBC 1,0 **SQLSetParam,**. Pour plus d’informations, consultez la section « commentaires ».  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN SQLBindParameter(  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ParameterNumber,  
      SQLSMALLINT     InputOutputType,  
      SQLSMALLINT     ValueType,  
      SQLSMALLINT     ParameterType,  
      SQLULEN         ColumnSize,  
      SQLSMALLINT     DecimalDigits,  
      SQLPOINTER      ParameterValuePtr,  
      SQLLEN          BufferLength,  
      SQLLEN *        StrLen_or_IndPtr);  
```
  
## <a name="arguments"></a>Arguments

 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ParameterNumber*  
 Entrée Numéro de paramètre, ordonné séquentiellement dans l’ordre croissant des paramètres, à partir de 1.  
  
 *InputOutputType*  
 Entrée Type du paramètre. Pour plus d’informations, consultez « argument *InputOutputType* » dans « Comments ».  
  
 *ValueType*  
 Entrée Type de données C du paramètre. Pour plus d’informations, consultez « argument *ValueType* » dans « Comments ».  
  
 *ParameterType*  
 Entrée Type de données SQL du paramètre. Pour plus d’informations, consultez « argument *ParameterType* » dans « Comments ».  
  
 *ColumnSize*  
 Entrée Taille de la colonne ou de l’expression du marqueur de paramètre correspondant. Pour plus d’informations, consultez «*Column* , argument » dans « Comments ».  
  
 Si votre application s’exécute sur un système d’exploitation Windows 64 bits, consultez [informations ODBC 64](../../../odbc/reference/odbc-64-bit-information.md)bits.  
  
 *DecimalDigits*  
 Entrée Chiffres décimaux de la colonne ou de l’expression du marqueur de paramètre correspondant. Pour plus d’informations sur la taille des colonnes, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
 *ParameterValuePtr*  
 [Entrée différée] Pointeur vers une mémoire tampon pour les données du paramètre. Pour plus d’informations, consultez « argument *ParameterValuePtr* » dans « Comments ».  
  
 *BufferLength*  
 [Entrée/sortie] Longueur en octets de la mémoire tampon *ParameterValuePtr* . Pour plus d’informations, consultez « argument *BufferLength* » dans « Comments ».  
  
 Consultez [ODBC 64-informations sur ODBC](../../../odbc/reference/odbc-64-bit-information.md), si votre application s’exécute sur un système d’exploitation 64 bits.  
  
 *StrLen_or_IndPtr*  
 [Entrée différée] Pointeur vers une mémoire tampon pour la longueur du paramètre. Pour plus d’informations, consultez «*StrLen_or_IndPtr* argument » dans « Comments ».  
  
## <a name="returns"></a>Retours

 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics

 Lorsque **SQLBindParameter** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLBindParameter** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  

|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|Le type de données identifié par l’argument *ValueType* ne peut pas être converti dans le type de données identifié par l’argument *ParameterType* . Notez que cette erreur peut être retournée par **SQLExecDirect**, **SQLExecute** ou **SQLPutData** au moment de l’exécution, plutôt que par **SQLBindParameter**.|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour l’argument *ParameterNumber* est inférieure à 1.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans le tampon **MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY003|Type de tampon d’application non valide|La valeur spécifiée par l’argument *ValueType* n’est pas un type de données C valide ou SQL_C_DEFAULT.|  
|HY004|Type de données SQL non valide|La valeur spécifiée pour l’argument *ParameterType* n’est ni un identificateur de type de données SQL ODBC valide, ni un identificateur de type de données SQL spécifique au pilote pris en charge par le pilote.|  
|HY009|Valeur d’argument non valide|(DM) l’argument *ParameterValuePtr* était un pointeur null, l’argument *StrLen_or_IndPtr* était un pointeur null et l’argument *InputOutputType* n’était pas SQL_PARAM_OUTPUT.<br /><br /> (DM) SQL_PARAM_OUTPUT, où l’argument *ParameterValuePtr* était un pointeur null, le type C était char ou Binary, et BufferLength (*cbValueMax*) était supérieur à 0.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de **SQLBindParameter** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY021|Informations de descripteur incohérentes|Les informations de descripteur vérifiées pendant une vérification de cohérence n’étaient pas cohérentes. (Consultez la section « vérifications de cohérence » dans **SQLSetDescField**.)<br /><br /> La valeur spécifiée pour l’argument *DecimalDigits* se trouvait en dehors de la plage de valeurs prises en charge par la source de données pour une colonne du type de données SQL spécifié par l’argument *ParameterType* .|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur de *BufferLength* est inférieure à 0. (Consultez la description du champ SQL_DESC_DATA_PTR dans **SQLSetDescField**.)|  
|HY104|Valeur de précision ou d’échelle non valide|La valeur spécifiée pour l’argument *Column* ou *DecimalDigits* se trouvait en dehors de la plage de valeurs prises en charge par la source de données pour une colonne du type de données SQL spécifié par l’argument *ParameterType* .|  
|HY105|Type de paramètre non valide|(DM) la valeur spécifiée pour l’argument *InputOutputType* n’était pas valide. (Voir « commentaires ».)|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ou la source de données ne prend pas en charge la conversion spécifiée par la combinaison de la valeur spécifiée pour l’argument *ValueType* et de la valeur spécifique au pilote spécifiée pour l’argument *ParameterType*.<br /><br /> La valeur spécifiée pour l’argument *ParameterType* était un identificateur de type de données SQL ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote ou la source de données.<br /><br /> Le pilote prend en charge uniquement ODBC 2. *x* et l’argument *ValueType* était l’un des éléments suivants :<br /><br /> SQL_C_NUMERIC SQL_C_SBIGINT SQL_C_UBIGINT<br /><br /> et tous les types de données Interval C listés dans les [types de données c](../../../odbc/reference/appendixes/c-data-types.md) de l’annexe D : types de données.<br /><br /> Le pilote ne prend en charge que les versions ODBC antérieures à 3,50, et l’argument *ValueType* était SQL_C_GUID.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires

 Une application appelle **SQLBindParameter** pour lier chaque marqueur de paramètre dans une instruction SQL. Les liaisons restent effectives jusqu’à ce que l’application appelle à nouveau **SQLBindParameter** , appelle **SQLFreeStmt** avec l’option SQL_RESET_PARAMS ou appelle **SQLSetDescField** pour définir le champ d’en-tête SQL_DESC_COUNT de APD sur 0.  
  
 Pour plus d’informations sur les paramètres, consultez [paramètres d’instruction](../../../odbc/reference/develop-app/statement-parameters.md). Pour plus d’informations sur les types de données de paramètre et les marqueurs de paramètres, consultez [types de données de paramètre](../../../odbc/reference/appendixes/parameter-data-types.md) et [marqueurs](../../../odbc/reference/appendixes/parameter-markers.md) de paramètres dans l’annexe C : grammaire SQL.  
  
## <a name="parameternumber-argument"></a>Argument ParameterNumber  
 Si *ParameterNumber* dans l’appel à **SQLBindParameter** est supérieur à la valeur de SQL_DESC_COUNT, **SQLSetDescField** est appelé pour augmenter la valeur de SQL_DESC_COUNT à *ParameterNumber*.  
  
## <a name="inputoutputtype-argument"></a>Argument InputOutputType  
 L’argument *InputOutputType* spécifie le type du paramètre. Cet argument définit le champ SQL_DESC_PARAMETER_TYPE de l’IPD. Tous les paramètres des instructions SQL qui n’appellent pas de procédures, telles que des instructions **Insert** , sont des *paramètres* *d’entrée* . Les paramètres dans les appels de procédure peuvent être des paramètres d’entrée, d’entrée/sortie ou de sortie. (Une application appelle **SQLProcedureColumns** pour déterminer le type d’un paramètre dans un appel de procédure ; les paramètres dont le type ne peut pas être déterminé sont supposés être des paramètres d’entrée.)  
  
 L’argument *InputOutputType* est l’une des valeurs suivantes :  
  
-   SQL_PARAM_INPUT. Le paramètre marque un paramètre dans une instruction SQL qui n’appelle pas de procédure, telle qu’une instruction **Insert** , ou il marque un paramètre d’entrée dans une procédure. Par exemple, les paramètres dans les **valeurs d’insertion into Employee ( ?, ?, ?)** sont des paramètres d’entrée, tandis que les paramètres de **{call AddEmp ( ?, ?, ?)}** peuvent être, mais pas nécessairement, des paramètres d’entrée.  
  
     Lorsque l’instruction est exécutée, le pilote envoie des données pour le paramètre à la source de données ; la \* mémoire tampon *ParameterValuePtr* doit contenir une valeur d’entrée valide, ou la mémoire tampon **StrLen_or_IndPtr* doit contenir SQL_NULL_DATA, SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC.  
  
     Si une application ne peut pas déterminer le type d’un paramètre dans un appel de procédure, elle définit *InputOutputType* sur SQL_PARAM_INPUT ; Si la source de données retourne une valeur pour le paramètre, le pilote l’ignore.  
  
-   SQL_PARAM_INPUT_OUTPUT. Le paramètre marque un paramètre d’entrée/sortie dans une procédure. Par exemple, le paramètre de **{Call GetEmpDept ( ?)}** est un paramètre d’entrée/sortie qui accepte le nom d’un employé et renvoie le nom du service de l’employé.  
  
     Lorsque l’instruction est exécutée, le pilote envoie des données pour le paramètre à la source de données ; la \* mémoire tampon *ParameterValuePtr* doit contenir une valeur d’entrée valide, ou la \* mémoire tampon de *StrLen_or_IndPtr* doit contenir SQL_NULL_DATA, SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC. Une fois l’instruction exécutée, le pilote retourne des données pour le paramètre à l’application ; Si la source de données ne retourne pas de valeur pour un paramètre d’entrée/sortie, le pilote définit la mémoire tampon **StrLen_or_IndPtr* sur SQL_NULL_DATA.  
  
    > [!NOTE]  
    >  Quand une application ODBC 1,0 appelle **SQLSetParam,** dans un pilote ODBC 2,0, le gestionnaire de pilotes convertit ce en un appel de **SQLBindParameter** dans lequel l’argument *InputOutputType* est défini sur SQL_PARAM_INPUT_OUTPUT.  
  
-   SQL_PARAM_OUTPUT. Le paramètre marque la valeur de retour d’une procédure ou d’un paramètre de sortie dans une procédure. dans les deux cas, ceux-ci sont appelés *paramètres de sortie*. Par exemple, le paramètre de **{ ? = Call GetNextEmpID}** est un paramètre de sortie qui retourne l’ID d’employé suivant.  
  
     Une fois l’instruction exécutée, le pilote retourne des données pour le paramètre à l’application, à moins que les arguments *ParameterValuePtr* et *StrLen_or_IndPtr* soient tous deux des pointeurs null, auquel cas le pilote ignore la valeur de sortie. Si la source de données ne retourne pas de valeur pour un paramètre de sortie, le pilote définit la mémoire tampon **StrLen_or_IndPtr* sur SQL_NULL_DATA.  
  
-   SQL_PARAM_INPUT_OUTPUT_STREAM. Indique qu’un paramètre d’entrée/sortie doit être diffusé en continu. **SQLGetData** peut lire les valeurs des paramètres en plusieurs parties. *BufferLength* est ignoré, car la longueur de la mémoire tampon est déterminée lors de l’appel de **SQLGetData**. La valeur de la mémoire tampon de *StrLen_or_IndPtr* doit contenir SQL_NULL_DATA, SQL_DEFAULT_PARAM, SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC. Un paramètre doit être lié en tant que paramètre de données en cours d’exécution, s’il est diffusé en continu à la sortie. *ParameterValuePtr* peut être toute valeur de pointeur non null qui sera retournée par **SQLParamData** en tant que jeton défini par l’utilisateur dont la valeur a été passée avec *ParameterValuePtr* à la fois pour l’entrée et la sortie. Pour plus d’informations, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
-   SQL_PARAM_OUTPUT_STREAM. Identique à SQL_PARAM_INPUT_OUTPUT_STREAM, pour un paramètre de sortie. **StrLen_or_IndPtr* est ignorée au moment de l’entrée.  
  
 Le tableau suivant répertorie les différentes combinaisons de *InputOutputType* et **StrLen_or_IndPtr*:  
  
|*InputOutputType*|**StrLen_or_IndPtr*|Résultat|Remarque sur ParameterValuePtr|  
|-----------------------|----------------------------|-------------|---------------------------------|  
|SQL_PARAM_INPUT|SQL_LEN_DATA_AT_EXEC (*Len*) ou SQL_DATA_AT_EXEC|Entrée en parties|*ParameterValuePtr* peut être n’importe quelle valeur de pointeur qui sera retournée par **SQLParamData** en tant que jeton défini par l’utilisateur dont la valeur a été passée avec *ParameterValuePtr*.|  
|SQL_PARAM_INPUT|Non SQL_LEN_DATA_AT_EXEC (*Len*) ou SQL_DATA_AT_EXEC|Mémoire tampon d’entrée|*ParameterValuePtr* est l’adresse de la mémoire tampon d’entrée.|  
|SQL_PARAM_OUTPUT|Ignoré à l’entrée.|Mémoire tampon de sortie|*ParameterValuePtr* est l’adresse de la mémoire tampon de sortie.|  
|SQL_PARAM_OUTPUT_STREAM|Ignoré à l’entrée.|Sortie diffusée en continu|*ParameterValuePtr* peut être n’importe quelle valeur de pointeur, qui sera retournée par **SQLParamData** en tant que jeton défini par l’utilisateur dont la valeur a été passée avec *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|SQL_LEN_DATA_AT_EXEC (*Len*) ou SQL_DATA_AT_EXEC|Entrée dans la mémoire tampon de parties et de sortie|*ParameterValuePtr* est l’adresse de la mémoire tampon de sortie, qui est également retournée par **SQLParamData** en tant que jeton défini par l’utilisateur dont la valeur a été passée avec *ParameterValuePtr*.|  
|SQL_PARAM_INPUT_OUTPUT|Non SQL_LEN_DATA_AT_EXEC (*Len*) ou SQL_DATA_AT_EXEC|Mémoire tampon d’entrée et mémoire tampon liée de sortie|*ParameterValuePtr* est l’adresse de la mémoire tampon d’entrée/sortie partagée.|
|SQL_PARAM_INPUT_OUTPUT_STREAM|SQL_LEN_DATA_AT_EXEC (*Len*) ou SQL_DATA_AT_EXEC|Entrée en pièces et sortie en continu|*ParameterValuePtr* peut être n’importe quelle valeur de pointeur non null, qui sera retournée par **SQLParamData** en tant que jeton défini par l’utilisateur dont la valeur a été passée avec *ParameterValuePtr* à la fois pour l’entrée et la sortie.|  
  
> [!NOTE]  
>  Le pilote doit décider quels types SQL sont autorisés lorsqu’une application lie un paramètre de sortie ou d’entrée-sortie en diffusion en continu. Le gestionnaire de pilotes ne génère pas d’erreur pour un type SQL non valide.  
  
## <a name="valuetype-argument"></a>ValueType, argument

 L’argument *ValueType* spécifie le type de données C du paramètre. Cet argument définit les champs SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE de l’APD. Il doit s’agir de l’une des valeurs de la section [types de données C](../../../odbc/reference/appendixes/c-data-types.md) de l’annexe D : types de données.  
  
 Si l’argument *ValueType* est l’un des types de données Interval, le champ SQL_DESC_TYPE de l’enregistrement *ParameterNumber* du APD est défini sur SQL_INTERVAL, le champ SQL_DESC_CONCISE_TYPE du APD est défini sur le type de données intervalle concis et le champ SQL_DESC_DATETIME_INTERVAL_CODE de l’enregistrement *ParameterNumber* est défini sur un sous-code pour le type de données Interval spécifique. (Voir l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).) La précision de l’intervalle par défaut (2) et la précision de l’intervalle par défaut (6), telles que définies dans les champs SQL_DESC_DATETIME_INTERVAL_PRECISION et SQL_DESC_PRECISION de l’APD, respectivement, sont utilisées pour les données. Si la précision par défaut n’est pas appropriée, l’application doit définir explicitement le champ descripteur à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 Si l’argument *ValueType* est l’un des types de données DateTime, le champ SQL_DESC_TYPE de l’enregistrement *ParameterNumber* du APD est défini sur SQL_DATETIME, le champ SQL_DESC_CONCISE_TYPE de l’enregistrement *ParameterNumber* de APD est défini sur le type de données C DateTime concis et le champ SQL_DESC_DATETIME_INTERVAL_CODE de l’enregistrement *ParameterNumber* est défini sur un sous-code pour le type de données DateTime spécifique. (Voir l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).)  
  
 Si l’argument *ValueType* est un type de données SQL_C_NUMERIC, la précision par défaut (qui est définie par le pilote) et l’échelle par défaut (0), telles que définies dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de APD, sont utilisées pour les données. Si la précision ou l’échelle par défaut n’est pas appropriée, l’application doit définir explicitement le champ descripteur à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 SQL_C_DEFAULT spécifie que la valeur du paramètre doit être transférée à partir du type de données C par défaut pour le type de données SQL spécifié avec *ParameterType*.  
  
 Vous pouvez également spécifier un type de données C étendu. Pour plus d’informations, consultez [types de données C dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).  
  
 Pour plus d’informations, consultez types de données [c par défaut](../../../odbc/reference/appendixes/default-c-data-types.md), [conversion de données à partir de c en types de données SQL](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md)et [conversion de données de types de données SQL en c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans l’annexe D : types de données.  
  
## <a name="parametertype-argument"></a>Argument ParameterType

 Il doit s’agir de l’une des valeurs indiquées dans la section [types de données SQL](../../../odbc/reference/appendixes/sql-data-types.md) de l’annexe D : types de données, ou il doit s’agir d’une valeur spécifique au pilote. Cet argument définit les champs SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE de l’IPD.  
  
 Si l’argument *ParameterType* est l’un des identificateurs DateTime, le champ SQL_DESC_TYPE de l’IPD est défini sur SQL_DATETIME, le champ SQL_DESC_CONCISE_TYPE de l’IPD est défini sur le type de données SQL DateTime concis et le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur la valeur de sous-code DateTime appropriée.  
  
 Si *ParameterType* est l’un des identificateurs d’intervalle, le champ SQL_DESC_TYPE de l’IPD est défini sur SQL_INTERVAL, le champ SQL_DESC_CONCISE_TYPE de l’IPD est défini sur le type de données de l’intervalle SQL concis et le champ SQL_DESC_DATETIME_INTERVAL_CODE de l’IPD est défini sur le sous-code d’intervalle approprié. Le champ SQL_DESC_DATETIME_INTERVAL_PRECISION de l’IPD est défini sur la précision de début de l’intervalle, et le champ SQL_DESC_PRECISION est défini sur la précision de l’intervalle en secondes, le cas échéant. Si la valeur par défaut de SQL_DESC_DATETIME_INTERVAL_PRECISION ou SQL_DESC_PRECISION n’est pas appropriée, l’application doit la définir explicitement en appelant **SQLSetDescField**. Pour plus d’informations sur l’un de ces champs, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Si l’argument *ValueType* est un type de données SQL_NUMERIC, la précision par défaut (qui est définie par le pilote) et l’échelle par défaut (0), telles que définies dans les champs SQL_DESC_PRECISION et SQL_DESC_SCALE de l’IPD, sont utilisées pour les données. Si la précision ou l’échelle par défaut n’est pas appropriée, l’application doit définir explicitement le champ descripteur à l’aide d’un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 Pour plus d’informations sur la façon dont les données sont converties, consultez Conversion de données à [partir de c en types](../../../odbc/reference/appendixes/converting-data-from-c-to-sql-data-types.md) de données SQL et [conversion de données de données SQL en types de données c](../../../odbc/reference/appendixes/converting-data-from-sql-to-c-data-types.md) dans l’annexe D : types de données.  
  
## <a name="columnsize-argument"></a>Argument Column

 L’argument *Column* spécifie la taille de la colonne ou de l’expression qui correspond au marqueur de paramètre, la longueur de ces données, ou les deux. Cet argument définit différents champs de l’IPD, selon le type de données SQL (argument *ParameterType* ). Les règles suivantes s’appliquent à ce mappage :  
  
-   Si *ParameterType* est SQL_CHAR, SQL_VARCHAR, SQL_LONGVARCHAR, SQL_BINARY, SQL_VARBINARY, SQL_LONGVARBINARY ou l’un des types de données DateTime ou interval SQL concis, le champ SQL_DESC_LENGTH de l’IPD est défini sur la valeur de *Column*. (Pour plus d’informations, consultez la section [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) de l’annexe D : types de données.)  
  
-   Si *ParameterType* est SQL_DECIMAL, SQL_NUMERIC, SQL_FLOAT, SQL_REAL ou SQL_DOUBLE, le champ SQL_DESC_PRECISION de l’IPD est défini sur la valeur de *Column*.  
  
-   Pour les autres types de données, l’argument *Column* est ignoré.  
  
 Pour plus d’informations, consultez « transmission des valeurs de paramètres » et SQL_DATA_AT_EXEC dans «*StrLen_or_IndPtr* argument ».  
  
## <a name="decimaldigits-argument"></a>Argument DecimalDigits

 Si *ParameterType* est SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP, SQL_INTERVAL_SECOND, SQL_INTERVAL_DAY_TO_SECOND, SQL_INTERVAL_HOUR_TO_SECOND ou SQL_INTERVAL_MINUTE_TO_SECOND, le champ SQL_DESC_PRECISION de l’IPD est défini sur *DecimalDigits*. Si *ParameterType* est SQL_NUMERIC ou SQL_DECIMAL, le champ SQL_DESC_SCALE de l’IPD est défini sur *DecimalDigits*. Pour tous les autres types de données, l’argument *DecimalDigits* est ignoré.  
  
## <a name="parametervalueptr-argument"></a>Argument ParameterValuePtr

 L’argument *ParameterValuePtr* pointe vers une mémoire tampon qui, lorsque **SQLExecute** ou **SQLExecDirect** est appelé, contient les données réelles du paramètre. Les données doivent être au format spécifié par l’argument *ValueType* . Cet argument définit le champ SQL_DESC_DATA_PTR du APD. Une application peut définir l’argument *ParameterValuePtr* sur un pointeur null, à condition que *\* StrLen_or_IndPtr* soit SQL_NULL_DATA ou SQL_DATA_AT_EXEC. (Cela s’applique uniquement aux paramètres d’entrée ou d’entrée/sortie.)  
  
 Si \* *StrLen_or_IndPtr* est le résultat de la macro SQL_LEN_DATA_AT_EXEC (*longueur*) ou SQL_DATA_AT_EXEC, *ParameterValuePtr* est une valeur de pointeur définie par l’application qui est associée au paramètre. Elle est retournée à l’application via **SQLParamData**. Par exemple, *ParameterValuePtr* peut être un jeton non nul, tel qu’un numéro de paramètre, un pointeur vers des données ou un pointeur vers une structure que l’application a utilisée pour lier les paramètres d’entrée. Toutefois, Notez que si le paramètre est un paramètre d’entrée/sortie, *ParameterValuePtr* doit être un pointeur vers une mémoire tampon dans laquelle la valeur de sortie sera stockée. Si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1, l’application peut utiliser la valeur désignée par l’attribut d’instruction SQL_ATTR_PARAMS_PROCESSED_PTR avec l’argument *ParameterValuePtr* . Par exemple, *ParameterValuePtr* peut pointer vers un tableau de valeurs et l’application peut utiliser la valeur pointée par SQL_ATTR_PARAMS_PROCESSED_PTR pour récupérer la valeur correcte du tableau. Pour plus d’informations, consultez « transmission des valeurs de paramètres » plus loin dans cette section.  
  
 Si l’argument *InputOutputType* est SQL_PARAM_INPUT_OUTPUT ou SQL_PARAM_OUTPUT, *ParameterValuePtr* pointe vers une mémoire tampon dans laquelle le pilote retourne la valeur de sortie. Si la procédure retourne un ou plusieurs jeux de résultats, il n' \* est pas garanti que la mémoire tampon *ParameterValuePtr* soit définie tant que tous les jeux de résultats/nombre de lignes n’ont pas été traités. Si la mémoire tampon n’est pas définie tant que le traitement n’est pas terminé, les paramètres de sortie et les valeurs de retour ne sont pas disponibles tant que **SQLMoreResults** n’a pas retourné SQL_NO_DATA. L’appel de **SQLCloseCursor** ou **SQLFreeStmt** avec une option de SQL_CLOSE entraîne la suppression de ces valeurs.  
  
 Si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1, *ParameterValuePtr* pointe vers un tableau. Une instruction SQL unique traite le tableau complet des valeurs d’entrée pour un paramètre d’entrée ou d’entrée/sortie et retourne un tableau de valeurs de sortie pour un paramètre d’entrée/sortie ou de sortie.  
  
## <a name="bufferlength-argument"></a>Argument BufferLength

 Pour les données de type caractère et binaire, l’argument *BufferLength* spécifie la longueur de la \* mémoire tampon *ParameterValuePtr* (s’il s’agit d’un élément unique) ou la longueur d’un élément dans le \* tableau *ParameterValuePtr* (si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1). Cet argument définit le champ d’enregistrement SQL_DESC_OCTET_LENGTH du APD. Si l’application spécifie plusieurs valeurs, *BufferLength* est utilisé pour déterminer l’emplacement des valeurs dans le tableau **ParameterValuePtr* , à la fois en entrée et en sortie. Pour les paramètres d’entrée/sortie et de sortie, il est utilisé pour déterminer s’il faut tronquer les données C binaires et de caractères lors de la sortie :  
  
-   Pour les données de type caractère C, si le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength*, les données de \* *ParameterValuePtr* sont tronquées à *BufferLength* moins la longueur d’un caractère de fin null et le pilote se termine par un caractère null.  
  
-   Pour les données C binaires, si le nombre d’octets disponibles à retourner est supérieur à *BufferLength*, les données de \* *ParameterValuePtr* sont tronquées à *BufferLength* octets.  
  
 Pour tous les autres types de données C, l’argument *BufferLength* est ignoré. La longueur de la \* mémoire tampon *ParameterValuePtr* (s’il s’agit d’un élément unique) ou la longueur d’un élément dans le \* *tableau ParameterValuePtr* (si l’application appelle **SQLSetStmtAttr** avec un argument d' *attribut* de SQL_ATTR_PARAMSET_SIZE pour spécifier plusieurs valeurs pour chaque paramètre) est supposé être la longueur du type de données C.  
  
 Pour les paramètres de sortie ou d’entrée/sortie diffusés en continu, l’argument *BufferLength* est ignoré, car la longueur de la mémoire tampon est spécifiée dans **SQLGetData**.  
  
> [!NOTE]  
>  Quand une application ODBC 1,0 appelle **SQLSetParam,** dans ODBC 3. *x* , le gestionnaire de pilotes convertit ce en un appel de **SQLBindParameter** dans lequel l’argument *BufferLength* est toujours SQL_SETPARAM_VALUE_MAX. Parce que le gestionnaire de pilotes renvoie une erreur si ODBC 3. *x* application définit *BufferLength* sur SQL_SETPARAM_VALUE_MAX, ODBC 3. *x* le pilote peut l’utiliser pour déterminer quand il est appelé par une application ODBC 1,0.  
  
> [!NOTE]  
>  Dans **SQLSetParam,**, la façon dont une application spécifie la longueur de la mémoire tampon **ParameterValuePtr* afin que le pilote puisse retourner des données de type caractère ou binaire, et la façon dont une application envoie un tableau de valeurs de paramètre de type caractère ou binaire au pilote, sont définies par le pilote.  
  
## <a name="strlen_or_indptr-argument"></a>Argument StrLen_or_IndPtr

 L’argument *StrLen_or_IndPtr* pointe vers une mémoire tampon qui, lorsque **SQLExecute** ou **SQLExecDirect** est appelé, contient l’un des éléments suivants. (Cet argument définit les SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_INDICATOR_PTR champs d’enregistrement des pointeurs de paramètre d’application.)  
  
-   Longueur de la valeur de paramètre stockée dans **ParameterValuePtr*. Cela est ignoré, à l’exception des données de type binaire ou caractère.  
  
-   SQL_NTS. La valeur du paramètre est une chaîne terminée par le caractère null.  
  
-   SQL_NULL_DATA. La valeur du paramètre est NULL.  
  
-   SQL_DEFAULT_PARAM. Une procédure consiste à utiliser la valeur par défaut d’un paramètre, au lieu d’une valeur extraite de l’application. Cette valeur est valide uniquement dans une procédure appelée dans la syntaxe canonique ODBC, puis uniquement si l’argument *InputOutputType* est SQL_PARAM_INPUT, SQL_PARAM_INPUT_OUTPUT ou SQL_PARAM_INPUT_OUTPUT_STREAM. Lorsque \* *StrLen_or_IndPtr* est SQL_DEFAULT_PARAM, les *arguments ValueType*, *ParameterType*, *Column*, *DecimalDigits*, *BufferLength* et *ParameterValuePtr* sont ignorés pour les paramètres d’entrée et sont utilisés uniquement pour définir la valeur du paramètre de sortie pour les paramètres d’entrée/sortie.  
  
-   Résultat de la macro SQL_LEN_DATA_AT_EXEC (*longueur*). Les données du paramètre sont envoyées avec **SQLPutData**. Si l’argument *ParameterType* est SQL_LONGVARBINARY, SQL_LONGVARCHAR ou un type de données long, spécifique à la source de données, et que le pilote retourne « Y » pour le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo**, *Length* est le nombre d’octets de données à envoyer pour le paramètre. dans le cas contraire, la *longueur* doit être une valeur non négative et est ignorée. Pour plus d’informations, consultez « transmission des valeurs de paramètres », plus loin dans cette section.  
  
     Par exemple, pour spécifier que 10 000 octets de données seront envoyés avec **SQLPutData** dans un ou plusieurs appels, pour un paramètre SQL_LONGVARCHAR, une application définit **StrLen_or_IndPtr* à SQL_LEN_DATA_AT_EXEC (10000).  
  
-   SQL_DATA_AT_EXEC. Les données du paramètre sont envoyées avec **SQLPutData**. Cette valeur est utilisée par les applications ODBC 1,0 quand elles appellent ODBC 3. *x* pilotes. Pour plus d’informations, consultez « transmission des valeurs de paramètres », plus loin dans cette section.  
  
 Si *StrLen_or_IndPtr* est un pointeur null, le pilote part du principe que toutes les valeurs de paramètre d’entrée sont non null et que les données de type caractère et binaire se terminent par un caractère null. Si *InputOutputType* est SQL_PARAM_OUTPUT ou *SQL_PARAM_OUTPUT_STREAM et que* les *StrLen_or_IndPtr* sont des pointeurs null, le pilote ignore la valeur de sortie.  
  
> [!NOTE]  
>  Les développeurs d’applications sont fortement déconseillés de spécifier un pointeur null pour *StrLen_or_IndPtr* lorsque le type de données du paramètre est SQL_C_BINARY. Pour vous assurer qu’un pilote ne tronque pas de manière inattendue SQL_C_BINARY données, *StrLen_or_IndPtr* doit contenir un pointeur vers une valeur de longueur valide.  
  
 Si l’argument *InputOutputType* est SQL_PARAM_INPUT_OUTPUT, SQL_PARAM_OUTPUT, SQL_PARAM_INPUT_OUTPUT_STREAM ou SQL_PARAM_OUTPUT_STREAM, *StrLen_or_IndPtr* pointe vers une mémoire tampon dans laquelle le pilote retourne SQL_NULL_DATA, le nombre d’octets disponibles à retourner dans \* *ParameterValuePtr* (à l’exception de l’octet de fin NULL des données caractère), ou SQL_NO_TOTAL (si le nombre d’octets disponibles à retourner ne peut pas être déterminé). Si la procédure retourne un ou plusieurs jeux de résultats, la définition de la mémoire tampon **StrLen_or_IndPtr* n’est pas garantie tant que tous les résultats n’ont pas été récupérés.  
  
 Si la valeur de l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est supérieure à 1, *StrLen_or_IndPtr* pointe vers un tableau de valeurs sqllen. Il peut s’agir de l’une des valeurs répertoriées plus haut dans cette section et qui sont traitées avec une seule instruction SQL.  
  
## <a name="passing-parameter-values"></a>Passage de valeurs de paramètre

 Une application peut passer la valeur d’un paramètre dans la \* mémoire tampon *ParameterValuePtr* ou avec un ou plusieurs appels à **SQLPutData**. Les paramètres dont les données sont transmises avec **SQLPutData** sont appelés paramètres de *données en* cours d’exécution. Ils sont généralement utilisés pour envoyer des données pour les paramètres SQL_LONGVARBINARY et SQL_LONGVARCHAR et peuvent être mélangés à d’autres paramètres.  
  
 Pour passer des valeurs de paramètre, une application exécute la séquence d’étapes suivante :  
  
1.  Appelle **SQLBindParameter** pour chaque paramètre afin de lier les mémoires tampons pour la valeur du paramètre (argument *ParameterValuePtr* ) et la longueur/l’indicateur (argument *StrLen_or_IndPtr* ). Pour les paramètres de données en cours d’exécution, *ParameterValuePtr* est une valeur de pointeur définie par l’application, telle qu’un numéro de paramètre ou un pointeur vers des données. La valeur est retournée ultérieurement et peut être utilisée pour identifier le paramètre.  
  
2.  Place des valeurs pour les paramètres d’entrée et d’entrée/sortie dans les \* tampons *ParameterValuePtr* et **StrLen_or_IndPtr* :  
  
    -   Pour les paramètres normaux, l’application place la valeur de paramètre dans la \* mémoire tampon *ParameterValuePtr* et la longueur de cette valeur dans la mémoire tampon **StrLen_or_IndPtr* . Pour plus d’informations, consultez [définition des valeurs des paramètres](../../../odbc/reference/develop-app/setting-parameter-values.md).  
  
    -   Pour les paramètres de données en cours d’exécution, l’application place le résultat de la macro SQL_LEN_DATA_AT_EXEC (*longueur*) (lors de l’appel d’un pilote ODBC 2,0) dans la mémoire tampon **StrLen_or_IndPtr* .  
  
3.  Appelle **SQLExecute** ou **SQLExecDirect** pour exécuter l’instruction SQL.  
  
    -   S’il n’existe aucun paramètre de données en cours d’exécution, le processus est terminé.  
  
    -   S’il existe des paramètres de données en cours d’exécution, la fonction retourne SQL_NEED_DATA.  
  
4.  Appelle **SQLParamData** pour récupérer la valeur définie par l’application spécifiée dans l’argument *ParameterValuePtr* de **SQLBindParameter** pour le premier paramètre de données en cours d’exécution à traiter. **SQLParamData** retourne SQL_NEED_DATA.  
  
    > [!NOTE]  
    >  Bien que les paramètres de données en cours d’exécution ressemblent aux colonnes de données en cours d’exécution, la valeur retournée par **SQLParamData** est différente pour chaque. Les paramètres de données en cours d’exécution sont des paramètres dans une instruction SQL pour laquelle des données sont envoyées avec **SQLPutData** lorsque l’instruction est exécutée avec **SQLExecDirect** ou **SQLExecute**. Elles sont liées à **SQLBindParameter**. La valeur retournée par **SQLParamData** est une valeur de pointeur passée à **SQLBindParameter** dans l’argument *ParameterValuePtr* . Les colonnes de données en cours d’exécution sont des colonnes d’un ensemble de lignes pour lesquelles des données sont envoyées avec **SQLPutData** lorsqu’une ligne est mise à jour ou ajoutée avec **SQLBulkOperations** ou mise à jour avec **SQLSetPos**. Elles sont liées à **SQLBindCol**. La valeur retournée par **SQLParamData** est l’adresse de la ligne dans la mémoire tampon **TargetValuePtr* (définie par un appel à **SQLBindCol**) qui est en cours de traitement.  
  
5.  Appelle **SQLPutData** une ou plusieurs fois pour envoyer des données pour le paramètre. Plusieurs appels sont nécessaires si la valeur des données est supérieure à la \* mémoire tampon *ParameterValuePtr* spécifiée dans **SQLPutData**; plusieurs appels à **SQLPutData** pour le même paramètre sont autorisés uniquement lors de l’envoi de données de caractères c à une colonne avec un type de données caractère, binaire ou spécifique à la source de données, ou lors de l’envoi de données binaires c à une colonne avec un type de données caractère, binaire  
  
6.  Appelle à nouveau **SQLParamData** pour signaler que toutes les données ont été envoyées pour le paramètre.  
  
    -   S’il y a plus de paramètres de données en cours d’exécution, **SQLParamData** retourne SQL_NEED_DATA et la valeur définie par l’application pour le prochain paramètre de données en cours d’exécution à traiter. L’application répète les étapes 4 et 5.  
  
    -   S’il n’y a plus de paramètres de données en cours d’exécution, le processus est terminé. Si l’instruction a été exécutée avec succès, **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO ; Si l’exécution a échoué, elle retourne SQL_ERROR. À ce stade, **SQLParamData** peut retourner tout SQLSTATE pouvant être retourné par la fonction utilisée pour exécuter l’instruction (**SQLExecDirect** ou **SQLExecute**).  
  
         Les valeurs de sortie de tous les paramètres d’entrée/sortie ou de sortie sont disponibles dans les \* tampons *ParameterValuePtr* et **StrLen_or_IndPtr* une fois que l’application a récupéré tous les jeux de résultats générés par l’instruction.  
  
 L’appel de **SQLExecute** ou de **SQLExecDirect** place l’instruction dans un état de SQL_NEED_DATA. À ce stade, l’application peut appeler uniquement **SQLCancel**, **SQLGetDiagField**, **SQLGetDiagRec**, **SQLGetFunctions**, **SQLParamData** ou **SQLPutData** avec l’instruction ou le *handle de connexion* associé à l’instruction. Si elle appelle une autre fonction avec l’instruction ou la connexion associée à l’instruction, la fonction retourne SQLSTATE HY010 (erreur de séquence de fonction). L’instruction conserve l’État SQL_NEED_DATA lorsque **SQLParamData** ou **SQLPutData** retourne une erreur, **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, ou l’instruction est annulée.  
  
 Si l’application appelle **SQLCancel** alors que le pilote a encore besoin de données pour les paramètres de données en cours d’exécution, le pilote annule l’exécution de l’instruction ; l’application peut ensuite appeler **SQLExecute** ou **SQLExecDirect** .  
  
## <a name="retrieving-streamed-output-parameters"></a>Récupération des paramètres de sortie diffusés en continu

 Quand une application définit *InputOutputType* sur SQL_PARAM_INPUT_OUTPUT_STREAM ou SQL_PARAM_OUTPUT_STREAM, la valeur du paramètre de sortie doit être récupérée par un ou plusieurs appels à **SQLGetData**. Lorsque le pilote a une valeur de paramètre de sortie diffusée en continu à retourner à l’application, il retourne SQL_PARAM_DATA_AVAILABLE en réponse à un appel aux fonctions suivantes : **SQLMoreResults**, **SQLExecute** et **SQLExecDirect**. Une application appelle **SQLParamData** pour déterminer quelle valeur de paramètre est disponible.  
  
 Pour plus d’informations sur les SQL_PARAM_DATA_AVAILABLE et les paramètres de sortie diffusés en continu, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="using-arrays-of-parameters"></a>Utilisation de tableaux de paramètres

 Quand une application prépare une instruction avec des marqueurs de paramètres et passe un tableau de paramètres, il existe deux façons différentes d’exécuter cette commande. L’une des méthodes consiste à ce que le pilote s’appuie sur les capacités de traitement de tableau de la back end, auquel cas l’instruction entière avec le tableau de paramètres est traitée comme une seule unité atomique. Oracle est un exemple de source de données qui prend en charge les fonctionnalités de traitement de tableau. Une autre façon d’implémenter cette fonctionnalité consiste à ce que le pilote génère un lot d’instructions SQL, une instruction SQL pour chaque ensemble de paramètres dans le tableau de paramètres et exécute le lot. Les tableaux de paramètres ne peuvent pas être utilisés avec une instruction **Update WHERE Current of** .  
  
 Lorsqu’un tableau de paramètres est traité, les jeux de résultats individuels/les nombres de lignes (un pour chaque jeu de paramètres) peuvent être disponibles, ou les nombres de jeux de résultats/lignes peuvent être cumulés dans un. L’option SQL_PARAM_ARRAY_ROW_COUNTS dans **SQLGetInfo** indique si les nombres de lignes sont disponibles pour chaque ensemble de paramètres (SQL_PARC_BATCH) ou si un seul nombre de lignes est disponible (SQL_PARC_NO_BATCH).  
  
 L’option SQL_PARAM_ARRAY_SELECTS dans **SQLGetInfo** indique si un jeu de résultats est disponible pour chaque ensemble de paramètres (SQL_PAS_BATCH) ou si un seul jeu de résultats est disponible (SQL_PAS_NO_BATCH). Si le pilote n’autorise pas l’exécution d’une instruction de génération de jeu de résultats avec un tableau de paramètres, SQL_PARAM_ARRAY_SELECTS retourne SQL_PAS_NO_SELECT.  
  
 Pour plus d’informations, consultez la [fonction SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md).  
  
 Pour prendre en charge les tableaux de paramètres, l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE est défini de façon à spécifier le nombre de valeurs pour chaque paramètre. Si le champ est supérieur à 1, les champs SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR du APD doivent pointer vers des tableaux. La cardinalité de chaque tableau est égale à la valeur de SQL_ATTR_PARAMSET_SIZE.  
  
 Le champ SQL_DESC_ROWS_PROCESSED_PTR du APD pointe vers une mémoire tampon qui contient le nombre de jeux de paramètres qui ont été traités, y compris les ensembles d’erreurs. À mesure que chaque ensemble de paramètres est traité, le pilote stocke une nouvelle valeur dans la mémoire tampon. Aucun nombre n’est retourné s’il s’agit d’un pointeur null. Lorsque des tableaux de paramètres sont utilisés, la valeur désignée par le champ SQL_DESC_ROWS_PROCESSED_PTR de l’APD est remplie même si la SQL_ERROR est retournée par la fonction Setting. Si SQL_NEED_DATA est retourné, la valeur désignée par le champ SQL_DESC_ROWS_PROCESSED_PTR du APD est définie sur le jeu de paramètres en cours de traitement.  
  
 Ce qui se produit lorsqu’un tableau de paramètres est lié et qu’une **mise à jour où l’instruction Current of** est exécutée est définie par le pilote.  
  
## <a name="column-wise-parameter-binding"></a>Liaison de paramètre Column-Wise

 Dans une liaison selon les colonnes, l’application lie des tableaux de paramètres et de longueur/indicateur distincts à chaque paramètre.  
  
 Pour utiliser la liaison selon les colonnes, l’application définit d’abord l’attribut d’instruction SQL_ATTR_PARAM_BIND_TYPE sur SQL_PARAM_BIND_BY_COLUMN. (Il s’agit de la valeur par défaut.) Pour chaque colonne à lier, l’application effectue les étapes suivantes :  
  
1.  Alloue un tableau de mémoires tampons de paramètres.  
  
2.  Alloue un tableau de mémoires tampons de longueur/d’indicateur.  
  
    > [!NOTE]  
    >  Si l’application écrit directement dans descripteurs lorsque la liaison selon les colonnes est utilisée, des tableaux distincts peuvent être utilisés pour les données de longueur et d’indicateur.  
  
3.  Appelle **SQLBindParameter** avec les arguments suivants :  
  
    -   *ValueType* est le type C d’un élément unique dans le tableau de mémoires tampons de paramètres.  
  
    -   *ParameterType* est le type SQL du paramètre.  
  
    -   *ParameterValuePtr* est l’adresse du tableau de mémoires tampons de paramètres.  
  
    -   *BufferLength* est la taille d’un élément unique dans le tableau de mémoires tampons de paramètres. L’argument *BufferLength* est ignoré lorsque les données sont de longueur fixe.  
  
    -   *StrLen_or_IndPtr* est l’adresse du tableau longueur/indicateur.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « argument ParameterValuePtr » dans « Comments », plus loin dans cette section. Pour plus d’informations sur la liaison selon les colonnes des paramètres, consultez [liaison de tableaux de paramètres](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="row-wise-parameter-binding"></a>Liaison de paramètre Row-Wise

 Dans une liaison selon les lignes, l’application définit une structure qui contient des mémoires tampons de paramètres et de longueur/indicateur pour chaque paramètre à lier.  
  
 Pour utiliser la liaison selon les lignes, l’application effectue les étapes suivantes :  
  
1.  Définit une structure pour contenir un ensemble unique de paramètres (y compris les mémoires tampons de paramètres et de longueur/d’indicateur) et alloue un tableau de ces structures.  
  
    > [!NOTE]  
    >  Si l’application écrit directement dans descripteurs lorsque la liaison selon les lignes est utilisée, des champs séparés peuvent être utilisés pour les données de longueur et d’indicateur.  
  
2.  Affecte à l’attribut d’instruction SQL_ATTR_PARAM_BIND_TYPE la taille de la structure qui contient un ensemble unique de paramètres ou la taille d’une instance d’une mémoire tampon dans laquelle les paramètres seront liés. La longueur doit inclure l’espace pour tous les paramètres liés, ainsi que tout remplissage de la structure ou de la mémoire tampon, pour s’assurer que lorsque l’adresse d’un paramètre lié est incrémentée avec la longueur spécifiée, le résultat pointe vers le début du même paramètre dans la ligne suivante. Lorsque vous utilisez l’opérateur *sizeof* en C ANSI, ce comportement est garanti.  
  
3.  Appelle **SQLBindParameter** avec les arguments suivants pour chaque paramètre à lier :  
  
    -   *ValueType* est le type du membre de la mémoire tampon des paramètres à lier à la colonne.  
  
    -   *ParameterType* est le type SQL du paramètre.  
  
    -   *ParameterValuePtr* est l’adresse du membre de la mémoire tampon du paramètre dans le premier élément du tableau.  
  
    -   *BufferLength* est la taille du membre de la mémoire tampon des paramètres.  
  
    -   *StrLen_or_IndPtr* est l’adresse du membre de longueur/indicateur à lier.  
  
 Pour plus d’informations sur l’utilisation de ces informations, consultez « argument *ParameterValuePtr* », plus loin dans cette section. Pour plus d’informations sur la liaison selon les lignes des paramètres, consultez les [tableaux de liaison des paramètres](../../../odbc/reference/develop-app/binding-arrays-of-parameters.md).  
  
## <a name="error-information"></a>Informations sur l'erreur

 Si un pilote n’implémente pas de tableaux de paramètres en tant que lots (l’option SQL_PARAM_ARRAY_ROW_COUNTS est égale à SQL_PARC_NO_BATCH), les situations d’erreur sont gérées comme si une instruction avait été exécutée. Si le pilote implémente des tableaux de paramètres en tant que lots, une application peut utiliser le champ d’en-tête SQL_DESC_ARRAY_STATUS_PTR de l’IPD pour déterminer le paramètre d’une instruction SQL ou le paramètre d’un tableau de paramètres entraînant le renvoi d’une erreur par **SQLExecDirect** ou **SQLExecute** . Ce champ contient des informations d’État pour chaque ligne de valeurs de paramètre. Si le champ indique qu’une erreur s’est produite, les champs de la structure de données de diagnostic indiquent le numéro de ligne et de paramètre du paramètre qui a échoué. Le nombre d’éléments dans le tableau est défini par le champ d’en-tête SQL_DESC_ARRAY_SIZE dans le APD, qui peut être défini par l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE.  
  
> [!NOTE]  
>  Le champ d’en-tête SQL_DESC_ARRAY_STATUS_PTR dans le APD est utilisé pour ignorer les paramètres. Pour plus d’informations sur les paramètres ignorés, consultez la section suivante, « ignorer un ensemble de paramètres ».  
  
 Lorsque **SQLExecute** ou **SQLExecDirect** retourne SQL_ERROR, les éléments du tableau vers lequel pointe le champ SQL_DESC_ARRAY_STATUS_PTR dans l’IPD contiennent SQL_PARAM_ERROR, SQL_PARAM_SUCCESS, SQL_PARAM_SUCCESS_WITH_INFO, SQL_PARAM_UNUSED ou SQL_PARAM_DIAG_UNAVAILABLE.  
  
 Pour chaque élément de ce tableau, la structure de données de diagnostic contient un ou plusieurs enregistrements d’État. Le champ SQL_DIAG_ROW_NUMBER de la structure indique le numéro de ligne des valeurs de paramètre à l’origine de l’erreur. S’il est possible de déterminer le paramètre particulier dans une ligne de paramètres à l’origine de l’erreur, le nombre de paramètres est entré dans le champ SQL_DIAG_COLUMN_NUMBER.  
  
 SQL_PARAM_UNUSED est entré lorsqu’un paramètre n’a pas été utilisé, car une erreur s’est produite dans un paramètre précédent qui a forcé l’abandon de **SQLExecute** ou **SQLExecDirect** . Par exemple, s’il existe 50 paramètres et qu’une erreur s’est produite lors de l’exécution de l’ensemble de paramètres Fortieth ayant provoqué l’abandon de **SQLExecute** ou **SQLExecDirect** , SQL_PARAM_UNUSED est entré dans le tableau d’état pour les paramètres 41 à 50.  
  
 SQL_PARAM_DIAG_UNAVAILABLE est entré lorsque le pilote traite des tableaux de paramètres comme une unité monolithique, donc il ne génère pas ce niveau de paramètre individuel d’informations d’erreur.  
  
 Certaines erreurs dans le traitement d’un ensemble unique de paramètres entraînent l’arrêt du traitement des jeux de paramètres suivants dans le tableau. D’autres erreurs n’affectent pas le traitement des paramètres suivants. Les erreurs qui vont cesser le traitement sont définies par le pilote. Si le traitement n’est pas arrêté, tous les paramètres du tableau sont traités, SQL_SUCCESS_WITH_INFO est retourné à la suite de l’erreur et la mémoire tampon définie par SQL_ATTR_PARAMS_PROCESSED_PTR est définie sur le nombre total d’ensembles de paramètres traités (comme défini par l’attribut d’instruction SQL_ATTR_PARAMSET_SIZE), qui comprend les ensembles d’erreurs.  
  
> [!CAUTION]  
>  Le comportement ODBC quand une erreur se produit dans le traitement d’un tableau de paramètres est différent dans ODBC 3. *x* qu’il était dans ODBC 2. *x*. Dans ODBC 2. *x*, la fonction a retourné SQL_ERROR et le traitement a cessé. La mémoire tampon vers laquelle pointe l’argument *Pirow* de **SQLParamOptions,** contenait le numéro de la ligne d’erreur. Dans ODBC 3. *x*, la fonction retourne SQL_SUCCESS_WITH_INFO et le traitement peut s’arrêter ou continuer. Si elle continue, la mémoire tampon spécifiée par SQL_ATTR_PARAMS_PROCESSED_PTR sera définie sur la valeur de tous les paramètres traités, y compris ceux qui ont provoqué une erreur. Ce changement de comportement peut entraîner des problèmes pour les applications existantes.  
  
 Lorsque **SQLExecute** ou **SQLExecDirect** retourne avant de terminer le traitement de tous les jeux de paramètres dans un tableau de paramètres, par exemple quand SQL_ERROR ou SQL_NEED_DATA est retourné, le tableau d’état contient des États pour les paramètres qui ont déjà été traités. L’emplacement désigné par le champ SQL_DESC_ROWS_PROCESSED_PTR dans l’IPD contient le numéro de ligne dans le tableau de paramètres qui a provoqué le code d’erreur SQL_ERROR ou SQL_NEED_DATA. Lorsqu’un tableau de paramètres est envoyé à une instruction SELECT, la disponibilité des valeurs de tableau d’État est définie par le pilote ; elles peuvent être disponibles après l’exécution de l’instruction ou lors de l’extraction des jeux de résultats.  
  
## <a name="ignoring-a-set-of-parameters"></a>Un ensemble de paramètres est ignoré

 Le champ SQL_DESC_ARRAY_STATUS_PTR du APD (tel que défini par l’attribut d’instruction SQL_ATTR_PARAM_STATUS_PTR) peut être utilisé pour indiquer qu’un ensemble de paramètres liés dans une instruction SQL doit être ignoré. Pour indiquer au pilote d’ignorer un ou plusieurs ensembles de paramètres lors de l’exécution, une application doit suivre les étapes suivantes :  
  
1.  Appelez **SQLSetDescField** pour définir le champ d’en-tête SQL_DESC_ARRAY_STATUS_PTR du APD de sorte qu’il pointe vers un tableau de valeurs SQLUSMALLINT pour contenir des informations d’État. Ce champ peut également être défini en appelant **SQLSetStmtAttr** avec un *attribut* de SQL_ATTR_PARAM_OPERATION_PTR, ce qui permet à une application de définir le champ sans obtenir de handle de descripteur.  
  
2.  Affectez à chaque élément du tableau défini par le champ SQL_DESC_ARRAY_STATUS_PTR de la APD l’une des deux valeurs suivantes :  
  
    -   SQL_PARAM_IGNORE, pour indiquer que la ligne est exclue de l’exécution de l’instruction.  
  
    -   SQL_PARAM_PROCEED, pour indiquer que la ligne est incluse dans l’exécution de l’instruction.  
  
3.  Appelez **SQLExecDirect** ou **SQLExecute** pour exécuter l’instruction préparée.  
  
 Les règles suivantes s’appliquent au tableau défini par le champ SQL_DESC_ARRAY_STATUS_PTR de l’APD :  
  
-   Par défaut, le pointeur a la valeur null.  
  
-   Si le pointeur a la valeur null, tous les jeux de paramètres sont utilisés, comme si tous les éléments avaient la valeur SQL_ROW_PROCEED.  
  
-   La définition d’un élément à SQL_PARAM_PROCEED ne garantit pas que l’opération utilisera ce jeu de paramètres particulier.  
  
-   SQL_PARAM_PROCEED est défini comme 0 dans le fichier d’en-tête.  
  
 Une application peut définir le champ SQL_DESC_ARRAY_STATUS_PTR dans APD pour qu’elle pointe vers le même tableau que celui désigné par le champ SQL_DESC_ARRAY_STATUS_PTR dans le IRD. Cela est utile lors de la liaison de paramètres à des données de ligne. Les paramètres peuvent ensuite être ignorés en fonction de l’état des données de ligne. En plus des SQL_PARAM_IGNORE, les codes suivants entraînent l’ignorance d’un paramètre dans une instruction SQL : SQL_ROW_DELETED, SQL_ROW_UPDATED et SQL_ROW_ERROR. En plus de SQL_PARAM_PROCEED, les codes suivants provoquent la poursuite d’une instruction SQL : SQL_ROW_SUCCESS, SQL_ROW_SUCCESS_WITH_INFO et SQL_ROW_ADDED.  
  
## <a name="rebinding-parameters"></a>Reliaison des paramètres

 Une application peut effectuer l’une des deux opérations suivantes pour modifier une liaison :  
  
-   Appelez **SQLBindParameter** pour spécifier une nouvelle liaison pour une colonne qui est déjà liée. Le pilote remplace l’ancienne liaison par la nouvelle.  
  
-   Spécifiez un décalage à ajouter à l’adresse tampon spécifiée par l’appel de liaison à **SQLBindParameter**. Pour plus d’informations, consultez la section suivante, « reliaison avec décalages ».  
  
## <a name="rebinding-with-offsets"></a>Reliaison avec décalages

 La reliaison des paramètres est particulièrement utile lorsqu’une application a une configuration de zone de mémoire tampon qui peut contenir de nombreux paramètres, mais un appel à **SQLExecDirect** ou à **SQLExecute** n’utilise que quelques paramètres. L’espace restant dans la zone de mémoire tampon peut être utilisé pour l’ensemble de paramètres suivant en modifiant la liaison existante par un décalage.  
  
 Le champ d’en-tête SQL_DESC_BIND_OFFSET_PTR dans le APD pointe sur le décalage de liaison. Si le champ n’a pas la valeur null, le pilote déréférence le pointeur et, si aucune des valeurs des champs SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR est un pointeur null, ajoute la valeur déréférencée à ces champs dans les enregistrements de descripteur au moment de l’exécution. Les nouvelles valeurs de pointeur sont utilisées lors de l’exécution des instructions SQL. L’offset reste valide après la reliaison. Étant donné que SQL_DESC_BIND_OFFSET_PTR est un pointeur vers le décalage plutôt que l’offset lui-même, une application peut modifier le décalage directement, sans avoir à appeler [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) ou [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md) pour modifier le champ de descripteur. Par défaut, le pointeur a la valeur null. Le champ SQL_DESC_BIND_OFFSET_PTR du ARD peut être défini par un appel à [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) ou par un appel à [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)avec un *fAttribute* de SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 Le décalage de liaison est toujours ajouté directement aux valeurs des champs SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR. Si le décalage est remplacé par une autre valeur, la nouvelle valeur est toujours ajoutée directement à la valeur dans chaque champ de descripteur. Le nouvel offset n’est pas ajouté à la somme de la valeur de champ et de tous les décalages précédents.  
  
## <a name="descriptors"></a>Descripteurs

 Le mode de liaison d’un paramètre est déterminé par les champs APD et IPD. Les arguments de **SQLBindParameter** sont utilisés pour définir ces champs de descripteur. Les champs peuvent également être définis par les fonctions **SQLSetDescField** , bien que **SQLBindParameter** soit plus efficace à utiliser car l’application n’a pas besoin d’obtenir un handle de descripteur pour appeler **SQLBindParameter**.  
  
> [!CAUTION]  
>  L’appel de **SQLBindParameter** pour une instruction peut affecter d’autres instructions. Cela se produit lorsque le ARD associé à l’instruction est explicitement alloué et est également associé à d’autres instructions. Comme **SQLBindParameter** modifie les champs de APD, les modifications s’appliquent à toutes les instructions auxquelles ce descripteur est associé. Si ce n’est pas le comportement requis, l’application doit dissocier ce descripteur des autres instructions avant d’appeler **SQLBindParameter**.  
  
 Conceptuellement, **SQLBindParameter** effectue les étapes suivantes dans l’ordre :  
  
1.  Appelle [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) pour obtenir le handle APD.  
  
2.  Appelle [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) pour récupérer le champ de SQL_DESC_COUNT du APD, et si la valeur de l’argument *ColumnNumber* dépasse la valeur de SQL_DESC_COUNT, appelle **SQLSetDescField** pour augmenter la valeur de SQL_DESC_COUNT à *ColumnNumber*.  
  
3.  Appelle [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) plusieurs fois pour affecter des valeurs aux champs suivants du APD :  
  
    -   Définit SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE sur la valeur de *ValueType*, sauf que si *ValueType* est l’un des identificateurs concis d’un sous-type DateTime ou interval, il définit SQL_DESC_TYPE sur SQL_DATETIME ou SQL_INTERVAL, respectivement, définit SQL_DESC_CONCISE_TYPE sur l’identificateur concis et définit SQL_DESC_DATETIME_INTERVAL_CODE sur le sous-code datetime ou Interval correspondant.  
  
    -   Définit le champ SQL_DESC_OCTET_LENGTH sur la valeur de *BufferLength*.  
  
    -   Affecte la valeur de *ParameterValue* à la valeur du champ SQL_DESC_DATA_PTR.  
  
    -   Affecte la valeur de *StrLen_Or_Ind* au champ SQL_DESC_OCTET_LENGTH_PTR.  
  
    -   Affecte également la valeur de *StrLen_Or_Ind* au champ SQL_DESC_INDICATOR_PTR.  
  
     Le paramètre *StrLen_Or_Ind* spécifie les informations d’indicateur et la longueur de la valeur de paramètre.  
  
4.  Appelle [SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md) pour obtenir le handle IPD.  
  
5.  Appelle [SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md) pour récupérer le champ SQL_DESC_COUNT de l’IPD et, si la valeur de l’argument *ColumnNumber* dépasse la valeur de SQL_DESC_COUNT, appelle **SQLSetDescField** pour augmenter la valeur de SQL_DESC_COUNT à *ColumnNumber*.  
  
6.  Appelle [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md) plusieurs fois pour affecter des valeurs aux champs suivants de l’IPD :  
  
    -   Définit SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE sur la valeur de *ParameterType*, sauf que si *ParameterType* est l’un des identificateurs concis d’un sous-type DateTime ou interval, il définit SQL_DESC_TYPE sur SQL_DATETIME ou SQL_INTERVAL, respectivement, définit SQL_DESC_CONCISE_TYPE sur l’identificateur concis et définit SQL_DESC_DATETIME_INTERVAL_CODE sur le sous-code datetime ou Interval correspondant.  
  
    -   Définit un ou plusieurs SQL_DESC_LENGTH, SQL_DESC_PRECISION et SQL_DESC_DATETIME_INTERVAL_PRECISION, selon le cas pour *ParameterType*.  
  
    -   Définit SQL_DESC_SCALE sur la valeur de *DecimalDigits*.  
  
 Si l’appel à **SQLBindParameter** échoue, le contenu des champs de descripteur qu’il aurait définis dans le APD n’est pas défini, et le champ SQL_DESC_COUNT du APD est inchangé. En outre, les champs SQL_DESC_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_TYPE de l’enregistrement approprié dans l’IPD ne sont pas définis et le champ SQL_DESC_COUNT de l’IPD est inchangé.  
  
## <a name="conversion-of-calls-to-and-from-sqlsetparam"></a>Conversion des appels vers et à partir de SQLSetParam,

 Quand une application ODBC 1,0 appelle **SQLSetParam,** dans ODBC 3. *x* Driver, ODBC 3. *x* Driver Manager mappe l’appel comme indiqué dans le tableau suivant.  
  
|Appeler par l’application ODBC 1,0|Appel à ODBC 3. pilote *x*|  
|----------------------------------|-------------------------------|  
|SQLSetParam, (StatementHandle, ParameterNumber, ValueType, ParameterType, LengthPrecision, ParameterScale, ParameterValuePtr, StrLen_or_IndPtr);|SQLBindParameter (StatementHandle, ParameterNumber, SQL_PARAM_INPUT_OUTPUT, ValueType, ParameterType,      *Column*,      *DecimalDigits*, ParameterValuePtr, SQL_SETPARAM_VALUE_MAX, StrLen_or_IndPtr);|  
  
## <a name="examples"></a>Exemples  

### <a name="a-use-sqlbindparameter-function"></a>R. Utiliser SQLBindParameter (fonction)

 Dans l’exemple suivant, une application prépare une instruction SQL pour insérer des données dans la table ORDERs. Pour chaque paramètre de l’instruction, l’application appelle **SQLBindParameter** pour spécifier le type de données C ODBC et le type de données SQL du paramètre, et pour lier une mémoire tampon à chaque paramètre. Pour chaque ligne de données, l’application assigne des valeurs de données à chaque paramètre et appelle **SQLExecute** pour exécuter l’instruction.  
  
 L’exemple suivant suppose que vous disposez d’une source de données ODBC sur votre ordinateur appelée Northwind qui est associée à la base de données Northwind.  
  
 Pour obtenir plus d’exemples de code, consultez fonction [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md), [fonction SQLProcedures](../../../odbc/reference/syntax/sqlprocedures-function.md), [fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)et [fonction SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md).  
  
```cpp
// SQLBindParameter_Function.cpp  
// compile with: ODBC32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define EMPLOYEE_ID_LEN 10  
  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLSMALLINT sCustID;  
  
SQLCHAR szEmployeeID[EMPLOYEE_ID_LEN];  
SQL_DATE_STRUCT dsOrderDate;  
SQLINTEGER cbCustID = 0, cbOrderDate = 0, cbEmployeeID = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, EMPLOYEE_ID_LEN, 0, szEmployeeID, 0, &cbEmployeeID);  
   retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_SSHORT, SQL_INTEGER, 0, 0, &sCustID, 0, &cbCustID);  
   retcode = SQLBindParameter(hstmt, 3, SQL_PARAM_INPUT, SQL_C_TYPE_DATE, SQL_TIMESTAMP, sizeof(dsOrderDate), 0, &dsOrderDate, 0, &cbOrderDate);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"INSERT INTO Orders(CustomerID, EmployeeID, OrderDate) VALUES (?, ?, ?)", SQL_NTS);  
  
   strcpy_s((char*)szEmployeeID, _countof(szEmployeeID), "BERGS");  
   sCustID = 5;  
   dsOrderDate.year = 2006;  
   dsOrderDate.month = 3;  
   dsOrderDate.day = 17;  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
### <a name="b-execute-a-stored-procedure-using-a-named-parameter"></a>B. Exécuter une procédure stockée à l’aide d’un paramètre nommé

 Dans l’exemple suivant, une application exécute une procédure stockée SQL Server à l’aide d’un paramètre nommé.  
  
```cpp
// SQLBindParameter_Function_2.cpp  
// compile with: ODBC32.lib  
// sample assumes the following stored procedure:  
// use northwind  
// DROP PROCEDURE SQLBindParameter  
// GO  
//   
// CREATE PROCEDURE SQLBindParameter @quote int  
// AS  
// delete from orders where OrderID >= @quote  
// GO  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
SQLHDESC hIpd = NULL;  
SQLHENV henv = NULL;  
SQLHDBC hdbc = NULL;  
SQLRETURN retcode;  
SQLHSTMT hstmt = NULL;  
SQLCHAR szQuote[50] = "100084";  
SQLINTEGER cbValue = SQL_NTS;  
  
int main() {  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
   retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
   retcode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
   retcode = SQLConnect(hdbc, (SQLCHAR*) "Northwind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
   retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   retcode = SQLPrepare(hstmt, (SQLCHAR*)"{call SQLBindParameter(?)}", SQL_NTS);  
   retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 50, 0, szQuote, 0, &cbValue);  
   retcode = SQLGetStmtAttr(hstmt, SQL_ATTR_IMP_PARAM_DESC, &hIpd, 0, 0);  
   retcode = SQLSetDescField(hIpd, 1, SQL_DESC_NAME, "@quote", SQL_NTS);  
  
   retcode = SQLExecute(hstmt);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Retour d’informations sur un paramètre dans une instruction|[Fonction SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Libération des tampons de paramètres sur l’instruction|[Fonction SQLFreeStmt](../../../odbc/reference/syntax/sqlfreestmt-function.md)|  
|Retour du nombre de paramètres d’instruction|[Fonction SQLNumParams](../../../odbc/reference/syntax/sqlnumparams-function.md)|  
|Retour du paramètre suivant pour l’envoi de données|[SQLParamData, fonction](../../../odbc/reference/syntax/sqlparamdata-function.md)|  
|Spécification de plusieurs valeurs de paramètre|[SQLParamOptions, fonction](../../../odbc/reference/syntax/sqlparamoptions-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[Fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Voir aussi

 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
