---
description: Fonction SQLSetEnvAttr
title: SQLSetEnvAttr fonction) | Microsoft Docs
ms.custom: ''
ms.date: 07/18/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLSetEnvAttr
apilocation:
- sqlsrv32.dll
- odbc32.dll
apitype: dllExport
f1_keywords:
- SQLSetEnvAttr
helpviewer_keywords:
- SQLSetEnvAttr function [ODBC]
ms.assetid: 0343241c-4b15-4d4b-aa2b-2e8ab5215cd2
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: ed611f343275c2d777d4f390154b160aa6346094
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99200957"
---
# <a name="sqlsetenvattr-function"></a>Fonction SQLSetEnvAttr
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLSetEnvAttr** définit les attributs qui régissent les aspects des environnements.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```  
  
## <a name="arguments"></a>Arguments  
 *EnvironmentHandle*  
 Entrée Handle d’environnement.  
  
 *Attribut*  
 Entrée Attribut à définir, indiqué dans « Comments ».  
  
 *ValuePtr*  
 Entrée Pointeur vers la valeur à associer à l' *attribut*. Selon la valeur de l' *attribut*, *ValuePtr* est une valeur entière de 32 bits ou pointe vers une chaîne de caractères se terminant par un caractère null.  
  
 *StringLength*  
 Entrée Si *ValuePtr* pointe vers une chaîne de caractères ou une mémoire tampon binaire, cet argument doit être la longueur de **ValuePtr*. Pour les données de type chaîne de caractères, cet argument doit contenir le nombre d’octets dans la chaîne.  
  
 Si *ValuePtr* est un entier, *StringLength* est ignoré.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLSetEnvAttr** retourne SQL_ERROR ou SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_ENV et un *handle* de *EnvironmentHandle*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLSetEnvAttr** et explique chacune d’elles dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire. Si un pilote ne prend pas en charge un attribut d’environnement, l’erreur peut être retournée uniquement pendant le temps de connexion.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01S02 ne|Valeur d’option modifiée|Le pilote ne prenait pas en charge la valeur spécifiée dans *ValuePtr* et substituait une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|L’argument d’attribut a identifié un attribut d’environnement qui nécessitait une valeur de chaîne et l’argument *ValuePtr* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) un descripteur de connexion a été alloué sur *EnvironmentHandle*.<br /><br /> (DM) **SQL_ATTR_ODBC_VERSION** n’a pas été défini avec **SQLSetEnvAttr** et l' *attribut* n’est pas égal à **SQL_ATTR_ODBC_VERSION**. Vous n’avez pas besoin de définir **SQL_ATTR_ODBC_VERSION** explicitement si vous utilisez **SQLAllocHandleStd**.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY024|Valeur d’attribut non valide|À partir de la valeur d' *attribut* spécifiée, une valeur non valide a été spécifiée dans *ValuePtr*.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|L’argument *StringLength* est inférieur à 0 mais n’a pas été SQL_NTS.|  
|HY092|Identificateur d’attribut/option non valide|(DM) la valeur spécifiée pour l' *attribut* argument n’était pas valide pour la version de ODBC prise en charge par le pilote.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYC00|Fonctionnalité facultative non implémentée|La valeur spécifiée pour l' *attribut* argument était un attribut d’environnement ODBC valide pour la version de ODBC prise en charge par le pilote, mais n’était pas pris en charge par le pilote.<br /><br /> (DM) l’argument d' *attribut* a été SQL_ATTR_OUTPUT_NTS, et *ValuePtr* a été SQL_FALSE.|  
  
## <a name="comments"></a>Commentaires  
 Une application peut appeler **SQLSetEnvAttr** uniquement si aucun handle de connexion n’est alloué sur l’environnement. Tous les attributs d’environnement définis avec succès par l’application pour l’environnement persistent jusqu’à ce que **SQLFreeHandle** soit appelé sur l’environnement. Plusieurs descripteurs d’environnement peuvent être alloués simultanément dans ODBC *3. x*.  
  
 Le format des informations définies via *ValuePtr* dépend de l' *attribut* spécifié. **SQLSetEnvAttr** accepte les informations d’attribut dans l’un des deux formats suivants : une chaîne de caractères se terminant par un caractère null ou une valeur entière de 32 bits. Le format de chaque est indiqué dans la description de l’attribut.  
  
 Il n’existe aucun attribut d’environnement propre au pilote.  
  
 Les attributs de connexion ne peuvent pas être définis par un appel à **SQLSetEnvAttr**. Si vous tentez cette opération, la commande SQLSTATE HY092 (identificateur d’attribut/option non valide) est retournée.  
  
|*Attribut*|Contenu *ValuePtr*|  
|-----------------|-------------------------|  
|SQL_ATTR_CONNECTION_POOLING (ODBC 3,8)|Valeur SQLUINTEGER de 32 bits qui active ou désactive le regroupement de connexions au niveau de l’environnement. Les valeurs suivantes sont utilisées :<br /><br /> SQL_CP_OFF = le regroupement de connexions est désactivé. Il s’agit de la valeur par défaut.<br /><br /> SQL_CP_ONE_PER_DRIVER = un seul pool de connexions est pris en charge pour chaque pilote. Chaque connexion dans un pool est associée à un seul pilote.<br /><br /> SQL_CP_ONE_PER_HENV = un seul pool de connexions est pris en charge pour chaque environnement. Chaque connexion dans un pool est associée à un environnement.<br /><br /> SQL_CP_DRIVER_AWARE = utilisez la fonctionnalité de sensibilisation du pool de connexions du pilote, si elle est disponible. Si le pilote ne prend pas en charge la reconnaissance des pools de connexions, SQL_CP_DRIVER_AWARE est ignoré et SQL_CP_ONE_PER_HENV est utilisé. Pour plus d’informations, consultez [regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md). Dans un environnement où certains pilotes prennent en charge et certains pilotes ne prennent pas en charge la reconnaissance des pools de connexions, SQL_CP_DRIVER_AWARE pouvez activer la fonctionnalité de reconnaissance des pools de connexions sur ces pilotes, mais cela revient à définir sur SQL_CP_ONE_PER_HENV sur les pilotes qui ne prennent pas en charge la fonctionnalité de reconnaissance des pools de connexions.<br /><br /> Le regroupement de connexions est activé en appelant **SQLSetEnvAttr** pour affecter à l’attribut SQL_ATTR_CONNECTION_POOLING la valeur SQL_CP_ONE_PER_DRIVER ou SQL_CP_ONE_PER_HENV. Cet appel doit être effectué avant que l’application n’alloue l’environnement partagé pour lequel le groupement de connexions doit être activé. Le handle d’environnement dans l’appel à **SQLSetEnvAttr** a la valeur null, ce qui rend SQL_ATTR_CONNECTION_POOLING un attribut de niveau processus. Une fois que le regroupement de connexions est activé, l’application alloue ensuite un environnement partagé implicite en appelant **SQLAllocHandle** avec l’argument *InputHandle* défini sur SQL_HANDLE_ENV.<br /><br /> Une fois que le regroupement de connexions a été activé et qu’un environnement partagé a été sélectionné pour une application, SQL_ATTR_CONNECTION_POOLING ne peut pas être réinitialisé pour cet environnement, car **SQLSetEnvAttr** est appelé avec un handle d’environnement null lors de la définition de cet attribut. Si cet attribut est défini alors que le regroupement de connexions est déjà activé sur un environnement partagé, l’attribut affecte uniquement les environnements partagés qui sont alloués par la suite.<br /><br /> Il est également possible d’activer le regroupement de connexions sur un environnement. Notez les points suivants à propos du regroupement de connexions d’environnement :<br /><br /> -L’activation du regroupement de connexions sur un handle NULL est un attribut de niveau processus. Les environnements alloués par la suite seront un environnement partagé et hériteront du paramètre de regroupement de connexions au niveau du processus.<br />-Une fois qu’un environnement est alloué, une application peut toujours modifier son paramètre de pool de connexions.<br />-Si le regroupement de connexions d’environnement est activé et que le pilote de la connexion utilise le regroupement de pilotes, le regroupement d’environnement est préférable.<br /><br /> SQL_ATTR_CONNECTION_POOLING est implémentée dans le gestionnaire de pilotes. Un pilote n’a pas besoin d’implémenter SQL_ATTR_CONNECTION_POOLING. Les applications ODBC 2,0 et 3,0 peuvent définir cet attribut d’environnement.<br /><br /> Pour plus d’informations, consultez [ODBC Connection Pooling](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_CP_MATCH (ODBC 3,0)|Valeur SQLUINTEGER de 32 bits qui détermine le mode de sélection d’une connexion à partir d’un pool de connexions. Lorsque **SQLConnect** ou **SQLDriverConnect** est appelé, le gestionnaire de pilotes détermine quelle connexion est réutilisée à partir du pool. Le gestionnaire de pilotes tente de faire correspondre les options de connexion dans l’appel et les attributs de connexion définis par l’application aux mots clés et aux attributs de connexion des connexions dans le pool. La valeur de cet attribut détermine le niveau de précision des critères de correspondance.<br /><br /> Les valeurs suivantes sont utilisées pour définir la valeur de cet attribut :<br /><br /> SQL_CP_STRICT_MATCH = seules les connexions qui correspondent exactement aux options de connexion dans l’appel et les attributs de connexion définis par l’application sont réutilisés. Il s’agit de la valeur par défaut.<br /><br /> SQL_CP_RELAXED_MATCH = les connexions avec des mots clés de chaîne de connexion correspondants peuvent être utilisées. Les mots clés doivent correspondre, mais tous les attributs de connexion ne doivent pas correspondre.<br /><br /> Pour plus d’informations sur la façon dont le gestionnaire de pilotes effectue la correspondance dans la connexion à une connexion regroupée, consultez [SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Pour plus d’informations sur le regroupement de connexions, consultez la page relative au [regroupement de connexions ODBC](../../../odbc/reference/develop-app/driver-manager-connection-pooling.md).|  
|SQL_ATTR_ODBC_VERSION (ODBC 3,0)|Entier 32 bits qui détermine si certaines fonctionnalités ont un comportement ODBC *2. x* ou un comportement ODBC *3. x* . Les valeurs suivantes sont utilisées pour définir la valeur de cet attribut :<br /><br /> SQL_OV_ODBC3_80 = le gestionnaire de pilotes et le pilote présentent le comportement ODBC 3,8 suivant :<br /><br /> -Le pilote retourne et attend les codes ODBC *3. x* pour la date, l’heure et l’horodateur.<br />-Le pilote retourne des codes SQLSTATE ODBC *3. x* lorsque **SQLError**, **SQLGetDiagField** ou **SQLGetDiagRec** est appelé.<br />-L’argument *nomcatalogue* dans un appel à **SQLTables** accepte un modèle de recherche.<br />-Le gestionnaire de pilotes prend en charge l’extensibilité du type de données C. Pour plus d’informations sur l’extensibilité du type de données C, consultez [types de données c dans ODBC](../../../odbc/reference/develop-app/c-data-types-in-odbc.md).<br /><br /> Pour plus d’informations, consultez [What’s New in ODBC 3,8](../../../odbc/reference/what-s-new-in-odbc-3-8.md).<br /><br /> SQL_OV_ODBC3 = le gestionnaire de pilotes et le pilote présentent le comportement ODBC *3. x* suivant :<br /><br /> -Le pilote retourne et attend les codes ODBC *3. x* pour la date, l’heure et l’horodateur.<br />-Le pilote retourne des codes SQLSTATE ODBC *3. x* lorsque **SQLError**, **SQLGetDiagField** ou **SQLGetDiagRec** est appelé.<br />-L’argument *nomcatalogue* dans un appel à **SQLTables** accepte un modèle de recherche.<br />-Le gestionnaire de pilotes ne prend pas en charge l’extensibilité du type de données C.<br /><br /> SQL_OV_ODBC2 = le gestionnaire de pilotes et le pilote présentent le comportement ODBC *2. x* suivant. Cela s’avère particulièrement utile pour une application ODBC *2. x* fonctionnant avec un pilote ODBC *3. x* .<br /><br /> -Le pilote retourne et attend les codes ODBC *2. x* pour la date, l’heure et l’horodateur.<br />-Le pilote retourne les codes SQLSTATE ODBC *2. x* lorsque **SQLError**, **SQLGetDiagField** ou **SQLGetDiagRec** est appelé.<br />-L’argument *nomcatalogue* dans un appel à **SQLTables** n’accepte pas de modèle de recherche.<br />-Le gestionnaire de pilotes ne prend pas en charge l’extensibilité du type de données C.<br /><br /> Une application doit définir cet attribut d’environnement avant d’appeler une fonction qui a un argument SQLHENV ou l’appel retourne SQLSTATE HY010 (erreur de séquence de fonction). Il s’agit d’un comportement spécifique au pilote, qu’il existe un comportement supplémentaire pour ces indicateurs environnementaux.<br /><br /> -Pour plus d’informations, consultez Déclaration de la version ODBC et des [changements de comportement](../../../odbc/reference/develop-app/behavioral-changes.md) [de l’application](../../../odbc/reference/develop-app/declaring-the-application-s-odbc-version.md) .|  
|SQL_ATTR_OUTPUT_NTS (ODBC 3,0)|Entier 32 bits qui détermine la façon dont le pilote retourne des données de type chaîne. Si SQL_TRUE, le pilote retourne des données de chaîne qui se terminent par un caractère null. Si SQL_FALSE, le pilote ne retourne pas de données de chaîne se terminant par null.<br /><br /> Par défaut, cet attribut a la valeur SQL_TRUE. Un appel à **SQLSetEnvAttr** pour lui affecter la valeur SQL_TRUE retourne SQL_SUCCESS. Un appel à **SQLSetEnvAttr** pour lui affecter la valeur SQL_FALSE retourne SQL_ERROR et SQLSTATE HYC00 (fonctionnalité facultative non implémentée).|  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Retour de la valeur d’un attribut d’environnement|[SQLGetEnvAttr, fonction](../../../odbc/reference/syntax/sqlgetenvattr-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Nouveautés d’ODBC 3.8](../../../odbc/reference/what-s-new-in-odbc-3-8.md)
