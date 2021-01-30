---
description: SQLGetDescRec, fonction
title: SQLGetDescRec fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescRec
helpviewer_keywords:
- SQLGetDescRec function [ODBC]
ms.assetid: 325e0907-8e87-44e8-a111-f39e636a9cbc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a340ecc75e7ae4f03c44051f594fda7305a2a00f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205641"
---
# <a name="sqlgetdescrec-function"></a>SQLGetDescRec, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetDescRec** retourne les paramètres actuels ou les valeurs de plusieurs champs d’un enregistrement de descripteur. Les champs retournés décrivent le nom, le type de données et le stockage des données de colonne ou de paramètre.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetDescRec(  
      SQLHDESC        DescriptorHandle,  
      SQLSMALLINT     RecNumber,  
      SQLCHAR *       Name,  
      SQLSMALLINT     BufferLength,  
      SQLSMALLINT *   StringLengthPtr,  
      SQLSMALLINT *   TypePtr,  
      SQLSMALLINT *   SubTypePtr,  
      SQLLEN *        LengthPtr,  
      SQLSMALLINT *   PrecisionPtr,  
      SQLSMALLINT *   ScalePtr,  
      SQLSMALLINT *   NullablePtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *DescriptorHandle*  
 Entrée Handle de descripteur.  
  
 *RecNumber*  
 Entrée Indique l’enregistrement de descripteur à partir duquel l’application recherche des informations. Les enregistrements de descripteur sont numérotés à partir de 1, le numéro d’enregistrement 0 étant l’enregistrement du signet. L’argument *recnumber* doit être inférieur ou égal à la valeur de SQL_DESC_COUNT. Si *recnumber* est inférieur ou égal à SQL_DESC_COUNT mais que la ligne ne contient pas de données pour une colonne ou un paramètre, un appel à **SQLGetDescRec** retourne les valeurs par défaut des champs. (Pour plus d’informations, consultez « initialisation des champs de descripteur » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).)  
  
 *Nom*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le champ SQL_DESC_NAME pour l’enregistrement du descripteur.  
  
 Si le *nom* est null, *StringLengthPtr* renvoie toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe le *nom*.  
  
 *BufferLength*  
 Entrée Longueur de la mémoire tampon de *nom* *, en caractères.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre de caractères de données disponibles à retourner dans la \* mémoire tampon de *nom* , à l’exclusion du caractère de fin null. Si le nombre de caractères est supérieur ou égal à *BufferLength*, les données du \* *nom* sont tronquées à *BufferLength* moins la longueur d’un caractère de fin null, et le pilote se termine par un caractère null.  
  
 *TypePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_TYPE pour l’enregistrement du descripteur.  
  
 *SubTypePtr*  
 Sortie Pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL, il s’agit d’un pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *LengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_OCTET_LENGTH pour l’enregistrement du descripteur.  
  
 *PrecisionPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_PRECISION pour l’enregistrement du descripteur.  
  
 *ScalePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_SCALE pour l’enregistrement du descripteur.  
  
 *NullablePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la valeur du champ SQL_DESC_NULLABLE pour l’enregistrement du descripteur.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA ou SQL_INVALID_HANDLE.  
  
 SQL_NO_DATA est retourné si *recnumber* est supérieur au nombre actuel d’enregistrements de descripteur.  
  
 SQL_NO_DATA est retourné si *DescriptorHandle* est un handle IRD et si l’instruction est dans l’État prepared ou Executed, mais qu’aucun curseur n’est associé à ce dernier.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetDescRec** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DESC et un *handle* de *DescriptorHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLGetDescRec** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le \* *nom* de la mémoire tampon n’est pas assez grand pour retourner le champ de descripteur entier. Par conséquent, le champ a été tronqué. La longueur du champ descripteur non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07009|Index de descripteur non valide|L’argument *FieldIdentifier* était un champ d’enregistrement, l’argument *recnumber* était défini sur 0 et l’argument *DescriptorHandle* était un handle IPD.<br /><br /> (DM) l’argument *recnumber* a la valeur 0 et l’attribut d’instruction SQL_ATTR_USE_BOOKMARKS a la valeur SQL_UB_OFF, et l’argument *DescriptorHandle* était un handle IRD.<br /><br /> L’argument *recnumber* est inférieur à 0.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY007|L’instruction associée n’est pas préparée|*DescriptorHandle* a été associé à un IRD et le descripteur d’instruction associé n’était pas dans l’État prepared ou Executed.|  
|HY010|Erreur de séquence de fonction|(DM) *DescriptorHandle* a été associé à un *StatementHandle* pour lequel une fonction d’exécution asynchrone (pas celle-ci) a été appelée et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) *DescriptorHandle* a été associé à *un StatementHandle* pour lequel **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *DescriptorHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de **SQLGetDescRec** .|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé à *DescriptorHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLGetDescRec** pour récupérer les valeurs des champs de descripteur suivants pour une seule colonne ou un seul paramètre :  
  
-   SQL_DESC_NAME  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_NULLABLE  
  
 **SQLGetDescRec** ne récupère pas les valeurs des champs d’en-tête.  
  
 Une application peut empêcher le retour de la valeur d’un champ en affectant à l’argument qui correspond au champ la valeur d’un pointeur null.  
  
 Quand une application appelle **SQLGetDescRec** pour récupérer la valeur d’un champ qui n’est pas défini pour un type de descripteur particulier, la fonction retourne SQL_SUCCESS mais la valeur retournée pour le champ n’est pas définie. Par exemple, l’appel de **SQLGetDescRec** pour le champ SQL_DESC_NAME ou SQL_DESC_NULLABLE d’un APD ou ARD retourne SQL_SUCCESS, mais une valeur non définie pour le champ.  
  
 Quand une application appelle **SQLGetDescRec** pour récupérer la valeur d’un champ défini pour un type de descripteur particulier mais qui n’a pas de valeur par défaut et n’a pas encore été définie, la fonction retourne SQL_SUCCESS mais la valeur retournée pour le champ n’est pas définie. Pour plus d’informations, consultez « initialisation des champs de descripteur » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 Les valeurs des champs peuvent également être récupérées individuellement par un appel à **SQLGetDescField**. Pour obtenir une description des champs dans un en-tête ou un enregistrement de descripteur, consultez [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une colonne|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Liaison d’un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtention d’un champ de descripteur|[Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Définition de plusieurs champs de descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
