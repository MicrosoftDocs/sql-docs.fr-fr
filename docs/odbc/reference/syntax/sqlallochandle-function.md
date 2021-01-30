---
description: SQLAllocHandle, fonction
title: Fonction SQLAllocHandle | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLAllocHandle
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLAllocHandle
helpviewer_keywords:
- SQLAllocHandle function [ODBC]
ms.assetid: 6e7fe420-8cf4-4e72-8dad-212affaff317
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1d546ddfa5f7e18365fec4bdff08032a8bba749f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99184337"
---
# <a name="sqlallochandle-function"></a>SQLAllocHandle, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLAllocHandle** alloue un handle d’environnement, de connexion, d’instruction ou de descripteur.  
  
> [!NOTE]  
>  Cette fonction est une fonction générique permettant d’allouer des handles qui remplacent les fonctions ODBC 2,0 **SQLAllocConnect**, **SQLAllocEnv** et **SQLAllocStmt,**. Pour autoriser les applications qui appellent **SQLAllocHandle** à fonctionner avec ODBC 2. *x* , un appel à **SQLAllocHandle** est mappé dans le gestionnaire de pilotes à **SQLAllocConnect**, **SQLAllocEnv** ou **SQLAllocStmt,**, selon le cas. Pour plus d’informations, consultez la section « commentaires ». Pour plus d’informations sur la façon dont le gestionnaire de pilotes mappe cette fonction quand ODBC 3. l’application *x* fonctionne avec ODBC 2. *x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLAllocHandle(  
      SQLSMALLINT   HandleType,  
      SQLHANDLE     InputHandle,  
      SQLHANDLE *   OutputHandlePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Type de descripteur à allouer par **SQLAllocHandle**. Il doit s’agir de l’une des valeurs suivantes :   
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN descripteur est utilisé uniquement par le gestionnaire de pilotes et le pilote. Les applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur la SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement de la sensibilisation aux Connection-Pool dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *InputHandle*  
 Entrée Handle d’entrée dans le contexte duquel le nouveau handle doit être alloué. Si *comme HandleType* est SQL_HANDLE_ENV, il s’agit d’SQL_NULL_HANDLE. Si *comme HandleType* est SQL_HANDLE_DBC, il doit s’agir d’un handle d’environnement et, s’il est SQL_HANDLE_STMT ou SQL_HANDLE_DESC, il doit s’agir d’un handle de connexion.  
  
 *OutputHandlePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le handle de la structure de données nouvellement allouée.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_INVALID_HANDLE ou SQL_ERROR.  
  
 Lors de l’allocation d’un handle autre qu’un handle d’environnement, si **SQLAllocHandle** retourne SQL_ERROR, il définit *OutputHandlePtr* sur SQL_NULL_HDBC, SQL_NULL_HSTMT ou SQL_NULL_HDESC, en fonction de la valeur de *comme HandleType*, sauf si l’argument de sortie est un pointeur null. L’application peut ensuite obtenir des informations supplémentaires à partir de la structure de données de diagnostic associée au descripteur dans l’argument *InputHandle* .  
  
## <a name="environment-handle-allocation-errors"></a>Erreurs d’allocation du handle d’environnement  
 L’allocation d’environnement se produit à la fois dans le gestionnaire de pilotes et dans chaque pilote. L’erreur retournée par **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV dépend du niveau dans lequel l’erreur s’est produite.  
  
 Si le gestionnaire de pilotes ne peut pas allouer de mémoire pour *\* OutputHandlePtr* quand **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV est appelé, ou si l’application fournit un pointeur null pour *OutputHandlePtr*, **SQLAllocHandle** retourne SQL_ERROR. Le gestionnaire de pilotes définit **OutputHandlePtr* sur SQL_NULL_HENV (sauf si l’application a fourni un pointeur null, qui retourne SQL_ERROR). Il n’existe aucun descripteur avec lequel associer des informations de diagnostic supplémentaires.  
  
 Le gestionnaire de pilotes n’appelle pas la fonction d’allocation de handle d’environnement au niveau du pilote tant que l’application n’a pas appelé **SQLConnect**, **SQLBrowseConnect** ou **SQLDriverConnect**. Si une erreur se produit dans la fonction **SQLAllocHandle** au niveau du pilote, la fonction **SQLConnect**, **SQLBrowseConnect** ou **SQLDriverConnect** au niveau du gestionnaire de pilotes retourne SQL_ERROR. La structure de données de diagnostic contient SQLSTATE IM004 (échec **SQLAllocHandle** du pilote). L’erreur est retournée sur un handle de connexion.  
  
 Pour plus d’informations sur le déroulement des appels de fonction entre le gestionnaire de pilotes et un pilote, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLAllocHandle** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec le *comme HandleType* et le *descripteur* appropriés définis sur la valeur de *InputHandle*. SQL_SUCCESS_WITH_INFO (mais pas SQL_ERROR) peut être retourné pour l’argument *OutputHandle* . Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLAllocHandle** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08003|Connexion non ouverte|(DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT ou SQL_HANDLE_DESC, mais la connexion spécifiée par l’argument *InputHandle* n’était pas ouverte. Le processus de connexion doit être correctement effectué (et la connexion doit être ouverte) pour que le pilote alloue une instruction ou un handle de descripteur.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans le tampon **MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|(DM) le gestionnaire de pilotes n’a pas pu allouer de mémoire pour le descripteur spécifié.<br /><br /> Le pilote n’a pas pu allouer de mémoire pour le descripteur spécifié.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) l’argument *OutputHandlePtr* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) l’argument *comme HandleType* a été SQL_HANDLE_DBC et **SQLSetEnvAttr** n’a pas été appelé pour définir l’attribut d’environnement SQL_ODBC_VERSION.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le **InputHandle** et s’exécutait quand la fonction **SQLAllocHandle** était appelée avec **comme handletype** défini sur SQL_HANDLE_STMT ou SQL_HANDLE_DESC.|  
|HY013|Erreur de gestion de la mémoire|L’argument *comme HandleType* a été SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC ; et l’appel de fonction n’a pas pu être traité parce que les objets de mémoire sous-jacents n’ont pas pu être accédés, probablement en raison de conditions de mémoire insuffisante.|  
|HY014|Limite du nombre de handles dépassée|La limite définie par le pilote pour le nombre de descripteurs pouvant être alloués pour le type de descripteur indiqué par l’argument *comme HandleType* a été atteinte.|  
|HY092|Identificateur d’attribut/option non valide|(DM) l’argument *comme HandleType* n’était pas : SQL_HANDLE_ENV, SQL_HANDLE_DBC, SQL_HANDLE_STMT ou SQL_HANDLE_DESC.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|L’argument *comme HandleType* a été SQL_HANDLE_DESC et le pilote était un ODBC 2. pilote *x* .|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) l’argument *comme HandleType* a été SQL_HANDLE_STMT, et le pilote n’était pas un pilote ODBC valide.<br /><br /> (DM) l’argument *comme HandleType* a été SQL_HANDLE_DESC, et le pilote ne prend pas en charge l’allocation d’un handle de descripteur.|  
  
## <a name="comments"></a>Commentaires  
 **SQLAllocHandle** est utilisé pour allouer des handles pour les environnements, les connexions, les instructions et les descripteurs, comme décrit dans les sections suivantes. Pour obtenir des informations générales sur les descripteurs, consultez [Handles](../../../odbc/reference/develop-app/handles.md).  
  
 Plusieurs identificateurs d’environnement, de connexion ou d’instruction peuvent être alloués par une application à la fois si plusieurs allocations sont prises en charge par le pilote. Dans ODBC, aucune limite n’est définie sur le nombre de handles d’environnement, de connexion, d’instruction ou de descripteur qui peuvent être alloués à un moment donné. Les pilotes peuvent imposer une limite au nombre d’un certain type de descripteur pouvant être alloué à la fois ; Pour plus d’informations, consultez la documentation du pilote.  
  
 Si l’application appelle **SQLAllocHandle** avec *\* OutputHandlePtr* défini sur un environnement, une connexion, une instruction ou un handle de descripteur qui existe déjà, le pilote remplace les informations *associées au* descripteur, sauf si l’application utilise le regroupement de connexions (voir « allocation d’un attribut d’environnement pour le regroupement de connexions » plus loin dans cette section). Le gestionnaire de pilotes ne vérifie pas si le *handle* entré dans *\* OutputHandlePtr* est déjà utilisé, ni ne vérifie le contenu précédent d’un descripteur avant de le remplacer.  
  
> [!NOTE]  
>  Il s’agit d’une programmation d’application ODBC incorrecte pour appeler **SQLAllocHandle** deux fois avec la même variable d’application définie pour *\* OutputHandlePtr* sans appeler **SQLFreeHandle** pour libérer le handle avant de le réallouer. Le remplacement des handles ODBC de telle manière peut entraîner des erreurs ou des comportements incohérents dans la partie des pilotes ODBC.  
  
 Sur les systèmes d’exploitation qui prennent en charge plusieurs threads, les applications peuvent utiliser le même environnement, la même connexion, la même instruction ou le même handle de descripteur sur des threads différents. Les pilotes doivent donc prendre en charge l’accès multithread sécurisé à ces informations ; une façon d’y parvenir, par exemple, consiste à utiliser une section critique ou un sémaphore. Pour plus d’informations sur les threads, consultez [Multithreading](../../../odbc/reference/develop-app/multithreading.md).  
  
 **SQLAllocHandle** ne définit pas l’attribut d’environnement SQL_ATTR_ODBC_VERSION lorsqu’il est appelé pour allouer un handle d’environnement. l’attribut Environment doit être défini par l’application, ou SQLSTATE HY010 (erreur de séquence de fonction) est retourné lorsque **SQLAllocHandle** est appelé pour allouer un handle de connexion.  
  
 Pour les applications conformes aux normes, **SQLAllocHandle** est mappé à **SQLAllocHandleStd** au moment de la compilation. La différence entre ces deux fonctions est que **SQLAllocHandleStd** définit l’attribut d’environnement SQL_ATTR_ODBC_VERSION sur SQL_OV_ODBC3 lorsqu’il est appelé avec l’argument *comme handletype* défini sur SQL_HANDLE_ENV. Cela est dû au fait que les applications conformes aux normes sont toujours ODBC 3. *x* applications. En outre, les normes ne nécessitent pas l’inscription de la version de l’application. Il s’agit de la seule différence entre ces deux fonctions ; dans le cas contraire, ils sont identiques. **SQLAllocHandleStd** est mappé à **SQLAllocHandle** à l’intérieur du gestionnaire de pilotes. Par conséquent, il n’est pas nécessaire que les pilotes tiers implémentent **SQLAllocHandleStd**.  
  
 Les applications ODBC 3,8 doivent utiliser :  
  
-   **SQLAllocHandle et non SQLAllocHandleStd** pour allouer un handle d’environnement.  
  
-   **SQLSetEnvAttr** pour définir l’attribut d’environnement SQL_ATTR_ODBC_VERSION sur SQL_OV_ODBC3_80.  
  
## <a name="allocating-an-environment-handle"></a>Allocation d'un handle d'environnement  
 Un handle d’environnement permet d’accéder à des informations globales, telles que des handles de connexion valides et des handles de connexion actifs. Pour obtenir des informations générales sur les handles d’environnement, consultez [Handles d’environnement](../../../odbc/reference/develop-app/environment-handles.md).  
  
 Pour demander un handle d’environnement, une application appelle **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV et un *InputHandle* de SQL_NULL_HANDLE. Le pilote alloue de la mémoire pour les informations d’environnement et transmet la valeur du descripteur associé dans l’argument *\* OutputHandlePtr* . L’application transmet la valeur *\* OutputHandle* dans tous les appels suivants qui requièrent un argument de handle d’environnement. Pour plus d’informations, consultez [allocation du handle d’environnement](../../../odbc/reference/develop-app/allocating-the-environment-handle.md).  
  
 Dans le cadre d’un handle d’environnement du gestionnaire de pilotes, s’il existe déjà un handle d’environnement de pilote, **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV n’est pas appelé dans ce pilote lorsqu’une connexion est établie, uniquement **sqlallochandle** avec un *comme HandleType* de SQL_HANDLE_DBC. Si le descripteur d’environnement d’un pilote n’existe pas sous le descripteur d’environnement du gestionnaire de pilotes, SQLAllocHandle avec un comme HandleType de SQL_HANDLE_ENV et SQLAllocHandle avec un comme HandleType de SQL_HANDLE_DBC sont appelés dans le pilote lorsque le premier descripteur de connexion de l’environnement est connecté au pilote.  
  
 Quand le gestionnaire de pilotes traite la fonction **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV, il vérifie le mot clé **trace** dans la section [ODBC] des informations système. S’il est défini sur 1, le gestionnaire de pilotes active le suivi pour l’application actuelle. Si l’indicateur de trace est défini, le suivi commence lorsque le premier handle d’environnement est alloué et se termine lorsque le dernier handle d’environnement est libéré. Pour plus d’informations, consultez [Configuration des sources de données](../../../odbc/reference/install/configuring-data-sources.md).  
  
 Après avoir alloué un handle d’environnement, une application doit appeler **SQLSetEnvAttr** sur le handle d’environnement pour définir l’attribut d’environnement SQL_ATTR_ODBC_VERSION. Si cet attribut n’est pas défini avant l’appel de **SQLAllocHandle** pour allouer un handle de connexion sur l’environnement, l’appel à allouer la connexion retourne SQLState HY010 (erreur de séquence de fonction). Pour plus d’informations, consultez [déclaration de la version ODBC de l’application](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md).  
  
## <a name="allocating-shared-environments-for-connection-pooling"></a>Allocation d’environnements partagés pour le regroupement de connexions  
 Les environnements peuvent être partagés entre plusieurs composants sur un seul processus. Un environnement partagé peut être utilisé par plusieurs composants en même temps. Quand un composant utilise un environnement partagé, il peut utiliser des connexions regroupées, ce qui lui permet d’allouer et d’utiliser une connexion existante sans recréer cette connexion.  
  
 Avant d’allouer un environnement partagé qui peut être utilisé pour le regroupement de connexions, une application doit appeler **SQLSetEnvAttr** pour définir l’attribut d’environnement SQL_ATTR_CONNECTION_POOLING sur SQL_CP_ONE_PER_DRIVER ou SQL_CP_ONE_PER_HENV. Dans ce cas, **SQLSetEnvAttr** est appelé avec *EnvironmentHandle* défini sur null, ce qui fait de l’attribut un attribut de niveau processus.  
  
 Une fois que le regroupement de connexions a été activé, une application appelle **SQLAllocHandle** avec l’argument *comme handletype* défini sur SQL_HANDLE_ENV. L’environnement alloué par cet appel sera un environnement partagé implicite, car le regroupement de connexions a été activé.  
  
 Lorsqu’un environnement partagé est alloué, l’environnement qui sera utilisé n’est pas déterminé jusqu’à ce que **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC soit appelé. À ce stade, le gestionnaire de pilotes tente de trouver un environnement existant qui correspond aux attributs d’environnement demandés par l’application. Si aucun environnement de ce type n’existe, il est créé en tant qu’environnement partagé. Le gestionnaire de pilotes gère un décompte de références pour chaque environnement partagé ; le nombre a la valeur 1 lors de la création de l’environnement. Si un environnement correspondant est trouvé, le handle de cet environnement est retourné à l’application et le décompte de références est incrémenté. Un handle d’environnement alloué de cette manière peut être utilisé dans toute fonction ODBC qui accepte un handle d’environnement comme argument d’entrée.  
  
## <a name="allocating-a-connection-handle"></a>Allocation d'un handle de connexion  
 Un descripteur de connexion permet d’accéder à des informations telles que les handles d’instruction et de descripteur valides sur la connexion et si une transaction est actuellement ouverte. Pour obtenir des informations générales sur les handles de connexion, consultez [Handles de connexion](../../../odbc/reference/develop-app/connection-handles.md).  
  
 Pour demander un handle de connexion, une application appelle **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC. L’argument *InputHandle* est défini sur le handle d’environnement qui a été retourné par l’appel à **SQLAllocHandle** qui a alloué ce handle. Le pilote alloue de la mémoire pour les informations de connexion et transmet à nouveau la valeur du handle associé dans *\* OutputHandlePtr*. L’application transmet la valeur *\* OutputHandlePtr* dans tous les appels suivants qui requièrent un handle de connexion. Pour plus d’informations, consultez [allocation d’un handle de connexion](../../../odbc/reference/develop-app/allocating-a-connection-handle-odbc.md).  
  
 Le gestionnaire de pilotes traite la fonction **SQLAllocHandle** et appelle la fonction **SQLAllocHandle** du pilote lorsque l’application appelle **SQLConnect**, **SQLBrowseConnect** ou **SQLDriverConnect**. (Pour plus d’informations, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).)  
  
 Si l’attribut d’environnement SQL_ATTR_ODBC_VERSION n’est pas défini avant l’appel de **SQLAllocHandle** pour allouer un handle de connexion sur l’environnement, l’appel à Allocate The Connection retourne SQLState HY010 (erreur de séquence de fonction).  
  
 Quand une application appelle **SQLAllocHandle** avec l’argument *InputHandle* ayant la valeur SQL_HANDLE_DBC et également défini sur un handle d’environnement partagé, le gestionnaire de pilotes tente de trouver un environnement partagé existant qui correspond aux attributs d’environnement définis par l’application. Si aucun de ces environnements n’existe, un tel environnement est créé, avec un nombre de références (géré par le gestionnaire de pilotes) de 1. Si un environnement partagé correspondant est trouvé, ce handle est retourné à l’application et son décompte de références est incrémenté.  
  
 La connexion réelle qui sera utilisée n’est pas déterminée par le gestionnaire de pilotes tant que **SQLConnect** ou **SQLDriverConnect** n’est pas appelé. Le gestionnaire de pilotes utilise les options de connexion dans l’appel à **SQLConnect** (ou les mots clés de connexion dans l’appel à **SQLDriverConnect**) et les attributs de connexion définis après l’allocation de connexion pour déterminer la connexion dans le pool à utiliser. Pour plus d’informations, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="allocating-a-statement-handle"></a>Allocation d'un descripteur d'instruction  
 Un descripteur d’instruction fournit l’accès aux informations de l’instruction, telles que les messages d’erreur, le nom du curseur et les informations d’État pour le traitement des instructions SQL. Pour obtenir des informations générales sur les descripteurs d’instruction, consultez [Handles d’instruction](../../../odbc/reference/develop-app/statement-handles.md).  
  
 Pour demander un descripteur d’instruction, une application se connecte à une source de données, puis appelle **SQLAllocHandle** avant d’envoyer des instructions SQL. Dans cet appel, *comme HandleType* doit être défini sur SQL_HANDLE_STMT et *InputHandle* doit être défini sur le handle de connexion qui a été retourné par l’appel à **SQLAllocHandle** qui a alloué ce handle. Le pilote alloue de la mémoire pour les informations de l’instruction, associe le descripteur d’instruction à la connexion spécifiée et repasse la valeur du handle associé dans *\* OutputHandlePtr*. L’application transmet la valeur *\* OutputHandlePtr* dans tous les appels suivants qui requièrent un descripteur d’instruction. Pour plus d’informations, consultez [allocation d’un descripteur d’instruction](../../../odbc/reference/develop-app/allocating-a-statement-handle-odbc.md).  
  
 Lorsque le descripteur d’instruction est alloué, le pilote alloue automatiquement un ensemble de quatre descripteurs et assigne les handles de ces descripteurs aux attributs d’instruction SQL_ATTR_APP_ROW_DESC, SQL_ATTR_APP_PARAM_DESC, SQL_ATTR_IMP_ROW_DESC et SQL_ATTR_IMP_PARAM_DESC. Celles-ci sont appelées descripteurs alloués de *manière implicite* . Pour allouer explicitement un descripteur d’application, consultez la section « allocation d’un handle de descripteur ».  
  
## <a name="allocating-a-descriptor-handle"></a>Allocation d’un handle de descripteur  
 Quand une application appelle **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DESC, le pilote alloue un descripteur d’application. Celles-ci sont appelées descripteurs *explicitement* alloués. L’application indique à un pilote d’utiliser un descripteur d’application explicitement alloué au lieu d’un descripteur d’instruction alloué automatiquement pour un handle d’instruction donné en appelant la fonction **SQLSetStmtAttr** avec l’attribut SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC. Un descripteur d’implémentation ne peut pas être alloué explicitement et un descripteur d’implémentation ne peut pas être spécifié dans un appel de fonction **SQLSetStmtAttr** .  
  
 Les descripteurs alloués explicitement sont associés à un handle de connexion plutôt qu’à un handle d’instruction (comme les descripteurs alloués automatiquement sont). Les descripteurs restent alloués uniquement lorsqu’une application est réellement connectée à la base de données. Étant donné que les descripteurs alloués explicitement sont associés à un handle de connexion, une application peut associer un descripteur explicitement alloué à plusieurs instructions dans une connexion. Un descripteur d’application implicitement alloué, en revanche, ne peut pas être associé à plusieurs descripteurs d’instruction. (Elle ne peut pas être associée à un handle d’instruction autre que celui pour lequel elle a été allouée.) Les handles de descripteurs alloués explicitement peuvent être libérés explicitement par l’application ou en appelant **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DESC ou implicitement lorsque la connexion est fermée.  
  
 Lorsque le descripteur alloué explicitement est libéré, le descripteur alloué implicitement est à nouveau associé à l’instruction. (L’attribut SQL_ATTR_APP_ROW_DESC ou SQL_ATTR_APP_PARAM_DESC de cette instruction est à nouveau défini sur le handle de descripteur alloué implicitement.) Cela est vrai pour toutes les instructions associées au descripteur explicitement alloué sur la connexion.  
  
 Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md), [fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md), [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md)et [fonction SQLSetCursorName](../../../odbc/reference/syntax/sqlsetcursorname-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Libération d’un handle d’environnement, de connexion, d’instruction ou de descripteur|[Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Préparation d’une instruction pour l’exécution|[Fonction SQLPrepare](../../../odbc/reference/syntax/sqlprepare-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un champ de descripteur|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition d’un attribut d’environnement|[Fonction SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
