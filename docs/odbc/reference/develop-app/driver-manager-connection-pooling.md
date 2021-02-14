---
description: Regroupement de connexions du gestionnaire de pilotes
title: Regroupement de connexions du gestionnaire de pilotes | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- connection pooling [ODBC]
- pooled connections [ODBC]
- connecting to driver [ODBC], connection pooling
- connecting to data source [ODBC], connection pooling
ms.assetid: ee95ffdb-5aa1-49a3-beb2-7695b27c3df9
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 8876696412360275fcf443a5543634c9ac537df6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070334"
---
# <a name="driver-manager-connection-pooling"></a>Regroupement de connexions du gestionnaire de pilotes
Le regroupement de connexions permet à une application d’utiliser une connexion à partir d’un pool de connexions qui n’ont pas besoin d’être rétablies pour chaque utilisation. Une fois qu’une connexion a été créée et placée dans un pool, une application peut réutiliser cette connexion sans exécuter le processus de connexion complet.  
  
 L’utilisation d’une connexion regroupée peut entraîner des gains de performances significatifs, car les applications peuvent économiser la surcharge liée à l’établissement d’une connexion. Cela peut être particulièrement important pour les applications de couche intermédiaire qui se connectent sur un réseau ou pour les applications qui se connectent et se déconnectent à plusieurs reprises, telles que les applications Internet.  
  
 En plus des gains de performances, l’architecture de regroupement de connexions permet à un environnement et à ses connexions associées d’être utilisés par plusieurs composants dans un même processus. Cela signifie que les composants autonomes dans le même processus peuvent interagir l’un avec l’autre sans être conscients l’un de l’autre. Une connexion dans un pool de connexions peut être utilisée de façon répétée par plusieurs composants.  
  
> [!NOTE]
>  Le regroupement de connexions peut être utilisé par une application ODBC qui présente ODBC 2. comportement *x* , tant que l’application peut appeler *SQLSetEnvAttr*. Lors de l’utilisation du regroupement de connexions, l’application ne doit pas exécuter d’instructions SQL qui modifient la base de données ou le contexte de la base de données, telles que la modification du \<*database name*> catalogue utilisé par une source de données.  


 Un pilote ODBC doit être entièrement thread-safe, et les connexions ne doivent pas avoir d’affinité de thread pour prendre en charge le regroupement de connexions. Cela signifie que le pilote est en mesure de gérer un appel sur n’importe quel thread à tout moment et qu’il est en mesure de se connecter à un thread, d’utiliser la connexion sur un autre thread et de se déconnecter sur un troisième thread.  
  
 Le pool de connexions est géré par le gestionnaire de pilotes. Les connexions sont extraites du pool lorsque l’application appelle **SQLConnect** ou **SQLDriverConnect** et sont retournées au pool lorsque l’application appelle **SQLDisconnect**. La taille du pool augmente de manière dynamique, en fonction des allocations de ressources demandées. Elle diminue en fonction du délai d’inactivité : si une connexion est inactive pendant un certain laps de temps (elle n’a pas été utilisée dans une connexion), elle est supprimée du pool. La taille du pool est limitée uniquement par les contraintes de mémoire et les limites sur le serveur.  
  
 Le gestionnaire de pilotes détermine si une connexion spécifique dans un pool doit être utilisée conformément aux arguments passés dans **SQLConnect** ou **SQLDriverConnect**, et en fonction des attributs de connexion définis après l’allocation de la connexion.  
  
 Quand le gestionnaire de pilotes regroupe les connexions, il doit être en mesure de déterminer si une connexion continue à fonctionner avant de distribuer la connexion. Dans le cas contraire, le gestionnaire de pilotes continue de transmettre la connexion morte à l’application à chaque fois qu’une défaillance réseau temporaire se produit. Un nouvel attribut de connexion a été défini dans ODBC 3 *. x*: SQL_ATTR_CONNECTION_DEAD. Il s’agit d’un attribut de connexion en lecture seule qui retourne soit SQL_CD_TRUE, soit SQL_CD_FALSE. La valeur SQL_CD_TRUE signifie que la connexion a été perdue, tandis que la valeur SQL_CD_FALSE signifie que la connexion est toujours active. (Les pilotes conformes aux versions antérieures d’ODBC peuvent également prendre en charge cet attribut.)  
  
 Un pilote doit implémenter cette option de manière efficace, sans nuire aux performances du regroupement de connexions. Plus précisément, un appel pour obtenir cet attribut de connexion ne doit pas provoquer d’aller-retour vers le serveur. Au lieu de cela, un pilote doit simplement retourner le dernier état connu de la connexion. La connexion est inactive si le dernier voyage au serveur a échoué et n’est pas mort si le dernier voyage a réussi.  
  
## <a name="remarks"></a>Notes  
 Si une connexion a été perdue (signalée via SQL_ATTR_CONNECTION_DEAD), le gestionnaire de pilotes ODBC détruira cette connexion en appelant SQLDisconnect dans le pilote. Les nouvelles demandes de connexion peuvent ne pas trouver de connexion utilisable dans le pool. Finalement, le gestionnaire de pilotes peut établir une nouvelle connexion, en supposant que le pool est vide.  
  
 Pour utiliser un pool de connexions, une application effectue les étapes suivantes :  
  
1.  Active le regroupement de connexions en appelant **SQLSetEnvAttr** pour définir l’attribut d’environnement SQL_ATTR_CONNECTION_POOLING sur SQL_CP_ONE_PER_DRIVER ou SQL_CP_ONE_PER_HENV. Cet appel doit être effectué avant que l’application n’alloue l’environnement partagé pour lequel le groupement de connexions doit être activé. Le handle d’environnement dans l’appel à **SQLSetEnvAttr** doit avoir la valeur null, ce qui rend SQL_ATTR_CONNECTION_POOLING un attribut de niveau processus. Si l’attribut a la valeur SQL_CP_ONE_PER_DRIVER, un seul pool de connexions est pris en charge pour chaque pilote. Si une application fonctionne avec de nombreux pilotes et peu d’environnements, cela peut être plus efficace, car il peut y avoir moins de comparaisons. Si la valeur est SQL_CP_ONE_PER_HENV, un seul pool de connexions est pris en charge pour chaque environnement. Si une application fonctionne avec de nombreux environnements et quelques pilotes, cela peut être plus efficace, car il peut y avoir moins de comparaisons. Le regroupement de connexions est désactivé en définissant SQL_ATTR_CONNECTION_POOLING sur SQL_CP_OFF.  
  
2.  Alloue un environnement en appelant **SQLAllocHandle** avec l’argument *comme handletype* défini sur SQL_HANDLE_ENV. L’environnement alloué par cet appel sera un environnement partagé implicite, car le regroupement de connexions a été activé. Toutefois, l’environnement à utiliser n’est pas déterminé jusqu’à ce que **SQLAllocHandle** avec un *comme HandleType* de SQL_HANDLE_DBC soit appelé sur cet environnement.  
  
3.  Alloue une connexion en appelant **SQLAllocHandle** avec *InputHandle* défini sur SQL_HANDLE_DBC, et le *InputHandle* défini sur le handle d’environnement alloué pour le regroupement de connexions. Le gestionnaire de pilotes tente de trouver un environnement existant qui correspond aux attributs d’environnement définis par l’application. Si aucun de ces environnements n’existe, un tel environnement est créé, avec un nombre de références (géré par le gestionnaire de pilotes) de 1. Si un environnement partagé correspondant est trouvé, l’environnement est retourné à l’application et son décompte de références est incrémenté. (La connexion réelle à utiliser n’est pas déterminée par le gestionnaire de pilotes tant que **SQLConnect** ou **SQLDriverConnect** n’est pas appelé.)  
  
4.  Appelle **SQLConnect** ou **SQLDriverConnect** pour établir la connexion. Le gestionnaire de pilotes utilise les options de connexion dans l’appel à **SQLConnect** (ou les mots clés de connexion dans l’appel à **SQLDriverConnect**) et les attributs de connexion définis après l’allocation de connexion pour déterminer la connexion dans le pool à utiliser.  
  
    > [!NOTE]  
    >  La façon dont une connexion demandée est mise en correspondance avec une connexion regroupée est déterminée par l’attribut d’environnement SQL_ATTR_CP_MATCH. Pour plus d’informations, consultez [SQLSetEnvAttr](../../../odbc/reference/syntax/sqlsetenvattr-function.md).  
  
     Les applications ODBC utilisant le regroupement de connexions doivent appeler [CoInitializeEx](/windows/win32/api/combaseapi/nf-combaseapi-coinitializeex) pendant l’initialisation de l’application et [CoUninitialize](/windows/win32/api/combaseapi/nf-combaseapi-couninitialize) lorsque l’application se ferme.  
  
5.  Appelle **SQLDisconnect** lorsqu’il est terminé avec la connexion. La connexion est retournée au pool de connexions et devient disponible en vue de sa réutilisation.  
  
 Pour une discussion détaillée, consultez [regroupement dans les composants d’accès aux données Microsoft](/previous-versions/ms810829(v=msdn.10)).  
  
## <a name="connection-pooling-considerations"></a>Considérations relatives au regroupement de connexions  
 L’exécution de l’une des actions suivantes à l’aide d’une commande SQL (plutôt que par le biais de l’API ODBC) peut affecter l’état de la connexion et provoquer des problèmes inattendus lorsque le regroupement de connexions est actif :  
  
-   Ouverture d’une connexion et modification de la base de données par défaut.  
  
-   Utilisation de l’instruction SET pour modifier toutes les options configurables (notamment SET ROWCOUNT, ANSI_NULL, IMPLICIT_TRANSACTIONS, SHOWPLAN, STATISTICs, TEXTSIZE et DATEFORMAT).  
  
-   Création de tables temporaires et de procédures stockées.  
  
 Si l’une de ces actions est effectuée en dehors de l’API ODBC, la personne suivante qui utilise la connexion héritera automatiquement des paramètres, tables ou procédures précédents.  
  
> [!NOTE]  
>  Ne vous attendez pas à ce que certains paramètres soient présents dans l’état de la connexion. Vous devez toujours définir l’état de la connexion dans votre application et vous assurer que l’application supprime tous les paramètres de regroupement de connexions inutilisés.  
  
## <a name="driver-aware-connection-pooling"></a>Regroupement de connexions prenant en charge les pilotes  
 À compter de Windows 8, un pilote ODBC peut utiliser des connexions dans le pool de manière plus efficace. Pour plus d’informations, consultez [regroupement de connexions prenant en charge les pilotes](../../../odbc/reference/develop-app/driver-aware-connection-pooling.md).  
  
## <a name="see-also"></a>Voir aussi  
 [Connexion à une source de données ou à un pilote](../../../odbc/reference/develop-app/connecting-to-a-data-source-or-driver.md)   
 [Développement d’un pilote ODBC](../../../odbc/reference/develop-driver/developing-an-odbc-driver.md)   
 [Regroupement dans les composants Microsoft Data Access](/previous-versions/ms810829(v=msdn.10))