---
description: Fonction SQLColAttribute
title: SQLColAttribute, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLColAttribute
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLColAttribute
helpviewer_keywords:
- SQLColAttribute function [ODBC]
ms.assetid: 8c45c598-cb01-4789-a571-e93619a18ed9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 43980caf7a592284aba87d57725c30ae9fe8e949
ms.sourcegitcommit: e40e75055c1435c5e3f9b6e3246be55526807b4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98151244"
---
# <a name="sqlcolattribute-function"></a>Fonction SQLColAttribute
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLColAttribute** retourne les informations de descripteur pour une colonne dans un jeu de résultats. Les informations de descripteur sont retournées sous forme de chaîne de caractères, de valeur dépendante d’un descripteur ou d’une valeur entière.  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon dont le gestionnaire de pilotes mappe cette fonction quand ODBC 3. l’application *x* fonctionne avec ODBC 2. *x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLColAttribute (  
      SQLHSTMT        StatementHandle,  
      SQLUSMALLINT    ColumnNumber,  
      SQLUSMALLINT    FieldIdentifier,  
      SQLPOINTER      CharacterAttributePtr,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLLEN *        NumericAttributePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ColumnNumber*  
 Entrée Numéro de l’enregistrement dans le IRD à partir duquel la valeur de champ doit être récupérée. Cet argument correspond au numéro de colonne des données de résultat, ordonné séquentiellement dans l’ordre croissant des colonnes, à partir de 1. Les colonnes peuvent être décrites dans n’importe quel ordre.  
  
 La colonne 0 peut être spécifiée dans cet argument, mais toutes les valeurs, à l’exception de SQL_DESC_TYPE et SQL_DESC_OCTET_LENGTH, retournent des valeurs non définies.  
  
 *FieldIdentifier*  
 Entrée Handle du descripteur. Ce handle définit le champ du IRD à interroger (par exemple, SQL_COLUMN_TABLE_NAME).  
  
 *CharacterAttributePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur dans le champ *FieldIdentifier* de la ligne *ColumnNumber* du IRD, si le champ est une chaîne de caractères. Dans le cas contraire, le champ n’est pas utilisé.  
  
 Si *CharacterAttributePtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *CharacterAttributePtr*.  
  
 *BufferLength*  
 Entrée Si *FieldIdentifier* est un champ défini par ODBC et que *CharacterAttributePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de \* *CharacterAttributePtr*. Si *FieldIdentifier* est un champ défini par ODBC et que \* *CharacterAttribute* PTR est un entier, ce champ est ignoré. Si *\* CharacterAttributePtr* est une chaîne Unicode (lors de l’appel de **SQLColAttributeW**), l’argument *BufferLength* doit être un nombre pair. Si *FieldIdentifier* est un champ défini par le pilote, l’application indique la nature du champ au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *CharacterAttributePtr* est un pointeur vers un pointeur, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *CharacterAttributePtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la mémoire tampon.  
  
-   Si *CharacterAttributePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *CharacterAttributePtr* est un pointeur vers un type de données de longueur fixe, *BufferLength* doit être l’un des éléments suivants : SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT ou SQL_IS_USMALLINT.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exclusion de l’octet de fin null pour les données caractères) disponible pour le retour dans **CharacterAttributePtr*.  
  
 Pour les données de type caractère, si le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength*, les informations du descripteur dans \* *CharacterAttributePtr* sont tronquées à *BufferLength* moins la longueur d’un caractère de fin null et le pilote se termine par un caractère null.  
  
 Pour tous les autres types de données, la valeur de *BufferLength* est ignorée et le pilote suppose que la taille de **CharacterAttributePtr* est de 32 bits.  
  
 *NumericAttributePtr*  
 Sortie Pointeur vers une mémoire tampon d’entiers dans laquelle retourner la valeur dans le champ *FieldIdentifier* de la ligne *ColumnNumber* du IRD, si le champ est un type de descripteur numérique, tel que SQL_DESC_COLUMN_LENGTH. Dans le cas contraire, le champ n’est pas utilisé. Notez que certains pilotes peuvent uniquement écrire la version inférieure de 32 bits ou 16 bits d’une mémoire tampon et que le bit d’ordre supérieur n’est pas modifié. Par conséquent, les applications doivent initialiser la valeur à 0 avant d’appeler cette fonction.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_STILL_EXECUTING, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLColAttribute** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLColAttribute** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le CharacterAttributePtr de mémoire tampon \*  n’est pas assez grand pour retourner la valeur de chaîne entière, donc la valeur de chaîne a été tronquée. La longueur de la valeur de chaîne non tronquée est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07005|L’instruction préparée n’est pas une *spécification de curseur*|L’instruction associée à *StatementHandle* n’a pas retourné de jeu de résultats et *FieldIdentifier* n’a pas été SQL_DESC_COUNT. Aucune colonne à décrire.|  
|07009|Index de descripteur non valide|(DM) la valeur spécifiée pour *ColumnNumber* était égale à 0 et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS était SQL_UB_OFF.<br /><br /> La valeur spécifiée pour l’argument *ColumnNumber* est supérieure au nombre de colonnes dans le jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagField** à partir de la structure de données de diagnostic décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*. Ensuite, la fonction a été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction aynchronous était toujours en cours d’exécution lors de l’appel de SQLColAttribute.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) la fonction a été appelée avant d’appeler **SQLPrepare**, **SQLExecDirect** ou une fonction de catalogue pour *StatementHandle*.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) *\* CharacterAttributePtr* est une chaîne de caractères et *BufferLength* a une valeur inférieure à 0 mais n’est pas égale à SQL_NTS.|  
|HY091|Identificateur de champ de descripteur non valide|La valeur spécifiée pour l’argument *FieldIdentifier* n’était pas l’une des valeurs définies et n’était pas une valeur définie par l’implémentation.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Pilote non conforme|La valeur spécifiée pour l’argument *FieldIdentifier* n’est pas prise en charge par le pilote.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
 En cas d’appel après **SQLPrepare** et avant **SQLExecute**, **SQLColAttribute** peut retourner tout SQLSTATE pouvant être retourné par **SQLPrepare** ou **SQLExecute**, en fonction du moment où la source de données évalue l’instruction SQL associée à *StatementHandle*.  
  
 Pour des raisons de performances, une application ne doit pas appeler **SQLColAttribute** avant d’exécuter une instruction.  
  
## <a name="comments"></a>Commentaires  
 Pour plus d’informations sur la façon dont les applications utilisent les informations retournées par **SQLColAttribute**, consultez [métadonnées du jeu de résultats](../../../odbc/reference/develop-app/result-set-metadata.md).  
  
 **SQLColAttribute** retourne des informations dans \* *NumericAttributePtr* ou dans \* *CharacterAttributePtr*. Les informations sur les entiers sont retournées dans \* *NumericAttributePtr* en tant que valeur sqllen ; tous les autres formats d’informations sont retournés dans \* *CharacterAttributePtr*. Lorsque des informations sont retournées dans \* *NumericAttributePtr*, le pilote ignore *CharacterAttributePtr*, *BufferLength* et *StringLengthPtr*. Lorsque des informations sont retournées dans \* *CharacterAttributePtr*, le pilote ignore *NumericAttributePtr*.  
  
 **SQLColAttribute** retourne des valeurs à partir des champs de descripteur du IRD. La fonction est appelée avec un handle d’instruction plutôt qu’un handle de descripteur. Les valeurs retournées par **SQLColAttribute** pour les valeurs *FieldIdentifier* indiquées plus loin dans cette section peuvent également être récupérées en appelant **SQLGetDescField** avec le handle IRD approprié.  
  
 Les champs de descripteur actuellement définis, la version d’ODBC dans laquelle ils ont été introduits et les arguments dans lesquels les informations sont retournées sont présentés plus loin dans cette section. d’autres types de descripteurs peuvent être définis par des pilotes pour tirer parti de différentes sources de données.  
  
 ODBC 3. *x* le pilote doit retourner une valeur pour chacun des champs du descripteur. Si un champ de descripteur ne s’applique pas à un pilote ou à une source de données et sauf indication contraire, le pilote retourne 0 dans \* *StringLengthPtr* ou une chaîne vide dans **CharacterAttributePtr*.  
  
## <a name="backward-compatibility"></a>Backward Compatibility  
 ODBC 3. la fonction *x* **SQLCOLATTRIBUTE** remplace le ODBC 2 déconseillé.  fonction x **SQLColAttributes**. Lors du mappage de **SQLColAttributes** à **SQLCOLATTRIBUTE** (quand ODBC 2.** l’application x fonctionne avec ODBC 3. *x* ) ou en mappant **SQLColAttribute** à **SQLColAttributes** (quand ODBC 3.** l’application x fonctionne avec ODBC 2. *x* ), le gestionnaire de pilotes transmet la valeur de *FieldIdentifier* à, le mappe à une nouvelle valeur, ou retourne une erreur, comme suit :  
  
> [!NOTE]  
>  Préfixe utilisé dans les valeurs *FieldIdentifier* dans ODBC 3. *x* a été modifié par rapport à celui utilisé dans ODBC 2. *x*. Le nouveau préfixe est « SQL_DESC ». l’ancien préfixe était « SQL_COLUMN ».  
  
-   Si la valeur **#define** de ODBC 2. *x* *FieldIdentifier* est identique à la valeur **#define** de ODBC 3. *x* *FieldIdentifier*, la valeur dans l’appel de fonction est simplement transmise.  
  
-   Les valeurs **#define** de ODBC 2. *x* *FieldIdentifiers* SQL_COLUMN_LENGTH, SQL_COLUMN_PRECISION et SQL_COLUMN_SCALE sont différents des valeurs **#define** de ODBC 3. *x* *FieldIdentifiers* SQL_DESC_PRECISION, SQL_DESC_SCALE et SQL_DESC_LENGTH. ODBC 2. *x* le pilote doit uniquement prendre en charge ODBC 2. valeurs *x* . ODBC 3. *x* le pilote doit prendre en charge les valeurs « SQL_COLUMN » et « SQL_DESC » pour ces trois *FieldIdentifiers*. Ces valeurs sont différentes, car la précision, l’échelle et la longueur sont définies différemment dans ODBC 3. *x* qu’elles étaient dans ODBC 2. *x*. Pour plus d’informations, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md).  
  
-   Si la valeur **#define** de ODBC 2. *x* *FieldIdentifier* est différent de la valeur **#define** de ODBC 3. *x* *FieldIdentifier*, comme c’est le cas avec le nombre, le nom et les valeurs Nullable, la valeur de l’appel de fonction est mappée à la valeur correspondante. Par exemple, SQL_COLUMN_COUNT est mappé à SQL_DESC_COUNT et SQL_DESC_COUNT est mappé à SQL_COLUMN_COUNT, en fonction de la direction du mappage.  
  
-   Si *FieldIdentifier* est une nouvelle valeur dans ODBC 3. *x*, pour laquelle il n’existait aucune valeur correspondante dans ODBC 2. *x*, il ne sera pas MAPPÉ quand ODBC 3. l’application *x* l’utilise dans un appel à **SQLCOLATTRIBUTE** dans un ODBC 2. *x* , et l’appel retourne SQLSTATE HY091 (identificateur de champ de descripteur non valide).  
  
 Le tableau suivant répertorie les types de descripteurs retournés par **SQLColAttribute**. Le type des valeurs *NumericAttributePtr* est **sqllen \** _.  
  
|_FieldIdentifier *|Information<br /><br /> retourné dans|Description|  
|-----------------------|---------------------------------|-----------------|  
|SQL_DESC_AUTO_UNIQUE_VALUE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la colonne est une colonne auto-incrémentée.<br /><br /> SQL_FALSE si la colonne n’est pas une colonne auto-incrémentée ou n’est pas numérique.<br /><br /> Ce champ est valide uniquement pour les colonnes de type de données numeric. Une application peut insérer des valeurs dans une ligne contenant une colonne AutoIncrement, mais ne peut généralement pas mettre à jour des valeurs dans la colonne.<br /><br /> Lorsqu’une insertion est effectuée dans une colonne AutoIncrement, une valeur unique est insérée dans la colonne au moment de l’insertion. L’incrément n’est pas défini, mais il est spécifique à la source de données. Une application ne doit pas supposer qu’une colonne AutoIncrement démarre à un point particulier ou qu’elle incrémente une valeur particulière.|  
|SQL_DESC_BASE_COLUMN_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nom de la colonne de base de la colonne du jeu de résultats. Si le nom d’une colonne de base n’existe pas (comme dans le cas des colonnes qui sont des expressions), cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_BASE_COLUMN_NAME de l’IRD, qui est un champ en lecture seule.|  
|SQL_DESC_BASE_TABLE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Nom de la table de base qui contient la colonne. Si le nom de la table de base ne peut pas être défini ou s’il n’est pas applicable, cette variable contient une chaîne vide.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_BASE_TABLE_NAME de l’IRD, qui est un champ en lecture seule.|  
|SQL_DESC_CASE_SENSITIVE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la colonne est traitée comme respectant la casse pour les classements et les comparaisons.<br /><br /> SQL_FALSE si la colonne n’est pas traitée comme respectant la casse pour les classements et les comparaisons ou n’est pas un caractère.|  
|SQL_DESC_CATALOG_NAME (ODBC 2,0)|*CharacterAttributePtr*|Catalogue de la table qui contient la colonne. La valeur retournée est définie par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les catalogues ou si le nom du catalogue ne peut pas être déterminé, une chaîne vide est retournée. Ce champ d’enregistrement VARCHAR n’est pas limité à 128 caractères.|  
|SQL_DESC_CONCISE_TYPE (ODBC 1,0)|*NumericAttributePtr*|Type de données concis.<br /><br /> Pour les types de données DateTime et Interval, ce champ retourne le type de données concis ; par exemple, SQL_TYPE_TIME ou SQL_INTERVAL_YEAR. (Pour plus d’informations, consultez [identificateurs et descripteurs](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) des types de données dans l’annexe D : types de données.)<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_CONCISE_TYPE du IRD.|  
|SQL_DESC_COUNT (ODBC 1,0)|*NumericAttributePtr*|Nombre de colonnes disponibles dans le jeu de résultats. La valeur 0 est retournée s’il n’y a pas de colonnes dans le jeu de résultats. La valeur de l’argument *ColumnNumber* est ignorée.<br /><br /> Ces informations sont retournées à partir du champ d’en-tête SQL_DESC_COUNT de l’IRD.|  
|SQL_DESC_DISPLAY_SIZE (ODBC 1,0)|*NumericAttributePtr*|Nombre maximal de caractères requis pour afficher les données de la colonne. Pour plus d’informations sur la taille d’affichage, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|SQL_DESC_FIXED_PREC_SCALE (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la colonne a une précision fixe et une échelle différente de zéro qui sont spécifiques à la source de données.<br /><br /> SQL_FALSE si la colonne n’a pas une précision fixe et une échelle différente de zéro qui sont spécifiques à la source de données.|  
|SQL_DESC_LABEL (ODBC 2,0)|*CharacterAttributePtr*|Étiquette ou titre de la colonne. Par exemple, une colonne nommée EmpName peut être nommée Employee Name ou peut être étiquetée avec un alias.<br /><br /> Si une colonne n’a pas d’étiquette, le nom de la colonne est retourné. Si la colonne n’a pas d’étiquette et qu’elle n’a pas de nom, une chaîne vide est retournée.|  
|SQL_DESC_LENGTH (ODBC 3,0)|*NumericAttributePtr*|Valeur numérique correspondant à la longueur maximale ou à la longueur de caractère réelle d’une chaîne de caractères ou d’un type de données binaire. Il s’agit de la longueur de caractère maximale pour un type de données de longueur fixe, ou de la longueur de caractère réelle pour un type de données de longueur variable. Sa valeur exclut toujours l’octet de fin null qui termine la chaîne de caractères.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_LENGTH du IRD.<br /><br /> Pour plus d’informations sur la longueur, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|SQL_DESC_LITERAL_PREFIX (ODBC 3,0)|*CharacterAttributePtr*|Ce champ d’enregistrement VARCHAR (128) contient le ou les caractères que le pilote reconnaît comme préfixe pour un littéral de ce type de données. Ce champ contient une chaîne vide pour un type de données pour lequel un préfixe littéral n’est pas applicable. Pour plus d’informations, consultez [préfixes et suffixes littéraux](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LITERAL_SUFFIX (ODBC 3,0)|*CharacterAttributePtr*|Ce champ d’enregistrement VARCHAR (128) contient le ou les caractères que le pilote reconnaît comme suffixe pour un littéral de ce type de données. Ce champ contient une chaîne vide pour un type de données pour lequel aucun suffixe littéral n’est applicable. Pour plus d’informations, consultez [préfixes et suffixes littéraux](../../../odbc/reference/develop-app/literal-prefixes-and-suffixes.md).|  
|SQL_DESC_LOCAL_TYPE_NAME (ODBC 3,0)|*CharacterAttributePtr*|Ce champ d’enregistrement VARCHAR (128) contient un nom localisé (langue native) pour le type de données qui peut être différent du nom normal du type de données. S’il n’y a pas de nom localisé, une chaîne vide est retournée. Ce champ est fourni à des fins d’affichage uniquement. Le jeu de caractères de la chaîne dépend des paramètres régionaux et est généralement le jeu de caractères par défaut du serveur.|  
|SQL_DESC_NAME (ODBC 3,0)|*CharacterAttributePtr*|Alias de colonne, s’il s’applique. Si l’alias de colonne ne s’applique pas, le nom de colonne est retourné. Dans les deux cas, SQL_DESC_UNNAMED est défini sur SQL_NAMED. S’il n’y a pas de nom de colonne ou d’alias de colonne, une chaîne vide est retournée et SQL_DESC_UNNAMED a la valeur SQL_UNNAMED.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_NAME du IRD.|  
|SQL_DESC_NULLABLE (ODBC 3,0)|*NumericAttributePtr*|SQL_ la valeur NULL si la colonne peut avoir des valeurs NULL ; SQL_NO_NULLS si la colonne n’a pas de valeurs NULL ; ou SQL_NULLABLE_UNKNOWN s’il n’est pas connu que la colonne accepte les valeurs NULL.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_NULLABLE du IRD.|  
|SQL_DESC_NUM_PREC_RADIX (ODBC 3,0)|*NumericAttributePtr*|Si le type de données dans le champ SQL_DESC_TYPE est un type de données numérique approximatif, ce champ SQLINTEGER destinée contient la valeur 2, car le champ SQL_DESC_PRECISION contient le nombre de bits. Si le type de données dans le champ SQL_DESC_TYPE est un type de données numérique exact, ce champ contient une valeur de 10, car le champ SQL_DESC_PRECISION contient le nombre de chiffres décimaux. Ce champ a la valeur 0 pour tous les types de données non numériques.|  
|SQL_DESC_OCTET_LENGTH (ODBC 3,0)|*NumericAttributePtr*|Longueur, en octets, d’une chaîne de caractères ou d’un type de données binaire. Pour les types caractère ou binaires de longueur fixe, il s’agit de la longueur réelle en octets. Pour les types caractère ou binaire de longueur variable, il s’agit de la longueur maximale en octets. Cette valeur n’inclut pas la marque de fin null.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_OCTET_LENGTH du IRD.<br /><br /> Pour plus d’informations sur la longueur, consultez [taille de colonne, chiffres décimaux, longueur d’octet de transfert et taille d’affichage](../../../odbc/reference/appendixes/column-size-decimal-digits-transfer-octet-length-and-display-size.md) dans l’annexe D : types de données.|  
|SQL_DESC_PRECISION (ODBC 3,0)|*NumericAttributePtr*|Une valeur numérique pour un type de données numérique indique la précision applicable. Pour les types de données SQL_TYPE_TIME, SQL_TYPE_TIMESTAMP et tous les types de données Interval représentant un intervalle de temps, sa valeur est la précision applicable du composant fractions de seconde.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_PRECISION du IRD.|  
|SQL_DESC_SCALE (ODBC 3,0)|*NumericAttributePtr*|Valeur numérique correspondant à l’échelle applicable pour un type de données numérique. Pour les types de données DECIMAL et NUMERIC, il s’agit de l’échelle définie. Elle n’est pas définie pour tous les autres types de données.<br /><br /> Ces informations sont retournées à partir du champ de l’enregistrement de mise à l’échelle de IRD.|  
|SQL_DESC_SCHEMA_NAME (ODBC 2,0)|*CharacterAttributePtr*|Schéma de la table qui contient la colonne. La valeur retournée est définie par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue. Si la source de données ne prend pas en charge les schémas ou si le nom du schéma ne peut pas être déterminé, une chaîne vide est retournée. Ce champ d’enregistrement VARCHAR n’est pas limité à 128 caractères.|  
|SQL_DESC_SEARCHABLE (ODBC 1,0)|*NumericAttributePtr*|SQL_PRED_NONE si la colonne ne peut pas être utilisée dans une clause WHERE. (C’est identique à la valeur SQL_UNSEARCHABLE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_CHAR si la colonne peut être utilisée dans une clause WHERE, mais uniquement avec le prédicat LIKE. (C’est identique à la valeur SQL_LIKE_ONLY dans ODBC 2. *x*.)<br /><br /> SQL_PRED_BASIC si la colonne peut être utilisée dans une clause WHERE avec tous les opérateurs de comparaison, à l’exception de LIKE. (C’est identique à la valeur SQL_EXCEPT_LIKE dans ODBC 2. *x*.)<br /><br /> SQL_PRED_SEARCHABLE si la colonne peut être utilisée dans une clause WHERE avec un opérateur de comparaison.<br /><br /> Les colonnes de type SQL_LONGVARCHAR et SQL_LONGVARBINARY retournent généralement SQL_PRED_CHAR.|  
|SQL_DESC_TABLE_NAME (ODBC 2,0)|*CharacterAttributePtr*|Nom de la table qui contient la colonne. La valeur retournée est définie par l’implémentation si la colonne est une expression ou si la colonne fait partie d’une vue.<br /><br /> Si le nom de la table ne peut pas être déterminé, une chaîne vide est retournée.|  
|SQL_DESC_TYPE (ODBC 3,0)|*NumericAttributePtr*|Valeur numérique qui spécifie le type de données SQL.<br /><br /> Lorsque *ColumnNumber* est égal à 0, SQL_BINARY est retourné pour les signets de longueur variable et SQL_INTEGER est retourné pour les signets de longueur fixe.<br /><br /> Pour les types de données DateTime et Interval, ce champ retourne le type de données verbose : SQL_DATETIME ou SQL_INTERVAL. (Pour plus d’informations, consultez [identificateurs et descripteurs](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md) des types de données dans l’annexe D : types de données.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_TYPE du IRD. **Remarque :**  Pour travailler sur ODBC 2. *x* , utilisez SQL_DESC_CONCISE_TYPE à la place.|  
|SQL_DESC_TYPE_NAME (ODBC 1,0)|*CharacterAttributePtr*|Nom du type de données dépendant de la source de données ; par exemple, « CHAR », « VARCHAR », « MONEY », « LONG VARBINARY » ou « CHAR () FOR BIT DATA ».<br /><br /> Si le type est inconnu, une chaîne vide est retournée.|  
|SQL_DESC_UNNAMED (ODBC 3,0)|*NumericAttributePtr*|SQL_NAMED ou SQL_UNNAMED. Si le champ SQL_DESC_NAME de la IRD contient un alias de colonne ou un nom de colonne, SQL_NAMED est retourné. S’il n’y a pas de nom de colonne ou d’alias de colonne, SQL_UNNAMED est retourné.<br /><br /> Ces informations sont retournées à partir du champ d’enregistrement SQL_DESC_UNNAMED du IRD.|  
|SQL_DESC_UNSIGNED (ODBC 1,0)|*NumericAttributePtr*|SQL_TRUE si la colonne n’est pas signée (ou n’est pas numérique).<br /><br /> SQL_FALSE si la colonne est signée.|  
|SQL_DESC_UPDATABLE (ODBC 1,0)|*NumericAttributePtr*|La colonne est décrite par les valeurs des constantes définies :<br /><br /> SQL_ATTR_READONLY SQL_ATTR_WRITE SQL_ATTR_READWRITE_UNKNOWN<br /><br /> SQL_DESC_UPDATABLE décrit la mise à jour de la colonne dans le jeu de résultats, et non la colonne dans la table de base. La mise à jour de la colonne de base sur laquelle la colonne de l’ensemble de résultats est basée peut être différente de la valeur de ce champ. La possibilité de mettre à jour une colonne peut être basée sur le type de données, les privilèges de l’utilisateur et la définition du jeu de résultats lui-même. S’il est impossible de savoir si une colonne peut être mise à jour, SQL_ATTR_READWRITE_UNKNOWN doit être retourné.|  
  
 **SQLColAttribute** est une alternative extensible à **SQLDescribeCol**. **SQLDescribeCol** retourne un ensemble fixe d’informations de descripteur basées sur ANSI-89 SQL. **SQLColAttribute** autorise l’accès à l’ensemble plus complet d’informations de descripteur disponibles dans les extensions de fournisseur SQL-92 et SGBD ANSI.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à une colonne dans un jeu de résultats|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur une colonne dans un jeu de résultats|[Fonction SQLDescribeCol](../../../odbc/reference/syntax/sqldescribecol-function.md)|  
|Extraction d’un bloc de données ou défilement dans un jeu de résultats|[Fonction SQLFetchScroll](../../../odbc/reference/syntax/sqlfetchscroll-function.md)|  
|Extraction de plusieurs lignes de données|[SQLFetch, fonction](../../../odbc/reference/syntax/sqlfetch-function.md)|  
  
## <a name="example"></a>Exemple  
 L’exemple de code suivant ne libère pas les handles et les connexions. Consultez la [fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md), [Sample ODBC Program](../../../odbc/reference/sample-odbc-program.md)et [SQLFreeStmt Function](../../../odbc/reference/syntax/sqlfreestmt-function.md) pour obtenir des exemples de code afin de libérer des handles et des instructions.  
  
```cpp  
// SQLColAttibute.cpp  
// compile with: user32.lib odbc32.lib  
  
#define UNICODE  
  
#include <windows.h>  
#include <sqlext.h>  
#include <strsafe.h>  
  
struct DataBinding {  
   SQLSMALLINT TargetType;  
   SQLPOINTER TargetValuePtr;  
   SQLINTEGER BufferLength;  
   SQLLEN StrLen_or_Ind;  
};  
  
void printStatementResult(SQLHSTMT hstmt) {  
   int bufferSize = 1024, i;  
   SQLRETURN retCode;  
   SQLSMALLINT numColumn = 0, bufferLenUsed;
   
   retCode = SQLNumResultCols(hstmt, &numColumn);  
   
   SQLPOINTER* columnLabels = (SQLPOINTER *)malloc( numColumn * sizeof(SQLPOINTER*) );  
   struct DataBinding* columnData = (struct DataBinding*)malloc( numColumn * sizeof(struct DataBinding) );  
  
   printf( "Columns from that table:\n" );  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnLabels[i] = (SQLPOINTER)malloc( bufferSize*sizeof(char) );  
  
      retCode = SQLColAttribute(hstmt, (SQLUSMALLINT)i + 1, SQL_DESC_LABEL, columnLabels[i], (SQLSMALLINT)bufferSize, &bufferLenUsed, NULL);  
      wprintf( L"Column %d: %s\n", i, (wchar_t*)columnLabels[i] );  
   }  
  
   // allocate memory for the binding  
   for ( i = 0 ; i < numColumn ; i++ ) {  
      columnData[i].TargetType = SQL_C_CHAR;  
      columnData[i].BufferLength = (bufferSize+1);  
      columnData[i].TargetValuePtr = malloc( sizeof(unsigned char)*columnData[i].BufferLength );  
   }  
  
   // setup the binding   
   for ( i = 0 ; i < numColumn ; i++ ) {  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, columnData[i].TargetType,   
         columnData[i].TargetValuePtr, columnData[i].BufferLength, &(columnData[i].StrLen_or_Ind));  
   }  
  
   printf( "Data from that table:\n" );  
   // fetch the data and print out the data  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt) ) {  
      int j;  
      for ( j = 0 ; j < numColumn ; j++ )  
         wprintf( L"%s: %hs\n", columnLabels[j], columnData[j].TargetValuePtr );  
      printf( "\n" );  
   }  
   printf( "\n" );   
}  
  
int main() {  
   int bufferSize = 1024, i, count = 1, numCols = 5;  
   wchar_t firstTableName[1024], * dbName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize ), * userName = (wchar_t *)malloc( sizeof(wchar_t)*bufferSize );  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
   SQLWCHAR connStrbuffer[1024];  
   SQLSMALLINT connStrBufferLen, bufferLen;  
   SQLRETURN retCode;  
  
   SQLHENV henv = NULL;   // Environment     
   SQLHDBC hdbc = NULL;   // Connection handle  
   SQLHSTMT hstmt = NULL;   // Statement handle  
  
   struct DataBinding* catalogResult = (struct DataBinding*) malloc( numCols * sizeof(struct DataBinding) );  
   SQLWCHAR* selectAllQuery = (SQLWCHAR *)malloc( sizeof(SQLWCHAR) * bufferSize );  
  
   // connect to database  
   retCode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
   retCode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLCHAR *)(void*)SQL_OV_ODBC3, -1);  
   retCode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
   retCode = SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)10, 0);  
   retCode = SQLDriverConnect(hdbc, desktopHandle, L"Driver={SQL Server}", SQL_NTS, connStrbuffer, 1025, &connStrBufferLen, SQL_DRIVER_PROMPT);  
   retCode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
   // display the database information  
   retCode = SQLGetInfo(hdbc, SQL_DATABASE_NAME, dbName, (SQLSMALLINT)bufferSize, (SQLSMALLINT *)&bufferLen);  
   retCode = SQLGetInfo(hdbc, SQL_USER_NAME, userName, (SQLSMALLINT)bufferSize, &bufferLen);  
  
   for ( i = 0 ; i < numCols ; i++ ) {  
      catalogResult[i].TargetType = SQL_C_CHAR;  
      catalogResult[i].BufferLength = (bufferSize + 1);  
      catalogResult[i].TargetValuePtr = malloc( sizeof(unsigned char)*catalogResult[i].BufferLength );  
   }  
  
   // Set up the binding. This can be used even if the statement is closed by closeStatementHandle  
   for ( i = 0 ; i < numCols ; i++ )  
      retCode = SQLBindCol(hstmt, (SQLUSMALLINT)i + 1, catalogResult[i].TargetType, catalogResult[i].TargetValuePtr, catalogResult[i].BufferLength, &(catalogResult[i].StrLen_or_Ind));  
  
   retCode = SQLTables( hstmt, (SQLWCHAR*)SQL_ALL_CATALOGS, SQL_NTS, L"", SQL_NTS, L"", SQL_NTS, L"", SQL_NTS );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   retCode = SQLTables( hstmt, dbName, SQL_NTS, userName, SQL_NTS, L"%", SQL_NTS, L"TABLE", SQL_NTS );  
  
   for ( retCode = SQLFetch(hstmt) ; retCode == SQL_SUCCESS || retCode == SQL_SUCCESS_WITH_INFO ; retCode = SQLFetch(hstmt), ++count )  
      if ( count == 1 )  
         StringCchPrintfW( firstTableName, 1024, L"%hs", catalogResult[2].TargetValuePtr );  
   retCode = SQLFreeStmt(hstmt, SQL_CLOSE);  
  
   wprintf( L"Select all data from the first table (%s)\n", firstTableName );  
   StringCchPrintfW( selectAllQuery, bufferSize, L"SELECT * FROM %s", firstTableName );  
  
   retCode = SQLExecDirect(hstmt, selectAllQuery, SQL_NTS);  
   printStatementResult(hstmt);  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md)
