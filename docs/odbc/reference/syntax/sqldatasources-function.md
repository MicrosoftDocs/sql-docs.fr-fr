---
description: SQLDataSources, fonction
title: SQLDataSources fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLDataSources
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLDataSources
helpviewer_keywords:
- SQLDataSources function [ODBC]
ms.assetid: 3f63b1b4-e70e-44cd-96c6-6878d50d0117
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f22039a9e5f1443f828523d67a4cfa6c10496b23
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99206101"
---
# <a name="sqldatasources-function"></a>SQLDataSources, fonction
**Conformité**  
 Version introduite : ODBC 1,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLDataSources** retourne des informations sur une source de données. Cette fonction est implémentée uniquement par le gestionnaire de pilotes.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLDataSources(  
     SQLHENV          EnvironmentHandle,  
     SQLUSMALLINT     Direction,  
     SQLCHAR *        ServerName,  
     SQLSMALLINT      BufferLength1,  
     SQLSMALLINT *    NameLength1Ptr,  
     SQLCHAR *        Description,  
     SQLSMALLINT      BufferLength2,  
     SQLSMALLINT *    NameLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 Entrée Handle d’environnement.  
  
 *Sens*  
 Entrée Détermine la source de données à propos de laquelle le gestionnaire de pilotes retourne des informations. Valeurs possibles :  
  
 SQL_FETCH_NEXT (pour extraire le nom de source de données suivant dans la liste), SQL_FETCH_FIRST (pour extraire à partir du début de la liste), SQL_FETCH_FIRST_USER (pour extraire le premier DSN utilisateur) ou SQL_FETCH_FIRST_SYSTEM (pour extraire le premier DSN système).  
  
 Lorsque la *direction* est définie sur SQL_FETCH_FIRST, les appels suivants à **SQLDataSources** avec la *direction* définie sur SQL_FETCH_NEXT retournent à la fois les noms DSN utilisateur et système. Lorsque la *direction* est définie sur SQL_FETCH_FIRST_USER, tous les appels suivants à **SQLDataSources** avec la *direction* définie sur SQL_FETCH_NEXT retourner uniquement les DSN utilisateur. Lorsque la *direction* est définie sur SQL_FETCH_FIRST_SYSTEM, tous les appels suivants à **SQLDataSources** avec la *direction* définie sur SQL_FETCH_NEXT retourner uniquement les noms de sources système.  
  
 *ServerName*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nom de la source de données.  
  
 Si *ServerName* a la valeur null, *NameLength1Ptr* renvoie toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *ServerName*.  
  
 *BufferLength1*  
 Entrée Longueur de la mémoire tampon du *serveur* *, en caractères ; Cela ne doit pas être plus long que SQL_MAX_DSN_LENGTH plus le caractère de fin null.  
  
 *NameLength1Ptr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans \* *ServerName*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength1*, le nom de la source de données dans \* *ServerName* est tronqué à *BufferLength1* moins la longueur d’un caractère de fin null.  
  
 *Description*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la description du pilote associé à la source de données. Par exemple, dBASE ou SQL Server.  
  
 Si *Description* a la valeur null, *NameLength2Ptr* renvoie toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe la *Description*.  
  
 *BufferLength2*  
 Entrée Longueur en caractères de la mémoire tampon de *Description* *.  
  
 *NameLength2Ptr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (à l’exception du caractère de fin null) disponibles à retourner dans la \* *Description*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength2*, la description du pilote dans \* *Description* est tronquée à *BufferLength2* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NO_DATA, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLDataSources** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_ENV et un *handle* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLDataSources** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au gestionnaire de pilotes (DM). (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|(DM) la mémoire tampon \* *ServerName* n’était pas suffisamment grande pour retourner le nom complet de la source de données. Par conséquent, le nom a été tronqué. La longueur de l’intégralité du nom de la source de données est retournée dans \* *NameLength1Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)<br /><br /> (DM) la description de la mémoire tampon \*  n’est pas assez grande pour retourner la description complète du pilote. Par conséquent, la description a été tronquée. La longueur de la description de la source de données non tronquée est retournée dans **NameLength2Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|(DM) une erreur s’est produite pour laquelle il n’existait pas de SQLSTATE spécifique et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|(DM) le gestionnaire de pilotes n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY010|Erreur de séquence de fonction|(DM) **SQLExecute**, **SQLExecDirect** ou **SQLMoreResults** a été appelé pour *StatementHandle* et a retourné SQL_PARAM_DATA_AVAILABLE. Cette fonction a été appelée avant que les données ne soient récupérées pour tous les paramètres transmis en continu.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *BufferLength1* est inférieure à 0.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength2* est inférieure à 0.|  
|HY103|Code de récupération non valide|(DM) la valeur spécifiée pour la *direction* de l’argument n’est pas égale à SQL_FETCH_FIRST, SQL_FETCH_FIRST_USER, SQL_FETCH_FIRST_SYSTEM ou SQL_FETCH_NEXT.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
  
## <a name="comments"></a>Commentaires  
 Étant donné que **SQLDataSources** est implémenté dans le gestionnaire de pilotes, il est pris en charge pour tous les pilotes, quelle que soit la conformité des normes d’un pilote particulier.  
  
 Une application peut appeler **SQLDataSources** plusieurs fois pour récupérer tous les noms de sources de données. Le gestionnaire de pilotes récupère ces informations à partir des informations système. Lorsqu’il n’y a plus de noms de sources de données, le gestionnaire de pilotes retourne SQL_NO_DATA. Si **SQLDataSources** est appelé avec SQL_FETCH_NEXT immédiatement après avoir retourné SQL_NO_DATA, le premier nom de source de données est retourné. Pour plus d’informations sur la façon dont une application utilise les informations retournées par **SQLDataSources**, consultez [choix d’une source de données ou d’un pilote](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
 Si SQL_FETCH_NEXT est passé à **SQLDataSources** la première fois qu’il est appelé, le premier nom de source de données est retourné.  
  
 Le pilote détermine la façon dont les noms des sources de données sont mappés à des sources de données réelles.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Détection et affichage des valeurs requises pour se connecter à une source de données|[Fonction SQLBrowseConnect](../../../odbc/reference/syntax/sqlbrowseconnect-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Connexion à une source de données à l’aide d’une chaîne de connexion ou d’une boîte de dialogue|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Renvoi des descriptions et des attributs des pilotes|[SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
