---
description: Fonction SQLGetDescField
title: SQLGetDescField fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDescField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDescField
helpviewer_keywords:
- SQLGetDescField function [ODBC]
ms.assetid: f09ff660-1e4a-4370-be85-90d4da0487d3
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0692c769921d8e0ee326b19f0b5d501367237c28
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99201947"
---
# <a name="sqlgetdescfield-function"></a>Fonction SQLGetDescField
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetDescField** retourne le paramètre ou la valeur actuelle d’un champ unique d’un enregistrement de descripteur.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetDescField(  
     SQLHDESC        DescriptorHandle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     FieldIdentifier,  
     SQLPOINTER      ValuePtr,  
     SQLINTEGER      BufferLength,  
     SQLINTEGER *    StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *DescriptorHandle*  
 Entrée Handle de descripteur.  
  
 *RecNumber*  
 Entrée Indique l’enregistrement de descripteur à partir duquel l’application recherche des informations. Les enregistrements de descripteur sont numérotés à partir de 0, le numéro d’enregistrement 0 étant l’enregistrement du signet. Si l’argument *FieldIdentifier* indique un champ d’en-tête, *recnumber* est ignoré. Si *recnumber* est inférieur ou égal à SQL_DESC_COUNT mais que la ligne ne contient pas de données pour une colonne ou un paramètre, un appel à **SQLGetDescField** retourne les valeurs par défaut des champs. (Pour plus d’informations, consultez « initialisation des champs de descripteur » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).)  
  
 *FieldIdentifier*  
 Entrée Indique le champ du descripteur dont la valeur doit être retournée. Pour plus d’informations, consultez la section « argument *FieldIdentifier* » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md).  
  
 *ValuePtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner les informations du descripteur. Le type de données dépend de la valeur de *FieldIdentifier*.  
  
 Si *ValuePtr* est de type entier, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur à 0 avant d’appeler cette fonction, car certains pilotes peuvent uniquement écrire le plus petit 32 bits ou le 16 bits d’une mémoire tampon et ne pas modifier le bit d’ordre supérieur.  
  
 Si *ValuePtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *ValuePtr*.  
  
 *BufferLength*  
 Entrée Si *FieldIdentifier* est un champ défini par ODBC et que *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de \* *ValuePtr*. Si *FieldIdentifier* est un champ défini par ODBC et \* *ValuePtr* est un entier, *BufferLength* est ignoré. Si la valeur de *\* ValuePtr* est un type de données Unicode (lors de l’appel de **SQLGetDescFieldW**), l’argument *BufferLength* doit être un nombre pair.  
  
 Si *FieldIdentifier* est un champ défini par le pilote, l’application indique la nature du champ au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *\* ValuePtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *\* ValuePtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *\* ValuePtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *\* ValuePtr* contient un type de données de longueur fixe, *BufferLength* est SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT ou SQL_IS_USMALLINT, selon le cas.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers la mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exception du nombre d’octets requis pour le caractère de fin null) disponibles à retourner dans **ValuePtr*.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_NO_DATA ou SQL_INVALID_HANDLE.  
  
 SQL_NO_DATA est retourné si *recnumber* est supérieur au nombre actuel d’enregistrements de descripteur.  
  
 SQL_NO_DATA est retourné si *DescriptorHandle* est un handle IRD et si l’instruction est dans l’État prepared ou Executed, mais qu’aucun curseur n’est associé à ce dernier.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLGetDescField** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_STMT et un *handle* de *StatementHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLGetDescField** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le ValuePtr de mémoire tampon \*  n’est pas assez grand pour retourner le champ de descripteur entier, donc le champ a été tronqué. La longueur du champ descripteur non tronqué est retournée dans **StringLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|07009|Index de descripteur non valide|(DM) l’argument *recnumber* était égal à 0, l’attribut d’instruction SQL_ATTR_USE_BOOKMARK était SQL_UB_OFF et l’argument *DescriptorHandle* était un handle IRD. (Cette erreur peut être retournée pour un descripteur alloué explicitement uniquement si le descripteur est associé à un descripteur d’instruction.)<br /><br /> L’argument *FieldIdentifier* était un champ d’enregistrement, l’argument *recnumber* était 0 et l’argument *DescriptorHandle* était un handle IPD.<br /><br /> L’argument *recnumber* est inférieur à 0.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY007|L’instruction associée n’est pas préparée|*DescriptorHandle* a été associé à un *StatementHandle* en tant que IRD et le descripteur d’instruction associé n’a pas été préparé ou exécuté.|  
|HY010|Erreur de séquence de fonction|(DM) *DescriptorHandle* a été associé à un *StatementHandle* pour lequel une fonction d’exécution asynchrone (pas celle-ci) a été appelée et était toujours en cours d’exécution quand cette fonction a été appelée.<br /><br /> (DM) *DescriptorHandle* a été associé à *un StatementHandle* pour lequel **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** a été appelé et retourné SQL_NEED_DATA. Cette fonction a été appelée avant l’envoi des données pour l’ensemble des paramètres ou des colonnes de données en cours d’exécution.<br /><br /> (DM) une fonction d’exécution asynchrone a été appelée pour le handle de connexion associé à *DescriptorHandle*. Cette fonction asynchrone était toujours en cours d’exécution lors de l’appel de la fonction **SQLGetDescField** .|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY021|Informations de descripteur incohérentes|Les champs SQL_DESC_TYPE et SQL_DESC_DATETIME_INTERVAL_CODE ne forment pas un type SQL ODBC valide, un type SQL spécifique au pilote valide (pour IPD) ou un type C ODBC valide (pour APD ou ARDs).|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) *\* ValuePtr* était une chaîne de caractères et *BufferLength* était inférieur à zéro.|  
|HY091|Identificateur de champ de descripteur non valide|*FieldIdentifier* n’est pas un champ défini par ODBC et n’est pas une valeur définie par l’implémentation.<br /><br /> *FieldIdentifier* n’a pas été défini pour *DescriptorHandle*.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *DescriptorHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLGetDescField** pour retourner la valeur d’un champ unique d’un enregistrement de descripteur. Un appel à **SQLGetDescField** peut retourner le paramètre de n’importe quel champ dans n’importe quel type de descripteur, y compris les champs d’en-tête, les champs d’enregistrement et les champs de signet. Une application peut obtenir les paramètres de plusieurs champs dans des descripteurs identiques ou différents, dans un ordre arbitraire, en effectuant des appels répétés à **SQLGetDescField**. **SQLGetDescField** peut également être appelé pour retourner des champs de descripteur définis par le pilote.  
  
 Pour des raisons de performances, une application ne doit pas appeler **SQLGetDescField** pour un IRD avant d’exécuter une instruction.  
  
 Les paramètres de plusieurs champs qui décrivent le nom, le type de données et le stockage des données de colonne ou de paramètre peuvent également être récupérés dans un seul appel à **SQLGetDescRec**. **SQLGetStmtAttr** peut être appelé pour retourner la valeur d’un champ unique dans l’en-tête de descripteur qui est également un attribut d’instruction. **SQLColAttribute**, **SQLDescribeCol** et **SQLDescribeParam** retournent des champs d’enregistrement ou de signet.  
  
 Quand une application appelle **SQLGetDescField** pour récupérer la valeur d’un champ qui n’est pas défini pour un type de descripteur particulier, la fonction retourne SQL_SUCCESS mais la valeur retournée pour le champ n’est pas définie. Par exemple, l’appel de **SQLGetDescField** pour le champ SQL_DESC_NAME ou SQL_DESC_NULLABLE d’un APD ou ARD retourne SQL_SUCCESS, mais une valeur non définie pour le champ.  
  
 Quand une application appelle **SQLGetDescField** pour récupérer la valeur d’un champ défini pour un type de descripteur particulier mais qui n’a pas de valeur par défaut et n’a pas encore été définie, la fonction retourne SQL_SUCCESS mais la valeur retournée pour le champ n’est pas définie. Pour plus d’informations sur l’initialisation des champs de descripteur et les descriptions des champs, consultez « initialisation des champs de descripteur » dans [SQLSetDescField](../../../odbc/reference/syntax/sqlsetdescfield-function.md). Pour plus d’informations sur les descripteurs, consultez [descripteurs](../../../odbc/reference/develop-app/descriptors.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention de plusieurs champs de descripteur|[SQLGetDescRec, fonction](../../../odbc/reference/syntax/sqlgetdescrec-function.md)|  
|Définition d’un champ de descripteur unique|[SQLSetDescField, fonction](../../../odbc/reference/syntax/sqlsetdescfield-function.md)|  
|Définition de plusieurs champs de descripteur|[SQLSetDescRec, fonction](../../../odbc/reference/syntax/sqlsetdescrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
