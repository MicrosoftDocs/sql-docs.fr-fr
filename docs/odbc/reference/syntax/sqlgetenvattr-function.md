---
description: SQLGetEnvAttr, fonction
title: SQLGetEnvAttr fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLGetEnvAttr
helpviewer_keywords:
- SQLGetEnvAttr function [ODBC]
ms.assetid: 01f4590f-427a-4280-a1c3-18de9f7d86c1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 35df579ce679b8375574a16eea16a2f5d84c3043
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99199847"
---
# <a name="sqlgetenvattr-function"></a>SQLGetEnvAttr, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetEnvAttr** retourne la valeur actuelle d’un attribut d’environnement.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetEnvAttr(  
     SQLHENV        EnvironmentHandle,  
     SQLINTEGER     Attribute,  
     SQLPOINTER     ValuePtr,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 Entrée Handle d’environnement.  
  
 *Attribut*  
 Entrée Attribut à récupérer.  
  
 *ValuePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur actuelle de l’attribut spécifié par l' *attribut*.  
  
 Si *ValuePtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *ValuePtr*.  
  
 *BufferLength*  
 Entrée Si *ValuePtr* pointe vers une chaîne de caractères, cet argument doit être la longueur de \* *ValuePtr*. Si \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si *\* ValuePtr* est une chaîne Unicode (lors de l’appel de **SQLGetEnvAttrW**), l’argument *BufferLength* doit être un nombre pair. Si la valeur de l’attribut n’est pas une chaîne de caractères, *BufferLength* est inutilisé.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exclusion du caractère de fin null) pouvant être retourné dans *\* ValuePtr*. Si la valeur de l’attribut est une chaîne de caractères et que le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength*, les données de \* *ValuePtr* sont tronquées à *BufferLength* moins la longueur d’un caractère de fin null et le pilote se termine par un caractère null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetEnvAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_ENV et un *handle* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetEnvAttr** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Les données retournées dans \* *ValuePtr* ont été tronquées pour être *BufferLength* moins le caractère de fin null. La longueur de la valeur de chaîne non tronquée est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQL_ATTR_ODBC_VERSION** n’a pas encore été défini via **SQLSetEnvAttr**. Vous n’avez pas besoin de définir **SQL_ATTR_ODBC_VERSION** explicitement si vous utilisez **SQLAllocHandleStd**.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY092|Identificateur d’attribut/option non valide|La valeur spécifiée pour l' *attribut* argument n’est pas valide pour la version de ODBC prise en charge par le pilote.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l' *attribut* argument était un attribut d’environnement ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant à *EnvironmentHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Pour obtenir la liste des attributs, consultez [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md). Il n’existe aucun attribut d’environnement propre au pilote. Si *attribute* spécifie un attribut qui retourne une chaîne, *ValuePtr* doit être un pointeur vers une mémoire tampon dans laquelle retourner la chaîne. La longueur maximale de la chaîne, y compris l’octet de fin null, sera *BufferLength* octets.  
  
 **SQLGetEnvAttr** peut être appelée à tout moment entre l’allocation et la libération d’un handle d’environnement. Tous les attributs d’environnement définis avec succès par l’application pour l’environnement persistent jusqu’à ce que **SQLFreeHandle** soit appelé sur le *EnvironmentHandle* avec un *comme HandleType* de SQL_HANDLE_ENV. Plusieurs descripteurs d’environnement peuvent être alloués simultanément dans ODBC 3 *. x*. Un attribut d’environnement sur un environnement n’est pas affecté lorsqu’un autre environnement a été alloué.  
  
> [!NOTE]
>  L’attribut d’environnement SQL_ATTR_OUTPUT_NTS est pris en charge par les applications conformes aux normes. Quand **SQLGetEnvAttr** est appelé, le gestionnaire de pilotes ODBC 3 *. x* retourne toujours SQL_TRUE pour cet attribut. SQL_ATTR_OUTPUT_NTS peut être défini sur SQL_TRUE uniquement par un appel à **SQLSetEnvAttr**.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Renvoi du paramètre d’un attribut de connexion|[Fonction SQLGetConnectAttr](../../../odbc/reference/syntax/sqlgetconnectattr-function.md)|  
|Renvoi du paramètre d’un attribut d’instruction|[Fonction SQLGetStmtAttr](../../../odbc/reference/syntax/sqlgetstmtattr-function.md)|  
|Définition d’un attribut de connexion|[Fonction SQLSetConnectAttr](../../../odbc/reference/syntax/sqlsetconnectattr-function.md)|  
|Définition d’un attribut d’environnement|[Fonction SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md)|  
|Définition d’un attribut d’instruction|[Fonction SQLSetStmtAttr](../../../odbc/reference/syntax/sqlsetstmtattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
