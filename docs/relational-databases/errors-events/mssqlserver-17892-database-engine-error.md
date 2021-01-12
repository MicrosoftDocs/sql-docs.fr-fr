---
description: MSSQLSERVER_17892
title: MSSQLSERVER_17892
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17892 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 905b961e2fbf882f59b050a3acb7ba0f9c2f9046
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099325"
---
# <a name="mssqlserver_17892"></a>MSSQLSERVER_17892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|17892|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|SRV_LOGON_FAILED_BY_TRIGGER|
|Texte du message|L’ouverture de session a échoué pour le nom d’ouverture de session \<Login Name> en raison de l’exécution d’un déclencheur.|
||

## <a name="explanation"></a>Explication

L’erreur 17892 est générée lorsque le code d’un déclencheur LOGON ne peut pas s’exécuter correctement. Les [déclencheurs LOGON](../triggers/logon-triggers.md) lancent des procédures stockées en réponse à un événement LOGON. Cet événement est déclenché lorsqu'une session utilisateur est établie avec une instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Un message d’erreur semblable au suivant s’affiche :

> Msg 17892, Niveau 14, État 1, Serveur \<Server Name>, Ligne 1  
L’ouverture de session a échoué pour le nom d’ouverture de session \<Login Name> en raison de l’exécution d’un déclencheur.

## <a name="possible-causes"></a>Causes possibles

Le problème peut se produire en cas d’erreur lors de l’exécution du code de déclencheur pour ce compte d’utilisateur. Voici certains des scénarios possibles :

- Le déclencheur tente d’insérer des données dans une table qui n’existe pas.
- Le nom d’utilisateur (login) n’a pas d’autorisations sur l’objet qui est référencé par le déclencheur LOGON.

## <a name="user-action"></a>Action requise

Vous pouvez utiliser l’une des solutions ci-dessous en fonction de votre scénario.

- **Scenario 1** : Vous avez actuellement accès à une session ouverte dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sous un compte d’administrateur.

  Dans ce cas, vous pouvez apporter la correction nécessaire pour corriger le code de votre déclencheur.

  - Exemple 1 : S’il n’existe pas d’objet référencé par le code du déclencheur, créez-en un afin que le déclencheur LOGIN puisse s’exécuter correctement.

  - Exemple 2 : S’il existe un objet référencé par le code du déclencheur, mais que les utilisateurs n’ont pas d’autorisations, accordez-leur les privilèges nécessaires pour accéder à l’objet.  
  
  Vous pouvez également supprimer ou désactiver le déclencheur LOGIN afin que les utilisateurs puissent continuer à se connecter à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

- **Scénario 2** : Aucune session active n’est ouverte avec des privilèges d’administrateur, mais une connexion administrateur dédiée (DAC) est activée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    Dans ce cas, vous pouvez utiliser la connexion DAC pour effectuer les mêmes étapes que celles décrites dans le cas 1, car les connexions DAC ne sont pas affectées par les déclencheurs LOGIN. Pour plus d’informations sur les connexions DAC, consultez : [Connexion de diagnostic pour les administrateurs de base de données](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md).

    Pour vérifier si la connexion DAC est activée dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], vous pouvez rechercher dans le journal des erreurs [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] un message similaire à celui-ci :

    > 2020-02-09 16:17:44.150 La prise en charge de connexion administrateur dédiée a été établie pour l’écoute locale sur le port 1434.  

- **Scénario 3** : La connexion DAC n’est pas activée sur votre serveur et vous ne disposez pas d’une session administrateur existante pour [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

    Dans ce scénario, le seul moyen de résoudre le problème consiste à effectuer les étapes suivantes :
  
    1. Arrêtez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et les services connexes.
    2. Démarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à partir de l’[invite de commandes](/previous-versions/sql/sql-server-2008-r2/ms180965(v=sql.105)), à l’aide des paramètres de démarrage `-c`, `-m` et `-f`. Cela désactive le déclencheur LOGIN et vous permet d’appliquer les mêmes mesures correctives présentées dans le **Cas 1** ci-dessus.
  
        > [!NOTE]
        > La procédure ci-dessus nécessite un compte *Administrateur système* ou un compte Administrateur équivalent.
  
         Pour plus d’informations sur ces comptes et sur les autres options de démarrage, consultez : [Options de démarrage du service moteur de base de données](../../database-engine/configure-windows/database-engine-service-startup-options.md).

## <a name="more-information"></a>Informations complémentaires

Une autre situation dans laquelle les déclencheurs LOGON échouent est lorsque vous utilisez la fonction `EVENTDATA`. Cette fonction retourne du code XML et respecte la casse.  Si vous créez le déclencheur LOGON suivant, avec l’intention de bloquer l’accès en fonction de l’adresse IP, vous pouvez rencontrer le problème suivant :

``` sql
 CREATE TRIGGER tr_logon_CheckIP  
 ON ALL SERVER  
 FOR LOGON  
 AS
 BEGIN
  IF IS_SRVROLEMEMBER ( 'sysadmin' ) = 1  
     BEGIN
         DECLARE @IP NVARCHAR ( 15 );  
         SET @IP = ( SELECT EVENTDATA ().value ( '(/EVENT_INSTANCE/ClientHost)[1]' , 'NVARCHAR(15)' ));  
         IF NOT EXISTS( SELECT IP FROM DBAWork.dbo.ValidIP WHERE IP = @IP )  
         ROLLBACK ;  
     END ;  
 END ;  
 GO
```

L’utilisateur n’a pas respecté la casse lorsqu’il a copié ce script à partir d’Internet dans cette partie du déclencheur :

```sql
 SELECT EVENTDATA ().value ( '(/event_instance/clienthost)[1]' , 'NVARCHAR(15)' ));  
```

Par conséquent, `EVENTDATA` a toujours retourné **NULL**, et toutes ses connexions d’administrateur système équivalentes se sont vu refuser l’accès. Dans ce cas, la connexion DAC n’a pas été activée. Nous avons donc été obligés de redémarrer le serveur avec les paramètres de démarrage listés ci-dessus afin de supprimer le déclencheur.