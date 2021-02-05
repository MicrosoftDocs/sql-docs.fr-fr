---
description: Membres de SQLServerStatement
title: Membres de SQLServerStatement | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 828cbaa9-ea7a-4986-95c3-5ba0d7d01d83
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 3eecadb565935db91ce97c211b46a73536087dda
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179999"
---
# <a name="sqlserverstatement-members"></a>Membres de SQLServerStatement
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  Les tableaux suivants présentent les membres exposés par la classe [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).  
  
## <a name="constructors"></a>Constructeurs  
 Aucun.  
  
## <a name="fields"></a>Champs  
 Aucun.  
  
## <a name="inherited-fields"></a>Champs hérités  
  
|Nom|Description|  
|----------|-----------------|  
|java.sql.Statement|CLOSE_ALL_RESULTS, CLOSE_CURRENT_RESULT, EXECUTE_FAILED, KEEP_CURRENT_RESULT, NO_GENERATED_KEYS, RETURN_GENERATED_KEYS, SUCCESS_NO_INFO|  
  
## <a name="methods"></a>Méthodes  
  
|Nom|Description|  
|----------|-----------------|  
|[addBatch](../../../connect/jdbc/reference/addbatch-method-sqlserverstatement.md)|Ajoute la commande SQL donnée à la liste actuelle de commandes de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[cancel](../../../connect/jdbc/reference/cancel-method-sqlserverstatement.md)|Annule l’instruction SQL actuellement exécutée par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearBatch](../../../connect/jdbc/reference/clearbatch-method-sqlserverstatement.md)|Vide la liste actuelle de commandes SQL de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[clearWarnings](../../../connect/jdbc/reference/clearwarnings-method-sqlserverstatement.md)|Efface tous les avertissements signalés sur cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[close](../../../connect/jdbc/reference/close-method-sqlserverstatement.md)|Libère immédiatement les ressources JDBC et la base de données de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md), au lieu de patienter jusqu’à leur libération automatique.|  
|[execute](../../../connect/jdbc/reference/execute-method-sqlserverstatement.md)|Exécute l'instruction SQL donnée, qui peut retourner plusieurs résultats.|  
|[executeBatch](../../../connect/jdbc/reference/executebatch-method-sqlserverstatement.md)|Envoie un lot de commandes à exécuter à la base de données. En cas d'exécution correcte de toutes les commandes, un tableau des nombres de mises à jour est retourné.|  
|[executeQuery](../../../connect/jdbc/reference/executequery-method-sqlserverstatement.md)|Exécute l’instruction SQL donnée et retourne un seul objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[executeUpdate](../../../connect/jdbc/reference/executeupdate-method-sqlserverstatement.md)|Exécute l'instruction SQL fournie, qui peut être une instruction INSERT, UPDATE, MERGE ou DELETE ; ou une instruction SQL qui ne retourne rien, telle qu'une instruction SQL DDL.|  
|[getConnection](../../../connect/jdbc/reference/getconnection-method-sqlserverstatement.md)|Récupère l’objet [SQLServerConnection](../../../connect/jdbc/reference/sqlserverconnection-class.md) qui a produit cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getFetchDirection](../../../connect/jdbc/reference/getfetchdirection-method-sqlserverstatement.md)|Récupère la direction d’extraction des lignes dans des tables de base de données. C’est la méthode par défaut pour les jeux de résultats générés à partir de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getFetchSize](../../../connect/jdbc/reference/getfetchsize-method-sqlserverstatement.md)|Récupère le nombre de lignes du jeu de résultats. C’est la taille d’extraction par défaut pour les objets du jeu de résultats générés à partir de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getGeneratedKeys](../../../connect/jdbc/reference/getgeneratedkeys-method-sqlserverstatement.md)|Récupère toutes les clés générées automatiquement du fait de l’exécution de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMaxFieldSize](../../../connect/jdbc/reference/getmaxfieldsize-method-sqlserverstatement.md)|Récupère le nombre maximal d’octets pouvant être retournés pour des valeurs de colonne de caractères ou binaire dans un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMaxRows](../../../connect/jdbc/reference/getmaxrows-method-sqlserverstatement.md)|Récupère le nombre maximal de lignes que peut contenir un objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) produit par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getMoreResults](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)|Passe au résultat suivant de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getQueryTimeout](../../../connect/jdbc/reference/getquerytimeout-method-sqlserverstatement.md)|Récupère le nombre de secondes pendant lesquelles [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] attend que cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) s’exécute.|  
|[getResponseBuffering](../../../connect/jdbc/reference/getresponsebuffering-method-sqlserverstatement.md)|Récupère le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSet](../../../connect/jdbc/reference/getresultset-method-sqlserverstatement.md)|Récupère le résultat actuel sous forme d’objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md).|  
|[getResultSetConcurrency](../../../connect/jdbc/reference/getresultsetconcurrency-method-sqlserverstatement.md)|Récupère la concurrence du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSetHoldability](../../../connect/jdbc/reference/getresultsetholdability-method-sqlserverstatement.md)|Récupère la capacité de mise en attente du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getResultSetType](../../../connect/jdbc/reference/getresultsettype-method-sqlserverstatement.md)|Récupère le type du jeu de résultats pour les objets [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) générés par cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[getUpdateCount](../../../connect/jdbc/reference/getupdatecount-method-sqlserverstatement.md)|Récupère le résultat actuel en tant que nombre de mises à jour.|  
|[getWarnings](../../../connect/jdbc/reference/getwarnings-method-sqlserverstatement.md)|Récupère le premier avertissement signalé par des appels sur cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md).|  
|[isClosed](../../../connect/jdbc/reference/isclosed-method-sqlserverstatement.md)|Indique si cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) a été fermé.|  
|[isPoolable](../../../connect/jdbc/reference/ispoolable-method-sqlserverstatement.md)|Retourne une valeur qui indique s'il est possible d'ajouter une instruction au regroupement d'instructions fourni par l'utilisateur.|  
|[isWrapperFor](../../../connect/jdbc/reference/iswrapperfor-method-sqlserverstatement.md)|Indique si cet objet d'instruction est un wrapper pour l'interface spécifiée.|  
|[setCursorName](../../../connect/jdbc/reference/setcursorname-method-sqlserverstatement.md)|Définit le nom de curseur SQL selon la chaîne donnée, qui est exécutée par les méthodes d'exécution suivantes.|  
|[setEscapeProcessing](../../../connect/jdbc/reference/setescapeprocessing-method-sqlserverstatement.md)|Définit le mode de traitement d'échappement.|  
|[setFetchDirection](../../../connect/jdbc/reference/setfetchdirection-method-sqlserverstatement.md)|Fournit un conseil au pilote JDBC concernant la direction de traitement des lignes du jeu de résultats.|  
|[setFetchSize](../../../connect/jdbc/reference/setfetchsize-method-sqlserverstatement.md)|Fournit un conseil au pilote JDBC concernant le nombre de lignes qui doivent être extraites depuis la base de données, lorsque davantage de lignes sont nécessaires. |  
|[setMaxFieldSize](../../../connect/jdbc/reference/setmaxfieldsize-method-sqlserverstatement.md)|Définit le nombre maximal d’octets dans une colonne [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) qui stocke des valeurs de caractères ou binaires selon le nombre d’octets donné.|  
|[setMaxRows](../../../connect/jdbc/reference/setmaxrows-method-sqlserverstatement.md)|Définit la limite du nombre maximal de lignes que tout objet [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) peut contenir sur le nombre donné.|  
|[setPoolable](../../../connect/jdbc/reference/setpoolable-method-sqlserverstatement.md)|Demande qu'une instruction soit regroupée ou non.|  
|[setQueryTimeout](../../../connect/jdbc/reference/setquerytimeout-method-sqlserverstatement.md)|Définit le nombre de secondes pendant lesquelles le pilote attend qu’un objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) s’exécute selon le nombre de secondes donné.|  
|[setResponseBuffering](../../../connect/jdbc/reference/setresponsebuffering-method-sqlserverstatement.md)|Définit le mode de mise en mémoire tampon des réponses de cet objet [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) selon **String full** ou **adaptive**, sans respect de la casse.|  
|[unwrap](../../../connect/jdbc/reference/unwrap-method-sqlserverstatement.md)|Retourne un objet qui implémente l’interface spécifiée, afin d’autoriser l’accès aux méthodes propres au [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)].|  
  
## <a name="inherited-methods"></a>Méthodes héritées  
  
|Classe héritée de :|Méthodes|  
|---------------------------|-------------|  
|java.lang.Object|clone, equals, getClass, hashCode, notify, notifyAll, toString, wait|  
|java.sql.Wrapper|isWrapperFor, unwrap|  
  
## <a name="see-also"></a>Voir aussi  
 [SQLServerStatement, classe](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
