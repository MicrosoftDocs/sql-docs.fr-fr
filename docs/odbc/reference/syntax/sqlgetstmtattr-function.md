---
description: Fonction SQLGetStmtAttr
title: SQLGetStmtAttr fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
apiname:
- SQLGetStmtAttr
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetStmtAttr
helpviewer_keywords:
- SQLGetStmtAttr function [ODBC]
ms.assetid: e321d460-e997-4527-aee6-207cf5a498e9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6af1813068b51780c9181f3e3e98559769495fe
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/13/2021
ms.locfileid: "98172371"
---
# <a name="sqlgetstmtattr-function"></a>Fonction SQLGetStmtAttr
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetStmtAttr** retourne le paramètre actuel d’un attribut d’instruction.  
  
> [!NOTE]  
>  Pour plus d’informations sur la façon dont le gestionnaire de pilotes mappe cette fonction quand ODBC 3. l’application *x* fonctionne avec ODBC 2. *x* , consultez [mappage des fonctions de remplacement pour la compatibilité descendante des applications](../../../odbc/reference/develop-app/mapping-replacement-functions-for-backward-compatibility-of-applications.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetStmtAttr(  
     SQLHSTMT        StatementHandle,  
     SQLINTEGER      Attribute,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *Attribut*  
 Entrée Attribut à récupérer.  
  
 *ValuePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur de l’attribut spécifié dans *attribute*.  
  
 Si *ValuePtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *ValuePtr*.  
  
 *BufferLength*  
 Entrée Si l' *attribut* est un attribut défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de \* *ValuePtr*. Si l' *attribut* est un attribut défini par ODBC et que \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si la valeur retournée dans *\* ValuePtr* est une chaîne Unicode (lors de l’appel de **SQLGetStmtAttrW**), l’argument *BufferLength* doit être un nombre pair.  
  
 Si l' *attribut* est un attribut défini par le pilote, l’application indique la nature de l’attribut au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *\* ValuePtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *\* ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *\* ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contient un type de données de longueur fixe, *BufferLength* est soit SQL_IS_INTEGER, soit SQL_IS_UINTEGER, selon le cas.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exclusion du caractère de fin null) pouvant être retourné dans *\* ValuePtr*. Si la valeur de l’attribut est une chaîne de caractères et que le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength*, les données de *\* ValuePtr* sont tronquées à *BufferLength* moins la longueur d’un caractère de fin null et le pilote se termine par un caractère null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetStmtAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetStmtAttr** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données retournées dans *\* ValuePtr* ont été tronquées pour être *BufferLength* moins la longueur d’un caractère de fin null. La longueur de la valeur de chaîne non tronquée est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|24 000|État de curseur non valide|L' *attribut* d’argument a été SQL_ATTR_ROW_NUMBER et le curseur n’était pas ouvert, ou le curseur était positionné avant le début du jeu de résultats ou après la fin du jeu de résultats.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans l’argument *MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLGetStmtAttr** .<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|*(DM) \* ValuePtr* est une chaîne de caractères et BufferLength était inférieur à zéro, mais n’est pas égal à SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l' *attribut* argument n’est pas valide pour la version de ODBC prise en charge par le pilote.|  
|HY109|Position de curseur non valide|L’argument d' *attribut* a été SQL_ATTR_ROW_NUMBER et la ligne a été supprimée ou n’a pas pu être extraite.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l' *attribut* argument était un attribut d’instruction ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote.|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à *StatementHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir des informations générales sur les attributs d’instruction, consultez [attributs d’instruction](../../../odbc/reference/develop-app/statement-attributes.md).  
  
 Un appel à **SQLGetStmtAttr** retourne dans *\* ValuePtr* la valeur de l’attribut d’instruction spécifié dans *attribute*. Cette valeur peut être une valeur SQLULEN ou une chaîne de caractères se terminant par un caractère null. Si la valeur est une valeur SQLULEN, certains pilotes peuvent uniquement écrire le plus petit 32-bit ou 16 bits d’une mémoire tampon et ne pas modifier le bit d’ordre supérieur. Par conséquent, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur à 0 avant d’appeler cette fonction. En outre, les arguments *BufferLength* et *StringLengthPtr* ne sont pas utilisés. Si la valeur est une chaîne terminée par le caractère null, l’application spécifie la longueur maximale de cette chaîne dans l’argument *BufferLength* et le pilote retourne la longueur de cette chaîne dans la mémoire tampon *\* StringLengthPtr* .  
  
 Pour permettre aux applications qui appellent **SQLGetStmtAttr** de fonctionner avec ODBC 2. *x* , un appel à **SQLGetStmtAttr** est mappé dans le gestionnaire de pilotes à **SQLGetStmtOption**.  
  
 Les attributs d’instruction suivants étant en lecture seule, ils peuvent être récupérés par **SQLGetStmtAttr**, mais ils ne sont pas définis par **SQLSetStmtAttr**:  
  
-   SQL_ATTR_IMP_PARAM_DESC  
  
-   SQL_ATTR_IMP_ROW_DESC  
  
-   SQL_ATTR_ROW_NUMBER  
  
 Pour obtenir la liste des attributs qui peuvent être définis et récupérés, consultez [SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi du paramètre d’un attribut de connexion|[Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
