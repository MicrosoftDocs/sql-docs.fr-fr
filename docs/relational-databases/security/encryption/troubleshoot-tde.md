---
title: Erreurs courantes liées aux clés gérées par le client dans Azure Key Vault
description: Découvrez comment identifier et résoudre les problèmes d’accès et les erreurs courantes avec le chiffrement TDE (Transparent Data Encryption) et les clés gérées par le client dans Azure Key Vault.
ms.custom: seo-lt-2019
helpviewer_keywords:
- troublshooting, tde akv
- tde akv configuration, troubleshooting
- tde troubleshooting
author: jaszymas
ms.prod: sql
ms.technology: security
ms.reviewer: vanto
ms.topic: conceptual
ms.date: 11/06/2019
ms.author: jaszymas
monikerRange: = azuresqldb-current || = azure-sqldw-latest
ms.openlocfilehash: 033346cf1d00d1f2acbd6b8de0796027c80814a7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100345374"
---
# <a name="common-errors-for-transparent-data-encryption-with-customer-managed-keys-in-azure-key-vault"></a>Erreurs courantes liées au chiffrement transparent des données avec des clés managées par le client dans Azure Key Vault

[!INCLUDE[asdb-asdbmi-asa](../../../includes/applies-to-version/asdb-asdbmi-asa.md)]

Cet article explique comment identifier et résoudre les problèmes d’accès à la clé Azure Key Vault ayant configuré une base de données pour utiliser [Transparent Data Encryption (TDE) avec les clés managées par le client dans Azure Key Vault](/azure/sql-database/transparent-data-encryption-byok-azure-sql) pour devenir inaccessible.

## <a name="introduction"></a>Introduction
Lorsque TDE est configuré pour utiliser une clé managée par le client dans Azure Key Vault, l’accès en continu à ce protecteur TDE est nécessaire pour que la base de données reste en ligne.  Si le serveur SQL logique perd l’accès au protecteur TDE managé par le client dans Azure Key Vault, une base de données se mettra à rejeter toutes les connexions en affichant le message d’erreur associé, et son état passera à *Inaccessible* dans le portail Azure.

Dans les 8 premières heures, si le problème d’accès à la clé Azure Key Vault sous-jacent est résolu, la base de données est automatiquement corrigée et mise en ligne. Cela signifie que pour tous les scénarios de panne réseau intermittente et temporaire, aucune action de l’utilisateur n’est requise et la base de données est automatiquement mise en ligne. Dans la plupart des cas, l’action de l’utilisateur est nécessaire pour résoudre le problème d’accès à la clé du coffre de clés sous-jacent. 

Si une base de données inaccessible n’est plus nécessaire, elle peut être supprimée immédiatement pour arrêter les coûts. Toutes les autres actions sur la base de données ne sont pas autorisées tant que l’accès à la clé Azure Key Vault n’a pas été restauré et que la base de données est de nouveau en ligne. La modification de l’option TDE à partir des clés managées par le client sur le serveur n’est pas non plus possible lorsqu’une base de données chiffrée avec des clés managées par le client est inaccessible. Cela est nécessaire pour protéger les données contre tout accès non autorisé, tandis que les autorisations sur le protecteur TDE ont été révoquées. 

Lorsqu’une base de données est inaccessible pendant plus de 8 heures, elle ne peut plus être corrigée automatiquement. Si l’accès à la clé Azure Key Vault a été restauré après cette période, vous devez revalider manuellement l’accès à la clé pour remettre la base de données en ligne. Dans ce cas, la remise en ligne de la base de données peut être très longue, selon la taille de celle-ci. Une fois la base de données de nouveau en ligne, les paramètres précédemment configurés tels que le [groupe de basculement](/azure/sql-database/sql-database-auto-failover-group) ou l’historique de récupération jusqu’à une date et heure, ainsi que toutes les étiquettes, **seront perdus**. Par conséquent, nous vous recommandons d’implémenter un système de notifications à l’aide de [groupes d’actions](/azure/azure-monitor/platform/action-groups) qui permettent d’être informé et de traiter les problèmes sous-jacents d’accès aux clés dès que possible. 

## <a name="common-errors-causing-databases-to-become-inaccessible"></a>Erreurs courantes provoquant l’inaccessibilité des bases de données

La plupart des problèmes qui se produisent lorsque vous utilisez TDE avec Key Vault sont provoquées par l’une des erreurs de configuration suivantes :

### <a name="the-key-vault-is-unavailable-or-doesnt-exist"></a>Le coffre de clés n’est pas disponible ou n’existe pas

- Le coffre de clés a été supprimé accidentellement.
- Le pare-feu a été configuré pour Azure Key Vault mais n’autorise pas l’accès aux services Microsoft.
- Une erreur réseau intermittente entraîne l’indisponibilité du coffre de clés.

### <a name="no-permissions-to-access-the-key-vault-or-the-key-doesnt-exist"></a>Aucune autorisation d’accéder au coffre de clés ou la clé n’existe pas

- La clé a été accidentellement supprimée, désactivée ou a expiré.
- L’AppId de l’instance SQL Server logique a été supprimé accidentellement.
- L’instance SQL Server logique a été déplacée vers un autre abonnement. Un nouvel AppId doit être créé si le serveur logique est déplacé vers un autre abonnement.
- Les autorisations accordées à l’AppId pour les clés ne sont pas suffisantes (Get, Wrap et Unwrap ne sont pas inclus).
- Les autorisations pour l’AppId de l’instance SQL Server logique ont été révoquées.

## <a name="identify-and-resolve-common-errors"></a>Identifier et résoudre les erreurs courantes

Dans cette section, nous répertorions les étapes de résolution des problèmes pour les erreurs les plus courantes.

### <a name="missing-server-identity"></a>Identité de serveur manquante

**Message d’erreur**

_401 AzureKeyVaultNoServerIdentity - L’identité du serveur n’est pas correctement configurée sur le serveur. Contactez le support._

**Détection**

Utilisez la cmdlet ou commande suivante pour vous assurer qu’une identité a bien été attribuée à l’instance SQL Server logique :

- Azure PowerShell : [Get-AzureRMSqlServer](/powershell/module/AzureRM.Sql/Get-AzureRmSqlServer) 

- Azure CLI : [az-sql-server-show](/cli/azure/sql/server#az-sql-server-show)

**Atténuation**

Utilisez la cmdlet ou commande suivante pour configurer une identité Azure AD (AppId) pour l’instance SQL Server logique :

- Azure PowerShell : [Set-AzureRmSqlServer](/powershell/module/azurerm.sql/set-azurermsqlserver) avec l’option `-AssignIdentity`.

- Azure CLI : [az sql server update](/cli/azure/sql/server#az-sql-server-update) avec l’option `--assign_identity`.

Dans le portail Azure, accédez au coffre de clés, puis à **Stratégies d’accès**. Procédez comme suit : 

 1. Utilisez le bouton **Ajouter** pour ajouter l’AppId du serveur créé à l’étape précédente. 
 1. Attribuez les autorisations de clé suivantes : Get, Wrap et Unwrap 

Pour en savoir plus, consultez [Affecter une identité Azure AD à votre serveur](/azure/sql-database/transparent-data-encryption-byok-azure-sql-configure#assign-an-azure-ad-identity-to-your-server).

> [!IMPORTANT]
> Si l’instance SQL Server logique a été transférée vers un nouvel abonné après la configuration initiale de TDE avec Azure Key Vault, répétez l’étape pour configurer l’identité Azure Active Directory pour créer un nouvel ID d’application. Ensuite, ajoutez l’AppId dans le coffre de clés et attribuez les autorisations appropriées à la clé. 
>

### <a name="missing-key-vault"></a>Coffre de clés manquant

**Message d’erreur**

_503 AzureKeyVaultConnectionFailed - Impossible d’effectuer l’opération sur le serveur en raison de l’échec des tentatives de connexion à Azure Key Vault._

**Détection**

Pour identifier l’URI de la clé et le coffre de clé :

1. Utilisez la cmdlet ou commande suivante pour obtenir l’URI de la clé d’une instance SQL Server logique spécifique :

    - Azure PowerShell : [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey)

    - Azure CLI : [az-sql-server-tde-key-show](/cli/azure/sql/server/tde-key#az-sql-server-tde-key-show) 

1. Utilisez l’URI de la clé pour identifier le coffre de clés :

    - Azure PowerShell : Vous pouvez inspecter les propriétés de la variable $MyServerKeyVaultKey pour obtenir plus d’informations sur le coffre de clés.

    - Azure CLI : Inspectez le protecteur de chiffrement de serveur retourné pour plus d’informations sur le coffre de clés.

**Atténuation**

Confirmez que le coffre de clés est disponible :

- Vérifiez que le coffre de clés est disponible et que l’instance SQL Server logique y a accès.
- Si le coffre de clés se trouve derrière un pare-feu, assurez-vous que la case permettant aux services Microsoft d’accéder au coffre de clés est cochée.
- Si le coffre de clés a été supprimé accidentellement, vous devez recommencer la configuration depuis le début.


### <a name="missing-key"></a>Clé manquante

**Messages d’erreur**

_404 ServerKeyNotFound - la clé serveur demandée est introuvable sur l’abonnement actuel._ 

_409 ServerKeyDoesNotExists - la clé du serveur n’existe pas._

**Détection**

Pour identifier l’URI de la clé et le coffre de clé :

- Utilisez la cmdlet ou les commandes dans [Coffre de clés manquant](#missing-key-vault) pour identifier l’URI de la clé qui est ajouté à l’instance SQL Server logique. L’exécution des commandes retourne la liste des clés.

**Atténuation**

Confirmez que le protecteur TDE est présent dans Key Vault :

1. Identifiez le coffre de clés et accédez à celui-ci dans le portail Azure.
1. Vérifiez que la clé identifiée par l’URI de la clé est présente.

### <a name="missing-permissions"></a>Autorisations manquantes

**Message d’erreur**

_401 AzureKeyVaultMissingPermissions - le serveur ne dispose pas des autorisations requises pour Azure Key Vault._

**Détection**

Pour identifier l’URI de la clé et le coffre de clés : 

- Utilisez la cmdlet ou les commandes dans [Coffre de clés manquant](#missing-key-vault) pour identifier le coffre de clés utilisé par l’instance SQL Server logique.

**Atténuation**

Vérifiez que l’instance SQL Server logique dispose des autorisations d’accès pour le coffre de clés et des autorisations appropriées pour accéder à la clé :

- Dans le portail Azure, accédez au coffre de clés > **Stratégies d’accès**. Recherchez l’AppId de l’instance SQL Server logique.  
- Si l’AppId est présent, vérifiez qu’il dispose des autorisations de clé suivantes : Get, Wrap et Unwrap.
- Si l’AppId n’est pas présent, ajoutez-le à l’aide du bouton **Ajouter**. 

## <a name="getting-tde-status-from-the-activity-log"></a>Obtention de l’état TDE depuis le journal d’activité

Pour permettre l’analyse de l’état de la base de données en raison de problèmes d’accès à la clé Azure Key Vault, les événements suivants sont enregistrés dans le [journal d’activité](/azure/service-health/alerts-activity-log-service-notifications) pour l’ID de la ressource en fonction de l’URL et de Subscription+Resourcegroup+ServerName+DatabaseName Azure Resource Manager : 

**Événement lorsque le service perd l’accès à la clé Azure Key Vault**

EventName : MakeDatabaseInaccessible 

État : Démarré 

Description : La base de données a perdu l’accès à la clé du coffre de clés Azure et est désormais inaccessible : <error message>   

 

**Événement lorsque le délai d’attente de 8 heures pour la réparation spontanée commence** 

EventName : MakeDatabaseInaccessible 

État : InProgress 

Description : La base de données attend que l’accès à la clé du coffre de clés Azure soit rétabli par l’utilisateur dans un délai de 8 heures.   

 

**Événement lorsque la base de données est automatiquement remise en ligne**

EventName : MakeDatabaseAccessible 

État : Opération réussie 

Description : L’accès de la base de données à la clé du coffre de clés Azure a été rétabli et la base de données est maintenant en ligne. 

 

**Événement lorsque le problème n’a pas été résolu dans le délai de 8 heures et que l’accès à la clé Azure Key Vault doit être validé manuellement** 

EventName : MakeDatabaseInaccessible 

État : Opération réussie 

Description : La base de données est inaccessible et nécessite que l’utilisateur résolve les erreurs du coffre de clés Azure et rétablisse l’accès à la clé du coffre de clés Azure à l’aide de la revalidation de la clé. 

 

**Événement lorsque la base de données est en ligne après la revalidation manuelle de la clé**

EventName : MakeDatabaseAccessible 

État : Opération réussie 

Description : L’accès de la base de données à la clé du coffre de clés Azure a été rétabli et la base de données est maintenant en ligne. 

 

**Événement lorsque la revalidation de l’accès à la clé Azure Key Vault a réussi et que la base de données est de nouveau en ligne**

EventName : MakeDatabaseAccessible 

État : Démarré 

Description : La restauration de l’accès de la base de données à la clé du coffre de clés Azure a démarré. 

 

**Événement lors de l’échec de la revalidation de l’accès à la clé Azure Key Vault**

EventName : MakeDatabaseAccessible 

État : Échec 

Description : La restauration de l’accès de la base de données à la clé du coffre de clés Azure a échoué. 


## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [Azure Resource Health](/azure/service-health/resource-health-overview).
- Configurez des [groupes d’actions](/azure/azure-monitor/platform/action-groups) pour recevoir des notifications et des alertes en fonction de vos préférences, par exemple e-mail/SMS/transmission de type push/vocale, application logique, Webhook, gestion des services informatiques ou Runbook Automation.