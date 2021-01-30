---
description: SQLSetDescField, fonction
title: SQLSetDescField fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescField
helpviewer_keywords:
- SQLSetDescField function [ODBC]
ms.assetid: 8c544388-fe9d-4f94-a0ac-fa0b9c9c88a5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: bd48da2caec1acba5cfe775a2baa1efaaf886aab
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200965"
---
# <a name="sqlsetdescfield-function"></a>SQLSetDescField, fonction

**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLSetDescField** définit la valeur d’un champ unique d’un enregistrement de descripteur.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
SQLRETURN SQLSetDescField(  
     SQLHDESC      DescriptorHandle,  
     SQLSMALLINT   RecNumber,  
     SQLSMALLINT   FieldIdentifier,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    BufferLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *DescriptorHandle*  
 Entrée Handle de descripteur.  
  
 *RecNumber*  
 Entrée Indique l’enregistrement du descripteur contenant le champ que l’application cherche à définir. Les enregistrements de descripteur sont numérotés à partir de 0, le numéro d’enregistrement 0 étant l’enregistrement du signet. L’argument *recnumber* est ignoré pour les champs d’en-tête.  
  
 *FieldIdentifier*  
 Entrée Indique le champ du descripteur dont la valeur doit être définie. Pour plus d’informations, consultez « argument *FieldIdentifier* » dans la section « commentaires ».  
  
 *ValuePtr*  
 Entrée Pointeur vers une mémoire tampon qui contient les informations du descripteur, ou une valeur entière. Le type de données dépend de la valeur de *FieldIdentifier*. Si *ValuePtr* est une valeur entière, elle peut être considérée comme étant de 8 octets (sqllen), de 4 octets (SQLINTEGER destinée) ou de 2 octets (SQLSMALLINT), en fonction de la valeur de l’argument *FieldIdentifier* .  
  
 *BufferLength*  
 Entrée Si *FieldIdentifier* est un champ défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de **ValuePtr*. Pour les données de type chaîne de caractères, cet argument doit contenir le nombre d’octets dans la chaîne.  
  
 Si *FieldIdentifier* est un champ défini par ODBC et *ValuePtr* est un entier, *BufferLength* est ignoré.  
  
 Si *FieldIdentifier* est un champ défini par le pilote, l’application indique la nature du champ au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *ValuePtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *ValuePtr* contient une valeur de longueur fixe, *BufferLength* est SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT ou SQL_IS_USMALLINT, selon le cas.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSetDescField** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DESC et un *handle* de *DescriptorHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLSetDescField** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée dans *\* ValuePtr* (si *ValuePtr* était un pointeur) ou la valeur dans *ValuePtr* (si *ValuePtr* était une valeur entière) ou *\* ValuePtr* n’était pas valide en raison des conditions de travail de l’implémentation, donc le pilote remplaçait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07009|Index de descripteur non valide|L’argument *FieldIdentifier* était un champ d’enregistrement, l’argument *recnumber* était 0 et l’argument *DescriptorHandle* est référencé par un handle IPD.<br /><br /> L’argument *recnumber* est inférieur à 0 et l’argument *DescriptorHandle* est référencé à un ARD ou un APD.<br /><br /> L’argument *recnumber* est supérieur au nombre maximal de colonnes ou de paramètres pris en charge par la source de données, et l’argument *DescriptorHandle* est référencé à un APD ou ARD.<br /><br /> (DM) l’argument *FieldIdentifier* a été SQL_DESC_COUNT, et l’argument *\* ValuePtr* était inférieur à 0.<br /><br /> L’argument *recnumber* était égal à 0, et l’argument *DescriptorHandle* faisait appel à un APD alloué de manière implicite. (Cette erreur ne se produit pas avec un descripteur d’application explicitement alloué, car il n’est pas connu qu’un descripteur d’application explicitement alloué est un APD ou un ARD jusqu’à l’exécution.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22001|Données de chaîne, tronquées à droite|L’argument *FieldIdentifier* a été SQL_DESC_NAME, et l’argument *BufferLength* était une valeur supérieure à SQL_MAX_IDENTIFIER_LEN.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) le *DescriptorHandle* a été associé à un *StatementHandle* pour lequel une fonction d’exécution asynchrone (pas celui-ci) a été appelée et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour le *StatementHandle* avec lequel le *DescriptorHandle* a été associé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *DescriptorHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLSetDescField** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des handles d’instruction associés à *DescriptorHandle* et retournés SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY016|Impossible de modifier un descripteur de ligne d’implémentation|L’argument *DescriptorHandle* a été associé à un IRD, et l’argument *FieldIdentifier* n’a pas été SQL_DESC_ARRAY_STATUS_PTR ou SQL_DESC_ROWS_PROCESSED_PTR.|  
|HY021|Informations de descripteur incohérentes|Les champs SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE ne forment pas un type SQL ODBC valide ou un type SQL spécifique au pilote valide (pour IPD) ou un type C ODBC valide (pour APD ou ARDs).<br /><br /> Les informations de descripteur vérifiées pendant une vérification de cohérence ne sont pas cohérentes. (Consultez « vérification de cohérence » dans **SQLSetDescRec**.)|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) *\* ValuePtr* est une chaîne de caractères et *BufferLength* est inférieur à zéro, mais n’est pas égal à SQL_NTS.<br /><br /> (DM) le pilote était un pilote ODBC 2 *. x* , le descripteur était un ARD, l’argument *ColumnNumber* était défini sur 0, et la valeur spécifiée pour l’argument *BufferLength* n’était pas égale à 4.|  
|HY091|Identificateur de champ de descripteur non valide|La valeur spécifiée pour l’argument *FieldIdentifier* n’est pas un champ défini par ODBC et n’est pas une valeur définie par l’implémentation.<br /><br /> L’argument *FieldIdentifier* n’était pas valide pour l’argument *DescriptorHandle* .<br /><br /> L’argument *FieldIdentifier* était un champ en lecture seule, défini par ODBC.|  
|HY092|Identificateur d’attribut/option non valide|La valeur de *\* ValuePtr* n’est pas valide pour l’argument *FieldIdentifier* .<br /><br /> L’argument *FieldIdentifier* a été SQL_DESC_UNNAMED et *ValuePtr* a été SQL_NAMED.|  
|HY105|Type de paramètre non valide|(DM) la valeur spécifiée pour le champ SQL_DESC_PARAMETER_TYPE n’était pas valide. (Pour plus d’informations, consultez la section « argument *InputOutputType* » dans **SQLBindParameter**.)|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [Nouveautés dans ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *DescriptorHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLSetDescField** pour définir un champ de descripteur à la fois. Un appel à **SQLSetDescField** définit un champ unique dans un seul descripteur. Cette fonction peut être appelée pour définir n’importe quel champ dans n’importe quel type de descripteur, à condition que le champ puisse être défini. (Consultez le tableau plus loin dans cette section.)  
  
> [!NOTE]  
>  Si un appel à **SQLSetDescField** échoue, le contenu de l’enregistrement de descripteur identifié par l’argument *recnumber* n’est pas défini.  
  
 D’autres fonctions peuvent être appelées pour définir plusieurs champs de descripteur avec un appel unique de la fonction. La fonction **SQLSetDescRec** définit un ensemble de champs qui affectent le type de données et la mémoire tampon liés à une colonne ou un paramètre (les champs SQL_DESC_TYPE, SQL_DESC_DATETIME_INTERVAL_CODE, SQL_DESC_OCTET_LENGTH, SQL_DESC_PRECISION, SQL_DESC_SCALE, SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR et SQL_DESC_INDICATOR_PTR). **SQLBindCol** ou **SQLBindParameter** peut être utilisé pour établir une spécification complète pour la liaison d’une colonne ou d’un paramètre. Ces fonctions définissent un groupe spécifique de champs de descripteur avec un appel de fonction.  
  
 **SQLSetDescField** peut être appelé pour modifier les mémoires tampons de liaison en ajoutant un décalage aux pointeurs de liaison (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR ou SQL_DESC_OCTET_LENGTH_PTR). Cela modifie les mémoires tampons de liaison sans appeler **SQLBindCol** ou **SQLBindParameter**, ce qui permet à une application de modifier SQL_DESC_DATA_PTR sans modifier d’autres champs, tels que SQL_DESC_DATA_TYPE.  
  
 Si une application appelle **SQLSetDescField** pour définir un champ autre que SQL_DESC_COUNT ou les champs différés SQL_DESC_DATA_PTR, SQL_DESC_OCTET_LENGTH_PTR ou SQL_DESC_INDICATOR_PTR, l’enregistrement devient indépendant.  
  
 Les champs d’en-tête de descripteur sont définis en appelant **SQLSetDescField** avec le *FieldIdentifier* approprié. De nombreux champs d’en-tête sont également des attributs d’instruction, donc ils peuvent également être définis par un appel à **SQLSetStmtAttr**. Cela permet aux applications de définir un champ de descripteur sans obtenir au préalable un handle de descripteur. Quand **SQLSetDescField** est appelé pour définir un champ d’en-tête, l’argument *recnumber* est ignoré.  
  
 Un *recnumber* de 0 est utilisé pour définir les champs de signet.  
  
> [!NOTE]  
>  L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS doit toujours être défini avant d’appeler **SQLSetDescField** pour définir les champs de signet. Bien que ce ne soit pas obligatoire, il est vivement recommandé.  
  
## <a name="sequence-of-setting-descriptor-fields"></a>Séquence de champs de descripteurs de paramètres  
 Lors de la définition des champs de descripteur en appelant **SQLSetDescField**, l’application doit suivre une séquence spécifique :  
  
1.  L’application doit d’abord définir le champ SQL_DESC_TYPE, SQL_DESC_CONCISE_TYPE ou SQL_DESC_DATETIME_INTERVAL_CODE.  
  
2.  Une fois que l’un de ces champs a été défini, l’application peut définir un attribut d’un type de données et le pilote affecte aux champs d’attribut de type de données les valeurs par défaut appropriées pour le type de données. La valeur par défaut automatique des champs d’attribut de type garantit que le descripteur est toujours prêt à être utilisé une fois que l’application a spécifié un type de données. Si l’application définit explicitement un attribut de type de données, elle remplace l’attribut par défaut.  
  
3.  Une fois que l’un des champs listés à l’étape 1 a été défini et que les attributs des types de données ont été définis, l’application peut définir SQL_DESC_DATA_PTR. Une vérification de cohérence des champs de descripteur est alors demandée. Si l’application modifie le type de données ou les attributs après avoir défini le champ SQL_DESC_DATA_PTR, le pilote affecte SQL_DESC_DATA_PTR à un pointeur null, en dissociant l’enregistrement. Cela force l’application à effectuer les étapes appropriées dans l’ordre, avant que l’enregistrement du descripteur ne soit utilisable.  
  
## <a name="initialization-of-descriptor-fields"></a>Initialisation de champs de descripteur  
 Lorsqu’un descripteur est alloué, les champs du descripteur peuvent être initialisés à une valeur par défaut, être initialisés sans valeur par défaut ou être non définis pour le type de descripteur. Les tableaux suivants indiquent l’initialisation de chaque champ pour chaque type de descripteur, avec « D » indiquant que le champ est initialisé avec une valeur par défaut, et « ND » indiquant que le champ est initialisé sans valeur par défaut. Si un nombre est affiché, la valeur par défaut du champ est ce nombre. Les tables indiquent également si un champ est en lecture/écriture (R/W) ou en lecture seule (R).  
  
 Les champs d’un IRD ont une valeur par défaut uniquement après la préparation ou l’exécution de l’instruction et le IRD a été rempli, et non lorsque le descripteur ou le descripteur d’instruction a été alloué. Tant que le IRD n’a pas été rempli, toute tentative d’accéder à un champ d’un IRD renvoie une erreur.  
  
 Certains champs de descripteur sont définis pour un ou plusieurs des types de descripteurs (ARDs et IRDs et APD et IPD). Quand un champ n’est pas défini pour un type de descripteur, il n’est pas requis par les fonctions qui utilisent ce descripteur.  
  
 Les champs accessibles par **SQLGetDescField** ne peuvent pas nécessairement être définis par **SQLSetDescField**. Les champs qui peuvent être définis par **SQLSetDescField** sont répertoriés dans les tableaux suivants.  
  
 L’initialisation des champs d’en-tête est décrite dans le tableau suivant.  
  
|Nom du champ d’en-tête|Type|R/W (Lecture/écriture)|Default|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_ALLOC_TYPE|SQLSMALLINT|ARD : R APD : R IRD : R IPD : R|ARD : SQL_DESC_ALLOC_AUTO pour implicite ou SQL_DESC_ALLOC_USER pour Explicit<br /><br /> APD : SQL_DESC_ALLOC_AUTO pour implicite ou SQL_DESC_ALLOC_USER pour Explicit<br /><br /> IRD : SQL_DESC_ALLOC_AUTO<br /><br /> IPD : SQL_DESC_ALLOC_AUTO|  
|SQL_DESC_ARRAY_SIZE|SQLULEN|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : [1] APD : [1] IRD : IPD inutilisé : inutilisé|  
|SQL_DESC_ARRAY_STATUS_PTR|SQLUSMALLINT*|ARD : R/W APD : R/W IRD : R/W IPD : R/W|ARD : PTR null APD : PTR null IRD : pointeur null PTR : PTR null|  
|SQL_DESC_BIND_OFFSET_PTR|SQLLEN|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : PTR null APD : PTR null IRD : IPD inutilisé : inutilisé|  
|SQL_DESC_BIND_TYPE|SQLINTEGER|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : SQL_BIND_BY_COLUMN<br /><br /> APD : SQL_BIND_BY_COLUMN<br /><br /> IRD : non utilisé<br /><br /> IPD : inutilisé|  
|SQL_DESC_COUNT|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : 0 APD : 0 IRD : D IPD : 0|  
|SQL_DESC_ROWS_PROCESSED_PTR|SQLULEN|ARD : APD inutilisé : IRD inutilisé : R/W IPD : R/W|ARD : APD inutilisé : IRD inutilisé : IPD PTR NULL : PTR null|  
  
 [1] ces champs sont définis uniquement lorsque l’IPD est automatiquement remplie par le pilote. Si ce n’est pas le cas, ils ne sont pas définis. Si une application tente de définir ces champs, SQLSTATE HY091 (identificateur de champ de descripteur non valide) est retourné.  
  
 L’initialisation des champs d’enregistrement est indiquée dans le tableau suivant.  
  
|Nom du champ d’enregistrement|Type|R/W (Lecture/écriture)|Default|  
|-----------------------|----------|----------|-------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE|SQLINTEGER|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_BASE_COLUMN_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_BASE_TABLE_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_CASE_SENSITIVE|SQLINTEGER|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : APD inutilisé : IRD inutilisé : D IPD : D [1]|  
|SQL_DESC_CATALOG_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_CONCISE_TYPE|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : SQL_C_ APD PAR DÉFAUT : SQL_C_ IRD PAR DÉFAUT : D IPD : ND|  
|SQL_DESC_DATA_PTR|SQLPOINTER|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : PTR null APD : PTR null IRD : IPD inutilisé : inutilisé [2]|  
|SQL_DESC_DATETIME_INTERVAL_CODE|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_DATETIME_INTERVAL_PRECISION|SQLINTEGER|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_DISPLAY_SIZE|SQLLEN|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_FIXED_PREC_SCALE|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : APD inutilisé : IRD inutilisé : D IPD : D [1]|  
|SQL_DESC_INDICATOR_PTR|SQLLEN|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : PTR null APD : PTR null IRD : IPD inutilisé : inutilisé|  
|SQL_DESC_LABEL|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_LENGTH|SQLULEN|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_LITERAL_PREFIX|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_LITERAL_SUFFIX|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_LOCAL_TYPE_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : APD inutilisé : IRD inutilisé : D IPD : D [1]|  
|SQL_DESC_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_NULLABLE|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_NUM_PREC_RADIX|SQLINTEGER|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_OCTET_LENGTH|SQLLEN|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_OCTET_LENGTH_PTR|SQLLEN|ARD : R/W APD : R/W IRD : IPD inutilisé : inutilisé|ARD : PTR null APD : PTR null IRD : IPD inutilisé : inutilisé|  
|SQL_DESC_PARAMETER_TYPE|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : IPD inutilisé : R/W|ARD : APD inutilisé : IRD inutilisé : IPD inutilisé : D = SQL_PARAM_INPUT|  
|SQL_DESC_PRECISION|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_ROWVER|SQLSMALLINT|ARD : non utilisé<br /><br /> APD : non utilisé<br /><br /> IRD : R<br /><br /> IPD : R|ARD : non utilisé<br /><br /> APD : non utilisé<br /><br /> IRD : ND<br /><br /> IPD : ND|  
|SQL_DESC_SCALE|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_SCHEMA_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_SEARCHABLE|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_TABLE_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
|SQL_DESC_TYPE|SQLSMALLINT|ARD : R/W APD : R/W IRD : R IPD : R/W|ARD : SQL_C_DEFAULT APD : SQL_C_DEFAULT IRD : D IPD : ND|  
|SQL_DESC_TYPE_NAME|SQLCHAR|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : APD inutilisé : IRD inutilisé : D IPD : D [1]|  
|SQL_DESC_UNNAMED|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : R/W|ARD : ND APD : ND IRD : D IPD : ND|  
|SQL_DESC_UNSIGNED|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : R|ARD : APD inutilisé : IRD inutilisé : D IPD : D [1]|  
|SQL_DESC_UPDATABLE|SQLSMALLINT|ARD : APD inutilisé : IRD inutilisé : R IPD : inutilisé|ARD : APD inutilisé : IRD inutilisé : D IPD : inutilisé|  
  
 [1] ces champs sont définis uniquement lorsque l’IPD est automatiquement remplie par le pilote. Si ce n’est pas le cas, ils ne sont pas définis. Si une application tente de définir ces champs, SQLSTATE HY091 (identificateur de champ de descripteur non valide) est retourné.  
  
 [2] le champ SQL_DESC_DATA_PTR dans l’IPD peut être défini pour forcer une vérification de cohérence. Dans un appel ultérieur à **SQLGetDescField** ou **SQLGetDescRec**, le pilote n’est pas obligé de retourner la valeur sur laquelle SQL_DESC_DATA_PTR a été défini.  
  
## <a name="fieldidentifier-argument"></a>Argument FieldIdentifier  
 L’argument *FieldIdentifier* indique le champ de descripteur à définir. Un descripteur contient l' *en-tête de descripteur,* composé des champs d’en-tête décrits dans la section suivante, « champs d’en-tête » et zéro ou plusieurs *enregistrements de descripteurs,* composés des champs d’enregistrement décrits dans la section qui suit la section « champs d’en-tête ».  
  
## <a name="header-fields"></a>Champs d’en-tête  
 Chaque descripteur possède un en-tête constitué des champs suivants :  
  
 **SQL_DESC_ALLOC_TYPE [All]**  
 Ce champ d’en-tête SQLSMALLINT en lecture seule spécifie si le descripteur a été alloué automatiquement par le pilote ou explicitement par l’application. L’application peut obtenir ce champ, mais pas le modifier. Le champ est défini sur SQL_DESC_ALLOC_AUTO par le pilote si le descripteur a été automatiquement alloué par le pilote. Elle est définie sur SQL_DESC_ALLOC_USER par le pilote si le descripteur a été explicitement alloué par l’application.  
  
 **SQL_DESC_ARRAY_SIZE [descripteurs d’application]**  
 Dans ARDs, ce champ d’en-tête SQLULEN spécifie le nombre de lignes dans l’ensemble de lignes. Il s’agit du nombre de lignes à retourner par un appel à **SQLFetch** ou **SQLFetchScroll** , ou à un appel à **SQLBulkOperations** ou **SQLSetPos**.  
  
 Dans APD, ce champ d’en-tête SQLULEN spécifie le nombre de valeurs pour chaque paramètre.  
  
 La valeur par défaut de ce champ est 1. Si SQL_DESC_ARRAY_SIZE est supérieur à 1, SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR de APD ou ARD pointent vers des tableaux. La cardinalité de chaque tableau est égale à la valeur de ce champ.  
  
 Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_ROW_ARRAY_SIZE. Ce champ dans le APD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_PARAMSET_SIZE.  
  
 **SQL_DESC_ARRAY_STATUS_PTR [All]**  
 Pour chaque type de descripteur, ce champ d’en-tête SQLUSMALLINT * pointe vers un tableau de valeurs SQLUSMALLINT. Ces tableaux sont nommés comme suit : tableau d’état de ligne (IRD), paramètre de tableau d’état des paramètres (IPD), table d’opération de ligne (ARD) et tableau d’opérations de paramètres (APD).  
  
 Dans IRD, ce champ d’en-tête pointe vers un tableau d’état de ligne contenant les valeurs d’État après un appel à **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** ou **SQLSetPos**. Le tableau contient autant d’éléments qu’il y a de lignes dans l’ensemble de lignes. L’application doit allouer un tableau de SQLUSMALLINTs et définir ce champ pour qu’il pointe vers le tableau. Par défaut, le champ est défini sur un pointeur null. Le pilote remplit le tableau, sauf si le champ SQL_DESC_ARRAY_STATUS_PTR est défini sur un pointeur null, auquel cas aucune valeur d’État n’est générée et le tableau n’est pas rempli.  
  
> [!CAUTION]  
>  Le comportement du pilote n’est pas défini si l’application définit les éléments du tableau d’état de ligne désigné par le champ SQL_DESC_ARRAY_STATUS_PTR de l’IRD.  
  
 Le tableau est initialement rempli par un appel à **SQLBulkOperations**, **SQLFetch**, **SQLFetchScroll** ou **SQLSetPos**. Si l’appel n’a pas retourné SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu du tableau pointé par ce champ n’est pas défini. Les éléments du tableau peuvent contenir les valeurs suivantes :  
  
-   SQL_ROW_SUCCESS : la ligne a été extraite et n’a pas été modifiée depuis sa dernière extraction.  
  
-   SQL_ROW_SUCCESS_WITH_INFO : la ligne a été extraite et n’a pas été modifiée depuis sa dernière extraction. Toutefois, un avertissement a été renvoyé à propos de la ligne.  
  
-   SQL_ROW_ERROR : une erreur s’est produite lors de l’extraction de la ligne.  
  
-   SQL_ROW_UPDATED : la ligne a été récupérée avec succès et a été mise à jour depuis sa dernière extraction. Si la ligne est à nouveau extraite, son état est SQL_ROW_SUCCESS.  
  
-   SQL_ROW_DELETED : la ligne a été supprimée depuis sa dernière extraction.  
  
-   SQL_ROW_ADDED : la ligne a été insérée par **SQLBulkOperations**. Si la ligne est à nouveau extraite, son état est SQL_ROW_SUCCESS.  
  
-   SQL_ROW_NOROW : l’ensemble de lignes chevauche la fin du jeu de résultats et aucune ligne correspondant à cet élément du tableau d’état de ligne n’a été retournée.  
  
 Ce champ dans le IRD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_ROW_STATUS_PTR.  
  
 Le champ SQL_DESC_ARRAY_STATUS_PTR du IRD est valide uniquement après que SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO a été retourné. Si le code de retour n’est pas l’un de ceux-là, l’emplacement vers lequel pointe la SQL_DESC_ROWS_PROCESSED_PTR n’est pas défini.  
  
 Dans l’IPD, ce champ d’en-tête pointe vers un tableau d’état des paramètres contenant des informations d’État pour chaque ensemble de valeurs de paramètre après un appel à **SQLExecute** ou **SQLExecDirect**. Si l’appel à **SQLExecute** ou à **SQLExecDirect** n’a pas retourné SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu du tableau pointé par ce champ n’est pas défini. L’application doit allouer un tableau de SQLUSMALLINTs et définir ce champ pour qu’il pointe vers le tableau. Le pilote remplit le tableau, sauf si le champ SQL_DESC_ARRAY_STATUS_PTR est défini sur un pointeur null, auquel cas aucune valeur d’État n’est générée et le tableau n’est pas rempli. Les éléments du tableau peuvent contenir les valeurs suivantes :  
  
-   SQL_PARAM_SUCCESS : l’instruction SQL a été exécutée avec succès pour cet ensemble de paramètres.  
  
-   SQL_PARAM_SUCCESS_WITH_INFO : l’instruction SQL a été exécutée avec succès pour cet ensemble de paramètres ; Toutefois, les informations d’avertissement sont disponibles dans la structure des données de diagnostic.  
  
-   SQL_PARAM_ERROR : une erreur s’est produite lors du traitement de cet ensemble de paramètres. Des informations supplémentaires sur l’erreur sont disponibles dans la structure des données de diagnostic.  
  
-   SQL_PARAM_UNUSED : ce jeu de paramètres n’était pas utilisé, peut-être en raison du fait qu’un jeu de paramètres précédent a provoqué une erreur qui a abandonné un traitement supplémentaire ou parce que SQL_PARAM_IGNORE a été défini pour cet ensemble de paramètres dans le tableau spécifié par le champ SQL_DESC_ARRAY_STATUS_PTR de l’APD.  
  
-   SQL_PARAM_DIAG_UNAVAILABLE : les informations de diagnostic ne sont pas disponibles. C’est le cas, par exemple, lorsque le pilote traite des tableaux de paramètres comme une unité monolithique et ne génère donc pas ce niveau d’informations d’erreur.  
  
 Ce champ dans l’IPD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_PARAM_STATUS_PTR.  
  
 Dans le ARD, ce champ d’en-tête pointe vers un tableau de valeurs d’opération de ligne qui peut être défini par l’application pour indiquer si cette ligne doit être ignorée pour les opérations **SQLSetPos** . Les éléments du tableau peuvent contenir les valeurs suivantes :  
  
-   SQL_ROW_PROCEED : la ligne est incluse dans l’opération en bloc à l’aide de **SQLSetPos**. (Ce paramètre ne garantit pas que l’opération aura lieu sur la ligne. Si la ligne a l’État SQL_ROW_ERROR dans le tableau d’état de ligne IRD, le pilote peut ne pas être en mesure d’effectuer l’opération dans la ligne.)  
  
-   SQL_ROW_IGNORE : la ligne est exclue de l’opération en bloc à l’aide de **SQLSetPos**.  
  
 Si aucun élément du tableau n’est défini, toutes les lignes sont incluses dans l’opération en bloc. Si la valeur du champ SQL_DESC_ARRAY_STATUS_PTR de l’ARD est un pointeur null, toutes les lignes sont incluses dans l’opération en bloc ; l’interprétation est la même que si le pointeur pointe vers un tableau valide et que tous les éléments du tableau étaient SQL_ROW_PROCEED. Si un élément du tableau est défini sur SQL_ROW_IGNORE, la valeur du tableau d’état de ligne pour la ligne ignorée n’est pas modifiée.  
  
 Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_ROW_OPERATION_PTR.  
  
 Dans le APD, ce champ d’en-tête pointe vers un tableau d’opérations de paramètres qui peut être défini par l’application pour indiquer si cet ensemble de paramètres doit être ignoré lorsque **SQLExecute** ou **SQLExecDirect** est appelé. Les éléments du tableau peuvent contenir les valeurs suivantes :  
  
-   SQL_PARAM_PROCEED : le jeu de paramètres est inclus dans l’appel **SQLExecute** ou **SQLExecDirect** .  
  
-   SQL_PARAM_IGNORE : le jeu de paramètres est exclu de l’appel **SQLExecute** ou **SQLExecDirect** .  
  
 Si aucun élément du tableau n’est défini, tous les jeux de paramètres du tableau sont utilisés dans les appels **SQLExecute** ou **SQLExecDirect** . Si la valeur du champ SQL_DESC_ARRAY_STATUS_PTR du APD est un pointeur null, tous les jeux de paramètres sont utilisés ; l’interprétation est la même que si le pointeur pointe vers un tableau valide et que tous les éléments du tableau étaient SQL_PARAM_PROCEED.  
  
 Ce champ dans le APD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_PARAM_OPERATION_PTR.  
  
 **SQL_DESC_BIND_OFFSET_PTR [descripteurs d’application]**  
 Ce champ d’en-tête SQLLEN * pointe vers le décalage de liaison. Elle est définie sur un pointeur NULL par défaut. Si ce champ n’est pas un pointeur null, le pilote déréférence le pointeur et ajoute la valeur déréférencée à chacun des champs différés ayant une valeur non null dans l’enregistrement du descripteur (SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR) au moment de l’extraction et utilise les nouvelles valeurs de pointeur lors de la liaison.  
  
 Le décalage de liaison est toujours ajouté directement aux valeurs des champs SQL_DESC_DATA_PTR, SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR. Si le décalage est remplacé par une autre valeur, la nouvelle valeur est toujours ajoutée directement à la valeur dans chaque champ de descripteur. Le nouvel offset n’est pas ajouté à la valeur de champ plus un décalage antérieur.  
  
 Ce champ est un *champ différé*: il n’est pas utilisé au moment où il est défini, mais il est utilisé ultérieurement par le pilote lorsqu’il doit déterminer des adresses pour les mémoires tampons de données.  
  
 Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_ROW_BIND_OFFSET_PTR. Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_PARAM_BIND_OFFSET_PTR.  
  
 Pour plus d’informations, consultez la description de la liaison selon les lignes dans [SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md) et [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md).  
  
 **SQL_DESC_BIND_TYPE [descripteurs d’application]**  
 Ce champ d’en-tête SQLUINTEGER définit l’orientation de liaison à utiliser pour la liaison de colonnes ou de paramètres.  
  
 Dans ARDs, ce champ spécifie l’orientation de liaison lorsque **SQLFetchScroll** ou **SQLFetch** est appelé sur le descripteur d’instruction associé.  
  
 Pour sélectionner une liaison selon les colonnes pour les colonnes, ce champ est défini sur SQL_BIND_BY_COLUMN (valeur par défaut).  
  
 Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l' *attribut* SQL_ATTR_ROW_BIND_TYPE.  
  
 Dans APD, ce champ spécifie l’orientation de liaison à utiliser pour les paramètres dynamiques.  
  
 Pour sélectionner une liaison selon les colonnes pour les paramètres, ce champ est défini sur SQL_BIND_BY_COLUMN (valeur par défaut).  
  
 Ce champ dans le APD peut également être défini en appelant **SQLSetStmtAttr** avec l' *attribut* SQL_ATTR_PARAM_BIND_TYPE.  
  
 **SQL_DESC_COUNT [All]**  
 Ce champ d’en-tête SQLSMALLINT spécifie l’index de base 1 de l’enregistrement numéroté le plus élevé qui contient des données. Lorsque le pilote définit la structure de données pour le descripteur, il doit également définir le champ SQL_DESC_COUNT pour afficher le nombre d’enregistrements significatifs. Quand une application alloue une instance de cette structure de données, elle n’a pas besoin de spécifier le nombre d’enregistrements pour lesquels elle doit être réservée. Étant donné que l’application spécifie le contenu des enregistrements, le pilote prend toutes les mesures nécessaires pour s’assurer que le handle de descripteur se réfère à une structure de données de la taille appropriée.  
  
 SQL_DESC_COUNT n’est pas le nombre de toutes les colonnes de données qui sont liées (si le champ se trouve dans un ARD) ou de tous les paramètres liés (si le champ se trouve dans un APD), mais le numéro de l’enregistrement portant le numéro le plus élevé. Si la colonne ou le paramètre avec un numéro le plus élevé est indépendant, SQL_DESC_COUNT est remplacé par le numéro de la colonne ou du paramètre le plus élevé suivant. Si une colonne ou un paramètre avec un nombre inférieur au nombre de la colonne la plus élevée est indépendant (en appelant **SQLBindCol** avec l’argument *TargetValuePtr* défini sur un pointeur null, ou **SQLBindParameter** avec l’argument *ParameterValuePtr* défini sur un pointeur null), SQL_DESC_COUNT n’est pas modifié. Si des colonnes ou des paramètres supplémentaires sont liés par des nombres supérieurs à l’enregistrement numéroté le plus élevé qui contient des données, le pilote augmente automatiquement la valeur dans le champ SQL_DESC_COUNT. Si toutes les colonnes sont indépendantes en appelant **SQLFreeStmt** avec l’option SQL_UNBIND, les champs SQL_DESC_COUNT dans les ARD et IRD sont définis sur 0. Si **SQLFreeStmt** est appelé avec l’option SQL_RESET_PARAMS, les champs SQL_DESC_COUNT dans les APD et IPD ont la valeur 0.  
  
 La valeur de SQL_DESC_COUNT peut être définie explicitement par une application en appelant **SQLSetDescField**. Si la valeur de SQL_DESC_COUNT est explicitement réduite, tous les enregistrements dont le nombre est supérieur à la nouvelle valeur dans SQL_DESC_COUNT sont effectivement supprimés. Si la valeur de SQL_DESC_COUNT est définie explicitement sur 0 et que le champ se trouve dans un ARD, toutes les mémoires tampons de données, à l’exception d’une colonne de signets liée, sont libérées.  
  
 Le nombre d’enregistrements dans ce champ d’un ARD n’inclut pas de colonne de signets liée. La seule façon de dissocier une colonne de signets consiste à définir le champ SQL_DESC_DATA_PTR sur un pointeur null.  
  
 **SQL_DESC_ROWS_PROCESSED_PTR [descripteurs d’implémentation]**  
 Dans un IRD, ce \* champ d’en-tête SQLULEN pointe vers une mémoire tampon contenant le nombre de lignes extraites après un appel à **SQLFetch** ou **SQLFetchScroll**, ou le nombre de lignes affectées dans une opération en bloc effectuée par un appel à **SQLBulkOperations** ou **SQLSetPos**, y compris les lignes d’erreur.  
  
 Dans une IPD, ce champ d’en-tête SQLUINTEGER * pointe vers une mémoire tampon contenant le nombre de jeux de paramètres qui ont été traités, y compris les ensembles d’erreurs. Aucun nombre n’est retourné s’il s’agit d’un pointeur null.  
  
 SQL_DESC_ROWS_PROCESSED_PTR est valide uniquement après SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO a été retourné après un appel à **SQLFetch** ou **SQLFetchScroll** (pour un champ IRD) ou **SQLExecute**, **SQLEXECDIRECT** ou **SQLParamData** (pour un champ IPD). Si l’appel qui remplit la mémoire tampon pointée par ce champ ne retourne pas SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu de la mémoire tampon n’est pas défini, à moins qu’il ne retourne SQL_NO_DATA, auquel cas la valeur de la mémoire tampon est définie sur 0.  
  
 Ce champ dans le ARD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_ROWS_FETCHED_PTR. Ce champ dans le APD peut également être défini en appelant **SQLSetStmtAttr** avec l’attribut SQL_ATTR_PARAMS_PROCESSED_PTR.  
  
 La mémoire tampon vers laquelle pointe ce champ est allouée par l’application. Il s’agit d’une mémoire tampon de sortie différée définie par le pilote. Elle est définie sur un pointeur NULL par défaut.  
  
## <a name="record-fields"></a>Champs d’enregistrement  
 Chaque descripteur contient un ou plusieurs enregistrements composés de champs qui définissent des données de colonne ou des paramètres dynamiques, selon le type de descripteur. Chaque enregistrement est une définition complète d’une seule colonne ou d’un seul paramètre.  
  
 **SQL_DESC_AUTO_UNIQUE_VALUE [IRDs]**  
 Ce champ d’enregistrement SQLINTEGER destinée en lecture seule contient SQL_TRUE si la colonne est une colonne à incrémentation automatique, ou SQL_FALSE si la colonne n’est pas une colonne à incrémentation automatique. Ce champ est en lecture seule, mais la colonne auto-incrémentée sous-jacente n’est pas nécessairement en lecture seule.  
  
 **SQL_DESC_BASE_COLUMN_NAME [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le nom de colonne de base de la colonne du jeu de résultats. Si le nom d’une colonne de base n’existe pas (comme dans le cas des colonnes qui sont des expressions), cette variable contient une chaîne vide.  
  
 **SQL_DESC_BASE_TABLE_NAME [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le nom de la table de base pour la colonne de l’ensemble de résultats. Si un nom de table de base ne peut pas être défini ou s’il n’est pas applicable, cette variable contient une chaîne vide.  
  
 **SQL_DESC_CASE_SENSITIVE [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLINTEGER destinée en lecture seule contient SQL_TRUE si la colonne ou le paramètre est traité comme respectant la casse pour les classements et les comparaisons, ou SQL_FALSE si la colonne n’est pas traitée comme respectant la casse pour les classements et les comparaisons, ou s’il s’agit d’une colonne non-caractère.  
  
 **SQL_DESC_CATALOG_NAME [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le catalogue de la table de base qui contient la colonne. La valeur de retour dépend du pilote si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les catalogues ou si le catalogue ne peut pas être déterminé, cette variable contient une chaîne vide.  
  
 **SQL_DESC_CONCISE_TYPE [All]**  
 Ce champ d’en-tête SQLSMALLINT spécifie le type de données concis pour tous les types de données, y compris les types de données DateTime et Interval.  
  
 Les valeurs des champs SQL_DESC_CONCISE_TYPE, SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE sont interdépendantes. Chaque fois que l’un des champs est défini, l’autre doit également être défini. SQL_DESC_CONCISE_TYPE peut être défini par un appel à **SQLBindCol** ou **SQLBindParameter**, ou **SQLSetDescField**. SQL_DESC_TYPE peut être défini par un appel à **SQLSetDescField** ou **SQLSetDescRec**.  
  
 Si SQL_DESC_CONCISE_TYPE est défini sur un type de données concis autre qu’un type de données Interval ou DateTime, le champ SQL_DESC_TYPE est défini sur la même valeur et le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur 0.  
  
 Si SQL_DESC_CONCISE_TYPE est défini sur le type de données DateTime ou Interval concis, le champ SQL_DESC_TYPE est défini sur le type détaillé correspondant (SQL_DATETIME ou SQL_INTERVAL) et le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur le sous-code approprié.  
  
 **SQL_DESC_DATA_PTR [descripteurs d’application et IPD]**  
 Ce champ d’enregistrement SQLPOINTER pointe vers une variable qui contiendra la valeur de paramètre (pour APD) ou la valeur de colonne (pour ARDs). Ce champ est un *champ différé*. Elle n’est pas utilisée au moment où elle est définie, mais elle est utilisée ultérieurement par le pilote pour récupérer des données.  
  
 La colonne spécifiée par le champ SQL_DESC_DATA_PTR du ARD est indépendante si l’argument *TargetValuePtr* dans un appel à **SQLBindCol** est un pointeur null ou si le champ SQL_DESC_DATA_PTR dans le ARD est défini par un appel à **SQLSetDescField** ou **SQLSetDescRec** à un pointeur null. Les autres champs ne sont pas affectés si le champ SQL_DESC_DATA_PTR est défini sur un pointeur null.  
  
 Si l’appel à **SQLFetch** ou **SQLFetchScroll** qui remplit la mémoire tampon désignée par ce champ n’a pas retourné SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu de la mémoire tampon n’est pas défini.  
  
 Chaque fois que le champ SQL_DESC_DATA_PTR d’un APD, ARD ou IPD est défini, le pilote vérifie que la valeur du champ SQL_DESC_TYPE contient l’un des types de données ODBC C valides ou un type de données spécifique au pilote, et que tous les autres champs affectant les types de données sont cohérents. L’invite d’une vérification de cohérence est la seule utilisation du champ SQL_DESC_DATA_PTR d’une IPD. Plus précisément, si une application définit le champ SQL_DESC_DATA_PTR d’une IPD et qu’elle appelle ensuite **SQLGetDescField** sur ce champ, elle n’est pas nécessairement retournée à la valeur qu’elle avait définie. Pour plus d’informations, consultez « vérifications de cohérence » dans [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_CODE [All]**  
 Ce champ d’enregistrement SQLSMALLINT contient le sous-code du type de données DateTime ou Interval spécifique lorsque le champ SQL_DESC_TYPE est SQL_DATETIME ou SQL_INTERVAL. Cela est vrai pour les types de données SQL et C. Le code se compose du nom du type de données avec « CODE » substitué pour « TYPE » ou « C_TYPE » (pour les types DateTime), ou « CODE » remplacé par « INTERVAL » ou « C_INTERVAL » (pour les types d’intervalle).  
  
 Si SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE dans un descripteur d’application ont la valeur SQL_C_DEFAULT et que le descripteur n’est pas associé à un descripteur d’instruction, le contenu de SQL_DESC_DATETIME_INTERVAL_CODE n’est pas défini.  
  
 Ce champ peut être défini pour les types de données DateTime énumérés dans le tableau suivant.  
  
|Types DateTime|DATETIME_INTERVAL_CODE|  
|--------------------|------------------------------|  
|SQL_TYPE_DATE/SQL_C_TYPE_DATE|SQL_CODE_DATE|  
|SQL_TYPE_TIME/SQL_C_TYPE_TIME|SQL_CODE_TIME|  
|SQL_TYPE_TIMESTAMP/SQL_C_TYPE_TIMESTAMP|SQL_CODE_TIMESTAMP|  
  
 Ce champ peut être défini pour les types de données d’intervalle listés dans le tableau suivant.  
  
|Type d'intervalle|DATETIME_INTERVAL_CODE|  
|-------------------|------------------------------|  
|SQL_INTERVAL_DAY/SQL_C_INTERVAL_DAY|SQL_CODE_DAY|  
|SQL_INTERVAL_DAY_TO_HOUR/SQL_C_INTERVAL_DAY_TO_HOUR|SQL_CODE_DAY_TO_HOUR|  
|SQL_INTERVAL_DAY_TO_MINUTE/SQL_C_INTERVAL_DAY_TO_MINUTE|SQL_CODE_DAY_TO_MINUTE|  
|SQL_INTERVAL_DAY_TO_SECOND/SQL_C_INTERVAL_DAY_TO_SECOND|SQL_CODE_DAY_TO_SECOND|  
|SQL_INTERVAL_HOUR/SQL_C_INTERVAL_HOUR|SQL_CODE_HOUR|  
|SQL_INTERVAL_HOUR_TO_MINUTE/SQL_C_INTERVAL_HOUR_TO_MINUTE|SQL_CODE_HOUR_TO_MINUTE|  
|SQL_INTERVAL_HOUR_TO_SECOND/SQL_C_INTERVAL_HOUR_TO_SECOND|SQL_CODE_HOUR_TO_SECOND|  
|SQL_INTERVAL_MINUTE/SQL_C_INTERVAL_MINUTE|SQL_CODE_MINUTE|  
|SQL_INTERVAL_MINUTE_TO_SECOND/SQL_C_INTERVAL_MINUTE_TO_SECOND|SQL_CODE_MINUTE_TO_SECOND|  
|SQL_INTERVAL_MONTH/SQL_C_INTERVAL_MONTH|SQL_CODE_MONTH|  
|SQL_INTERVAL_SECOND/SQL_C_INTERVAL_SECOND|SQL_CODE_SECOND|  
|SQL_INTERVAL_YEAR/SQL_C_INTERVAL_YEAR|SQL_CODE_YEAR|  
|SQL_INTERVAL_YEAR_TO_MONTH/SQL_C_INTERVAL_YEAR_TO_MONTH|SQL_CODE_YEAR_TO_MONTH|  
  
 Pour plus d’informations sur les intervalles de données et ce champ, consultez [identificateurs et descripteurs des types de données](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md).  
  
 **SQL_DESC_DATETIME_INTERVAL_PRECISION [All]**  
 Ce champ d’enregistrement SQLINTEGER destinée contient la précision de début de l’intervalle si le champ SQL_DESC_TYPE est SQL_INTERVAL. Lorsque le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur un type de données Interval, ce champ est défini sur la précision de l’intervalle par défaut.  
  
 **SQL_DESC_DISPLAY_SIZE [IRDs]**  
 Ce champ d’enregistrement SQLINTEGER destinée en lecture seule contient le nombre maximal de caractères requis pour afficher les données de la colonne.  
  
 **SQL_DESC_FIXED_PREC_SCALE [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLSMALLINT en lecture seule a la valeur SQL_TRUE si la colonne est une colonne numérique exacte et a une précision fixe et une échelle différente de zéro, ou pour SQL_FALSE si la colonne n’est pas une colonne numérique exacte avec une précision et une échelle fixes.  
  
 **SQL_DESC_INDICATOR_PTR [descripteurs d’application]**  
 Dans ARDs, ce champ de l’enregistrement SQLLEN * pointe vers la variable indicateur. Cette variable contient SQL_NULL_DATA si la valeur de colonne est NULL. Pour APD, la variable indicateur est définie sur SQL_NULL_DATA pour spécifier des arguments dynamiques NULL. Sinon, la variable est égale à zéro (sauf si les valeurs de SQL_DESC_INDICATOR_PTR et SQL_DESC_OCTET_LENGTH_PTR sont le même pointeur).  
  
 Si le champ SQL_DESC_INDICATOR_PTR dans un ARD est un pointeur null, le pilote n’est pas en mesure de retourner des informations indiquant si la colonne est NULL ou non. Si la colonne est NULL et que SQL_DESC_INDICATOR_PTR est un pointeur null, SQLSTATE 22002 (variable indicateur requise mais non fournie) est retournée lorsque le pilote tente de remplir la mémoire tampon après un appel à **SQLFetch** ou **SQLFetchScroll**. Si l’appel à **SQLFetch** ou **SQLFetchScroll** n’a pas retourné SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu de la mémoire tampon n’est pas défini.  
  
 Le champ SQL_DESC_INDICATOR_PTR détermine si le champ désigné par SQL_DESC_OCTET_LENGTH_PTR est défini. Si la valeur de données d’une colonne est NULL, le pilote définit la variable d’indicateur sur SQL_NULL_DATA. Le champ vers lequel pointe SQL_DESC_OCTET_LENGTH_PTR n’est pas défini. Si aucune valeur NULL n’est rencontrée pendant l’extraction, la mémoire tampon vers laquelle pointe SQL_DESC_INDICATOR_PTR est définie à zéro et la mémoire tampon vers laquelle pointe SQL_DESC_OCTET_LENGTH_PTR est définie sur la longueur des données.  
  
 Si le champ SQL_DESC_INDICATOR_PTR dans un APD est un pointeur null, l’application ne peut pas utiliser cet enregistrement de descripteur pour spécifier des arguments NULL.  
  
 Ce champ est un *champ différé*: il n’est pas utilisé au moment où il est défini, mais il est utilisé ultérieurement par le pilote pour indiquer la possibilité de valeur null (pour ARDs) ou pour déterminer la possibilité de valeur null (pour APD).  
  
 **SQL_DESC_LABEL [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient l’étiquette ou le titre de la colonne. Si la colonne n’a pas d’étiquette, cette variable contient le nom de la colonne. Si la colonne est sans nom et sans étiquette, cette variable contient une chaîne vide.  
  
 **SQL_DESC_LENGTH [All]**  
 Ce champ d’enregistrement SQLULEN est soit la longueur maximale ou réelle d’une chaîne de caractères en caractères, soit un type de données binaire en octets. Il s’agit de la longueur maximale pour un type de données de longueur fixe, ou de la longueur réelle pour un type de données de longueur variable. Sa valeur exclut toujours le caractère de fin null qui termine la chaîne de caractères. Pour les valeurs dont le type est SQL_TYPE_DATE, SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP ou l’un des types de données SQL Interval, ce champ a la longueur en caractères de la représentation sous forme de chaîne de caractères de la valeur DateTime ou Interval.  
  
 La valeur de ce champ peut être différente de la valeur de « Length » telle que définie dans ODBC 2 *. x*. Pour plus d’informations, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_LITERAL_PREFIX [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le ou les caractères que le pilote reconnaît comme préfixe pour un littéral de ce type de données. Cette variable contient une chaîne vide pour un type de données pour lequel un préfixe littéral n’est pas applicable.  
  
 **SQL_DESC_LITERAL_SUFFIX [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le ou les caractères que le pilote reconnaît comme suffixe pour un littéral de ce type de données. Cette variable contient une chaîne vide pour un type de données pour lequel aucun suffixe littéral n’est applicable.  
  
 **SQL_DESC_LOCAL_TYPE_NAME [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient un nom localisé (langue native) pour le type de données qui peut être différent du nom normal du type de données. S’il n’existe pas de nom localisé, une chaîne vide est retournée. Ce champ est fourni à des fins d’affichage uniquement.  
  
 **SQL_DESC_NAME [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLCHAR * dans un descripteur de ligne contient l’alias de colonne, s’il s’applique. Si l’alias de colonne ne s’applique pas, le nom de colonne est retourné. Dans les deux cas, le pilote définit le champ SQL_DESC_UNNAMED sur SQL_NAMED lorsqu’il définit le champ SQL_DESC_NAME. S’il n’y a pas de nom de colonne ou d’alias de colonne, le pilote retourne une chaîne vide dans le champ SQL_DESC_NAME et définit le champ SQL_DESC_UNNAMED sur SQL_UNNAMED.  
  
 Une application peut définir le champ SQL_DESC_NAME d’une IPD sur un nom de paramètre ou un alias pour spécifier les paramètres de procédure stockée par nom. (Pour plus d’informations, consultez [liaison de paramètres par nom (paramètres nommés)](../../../odbc/reference/develop-app/binding-parameters-by-name-named-parameters.md).) Le champ SQL_DESC_NAME d’un IRD est un champ en lecture seule ; SQLSTATE HY091 (identificateur de champ de descripteur non valide) est retourné si une application tente de la définir.  
  
 Dans IPD, ce champ n’est pas défini si le pilote ne prend pas en charge les paramètres nommés. Si le pilote prend en charge les paramètres nommés et est en capacité de décrire les paramètres, le nom du paramètre est retourné dans ce champ.  
  
 **SQL_DESC_NULLABLE [descripteurs d’implémentation]**  
 Dans IRDs, ce champ d’enregistrement SQLSMALLINT en lecture seule est SQL_NULLABLE si la colonne peut avoir des valeurs NULL, SQL_NO_NULLS si la colonne n’a pas de valeurs NULL, ou SQL_NULLABLE_UNKNOWN s’il n’est pas connu que la colonne accepte les valeurs NULL. Ce champ est lié à la colonne de l’ensemble de résultats, et non à la colonne de base.  
  
 Dans IPD, ce champ a toujours la valeur SQL_NULLABLE, car les paramètres dynamiques sont toujours Nullable et ne peuvent pas être définis par une application.  
  
 **SQL_DESC_NUM_PREC_RADIX [All]**  
 Ce champ SQLINTEGER destinée contient la valeur 2 si le type de données dans le champ SQL_DESC_TYPE est un type de données numérique approximatif, car le champ SQL_DESC_PRECISION contient le nombre de bits. Ce champ contient une valeur de 10 si le type de données dans le champ SQL_DESC_TYPE est un type de données numérique exact, car le champ SQL_DESC_PRECISION contient le nombre de chiffres décimaux. Ce champ a la valeur 0 pour tous les types de données non numériques.  
  
 **SQL_DESC_OCTET_LENGTH [All]**  
 Ce champ d’enregistrement SQLLEN contient la longueur, en octets, d’une chaîne de caractères ou d’un type de données binaire. Pour les types caractère ou binaires de longueur fixe, il s’agit de la longueur réelle en octets. Pour les types caractère ou binaire de longueur variable, il s’agit de la longueur maximale en octets. Cette valeur exclut toujours l’espace pour le caractère de fin null pour les descripteurs d’implémentation et inclut toujours l’espace pour le caractère de fin null pour les descripteurs d’application. Pour les données d’application, ce champ contient la taille de la mémoire tampon. Pour APD, ce champ est défini uniquement pour les paramètres de sortie ou d’entrée/sortie.  
  
 **SQL_DESC_OCTET_LENGTH_PTR [descripteurs d’application]**  
 Ce champ SQLLEN * record pointe vers une variable qui contient la longueur totale, en octets, d’un argument dynamique (pour les descripteurs de paramètre) ou d’une valeur de colonne liée (pour les descripteurs de ligne).  
  
 Pour un APD, cette valeur est ignorée pour tous les arguments, à l’exception des chaînes de caractères et binaires ; Si ce champ pointe vers SQL_NTS, l’argument dynamique doit se terminer par un caractère null. Pour indiquer qu’un paramètre lié est un paramètre de données en cours d’exécution, une application définit ce champ dans l’enregistrement approprié du APD à une variable qui, au moment de l’exécution, contiendra la valeur SQL_DATA_AT_EXEC ou le résultat de la macro SQL_LEN_DATA_AT_EXEC. S’il existe plusieurs champs de ce type, SQL_DESC_DATA_PTR peut être défini sur une valeur qui identifie de façon unique le paramètre pour aider l’application à déterminer quel paramètre est demandé.  
  
 Si le champ OCTET_LENGTH_PTR d’un ARD est un pointeur null, le pilote ne retourne pas d’informations sur la longueur de la colonne. Si le champ SQL_DESC_OCTET_LENGTH_PTR d’un APD est un pointeur null, le pilote part du principe que les chaînes de caractères et les valeurs binaires se terminent par un caractère null. (Les valeurs binaires ne doivent pas être terminées par un caractère null, mais doivent avoir une longueur pour éviter la troncation.)  
  
 Si l’appel à **SQLFetch** ou **SQLFetchScroll** qui remplit la mémoire tampon désignée par ce champ n’a pas retourné SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, le contenu de la mémoire tampon n’est pas défini. Ce champ est un *champ différé*. Elle n’est pas utilisée au moment où elle est définie, mais elle est utilisée ultérieurement par le pilote pour déterminer ou indiquer la longueur en octets des données.  
  
 **SQL_DESC_PARAMETER_TYPE [IPD]**  
 Ce champ d’enregistrement SQLSMALLINT a la valeur SQL_PARAM_INPUT pour un paramètre d’entrée, SQL_PARAM_INPUT_OUTPUT pour un paramètre d’entrée/sortie, SQL_PARAM_OUTPUT pour un paramètre de sortie, SQL_PARAM_INPUT_OUTPUT_STREAM pour un paramètre de flux d’entrée/sortie, ou SQL_PARAM_OUTPUT_STREAM pour un paramètre de sortie en continu. Elle est définie sur SQL_PARAM_INPUT par défaut.  
  
 Pour une IPD, le champ est défini sur SQL_PARAM_INPUT par défaut si l’IPD n’est pas automatiquement remplie par le pilote (l’attribut d’instruction SQL_ATTR_ENABLE_AUTO_IPD est SQL_FALSE). Une application doit définir ce champ dans l’IPD pour les paramètres qui ne sont pas des paramètres d’entrée.  
  
 **SQL_DESC_PRECISION [All]**  
 Ce champ d’enregistrement SQLSMALLINT contient le nombre de chiffres pour un type numérique exact, le nombre de bits dans la mantisse (précision binaire) pour un type numérique approximatif, ou le nombre de chiffres dans le composant fractions de seconde pour le type de données SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP ou SQL_INTERVAL_SECOND. Ce champ n’est pas défini pour tous les autres types de données.  
  
 La valeur de ce champ peut être différente de la valeur de « précision » telle que définie dans ODBC 2 *. x*. Pour plus d’informations, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_ROWVER [descripteurs d’implémentation]**  
 Ce champ SQLSMALLINTrecord indique si une colonne est automatiquement modifiée par le SGBD lorsqu’une ligne est mise à jour (par exemple, une colonne de type « timestamp » dans SQL Server). La valeur de ce champ d’enregistrement est définie sur SQL_TRUE si la colonne est une colonne de contrôle de version de ligne, et sur SQL_FALSE dans le cas contraire. Cet attribut de colonne est similaire à l’appel de **SQLSpecialColumns** avec IdentifierType de SQL_ROWVER pour déterminer si une colonne est automatiquement mise à jour.  
  
 **SQL_DESC_SCALE [All]**  
 Ce champ d’enregistrement SQLSMALLINT contient l’échelle définie pour les types de données decimal et numeric. Le champ n’est pas défini pour tous les autres types de données.  
  
 La valeur de ce champ peut être différente de la valeur de « Scale » telle que définie dans ODBC 2 *. x*. Pour plus d’informations, consultez l' [annexe D : types de données](../../../odbc/reference/appendixes/appendix-d-data-types.md).  
  
 **SQL_DESC_SCHEMA_NAME [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le nom de schéma de la table de base qui contient la colonne. La valeur de retour dépend du pilote si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les schémas ou si le nom du schéma ne peut pas être déterminé, cette variable contient une chaîne vide.  
  
 **SQL_DESC_SEARCHABLE [IRDs]**  
 Ce champ d’enregistrement SQLSMALLINT en lecture seule est défini sur l’une des valeurs suivantes :  
  
-   SQL_PRED_NONE si la colonne ne peut pas être utilisée dans une clause **Where** . (Il est identique à la valeur SQL_UNSEARCHABLE dans ODBC 2 *. x*).  
  
-   SQL_PRED_CHAR si la colonne peut être utilisée dans une clause **Where** , mais uniquement avec le prédicat **Like** . (Il est identique à la valeur SQL_LIKE_ONLY dans ODBC 2 *. x*).  
  
-   SQL_PRED_BASIC si la colonne peut être utilisée dans une clause **Where** avec tous les opérateurs de comparaison, à l’exception de **Like**. (Il est identique à la valeur SQL_EXCEPT_LIKE dans ODBC 2 *. x*).  
  
-   SQL_PRED_SEARCHABLE si la colonne peut être utilisée dans une clause **Where** avec un opérateur de comparaison.  
  
 **SQL_DESC_TABLE_NAME [IRDs]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le nom de la table de base qui contient cette colonne. La valeur de retour dépend du pilote si la colonne est une expression ou si la colonne fait partie d’une vue.  
  
 **SQL_DESC_TYPE [All]**  
 Ce champ d’enregistrement SQLSMALLINT spécifie le type de données SQL ou C concis pour tous les types de données, à l’exception des types de données DateTime et Interval. Pour les types de données DateTime et Interval, ce champ spécifie le type de données verbose, qui est SQL_DATETIME ou SQL_INTERVAL.  
  
 Chaque fois que ce champ contient SQL_DATETIME ou SQL_INTERVAL, le champ SQL_DESC_DATETIME_INTERVAL_CODE doit contenir le sous-code approprié pour le type concis. Pour les types de données DateTime, SQL_DESC_TYPE contient SQL_DATETIME et le champ SQL_DESC_DATETIME_INTERVAL_CODE contient un sous-code pour le type de données DateTime spécifique. Pour les types de données Interval, SQL_DESC_TYPE contient SQL_INTERVAL et le champ SQL_DESC_DATETIME_INTERVAL_CODE contient un sous-code pour le type de données Interval spécifique.  
  
 Les valeurs des champs SQL_DESC_TYPE et SQL_DESC_CONCISE_TYPE sont interdépendantes. Chaque fois que l’un des champs est défini, l’autre doit également être défini. SQL_DESC_TYPE peut être défini par un appel à **SQLSetDescField** ou **SQLSetDescRec**. SQL_DESC_CONCISE_TYPE peut être défini par un appel à **SQLBindCol** ou **SQLBindParameter**, ou **SQLSetDescField**.  
  
 Si SQL_DESC_TYPE est défini sur un type de données concis autre qu’un type de données Interval ou DateTime, le champ SQL_DESC_CONCISE_TYPE est défini sur la même valeur et le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur 0.  
  
 Si SQL_DESC_TYPE est défini sur le type de données DateTime ou Interval détaillé (SQL_DATETIME ou SQL_INTERVAL) et si le champ SQL_DESC_DATETIME_INTERVAL_CODE est défini sur le sous-code approprié, le champ de TYPE SQL_DESC_CONCISE est défini sur le type concis correspondant. Si vous tentez de définir SQL_DESC_TYPE sur l’un des types DateTime ou Interval concis, vous retournerez SQLSTATE HY021 (informations de descripteur incohérentes).  
  
 Lorsque le champ SQL_DESC_TYPE est défini par un appel à **SQLBindCol**, **SQLBindParameter** ou **SQLSetDescField**, les champs suivants sont définis sur les valeurs par défaut suivantes, comme indiqué dans le tableau ci-dessous. Les valeurs des champs restants du même enregistrement ne sont pas définies.  
  
|Valeur de SQL_DESC_TYPE|Autres champs définis implicitement|  
|------------------------------|---------------------------------|  
|SQL_CHAR, SQL_VARCHAR, SQL_C_CHAR, SQL_C_VARCHAR|SQL_DESC_LENGTH a la valeur 1. SQL_DESC_PRECISION a la valeur 0.|  
|SQL_DATETIME|Lorsque SQL_DESC_DATETIME_INTERVAL_CODE a la valeur SQL_CODE_DATE ou SQL_CODE_TIME, SQL_DESC_PRECISION a la valeur 0. Lorsqu’il est défini sur SQL_DESC_TIMESTAMP, SQL_DESC_PRECISION a la valeur 6.|  
|SQL_DECIMAL, SQL_NUMERIC SQL_C_NUMERIC|SQL_DESC_SCALE a la valeur 0. SQL_DESC_PRECISION est défini sur la précision définie par l’implémentation pour le type de données respectif.<br /><br /> Pour plus d’informations sur la liaison manuelle d’une valeur SQL_C_NUMERIC, consultez [SQL to C : Numeric](../../../odbc/reference/appendixes/sql-to-c-numeric.md) .|  
|SQL_FLOAT, SQL_C_FLOAT|SQL_DESC_PRECISION est défini sur la précision par défaut définie par l’implémentation pour SQL_FLOAT.|  
|SQL_INTERVAL|Lorsque SQL_DESC_DATETIME_INTERVAL_CODE est défini sur un type de données Interval, SQL_DESC_DATETIME_INTERVAL_PRECISION a la valeur 2 (précision d’intervalle par défaut). Lorsque l’intervalle comporte un composant seconds, SQL_DESC_PRECISION a la valeur 6 (la précision par défaut de l’intervalle en secondes).|  
  
 Quand une application appelle **SQLSetDescField** pour définir des champs d’un descripteur plutôt que d’appeler **SQLSetDescRec**, l’application doit d’abord déclarer le type de données. Dans ce cas, les autres champs indiqués dans le tableau précédent sont définis de manière implicite. Si l’une des valeurs définies implicitement est inacceptable, l’application peut ensuite appeler **SQLSetDescField** ou **SQLSetDescRec** pour définir explicitement la valeur inacceptable.  
  
 **SQL_DESC_TYPE_NAME [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLCHAR * en lecture seule contient le nom du type dépendant de la source de données (par exemple, « CHAR », « VARCHAR », etc.). Si le nom du type de données est inconnu, cette variable contient une chaîne vide.  
  
 **SQL_DESC_UNNAMED [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLSMALLINT dans un descripteur de ligne est défini par le pilote à SQL_NAMED ou SQL_UNNAMED lorsqu’il définit le champ SQL_DESC_NAME. Si le champ SQL_DESC_NAME contient un alias de colonne ou si l’alias de colonne ne s’applique pas, le pilote définit le champ SQL_DESC_UNNAMED sur SQL_NAMED. Si une application définit le champ SQL_DESC_NAME d’une IPD sur un nom de paramètre ou un alias, le pilote définit le champ SQL_DESC_UNNAMED de l’IPD sur SQL_NAMED. S’il n’y a pas de nom de colonne ou d’alias de colonne, le pilote définit le champ SQL_DESC_UNNAMED sur SQL_UNNAMED.  
  
 Une application peut définir le champ SQL_DESC_UNNAMED d’une IPD sur SQL_UNNAMED. Un pilote retourne SQLSTATE HY091 (identificateur de champ de descripteur non valide) si une application tente de définir le champ SQL_DESC_UNNAMED d’une IPD sur SQL_NAMED. Le champ SQL_DESC_UNNAMED d’un IRD est en lecture seule ; SQLSTATE HY091 (identificateur de champ de descripteur non valide) est retourné si une application tente de la définir.  
  
 **SQL_DESC_UNSIGNED [descripteurs d’implémentation]**  
 Ce champ d’enregistrement SQLSMALLINT en lecture seule est défini sur SQL_TRUE si le type de colonne est non signé ou non numérique, ou SQL_FALSE si le type de colonne est signé.  
  
 **SQL_DESC_UPDATABLE [IRDs]**  
 Ce champ d’enregistrement SQLSMALLINT en lecture seule est défini sur l’une des valeurs suivantes :  
  
-   SQL_ATTR_READ_ONLY si la colonne de l’ensemble de résultats est en lecture seule.  
  
-   SQL_ATTR_WRITE si la colonne de l’ensemble de résultats est en lecture-écriture.  
  
-   SQL_ATTR_READWRITE_UNKNOWN s’il n’est pas connu que la colonne de l’ensemble de résultats peut être mise à jour ou non.  
  
 SQL_DESC_UPDATABLE décrit la mise à jour de la colonne dans le jeu de résultats, et non la colonne dans la table de base. La possibilité de mise à jour de la colonne dans la table de base sur laquelle est basée cette colonne de jeu de résultats peut être différente de la valeur de ce champ. La possibilité de mettre à jour une colonne peut être basée sur le type de données, les privilèges de l’utilisateur et la définition du jeu de résultats lui-même. S’il est impossible de savoir si une colonne peut être mise à jour, SQL_ATTR_READWRITE_UNKNOWN doit être retourné.  
  
## <a name="consistency-checks"></a>Vérifications de cohérence  
 Une vérification de cohérence est effectuée automatiquement par le pilote chaque fois qu’une application transmet une valeur pour le champ SQL_DESC_DATA_PTR de la ARD, APD ou IPD. Si l’un des champs est incohérent avec d’autres champs, **SQLSetDescField** retourne SQLState HY021 (informations de descripteur incohérentes). Pour plus d’informations, consultez « vérification de cohérence » dans [SQLSetDescRec](../../../odbc/reference/syntax/sqlsetdescrec-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une colonne|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Liaison d’un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtention d’un champ de descripteur|[Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition de plusieurs champs de descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)
