---
description: SQLGetDiagRec, fonction
title: Fonction SQLGetDiagRec | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLGetDiagRec
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLGetDiagRec
helpviewer_keywords:
- SQLGetDiagRec function [ODBC]
ms.assetid: ebdbac93-3d68-438f-8416-ef1f08e04269
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93b5b85d6c20a58d314dcb7f7c53f391c15a1491
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194179"
---
# <a name="sqlgetdiagrec-function"></a>SQLGetDiagRec, fonction
**Conformité**  
 Version introduite : ODBC 3,0 conformité aux normes : ISO 92  
  
 **Résumé**  
 **SQLGetDiagRec** retourne les valeurs actuelles de plusieurs champs d’un enregistrement de diagnostic qui contient des informations d’erreur, d’avertissement et d’État. Contrairement à **SQLGetDiagField**, qui retourne un champ de diagnostic par appel, **SQLGetDiagRec** retourne plusieurs champs couramment utilisés d’un enregistrement de diagnostic, y compris SQLSTATE, le code d’erreur natif et le texte du message de diagnostic.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp  
  
SQLRETURN SQLGetDiagRec(  
     SQLSMALLINT     HandleType,  
     SQLHANDLE       Handle,  
     SQLSMALLINT     RecNumber,  
     SQLCHAR *       SQLState,  
     SQLINTEGER *    NativeErrorPtr,  
     SQLCHAR *       MessageText,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   TextLengthPtr);  
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
 Entrée Indique l’enregistrement d’État à partir duquel l’application recherche des informations. Les enregistrements d’État sont numérotés à partir de 1.  
  
 *SQLState*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner un code SQLSTATE de cinq caractères (et une valeur NULL de fin) pour l’enregistrement de diagnostic *recnumber*. Les deux premiers caractères indiquent la classe ; les trois suivants indiquent la sous-classe. Ces informations sont contenues dans le champ diagnostic SQL_DIAG_SQLSTATE. Pour plus d’informations, consultez [sqlstates](../../../odbc/reference/develop-app/sqlstates.md).  
  
 *NativeErrorPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le code d’erreur natif, propre à la source de données. Ces informations sont contenues dans le champ diagnostic SQL_DIAG_NATIVE.  
  
 *MessageText*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner la chaîne de texte du message de diagnostic. Ces informations sont contenues dans le champ diagnostic SQL_DIAG_MESSAGE_TEXT. Pour obtenir le format de la chaîne, consultez [diagnostic messages](../../../odbc/reference/develop-app/diagnostic-messages.md).  
  
 Si *MessageText* a la valeur null, *TextLengthPtr* retourne toujours le nombre total de caractères (à l’exception du caractère de fin null pour les données de type caractère) disponibles pour retourner dans la mémoire tampon vers laquelle pointe *MessageText*.  
  
 *BufferLength*  
 Entrée Longueur de la mémoire tampon **MessageText* en caractères. Il n’y a pas de longueur maximale du texte du message de diagnostic.  
  
 *TextLengthPtr*  
 Sortie Pointeur vers une mémoire tampon dans laquelle retourner le nombre total de caractères (sans compter le nombre de caractères requis pour le caractère de fin null) pouvant être retourné dans *\* MessageText*. Si le nombre de caractères disponibles à retourner est supérieur à *BufferLength*, le texte du message de diagnostic dans *\* MessageText* est tronqué à *BufferLength* moins la longueur d’un caractère de fin null.  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 **SQLGetDiagRec** ne publie pas d’enregistrements de diagnostic pour lui-même. Elle utilise les valeurs de retour suivantes pour signaler le résultat de sa propre exécution :  
  
-   SQL_SUCCESS : la fonction a retourné des informations de diagnostic.  
  
-   SQL_SUCCESS_WITH_INFO : le \* tampon *MessageText* est trop petit pour contenir le message de diagnostic demandé. Aucun enregistrement de diagnostic n’a été généré. Pour déterminer qu’une troncation s’est produite, l’application doit comparer *BufferLength* au nombre réel d’octets disponibles, qui est écrit dans **StringLengthPtr*.  
  
-   SQL_INVALID_HANDLE : le handle indiqué par *comme HandleType* et *handle* n’est pas un handle valide.  
  
-   SQL_ERROR : l’un des éléments suivants s’est produit :  
  
    -   *Recnumber* était négatif ou égal à 0.  
  
    -   *BufferLength* est inférieur à zéro.  
  
    -   Lors de l’utilisation d’une notification asynchrone, l’opération asynchrone sur le descripteur n’est pas terminée.  
  
-   SQL_NO_DATA : *recnumber* est supérieur au nombre d’enregistrements de diagnostics qui existaient pour le descripteur spécifié dans *descripteur.* La fonction retourne également SQL_NO_DATA pour tout *recnumber* positif s’il n’y a pas d’enregistrements de diagnostic pour *descripteur*.  
  
## <a name="comments"></a>Commentaires  
 Une application appelle en général **SQLGetDiagRec** lorsqu’un appel précédent à une fonction ODBC a retourné SQL_ERROR ou SQL_SUCCESS_WITH_INFO. Toutefois, étant donné qu’une fonction ODBC peut poster zéro, un ou plusieurs enregistrements de diagnostic chaque fois qu’elle est appelée, une application peut appeler **SQLGetDiagRec** après n’importe quel appel de fonction ODBC. Une application peut appeler **SQLGetDiagRec** plusieurs fois pour retourner une partie ou l’ensemble des enregistrements de la structure de données de diagnostic. ODBC n’impose aucune limite au nombre d’enregistrements de diagnostic pouvant être stockés à un moment donné.  
  
 **SQLGetDiagRec** ne peut pas être utilisé pour retourner des champs de l’en-tête de la structure de données de diagnostic. (L’argument *recnumber* doit être supérieur à 0.) L’application doit appeler **SQLGetDiagField** à cet effet.  
  
 **SQLGetDiagRec** récupère uniquement les informations de diagnostic récemment associées au handle spécifié dans l’argument *handle* . Si l’application appelle une autre fonction ODBC, à l’exception de **SQLGetDiagRec**, **SQLGetDiagField** ou **SQLError**, toutes les informations de diagnostic issues des appels précédents sur le même handle sont perdues.  
  
 Une application peut analyser tous les enregistrements de diagnostic en boucle, en incrémentant *recnumber*, tant que **SQLGetDiagRec** retourne SQL_SUCCESS. Les appels à **SQLGetDiagRec** ne sont pas destructifs pour les champs d’en-tête et d’enregistrement. L’application peut appeler **SQLGetDiagRec** à nouveau ultérieurement pour récupérer un champ d’un enregistrement tant qu’aucune autre fonction, à l’exception de **SQLGetDiagRec**, **SQLGetDiagField** ou **SQLError**, n’a été appelée dans l’intervalle. L’application peut également récupérer le nombre total d’enregistrements de diagnostic disponibles en appelant **SQLGetDiagField** pour récupérer la valeur du champ SQL_DIAG_NUMBER, puis en appelant **SQLGetDiagRec** plusieurs fois.  
  
 Pour obtenir une description des champs de la structure de données de diagnostic, consultez [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md). Pour plus d’informations, consultez [utilisation de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/using-sqlgetdiagrec-and-sqlgetdiagfield.md) et [implémentation de SQLGetDiagRec et SQLGetDiagField](../../../odbc/reference/develop-app/implementing-sqlgetdiagrec-and-sqlgetdiagfield.md).  
  
 L’appel d’une API autre que celle qui est exécutée de manière asynchrone génère HY010 « erreur de séquence de fonction ». Toutefois, l’enregistrement d’erreur ne peut pas être récupéré avant la fin de l’opération asynchrone.  
  
## <a name="handletype-argument"></a>Argument comme HandleType  
 Chaque type de handle peut être associé à des informations de diagnostic. L’argument *comme HandleType* indique le type de handle de l’argument *descripteur* .  
  
 Certains champs d’en-tête et d’enregistrement ne peuvent pas être renvoyés pour les handles d’environnement, de connexion, d’instruction et de descripteur. Les descripteurs pour lesquels un champ n’est pas applicable sont indiqués dans les sections champs d’en-tête et champs d’enregistrement dans [SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md).  
  
 Un appel à **SQLGetDiagRec** retourne SQL_INVALID_HANDLE si *comme HandleType* est SQL_HANDLE_SENV, ce qui désigne un handle d’environnement partagé. Toutefois, si *comme HandleType* est SQL_HANDLE_ENV, *handle* peut être un handle d’environnement partagé ou non partagé.  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Obtention d’un champ d’un enregistrement de diagnostic ou d’un champ de l’en-tête de diagnostic|[Fonction SQLGetDiagField](../../../odbc/reference/syntax/sqlgetdiagfield-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Informations de référence sur l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)   
 [Exemple de programme ODBC](../../../odbc/reference/sample-odbc-program.md)
