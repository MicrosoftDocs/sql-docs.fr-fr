---
title: Fonction SQLDriverConnect | Microsoft Docs
description: La fonction SQLDriverConnect fait partie de l’API ODBC standard et cette documentation de référence fournit des informations sur sa syntaxe.
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDriverConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDriverConnect
helpviewer_keywords:
- SQLDriverConnect function [ODBC]
ms.assetid: e299be1d-5c74-4ede-b6a3-430eb189134f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58c9f2071825c6c7d8f873f300e8e5c3f46af9b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195634"
---
# <a name="sqldriverconnect-function"></a>Fonction SQLDriverConnect
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLDriverConnect** est une alternative à **SQLConnect**. Il prend en charge des sources de données qui requièrent plus d’informations de connexion que les trois arguments de **SQLConnect**, des boîtes de dialogue pour inviter l’utilisateur à fournir toutes les informations de connexion, ainsi que les sources de données qui ne sont pas définies dans les informations système. Pour plus d’informations, consultez [connexion avec SQLDriverConnect](../develop-app/connecting-with-sqldriverconnect.md).  

## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLDriverConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLHWND         WindowHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr,  
     SQLUSMALLINT    DriverCompletion);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *WindowHandle*  
 Entrée Handle de fenêtre. L’application peut passer le handle de la fenêtre parente, le cas échéant, ou un pointeur null si le handle de fenêtre n’est pas applicable ou si **SQLDriverConnect** ne présente pas de boîtes de dialogue.  
  
 *InConnectionString*  
 Entrée Une chaîne de connexion complète (consultez la syntaxe dans « Comments »), une chaîne de connexion partielle ou une chaîne vide.  
  
 *StringLength1*  
 Entrée Longueur de **InConnectionString*, en caractères si la chaîne est Unicode, ou octets si la chaîne est ANSI ou DBCS.  
  
 *OutConnectionString*  
 Sortie Pointeur vers une mémoire tampon pour la chaîne de connexion terminée. Une fois la connexion établie à la source de données cible, cette mémoire tampon contient la chaîne de connexion terminée. Les applications doivent allouer au moins 1 024 caractères pour cette mémoire tampon.  
  
 Si *OutConnectionString* a la valeur null, *StringLength2Ptr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *OutConnectionString*.  
  
 *BufferLength*  
 Entrée Longueur de la mémoire tampon **OutConnectionString* , en caractères.  
  
 *StringLength2Ptr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans \* *OutConnectionString*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength*, la chaîne de connexion terminée dans \* *OutConnectionString* est tronquée à *BufferLength* moins la longueur d’un caractère de fin null.  
  
 *DriverCompletion*  
 Entrée Indicateur qui spécifie si le gestionnaire de pilotes ou le pilote doit demander des informations supplémentaires sur la connexion :  
  
 SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED ou SQL_DRIVER_NOPROMPT.  
  
 (Pour plus d’informations, consultez « commentaires ».)  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLDriverConnect** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *FHandleType* de SQL_HANDLE_DBC et un *hHandle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLDriverConnect** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le OutConnectionString de mémoire tampon \*  n’est pas assez grand pour retourner la chaîne de connexion entière, donc la chaîne de connexion a été tronquée. La longueur de la chaîne de connexion non tronquée est retournée dans **StringLength2Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S00|Attribut de chaîne de connexion non valide|Un mot clé d’attribut non valide a été spécifié dans la chaîne de connexion (*InConnectionString*), mais le pilote a pu se connecter à la source de données. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée pointée par l’argument *ValuePtr* dans **SQLSetConnectAttr** et substituait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S08|Erreur lors de l’enregistrement du fichier DSN|La chaîne dans *\* InConnectionString* contenait un mot clé **FILEDSN** , mais le fichier. DSN n’a pas été enregistré. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S09|Mot clé non valide|(DM) la chaîne dans *\* InConnectionString* contenait un mot clé **SaveFile** , mais pas un mot clé de **pilote** ou **FILEDSN** . (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08001|Le client ne peut pas établir la connexion|Le pilote n’a pas pu établir une connexion avec la source de données.|  
|08002|Nom de connexion en cours d’utilisation|(DM) le *ConnectionHandle* spécifié a déjà été utilisé pour établir une connexion avec une source de données, et la connexion était toujours ouverte.|  
|08004|Le serveur a rejeté la connexion|La source de données a rejeté l’établissement de la connexion pour des raisons définies par l’implémentation.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote essayait de se connecter a échoué avant la fin du traitement de la fonction **SQLDriverConnect** .|  
|28000|Spécification d’autorisation non valide|Soit l’identificateur d’utilisateur, soit la chaîne d’autorisation, ou les deux, comme spécifié dans la chaîne de connexion (*InConnectionString*), des restrictions non respectées sont définies par la source de données.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* szMessageText* décrit l’erreur et sa cause.|  
|HY000|Erreur générale : nom de source de fichier non valide|(DM) la chaîne dans **InConnectionString* contenait un mot clé FileDSN, mais le nom du fichier. DSN est introuvable.|  
|HY000|Erreur générale : impossible de créer le tampon de fichier|(DM) la chaîne dans **InConnectionString* contenait un mot clé FileDSN, mais le fichier. DSN était illisible.|  
|HY001|Erreur d’allocation de mémoire|Le gestionnaire de pilotes n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou la fin de la fonction **SQLDriverConnect** .<br /><br /> Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *ConnectionHandle*. La fonction a été appelée, et avant la fin de l’exécution, la [fonction SQLCancelHandle](../../../odbc/reference/syntax/sqlcancelhandle-function.md) a été appelée sur le *ConnectionHandle*, puis la fonction **SQLDriverConnect** a été appelée à nouveau sur le *ConnectionHandle*.<br /><br /> Ou bien, la fonction **SQLDriverConnect** a été appelée et avant la fin de l’exécution, **SQLCancelHandle** a été appelé sur le *ConnectionHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) une autre fonction d’exécution asynchrone (non **SQLDriverConnect**) a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution lors de l’appel de la fonction **SQLDriverConnect** .|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction **SQLDriverConnect** , car les objets de mémoire sous-jacents n’ont pas pu être accédés, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *StringLength1* est inférieure à 0 et n’est pas égale à SQL_NTS.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength* est inférieure à 0.|  
|HY092|Identificateur d’attribut/option non valide|(DM) l’argument *DriverCompletion* a été SQL_DRIVER_PROMPT, et l’argument *WindowHandle* était un pointeur null.|  
|HY110|Fin du pilote non valide|(DM) la valeur spécifiée pour l’argument *DriverCompletion* n’est pas égale à SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE, SQL_DRIVER_COMPLETE_REQUIRED ou SQL_DRIVER_NOPROMPT.<br /><br /> Le regroupement de connexions (DM) a été activé, et la valeur spécifiée pour l’argument *DriverCompletion* n’était pas égale à SQL_DRIVER_NOPROMPT.|  
|HYC00|Fonctionnalité facultative non implémentée|Le pilote ne prend pas en charge la version du comportement ODBC demandée par l’application.|  
|HYT00|Délai expiré|Le délai d’expiration de la connexion a expiré avant la fin de la connexion à la source de données. Le délai d’attente est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant au nom de source de données spécifié ne prend pas en charge la fonction.|  
|IM002|Source de données introuvable et aucun pilote par défaut spécifié|(DM) le nom de source de données spécifié dans la chaîne de connexion (*InConnectionString*) est introuvable dans les informations système et il n’y a pas de spécification de pilote par défaut.<br /><br /> La source de données ODBC (DM) et les informations de pilote par défaut sont introuvables dans les informations système.|  
|IM003|Le pilote spécifié n’a pas pu être chargé|(DM) le pilote figurant dans la spécification de la source de données dans les informations système ou spécifié par le mot clé **Driver** est introuvable ou n’a pas pu être chargé pour une raison quelconque.|  
|IM004|Échec du **SQLAllocHandle** du pilote sur SQL_HANDLE_ENV|(DM) au cours de **SQLDriverConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *fHandleType* de SQL_HANDLE_ENV et le pilote a renvoyé une erreur.|  
|IM005|Échec de **SQLAllocHandle** du pilote sur SQL_HANDLE_DBC.|(DM) au cours de **SQLDriverConnect**, le gestionnaire de pilotes a appelé la fonction **SQLAllocHandle** du pilote avec un *fHandleType* de SQL_HANDLE_DBC et le pilote a renvoyé une erreur.|  
|IM006|Échec de **SQLSetConnectAttr** du pilote|(DM) au cours de **SQLDriverConnect**, le gestionnaire de pilotes a appelé la fonction **SQLSetConnectAttr** du pilote et le pilote a renvoyé une erreur.|  
|IM007|Aucune source de données ni aucun pilote spécifiés ; dialogue interdit|Aucun nom ou pilote de source de données n’a été spécifié dans la chaîne de connexion, et *DriverCompletion* a été SQL_DRIVER_NOPROMPT.|  
|IM008|Échec de la boîte de dialogue|Le pilote a tenté d’afficher sa boîte de dialogue de connexion et a échoué.<br /><br /> *WindowHandle* était un pointeur null et *DriverCompletion* n’était pas SQL_DRIVER_NO_PROMPT.|  
|IM009|Impossible de charger la DLL de traduction|Le pilote n’a pas pu charger la DLL de traduction spécifiée pour la source de données ou pour la connexion.|  
|IM010|Le nom de la source de données est trop long|(DM) la valeur de l’attribut du mot clé DSN était supérieure à SQL_MAX_DSN_LENGTH caractères.|  
|IM011|Le nom du pilote est trop long|(DM) la valeur de l’attribut du mot clé **Driver** était supérieure à 255 caractères.|  
|IM012|Erreur de syntaxe du mot clé du pilote|(DM) la paire mot clé-valeur pour le mot clé **Driver** contenait une erreur de syntaxe.<br /><br /> (DM) la chaîne contenue dans *\* InConnectionString* contenait un mot clé **FILEDSN** , mais le fichier. DSN ne contenait pas de mot clé de **pilote** ou de mot clé **DSN** .|  
|IM014|Le nom de source de donnée spécifié contient une incompatibilité d’architecture entre le pilote et l’application|(DM) l’application 32 bits utilise un DSN se connectant à un pilote 64 bits ; ou vice versa.|  
|IM015|Échec du SQLDriverConnect du pilote sur SQL_HANDLE_DBC_INFO_HANDLE|Si un pilote retourne SQL_ERROR, le gestionnaire de pilotes renverra SQL_ERROR à l’application et la connexion échouera.<br /><br /> Pour plus d’informations sur la SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement de la sensibilisation aux Connection-Pool dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
|S1118|Le pilote ne prend pas en charge la notification asynchrone|Si le pilote ne prend pas en charge les notifications asynchrones, vous ne pouvez pas définir SQL_ATTR_ASYNC_DBC_EVENT ou SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="comments"></a>Commentaires  
 Une chaîne de connexion a la syntaxe suivante :  
  
 *Connection-String* :: = *Empty-String*[;] &#124; *attribut*[;] &#124; *attribut*; *chaîne de connexion*  
  
 *Empty-String* :: =*attribute* :: = *attribute-Keyword* = *attribute-value* &#124; Driver = [{]*attribute-value*[}]  
  
 *attribute-Keyword* :: = DSN &#124; UID &#124; pwd &#124; *attribute-defined-Keyword*  
  
 *attribute-value* :: = *chaîne de caractères*  
  
 *Driver-Defined-Attribute-Keyword* :: = *identifier*  
  
 où *Character-String* comporte zéro caractère ou plus ; l' *identificateur* comporte un ou plusieurs caractères ; *attribute-Keyword* ne respecte pas la casse ; *attribute-value* peut être sensible à la casse ; et la valeur du mot clé **DSN** ne se compose pas uniquement d’espaces blancs.  
  
 En raison de la grammaire des fichiers de chaîne de connexion et d’initialisation, les mots clés et les valeurs d’attribut qui contiennent les caractères **[] {} (),;? \* = ! @** les accolades ne doivent pas être mises entre accolades. La valeur du mot clé **DSN** ne peut pas se composer uniquement d’espaces blancs et ne doit pas contenir d’espaces à gauche. En raison de la grammaire des informations système, les mots clés et les noms de sources de données ne peuvent pas contenir de barre oblique inverse ( \\ ).  
  
 Les applications n’ont pas besoin d’ajouter des accolades autour de la valeur d’attribut après le mot clé **Driver** , à moins que l’attribut contienne un point-virgule (;), auquel cas les accolades sont requises. Si la valeur d’attribut que le pilote reçoit comprend des accolades, le pilote ne doit pas les supprimer, mais il doit faire partie de la chaîne de connexion retournée.  
  
 Une valeur de chaîne de DSN ou de connexion placée entre accolades ( {} ) contenant l’un des caractères **[] {} (),;? \* = ! @** est passé intact au pilote. Toutefois, lorsque vous utilisez ces caractères dans un mot clé, le gestionnaire de pilotes retourne une erreur lors de l’utilisation de fichiers DSN, mais transmet la chaîne de connexion au pilote pour les chaînes de connexion standard. Évitez d’utiliser des accolades incorporées dans une valeur de mot clé.  
  
 La chaîne de connexion peut inclure un nombre quelconque de mots clés définis par le pilote. Étant donné que le mot clé **Driver** n’utilise pas les informations des informations système, le pilote doit définir suffisamment de mots clés pour qu’un pilote puisse se connecter à une source de données en utilisant uniquement les informations contenues dans la chaîne de connexion. (Pour plus d’informations, consultez « Instructions relatives aux pilotes », plus loin dans cette section.) Le pilote définit les mots clés requis pour la connexion à la source de données.  
  
 Le tableau suivant décrit les valeurs d’attribut des mots clés **DSN**, **FILEDSN**, **Driver**, **UID**, **PWD** et **SaveFile** .  
  
|Mot clé|Description de la valeur d’attribut|  
|-------------|---------------------------------|  
|**DSN**|Nom d’une source de données telle qu’elle est retournée par **SQLDataSources** ou la boîte de dialogue sources de données de **SQLDriverConnect**.|  
|**FILEDSN**|Nom d’un fichier. DSN à partir duquel une chaîne de connexion sera générée pour la source de données. Ces sources de données sont appelées sources de données de fichier.|  
|**PILOTE**|Description du pilote telle qu’elle est retournée par la fonction **SQLDrivers** . Par exemple, RDB ou SQL Server.|  
|**UID**|ID utilisateur.|  
|**PWD**|Le mot de passe correspondant à l’ID d’utilisateur, ou une chaîne vide s’il n’existe aucun mot de passe pour l’ID d’utilisateur (PWD =;).|  
|**SAVEFILE**|Nom de fichier d’un fichier. DSN dans lequel les valeurs d’attribut des mots clés utilisés pour effectuer la connexion actuelle doivent être enregistrées.|  
  
 Pour plus d’informations sur la façon dont une application choisit une source de données ou un pilote, consultez [choix d’une source de données ou](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md)d’un pilote.  
  
 Si des mots clés sont répétés dans la chaîne de connexion, le pilote utilise la valeur associée à la première occurrence du mot clé. Si les mots clés du **DSN** et du **pilote** sont inclus dans la même chaîne de connexion, le gestionnaire de pilotes et le pilote utilisent le mot clé qui apparaît en premier.  
  
 Les mots clés **FILEDSN** et **DSN** s’excluent mutuellement : quel que soit le mot clé qui apparaît en premier est utilisé, et celui qui est le second est ignoré. Les mots clés de l' **FILEDSN** et du **pilote** , en revanche, ne s’excluent pas mutuellement. Si un mot clé apparaît dans une chaîne de connexion avec **FILEDSN**, la valeur d’attribut du mot clé dans la chaîne de connexion est utilisée à la place de la valeur d’attribut du même mot clé dans le fichier. DSN.  
  
 Si le mot clé **FILEDSN** est utilisé, les mots clés spécifiés dans un fichier. DSN sont utilisés pour créer une chaîne de connexion. (Pour plus d’informations, consultez « sources de données de fichier » plus loin dans cette section.) Le mot clé **UID** est facultatif. un fichier. DSN peut être créé avec uniquement le mot clé **Driver** . Le mot clé **PWD** n’est pas stocké dans un fichier. DSN. Le répertoire par défaut pour l’enregistrement et le chargement d’un fichier. DSN est une combinaison du chemin d’accès spécifié par **CommonFileDir** dans HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ Windows\CurrentVersion et « ODBC\DataSources ». (Si CommonFileDir était « C:\Program Files\Common Files », le répertoire par défaut serait « C:\Program Files\Common Files\ODBC\Data Sources ».)  
  
> [!NOTE]  
>  Un fichier. DSN peut être manipulé directement en appelant les fonctions [SQLReadFileDSN](../../../odbc/reference/syntax/sqlreadfiledsn-function.md) et [SQLWRITEFILEDSN](../../../odbc/reference/syntax/sqlwritefiledsn-function.md) dans la dll du programme d’installation.  
  
 Si le mot clé **SaveFile** est utilisé, les valeurs d’attribut des mots clés utilisés pour effectuer la connexion actuelle et réussie seront enregistrées en tant que fichier. DSN avec le nom de la valeur d’attribut du mot clé **SaveFile** . Le mot clé **SaveFile** doit être utilisé conjointement avec le mot clé **Driver** , le mot clé **FILEDSN** , ou les deux, ou la fonction retourne SQL_SUCCESS_WITH_INFO avec SQLSTATE 01S09 (mot clé non valide). Le mot clé **SaveFile** doit apparaître avant le mot clé **Driver** dans la chaîne de connexion, sinon les résultats ne sont pas définis.  
  
## <a name="driver-manager-guidelines"></a>Instructions du gestionnaire de pilotes  
 Le gestionnaire de pilotes construit une chaîne de connexion à transmettre au pilote dans l’argument *InConnectionString* de la fonction **SQLDriverConnect** du pilote. Le gestionnaire de pilotes ne modifie pas l’argument *InConnectionString* qui lui est transmis par l’application.  
  
 L’action du gestionnaire de pilotes est basée sur la valeur de l’argument *DriverCompletion* :  
  
-   SQL_DRIVER_PROMPT : si la chaîne de connexion ne contient pas le mot clé **Driver**, **DSN** ou **FILEDSN** , le gestionnaire de pilotes affiche la boîte de dialogue sources de données. Il construit une chaîne de connexion à partir du nom de source de données retourné par la boîte de dialogue et de tout autre mot clé qui lui est transmis par l’application. Si le nom de la source de données retourné par la boîte de dialogue est vide, le gestionnaire de pilotes spécifie la paire mot clé/valeur DSN = default. (Cette boîte de dialogue n’affiche pas de source de données avec le nom « default ».)  
  
-   SQL_DRIVER_COMPLETE ou SQL_DRIVER_COMPLETE_REQUIRED : si la chaîne de connexion spécifiée par l’application contient le mot clé **DSN** , le gestionnaire de pilotes copie la chaîne de connexion spécifiée par l’application. Dans le cas contraire, elle effectue les mêmes actions que quand *DriverCompletion* est SQL_DRIVER_PROMPT.  
  
-   SQL_DRIVER_NOPROMPT : le gestionnaire de pilotes copie la chaîne de connexion spécifiée par l’application.  
  
 Si la chaîne de connexion spécifiée par l’application contient le mot clé **Driver** , le gestionnaire de pilotes copie la chaîne de connexion spécifiée par l’application.  
  
 À l’aide de la chaîne de connexion qu’il a construite, le gestionnaire de pilotes détermine le pilote à utiliser, se connecte à ce pilote et transmet la chaîne de connexion qu’il a construite au pilote. Pour plus d’informations sur l’interaction du gestionnaire de pilotes et du pilote, consultez la section « commentaires » dans la [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Si la chaîne de connexion ne contient pas le mot clé **Driver** , le gestionnaire de pilotes détermine le pilote à utiliser comme suit :  
  
1.  Si la chaîne de connexion contient le mot clé **DSN** , le gestionnaire de pilotes récupère le pilote associé à la source de données à partir des informations système.  
  
2.  Si la chaîne de connexion ne contient pas le mot clé **DSN** ou si la source de données est introuvable, le gestionnaire de pilotes récupère le pilote associé à la source de données par défaut à partir des informations système. (Pour plus d’informations, consultez [sous-clé par défaut](../../../odbc/reference/install/default-subkey.md).) Le gestionnaire de pilotes remplace la valeur du mot clé **DSN** dans la chaîne de connexion par « default ».  
  
3.  Si le mot clé **DSN** dans la chaîne de connexion est défini sur « Default », le gestionnaire de pilotes récupère le pilote associé à la source de données par défaut à partir des informations système.  
  
4.  Si la source de données est introuvable et que la source de données par défaut est introuvable, le gestionnaire de pilotes retourne SQL_ERROR avec SQLSTATE IM002 (la source de données est introuvable et aucun pilote par défaut n’est spécifié).  
  
## <a name="file-data-sources"></a>Sources de données de fichier  
 Si la chaîne de connexion spécifiée par l’application dans l’appel à **SQLDriverConnect** contient le mot clé **FILEDSN** et que ce mot clé n’est pas remplacé par le mot clé **DSN** ou **Driver** , le gestionnaire de pilotes crée une chaîne de connexion à l’aide des informations contenues dans le fichier. DSN et l’argument *InConnectionString* . Le gestionnaire de pilotes se déroule comme suit :  
  
1.  Vérifie si le nom de fichier du fichier. DSN est valide. Si ce n’est pas le cas, elle retourne SQL_ERROR avec SQLSTATE IM014 (nom du fichier DSN non valide). Si le nom de fichier est une chaîne vide ("") et que SQL_DRIVER_NOPROMPT n’est pas spécifié, la boîte **de dialogue Ouvrir un fichier** s’affiche. Si le nom de fichier contient un chemin d’accès valide, mais qu’il n’existe pas de nom de fichier ou de nom de fichier non valide, et que SQL_DRIVER_NOPROMPT n’est pas spécifié, la boîte de dialogue **ouvrir un fichier** s’affiche avec le répertoire en cours défini sur celui spécifié dans le nom de fichier. Si le nom de fichier est une chaîne vide ("") ou si le nom de fichier contient un chemin d’accès valide, mais qu’il n’existe pas de nom de fichier ou de nom de fichier non valide, et que SQL_DRIVER_NOPROMPT est spécifié, SQL_ERROR est retourné avec SQLSTATE IM014 (nom de source de fichier non valide).  
  
2.  Lit tous les mots clés dans la section [ODBC] du fichier. DSN. Si le mot clé **Driver** n’est pas présent, il retourne SQL_ERROR avec SQLSTATE IM012 (erreur de syntaxe du mot clé du pilote), sauf si le fichier. DSN n’est pas partageable et contient donc uniquement le mot clé **DSN** .  
  
     Si la source de données du fichier ne peut pas être partagée, le gestionnaire de pilotes lit la valeur du mot clé **DSN** et se connecte en fonction des besoins à la source de données utilisateur ou système désignée par la source de données du fichier non partageable. Les étapes 3 à 5 ne sont pas effectuées.  
  
3.  Construit une chaîne de connexion pour le pilote. La chaîne de connexion du pilote est l’Union des mots clés spécifiés dans le fichier. DSN et ceux spécifiés dans la chaîne de connexion d’application d’origine. Les règles de construction de la chaîne de connexion du pilote dont les mots clés se chevauchent sont les suivantes :  
  
    -   Si le mot clé **Driver** existe dans la chaîne de connexion de l’application et que les pilotes spécifiés par les mots clés du **pilote** ne sont pas identiques dans le fichier. DSN et la chaîne de connexion de l’application, les informations du pilote dans le fichier. DSN sont ignorées et les informations du pilote dans la chaîne de connexion de l’application sont utilisées. Si les pilotes spécifiés par le mot clé **Driver** sont identiques dans le fichier. DSN et dans la chaîne de connexion de l’application, alors que tous les mots clés se chevauchent, ceux spécifiés dans la chaîne de connexion de l’application ont la priorité sur ceux spécifiés dans le fichier. DSN.  
  
    -   Dans la nouvelle chaîne de connexion, le mot clé **FILEDSN** est éliminé.  
  
4.  Charge le pilote en recherchant dans l’entrée de Registre HKEY_LOCAL_MACHINE\SOFTWARE\ODBC\ODBCINST.INI\\<nom du pilote \> \Driver. où \<Driver Name> est spécifié par le mot clé **Driver** .  
  
5.  Transmet au pilote la nouvelle chaîne de connexion.  
  
 Pour obtenir des exemples de fichiers. DSN, consultez [connexion à l’aide de sources de données de fichiers](../../../odbc/reference/develop-app/connecting-using-file-data-sources.md).  
  
## <a name="savefile-keyword"></a>SAVEFILE (mot clé)  
 Si la chaîne de connexion spécifiée par l’application contient le mot clé **SaveFile** , le gestionnaire de pilotes enregistre la chaîne de connexion dans un fichier. DSN. Le gestionnaire de pilotes se déroule comme suit :  
  
1.  Vérifie si le nom de fichier du fichier. DSN inclus comme valeur d’attribut du mot clé **SaveFile** est valide. Si ce n’est pas le cas, elle retourne SQL_ERROR avec SQLSTATE IM014 (nom du fichier DSN non valide). La validité du nom de fichier est déterminée par les règles d’affectation de noms système standard. Si le nom de fichier est une chaîne vide ("") et que l’argument *DriverCompletion* n’est pas SQL_DRIVER_NOPROMPT, le nom de fichier est valide. Si le nom de fichier existe déjà, si *DriverCompletion* est SQL_DRIVER_NOPROMPT, le fichier est remplacé. Si *DriverCompletion* est SQL_DRIVER_PROMPT, SQL_DRIVER_COMPLETE ou SQL_DRIVER_COMPLETE_REQUIRED, une boîte de dialogue invite l’utilisateur à spécifier si le fichier doit être remplacé. Si aucun n’est entré, la boîte de dialogue **enregistrement de fichier** s’affiche.  
  
2.  Si le pilote retourne SQL_SUCCESS et que le nom de fichier n’est pas une chaîne vide, le gestionnaire de pilotes écrit les informations de connexion retournées dans l’argument *OutConnectionString* dans le fichier spécifié au format spécifié dans la section « chaînes de connexion », plus haut dans cette section.  
  
3.  Si le pilote retourne SQL_SUCCESS et que le nom de fichier était une chaîne vide (« »), le gestionnaire de pilotes appelle la boîte de dialogue courante d' **enregistrement de fichier** avec le *HWND* spécifié et écrit les informations de connexion retournées dans *OutConnectionString* dans le fichier spécifié dans la boîte de dialogue File-Save commune avec le format spécifié dans la section «chaînes de connexion » plus haut dans cette section.  
  
4.  Si le pilote retourne SQL_SUCCESS, il retourne l’argument *OutConnectionString* contenant la chaîne de connexion à l’application.  
  
5.  Si le pilote retourne SQL_SUCCESS_WITH_INFO ou SQL_ERROR, le gestionnaire de pilotes renvoie la valeur SQLSTATE à l’application.  
  
## <a name="driver-guidelines"></a>Instructions relatives aux pilotes  
 Le pilote vérifie si la chaîne de connexion qui lui est transmise par le gestionnaire de pilotes contient le mot clé **DSN** ou **Driver** . Si la chaîne de connexion contient le mot clé **Driver** , le pilote ne peut pas récupérer les informations relatives à la source de données à partir des informations système. Si la chaîne de connexion contient le mot clé **DSN** ou ne contient pas le mot clé **DSN** ou **Driver** , le pilote peut récupérer des informations sur la source de données à partir des informations système comme suit :  
  
1.  Si la chaîne de connexion contient le mot clé **DSN** , le pilote récupère les informations pour la source de données spécifiée.  
  
2.  Si la chaîne de connexion ne contient pas le mot clé **DSN** , si la source de données spécifiée est introuvable ou si le mot clé **DSN** est défini sur « Default », le pilote récupère les informations de la source de données par défaut.  
  
 Le pilote utilise toutes les informations récupérées à partir des informations système pour augmenter les informations qui lui sont passées dans la chaîne de connexion. Si les informations du système dupliquent les informations dans la chaîne de connexion, le pilote utilise les informations de la chaîne de connexion.  
  
 En fonction de la valeur de *DriverCompletion*, le pilote invite l’utilisateur à fournir des informations de connexion, telles que l’ID d’utilisateur et le mot de passe, et se connecte à la source de données :  
  
-   SQL_DRIVER_PROMPT : le pilote affiche une boîte de dialogue, en utilisant les valeurs de la chaîne de connexion et les informations système (le cas échéant) comme valeurs initiales. Lorsque l’utilisateur quitte la boîte de dialogue, le pilote se connecte à la source de données. Il construit également une chaîne de connexion à partir de la valeur du mot clé **DSN** ou **Driver** dans \* *InConnectionString* , ainsi que les informations retournées par la boîte de dialogue. Il place cette chaîne de connexion dans la mémoire tampon **OutConnectionString* .  
  
-   SQL_DRIVER_COMPLETE ou SQL_DRIVER_COMPLETE_REQUIRED : si la chaîne de connexion contient suffisamment d’informations et que ces informations sont correctes, le pilote se connecte à la source de données et copie \* *InConnectionString* vers \* *OutConnectionString*. Si des informations sont manquantes ou incorrectes, le pilote effectue les mêmes actions que lorsque *DriverCompletion* est SQL_DRIVER_PROMPT, sauf que si *DriverCompletion* est SQL_DRIVER_COMPLETE_REQUIRED, le pilote désactive les contrôles pour toute information non requise pour la connexion à la source de données.  
  
-   SQL_DRIVER_NOPROMPT : si la chaîne de connexion contient suffisamment d’informations, le pilote se connecte à la source de données et copie \* *InConnectionString* vers \* *OutConnectionString*. Dans le cas contraire, le pilote retourne SQL_ERROR pour **SQLDriverConnect**.  
  
 En cas de connexion réussie à la source de données, le pilote définit également \* *StringLength2Ptr* sur la longueur de la chaîne de connexion de sortie qui est disponible pour retourner dans **OutConnectionString*.  
  
 Si l’utilisateur annule une boîte de dialogue présentée par le gestionnaire de pilotes ou le pilote, **SQLDriverConnect** retourne SQL_NO_DATA.  
  
 Pour plus d’informations sur la façon dont le gestionnaire de pilotes et le pilote interagissent au cours du processus de connexion, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Si un pilote prend en charge **SQLDriverConnect**, la section du mot clé Driver des informations système pour le pilote doit contenir le mot clé **ConnectFunctions** avec le deuxième caractère défini sur « Y ».  
  
## <a name="connecting-when-connection-pooling-is-enabled"></a>Connexion lorsque le regroupement de connexions est activé  
 Le regroupement de connexions permet à une application de réutiliser une connexion qui a déjà été créée. Quand **SQLDriverConnect** est appelé, le gestionnaire de pilotes essaie d’établir la connexion à l’aide d’une connexion qui fait partie d’un pool de connexions dans un environnement désigné pour le regroupement de connexions. Pour plus d’informations sur le regroupement de connexions, consultez [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md).  
  
 Une application peut définir SQL_ATTR_RESET_CONNECTION avant d’appeler SQLDisconnect sur une connexion où le regroupement est activé. Pour plus d’informations, consultez [fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md).  
  
 Les restrictions suivantes s’appliquent lorsqu’une application appelle **SQLDriverConnect** pour se connecter à une connexion regroupée :  
  
-   Aucun traitement de regroupement de connexions n’est effectué lorsque le mot clé **SaveFile** est spécifié dans la chaîne de connexion.  
  
-   Si le regroupement de connexions est activé, **SQLDriverConnect** ne peut être appelé qu’avec un argument *DriverCompletion* de SQL_DRIVER_NOPROMPT ; Si **SQLDriverConnect** est appelé avec n’importe quel autre *DriverCompletion*, la fonction SQLSTATE HY110 (saisie semi-automatique du pilote non valide) est retournée.  
  
## <a name="connection-attributes"></a>Attributs de connexion  
 L’attribut de connexion SQL_ATTR_LOGIN_TIMEOUT, défini à l’aide de **SQLSetConnectAttr**, définit le nombre de secondes d’attente pour qu’une demande de connexion se termine avec une connexion réussie par le pilote avant de retourner à l’application. Si l’utilisateur est invité à terminer la chaîne de connexion, une période d’attente pour chaque demande de connexion commence lorsque le pilote démarre le processus de connexion.  
  
 Le pilote ouvre la connexion en mode d’accès SQL_MODE_READ_WRITE par défaut. Pour définir le mode d’accès à SQL_MODE_READ_ONLY, l’application doit appeler **SQLSetConnectAttr** avec l’attribut SQL_ATTR_ACCESS_MODE avant d’appeler **SQLDriverConnect**.  
  
 Si une bibliothèque de traduction par défaut est spécifiée dans les informations système de la source de données, le pilote la charge. Une autre bibliothèque de traduction peut être chargée en appelant **SQLSetConnectAttr** avec l’attribut SQL_ATTR_TRANSLATE_LIB. Vous pouvez spécifier une option de traduction en appelant **SQLSetConnectAttr** avec l’option SQL_ATTR_TRANSLATE_OPTION.  
  
 Pour plus d’informations, consultez [connexion avec SQLDriverConnect](../../../odbc/reference/develop-app/connecting-with-sqldriverconnect.md).  
  
```cpp  
// SQLDriverConnect_ref.cpp  
// compile with: odbc32.lib user32.lib  
#include <windows.h>  
#include <sqlext.h>  
  
int main() {  
   SQLHENV henv;  
   SQLHDBC hdbc;  
   SQLHSTMT hstmt;  
   SQLRETURN retcode;  
  
   SQLCHAR OutConnStr[255];  
   SQLSMALLINT OutConnStrLen;  
  
   HWND desktopHandle = GetDesktopWindow();   // desktop's window handle  
  
   // Allocate environment handle  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);  
  
   // Set the ODBC version environment attribute  
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);   
  
      // Allocate connection handle  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);   
  
         // Set login timeout to 5 seconds  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            SQLSetConnectAttr(hdbc, SQL_LOGIN_TIMEOUT, (SQLPOINTER)5, 0);  
  
            retcode = SQLDriverConnect( // SQL_NULL_HDBC  
               hdbc,   
               desktopHandle,   
               (SQLCHAR*)"driver=SQL Server",   
               _countof("driver=SQL Server"),  
               OutConnStr,  
               255,   
               &OutConnStrLen,  
               SQL_DRIVER_PROMPT );  
  
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
  
 Consultez également [exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Découverte et énumération des valeurs requises pour se connecter à une source de données|[Fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Déconnexion d’une source de données|[SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Renvoi des descriptions et des attributs des pilotes|[SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|Libération d’un descripteur|[Fonction SQLFreeHandle](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
