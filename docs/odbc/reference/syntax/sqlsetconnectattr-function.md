---
description: Fonction SQLSetConnectAttr
title: Fonction SQLSetConnectAttr | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetConnectAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLSetConnectAttr
helpviewer_keywords:
- SQLSetConnectAttr function [ODBC]
ms.assetid: 97fc7445-5a66-4eb9-8e77-10990b5fd685
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06935fe7eca6f19a8c6fcc052cd3a9503144a296
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192449"
---
# <a name="sqlsetconnectattr-function"></a>Fonction SQLSetConnectAttr
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLSetConnectAttr** définit les attributs qui régissent les aspects des connexions.  
  
> [!NOTE]
>  Pour plus d’informations sur le mappage de cette fonction par le gestionnaire de pilotes lorsqu’une application ODBC 3 *. x* utilise un pilote ODBC 2 *. x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSetConnectAttr(  
     SQLHDBC       ConnectionHandle,  
     SQLINTEGER    Attribute,  
     SQLPOINTER    ValuePtr,  
     SQLINTEGER    StringLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *Attribut*  
 Entrée Attribut à définir, indiqué dans « Comments ».  
  
 *ValuePtr*  
 Entrée Pointeur vers la valeur à associer à l' *attribut*. Selon la valeur de l' *attribut*, *ValuePtr* est une valeur entière non signée ou pointe vers une chaîne de caractères terminée par le caractère null. Notez que le type intégral de l’argument d' *attribut* ne peut pas être de longueur fixe. pour plus d’informations, consultez la section commentaires.  
  
 *StringLength*  
 Entrée Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de **ValuePtr*. Pour les données de type chaîne de caractères, cet argument doit contenir le nombre d’octets dans la chaîne.  
  
 Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* est un entier, *StringLength* est ignoré.  
  
 Si l' *attribut* est un attribut défini par le pilote, l’application indique la nature de l’attribut au gestionnaire de pilotes en définissant l’argument *StringLength* . *StringLength* peut avoir les valeurs suivantes :  
  
-   Si *ValuePtr* est un pointeur vers une chaîne de caractères, *StringLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *StringLength*. Cela place une valeur négative dans *StringLength*.  
  
-   Si *ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *StringLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *ValuePtr* contient une valeur de longueur fixe, *StringLength* est soit SQL_IS_INTEGER, soit SQL_IS_UINTEGER, selon le cas.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSetConnectAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLSetConnectAttr** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
 Le pilote peut retourner SQL_SUCCESS_WITH_INFO pour fournir des informations sur le résultat de la définition d’une option.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée dans *ValuePtr* et substituait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08002|Nom de connexion en cours d’utilisation|L’argument d' *attribut* était SQL_ATTR_ODBC_CURSORS, et le pilote était déjà connecté à la source de données.|  
|08003|Connexion non ouverte|(DM) une valeur d' *attribut* a été spécifiée qui nécessitait une connexion ouverte, mais le *ConnectionHandle* n’était pas dans un état connecté.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|24 000|État de curseur non valide|L’argument d' *attribut* a été SQL_ATTR_CURRENT_CATALOG et un jeu de résultats était en attente.|  
|25000|Opération non conforme dans une transaction locale|Une connexion était dans une transaction locale lors de la tentative d’inscription dans une connexion de transaction distribuée (DTC) en définissant l’attribut de connexion SQL_ATTR_ENLIST_IN_DTC.<br /><br /> Une connexion est déjà inscrite dans un DTC.<br /><br /> Une connexion a été inscrite dans une connexion de transaction distribuée et une transaction locale a été démarrée en définissant SQL_ATTR_AUTOCOMMIT sur SQL_AUTOCOMMIT_OFF.|  
|3D000|Nom de catalogue non valide|L’argument d' *attribut* a été SQL_CURRENT_CATALOG, et le nom de catalogue spécifié n’était pas valide.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *ConnectionHandle*. La fonction **SQLSetConnectAttr** a été appelée, et avant la fin de l’exécution, la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) a été appelée sur le *ConnectionHandle*, puis la fonction **SQLSetConnectAttr** a été appelée à nouveau sur le *ConnectionHandle*.<br /><br /> Ou bien, la fonction  **SQLSetConnectAttr** a été appelée et avant la fin de l’exécution, **SQLCancelHandle** a été appelé sur le *ConnectionHandle* à partir d’un thread différent dans une application multithread.|  
|HY009|Utilisation non valide d’un pointeur null|L’argument d' *attribut* a identifié un attribut de connexion qui nécessitait une valeur de chaîne et l’argument *ValuePtr* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour un *StatementHandle* associé à *ConnectionHandle* et s’exécutait toujours lorsque **SQLSetConnectAttr** était appelé.<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des handles d’instruction associés à *ConnectionHandle* et retournés SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour un *StatementHandle* associé au *ConnectionHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) **SQLBrowseConnect** a été appelé pour *ConnectionHandle* et a retourné SQL_NEED_DATA. Cette fonction a été appelée avant que **SQLBrowseConnect** retourne SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.|  
|HY011|L’attribut ne peut pas être défini maintenant|L’argument d' *attribut* était SQL_ATTR_TXN_ISOLATION et une transaction était ouverte.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY024|Valeur d’attribut non valide|À partir de la valeur d' *attribut* spécifiée, une valeur non valide a été spécifiée dans *ValuePtr*. (Le gestionnaire de pilotes retourne ce SQLSTATE uniquement pour les attributs de connexion et d’instruction qui acceptent un ensemble discret de valeurs, telles que SQL_ATTR_ACCESS_MODE ou SQL_ATTR_ASYNC_ENABLE. Pour tous les autres attributs de connexion et d’instruction, le pilote doit vérifier la valeur spécifiée dans *ValuePtr*.)<br /><br /> L’argument d' *attribut* était SQL_ATTR_TRACEFILE ou SQL_ATTR_TRANSLATE_LIB, et *ValuePtr* était une chaîne vide.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|*(DM) \* ValuePtr* est une chaîne de caractères, et l’argument *StringLength* a une valeur inférieure à 0 mais n’a pas été SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|(DM) la valeur spécifiée pour l' *attribut* argument n’était pas valide pour la version de ODBC prise en charge par le pilote.<br /><br /> (DM) la valeur spécifiée pour l' *attribut* d’argument était un attribut en lecture seule.|  
|HY114|Le pilote ne prend pas en charge l’exécution de fonctions asynchrones au niveau de la connexion|(DM) une application a tenté d’activer l’exécution de fonctions asynchrones avec SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE pour un pilote qui ne prend pas en charge les opérations de connexion asynchrone.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HY121|La bibliothèque de curseurs et le regroupement de Driver-Aware ne peuvent pas être activés en même temps|Pour plus d’informations, consultez [regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l' *attribut* argument était un attribut de connexion ou d’instruction ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *ConnectionHandle* ne prend pas en charge la fonction.|  
|IM009|Impossible de charger la DLL de traduction|Le pilote n’a pas pu charger la DLL de traduction spécifiée pour la connexion. Cette erreur peut être retournée uniquement lorsque l' *attribut* est SQL_ATTR_TRANSLATE_LIB.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
|S1118|Le pilote ne prend pas en charge la notification asynchrone|SQL_ATTR_ASYNC_DBC_EVENT a été défini (une fois la connexion établie), mais la notification asynchrone n’est pas prise en charge par le pilote.|  
  
 Lorsque l' *attribut* est un attribut d’instruction, **SQLSetConnectAttr** peut retourner toute SQLSTATE renvoyée par **SQLSetStmtAttr**.  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir des informations générales sur les attributs de connexion, consultez [attributs de connexion](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Les attributs actuellement définis et la version de ODBC dans laquelle ils ont été introduits sont indiqués dans le tableau plus loin dans cette section. Il est supposé que davantage d’attributs seront définis pour tirer parti de différentes sources de données. Une plage d’attributs est réservée par ODBC ; les développeurs de pilotes doivent réserver des valeurs pour leur propre utilisation propre au pilote à partir d’Open Group.  
  
> [!NOTE]
>  La possibilité de définir des attributs d’instruction au niveau de la connexion en appelant **SQLSetConnectAttr** est dépréciée dans ODBC 3 *. x*. Les applications ODBC 3 *. x* ne doivent jamais définir d’attributs d’instruction au niveau de la connexion. Les attributs d’instruction ODBC 3 *. x* ne peuvent pas être définis au niveau de la connexion, à l’exception des attributs SQL_ATTR_METADATA_ID et SQL_ATTR_ASYNC_ENABLE, qui sont des attributs de connexion et des attributs d’instruction et qui peuvent être définis au niveau de la connexion ou de l’instruction.  
> 
>  Les pilotes ODBC 3 *. x* doivent uniquement prendre en charge cette fonctionnalité s’ils doivent fonctionner avec des applications ODBC 2 *. x* qui définissent les options d’instruction ODBC 2 *. x* au niveau de la connexion. Pour plus d’informations, consultez le [mappage SQLSetConnectOption](../../../odbc/reference/appendixes/sqlsetconnectoption-mapping.md) dans annexe G : instructions relatives aux pilotes pour la compatibilité descendante.  
  
 Une application peut appeler **SQLSetConnectAttr** à tout moment entre l’allocation et la libération de la connexion. Tous les attributs de connexion et d’instruction définis avec succès par l’application pour la connexion persistent jusqu’à ce que **SQLFreeHandle** soit appelé sur la connexion. Par exemple, si une application appelle **SQLSetConnectAttr** avant de se connecter à une source de données, l’attribut persiste même si **SQLSetConnectAttr** échoue dans le pilote lorsque l’application se connecte à la source de données ; Si une application définit un attribut spécifique au pilote, l’attribut persiste même si l’application se connecte à un autre pilote sur la connexion.  
  
 Certains attributs de connexion peuvent être définis uniquement avant qu’une connexion ait été établie. d’autres peuvent être définis uniquement après qu’une connexion a été établie. Le tableau suivant indique les attributs de connexion qui doivent être définis avant ou après qu’une connexion a été établie. *Indique que* l’attribut peut être défini avant ou après la connexion.  
  
|Attribut|Définir la connexion avant ou après ?|  
|---------------|-------------------------------------|  
|SQL_ATTR_ACCESS_MODE|Soit [1]|  
|SQL_ATTR_ASYNC_DBC_EVENT|Vous pouvez soit utiliser|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE|Soit [4]|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK|Vous pouvez soit utiliser|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT|Vous pouvez soit utiliser|  
|SQL_ATTR_ASYNC_ENABLE|Soit [2]|  
|SQL_ATTR_AUTO_IPD|Vous pouvez soit utiliser|  
|SQL_ATTR_AUTOCOMMIT|Soit [5]|  
|SQL_ATTR_CONNECTION_DEAD|After|  
|SQL_ATTR_CONNECTION_TIMEOUT|Vous pouvez soit utiliser|  
|SQL_ATTR_CURRENT_CATALOG|Soit [1]|  
|SQL_ATTR_DBC_INFO_TOKEN|After|  
|SQL_ATTR_ENLIST_IN_DTC|After|  
|SQL_ATTR_LOGIN_TIMEOUT|Avant|  
|SQL_ATTR_METADATA_ID|Vous pouvez soit utiliser|  
|SQL_ATTR_ODBC_CURSORS|Avant|  
|SQL_ATTR_PACKET_SIZE|Avant|  
|SQL_ATTR_QUIET_MODE|Vous pouvez soit utiliser|  
|SQL_ATTR_TRACE|Vous pouvez soit utiliser|  
|SQL_ATTR_TRACEFILE|Vous pouvez soit utiliser|  
|SQL_ATTR_TRANSLATE_LIB|After|  
|SQL_ATTR_TRANSLATE_OPTION|After|  
|SQL_ATTR_TXN_ISOLATION|Soit [3]|  
  
 [1] SQL_ATTR_ACCESS_MODE et SQL_ATTR_CURRENT_CATALOG peuvent être définis avant ou après la connexion, en fonction du pilote. Toutefois, les applications interopérables les définissent avant de se connecter, car certains pilotes ne prennent pas en charge leur modification après la connexion.  
  
 [2] SQL_ATTR_ASYNC_ENABLE doit être définie avant qu’une instruction soit active.  
  
 [3] SQL_ATTR_TXN_ISOLATION ne peut être définie que s’il n’y a pas de transactions ouvertes sur la connexion. Certains attributs de connexion prennent en charge la substitution d’une valeur similaire si la source de données ne prend pas en charge la valeur spécifiée dans \* *ValuePtr*. Dans ce cas, le pilote retourne SQL_SUCCESS_WITH_INFO et SQLSTATE 01S02 ne (valeur d’option modifiée). Par exemple, si l' *attribut* est SQL_ATTR_PACKET_SIZE et \* *ValuePtr* dépasse la taille de paquet maximale, le pilote remplace la taille maximale. Pour déterminer la valeur substituée, une application appelle **SQLGetConnectAttr**.  
  
 [4] si SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE est défini avant l’ouverture d’une connexion, le gestionnaire de pilotes définit l’attribut du pilote lorsque le pilote est chargé lors d’un appel à **SQLBrowseConnect**, **SQLConnect** ou **SQLDriverConnect**. Avant un appel à **SQLBrowseConnect**, **SQLConnect** ou **SQLDriverConnect**, le gestionnaire de pilotes ne sait pas quel pilote se connecter et ne sait pas si le pilote prend en charge les opérations de connexion asynchrones. Par conséquent, le gestionnaire de pilotes retourne toujours SQL_SUCCESS. Toutefois, si le pilote ne prend pas en charge les opérations de connexion asynchrones, l’appel de **SQLBrowseConnect**, **SQLConnect** ou **SQLDriverConnect** échoue.  
  
 [5] lorsque SQL_ATTR_AUTOCOMMIT a la valeur FALSe, les applications doivent appeler SQLEndTran (SQL_ROLLBACK) si une API retourne SQL_ERROR pour garantir la cohérence transactionnelle.  
  
 Le format des informations définies dans le \* tampon *ValuePtr* dépend de l' *attribut* spécifié. **SQLSetConnectAttr** accepte les informations d’attribut dans l’un des deux formats suivants : une chaîne de caractères se terminant par un caractère null ou une valeur entière. Le format de chaque est indiqué dans la description de l’attribut. Les chaînes de caractères pointées par l’argument *ValuePtr* de **SQLSetConnectAttr** ont une longueur de *StringLength* octets.  
  
 L’argument *StringLength* est ignoré si la longueur est définie par l’attribut, comme c’est le cas pour tous les attributs introduits dans ODBC 2 *. x* ou une version antérieure.  
  
|*Attribut*|Contenu *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_ACCESS_MODE (ODBC 1,0)|Valeur SQLUINTEGER. SQL_MODE_READ_ONLY est utilisé par le pilote ou la source de données comme indicateur que la connexion n’est pas requise pour prendre en charge les instructions SQL qui provoquent des mises à jour. Ce mode peut être utilisé pour optimiser les stratégies de verrouillage, la gestion des transactions ou d’autres zones appropriées au pilote ou à la source de données. Le pilote n’est pas requis pour empêcher l’envoi de ces instructions à la source de données. Le comportement du pilote et de la source de données lorsqu’il est demandé de traiter les instructions SQL qui ne sont pas en lecture seule pendant une connexion en lecture seule est défini par l’implémentation. SQL_MODE_READ_WRITE est la valeur par défaut.|  
|SQL_ATTR_ASYNC_DBC_EVENT (ODBC 3,8)|Valeur SQLPOINTER qui est un handle d’événement.<br /><br /> La notification de la fin des fonctions asynchrones est activée en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_ASYNC_STMT_EVENT et en spécifiant le descripteur d’événement. **Remarque :**  La méthode de notification n’est pas prise en charge avec la bibliothèque de curseurs. Une application recevra un message d’erreur si elle tente d’activer la bibliothèque de curseurs via SQLSetConnectAttr, lorsque la méthode de notification est activée.|  
|SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE (ODBC 3,8)|Valeur SQLUINTEGER qui active ou désactive l’exécution asynchrone des fonctions sélectionnées sur le handle de connexion. Pour plus d’informations, consultez [exécution asynchrone (méthode d’interrogation)](../../../odbc/reference/develop-app/asynchronous-execution-polling-method.md).<br /><br /> SQL_ASYNC_DBC_ENABLE_ON = activer l’opération asynchrone pour les fonctions liées à la connexion spécifiées.<br /><br /> SQL_ASYNC_DBC_ENABLE_OFF = (valeur par défaut) désactive l’opération asynchrone pour les fonctions liées à la connexion spécifiées.<br /><br /> La définition de SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE est toujours synchrone (autrement dit, elle ne retourne jamais SQL_STILL_EXECUTING).<br /><br /> L’exécution asynchrone des opérations d’instruction est activée avec SQL_ATTR_ASYNC_ENABLE.|  
|SQL_ATTR_ASYNC_DBC_PCALLBACK (ODBC 3,8)|Valeur SQLPOINTER qui pointe vers la structure de contexte.<br /><br /> Seul le gestionnaire de pilotes peut appeler la fonction **SQLSetStmtAttr** d’un pilote avec cet attribut.|  
|SQL_ATTR_ASYNC_DBC_PCONTEXT (ODBC 3,8)|Valeur SQLPOINTER qui pointe vers la structure de contexte.<br /><br /> Seul le gestionnaire de pilotes peut appeler la fonction **SQLSetStmtAttr** d’un pilote avec cet attribut.|  
|SQL_ATTR_ASYNC_ENABLE (ODBC 3,0)|Valeur SQLULEN qui spécifie si une fonction appelée avec une instruction sur la connexion spécifiée est exécutée de façon asynchrone :<br /><br /> SQL_ASYNC_ENABLE_OFF = désactiver la prise en charge de l’exécution asynchrone au niveau de la connexion pour les opérations d’instruction (valeur par défaut).<br /><br /> SQL_ASYNC_ENABLE_ON = activer la prise en charge de l’exécution asynchrone au niveau de la connexion pour les opérations d’instruction.<br /><br /> Cet attribut peut être défini si **SQLGetInfo** avec le type d’informations SQL_ASYNC_MODE retourne SQL_AM_CONNECTION ou SQL_AM_STATEMENT.|  
|SQL_ATTR_AUTO_IPD (ODBC 3,0)|Valeur SQLUINTEGER en lecture seule qui spécifie si le remplissage automatique de la IPD après un appel à **SQLPrepare** est pris en charge :<br /><br /> SQL_TRUE = remplissage automatique de la IPD après un appel à **SQLPrepare** est pris en charge par le pilote.<br /><br /> SQL_FALSE = remplissage automatique de la IPD après un appel à **SQLPrepare** n’est pas pris en charge par le pilote. Les serveurs qui ne prennent pas en charge les instructions préparées ne seront pas en mesure de remplir automatiquement l’IPD.<br /><br /> Si SQL_TRUE est retourné pour l’attribut de connexion SQL_ATTR_AUTO_IPD, l’attribut d’instruction SQL_ATTR_ENABLE_AUTO_IPD peut être défini pour activer ou désactiver le remplissage automatique de l’IPD. Si SQL_ATTR_AUTO_IPD est SQL_FALSE, SQL_ATTR_ENABLE_AUTO_IPD ne peut pas être défini sur SQL_TRUE. La valeur par défaut de SQL_ATTR_ENABLE_AUTO_IPD est égale à la valeur de SQL_ATTR_AUTO_IPD.<br /><br /> Cet attribut de connexion peut être retourné par **SQLGetConnectAttr** mais ne peut pas être défini par **SQLSetConnectAttr**.|  
|SQL_ATTR_AUTOCOMMIT (ODBC 1,0)|Valeur de SQLUINTEGER qui spécifie s’il faut utiliser le mode de validation automatique ou de validation manuelle :<br /><br /> SQL_AUTOCOMMIT_OFF = le pilote utilise le mode de validation manuelle, et l’application doit valider ou restaurer explicitement des transactions avec **SQLEndTran**.<br /><br /> SQL_AUTOCOMMIT_ON = le pilote utilise le mode de validation automatique. Chaque instruction est validée immédiatement après son exécution. Il s’agit de la valeur par défaut. Toutes les transactions ouvertes sur la connexion sont validées lorsque SQL_ATTR_AUTOCOMMIT est défini sur SQL_AUTOCOMMIT_ON pour passer du mode de validation manuelle au mode de validation automatique.<br /><br /> Pour plus d’informations, consultez [mode de validation](../../../odbc/reference/develop-app/commit-mode.md). **Important :**  Certaines sources de données suppriment les plans d’accès et ferment les curseurs pour toutes les instructions d’une connexion chaque fois qu’une instruction est validée ; le mode de validation automatique peut provoquer cette situation après l’exécution de chaque instruction non-requête ou lorsque le curseur est fermé pour une requête. Pour plus d’informations, consultez les types d’informations SQL_CURSOR_COMMIT_BEHAVIOR et SQL_CURSOR_ROLLBACK_BEHAVIOR dans [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) et [effet des transactions sur les curseurs et les instructions préparées](../../../odbc/reference/develop-app/effect-of-transactions-on-cursors-and-prepared-statements.md). <br /><br /> Lorsqu’un lot est exécuté en mode de validation automatique, deux opérations sont possibles. Le lot entier peut être traité comme une unité autovalidable, ou chaque instruction d’un lot est traitée comme une unité autovalidable. Certaines sources de données peuvent prendre en charge ces deux comportements et peuvent offrir un moyen de choisir l’un ou l’autre. Elle est définie par le pilote, qu’un lot soit traité comme une unité autovalidable ou que chaque instruction du lot soit validée automatiquement.|  
|SQL_ATTR_CONNECTION_DEAD<br /><br /> (ODBC 3,5)|Valeur SQLUINTEGER en lecture seule qui indique l’état de la connexion. Si SQL_CD_TRUE, la connexion a été perdue. Si SQL_CD_FALSE, la connexion est toujours active.|  
|SQL_ATTR_CONNECTION_TIMEOUT (ODBC 3,0)|Valeur SQLUINTEGER correspondant au nombre de secondes d’attente de la fin d’une requête sur la connexion avant de retourner à l’application. Le pilote doit retourner SQLSTATE HYT00 (délai d’expiration expiré) à chaque fois qu’il est possible d’expirer dans une situation qui n’est pas associée à l’exécution de la requête ou à la connexion.<br /><br /> Si *ValuePtr* est égal à 0 (valeur par défaut), il n’y a aucun délai d’attente.|  
|SQL_ATTR_CURRENT_CATALOG (ODBC 2,0)|Chaîne de caractères contenant le nom du catalogue à utiliser par la source de données. Par exemple, dans SQL Server, le catalogue est une base de données. par conséquent, le pilote envoie une instruction **use** _Database_ à la source de données, où *Database* est la base de données spécifiée dans \* *ValuePtr*. Pour un pilote à niveau unique, le catalogue peut être un répertoire, donc le pilote remplace son répertoire actuel par le répertoire spécifié dans **ValuePtr*.|  
|SQL_ATTR_DBC_INFO_TOKEN (ODBC 3,8|Valeur SQLPOINTER utilisée pour rétablir le jeton d’informations de connexion dans le handle DBC lorsque [](../../../odbc/reference/syntax/sqlrateconnection-function.md)le paramètre ( \* *pRating*) du SQLRateConnection n’est pas égal à 100.<br /><br /> SQL_ATTR_DBC_INFO_TOKEN est défini uniquement. Il n’est pas possible d’utiliser **SQLGetConnectAttr** ou **sqlgetconnectoption,** pour récupérer cette valeur. Le pilote **SQLSetConnectAttr** du gestionnaire de pilotes n’accepte pas les SQL_ATTR_DBC_INFO_TOKEN, car une application ne doit pas définir cet attribut.<br /><br /> Si un pilote retourne SQL_ERROR après la définition de SQL_ATTR_DBC_INFO_TOKEN, la connexion obtenue à partir du pool est libérée. Le gestionnaire de pilotes essaiera ensuite d’obtenir une autre connexion à partir du pool. Pour plus d’informations, consultez [développement Connection-Pool la sensibilisation à un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md) .|  
|SQL_ATTR_ENLIST_IN_DTC (ODBC 3,0)|Valeur de SQLPOINTER qui spécifie s’il faut utiliser le pilote ODBC dans les transactions distribuées coordonnées par les services de composants Microsoft.<br /><br /> Transmettez un objet transaction OLE DTC qui spécifie la transaction à exporter vers SQL Server ou SQL_DTC_DONE pour mettre fin à l’Association DTC de la connexion.<br /><br /> Le client appelle la méthode Microsoft Distributed Transaction Coordinator (MS DTC) OLE ITransactionDispenser :: BeginTransaction pour commencer une transaction MS DTC et créer un objet de transaction MS DTC qui représente la transaction. L’application appelle ensuite SQLSetConnectAttr avec l’option SQL_ATTR_ENLIST_IN_DTC pour associer l’objet de transaction à la connexion ODBC. Toute l'activité de base de données connexe sera effectuée sous la protection de la transaction MS DTC. L'application appelle SQLSetConnectAttr avec SQL_DTC_DONE pour mettre un terme à l'association de DTC avec la connexion. Pour plus d'informations, consultez la documentation MS DTC.|  
|SQL_ATTR_LOGIN_TIMEOUT (ODBC 1,0)|Valeur SQLUINTEGER correspondant au nombre de secondes d’attente de la fin d’une demande de connexion avant de retourner à l’application. La valeur par défaut dépend du pilote. Si *ValuePtr* a la valeur 0, le délai d’attente est désactivé et une tentative de connexion attend indéfiniment.<br /><br /> Si le délai d’expiration spécifié dépasse le délai d’expiration de connexion maximal dans la source de données, le pilote remplace cette valeur et retourne SQLSTATE 01S02 ne (valeur d’option modifiée).|  
|SQL_ATTR_METADATA_ID (ODBC 3,0)|Valeur SQLUINTEGER qui détermine la façon dont les arguments de chaîne des fonctions de catalogue sont traités.<br /><br /> Si SQL_TRUE, l’argument de chaîne des fonctions de catalogue est traité comme des identificateurs. Le cas n’est pas significatif. Pour les chaînes non délimitées, le pilote supprime tous les espaces de fin et la chaîne est pliée en majuscules. Pour les chaînes délimitées, le pilote supprime tous les espaces de début ou de fin et prend littéralement tout ce qui se trouve entre les délimiteurs. Si l’un de ces arguments est défini sur un pointeur null, la fonction retourne SQL_ERROR et SQLSTATE HY009 (utilisation non valide du pointeur null).<br /><br /> Si SQL_FALSE, les arguments de chaîne des fonctions de catalogue ne sont pas traités comme des identificateurs. Le cas est significatif. Ils peuvent contenir un modèle de recherche de chaîne ou non, en fonction de l’argument.<br /><br /> La valeur par défaut est SQL_FALSE.<br /><br /> L’argument *TABLETYPE* de **SQLTables**, qui prend une liste de valeurs, n’est pas affecté par cet attribut.<br /><br /> Les SQL_ATTR_METADATA_ID peuvent également être définies au niveau de l’instruction. (Il s’agit du seul attribut de connexion qui est également un attribut d’instruction.)<br /><br /> Pour plus d’informations, consultez [arguments dans les fonctions de catalogue](../../../odbc/reference/develop-app/arguments-in-catalog-functions.md).|  
|SQL_ATTR_ODBC_CURSORS (ODBC 2,0)|Valeur SQLULEN spécifiant le mode d’utilisation de la bibliothèque de curseurs ODBC par le gestionnaire de pilotes :<br /><br /> SQL_CUR_USE_IF_NEEDED = le gestionnaire de pilotes utilise la bibliothèque de curseurs ODBC uniquement si nécessaire. Si le pilote prend en charge l’option SQL_FETCH_PRIOR dans **SQLFetchScroll**, le gestionnaire de pilotes utilise les fonctionnalités de défilement du pilote. Dans le cas contraire, il utilise la bibliothèque de curseurs ODBC.<br /><br /> SQL_CUR_USE_ODBC = le gestionnaire de pilotes utilise la bibliothèque de curseurs ODBC.<br /><br /> SQL_CUR_USE_DRIVER = le gestionnaire de pilotes utilise les fonctionnalités de défilement du pilote. Il s'agit du paramètre par défaut.<br /><br /> Pour plus d’informations sur la bibliothèque de curseurs ODBC, consultez [annexe F : bibliothèque de curseurs ODBC](../../../odbc/reference/appendixes/appendix-f-odbc-cursor-library.md). **Avertissement :**  La bibliothèque de curseurs sera supprimée dans une future version de Windows. Évitez d’utiliser cette fonctionnalité dans de nouveaux travaux de développement et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Microsoft recommande l’utilisation de la fonctionnalité de curseur du pilote.|  
|SQL_ATTR_PACKET_SIZE (ODBC 2,0)|Valeur SQLUINTEGER spécifiant la taille du paquet réseau en octets. **Remarque :**  De nombreuses sources de données ne prennent pas en charge cette option ou ne peuvent être retournées que si la taille du paquet réseau n’est pas définie. <br /><br /> Si la taille spécifiée dépasse la taille de paquet maximale ou est inférieure à la taille de paquet minimale, le pilote remplace cette valeur et retourne SQLSTATE 01S02 ne (valeur d’option modifiée).<br /><br /> Si l’application définit la taille du paquet après qu’une connexion a déjà été établie, le pilote retourne SQLSTATE HY011 (l’attribut ne peut pas être défini maintenant).|  
|SQL_ATTR_QUIET_MODE (ODBC 2,0)|Handle de fenêtre (HWND).<br /><br /> Si le handle de fenêtre est un pointeur null, le pilote n’affiche pas de boîtes de dialogue.<br /><br /> Si le handle de fenêtre n’est pas un pointeur null, il doit s’agir du handle de fenêtre parente de l’application. Il s’agit de la valeur par défaut. Le pilote utilise ce handle pour afficher des boîtes de dialogue. **Remarque :**  L’attribut de connexion SQL_ATTR_QUIET_MODE ne s’applique pas aux boîtes de dialogue affichées par **SQLDriverConnect**.|  
|SQL_ATTR_TRACE (ODBC 1,0)|Une valeur SQLUINTEGER indiquant au gestionnaire de pilotes s’il faut effectuer le suivi :<br /><br /> SQL_OPT_TRACE_OFF = non suivi (valeur par défaut)<br /><br /> SQL_OPT_TRACE_ON = suivi sur<br /><br /> Lorsque le suivi est activé, le gestionnaire de pilotes écrit chaque appel de fonction ODBC dans le fichier de trace. **Remarque :**  Lorsque le suivi est activé, le gestionnaire de pilotes peut retourner SQLSTATE IM013 (erreur de fichier de trace) à partir de n’importe quelle fonction. <br /><br /> Une application spécifie un fichier de trace avec l’option SQL_ATTR_TRACEFILE. Si le fichier existe déjà, le gestionnaire de pilotes s’ajoute au fichier. Dans le cas contraire, il crée le fichier. Si le suivi est activé et qu’aucun fichier de trace n’a été spécifié, le gestionnaire de pilotes écrit dans le fichier SQL. Connectez-vous au répertoire racine.<br /><br /> Une application peut définir la variable **ODBCSharedTraceFlag** pour activer le traçage dynamique. Le suivi est ensuite activé pour toutes les applications ODBC en cours d’exécution. Si une application désactive le suivi, elle est désactivée uniquement pour cette application.<br /><br /> Si le mot clé **trace** dans les informations système est défini sur 1 lorsqu’une application appelle **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV, le suivi est activé pour tous les descripteurs. Elle est activée uniquement pour l’application qui a appelé **SQLAllocHandle**.<br /><br /> L’appel de **SQLSetConnectAttr** avec un *attribut* de SQL_ATTR_TRACE ne nécessite pas que l’argument *ConnectionHandle* soit valide et ne retourne pas SQL_ERROR si *ConnectionHandle* a la valeur null. Cet attribut s’applique à toutes les connexions.|  
|SQL_ATTR_TRACEFILE (ODBC 1,0)|Chaîne de caractères se terminant par un caractère null qui contient le nom du fichier de trace.<br /><br /> La valeur par défaut de l’attribut SQL_ATTR_TRACEFILE est spécifiée avec le mot clé **TRACEFILE** dans les informations système. Pour plus d’informations, consultez la rubrique [sous-clé ODBC](../../../odbc/reference/install/odbc-subkey.md).<br /><br /> L’appel de **SQLSetConnectAttr** avec un *attribut* de SQL_ATTR_TRACEFILE ne requiert pas que l’argument *ConnectionHandle* soit valide et ne retourne pas SQL_ERROR si *ConnectionHandle* n’est pas valide. Cet attribut s’applique à toutes les connexions.|  
|SQL_ATTR_TRANSLATE_LIB (ODBC 1,0)|Chaîne de caractères se terminant par un caractère null qui contient le nom d’une bibliothèque contenant les fonctions **SQLDriverToDataSource** et **SQLDataSourceToDriver** auxquelles le pilote accède pour effectuer des tâches telles que la traduction de jeu de caractères. Cette option peut être spécifiée uniquement si le pilote s’est connecté à la source de données. Le paramètre de cet attribut est conservé entre les connexions. Pour plus d’informations sur la traduction de données, consultez [dll de traduction](../../../odbc/reference/develop-app/translation-dlls.md) et [référence des fonctions DLL de traduction](../../../odbc/reference/syntax/translation-dll-api-reference.md).|  
|SQL_ATTR_TRANSLATE_OPTION (ODBC 1,0)|Valeur d’indicateur 32 bits qui est passée à la DLL de traduction. Cet attribut peut être spécifié uniquement si le pilote s’est connecté à la source de données. Pour plus d’informations sur la traduction de données, consultez [dll de traduction](../../../odbc/reference/develop-app/translation-dlls.md).|  
|SQL_ATTR_TXN_ISOLATION (ODBC 1,0)|Masque de bits 32 bits qui définit le niveau d’isolation de la transaction pour la connexion actuelle. Une application doit appeler **SQLEndTran** pour valider ou restaurer toutes les transactions ouvertes sur une connexion, avant d’appeler **SQLSetConnectAttr** avec cette option.<br /><br /> Les valeurs valides pour *ValuePtr* peuvent être déterminées en appelant **SQLGetInfo** avec *infotype* égal à SQL_TXN_ISOLATION_OPTIONS.<br /><br /> Pour obtenir une description des niveaux d’isolation des transactions, consultez la description du type d’informations SQL_DEFAULT_TXN_ISOLATION dans [SQLGetInfo](../../../odbc/reference/syntax/sqlgetinfo-function.md) et les [niveaux d’isolation des transactions](../../../odbc/reference/develop-app/transaction-isolation-levels.md).|  
  
 [1] ces fonctions peuvent être appelées de façon asynchrone uniquement si le descripteur est un descripteur d’implémentation et non un descripteur d’application.  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Renvoi du paramètre d’un attribut de connexion|[Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
