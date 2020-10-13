---
description: Utilisation des SID de service pour accorder des autorisations aux services dans SQL Server
title: Utilisation des SID de service pour accorder des autorisations aux services
ms.custom: seo-dt-2019
author: randomnote1
ms.author: dareist
ms.date: 05/02/2019
ms.topic: conceptual
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.openlocfilehash: 0174ce5aae88406719fbf57c53734d535476a799
ms.sourcegitcommit: 4d370399f6f142e25075b3714e5c2ce056b1bfd0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91868151"
---
# <a name="using-service-sids-to-grant-permissions-to-services-in-sql-server"></a>Utilisation des SID de service pour accorder des autorisations aux services dans SQL Server

SQL Server utilise [identificateurs de sécurité (SID) par service](https://support.microsoft.com/help/2620201/sql-server-uses-a-service-sid-to-provide-service-isolation) pour permettre d’accorder directement les autorisations à un service spécifique. Cette méthode est utilisée par SQL Server pour accorder des autorisations aux services de moteur et d’agent (NT SERVICE\MSSQL$<InstanceName> et NT SERVICE\SQLAGENT$<InstanceName> respectivement). Avec cette méthode, ces services peuvent accéder au moteur de base de données uniquement lorsque les services sont en cours d’exécution.

Cette méthode peut aussi être utilisée lorsque vous accordez des autorisations à d’autres services. Avec l’ID de sécurité de service, vous éliminez la contrainte de gérer et maintenir les comptes de service et fournissez un contrôle plus strict et granulaire sur les autorisations accordées aux ressources système.

Voici quelques exemples de services où un ID de sécurité de service peut être utilisé :

- Service de contrôle d’intégrité d’Operations Manager (NT SERVICE\HealthService)
- Service de clustering de basculement Windows Server (WSFC) (NT SERVICE\ClusSvc)

Certains services n’ont pas d’ID de sécurité de service par défaut. L’ID de sécurité de service doit être créé à l’aide de [SC.exe](/windows/desktop/services/configuring-a-service-using-sc). [Cette méthode](https://kevinholman.com/2016/08/25/sql-mp-run-as-accounts-no-longer-required/) a été adoptée par les administrateurs de Microsoft Operations Manager afin d’accorder l’autorisation à HealthService au sein de SQL server.

Une fois l’ID créé et confirmé, il doit être autorisé dans SQL Server. Cela se fait en créant une connexion Windows à l’aide de [SQL Server Management Studio (SSMS)](../../ssms/download-sql-server-management-studio-ssms.md) ou d’une requête. Une fois que la connexion est créée, les autorisations peuvent être octroyées, ajoutées aux rôles et mappées aux bases de données comme toute autre connexion.

> [!TIP]
> Si l’erreur `Login failed for user 'NT AUTHORITY\SYSTEM'` se produit, vérifiez si l’ID de sécurité de service existe pour le service souhaité, la connexion ID de sécurité de service a été créée dans SQL Server, et les autorisations appropriées ont été accordées pour l’ID de sécurité de service dans SQL Server.

## <a name="security"></a>Sécurité

### <a name="eliminate-service-accounts"></a>Éliminer les comptes de service

En règle générale, les comptes de service sont utilisés pour permettre aux services de se connecter à SQL Server. Les comptes de service ajoutent une couche supplémentaire de complexité de gestion car il est nécessaire de maintenir et mettre à jour régulièrement le mot de passe de compte de service. En outre, les informations d’identification du compte de service peuvent être utilisées par un individu afin de tenter de masquer ses activités lorsqu’il exécute des actions dans l’instance.

### <a name="granular-permissions-to-system-accounts"></a>Autorisations granulaires aux comptes système

Les comptes système disposent historiquement d’autorisations en créant une connexion pour les comptes [LocalSystem](/windows/win32/services/localsystem-account) ([NT AUTHORITY\SYSTEM en en-us](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Localized_service_names)) ou [NetworkService](/windows/desktop/Services/networkservice-account) ([ NT AUTHORITY\NETWORK SERVICE en en-us](../../database-engine/configure-windows/configure-windows-service-accounts-and-permissions.md#Localized_service_names)) et en octroyant des autorisations à ces connexions. Cette méthode autorise l’accès à n’importe quel processus ou service dans SQL, qui s’exécute comme un compte système.

L’utilisation d’un Id de sécurité de service permet d’octroyer des autorisations à un service spécifique. Le service a uniquement accès aux ressources pour lesquelles des autorisations ont été accordées lorsqu’il s’exécute. Par exemple, si `HealthService` est en cours d’exécution en tant que `LocalSystem` et bénéficie de `View Server State`, le compte `LocalSystem` aura uniquement l’autorisation `View Server State` lorsqu’il est en cours d’exécution dans le contexte de `HealthService`. Si un autre processus tente d’accéder à l’état du serveur SQL en tant que `LocalSystem`, il n’aura pas accès.

## <a name="examples"></a>Exemples

### <a name="a-create-a-service-sid"></a>R. Créer un ID de service

La commande PowerShell suivante crée un SID de service au niveau service de contrôle d’intégrité d’Operations Manager.

```PowerShell
sc.exe --% sidtype "HealthService" unrestricted
```

> [!IMPORTANT]
> `--%` indique à PowerShell d’arrêter d’analyser le reste de la commande. Cela est utile lorsque vous utilisez des applications et des commandes héritées.

### <a name="b-query-a-service-sid"></a>B. Interroger un ID de sécurité de service

Pour vérifier un ID de sécurité de service ou pour s’assurer de son existence, exécutez la commande suivante dans PowerShell.

```PowerShell
sc.exe --% qsidtype "HealthService"
```

> [!IMPORTANT]
> `--%` indique à PowerShell d’arrêter d’analyser le reste de la commande. Cela est utile lorsque vous utilisez des applications et des commandes héritées.

### <a name="c-add-a-newly-created-service-sid-as-a-login"></a>C. Ajouter un nouvel ID de sécurité de service en tant que Connexion

L’exemple suivant crée une connexion pour le service de contrôle d’intégrité d’Operations Manager à l’aide de T-SQL.

```SQL
CREATE LOGIN [NT SERVICE\HealthService] FROM WINDOWS
GO
```

### <a name="d-add-an-existing-service-sid-as-a-login"></a>D. Ajouter un SID de service existant en tant que connexion

L’exemple suivant crée une connexion pour le service de cluster à l’aide de T-SQL. L’octroi d’autorisations pour le service de cluster élimine directement la nécessité d’accorder des autorisations excessives au compte système.

```SQL
CREATE LOGIN [NT SERVICE\ClusSvc] FROM WINDOWS
GO
```

### <a name="e-grant-permissions-to-a-service-sid"></a>E. Accorder des autorisations au SID de service

Accordez les autorisations requises pour gérer les groupes de disponibilité pour le service de cluster.

```SQL
GRANT ALTER ANY AVAILABILITY GROUP TO [NT SERVICE\ClusSvc]
GO

GRANT CONNECT SQL TO [NT SERVICE\ClusSvc]
GO

GRANT VIEW SERVER STATE TO [NT SERVICE\ClusSvc]
GO
```

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur la structure du sid de service, consultez [structure de SERVICE_SID_INFO](/windows/win32/api/winsvc/ns-winsvc-service_sid_info).

Découvrez les options supplémentaires disponibles lors de la [création d’une connexion](../../t-sql/statements/create-login-transact-sql.md).

Pour utiliser la sécurité basée sur les rôles avec les SID de service, consultez la [création de rôles](../../t-sql/statements/create-role-transact-sql.md) dans SQL Server.

Découvrez les différentes façons d’[accorder des autorisations](../../t-sql/statements/grant-transact-sql.md) aux SID de service dans SQL Server.