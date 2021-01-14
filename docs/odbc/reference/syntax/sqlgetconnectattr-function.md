---
description: Fonction SQLGetConnectAttr
title: SQLGetConnectAttr fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetConnectOption
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetConnectAttr
helpviewer_keywords:
- SQLGetConnectAttr function [ODBC]
ms.assetid: 2cb4ffa8-19d3-4664-8c2f-6682cdcc3f33
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 5da83146d0c6167a08f36a648749470830795703
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98171861"
---
# <a name="sqlgetconnectattr-function"></a>Fonction SQLGetConnectAttr
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetConnectAttr** retourne le paramètre actuel d’un attribut de connexion.  
  
> [!NOTE]
>  Pour plus d’informations sur le mappage de cette fonction par le gestionnaire de pilotes lorsqu’une application ODBC 3 *. x* utilise un pilote ODBC 2 *. x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetConnectAttr(  
     SQLHDBC        ConnectionHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *Attribut*  
 Entrée Attribut à récupérer.  
  
 *ValuePtr*  
 Sortie Pointeur vers la mémoire dans lequel retourner la valeur actuelle de l’attribut spécifié par l' *attribut*. Pour les attributs de type entier, certains pilotes peuvent uniquement écrire le plus petit 32-bit ou 16 bits d’une mémoire tampon et ne pas modifier le bit d’ordre supérieur. Par conséquent, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur à 0 avant d’appeler cette fonction.  
  
 Si *ValuePtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *ValuePtr*.  
  
 *BufferLength*  
 Entrée Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de \* *ValuePtr*. Si l' *attribut* est un attribut défini par ODBC et que *\* ValuePtr* est un entier, *BufferLength* est ignoré. Si la valeur de *\* ValuePtr* est une chaîne Unicode (lors de l’appel de **SQLGetConnectAttrW**), l’argument *BufferLength* doit être un nombre pair.  
  
 Si l' *attribut* est un attribut défini par le pilote, l’application indique la nature de l’attribut au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *\* ValuePtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la chaîne.  
  
-   Si *\* ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *\* ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contient un type de données de longueur fixe, *BufferLength* est soit SQL_IS_INTEGER, soit SQL_IS_UINTEGER, selon le cas.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exclusion du caractère de fin null) pouvant être retourné dans \* *ValuePtr*. Si la valeur de l’attribut est une chaîne de caractères et que le nombre d’octets disponibles à retourner est supérieur à *BufferLength* moins la longueur du caractère de fin null, les données de *\* ValuePtr* sont tronquées à *BufferLength* moins la longueur du caractère de fin null et le pilote se termine par un caractère null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetConnectAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue à partir de la structure de données de diagnostic en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetConnectAttr** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données retournées dans \* *ValuePtr* ont été tronquées pour être *BufferLength* moins la longueur d’un caractère de fin null. La longueur de la valeur de chaîne non tronquée est retournée dans *\* StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08003|Connexion non ouverte|(DM) une valeur d' *attribut* nécessitant une connexion ouverte a été spécifiée.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur renvoyé par la structure de données de diagnostic par l’argument *MessageText* dans **SQLGetDiagField** décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLBrowseConnect** a été appelé pour *ConnectionHandle* et a retourné SQL_NEED_DATA. Cette fonction a été appelée avant que **SQLBrowseConnect** retourne SQL_SUCCESS_WITH_INFO ou SQL_SUCCESS.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *ConnectionHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) *\* ValuePtr* est une chaîne de caractères et BufferLength est inférieur à zéro, mais n’est pas égal à SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l' *attribut* argument n’est pas valide pour la version de ODBC prise en charge par le pilote.|  
|HY114|Le pilote ne prend pas en charge l’exécution de fonctions asynchrones au niveau de la connexion|(DM) une application a tenté d’activer l’exécution de fonctions asynchrones avec SQL_ATTR_ASYNC_DBC_FUNCTIONS_ENABLE pour un pilote qui ne prend pas en charge les opérations de connexion asynchrone.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l' *attribut* argument était un attribut de connexion ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote qui correspond au *ConnectionHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir des informations générales sur les attributs de connexion, consultez [attributs de connexion](../../../odbc/reference/develop-app/connection-attributes.md).  
  
 Pour obtenir la liste des attributs qui peuvent être définis, consultez [SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md). Notez que si *attribute* spécifie un attribut qui retourne une chaîne, *ValuePtr* doit être un pointeur vers une mémoire tampon pour la chaîne. La longueur maximale de la chaîne retournée, y compris le caractère de fin null, sera *BufferLength* octets.  
  
 Selon l’attribut, une application n’a pas besoin d’établir une connexion avant d’appeler **SQLGetConnectAttr**. Toutefois, si **SQLGetConnectAttr** est appelé et que l’attribut spécifié n’a pas de valeur par défaut et qu’il n’a pas été défini par un appel antérieur à **SQLSetConnectAttr**, **SQLGetConnectAttr** retourne SQL_NO_DATA.  
  
 Si l' *attribut* est SQL_ATTR_ TRACE ou SQL_ATTR_ TRACEFILE, *ConnectionHandle* ne doit pas être valide et **SQLGetConnectAttr** ne retourne pas SQL_ERROR ou SQL_INVALID_HANDLE si *ConnectionHandle* n’est pas valide. Ces attributs s’appliquent à toutes les connexions. **SQLGetConnectAttr** retourne SQL_ERROR ou SQL_INVALID_HANDLE si un autre argument n’est pas valide.  
  
 Bien qu’une application puisse définir des attributs d’instruction à l’aide de **SQLSetConnectAttr**, une application ne peut pas utiliser **SQLGetConnectAttr** pour récupérer des valeurs d’attribut d’instruction ; elle doit appeler **SQLGetStmtAttr** pour récupérer le paramètre des attributs d’instruction.  
  
 Les attributs de connexion SQL_ATTR_AUTO_IPD et SQL_ATTR_CONNECTION_DEAD peuvent être retournés par un appel à **SQLGetConnectAttr** , mais ils ne peuvent pas être définis par un appel à **SQLSetConnectAttr**.  
  
> [!NOTE]  
>  Il n’existe aucune prise en charge asynchrone pour **SQLGetConnectAttr**. Lors de l’implémentation de **SQLGetConnectAttr**, un pilote peut améliorer les performances en minimisant le nombre de fois où les informations sont envoyées ou demandées à partir du serveur.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi du paramètre d’un attribut d’instruction|[Fonction SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’environnement|[Fonction SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
