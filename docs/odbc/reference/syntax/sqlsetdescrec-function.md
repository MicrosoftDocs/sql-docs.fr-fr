---
description: SQLSetDescRec, fonction
title: SQLSetDescRec fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetDescRec
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetDescRec
helpviewer_keywords:
- SQLSetDescRec function [ODBC]
ms.assetid: bf55256c-7eb7-4e3f-97ef-b0fee09ba829
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 65e0a789f38ad9f0d1d4b624caca896d01356ce2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160190"
---
# <a name="sqlsetdescrec-function"></a>SQLSetDescRec, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 La fonction **SQLSetDescRec** définit plusieurs champs de descripteur qui affectent le type de données et la mémoire tampon liés à une colonne ou à des données de paramètre.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSetDescRec(  
      SQLHDESC      DescriptorHandle,  
      SQLSMALLINT   RecNumber,  
      SQLSMALLINT   Type,  
      SQLSMALLINT   SubType,  
      SQLLEN        Length,  
      SQLSMALLINT   Precision,  
      SQLSMALLINT   Scale,  
      SQLPOINTER    DataPtr,  
      SQLLEN *      StringLengthPtr,  
      SQLLEN *      IndicatorPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *DescriptorHandle*  
 Entrée Handle de descripteur. Ce ne doit pas être un handle IRD.  
  
 *RecNumber*  
 Entrée Indique l’enregistrement du descripteur qui contient les champs à définir. Les enregistrements de descripteur sont numérotés à partir de 0, le numéro d’enregistrement 0 étant l’enregistrement du signet. Cet argument doit être supérieur ou égal à 0. Si *recnumber* est supérieur à la valeur de SQL_DESC_COUNT, SQL_DESC_COUNTis remplacé par la valeur de *recnumber*.  
  
 *Type*  
 Entrée Valeur sur laquelle définir le champ SQL_DESC_TYPE pour l’enregistrement du descripteur.  
  
 *Sous-type*  
 Entrée Pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL, il s’agit de la valeur à laquelle définir le champ SQL_DESC_DATETIME_INTERVAL_CODE.  
  
 *Durée*  
 Entrée Valeur sur laquelle définir le champ SQL_DESC_OCTET_LENGTH pour l’enregistrement du descripteur.  
  
 *Précision*  
 Entrée Valeur sur laquelle définir le champ SQL_DESC_PRECISION pour l’enregistrement du descripteur.  
  
 *Mettre à l’échelle*  
 Entrée Valeur sur laquelle définir le champ SQL_DESC_SCALE pour l’enregistrement du descripteur.  
  
 *DataPtr*  
 [Entrée ou sortie différée] Valeur sur laquelle définir le champ SQL_DESC_DATA_PTR pour l’enregistrement du descripteur. *DataPtr* peut être défini sur un pointeur null.  
  
 L’argument *DataPtr* peut être défini sur un pointeur null pour définir le champ SQL_DESC_DATA_PTR sur un pointeur null. Si le descripteur de l’argument *DescriptorHandle* est associé à un ARD, cela annule la liaison de la colonne.  
  
 *StringLengthPtr*  
 [Entrée ou sortie différée] Valeur sur laquelle définir le champ SQL_DESC_OCTET_LENGTH_PTR pour l’enregistrement du descripteur. *StringLengthPtr* peut être défini sur un pointeur null pour définir le champ SQL_DESC_OCTET_LENGTH_PTR sur un pointeur null.  
  
 *IndicatorPtr*  
 [Entrée ou sortie différée] Valeur sur laquelle définir le champ SQL_DESC_INDICATOR_PTR pour l’enregistrement du descripteur. *IndicatorPtr* peut être défini sur un pointeur null pour définir le champ SQL_DESC_INDICATOR_PTR sur un pointeur null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSetDescRec** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DESC et un *handle* de *DescriptorHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLSetDescRec** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07009|Index de descripteur non valide|L’argument *recnumber* a été défini sur 0, et le *DescriptorHandle* a référencé un handle IPD.<br /><br /> L’argument *recnumber* est inférieur à 0.<br /><br /> L’argument *recnumber* a une valeur supérieure au nombre maximal de colonnes ou de paramètres que la source de données peut prendre en charge, et l’argument *DESCRIPTORHANDLE* était un APD, un IPD ou un ARD.<br /><br /> L’argument *recnumber* était égal à 0, et l’argument *DescriptorHandle* faisait appel à un APD alloué de manière implicite. (Cette erreur ne se produit pas avec un descripteur d’application explicitement alloué, car il n’est pas connu qu’un descripteur d’application explicitement alloué est un APD ou un ARD jusqu’à l’exécution.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) le *DescriptorHandle* a été associé à un *StatementHandle* pour lequel une fonction d’exécution asynchrone (pas celui-ci) a été appelée et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour le *StatementHandle* avec lequel le *DescriptorHandle* a été associé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *DescriptorHandle*. Cette fonction aynchronous était toujours en cours d’exécution lors de l’appel de la fonction **SQLSetDescRec** .<br /><br /> (DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour l’un des handles d’instruction associés à *DescriptorHandle* et retournés SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY016|Impossible de modifier un descripteur de ligne d’implémentation|L’argument *DescriptorHandle* a été associé à un IRD.|  
|HY021|Informations de descripteur incohérentes|Le champ de *type* , ou tout autre champ associé au champ SQL_DESC_TYPE dans le descripteur, n’était pas valide ou est cohérent.<br /><br /> Les informations de descripteur vérifiées pendant une vérification de cohérence ne sont pas cohérentes. (Consultez « vérifications de cohérence », plus loin dans cette section.)|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) le pilote était un pilote ODBC *2. x* , le descripteur était un ARD, l’argument *ColumnNumber* était défini sur 0, et la valeur spécifiée pour l’argument *BufferLength* n’était pas égale à 4.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *DescriptorHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLSetDescRec** pour définir les champs suivants pour une seule colonne ou un seul paramètre :  
  
-   SQL_DESC_TYPE  
  
-   SQL_DESC_DATETIME_INTERVAL_CODE (pour les enregistrements dont le type est SQL_DATETIME ou SQL_INTERVAL)  
  
-   SQL_DESC_OCTET_LENGTH  
  
-   SQL_DESC_PRECISION  
  
-   SQL_DESC_SCALE  
  
-   SQL_DESC_DATA_PTR  
  
-   SQL_DESC_OCTET_LENGTH_PTR  
  
-   SQL_DESC_INDICATOR_PTR  
  
> [!NOTE]  
>  Si un appel à **SQLSetDescRec** échoue, le contenu de l’enregistrement de descripteur identifié par l’argument *recnumber* n’est pas défini.  
  
 Lors de la liaison d’une colonne ou d’un paramètre, **SQLSetDescRec** vous permet de modifier plusieurs champs qui affectent la liaison sans appeler **SQLBindCol** ou **SQLBindParameter** ou effectuer plusieurs appels à **SQLSetDescField**. **SQLSetDescRec** peut définir des champs sur un descripteur qui n’est pas actuellement associé à une instruction. Notez que **SQLBindParameter** définit plus de champs que **SQLSetDescRec**, peut définir des champs sur un APD et un IPD dans un appel et ne nécessite pas de handle de descripteur.  
  
> [!NOTE]  
>  L’attribut d’instruction SQL_ATTR_USE_BOOKMARKS doit toujours être défini avant d’appeler **SQLSetDescRec** avec un argument *recnumber* égal à 0 pour définir les champs de signet. Bien que ce ne soit pas obligatoire, il est vivement recommandé.  
  
## <a name="consistency-checks"></a>Vérifications de cohérence  
 Une vérification de cohérence est effectuée automatiquement par le pilote chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR d’un APD, d’un ARD ou d’une IPD. Si l’un des champs est incohérent avec d’autres champs, **SQLSetDescRec** retourne SQLState HY021 (informations de descripteur incohérentes).  
  
 Chaque fois qu’une application définit le champ SQL_DESC_DATA_PTR d’un APD, d’un ARD ou d’un IPD, le pilote vérifie que la valeur du champ SQL_DESC_TYPE et les valeurs applicables à ce champ SQL_DESC_TYPE sont valides et cohérentes. Cette vérification est toujours effectuée lorsque **SQLBindParameter** ou **SQLBindCol** est appelé ou lorsque **SQLSETDESCREC** est appelé pour un APD, ARD ou IPD. Cette vérification de cohérence comprend les vérifications suivantes sur les champs de descripteur :  
  
-   Le champ SQL_DESC_TYPE doit être l’un des types ODBC C ou SQL valides ou un type SQL spécifique au pilote. Le champ SQL_DESC_CONCISE_TYPE doit être l’un des types ODBC C ou SQL valides ou un type C ou SQL spécifique au pilote, y compris les types DateTime et Interval concis.  
  
-   Si le champ d’enregistrement SQL_DESC_TYPE est SQL_DATETIME ou SQL_INTERVAL, le champ SQL_DESC_DATETIME_INTERVAL_CODE doit être l’un des codes d’intervalle ou DateTime valides. (Consultez la description du champ SQL_DESC_DATETIME_INTERVAL_CODE dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).)  
  
-   Si le champ SQL_DESC_TYPE indique un type numérique, les champs SQL_DESC_PRECISION et SQL_DESC_SCALE sont vérifiés comme valides.  
  
-   Si le champ SQL_DESC_CONCISE_TYPE est un type de données Time ou TIMESTAMP, un type Interval avec un composant seconds ou l’un des types de données Interval avec un composant heure, le champ SQL_DESC_PRECISION est vérifié comme étant une précision de secondes valide.  
  
-   Si le SQL_DESC_CONCISE_TYPE est un type de données Interval, le champ SQL_DESC_DATETIME_INTERVAL_PRECISION est vérifié comme étant une valeur de précision d’intervalle valide.  
  
 Le champ SQL_DESC_DATA_PTR d’une IPD n’est normalement pas défini ; Toutefois, une application peut le faire pour forcer une vérification de cohérence des champs IPD. Une vérification de cohérence ne peut pas être effectuée sur un IRD. La valeur dont le champ SQL_DESC_DATA_PTR de l’IPD est défini n’est pas réellement stockée et ne peut pas être récupérée par un appel à **SQLGetDescField** ou **SQLGetDescRec**; le paramètre est défini uniquement pour forcer la vérification de cohérence.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une colonne|[Fonction SQLBindCol](../../../odbc/reference/syntax/sqlbindcol-function.md)|  
|Liaison d’un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Obtention d’un seul champ de descripteur|[Fonction SQLGetDescField](../../../odbc/reference/syntax/sqlgetdescfield-function.md)|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition des champs de descripteur uniques|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
