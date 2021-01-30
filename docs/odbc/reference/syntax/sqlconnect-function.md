---
description: Fonction SQLConnect
title: Fonction SQLConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLConnect
helpviewer_keywords:
- SQLConnect function [ODBC]
ms.assetid: 59075e46-a0ca-47bf-972a-367b08bb518d
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 90d982db52e0656fba25eb54065ea84b751a7b18
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206138"
---
# <a name="sqlconnect-function"></a>Fonction SQLConnect
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLConnect** établit des connexions à un pilote et à une source de données. Le handle de connexion fait référence au stockage de toutes les informations sur la connexion à la source de données, y compris l’État, l’état de la transaction et les informations sur l’erreur.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLConnect(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      ServerName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      UserName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      Authentication,  
     SQLSMALLINT    NameLength3);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *ServerName*  
 Entrée Nom de la source de données. Les données peuvent se trouver sur le même ordinateur que le programme, ou sur un autre ordinateur sur un réseau. Pour plus d’informations sur la façon dont une application choisit une source de données, consultez [choix d’une source de données ou](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)d’un pilote.  
  
 *NameLength1*  
 Entrée Longueur de **ServerName* en caractères.  
  
 *UserName*  
 Entrée Identificateur de l’utilisateur.  
  
 *NameLength2*  
 Entrée Longueur de **nom d’utilisateur* en caractères.  
  
 *Authentification*  
 Entrée Chaîne d’authentification (généralement le mot de passe).  
  
 *NameLength3*  
 Entrée Longueur de **authentification* en caractères.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLConnect** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLConnect** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée de l’argument *ValuePtr* dans **SQLSetConnectAttr** et substituait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08001|Le client ne peut pas établir la connexion|Le pilote n’a pas pu établir une connexion avec la source de données.|  
|08002|Nom de connexion en cours d’utilisation|(DM) le *ConnectionHandle* spécifié a déjà été utilisé pour établir une connexion avec une source de données, et la connexion était toujours ouverte ou l’utilisateur parvient à accéder à une connexion.|  
|08004|Le serveur a rejeté la connexion|La source de données a rejeté l’établissement de la connexion pour des raisons définies par l’implémentation.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote essayait de se connecter a échoué avant la fin du traitement de la fonction.|  
|28000|Spécification d’autorisation non valide|La valeur spécifiée pour l’argument *nom d’utilisateur* ou la valeur spécifiée pour l' *authentification* de l’argument n’a pas respecté les restrictions définies par la source de données.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|(DM) le gestionnaire de pilotes n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *ConnectionHandle*. La fonction **SQLConnect** a été appelée, et avant la fin de l’exécution, la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) a été appelée sur le *ConnectionHandle*, puis la fonction **SQLConnect** a été appelée à nouveau sur le *ConnectionHandle*.<br /><br /> Ou bien, la fonction **SQLConnect** a été appelée et avant la fin de l’exécution, **SQLCancelHandle** a été appelé sur le *ConnectionHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *NameLength1*, *NameLength2* ou *NameLength3* est inférieure à 0, mais n’est pas égale à SQL_NTS.<br /><br /> (DM) la valeur spécifiée pour l’argument *NameLength1* a dépassé la longueur maximale pour un nom de source de données.|  
|HYT00|Délai expiré|Le délai d’expiration de la requête a expiré avant la fin de la connexion à la source de données. Le délai d’attente est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HY114|Le pilote ne prend pas en charge l’exécution de fonctions asynchrones au niveau de la connexion|(DM) l’application a activé l’opération asynchrone sur le handle de connexion avant d’établir la connexion. Toutefois, le pilote ne prend pas en charge les opérations asynchrones sur le handle de connexion.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote spécifié par le nom de la source de données ne prend pas en charge la fonction.|  
|IM002|Source de données introuvable et aucun pilote par défaut spécifié|(DM) le nom de source de données spécifié dans l’argument *ServerName* est introuvable dans les informations système, et il n’y avait pas de spécification de pilote par défaut.|  
|IM003|Impossible de connecter le pilote spécifié à|(DM) le pilote listé dans la spécification de la source de données dans informations système est introuvable ou n’a pas pu être connecté pour une autre raison.|  
|IM004|Échec du SQLAllocHandle du pilote sur SQL_HANDLE_ENV|(DM) au cours de **SQLConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *comme HandleType* de SQL_HANDLE_ENV et le pilote a renvoyé une erreur.|  
|IM005|Échec du SQLAllocHandle du pilote sur SQL_HANDLE_DBC|(DM) au cours de **SQLConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *comme HandleType* de SQL_HANDLE_DBC et le pilote a renvoyé une erreur.|  
|IM006|Échec de SQLSetConnectAttr du pilote|Au cours de **SQLConnect**, le gestionnaire de pilotes a appelé la fonction **SQLSetConnectAttr** du pilote et le pilote a renvoyé une erreur. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|IM009|Impossible de se connecter à la DLL de traduction|Le pilote n’a pas pu se connecter à la DLL de traduction spécifiée pour la source de données.|  
|IM010|Le nom de la source de données est trop long|Le nom du *\* serveur* (DM) est plus long que SQL_MAX_DSN_LENGTH caractères.|  
|IM014|Le nom de source de donnée spécifié contient une incompatibilité d’architecture entre le pilote et l’application|(DM) l’application 32 bits utilise un DSN se connectant à un pilote 64 bits ; ou vice versa.|  
|IM015|Échec de SQLConnect sur SQL_HANDLE_DBC_INFO_HANDLE du pilote|Si un pilote retourne SQL_ERROR, le gestionnaire de pilotes renverra SQL_ERROR à l’application et la connexion échouera.<br /><br /> Pour plus d’informations sur la SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement de la sensibilisation aux Connection-Pool dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
|S1118|Le pilote ne prend pas en charge la notification asynchrone|Si le pilote ne prend pas en charge les notifications asynchrones, vous ne pouvez pas définir SQL_ATTR_ASYNC_DBC_EVENT ou SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Commentaires  
 Pour plus d’informations sur la raison pour laquelle une application utilise **SQLConnect**, consultez [connexion avec SQLConnect](../../../odbc/reference/develop-app/connecting-with-sqlconnect.md).  
  
 Le gestionnaire de pilotes ne se connecte pas à un pilote tant que l’application n’a pas appelé une fonction (**SQLConnect**, **SQLDriverConnect** ou **SQLBrowseConnect**) pour se connecter au pilote. Jusqu’à ce stade, le gestionnaire de pilotes fonctionne avec ses propres Handles et gère les informations de connexion. Lorsque l’application appelle une fonction de connexion, le gestionnaire de pilotes vérifie si un pilote est actuellement connecté pour le *ConnectionHandle* spécifié :  
  
-   Si un pilote n’est pas connecté à, le gestionnaire de pilotes se connecte au pilote et appelle **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV, **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC, **SQLSetConnectAttr** (si l’application a spécifié des attributs de connexion) et la fonction de connexion dans le pilote. Le gestionnaire de pilotes retourne SQLSTATE IM006 (échec de **SQLSetConnectOption** du pilote) et SQL_SUCCESS_WITH_INFO pour la fonction de connexion si le pilote a retourné une erreur pour **SQLSetConnectAttr**. Pour plus d’informations, consultez [connexion à une source de données ou à un pilote](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md).  
  
-   Si le pilote spécifié est déjà connecté à sur le *ConnectionHandle*, le gestionnaire de pilotes appelle uniquement la fonction de connexion dans le pilote. Dans ce cas, le pilote doit s’assurer que tous les attributs de connexion du *ConnectionHandle* conservent leurs paramètres actuels.  
  
-   Si un autre pilote est connecté, le gestionnaire de pilotes appelle **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DBC, puis, si aucun autre pilote n’est connecté dans cet environnement, il appelle **sqlfreehandle** avec un *comme HandleType* de SQL_HANDLE_ENV dans le pilote connecté, puis il déconnecte ce pilote. Il effectue ensuite les mêmes opérations que lorsqu’un pilote n’est pas connecté à.  
  
 Le pilote alloue ensuite des handles et s’initialise lui-même.  
  
 Lorsque l’application appelle **SQLDisconnect**, le gestionnaire de pilotes appelle **SQLDisconnect** dans le pilote. Toutefois, il ne déconnecte pas le pilote. Cela permet de conserver le pilote en mémoire pour les applications qui se connectent et se déconnectent à plusieurs reprises d’une source de données. Lorsque l’application appelle **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_DBC, le gestionnaire de pilotes appelle **sqlfreehandle** avec un *comme HandleType* de SQL_HANDLE_DBC, puis **SQLFreeHandle** avec un *comme HandleType* de SQL_HANDLE_ENV dans le pilote, puis déconnecte le pilote.  
  
 Une application ODBC peut établir plusieurs connexions.  
  
## <a name="driver-manager-guidelines"></a>Instructions du gestionnaire de pilotes  
 Le contenu de **ServerName* affecte la façon dont le gestionnaire de pilotes et un pilote fonctionnent ensemble pour établir une connexion à une source de données.  
  
-   Si \* *ServerName* contient un nom de source de données valide, le gestionnaire de pilotes localise la spécification de source de données correspondante dans les informations système et se connecte au pilote associé. Le gestionnaire de pilotes passe chaque argument **SQLConnect** au pilote.  
  
-   Si le nom de la source de données est introuvable ou si *ServerName* est un pointeur null, le gestionnaire de pilotes localise la spécification de la source de données par défaut et se connecte au pilote associé. Le gestionnaire de pilotes passe au pilote les arguments *username* et *Authentication* inchangés et « Default » pour l’argument *ServerName* .  
  
-   Si l’argument *ServerName* est défini sur « Default », le gestionnaire de pilotes localise la spécification de la source de données par défaut et se connecte au pilote associé. Le gestionnaire de pilotes passe chaque argument **SQLConnect** au pilote.  
  
-   Si le nom de la source de données est introuvable ou si *ServerName* est un pointeur null et que la spécification de la source de données par défaut n’existe pas, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE IM002 (le nom de la source de données est introuvable et aucun pilote par défaut n’a été spécifié).  
  
 Une fois connecté à par le gestionnaire de pilotes, un pilote peut localiser sa spécification de source de données correspondante dans les informations système et utiliser des informations spécifiques au pilote à partir de la spécification pour terminer son ensemble d’informations de connexion requises.  
  
 Si une bibliothèque de traduction par défaut est spécifiée dans les informations système de la source de données, le pilote s’y connecte. Une autre bibliothèque de traduction peut être connectée à en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_TRANSLATE_LIB. Vous pouvez spécifier une option de traduction en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_TRANSLATE_OPTION.  
  
 Si un pilote prend en charge **SQLConnect**, la section du mot clé Driver des informations système pour le pilote doit contenir le mot clé **ConnectFunctions** avec le premier caractère défini sur « Y ».  
  
### <a name="connection-pooling"></a>Regroupement de connexions  
 Le regroupement de connexions permet à une application de réutiliser une connexion qui a déjà été créée. Lorsque le regroupement de connexions est activé et que **SQLConnect** est appelé, le gestionnaire de pilotes essaie d’établir la connexion à l’aide d’une connexion qui fait partie d’un pool de connexions dans un environnement désigné pour le regroupement de connexions. Cet environnement est un environnement partagé qui est utilisé par toutes les applications qui utilisent les connexions dans le pool.  
  
 Le regroupement de connexions est activé avant l’allocation de l’environnement en appelant **SQLSetEnvAttr** pour définir SQL_ATTR_CONNECTION_POOLING sur SQL_CP_ONE_PER_DRIVER (qui spécifie un maximum d’un pool par pilote) ou SQL_CP_ONE_PER_HENV (qui spécifie un maximum d’un pool par environnement). Dans ce cas, **SQLSetEnvAttr** est appelé avec *EnvironmentHandle* défini sur null, ce qui fait de l’attribut un attribut de niveau processus. Si SQL_ATTR_CONNECTION_POOLING est défini sur SQL_CP_OFF, le regroupement de connexions est désactivé.  
  
 Une fois que le regroupement de connexions a été activé, **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_ENV est appelé pour allouer un environnement. L’environnement alloué par cet appel est un environnement partagé, car le regroupement de connexions a été activé. Toutefois, l’environnement qui sera utilisé n’est pas déterminé tant que **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC n’est pas appelé.  
  
 **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC est appelé pour allouer une connexion. Le gestionnaire de pilotes tente de trouver un environnement partagé existant qui correspond aux attributs d’environnement définis par l’application. Si aucun environnement de ce type n’existe, il est créé en tant qu' *environnement partagé* implicite. Si un environnement partagé correspondant est trouvé, le handle d’environnement est retourné à l’application et son décompte de références est incrémenté.  
  
 Toutefois, la connexion qui sera utilisée n’est pas déterminée tant que **SQLConnect** n’est pas appelé. À ce stade, le gestionnaire de pilotes tente de trouver une connexion existante dans le pool de connexions qui correspond aux critères demandés par l’application. Ces critères incluent les options de connexion demandées dans l’appel à **SQLConnect** (les valeurs des mots clés *ServerName*, *username* et *Authentication* ) et tous les attributs de connexion définis depuis **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC a été appelé. Le gestionnaire de pilotes vérifie ces critères par rapport aux mots clés et attributs de connexion correspondants dans les connexions dans le pool. Si une correspondance est trouvée, la connexion dans le pool est utilisée. Si aucune correspondance n’est trouvée, une nouvelle connexion est créée.  
  
 Si l’attribut d’environnement SQL_ATTR_CP_MATCH est défini sur SQL_CP_STRICT_MATCH, la correspondance doit être exacte pour qu’une connexion dans le pool soit utilisée. Si l’attribut d’environnement SQL_ATTR_CP_MATCH est défini sur SQL_CP_RELAXED_MATCH, les options de connexion dans l’appel à **SQLConnect** doivent correspondre, mais tous les attributs de connexion doivent correspondre.  
  
 Les règles suivantes sont appliquées lorsqu’un attribut de connexion, tel que défini par l’application avant **SQLConnect** est appelé, ne correspond pas à l’attribut de connexion de la connexion dans le pool :  
  
-   Si l’attribut de connexion doit être défini avant que la connexion soit établie :  
  
     Si SQL_ATTR_CP_MATCH est SQL_CP_STRICT_MATCH, SQL_ATTR_PACKET_SIZE dans la connexion regroupée doit être identique à l’attribut défini par l’application. Si SQL_CP_RELAXED_MATCH, les valeurs de SQL_ATTR_PACKET_SIZE peuvent être différentes.  
  
     La valeur de SQL_ATTR_LOGIN_VALUE n’affecte pas la correspondance.  
  
-   Si l’attribut de connexion peut être défini avant ou après l’établissement de la connexion :  
  
     Si l’attribut de connexion n’a pas été défini par l’application mais a été défini sur la connexion dans le pool, et qu’il existe une valeur par défaut, l’attribut de connexion dans la connexion regroupée est rétabli à la valeur par défaut et une correspondance est déclarée. Si aucune valeur par défaut n’est définie, la connexion regroupée n’est pas considérée comme une correspondance.  
  
     Si l’attribut de connexion a été défini par l’application mais n’a pas été défini sur la connexion dans le pool, l’attribut de connexion sur le pool est remplacé par celui défini par l’application et une correspondance est déclarée.  
  
     Si l’attribut de connexion a été défini par l’application et a également été défini sur la connexion dans le pool, mais que les valeurs sont différentes, la valeur de l’attribut de connexion de l’application est utilisée et une correspondance est déclarée.  
  
-   Si les valeurs des attributs de connexion spécifiques au pilote ne sont pas identiques et que SQL_ATTR_CP_MATCH est défini sur SQL_CP_STRICT_MATCH, la connexion dans le pool n’est pas utilisée.  
  
 Lorsque l’application appelle **SQLDisconnect** pour se déconnecter, la connexion est retournée au pool de connexions et peut être réutilisée.  
  
### <a name="optimizing-connection-pooling-performance"></a>Optimisation des performances du regroupement de connexions  
 Lorsque des transactions distribuées sont impliquées, il est possible d’optimiser les performances du regroupement de connexions à l’aide de **SQL_DTC_TRANSITION_COST**, qui est un masque de SQLUINTEGER. Les transitions référencées sont les transitions de l’attribut de connexion SQL_ATTR_ENLIST_IN_DTC passage de la valeur 0 à la valeur différente de zéro, et vice versa. Il s’agit d’une connexion qui n’est pas inscrite dans une transaction distribuée à inscrite dans une transaction distribuée, et vice versa. Selon la façon dont le pilote a implémenté l’inscription (définition de l’attribut de connexion SQL_ATTR_ENLIST_IN_DTC), ces transitions peuvent être onéreuses et doivent donc être évitées pour des performances optimales.  
  
 La valeur retournée par le pilote contient une combinaison des bits suivants :  
  
-   **SQL_DTC_ENLIST_EXPENSIVE**, quand elle est définie, implique que la transition zéro vers une autre valeur est beaucoup plus coûteuse qu’une transition de la valeur différente de zéro à une autre valeur différente de zéro (en inscrit une connexion précédemment inscrite dans sa prochaine transaction).  
  
-   **SQL_DTC_UNENLIST_EXPENSIVE**, quand elle est définie, implique que la transition différente de zéro à zéro est beaucoup plus coûteuse que l’utilisation d’une connexion dont l’attribut SQL_ATTR_ENLIST_IN_DTC est déjà défini à zéro.  
  
 Il existe un compromis entre performances et utilisation de la connexion. Si un pilote indique qu’une ou plusieurs de ces transitions sont coûteuses, le dispositif de regroupement de connexions du gestionnaire de pilotes y répond en conservant plus de connexions dans le pool. Certaines des connexions dans le pool sont préférables pour une utilisation non transactionnelle, et d’autres sont préférables pour une utilisation transactionnelle. Toutefois, si le pilote indique que ces transitions ne sont pas coûteuses, il est possible d’utiliser moins de connexions, par exemple en alternant entre une utilisation transactionnelle et non transactionnelle.  
  
 Les pilotes qui ne prennent pas en charge les SQL_ATTR_ENLIST_IN_DTC n’ont pas besoin de prendre en charge SQL_DTC_TRANSITION_COST. Pour les pilotes qui prennent en charge SQL_ATTR_ENLIST_IN_DTC mais pas SQL_DTC_TRANSITION_COST, il est supposé que les transitions ne sont pas coûteuses, comme si le pilote retournait 0 (aucun bit défini) pour cette valeur.  
  
 Bien que SQL_DTC_TRANSITION_COST ait été introduite dans ODBC 3,5, ODBC 2. *x* peut également le prendre en charge, car le gestionnaire de pilotes interrogera ces informations quelle que soit la version du pilote.  
  
### <a name="code-example"></a>Exemple de code  
 Dans l’exemple suivant, une application alloue des handles d’environnement et de connexion. Il se connecte ensuite à la source de données SalesOrders avec l’ID d’utilisateur JohnS et le mot de passe Sesame et traite les données. Lorsqu’il a terminé le traitement des données, il se déconnecte de la source de données et libère les handles.  
  
```cpp  
// SQLConnect_ref.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR * OutConnStr = (SQLCHAR * )malloc(255);  
   SQLSMALLINT * OutConnStrLen = (SQLSMALLINT *)malloc(255);  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (void*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            // Connect to data source  
            retcode = SQLConnect(hdbc, (SQLCHAR*) "NorthWind", SQL_NTS, (SQLCHAR*) NULL, 0, NULL, 0);  
  
            // Allocate statement handle  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);   
  
               // Process data  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               }  
  
               SQLDisconnect(hdbc);  
            }  
  
            SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
         }  
      }  
      SQLFreeHandle(SQL_HANDLE_ENV, henv);  
   }  
}  
```  
  
### <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Découverte et énumération des valeurs requises pour se connecter à une source de données|[Fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Déconnexion d’une source de données|[SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Connexion à une source de données à l’aide d’une chaîne de connexion ou d’une boîte de dialogue|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Renvoi du paramètre d’un attribut de connexion|[Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
