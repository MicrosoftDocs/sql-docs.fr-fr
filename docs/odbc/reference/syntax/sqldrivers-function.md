---
description: SQLDrivers, fonction
title: SQLDrivers fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDrivers
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDrivers
helpviewer_keywords:
- SQLDrivers function [ODBC]
ms.assetid: 6b5b7514-e9cb-4cfd-8b7a-ab51dfab9efa
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 905dfe1bc24872468868abef9f1c8cc2323d110a
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99195618"
---
# <a name="sqldrivers-function"></a>SQLDrivers, fonction
**Conformité**  
 Version introduite : ODBC 2,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLDrivers** répertorie les descriptions des pilotes et les mots clés des attributs du pilote. Cette fonction est implémentée uniquement par le gestionnaire de pilotes.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLDrivers(  
     SQLHENV         EnvironmentHandle,  
     SQLUSMALLINT    Direction,  
     SQLCHAR *       DriverDescription,  
     SQLSMALLINT     BufferLength1,  
     SQLSMALLINT *   DescriptionLengthPtr,  
     SQLCHAR *       DriverAttributes,  
     SQLSMALLINT     BufferLength2,  
     SQLSMALLINT *   AttributesLengthPtr);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 Entrée Handle d’environnement.  
  
 *Sens*  
 Entrée Détermine si le gestionnaire de pilotes extrait la description suivante du pilote dans la liste (SQL_FETCH_NEXT) ou si la recherche commence au début de la liste (SQL_FETCH_FIRST).  
  
 *DriverDescription*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la description du pilote.  
  
 Si *DriverDescription* a la valeur null, *DescriptionLengthPtr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *DriverDescription*.  
  
 *BufferLength1*  
 Entrée Longueur de la mémoire tampon **DriverDescription* , en caractères.  
  
 *DescriptionLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans \* *DriverDescription*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength1*, la description du pilote dans \* *DriverDescription* est tronquée à *BufferLength1* moins la longueur d’un caractère de fin null.  
  
 *DriverAttributes*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la liste des paires de valeurs d’attribut de pilote (consultez « Comments »).  
  
 Si *DriverAttributes* a la valeur null, *AttributesLengthPtr* retourne toujours le nombre total d’octets (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *DriverAttributes*.  
  
 *BufferLength2*  
 Entrée Longueur de la \* mémoire tampon *DriverAttributes* , en caractères. Si la valeur *\* DriverDescription* est une chaîne Unicode (lors de l’appel de **SQLDriversW**), l’argument *BufferLength* doit être un nombre pair.  
  
 *AttributesLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total d’octets (à l’exclusion de l’octet de fin null) disponibles à retourner dans \* *DriverAttributes*. Si le nombre d’octets disponibles à retourner est supérieur ou égal à *BufferLength2*, la liste des paires de valeurs d’attributs dans \* *DriverAttributes* est tronquée à *BufferLength2* moins la longueur du caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLDrivers** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_ENV et un *handle* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDrivers** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au gestionnaire de pilotes (DM). (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|(DM) la mémoire tampon \* *DriverDescription* n’était pas suffisamment grande pour retourner la description complète du pilote. Par conséquent, la description a été tronquée. La longueur de la description complète du pilote est retournée dans \* *DescriptionLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> (DM) la mémoire tampon \* *DriverAttributes* n’était pas suffisamment grande pour retourner la liste complète des paires valeur-attribut. Par conséquent, la liste a été tronquée. La longueur de la liste non tronquée des paires valeur d’attribut est retournée dans **AttributesLengthPtr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|(DM) le gestionnaire de pilotes n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength1* est inférieure à 0.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength2* est inférieure à 0 ou égale à 1.|  
|HY103|Code de récupération non valide|(DM) la valeur spécifiée pour la *direction* de l’argument n’est pas égale à SQL_FETCH_FIRST ou SQL_FETCH_NEXT.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Commentaires  
 **SQLDrivers** retourne la description du pilote dans la \* mémoire tampon *DriverDescription* . Elle retourne des informations supplémentaires sur le pilote dans la \* mémoire tampon *DriverAttributes* sous la forme d’une liste de paires mot clé-valeur. Tous les mots clés figurant dans les informations système pour les pilotes sont renvoyés pour tous les pilotes, à l’exception de **CreateDSN**, qui est utilisé pour demander la création de sources de données et est donc facultatif. Chaque paire se termine par un octet NULL, et la liste complète se termine par un octet Null (autrement dit, deux octets de valeur null marquent la fin de la liste). Par exemple, un pilote basé sur des fichiers utilisant la syntaxe C peut retourner la liste d’attributs suivante (« \ 0 » représente un caractère null) :  
  
```  
FileUsage=1\0FileExtns=*.dbf\0\0  
```  
  
 Si \* *DriverAttributes* n’est pas suffisamment grand pour contenir la totalité de la liste, la liste est tronquée, **SQLDrivers** retourne SQLState 01004 (données tronquées) et la longueur de la liste (à l’exception de l’octet final de fin null) est retournée dans **AttributesLengthPtr*.  
  
 Les mots clés d’attribut de pilote sont ajoutés à partir des informations système lorsque le pilote est installé. Pour plus d’informations, consultez [Installing ODBC Components](../../../odbc/reference/install/installing-odbc-components.md).  
  
 Une application peut appeler **SQLDrivers** plusieurs fois pour récupérer toutes les descriptions des pilotes. Le gestionnaire de pilotes récupère ces informations à partir des informations système. Lorsqu’il n’y a plus de description du pilote, **SQLDrivers** retourne SQL_NO_DATA. Si **SQLDrivers** est appelé avec SQL_FETCH_NEXT immédiatement après avoir retourné SQL_NO_DATA, il retourne la première description du pilote. Pour plus d’informations sur la façon dont une application utilise les informations retournées par **SQLDrivers**, consultez [choix d’une source de données ou d’un pilote](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si SQL_FETCH_NEXT est passé à **SQLDrivers** la première fois qu’il est appelé, **SQLDrivers** retourne le premier nom de la source de données.  
  
 Étant donné que **SQLDrivers** est implémenté dans le gestionnaire de pilotes, il est pris en charge pour tous les pilotes, quelle que soit la conformité des normes d’un pilote particulier.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Détection et affichage des valeurs requises pour se connecter à une source de données|[Fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Retour de noms de sources de données|[SQLDataSources, fonction](../../../odbc/reference/syntax/sqldatasources-function.md)|  
|Connexion à une source de données à l’aide d’une chaîne de connexion ou d’une boîte de dialogue|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
