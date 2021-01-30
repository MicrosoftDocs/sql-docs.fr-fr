---
description: Fonction SQLGetDiagField
title: SQLGetDiagField, fonction | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagField
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagField
helpviewer_keywords:
- SQLGetDiagField function [ODBC]
ms.assetid: 1dbc4398-97a8-4585-bb77-1f7ea75e24c4
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 733933691297f22934fe59e0c337dfd6093ddd3b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194190"
---
# <a name="sqlgetdiagfield-function"></a>Fonction SQLGetDiagField

**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetDiagField** retourne la valeur actuelle d’un champ d’un enregistrement de la structure de données de diagnostic (associée à un handle spécifié) qui contient des informations d’erreur, d’avertissement et d’État.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp

SQLRETURN SQLGetDiagField(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLSMALLINT     DiagIdentifier,  
     SQLPOINTER      DiagInfoPtr,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *HandleType*  
 Entrée Identificateur de type de handle qui décrit le type de handle pour lequel les diagnostics sont requis. Doit prendre l'une des valeurs suivantes :  
  
-   SQL_HANDLE_DBC  
  
-   SQL_HANDLE_DBC_INFO_TOKEN  
  
-   SQL_HANDLE_DESC  
  
-   SQL_HANDLE_ENV  
  
-   SQL_HANDLE_STMT  
  
 SQL_HANDLE_DBC_INFO_TOKEN descripteur est utilisé uniquement par le gestionnaire de pilotes et le pilote. Les applications ne doivent pas utiliser ce type de handle. Pour plus d’informations sur la SQL_HANDLE_DBC_INFO_TOKEN, consultez [développement de la sensibilisation aux Connection-Pool dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md).  
  
 *Handle*  
 Entrée Handle pour la structure de données de diagnostic, du type indiqué par *comme HandleType*. Si *comme HandleType* a la valeur SQL_HANDLE_ENV, *descripteur* peut être un handle d’environnement partagé ou non partagé.  
  
 *RecNumber*  
 Entrée Indique l’enregistrement d’État à partir duquel l’application recherche des informations. Les enregistrements d’État sont numérotés à partir de 1. Si l’argument *DiagIdentifier* indique un champ de l’en-tête de diagnostic, *recnumber* est ignoré. Si ce n’est pas le cas, il doit être supérieur à 0.  
  
 *DiagIdentifier*  
 Entrée Indique le champ du diagnostic dont la valeur doit être retournée. Pour plus d’informations, consultez la section « argument *DiagIdentifier* » dans « Comments ».  
  
 *DiagInfoPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner les informations de diagnostic. Le type de données dépend de la valeur de *DiagIdentifier*. Si *DiagInfoPtr* est un type entier, les applications doivent utiliser une mémoire tampon de SQLULEN et initialiser la valeur à 0 avant d’appeler cette fonction, car certains pilotes peuvent uniquement écrire le plus petit 32 bits ou 16 bits d’une mémoire tampon et ne pas modifier le bit d’ordre supérieur.  
  
 Si *DiagInfoPtr* a la valeur null, *StringLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *DiagInfoPtr*.  
  
 *BufferLength*  
 Entrée Si *DiagIdentifier* est un diagnostic défini par ODBC et que *DiagInfoPtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de \* *DiagInfoPtr*. Si *DiagIdentifier* est un champ défini par ODBC et \* *DiagInfoPtr* est un entier, *BufferLength* est ignoré. Si la valeur de *\* DiagInfoPtr* est une chaîne Unicode (lors de l’appel de **SQLGetDiagFieldW**), l’argument *BufferLength* doit être un nombre pair.  
  
 Si *DiagIdentifier* est un champ défini par le pilote, l’application indique la nature du champ au gestionnaire de pilotes en définissant l’argument *BufferLength* . *BufferLength* peut avoir les valeurs suivantes :  
  
-   Si *DiagInfoPtr* est un pointeur vers une chaîne de caractères, *BufferLength* est la longueur de la chaîne ou SQL_NTS.  
  
-   Si *DiagInfoPtr* est un pointeur vers une mémoire tampon binaire, l’application place le résultat de la macro SQL_LEN_BINARY_ATTR (*longueur*) dans *BufferLength*. Cela place une valeur négative dans *BufferLength*.  
  
-   Si *DiagInfoPtr* est un pointeur vers une valeur autre qu’une chaîne de caractères ou une chaîne binaire, *BufferLength* doit avoir la valeur SQL_IS_POINTER.  
  
-   Si *\* DiagInfoPtr* contient un type de données de longueur fixe, *BufferLength* est SQL_IS_INTEGER, SQL_IS_UINTEGER, SQL_IS_SMALLINT ou SQL_IS_USMALLINT, selon le cas.  
  
 *StringLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exception du nombre d’octets requis pour le caractère de fin null) disponibles pour retourner dans \* *DiagInfoPtr*, pour les données de type caractère. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength*, le texte de \* *DiagInfoPtr* est tronqué à *BufferLength* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_NO_DATA.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLGetDiagField** ne publie pas d’enregistrements de diagnostic pour lui-même. Elle utilise les valeurs de retour suivantes pour signaler le résultat de sa propre exécution :  
  
-   SQL_SUCCESS : la fonction a retourné des informations de diagnostic.  
  
-   SQL_SUCCESS_WITH_INFO : \* *DiagInfoPtr* est trop petit pour contenir le champ de diagnostic demandé. Par conséquent, les données du champ diagnostic ont été tronquées. Pour déterminer qu’une troncation s’est produite, l’application doit comparer *BufferLength* au nombre réel d’octets disponibles, qui est écrit dans **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE : le handle indiqué par *comme HandleType* et *handle* n’est pas un handle valide.  
  
-   SQL_ERROR : l’un des éléments suivants s’est produit :  
  
    -   *L’argument DiagIdentifier* ne faisait pas partie des valeurs valides.  
  
    -   *L’argument DiagIdentifier* a été SQL_DIAG_CURSOR_ROW_COUNT, SQL_DIAG_DYNAMIC_FUNCTION, SQL_DIAG_DYNAMIC_FUNCTION_CODE ou SQL_DIAG_ROW_COUNT, mais *handle* n’était pas un descripteur d’instruction. (Le gestionnaire de pilotes retourne ce diagnostic.)  
  
    -   *L’argument recnumber* était négatif ou égal à 0 lorsque *DiagIdentifier* indiquait un champ d’un enregistrement de diagnostic. *Recnumber* est ignoré pour les champs d’en-tête.  
  
    -   La valeur demandée était une chaîne de caractères et *BufferLength* était inférieur à zéro.  
  
    -   Lors de l’utilisation d’une notification asynchrone, l’opération asynchrone sur le descripteur n’est pas terminée.  
  
-   SQL_NO_DATA : *recnumber* est supérieur au nombre d’enregistrements de diagnostics qui existaient pour le descripteur spécifié dans *descripteur.* La fonction retourne également SQL_NO_DATA pour tout *recnumber* positif s’il n’y a pas d’enregistrements de diagnostic pour *descripteur*.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle généralement **SQLGetDiagField** pour accomplir l’un des trois objectifs suivants :  
  
1.  Pour obtenir des informations d’erreur ou d’avertissement spécifiques lorsqu’un appel de fonction a retourné SQL_ERROR ou SQL_SUCCESS_WITH_INFO (ou SQL_NEED_DATA pour la fonction **SQLBrowseConnect** ).  
  
2.  Pour déterminer le nombre de lignes dans la source de données qui ont été affectées lorsque des opérations d’insertion, de suppression ou de mise à jour ont été effectuées avec un appel à **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos** (à partir du champ d’en-tête SQL_DIAG_ROW_COUNT), ou pour déterminer le nombre de lignes qui existent dans le curseur ouvert actuel, si le pilote peut fournir ces informations (à SQL_DIAG_CURSOR_ROW_COUNT partir  
  
3.  Pour déterminer la fonction qui a été exécutée par un appel à **SQLExecDirect** ou **SQLExecute** (à partir des champs d’en-tête SQL_DIAG_DYNAMIC_FUNCTION et SQL_DIAG_DYNAMIC_FUNCTION_CODE).  
  
 Toute fonction ODBC peut poster zéro, un ou plusieurs enregistrements de diagnostic chaque fois qu’elle est appelée, de sorte qu’une application peut appeler **SQLGetDiagField** après n’importe quel appel de fonction ODBC. Le nombre d’enregistrements de diagnostic pouvant être stockés à un moment donné n’est pas limité. **SQLGetDiagField** récupère uniquement les informations de diagnostic récemment associées à la structure de données de diagnostic spécifiée dans l’argument de *handle* . Si l’application appelle une fonction ODBC autre que **SQLGetDiagField** ou **SQLGetDiagRec**, toutes les informations de diagnostic d’un appel précédent avec le même handle sont perdues.  
  
 Une application peut analyser tous les enregistrements de diagnostic en incrémentant *recnumber*, à condition que **SQLGetDiagField** retourne SQL_SUCCESS. Le nombre d’enregistrements d’État est indiqué dans le champ d’en-tête SQL_DIAG_NUMBER. Les appels à **SQLGetDiagField** sont non destructifs pour les champs d’en-tête et d’enregistrement. L’application peut appeler **SQLGetDiagField** à nouveau ultérieurement pour récupérer un champ d’un enregistrement, tant qu’une fonction autre que les fonctions de diagnostic n’a pas été appelée en attente, ce qui représenterait les enregistrements sur le même handle.  
  
 Une application peut appeler **SQLGetDiagField** pour retourner n’importe quel champ de diagnostic à tout moment, à l’exception de SQL_DIAG_CURSOR_ROW_COUNT ou SQL_DIAG_ROW_COUNT, qui retournera SQL_ERROR si *handle* n’est pas un descripteur d’instruction. Si un autre champ de diagnostic n’est pas défini, l’appel à **SQLGetDiagField** retourne SQL_SUCCESS (à condition qu’aucun autre diagnostic n’ait été détecté) et qu’une valeur non définie soit retournée pour le champ.  
  
 Pour plus d’informations, consultez [utilisation de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) et [implémentation de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 L’appel d’une API autre que celle qui est exécutée de manière asynchrone génère HY010 « erreur de séquence de fonction ». Toutefois, l’enregistrement d’erreur ne peut pas être récupéré avant la fin de l’opération asynchrone.  
  
## <a name="handletype-argument"></a>Argument comme HandleType  
 Chaque type de handle peut être associé à des informations de diagnostic. L’argument *comme HandleType* indique le type de handle du *descripteur*.  
  
 Certains champs d’en-tête et d’enregistrement ne peuvent pas être renvoyés pour les handles d’environnement, de connexion, d’instruction et de descripteur. Les descripteurs pour lesquels un champ n’est pas applicable sont indiqués dans les sections champs d’en-tête et champs d’enregistrement ci-après.  
  
 Si *comme HandleType* a la valeur SQL_HANDLE_ENV, *descripteur* peut être un handle d’environnement partagé ou non partagé.  
  
 Aucun champ de diagnostic d’en-tête spécifique au pilote ne doit être associé à un handle d’environnement.  
  
 Les seuls champs d’en-tête de diagnostic définis pour un handle de descripteur sont SQL_DIAG_NUMBER et SQL_DIAG_RETURNCODE.  
  
## <a name="diagidentifier-argument"></a>Argument DiagIdentifier  
 Cet argument indique l’identificateur du champ requis à partir de la structure de données de diagnostic. Si *recnumber* est supérieur ou égal à 1, les données du champ décrivent les informations de diagnostic retournées par une fonction. Si *recnumber* a la valeur 0, le champ se trouve dans l’en-tête de la structure de données de diagnostic et contient par conséquent des données relatives à l’appel de fonction qui a retourné les informations de diagnostic, et non pas aux informations spécifiques.  
  
 Les pilotes peuvent définir des champs d’en-tête et d’enregistrement spécifiques au pilote dans la structure de données de diagnostic.  
  
 Une application ODBC 3 *. x* fonctionnant avec un pilote ODBC 2 *. x* pourra appeler **SQLGetDiagField** uniquement avec un argument *DiagIdentifier* de SQL_DIAG_CLASS_ORIGIN, SQL_DIAG_CLASS_SUBCLASS_ORIGIN, SQL_DIAG_CONNECTION_NAME, SQL_DIAG_MESSAGE_TEXT, SQL_DIAG_NATIVE, SQL_DIAG_NUMBER, SQL_DIAG_RETURNCODE, SQL_DIAG_SERVER_NAME ou SQL_DIAG_SQLSTATE. Tous les autres champs de diagnostic retournent SQL_ERROR.  
  
## <a name="header-fields"></a>Champs d’en-tête  
 Les champs d’en-tête répertoriés dans le tableau suivant peuvent être inclus dans l’argument *DiagIdentifier* .  
  
|DiagIdentifier|Type de retour|Retours|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CURSOR_ROW_COUNT|SQLLEN|Ce champ contient le nombre de lignes dans le curseur. Sa sémantique dépend des types d’informations **SQLGetInfo** SQL_DYNAMIC_CURSOR_ATTRIBUTES2, SQL_FORWARD_ONLY_CURSOR_ATTRIBUTES2, SQL_KEYSET_CURSOR_ATTRIBUTES2 et SQL_STATIC_CURSOR_ATTRIBUTES2, qui indiquent les nombres de lignes disponibles pour chaque type de curseur (dans les bits SQL_CA2_CRC_EXACT et SQL_CA2_CRC_APPROXIMATE).<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et seulement après l’appel de **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** . L’appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_CURSOR_ROW_COUNT sur un autre qu’un descripteur d’instruction retourne SQL_ERROR.|  
|SQL_DIAG_DYNAMIC_FUNCTION|SQLCHAR|Il s’agit d’une chaîne qui décrit l’instruction SQL exécutée par la fonction sous-jacente. (Consultez « valeurs des champs de fonction dynamique », plus loin dans cette section, pour obtenir des valeurs spécifiques.) Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et uniquement après un appel à **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults**. L’appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_DYNAMIC_FUNCTION sur un autre qu’un descripteur d’instruction retourne SQL_ERROR. La valeur de ce champ est non définie avant un appel à **SQLExecute** ou à **SQLExecDirect**.|  
|SQL_DIAG_DYNAMIC_FUNCTION_CODE|SQLINTEGER|Il s’agit d’un code numérique qui décrit l’instruction SQL exécutée par la fonction sous-jacente. (Consultez « valeurs des champs de fonction dynamique », plus loin dans cette section, pour obtenir une valeur spécifique.) Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction et uniquement après un appel à **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults**. L’appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_DYNAMIC_FUNCTION_CODE sur un autre qu’un descripteur d’instruction retourne SQL_ERROR. La valeur de ce champ est non définie avant un appel à **SQLExecute** ou à **SQLExecDirect**.|  
|SQL_DIAG_NUMBER|SQLINTEGER|Nombre d’enregistrements d’état disponibles pour le handle spécifié.|  
|SQL_DIAG_RETURNCODE|SQLRETURN|Code de retour retourné par la fonction. Pour obtenir la liste des codes de retour, consultez [codes de retour](../../../odbc/reference/develop-app/return-codes-odbc.md). Le pilote n’a pas besoin d’implémenter SQL_DIAG_RETURNCODE ; Il est toujours implémenté par le gestionnaire de pilotes. Si aucune fonction n’a encore été appelée sur le *handle*, SQL_SUCCESS est retourné pour SQL_DIAG_RETURNCODE.|  
|SQL_DIAG_ROW_COUNT|SQLLEN|Nombre de lignes affectées par une insertion, une suppression ou une mise à jour effectuée par **SQLExecute**, **SQLExecDirect**, **SQLBulkOperations** ou **SQLSetPos**. Elle est définie par le pilote après l’exécution d’une *spécification de curseur* . Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction. L’appel de **SQLGetDiagField** avec un *DiagIdentifier* de SQL_DIAG_ROW_COUNT sur un autre qu’un descripteur d’instruction retourne SQL_ERROR. Les données de ce champ sont également retournées dans l’argument *RowCountPtr* de **SQLRowCount**. Les données de ce champ sont réinitialisées après chaque appel de fonction non-diagnostic, tandis que le nombre de lignes retourné par **SQLRowCount** reste le même jusqu’à ce que l’instruction soit rétablie à l’État prepared ou allocated.|  
  
## <a name="record-fields"></a>Champs d’enregistrement  
 Les champs d’enregistrement répertoriés dans le tableau suivant peuvent être inclus dans l’argument *DiagIdentifier* .  
  
|DiagIdentifier|Type de retour|Retours|  
|--------------------|-----------------|-------------|  
|SQL_DIAG_CLASS_ORIGIN|SQLCHAR|Chaîne qui indique le document qui définit la partie classe de la valeur SQLSTATE dans cet enregistrement. Sa valeur est « ISO 9075 » pour toutes les valeurs SQLSTATE définies par Open Group et l’interface ISO au niveau de l’appel. Pour les valeurs SQLSTATE spécifiques à ODBC (toutes celles dont la classe SQLSTATE est « IM »), sa valeur est « ODBC 3,0 ».|  
|SQL_DIAG_COLUMN_NUMBER|SQLINTEGER|Si le champ SQL_DIAG_ROW_NUMBER est un numéro de ligne valide dans un ensemble de lignes ou un ensemble de paramètres, ce champ contient la valeur qui représente le numéro de colonne dans le jeu de résultats ou le numéro de paramètre dans le jeu de paramètres. Les numéros de colonne du jeu de résultats commencent toujours à 1 ; Si cet enregistrement d’État appartient à une colonne de signets, le champ peut être égal à zéro. Les numéros de paramètres commencent à 1. Elle a la valeur SQL_NO_COLUMN_NUMBER si l’enregistrement d’État n’est pas associé à un numéro de colonne ou à un numéro de paramètre. Si le pilote ne peut pas déterminer le numéro de colonne ou le numéro de paramètre auquel cet enregistrement est associé, ce champ a la valeur SQL_COLUMN_NUMBER_UNKNOWN.<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction.|  
|SQL_DIAG_CONNECTION_NAME|SQLCHAR|Chaîne qui indique le nom de la connexion à laquelle l’enregistrement de diagnostic est associé. Ce champ est défini par le pilote. Pour les structures de données de diagnostic associées au handle d’environnement et pour les diagnostics qui ne sont associées à aucune connexion, ce champ est une chaîne de longueur nulle.|  
|SQL_DIAG_MESSAGE_TEXT|SQLCHAR|Message d’information sur l’erreur ou l’avertissement. Ce champ est mis en forme comme décrit dans la section [messages de diagnostic](../../../odbc/reference/develop-app/diagnostic-messages.md). Il n’y a pas de longueur maximale pour le texte du message de diagnostic.|  
|SQL_DIAG_NATIVE|SQLINTEGER|Code d’erreur natif propre à la source de données/pilote. S’il n’existe aucun code d’erreur natif, le pilote retourne 0.|  
|SQL_DIAG_ROW_NUMBER|SQLLEN|Ce champ contient le numéro de ligne dans l’ensemble de lignes, ou le numéro de paramètre dans le jeu de paramètres, avec lequel l’enregistrement d’État est associé. Les numéros de ligne et les numéros de paramètres commencent par 1. Ce champ a la valeur SQL_NO_ROW_NUMBER si cet enregistrement d’État n’est pas associé à un numéro de ligne ou un numéro de paramètre. Si le pilote ne peut pas déterminer le numéro de ligne ou le numéro de paramètre auquel cet enregistrement est associé, la valeur de ce champ est SQL_ROW_NUMBER_UNKNOWN.<br /><br /> Le contenu de ce champ est défini uniquement pour les descripteurs d’instruction.|  
|SQL_DIAG_SERVER_NAME|SQLCHAR|Chaîne qui indique le nom du serveur auquel l’enregistrement de diagnostic est associé. Il est identique à la valeur retournée pour un appel à **SQLGetInfo** avec l’option SQL_DATA_SOURCE_NAME. Pour les structures de données de diagnostic associées au handle d’environnement et pour les diagnostics qui ne sont associées à aucun serveur, ce champ est une chaîne de longueur nulle.|  
|SQL_DIAG_SQLSTATE|SQLCHAR|Code de diagnostic SQLSTATE à cinq caractères. Pour plus d’informations, consultez [sqlstates](../../../odbc/reference/develop-app/sqlstates.md).|  
|SQL_DIAG_SUBCLASS_ORIGIN|SQLCHAR|Chaîne avec le même format et les mêmes valeurs valides que SQL_DIAG_CLASS_ORIGIN, qui identifie la partie de définition de la partie sous-classe du code SQLSTATE. Les SQLSTATEs spécifiques à ODBC pour lesquelles « ODBC 3,0 » est retourné sont les suivantes :<br /><br /> 01S00, 01S01, 01S02 NE, 01S06, 01S07, 07S01, 08S01, 21S01, 21S02, 25S01, 25S02, 25S03, 42S01, 42S02, 42S11, 42S12, 42S21, 42S22, HY095, HY097, HY098, HY099, HY100, HY101, HY105, HY107, HY109, HY110, HY111, HYT00, HYT01, IM001, IM002, IM003, IM004, IM005, IM006, IM007, IM008, IM010, IM011, IM012.|  
  
## <a name="values-of-the-dynamic-function-fields"></a>Valeurs des champs de fonction dynamique  
 Le tableau suivant décrit les valeurs de SQL_DIAG_DYNAMIC_FUNCTION et SQL_DIAG_DYNAMIC_FUNCTION_CODE qui s’appliquent à chaque type d’instruction SQL exécutée par un appel à **SQLExecute** ou **SQLExecDirect**. Le pilote peut ajouter des valeurs définies par le pilote à celles listées.  
  
|Instruction SQL<br /><br /> effectuées|Valeur de <br /><br /> SQL_DIAG_DYNAMIC_FUNCTION|Valeur de <br /><br /> SQL_DIAG_DYNAMIC_FUNCTION_CODE|  
|--------------------------------|-----------------------------------------------|-----------------------------------------------------|  
|*ALTER-Domain-Statement*|« ALTER DOMAIN »|SQL_DIAG_ALTER_DOMAIN|  
|*ALTER-TABLE-Statement*|« ALTER TABLE »|SQL_DIAG_ALTER_TABLE|  
|*assertion-définition*|« CRÉER UNE ASSERTION »|SQL_DIAG_CREATE_ASSERTION|  
|*définition de jeu de caractères*|« CRÉER UN JEU DE CARACTÈRES »|SQL_DIAG_CREATE_CHARACTER_SET|  
|*classement-définition*|« CRÉER UN CLASSEMENT »|SQL_DIAG_CREATE_COLLATION|  
|*définition de domaine*|« CRÉER UN DOMAINE »|SQL_DIAG_CREATE_DOMAIN|
|*instruction CREATE-index*|« CRÉER UN INDEX »|SQL_DIAG_CREATE_INDEX|  
|*instruction CREATE-TABLE*|« CREATE TABLE »|SQL_DIAG_CREATE_TABLE|  
|*Create-View-Statement*|« CRÉER UNE VUE »|SQL_DIAG_CREATE_VIEW|  
|*spécification de curseur*|« SÉLECTIONNER UN CURSEUR »|SQL_DIAG_SELECT_CURSOR|  
|*Delete-instruction-positionnée*|« CURSEUR DE SUPPRESSION DYNAMIQUE »|SQL_DIAG_DYNAMIC_DELETE_CURSOR|  
|*instruction DELETE-recherchée*|« SUPPRIMER OÙ »|SQL_DIAG_DELETE_WHERE|  
|*Drop-assertion-Statement*|« DROP ASSERTION »|SQL_DIAG_DROP_ASSERTION|  
|*Drop-Character-Set-stmt*|« SUPPRIMER LE JEU DE CARACTÈRES »|SQL_DIAG_DROP_CHARACTER_SET|  
|*Drop-collation-Statement*|« SUPPRIMER LE CLASSEMENT »|SQL_DIAG_DROP_COLLATION|  
|*Drop-Domain-Statement*|« SUPPRIMER UN DOMAINE »|SQL_DIAG_DROP_DOMAIN|  
|*Drop-Index-Statement*|« DROP INDEX »|SQL_DIAG_DROP_INDEX|  
|*Drop-Schema-Statement*|« SUPPRIMER LE SCHÉMA »|SQL_DIAG_DROP_SCHEMA|  
|*drop-table-Statement*|« DROP TABLE »|SQL_DIAG_DROP_TABLE|  
|*Drop-translation-Statement*|« DROP TRANSLATION »|SQL_DIAG_DROP_TRANSLATION|  
|*Drop-View-Statement*|« DROP VIEW »|SQL_DIAG_DROP_VIEW|  
|*grantstatement*|LICENCE|SQL_DIAG_GRANT|
|*instruction INSERT*|Insérer|SQL_DIAG_INSERT|  
|*ODBC-Procedure-extension*|INVOQU|APPEL SQL_DIAG_|  
|*Revoke-Statement*|SUPPRIMÉS|SQL_DIAG_REVOKE|  
|*définition de schéma*|« CRÉER UN SCHÉMA »|SQL_DIAG_CREATE_SCHEMA|  
|*traduction-définition*|« CRÉER UNE TRADUCTION »|SQL_DIAG_CREATE_TRANSLATION|  
|*mise à jour-instruction-positionnée*|« CURSEUR DE MISE À JOUR DYNAMIQUE »|SQL_DIAG_DYNAMIC_UPDATE_CURSOR|  
|*Update-instruction-recherchée*|« METTRE À JOUR OÙ »|SQL_DIAG_UPDATE_WHERE|  
|Unknown|*chaîne vide*|SQL_DIAG_UNKNOWN_STATEMENT|  

<!--
These two malformed table rows were fixed by educated GUESS only.
Each pair starts with the original flawed row.
Flawed because treated as only two cells by HTML render,
and because missing info anyway.
Also, these flawed rows lacked '|' as their first nonWhitespace character (although markdown technically allows this omission, unfortunately).
Arguably the following SQL.H file shows the sequence of the flawed rows in the table was suboptimal also.

ftp://www.fpc.org/fpc32/VS6Disk1/VC98/INCLUDE/SQL.H

GeneMi , 2019/01/19
- - - - - - - - - - - - - -

n-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|  

|*domain-definition*|"CREATE DOMAIN"|SQL_DIAG_CREATE_DOMAIN|

-statement*|"GRANT"|SQL_DIAG_GRANT|  

|*grant-statement*|"GRANT"|SQL_DIAG_GRANT|

-->

## <a name="sequence-of-status-records"></a>Séquence d’enregistrements d’état

 Les enregistrements d’État sont positionnés dans une séquence basée sur le numéro de ligne et le type de diagnostic. Le gestionnaire de pilotes détermine l’ordre final dans lequel retourner les enregistrements d’État qu’il génère. Le pilote détermine l’ordre final dans lequel retourner les enregistrements d’État qu’il génère.  
  
 Si les enregistrements de diagnostic sont publiés à la fois par le gestionnaire de pilotes et par le pilote, le gestionnaire de pilotes est chargé de les classer.  
  
 S’il y a deux ou plusieurs enregistrements d’État, la séquence des enregistrements est déterminée en premier par numéro de ligne. Les règles suivantes s’appliquent à la détermination de la séquence d’enregistrements de diagnostic par ligne :  
  
-   Les enregistrements qui ne correspondent à aucune ligne apparaissent devant les enregistrements qui correspondent à une ligne particulière, car SQL_NO_ROW_NUMBER est défini sur-1.  
  
-   Les enregistrements pour lesquels le numéro de ligne est inconnu apparaissent devant tous les autres enregistrements, car SQL_ROW_NUMBER_UNKNOWN est défini sur-2.  
  
-   Pour tous les enregistrements qui se rapportent à des lignes spécifiques, les enregistrements sont triés selon la valeur du champ SQL_DIAG_ROW_NUMBER. Toutes les erreurs et tous les avertissements de la première ligne affectée sont répertoriés, puis toutes les erreurs et tous les avertissements de la ligne suivante sont affectés, et ainsi de suite.  
  
> [!NOTE]
>  Le gestionnaire de pilotes ODBC 3 *. x* ne commande pas les enregistrements d’État dans la file d’attente de diagnostic si SQLSTATE 01S01 (erreur dans la ligne) est retourné par un pilote ODBC 2 *. x* ou si SQLSTATE 01S01 (erreur dans la ligne) est retourné par un pilote ODBC 3 *. x* quand **SQLExtendedFetch** est appelé ou que **SQLSetPos** est appelé sur un curseur positionné avec **SQLExtendedFetch**.  
  
 Dans chaque ligne, ou pour tous les enregistrements qui ne correspondent pas à une ligne ou pour lesquels le numéro de ligne est inconnu, ou pour tous les enregistrements dont le numéro de ligne est égal à SQL_NO_ROW_NUMBER, le premier enregistrement indiqué est déterminé à l’aide d’un ensemble de règles de tri. Après le premier enregistrement, l’ordre des autres enregistrements affectant une ligne n’est pas défini. Une application ne peut pas supposer que les erreurs précèdent les avertissements après le premier enregistrement. Les applications doivent analyser l’intégralité de la structure de données de diagnostic pour obtenir des informations complètes sur l’échec d’un appel à une fonction.  
  
 Les règles suivantes sont utilisées pour déterminer le premier enregistrement au sein d’une ligne. L’enregistrement avec le rang le plus élevé est le premier enregistrement. La source d’un enregistrement (gestionnaire de pilotes, pilote, passerelle, etc.) n’est pas prise en compte lors du classement des enregistrements.  
  
-   **Erreurs** Les enregistrements d’État qui décrivent les erreurs ont le rang le plus élevé. Les règles suivantes sont appliquées aux erreurs de tri :  
  
    -   Les enregistrements qui indiquent un échec de transaction ou un échec de transaction possible dérangent tous les autres enregistrements.  
  
    -   Si deux ou plusieurs enregistrements décrivent la même condition d’erreur, alors les SQLSTATEs définis par la spécification de l’interface CLI de groupe ouverte (classes 03 à HZ) dérangent les SQLSTATEs définies par ODBC et par le pilote.  
  
-   **Aucune valeur de données définie par l’implémentation** Les enregistrements d’État qui décrivent les valeurs de données non définies par le pilote (classe 02) ont le deuxième rang le plus élevé.  
  
-   **Avertissements** Les enregistrements d’État qui décrivent les avertissements (classe 01) ont le rang le plus bas. Si au moins deux enregistrements décrivent la même condition d’avertissement, les SQLSTATE d’avertissement définies par la spécification CLI de groupe ouverte sont prioritaires par rapport aux SQLSTATEs définies par ODBC et par le pilote.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention de plusieurs champs d’une structure de données de diagnostic|[SQLGetDiagRec, fonction](sqlgetdiagrec-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
