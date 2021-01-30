---
description: Fonction SQLNativeSql
title: SQLNativeSql fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLNativeSql
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLNativeSql
helpviewer_keywords:
- SQLNativeSql function [ODBC]
ms.assetid: b8efc247-27ab-4a00-92b6-1400785783fe
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7cc73e184b6b092e1de4eef40de8eaadd11d738b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99160788"
---
# <a name="sqlnativesql-function"></a>Fonction SQLNativeSql
**Conformité**  
 Version introduite : ODBC 1,0 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLNativeSql** retourne la chaîne SQL telle qu’elle a été modifiée par le pilote. **SQLNativeSql** n’exécute pas l’instruction SQL.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLNativeSql(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      InStatementText,  
     SQLINTEGER     TextLength1,  
     SQLCHAR *      OutStatementText,  
     SQLINTEGER     BufferLength,  
     SQLINTEGER *   TextLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *ConnectionHandle*  
 [Entrée] Handle de connexion.  
  
 *InStatementText*  
 Entrée Chaîne de texte SQL à traduire.  
  
 *TextLength1*  
 Entrée Longueur en caractères de la chaîne de texte **InStatementText* .  
  
 *OutStatementText*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la chaîne SQL traduite.  
  
 Si *OutStatementText* a la valeur null, *TextLength2Ptr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *OutStatementText*.  
  
 *BufferLength*  
 Entrée Nombre de caractères dans la \* mémoire tampon *OutStatementText* . Si la valeur retournée dans *\* InStatementText* est une chaîne Unicode (lors de l’appel de **SQLNativeSqlW**), l’argument *BufferLength* doit être un nombre pair.  
  
 *TextLength2Ptr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (sans fin null) disponibles pour retourner dans \* *OutStatementText*. Si le nombre de caractères disponibles à retourner est supérieur ou égal à *BufferLength*, la chaîne SQL traduite dans \* *OutStatementText* est tronquée à *BufferLength* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Quand **SQLNativeSql** retourne soit SQL_ERROR, soit SQL_SUCCESS_WITH_INFO, une valeur SQLSTATE associée peut être obtenue en appelant **SQLGetDiagRec** avec un *comme HandleType* de SQL_HANDLE_DBC et un *handle* de *ConnectionHandle*. Le tableau suivant répertorie les valeurs SQLSTATE couramment retournées par **SQLNativeSql** et les explique dans le contexte de cette fonction. la notation « (DM) » précède les descriptions des SQLSTATEs retournées par le gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur|Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information spécifique au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|01004|Données de chaîne, tronquées à droite|Le OutStatementText de mémoire tampon \*  n’est pas assez grand pour retourner la chaîne SQL entière, donc la chaîne SQL a été tronquée. La longueur de la chaîne SQL non tronquée est retournée dans **TextLength2Ptr*. (La fonction retourne SQL_SUCCESS_WITH_INFO.)|  
|08003|Connexion non ouverte|Le *ConnectionHandle* n’était pas dans un état connecté.|  
|08S01|Échec de la liaison de communication|Le lien de communication entre le pilote et la source de données à laquelle le pilote a été connecté a échoué avant la fin du traitement de la fonction.|  
|22007|Format de date/heure non valide|**InStatementText* contenait une clause Escape avec une valeur de date, d’heure ou d’horodatage non valide.|  
|24 000|État de curseur non valide|Le curseur référencé dans l’instruction a été placé avant le début du jeu de résultats ou après la fin du jeu de résultats. Cette erreur ne peut pas être retournée par un pilote disposant d’une implémentation de curseur SGBD native.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucune SQLSTATE spécifique n’a été définie et pour lesquelles aucune SQLSTATE spécifique à l’implémentation n’a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans la mémoire tampon *\* MessageText* décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou l’achèvement de la fonction.|  
|HY009|Utilisation non valide d’un pointeur null|(DM) **InStatementText* était un pointeur null.|  
|HY010|Erreur de séquence de fonction|(DM) une fonction d’exécution asynchrone a été appelée pour le *ConnectionHandle* et était toujours en cours d’exécution quand cette fonction a été appelée.|  
|HY013|Erreur de gestion de la mémoire|Impossible de traiter l’appel de fonction, car les objets mémoire sous-jacents sont inaccessibles, probablement en raison de conditions de mémoire insuffisante.|  
|HY090|Longueur de chaîne ou de mémoire tampon non valide|(DM) l’argument *TextLength1* est inférieur à 0, mais n’est pas égal à SQL_NTS.|  
|||(DM) l’argument *BufferLength* est inférieur à 0 et l’argument *OutStatementText* n’était pas un pointeur null.|  
|HY109|Position de curseur non valide|La ligne active du curseur a été supprimée ou n’a pas été extraite. Cette erreur ne peut pas être retournée par un pilote disposant d’une implémentation de curseur SGBD native.|  
|HY117|La connexion est interrompue en raison d’un état de transaction inconnu. Seules les fonctions de déconnexion et de lecture seule sont autorisées.|(DM) pour plus d’informations sur l’état suspendu, consultez [fonction SQLEndTran](../../../odbc/reference/syntax/sqlendtran-function.md).|  
|HYT01|Délai d’attente de connexion expiré|Le délai d’attente de connexion a expiré avant que la source de données ait répondu à la demande. Le délai d’expiration de la connexion est défini par le biais de **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Le pilote ne prend pas en charge cette fonction|(DM) le pilote associé au *ConnectionHandle* ne prend pas en charge la fonction.|  
  
## <a name="comments"></a>Commentaires  
 Voici des exemples de ce que **SQLNativeSql** peut retourner pour la chaîne SQL d’entrée suivante qui contient la fonction scalaire Convert. Supposons que la colonne EmpID est de type entier dans la source de données :  
  
```sql  
SELECT { fn CONVERT (empid, SQL_SMALLINT) } FROM employee  
```  
  
 Un pilote pour Microsoft SQL Server peut retourner la chaîne SQL traduite suivante :  
  
```sql  
SELECT convert (smallint, empid) FROM employee  
```  
  
 Un pilote pour le serveur ORACLE peut retourner la chaîne SQL traduite suivante :  
  
```sql  
SELECT to_number (empid) FROM employee  
```  
  
 Un pilote pour Ingres peut retourner la chaîne SQL traduite suivante :  
  
```sql  
SELECT int2 (empid) FROM employee  
```  
  
 Pour plus d’informations, consultez [exécution directe](../../../odbc/reference/develop-app/direct-execution-odbc.md) et [Exécution préparée](../../../odbc/reference/develop-app/prepared-execution-odbc.md).  
  
## <a name="related-functions"></a>Fonctions connexes  
 Aucun.  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)
