---
description: SQLParamData, fonction
title: SQLParamData, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLParamData
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLParamData
helpviewer_keywords:
- SQLParamData function [ODBC]
ms.assetid: 68fe010d-9539-4e5b-a260-c8d32423b1db
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e050102612c7e5d370e730909522fa23e801a8f2
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195847"
---
# <a name="sqlparamdata-function"></a>SQLParamData, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLParamData** est utilisé avec **SQLPutData** pour fournir des données de paramètre au moment de l’exécution de l’instruction, et avec **SQLGetData** pour récupérer les données de paramètre de sortie diffusées en continu.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLParamData(  
     SQLHSTMT       StatementHandle,  
     SQLPOINTER *   ValuePtrPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *StatementHandle*  
 Entrée Descripteur d’instruction.  
  
 *ValuePtrPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner l’adresse de la mémoire tampon *ParameterValuePtr* spécifiée dans **SQLBindParameter** (pour les données de paramètre) ou l’adresse de la mémoire tampon *TargetValuePtr* spécifiée dans **SQLBindCol** (pour les données de la colonne), comme contenu dans le champ d’enregistrement du descripteur de SQL_DESC_DATA_PTR.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_NO_DATA, SQL_STILL_EXECUTING, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_PARAM_DATA_AVAILABLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLParamData** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLParamData** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07006|Violation d’attribut de type de données restreint|La valeur de données identifiée par l’argument *ValueType* dans **SQLBindParameter** pour le paramètre lié n’a pas pu être convertie dans le type de données identifié par l’argument *ParameterType* dans **SQLBindParameter**.<br /><br /> La valeur de données retournée pour un paramètre lié en tant que SQL_PARAM_INPUT_OUTPUT ou SQL_PARAM_OUTPUT n’a pas pu être convertie dans le type de données identifié par l’argument *ValueType* dans **SQLBindParameter**.<br /><br /> (Si les valeurs de données pour une ou plusieurs lignes n’ont pas pu être converties, mais qu’une ou plusieurs lignes ont été correctement retournées, cette fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22026|Chaîne de données ou longueur non correspondante|Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y », et moins de données ont été envoyées pour un paramètre long (le type de données était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un long type de données spécifique à la source de données) que celui spécifié avec l’argument *StrLen_or_IndPtr* dans **SQLBindParameter**.<br /><br /> Le type d’informations SQL_NEED_LONG_DATA_LEN dans **SQLGetInfo** était « Y », et moins de données ont été envoyées pour une longue colonne (le type de données était SQL_LONGVARCHAR, SQL_LONGVARBINARY ou un type de données long spécifique à la source de données) que celui spécifié dans la mémoire tampon de longueur correspondant à une colonne dans une ligne de données qui a été ajoutée ou mise à jour avec **SQLBulkOperations** ou mis à jour avec **SQLSetPos**|  
|40001|Échec de la sérialisation|La transaction a été restaurée en raison d’un blocage de ressource avec une autre transaction.|  
|40003|Saisie semi-automatique des instructions inconnue|La connexion associée a échoué pendant l’exécution de cette fonction et l’état de la transaction ne peut pas être déterminé.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer de la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY008|Opération annulée|Le traitement asynchrone a été activé pour *StatementHandle*. La fonction a été appelée, et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle*; la fonction a ensuite été appelée à nouveau sur le *StatementHandle*.<br /><br /> La fonction a été appelée et avant la fin de l’exécution, **SQLCancel** ou **SQLCancelHandle** a été appelé sur le *StatementHandle* à partir d’un thread différent dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM) l’appel de fonction précédent n’était pas un appel à **SQLExecDirect**, à **SQLExecute**, à **SQLBulkOperations** ou à **SQLSetPos** où le code de retour était SQL_NEED_DATA, ou l’appel de fonction précédent était un appel à **SQLPutData**.<br /><br /> L’appel de fonction précédent était un appel à **SQLParamData**.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *StatementHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLParamData** .<br /><br /> (DM) une fonction d’exécution asynchrone (pas celle-ci) a été appelée pour le *StatementHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé pour *StatementHandle* et a retourné SQL_NEED_DATA. **SQLCancel** a été appelé avant l’envoi des données pour l’ensemble des colonnes ou des paramètres de données en cours d’exécution.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote qui correspond au *StatementHandle* ne prend pas en charge la fonction.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédent sur le descripteur retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelé sur le handle pour effectuer un traitement postérieur et terminer l’opération.|  
  
 Si **SQLParamData** est appelé lors de l’envoi de données pour un paramètre dans une instruction SQL, il peut retourner tout SQLSTATE pouvant être retourné par la fonction appelée pour exécuter l’instruction (**SQLExecute** ou **SQLExecDirect**). Si elle est appelée lors de l’envoi de données pour une colonne en cours de mise à jour ou d’ajout avec **SQLBulkOperations** ou si elle est mise à jour avec **SQLSetPos**, elle peut renvoyer n’importe quel SQLSTATE pouvant être retourné par **SQLBulkOperations** ou **SQLSetPos**.  
  
## <a name="comments"></a>Commentaires  
 **SQLParamData** peut être appelé pour fournir des données en cours d’exécution pour deux utilisations : les données de paramètre qui seront utilisées dans un appel à **SQLExecute** ou **SQLExecDirect**, ou les données de colonne qui seront utilisées lorsqu’une ligne est mise à jour ou ajoutée par un appel à **SQLBulkOperations** ou mises à jour par un appel à **SQLSetPos**. Au moment de l’exécution, **SQLParamData** retourne à l’application un indicateur des données requises par le pilote.  
  
 Quand une application appelle **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos**, le pilote retourne SQL_NEED_DATA s’il a besoin de données en cours d’exécution. Une application appelle ensuite **SQLParamData** pour déterminer les données à envoyer. Si le pilote nécessite des données de paramètre, le pilote renvoie dans la mémoire tampon de sortie *\* ValuePtrPtr* la valeur que l’application place dans la mémoire tampon de l’ensemble de lignes. L’application peut utiliser cette valeur pour déterminer les données de paramètre demandées par le pilote. Si le pilote nécessite des données de colonne, le pilote renvoie dans la mémoire tampon *\* ValuePtrPtr* l’adresse à laquelle la colonne a été initialement liée, comme suit :  
  
 *Adresse liée*  +  *Décalage de liaison* + ((*numéro de ligne* -1) x taille de l' *élément*)  
  
 où les variables sont définies comme indiqué dans le tableau suivant.  
  
|Variable|Description|  
|--------------|-----------------|  
|*Adresse liée*|Adresse spécifiée avec l’argument *TargetValuePtr* dans **SQLBindCol**.|  
|*Décalage de liaison*|Valeur stockée à l’adresse spécifiée avec l’attribut d’instruction SQL_ATTR_ROW_BIND_OFFSET_PTR.|  
|*Numéro de ligne*|Numéro de base 1 de la ligne dans l’ensemble de lignes. Pour les extractions sur une seule ligne, il s’agit de la valeur par défaut 1.|  
|*Taille de l’élément*|Valeur de l’attribut d’instruction SQL_ATTR_ROW_BIND_TYPE pour les tampons de données et de longueur/d’indicateur.|  
  
 Elle retourne également SQL_NEED_DATA, qui est un indicateur pour l’application qu’elle doit appeler **SQLPutData** pour envoyer les données.  
  
 L’application appelle **SQLPutData** autant de fois que nécessaire pour envoyer les données de données en cours d’exécution pour la colonne ou le paramètre. Une fois que toutes les données ont été envoyées pour la colonne ou le paramètre, l’application appelle **SQLParamData** à nouveau. Si **SQLParamData** retourne à nouveau SQL_NEED_DATA, les données doivent être envoyées pour un autre paramètre ou une autre colonne. Par conséquent, l’application appelle **SQLPutData**. Si toutes les données en cours d’exécution ont été envoyées pour tous les paramètres ou colonnes, **SQLParamData** retourne SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO, la valeur de *\* ValuePtrPtr* n’est pas définie, et l’instruction SQL peut être exécutée ou l’appel **SQLBulkOperations** ou **SQLSetPos** peut être traité.  
  
 Si **SQLParamData** fournit des données de paramètre pour une instruction UPDATE ou DELETE recherchée qui n’affecte aucune ligne de la source de données, l’appel à **SQLParamData** retourne SQL_NO_DATA.  
  
 Pour plus d’informations sur la façon dont les données de paramètre de données en cours d’exécution sont transmises au moment de l’exécution de l’instruction, consultez « transmission des valeurs de paramètres » dans [SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md) et [envoi de données de type long](../../../odbc/reference/develop-app/sending-long-data.md). Pour plus d’informations sur la mise à jour ou l’ajout des données de colonne de données en cours d’exécution, consultez la section « utilisation de SQLSetPos » dans [SQLSetPos](../../../odbc/reference/syntax/sqlsetpos-function.md), « exécution de mises à jour en bloc à l’aide de signets » dans [SQLBulkOperations](../../../odbc/reference/syntax/sqlbulkoperations-function.md)et [long Data, SQLSetPos et SQLBulkOperations](../../../odbc/reference/develop-app/long-data-and-sqlsetpos-and-sqlbulkoperations.md).  
  
 **SQLParamData** peut être appelé pour extraire les paramètres de sortie diffusés en continu. Quand **SQLMoreResults**, **SQLExecute**, **SQLGetData** ou **SQLExecDirect** retourne SQL_PARAM_DATA_AVAILABLE, appelez **SQLParamData** pour déterminer quel paramètre a une valeur disponible. Pour plus d’informations sur les SQL_PARAM_DATA_AVAILABLE et les paramètres de sortie diffusés en continu, consultez [récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md).  
  
## <a name="code-example"></a>Exemple de code  
 Consultez [SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Liaison d’une mémoire tampon à un paramètre|[Fonction SQLBindParameter](../../../odbc/reference/syntax/sqlbindparameter-function.md)|  
|Annulation du traitement des instructions|[SQLCancel, fonction](../../../odbc/reference/syntax/sqlcancel-function.md)|  
|Retour d’informations sur un paramètre dans une instruction|[Fonction SQLDescribeParam](../../../odbc/reference/syntax/sqldescribeparam-function.md)|  
|Exécution d’une instruction SQL|[SQLExecDirect, fonction](../../../odbc/reference/syntax/sqlexecdirect-function.md)|  
|Exécution d’une instruction SQL préparée|[SQLExecute, fonction](../../../odbc/reference/syntax/sqlexecute-function.md)|  
|Envoi de données de paramètre au moment de l’exécution|[Fonction SQLPutData](../../../odbc/reference/syntax/sqlputdata-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Récupération des paramètres de sortie à l’aide de SQLGetData](../../../odbc/reference/develop-app/retrieving-output-parameters-using-sqlgetdata.md)
