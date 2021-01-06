---
title: Créer un point de terminaison de mise en miroir de bases de données (Transact-SQL)
description: Utilisez Transact-SQL pour créer un point de terminaison de mise en miroir de bases de données avec l’authentification Windows dans SQL Server.
ms.custom: seo-lt-2019
ms.date: 05/17/2016
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- database mirroring [SQL Server], deployment
- database mirroring [SQL Server], endpoint
- endpoints [SQL Server], database mirroring
- Windows authentication [SQL Server]
- database mirroring [SQL Server], security
ms.assetid: baf1a4b1-6790-4275-b261-490bca33bdb9
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 4a05341397185deefcc6de5d25ded34e910d7fac
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97642145"
---
# <a name="create-a-database-mirroring-endpoint-for-windows-authentication-transact-sql"></a>Créer un point de terminaison de mise en miroir de bases de données pour l'authentification Windows (Transact-SQL)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  Cette rubrique explique comment créer un point de terminaison de mise en miroir de bases de données qui utilise l'authentification Windows dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]. Pour prendre en charge la mise en miroir de bases de données ou [!INCLUDE[ssHADR](../../includes/sshadr-md.md)] , chaque instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nécessite un point de terminaison de mise en miroir de bases de données. Une instance de serveur ne peut disposer que d'un seul point de terminaison de mise en miroir de bases de données, lequel possède un port unique. Un point de terminaison de mise en miroir de bases de données peut utiliser n'importe quel point disponible sur le système local lors de la création du point de terminaison. Toutes les sessions de mise en miroir des bases de données sur une instance de serveur écoutent ce port, et toutes les connexions entrantes pour la mise en miroir des bases de données utilisent ce port.  
  
> [!IMPORTANT]  
>  Si un point de terminaison de mise en miroir de bases de données existe et est déjà utilisé, nous vous recommandons de l'utiliser. La suppression d'un point de terminaison en cours d'utilisation interrompt les sessions existantes.  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  [Sécurité](#Security)  
  
-   **Pour créer un point de terminaison de mise en miroir de bases de données à l’aide de**  [Transact-SQL](#TsqlProcedure)  
  
##  <a name="before-you-begin"></a><a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
 Les méthodes d'authentification et de chiffrement d'instance de serveur sont établies par l'administrateur système.  
  
> [!WARNING]  
>  L'algorithme RC4 est déconseillé. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Nous vous recommandons d'utiliser AES.  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Requiert l’autorisation `CREATE ENDPOINT` ou l’appartenance au rôle serveur fixe `sysadmin`. Pour plus d’informations, consultez [Autorisations de point de terminaison GRANT &#40;Transact-SQL&#41;](../../t-sql/statements/grant-endpoint-permissions-transact-sql.md).  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-database-mirroring-endpoint-that-uses-windows-authentication"></a>Pour créer un point de terminaison de mise en miroir de bases de données qui utilise l'authentification Windows  
  
1.  Connectez-vous à l'instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] sur laquelle vous voulez créer un point de terminaison de mise en miroir de base de données.  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Déterminez s'il existe déjà un point de terminaison de mise en miroir de base de données à l'aide de l'instruction suivante :  
  
    ```sql  
    SELECT name, role_desc, state_desc FROM sys.database_mirroring_endpoints;   
    ```  
  
    > [!IMPORTANT]  
    >  S'il existe déjà un point de terminaison de mise en miroir de base de données pour l'instance de serveur, utilisez-le pour toutes les autres sessions établies sur l'instance de serveur.  
  
4.  Pour utiliser Transact-SQL afin de créer un point de terminaison qui sera utilisé avec l’authentification Windows, utilisez une instruction `CREATE ENDPOINT`. L'instruction prend la forme générale suivante :  
  
     ```syntaxsql
     CREATE ENDPOINT *\<endpointName>*  
  
     STATE=STARTED  
  
     AS TCP ( LISTENER_PORT = *\<listenerPortList>* )  
  
     FOR DATABASE_MIRRORING  
  
     (  
  
     [ AUTHENTICATION = **WINDOWS** [ *\<authorizationMethod>* ]  
  
     ]  
  
     [ [**,**] ENCRYPTION = **REQUIRED**  
  
     [ ALGORITHM { *\<algorithm>* } ]  
  
     ]  
  
     [**,**] ROLE = *\<role>*  
  
     )  
     ```
     
     Où :  
  
    -   *\<endpointName>* représente le nom unique du point de terminaison de mise en miroir de base de données de l'instance de serveur.  
  
    -   STARTED spécifie que le point de terminaison doit être démarré et commencer à écouter les connexions. Un point de terminaison de mise en miroir de base de données est en général créé dans l'état STARTED. D'une autre manière, vous pouvez démarrer une session dans un état STOPPED (par défaut) ou DISABLED.  
  
    -   *\<listenerPortList>* est un numéro de port unique (*nnnn*) sur lequel le serveur doit écouter les messages de mise en miroir de base de données. Seul le protocole TCP est autorisé ; la spécification d'un autre protocole produit une erreur.  
  
         Un numéro de port peut servir une fois seulement pour chaque ordinateur. Un point de terminaison de mise en miroir de bases de données peut utiliser n'importe quel point disponible sur le système local lors de la création du point de terminaison. Pour identifier les ports en cours d'utilisation par les points de terminaison TCP, utilisez l'instruction Transact-SQL suivante :  
  
        ```sql  
        SELECT name, port FROM sys.tcp_endpoints;  
        ```  
  
        > [!IMPORTANT]  
        >  Chaque instance de serveur nécessite un port d'écoute et un seul.  
  
    -   Pour l'authentification Windows, l'option AUTHENTICATION est facultative, à moins que vous vouliez que le point de terminaison utilise uniquement NTLM ou Kerberos pour authentifier les connexions. *\<authorizationMethod>* spécifie la méthode utilisée pour authentifier les connexions parmi l’une des suivantes : NTLM, KERBEROS ou NEGOTIATE. NEGOTIATE, méthode par défaut, impose au point de terminaison d'utiliser le protocole de négociation Windows pour choisir NTLM ou Kerberos. La négociation active les connexions avec ou sans authentification, selon le niveau d'authentification du point de terminaison opposé.  
  
    -   ENCRYPTION est défini sur REQUIRED par défaut. En d'autres termes, toutes les connexions à ce point de terminaison doivent utiliser le chiffrement. Toutefois, vous pouvez désactiver le chiffrement ou le rendre facultatif sur un point de terminaison. Les alternatives sont les suivantes :  
  
        |Valeur|Définition|  
        |-----------|----------------|  
        |DISABLED|Spécifie que les données envoyées sur une connexion ne sont pas chiffrées.|  
        |SUPPORTED|Spécifie que les données sont chiffrées uniquement si le point de terminaison opposé spécifie SUPPORTED ou REQUIRED.|  
        |REQUIRED|Spécifie que les données envoyées sur une connexion doivent être chiffrées.|  
  
         Si un point de terminaison exige un chiffrement, l'autre point de terminaison doit avoir l'instruction ENCRYPTION définie sur SUPPORTED ou REQUIRED.  
  
    -   *\<algorithm>* propose la possibilité de spécifier les normes de chiffrement du point de terminaison. La valeur de *\<algorithm>* peut être l'un des algorithmes ou combinaisons d'algorithmes suivants : RC4, AES, AES RC4 ou RC4 AES.  
  
         AES RC4 spécifie que ce point de terminaison va négocier l'algorithme de chiffrement et donner la préférence à l'algorithme AES. RC4 AES spécifie que ce point de terminaison va négocier l'algorithme de chiffrement et donner la préférence à l'algorithme RC4. Si les deux points de terminaison spécifient les deux algorithmes mais dans des ordres différents, le point de terminaison acceptant la connexion a le dernier mot. Entrez le même algorithme de manière explicite pour éviter les erreurs de connexion entre les différents serveurs.
  
        > [!WARNING]  
        >  L'algorithme RC4 est déconseillé. [!INCLUDE[ssNoteDepFutureDontUse](../../includes/ssnotedepfuturedontuse-md.md)] Nous vous recommandons d'utiliser AES.  
  
    -   *\<role>* définit le ou les rôles que le serveur peut endosser. La spécification de ROLE est obligatoire. Toutefois, le rôle du point de terminaison concerne uniquement la mise en miroir de bases de données. Pour [!INCLUDE[ssHADR](../../includes/sshadr-md.md)], le rôle du point de terminaison est ignoré.  
  
         Pour qu'une instance de serveur puisse occuper un rôle pour une session de mise en miroir de base de données et un rôle différent pour une autre session, spécifiez ROLE=ALL. Pour restreindre la fonction d’une instance de serveur à partenaire ou à témoin, spécifiez `ROLE = PARTNER` ou `ROLE = WITNESS`, respectivement.  
  
        > [!NOTE]  
        >  Pour plus d’informations sur les options de mise en miroir de bases de données des différentes éditions de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], consultez [Fonctionnalités prises en charge par les éditions de SQL Server 2016](~/sql-server/editions-and-supported-features-for-sql-server-2016.md).  
  
     Pour une description complète de la syntaxe de CREATE ENDPOINT, consultez [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md).  
  
    > [!NOTE]  
    >  Pour modifier un point de terminaison existant, utilisez [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md).  
  
###  <a name="example-creating-endpoints-to-support-for-database-mirroring-transact-sql"></a><a name="TsqlExample"></a> Exemple : Création de points de terminaison pour prendre en charge la mise en miroir de bases de données (Transact-SQL)  
 L'exemple suivant crée des points de terminaison de mise en miroir de bases de données pour les instances de serveur par défaut sur trois systèmes informatiques distincts :  
  
|Rôle de l'instance de serveur|Nom de l'ordinateur hôte|  
|-----------------------------|---------------------------|  
|Partenaire (au départ, dans le rôle de principal)|`SQLHOST01\.`|  
|Partenaire (au départ, dans le rôle de serveur miroir)|`SQLHOST02\.`|  
|Témoin|`SQLHOST03\.`|  
  
 Dans cet exemple, les trois points de terminaison utilisent le numéro de port 7022, bien que n'importe quel autre numéro de port disponible pourrait convenir. L'option AUTHENTICATION est inutile puisque les points de terminaison utilisent le type par défaut, c'est-à-dire l'authentification Windows. L'option ENCRYPTION est également inutile dans la mesure où la négociation de la méthode d'authentification d'une connexion est prévue sur tous les points de terminaison, ce qui correspond au comportement par défaut de l'authentification Windows. Également caractéristique de ce comportement par défaut, le chiffrement est obligatoire sur tous les points de terminaison.  
  
 Chaque instance de serveur est limitée à un rôle exclusif de partenaire ou de témoin, ce rôle est expressément spécifié par le point de terminaison de chaque serveur (ROLE=PARTNER ou ROLE=WITNESS).  
  
> [!IMPORTANT]  
>  Chaque instance de serveur ne peut disposer que d'un seul point de terminaison. Ainsi, pour utiliser une instance de serveur tour à tour comme partenaire dans certaines sessions ou témoin dans d'autres, spécifiez ROLE=ALL.  
  
```sql  
--Endpoint for initial principal server instance, which  
--is the only server instance running on SQLHOST01.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for initial mirror server instance, which  
--is the only server instance running on SQLHOST02.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=PARTNER);  
GO  
--Endpoint for witness server instance, which  
--is the only server instance running on SQLHOST03.  
CREATE ENDPOINT endpoint_mirroring  
    STATE = STARTED  
    AS TCP ( LISTENER_PORT = 7022 )  
    FOR DATABASE_MIRRORING (ROLE=WITNESS);  
GO  
```  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> Tâches associées  
 **Pour configurer un point de terminaison de mise en miroir de bases de données**  
  
-   [Créer un point de terminaison de mise en miroir de bases de données pour les groupes de disponibilité Always On &#40;SQL Server PowerShell&#41;](../../database-engine/availability-groups/windows/database-mirroring-always-on-availability-groups-powershell.md)  
  
-   [Utiliser des certificats pour un point de terminaison de mise en miroir de bases de données &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/use-certificates-for-a-database-mirroring-endpoint-transact-sql.md)  
  
    -   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions sortantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-outbound-connections.md)  
  
    -   [Autoriser un point de terminaison de mise en miroir de bases de données à utiliser des certificats pour les connexions entrantes &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/database-mirroring-use-certificates-for-inbound-connections.md)  
  
-   [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)  
  
-   [Spécifier l’URL de point de terminaison lors de l’ajout ou lors de la modification d’un réplica de disponibilité &#40;SQL Server&#41;](../../database-engine/availability-groups/windows/specify-endpoint-url-adding-or-modifying-availability-replica.md)  
  
 **Pour afficher des informations sur le point de terminaison de mise en miroir de bases de données**  
  
-   [sys.database_mirroring_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-database-mirroring-endpoints-transact-sql.md)  
  
## <a name="see-also"></a>Voir aussi  
 [ALTER ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/alter-endpoint-transact-sql.md)   
 [Choisir un algorithme de chiffrement](../../relational-databases/security/encryption/choose-an-encryption-algorithm.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [Spécifier une adresse réseau de serveur &#40;mise en miroir de bases de données&#41;](../../database-engine/database-mirroring/specify-a-server-network-address-database-mirroring.md)   
 [Exemple : Configuration de la mise en miroir de bases de données à l’aide de l’authentification Windows &#40;Transact-SQL&#41;](../../database-engine/database-mirroring/example-setting-up-database-mirroring-using-windows-authentication-transact-sql.md)   
 [Point de terminaison de mise en miroir de bases de données &#40;SQL Server&#41;](../../database-engine/database-mirroring/the-database-mirroring-endpoint-sql-server.md)  
  
  

