---
title: ISSAsynchStatus::GetStatus (pilote OLE DB) | Microsoft Docs
description: Découvrez comment la méthode ISSAsynchStatus::GetStatus retourne l’état d’une opération s’exécutant de façon asynchrone dans OLE DB Driver pour SQL Server.
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- ISSAsynchStatus::GetStatus (OLE DB)
apitype: COM
helpviewer_keywords:
- GetStatus method
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 7220f8c6552858fda5d6c42b0a4ea2d7ee0c7857
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99183727"
---
# <a name="issasynchstatusgetstatus-ole-db"></a>ISSAsynchStatus::GetStatus (OLE DB)
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  Retourne l'état d'une opération s'exécutant de manière asynchrone.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
HRESULT GetStatus(  
        HCHAPTER hChapter,  
        DBASYNCHOP eOperation,  
        DBCOUNTITEM *pulProgress,  
        DBCOUNTITEM *pulProgressMax,  
        DBASYNCHPHASE *peAsynchPhase,  
        LPOLESTR *ppwszStatusText);  
```  
  
## <a name="arguments"></a>Arguments  
 *hChapter*[in]  
 Handle du chapitre. Si l'objet interrogé n'est pas un objet rowset ou que l'opération ne s'applique pas à un chapitre, vous devez attribuer au handle la valeur DB_NULL_HCHAPTER, celle-ci étant ignorée par le fournisseur.  
  
 *eOperation*[in]  
 L'opération pour laquelle l'état asynchrone est demandé. La valeur suivante doit être utilisée :  
  
 DBASYNCHOP_OPEN : le consommateur demande des informations sur l'ouverture ou le remplissage asynchrone d'un ensemble de lignes ou sur l'initialisation asynchrone d'un objet source de données. Si le fournisseur est un fournisseur conforme à OLE DB 2.5 qui prend en charge la liaison d'URL directe, le consommateur demande des informations sur l'initialisation ou le remplissage asynchrone d'une source de données, d'un ensemble de lignes, d'une ligne ou d'un objet de flux.  
  
 *pulProgress*[out]  
 Pointeur vers la mémoire dans lequel la progression actuelle de l'opération asynchrone relative à la valeur maximale attendue indiquée dans le paramètre *pulProgressMax* est retournée. Pour plus d'informations sur la signification de *pulProgress*, consultez la description de *peAsynchPhase*.  
  
 Si *pulProgress* est un pointeur null, aucune progression n'est retournée.  
  
 *pulProgressMax*[out]  
 Pointeur vers la mémoire dans lequel la valeur maximale attendue du paramètre *pulProgress* est retournée. Cette valeur peut varier selon les appels à cette méthode. Pour plus d'informations sur la signification de *pulProgressMax*, consultez la description de *peAsynchPhase*.  
  
 Si *pulProgressMax* est un pointeur null, aucune valeur maximale attendue n'est retournée.  
  
 *peAsynchPhase*[out]  
 Pointeur vers la mémoire dans lequel des informations supplémentaires concernant la progression de l’opération asynchrone sont retournées. Les valeurs valides sont les suivantes :  
  
 DBASYNCHPHASE_INITIALIZATION : l'objet est dans une phase d'initialisation. Les arguments *pulProgress* et *pulProgressMax* indiquent un taux d'achèvement estimé. L'objet n'est pas encore entièrement matérialisé. Toute tentative d'appel d'autres interfaces peut échouer, et le jeu complet d'interfaces peut ne pas être disponible sur l'objet. Si l'opération asynchrone est le résultat de l'appel à **ICommand::Execute** pour une commande qui met à jour, supprime ou insère des lignes, et que *cParamSets* est supérieur à 1, *pulProgress* et *pulProgressMax* peuvent indiquer la progression pour un jeu unique de paramètres ou pour le tableau complet de jeux de paramètres.  
  
 DBASYNCHPHASE_POPULATION : l'objet est dans une phase de remplissage. Bien que l'ensemble de lignes soit entièrement initialisé et que la plage complète d'interfaces soit disponible sur l'objet, d'autres lignes peuvent ne pas être encore remplies dans l'ensemble de lignes. Même si *pulProgress* et *pulProgressMax* peuvent être basés sur le nombre de lignes remplies, ils sont en général basés sur la durée ou l'effort requis pour remplir l'ensemble de lignes. Un appelant doit par conséquent utiliser ces informations comme une estimation approximative de la durée du processus, et non comme le nombre de lignes éventuel. Cette phase est uniquement retournée lors du remplissage d'un ensemble de lignes ; elle n'est jamais retournée lors de l'initialisation d'un objet source de données ou par l'exécution d'une commande qui met à jour, supprime ou insère des lignes.  
  
 DBASYNCHPHASE_COMPLETE : l'ensemble du traitement asynchrone ayant été réalisé par l'objet. La méthode **ISSAsynchStatus::GetStatus** retourne un HRESULT qui indique le résultat de l’opération. En général, il s'agit du HRESULT qui aurait été retourné si l'opération avait été appelée de manière synchrone. Si l'opération asynchrone est le résultat de l'appel à **ICommand::Execute** pour une commande qui met à jour, supprime ou insère des lignes, *pulProgress* et *pulProgressMax* sont égaux au nombre total de lignes affectées par la commande. Si *cParamSets* est supérieur à 1, il s'agit du nombre total de lignes affectées par tous les jeux de paramètres spécifiés dans l'exécution. Si *peAsynchPhase* est un pointeur null, aucun code d'état n'est retourné.  
  
 DBASYNCHPHASE_CANCELED : le traitement asynchrone de l'objet a été abandonné. La méthode **ISSAsynchStatus::GetStatus** retourne DB_E_CANCELED. Si l'opération asynchrone est le résultat de l'appel à **ICommand::Execute** pour une commande qui met à jour, supprime ou insère des lignes, *pulProgress* est égal au nombre total de lignes, pour tous les paramètres définis, affectées par la commande avant l'annulation.  
  
 *ppwszStatusText*[in/out]  
 Pointeur vers la mémoire contenant des informations supplémentaires sur l'opération. Un fournisseur peut utiliser cette valeur pour faire la différence entre des éléments d'une opération, par exemple les différentes ressources en cours d'accès. Cette chaîne est localisée selon la propriété DBPROP_INIT_LCID sur l'objet source de données.  
  
 Si *ppwszStatusText* n'a pas la valeur Null en entrée, le fournisseur retourne l'état associé à l'élément particulier identifié par *ppwszStatusText*. Si *ppwszStatusText* n'indique pas un élément d' *eOperation*, le fournisseur retourne S_OK avec la même valeur attribuée à *pulProgress* et à *pulProgressMax* . Si le fournisseur ne fait pas la différence entre des éléments basés sur un identificateur textuel, il attribue la valeur Null à *ppwszStatusText* et retourne des informations sur l'opération dans son ensemble ; sinon, si *ppwszStatusText* n'a pas la valeur Null en entrée, le fournisseur laisse *ppwszStatusText* inchangé.  
  
 Si *ppwszStatusText* a la valeur Null en entrée, le fournisseur attribue à *ppwszStatusText* une valeur qui indique plus d’informations sur l’opération, ou une valeur Null si aucune information n’est disponible ou si la méthode **ISSAsynchStatus::GetStatus** retourne une erreur. Lorsque *ppwszStatusText* a la valeur Null en entrée, le fournisseur alloue de la mémoire pour la chaîne d'état et retourne l'adresse à cette mémoire. Le consommateur libère cette mémoire avec **IMalloc::Free** lorsqu'il n'a plus besoin de la chaîne.  
  
 Si *ppwszStatusText* a la valeur Null en entrée, aucune chaîne d'état n'est retournée et le fournisseur retourne des informations sur n'importe quel élément de l'opération ou sur l'opération en général.  
  
## <a name="return-code-values"></a>Codet de retour  
 S_OK  
 Retour réussi de la méthode.  
  
-   Si *peAsynchPhase* est égal à DBASYNCHPHASE_INITIALIZATION, l'objet n'est pas encore entièrement initialisé ; toute tentative d'appel à d'autres interfaces peut échouer, et le jeu complet d'interfaces peut ne pas être disponible sur l'objet.  
  
-   Si *peAsynchPhase* est égal à DBASYNCHPHASE_POPULATION, l'ensemble de lignes est entièrement initialisé et la plage complète d'interfaces est disponible sur l'objet ; toutefois, d'autres lignes peuvent ne pas être encore remplies dans l'ensemble de lignes.  
  
-   Si *peAsynchPhase* est égal à DBASYNCHPHASE_COMPLETE, le traitement asynchrone de l'objet est entièrement terminé. L'objet est entièrement initialisé et rempli.  
  
 DB_E_CANCELED  
 Le traitement asynchrone a été annulé pendant le remplissage de l'ensemble de lignes. Le remplissage s'arrête, mais l'ensemble de lignes reste valide pour les lignes déjà remplies.  
  
 Le traitement asynchrone a été annulé pendant l'initialisation de l'objet source de données. L'objet source de données se trouve dans un état non initialisé.  
  
 E_INVALIDARG  
 Le paramètre *hChapter* est incorrect.  
  
 E_UNEXPECTED  
 La méthode **ISSAsynchStatus::GetStatus** a été appelée sur un objet source de données, et **IDBInitialize::Initialize** n’a pas été appelé sur l’objet source de données.  
  
 La méthode **ISSAsynchStatus::GetStatus** a été appelée sur un ensemble de lignes, **ITransaction::Commit** ou **ITransaction::Abort** a été appelé, et l’objet se trouve dans un état zombie.  
  
 La méthode **ISSAsynchStatus::GetStatus** a été appelée sur un ensemble de lignes qui a été annulé de manière asynchrone dans sa phase d’initialisation. L'ensemble de lignes se trouve dans un état zombie.  
  
 E_FAIL  
 Une erreur spécifique au fournisseur s'est produite.  
  
## <a name="remarks"></a>Notes  
 La méthode **ISSAsynchStatus::GetStatus** se comporte exactement comme la méthode **IDBAsynchStatus::GetStatus**, à la différence près que si l’initialisation d’un objet source de données est abandonnée, E_UNEXPECTED est retourné au lieu de DB_E_CANCELED (pourtant, [ISSAsynchStatus::WaitForAsynchCompletion](../../oledb/ole-db-interfaces/issasynchstatus-waitforasynchcompletion-ole-db.md) retourne DB_E_CANCELED). Cela est dû au fait que l’objet source de données ne reste pas dans l’état zombie habituel après un abandon, et ce ,pour autoriser d’autres tentatives d’initialisation.  
  
 Si l'ensemble de lignes est initialisé ou rempli de manière asynchrone, il doit prendre en charge cette méthode.  
  
 En plus des valeurs de retour répertoriées, **ISSAsynchStatus::GetStatus** peut retourner tout HRESULT qui aurait été retourné par la méthode ayant initialisé l'opération asynchrone, indiquant la réussite ou l'échec de l'opération.  
  
 Certaines opérations asynchrones peuvent uniquement être en mesure de retourner des états « fini » et « non fini ». Elles doivent attribuer la valeur 1 à *pulProgressMax* pour indiquer la granularité de leur estimation (tout ou rien). Leurs réponses sont donc 0/1 ou 1/1.  
  
 Un fournisseur peut modifier *pulProgressMax* dans des appels consécutifs, et même retourner un taux inférieur à celui indiqué précédemment, si cela reflète une meilleure estimation du degré d'achèvement de la tâche.  
  
 Le fournisseur n'est pas obligé de garantir davantage de précision, mais il y est encouragé dans les cas des estimations d'achèvement raisonnables peuvent être faites. L'utilisation principale de cette fonction étant vraisemblablement de donner les commentaires de progression à l'utilisateur, de tels efforts améliorent la qualité de l'interface utilisateur. La satisfaction de l'utilisateur augmente avec la qualité des commentaires sur une tâche invisible de longue durée.  
  
 Le fait d'appeler **ISSAsynchStatus::GetStatus** sur un objet source de données initialisé ou un ensemble de lignes rempli, ou de passer une valeur pour *eOperation* autre que DBASYNCHOP_OPEN, retourne S_OK avec la même valeur attribuée à *pulProgress* et à *pulProgressMax* . Si la méthode **ISSAsynchStatus::GetStatus** est appelée sur un objet créé à partir de l’exécution d’une commande qui met à jour, supprime ou insère des lignes, *pulProgress* et *pulProgressMax* indiquent le nombre total de lignes affectées par la commande.  
  
## <a name="see-also"></a>Voir aussi  
 [Exécution d’opérations asynchrones](../../oledb/features/performing-asynchronous-operations.md)   
 [ISSAsynchStatus &#40;OLE DB&#41;](../../oledb/ole-db-interfaces/issasynchstatus-ole-db.md)  
  
  
