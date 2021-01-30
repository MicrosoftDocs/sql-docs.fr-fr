---
description: SQLRateConnection, fonction
title: SQLRateConnection fonction) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SQLRateConnection function [ODBC]
ms.assetid: e8da2ffb-d6ef-4ca7-824f-57afd29585d8
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 40aec3459e43d2684bd80f0ced55171314551d92
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99192546"
---
# <a name="sqlrateconnection-function"></a>SQLRateConnection, fonction
**Conformité**  
 Version introduite : ODBC 3,81 conforme aux normes : ODBC  
  
 **Résumé**  
 **SQLRateConnection** détermine si un pilote peut réutiliser une connexion existante dans le pool de connexions.  
  
## <a name="syntax"></a>Syntaxe  
  
```cpp
  
SQLRETURN  SQLRateConnection(  
                SQLHDBC_INFO_TOKEN   hRequest,  
                SQLHDBC              hCandidateConnection,  
                BOOL                 fRequiredTransactionEnlistment,  
                TRANSID              transId,  
                DWORD *              pRating );  
```  
  
## <a name="arguments"></a>Arguments  
 *hRequest*  
 Entrée Handle de jeton représentant la nouvelle demande de connexion d’application.  
  
 *hCandidateConnection*  
 Entrée Connexion existante dans le pool de connexions. La connexion doit être dans un état ouvert.  
  
 *fRequiredTransactionEnlistment*  
 Entrée Si la valeur est TRUE, la réutilisation du *hCandidateConnection* de la connexion existante pour la nouvelle demande de connexion (*hRequest*) nécessite une inscription supplémentaire.  
  
 *transId*  
 Entrée Si *fRequiredTransactionEnlistment* a la valeur true, *transId* représente la transaction DTC que la demande doit inscrire. Si *fRequiredTransactionEnlistment* a la valeur false, *transId* sera ignoré.  
  
 *pRating*  
 Sortie l’évaluation de la réutilisation de *hCandidateConnection* pour le *hRequest*. Cette évaluation sera comprise entre 0 et 100 (inclus).  
  
## <a name="returns"></a>Retours  
 SQL_SUCCESS, SQL_ERROR ou SQL_INVALID_HANDLE.  
  
## <a name="diagnostics"></a>Diagnostics  
 Le gestionnaire de pilotes ne traitera pas les informations de diagnostic retournées par cette fonction.  
  
## <a name="remarks"></a>Notes  
 **SQLRateConnection** produit un score compris entre 0 et 100 (inclus) indiquant la manière dont une connexion existante correspond à la demande.  
  
|Score|Signification (lorsque SQL_SUCCESS est retourné)|  
|-----------|-----------------------------------------------|  
|0|*hCandidateConnection* ne doit pas être réutilisé pour le *hRequest*.|  
|Toutes les valeurs comprises entre 1 et 98 (inclusive)|Plus le score est élevé, plus la correspondance avec *hRequest* est proche de *hCandidateConnection* .|  
|99|Il n’existe que des incompatibilités dans les attributs non significatifs.  Le gestionnaire de pilotes doit arrêter la boucle d’évaluation.|  
|100|Correspondance parfaite.  Le gestionnaire de pilotes doit arrêter la boucle d’évaluation.|  
|Toute autre valeur supérieure à 100|*hCandidateConnection* est marqué comme Dead et ne peut pas être réutilisé même dans une demande de connexion ultérieure.|  
  
 Le gestionnaire de pilotes marque une connexion comme étant morte si le code de retour n’est pas SQL_SUCCESS (y compris SQL_SUCCESS_WITH_INFO) ou si l’évaluation est supérieure à 100. Cette connexion morte n’est pas réutilisée (même dans les demandes de connexion ultérieures) et finit par dépasser le délai d’attente de CPTimeout. Le gestionnaire de pilotes continuera à trouver une autre connexion entre le pool et le tarif.  
  
 Si le gestionnaire de pilotes réutilise une connexion dont le score est strictement inférieur à 100 (y compris 99), le gestionnaire de pilotes appellera SQLSetConnectAttr (SQL_ATTR_DBC_INFO_TOKEN) pour réinitialiser la connexion à l’État demandé par l’application. Le pilote ne doit pas réinitialiser la connexion dans cet appel de fonction.  
  
 Si *fRequiredTransactionEnlistment* a la valeur true, la réutilisation de *hCandidateConnection* nécessite une inscription supplémentaire (*transId* ! = null) ou une désinscription (*transId* = = null). Cela indique le coût de la réutilisation d’une connexion et si le pilote doit inscrire/désinscrire la connexion s’il va réutiliser la connexion. Si *fRequireTransactionEnlistment* a la valeur false, le pilote doit ignorer la valeur de *transId*.  
  
 Le gestionnaire de pilotes garantit que le handle HENV parent de *hRequest* et de *hCandidateConnection* est identique. Le gestionnaire de pilotes garantit que l’ID de pool associé à *hRequest* et *hCandidateConnection* sont identiques.  
  
 Les applications ne doivent pas appeler cette fonction directement. Un pilote ODBC qui prend en charge le regroupement de connexions prenant en charge les pilotes doit implémenter cette fonction.  
  
 Incluez sqlspi. h pour le développement du pilote ODBC.  
  
## <a name="see-also"></a>Voir aussi  
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md)   
 [Développement de la reconnaissance des pools de connexions dans un pilote ODBC](../../../odbc/reference/develop-driver/developing-connection-pool-awareness-in-an-odbc-driver.md)
