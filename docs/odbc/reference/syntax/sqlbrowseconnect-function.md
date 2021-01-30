---
description: Fonction SQLBrowseConnect
title: Fonction SQLBrowseConnect | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLBrowseConnect
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLBrowseConnect
helpviewer_keywords:
- SQLBrowseConnect function [ODBC]
ms.assetid: b7f1be66-e6c7-4790-88ec-62b7662103c0
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33983c9fb41625fa6479c47045ca4a04738a1463
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181070"
---
# <a name="sqlbrowseconnect-function"></a>Fonction SQLBrowseConnect
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLBrowseConnect** prend en charge une méthode itérative pour découvrir et énumérer les attributs et les valeurs d’attribut requis pour se connecter à une source de données. Chaque appel à **SQLBrowseConnect** retourne des niveaux successifs d’attributs et de valeurs d’attribut. Lorsque tous les niveaux ont été énumérés, une connexion à la source de données est terminée et une chaîne de connexion complète est retournée par **SQLBrowseConnect**. Un code de retour de SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO indique que toutes les informations de connexion ont été spécifiées et que l’application est maintenant connectée à la source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLBrowseConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *InConnectionString*  
 Entrée Parcourir la chaîne de connexion de la demande (voir « argument *InConnectionString* » dans « Comments »).  
  
 *StringLength1*  
 Entrée Longueur de **InConnectionString* en caractères.  
  
 *OutConnectionString*  
 Sortie Pointeur vers une mémoire tampon de caractères dans laquelle retourner la chaîne de connexion du résultat de navigation (voir « argument *OutConnectionString* » dans « Comments »).  
  
 Si *OutConnectionString* a la valeur null, *StringLength2Ptr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *OutConnectionString*.  
  
 *BufferLength*  
 Entrée Longueur, en caractères, de la mémoire tampon **OutConnectionString* .  
  
 *StringLength2Ptr*  
 Sortie Nombre total de caractères (à l’exception de la fin null) pouvant être renvoyés dans \* *OutConnectionString*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength*, la chaîne de connexion dans \* *OutConnectionString* est tronquée à *BufferLength* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLBrowseConnect** retourne SQL_ERROR, SQL_SUCCESS_WITH_INFO ou SQL_NEED_DATA, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle de ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLBrowseConnect** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le OutConnectionString de mémoire tampon \*  n’est pas assez grand pour retourner la chaîne de connexion du résultat de navigation entier, donc la chaîne a été tronquée. La mémoire tampon **StringLength2Ptr* contient la longueur de la chaîne de connexion des résultats de navigation non tronqués. (La fonction retourne SQL_NEED_DATA.)|  
|01S00|Attribut de chaîne de connexion non valide|Un mot clé d’attribut non valide a été spécifié dans la chaîne de connexion de la requête de navigation (*InConnectionString*). (La fonction retourne SQL_NEED_DATA.)<br /><br /> Un mot clé d’attribut a été spécifié dans la chaîne de connexion de la demande de navigation (*InConnectionString*) qui ne s’applique pas au niveau de connexion actuel. (La fonction retourne SQL_NEED_DATA.)|  
|01S02 ne|Valeur modifiée|Le pilote ne prenait pas en charge la valeur spécifiée de l’argument *ValuePtr* dans **SQLSetConnectAttr** et substituait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08001|Le client ne peut pas établir la connexion|Le pilote n’a pas pu établir une connexion avec la source de données.|  
|08002|Nom de connexion en cours d’utilisation|(DM) la connexion spécifiée a déjà été utilisée pour établir une connexion avec une source de données, et la connexion a été ouverte.|  
|08004|Le serveur a rejeté la connexion|La source de données a rejeté l’établissement de la connexion pour des raisons définies par l’implémentation.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote essayait de se connecter a échoué avant la fin du traitement de la fonction.|  
|28000|Spécification d’autorisation non valide|L’identificateur ou la chaîne d’autorisation de l’utilisateur, ou les deux, comme spécifié dans la chaîne de connexion de la demande de navigation (*InConnectionString*), des restrictions sont enfreintes définies par la source de données.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|(DM) le gestionnaire de pilotes n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.<br /><br /> Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Une opération asynchrone a été annulée par l’appel de la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md). Ensuite, la fonction d’origine a été appelée à nouveau sur le *ConnectionHandle*.<br /><br /> Une opération a été annulée en appelant **SQLCancelHandle** sur le *ConnectionHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *StringLength1* est inférieure à 0 et n’est pas égale à SQL_NTS.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength* est inférieure à 0.|  
|HY114|Le pilote ne prend pas en charge l’exécution de fonctions asynchrones au niveau de la connexion|(DM) l’application a activé l’opération asynchrone sur le handle de connexion avant d’établir la connexion. Toutefois, le pilote ne prend pas en charge les opérations asynchrones sur un handle de connexion.|  
|HYT00|Délai expiré|Le délai d’expiration de la connexion a expiré avant la fin de la connexion à la source de données. Le délai d’attente est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant au nom de source de données spécifié ne prend pas en charge la fonction.|  
|IM002|Source de données introuvable et aucun pilote par défaut spécifié|(DM) le nom de source de données spécifié dans la chaîne de connexion de la demande de navigation (*InConnectionString*) est introuvable dans les informations système, et il n’y avait pas de spécification de pilote par défaut.<br /><br /> La source de données ODBC (DM) et les informations de pilote par défaut sont introuvables dans les informations système.|  
|IM003|Le pilote spécifié n’a pas pu être chargé|(DM) le pilote figurant dans la spécification de la source de données dans les informations système ou spécifié par le mot clé **Driver** est introuvable ou n’a pas pu être chargé pour une raison quelconque.|  
|IM004|Échec du **SQLAllocHandle** du pilote sur SQL_HANDLE _ENV|(DM) durant la période **SQLBrowseConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *comme HandleType* de SQL_HANDLE_ENV et le pilote a renvoyé une erreur.|  
|IM005|Échec du **SQLAllocHandle** du pilote sur SQL_HANDLE_DBC|(DM) durant la période **SQLBrowseConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *comme HandleType* de SQL_HANDLE_DBC et le pilote a renvoyé une erreur.|  
|IM006|Échec de **SQLSetConnectAttr** du pilote|(DM) pendant l’ajout de **SQLBrowseConnect**, le gestionnaire de pilotes a appelé la fonction **SQLSetConnectAttr** du pilote et le pilote a renvoyé une erreur.|  
|IM009|Impossible de charger la DLL de traduction|Le pilote n’a pas pu charger la DLL de traduction spécifiée pour la source de données ou pour la connexion.|  
|IM010|Le nom de la source de données est trop long|(DM) la valeur de l’attribut du mot clé DSN était supérieure à SQL_MAX_DSN_LENGTH caractères.|  
|IM011|Le nom du pilote est trop long|(DM) la valeur de l’attribut du mot clé DRIVER était supérieure à 255 caractères.|  
|IM012|Erreur de syntaxe du mot clé du pilote|(DM) la paire mot clé-valeur pour le mot clé DRIVER contenait une erreur de syntaxe.|  
|IM014|Le nom de source de donnée spécifié contient une incompatibilité d’architecture entre le pilote et l’application|(DM) l’application 32 bits utilise un DSN se connectant à un pilote 64 bits ; ou vice versa.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
|S1118|Le pilote ne prend pas en charge la notification asynchrone|Si le pilote ne prend pas en charge les notifications asynchrones, vous ne pouvez pas définir SQL_ATTR_ASYNC_DBC_EVENT ou SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="inconnectionstring-argument"></a>Argument InConnectionString  
 Une chaîne de connexion de requête de navigation a la syntaxe suivante :  
  
 *Connection-String* :: = *attribute*[ `;` ] &#124; *attribut* `;` *-chaîne de connexion*;<br>
 *attribute ::* = *attribute-Keyword* `=` *attribute-value* &#124; `DRIVER=` [ `{` ]*attribute-value*[ `}` ]<br>
 *attribute-Keyword* :: = `DSN` &#124; `UID` &#124; `PWD` &#124; *attribute-defined-Driver-Keyword*<br>
 *attribute-value* :: = *chaîne de caractères*<br>
 *Driver-Defined-Attribute-Keyword* :: = *identifier*<br>
  
 où *Character-String* comporte zéro caractère ou plus ; l' *identificateur* comporte un ou plusieurs caractères ; *attribute-Keyword* ne respecte pas la casse ; *attribute-value* peut être sensible à la casse ; et la valeur du mot clé **DSN** ne se compose pas uniquement d’espaces blancs. En raison de la grammaire des fichiers de chaîne de connexion et d’initialisation, les mots clés et les valeurs d’attribut qui contiennent les caractères **[] {} (),;? \* = ! @** doit être évité. En raison de la grammaire dans les informations système, les mots clés et les noms de sources de données ne peuvent pas contenir de barre oblique inverse ( \\ ). Pour ODBC 2. *x* Driver, les accolades sont requises autour de la valeur d’attribut du mot clé Driver.  
  
 Si des mots clés sont répétés dans la chaîne de connexion de la requête de navigation, le pilote utilise la valeur associée à la première occurrence du mot clé. Si les mots clés du **DSN** et du **pilote** sont inclus dans la même chaîne de connexion de demande de navigation, le gestionnaire de pilotes et le pilote utilisent le mot clé qui apparaît en premier.  
  
 Pour plus d’informations sur la façon dont une application choisit une source de données ou un pilote, consultez [choix d’une source de données ou](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)d’un pilote.  
  
## <a name="outconnectionstring-argument"></a>Argument OutConnectionString  
 La chaîne de connexion des résultats de navigation est une liste d’attributs de connexion. Un attribut de connexion se compose d’un mot clé d’attribut et d’une valeur d’attribut correspondante. La chaîne de connexion des résultats de navigation a la syntaxe suivante :  
  
 *Connection-String* :: = *attribute*[ `;` ] &#124; *attribut* `;` *-chaîne de connexion*<br>
 *attribute ::* = [ `*` ]*attribut-Keyword* `=` *attribute-value*<br>
 *attribute-Keyword* :: = *ODBC-attribute-Keyword* &#124;- *Defined-Attribute-Keyword*<br>
 *ODBC-attribute-Keyword* = `UID` { `PWD` &#124;} [ `:` *Localized-identifier*] *Driver-Defined-Attribute-Keyword* :: = *identifier*[ `:` *Localized-identifier*] *attribute-value* :: = `{` *attribute-value-List* `}` &#124; `?` (les accolades sont littérales ; elles sont retournées par le pilote.)<br>
 *attribute-value-List* :: = *chaîne de caractères* [ `:` *Localized-Character String*] &#124; *chaîne de caractères* [ `:` *Localized-Character String*] `,` *attribute-value-List*<br>
  
 où la chaîne de *caractères* et la *chaîne de caractères localisés* contiennent zéro, un ou plusieurs caractères ; l' *identificateur* et l' *identificateur localisé* comportent un ou plusieurs caractères ; *attribute-Keyword* ne respecte pas la casse ; et *attribute-value* peut être sensible à la casse. En raison de la grammaire des fichiers de chaîne de connexion et d’initialisation, les mots clés, les identificateurs localisés et les valeurs d’attribut qui contiennent les caractères **[] {} (),;? \* = ! @** doit être évité. En raison de la grammaire dans les informations système, les mots clés et les noms de sources de données ne peuvent pas contenir de barre oblique inverse ( \\ ).  
  
 La syntaxe de la chaîne de connexion des résultats de navigation est utilisée conformément aux règles sémantiques suivantes :  
  
-   Si un astérisque ( \* ) précède un *mot clé d’attribut*, l' *attribut* est facultatif et peut être omis dans l’appel suivant à **SQLBrowseConnect**.  
  
-   Les mots clés d’attribut **UID** et **PWD** ont la même signification que celle définie dans **SQLDriverConnect**.  
  
-   Nom du *mot-clé-définition du pilote* : type d’attribut pour lequel une valeur d’attribut peut être fournie. Par exemple, il peut s’agir d’un **serveur**, d’une **base de données**, d’un **hôte** ou d’un **SGBD**.  
  
-   *ODBC-attribute-Keywords* et *Driver-Defined-Attribute-Keywords* incluent une version localisée ou conviviale du mot clé. Cela peut être utilisé par les applications comme une étiquette dans une boîte de dialogue. Toutefois, **UID**, **PWD** ou l' *identificateur* seul doit être utilisé lors du passage d’une chaîne de requête de navigation au pilote.  
  
-   {*Attribute-value-List*} est une énumération des valeurs réelles valides pour le *mot clé attribute* correspondant. Notez que les accolades ( {} ) n’indiquent pas une liste de choix ; elles sont retournées par le pilote. Par exemple, il peut s’agir d’une liste de noms de serveurs ou d’une liste de noms de base de données.  
  
-   Si la *valeur d’attribut* est un point d’interrogation unique ( ?), une seule valeur correspond au *mot clé d’attribut*. Par exemple, UID = JohnS ; PWD = Sesame.  
  
-   Chaque appel à **SQLBrowseConnect** retourne uniquement les informations requises pour satisfaire le niveau suivant du processus de connexion. Le pilote associe les informations d’État au handle de connexion afin que le contexte puisse toujours être déterminé à chaque appel.  
  
## <a name="using-sqlbrowseconnect"></a>Utilisation de SQLBrowseConnect  
 **SQLBrowseConnect** requiert une connexion allouée. Le gestionnaire de pilotes charge le pilote qui a été spécifié dans ou qui correspond au nom de la source de données spécifié dans la chaîne de connexion de la demande de navigation initiale. Pour plus d’informations sur le moment où cela se produit, consultez la section « commentaires » dans la [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Le pilote peut établir une connexion avec la source de données pendant le processus de navigation. Si **SQLBrowseConnect** retourne SQL_ERROR, les connexions en suspens sont arrêtées et la connexion est retournée à un État non connecté.  
  
> [!NOTE]  
>  **SQLBrowseConnect** ne prend pas en charge le regroupement de connexions. Si **SQLBrowseConnect** est appelé alors que le regroupement de connexions est activé, SQLSTATE HY000 (erreur générale) est retourné.  
  
 Lorsque **SQLBrowseConnect** est appelé pour la première fois sur une connexion, la chaîne de connexion de la requête de navigation doit contenir le mot clé **DSN** ou le mot clé **Driver** . Si la chaîne de connexion de la requête de navigation contient le mot clé **DSN** , le gestionnaire de pilotes localise une spécification de source de données correspondante dans les informations système :  
  
-   Si le gestionnaire de pilotes trouve la spécification de source de données correspondante, il charge la DLL de pilote associée ; le pilote peut récupérer des informations sur la source de données à partir des informations système.  
  
-   Si le gestionnaire de pilotes ne trouve pas la spécification correspondante de la source de données, il localise la spécification de la source de données par défaut et charge la DLL du pilote associé ; le pilote peut récupérer des informations sur la source de données par défaut à partir des informations système. La valeur par défaut est transmise au pilote du DSN.  
  
-   Si le gestionnaire de pilotes ne trouve pas la spécification de source de données correspondante et qu’il n’existe aucune spécification de source de données par défaut, il retourne SQL_ERROR avec SQLSTATE IM002 (la source de données est introuvable et aucun pilote par défaut n’est spécifié).  
  
 Si la chaîne de connexion de la requête de navigation contient le mot clé **Driver** , le gestionnaire de pilotes charge le pilote spécifié. Il n’essaie pas de localiser une source de données dans les informations système. Étant donné que le mot clé **Driver** n’utilise pas les informations des informations système, le pilote doit définir suffisamment de mots clés pour qu’un pilote puisse se connecter à une source de données en utilisant uniquement les informations contenues dans les chaînes de connexion de la requête de navigation.  
  
 À chaque appel de **SQLBrowseConnect**, l’application spécifie les valeurs d’attribut de connexion dans la chaîne de connexion de la demande de navigation. Le pilote retourne des niveaux d’attributs et de valeurs d’attribut successifs dans la chaîne de connexion du résultat de navigation. elle retourne SQL_NEED_DATA tant qu’il y a des attributs de connexion qui n’ont pas encore été énumérés dans la chaîne de connexion de la demande de navigation. L’application utilise le contenu de la chaîne de connexion du résultat de navigation pour générer la chaîne de connexion de la demande de navigation pour l’appel suivant à **SQLBrowseConnect**. Tous les attributs obligatoires (ceux qui ne sont pas précédés d’un astérisque dans l’argument *OutConnectionString* ) doivent être inclus dans l’appel suivant à **SQLBrowseConnect**. Notez que l’application ne peut pas utiliser le contenu des chaînes de connexion de résultats de parcours précédentes lors de la génération de la chaîne de connexion de requête de navigation actuelle. autrement dit, il ne peut pas spécifier des valeurs différentes pour les attributs définis aux niveaux précédents.  
  
 Lorsque tous les niveaux de connexion et leurs attributs associés ont été énumérés, le pilote retourne SQL_SUCCESS, la connexion à la source de données est terminée et une chaîne de connexion complète est retournée à l’application. La chaîne de connexion peut être utilisée conjointement avec **SQLDriverConnect**, avec l’option SQL_DRIVER_NOPROMPT pour établir une autre connexion. Toutefois, la chaîne de connexion complète ne peut pas être utilisée dans un autre appel de **SQLBrowseConnect**. Si **SQLBrowseConnect** était appelé à nouveau, toute la séquence d’appels doit être répétée.  
  
 **SQLBrowseConnect** retourne également SQL_NEED_DATA s’il existe des erreurs récupérables et non fatales pendant le processus de navigation ; par exemple, un mot de passe ou un mot clé d’attribut non valide fourni par l’application. Lorsque SQL_NEED_DATA est retourné et que la chaîne de connexion du résultat de navigation n’est pas modifiée, une erreur s’est produite et l’application peut appeler **SQLGetDiagRec** pour renvoyer le SQLSTATE pour les erreurs d’accès au moment de l’exécution. Cela permet à l’application de corriger l’attribut et de poursuivre l’exploration.  
  
 Une application peut arrêter le processus de navigation à tout moment en appelant **SQLDisconnect**. Le pilote met fin à toutes les connexions en suspens et renvoie la connexion à un État non connecté.  
  
 Si les opérations asynchrones sont activées sur le handle de connexion, **SQLBrowseConnect** peut également retourner SQL_STILL_EXECUTING. Lorsqu’il retourne SQL_NEED_DATA, une application doit utiliser **SQLDisconnect** pour annuler le processus de navigation. Si **SQLBrowseConnect** retourne SQL_STILL_EXECUTING, une application doit utiliser **SQLCancelHandle** pour annuler l’opération en cours. L’appel de **SQLCancelHandle** après le retour de la fonction SQL_NEED_DATA n’a aucun effet.  
  
 Pour plus d’informations, consultez [connexion avec SQLBrowseConnect](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md).  
  
 Si un pilote prend en charge **SQLBrowseConnect**, la section du mot clé Driver dans les informations système pour le pilote doit contenir le mot clé **ConnectFunctions** avec le troisième caractère défini sur « Y ».  
  
## <a name="code-example"></a>Exemple de code  
  
> [!NOTE]  
>  Si vous vous connectez à un fournisseur de sources de données qui prend en charge l’authentification Windows, vous devez spécifier à `Trusted_Connection=yes` la place de l’ID d’utilisateur et des informations de mot de passe dans la chaîne de connexion.  
  
 Dans l’exemple suivant, une application appelle **SQLBrowseConnect** à plusieurs reprises. Chaque fois que **SQLBrowseConnect** retourne SQL_NEED_DATA, il transmet des informations sur les données dont il a besoin dans \* *OutConnectionString*. L’application transmet *OutConnectionString* à sa routine **GetUserInput** (non illustrée). **GetUserInput** analyse les informations, crée et affiche une boîte de dialogue et retourne les informations entrées par l’utilisateur dans \* *InConnectionString*. L’application transmet les informations de l’utilisateur au pilote lors du prochain appel à **SQLBrowseConnect**. Une fois que l’application a fourni toutes les informations nécessaires pour que le pilote se connecte à la source de données, **SQLBrowseConnect** retourne SQL_SUCCESS et l’application continue.  
  
 Pour obtenir un exemple plus détaillé de la connexion à un pilote SQL Server en appelant **SQLBrowseConnect**, consultez [SQL Server exemple de navigation](../../../odbc/reference/develop-app/sql-server-browsing-example.md).  
  
 Par exemple, pour vous connecter aux ventes de la source de données, les actions suivantes peuvent se produire. Tout d’abord, l’application transmet la chaîne suivante à **SQLBrowseConnect**:  
  
```  
"DSN=Sales"  
```  
  
 Le gestionnaire de pilotes charge le pilote associé à la source de données Sales. Il appelle ensuite la fonction **SQLBrowseConnect** du pilote avec les mêmes arguments qu’il a reçus de l’application. Le pilote retourne la chaîne suivante dans **OutConnectionString*:  
  
```  
"HOST:Server={red,blue,green};UID:ID=?;PWD:Password=?"  
```  
  
 L’application transmet cette chaîne à sa routine **GetUserInput** , qui génère une boîte de dialogue qui invite l’utilisateur à sélectionner le serveur rouge, bleu ou vert et à entrer un ID d’utilisateur et un mot de passe. La routine transmet les informations spécifiées par l’utilisateur suivantes dans \* *InConnectionString*, que l’application passe à **SQLBrowseConnect**:  
  
```  
"HOST=red;UID=Smith;PWD=Sesame"  
```  
  
 **SQLBrowseConnect** utilise ces informations pour se connecter au serveur Red Server en tant que Smith avec le mot de passe Sesame, puis retourne la chaîne suivante dans **OutConnectionString*:  
  
```  
"*DATABASE:Database={SalesEmployees,SalesGoals,SalesOrders}"  
```  
  
 L’application transmet cette chaîne à sa routine **GetUserInput** , qui génère une boîte de dialogue qui invite l’utilisateur à sélectionner une base de données. L’utilisateur sélectionne EmpData et l’application appelle **SQLBrowseConnect** une dernière fois avec cette chaîne :  
  
```  
"DATABASE=SalesOrders"  
```  
  
 Il s’agit de la dernière information dont le pilote a besoin pour se connecter à la source de données ; **SQLBrowseConnect** retourne SQL_SUCCESS, et **OutConnectionString* contient la chaîne de connexion terminée :  
  
```cpp  
// SQLBrowseConnect_Function.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define BRWS_LEN 100  
SQLHENV henv;  
SQLHDBC hdbc;  
SQLHSTMT hstmt;  
SQLRETURN retcode;  
SQLCHAR szConnStrIn[BRWS_LEN], szConnStrOut[BRWS_LEN];  
SQLSMALLINT cbConnStrOut;  
  
void GetUserInput(SQLCHAR * szConnStrOut, SQLCHAR * szConnStrIn) {}  
  
int main() {  
   // Allocate the environment handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);        
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      // Set the version environment attribute.  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
         // Allocate the connection handle.  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // Call SQLBrowseConnect until it returns a value other than SQL_NEED_DATA   
            // (pass data source name the first time).  If SQL_NEED_DATA is returned, call GetUserInput   
            // (not shown) to build a dialog from the values in szConnStrOut.  The user-supplied values   
            // are returned in szConnStrIn, which is passed in the next call to SQLBrowseConnect.  
  
            strcpy_s((char*)szConnStrIn, _countof(szConnStrIn), "DSN=Sales");  
            do {  
               retcode = SQLBrowseConnect(hdbc, szConnStrIn, SQL_NTS,  
                  szConnStrOut, BRWS_LEN, &cbConnStrOut);  
               if (retcode == SQL_NEED_DATA)  
                  GetUserInput(szConnStrOut, szConnStrIn);  
            } while (retcode == SQL_NEED_DATA);  
  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
               // Allocate the statement handle.  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                  // Process data after successful connection  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               SQLDisconnect(hdbc);  
            }  
         }  
         SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      }  
   }  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un handle de connexion|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Déconnexion d’une source de données|[SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Connexion à une source de données à l’aide d’une chaîne de connexion ou d’une boîte de dialogue|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Renvoi des descriptions et des attributs des pilotes|[SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Libération d’un descripteur de connexion|[Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
