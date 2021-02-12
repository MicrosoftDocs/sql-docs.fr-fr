---
title: Connecter des instances SQL Server à Azure Arc à grande échelle
titleSuffix: ''
description: Dans cet article, vous allez apprendre à connecter des instances SQL Server comme des serveurs SQL dotés d’Azure Arc en utilisant un principal de service.
author: anosov1960
ms.author: sashan
ms.reviewer: mikeray
ms.date: 09/10/2020
ms.topic: conceptual
ms.prod: sql
ms.openlocfilehash: b78c76504cb9ffb76b62c1867630760fe52cf22c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100063184"
---
# <a name="connect-sql-server-instances-to-azure-arc-at-scale"></a>Connecter des instances SQL Server à Azure Arc à grande échelle

Vous pouvez connecter plusieurs instances SQL Server installées sur plusieurs machines Windows ou Linux à Azure Arc en utilisant le [script généré pour une seule machine](connect.md). Le script se connecte et inscrit chaque machine et les instances SQL Server installées sur celle-ci à Azure Arc. Pour une expérience optimale, nous vous recommandons d’utiliser un [principal de service](/azure/active-directory/develop/app-objects-and-service-principals) Azure Active Directory. Un principal de service est une identité de gestion limitée spéciale qui ne dispose que de l’autorisation minimale nécessaire pour connecter des machines à Azure et pour créer les ressources Azure pour un serveur ou serveur SQL doté d’Azure Arc. Cela est plus sûr que d’utiliser un compte doté de privilèges plus élevés comme un Administrateur client et respecte nos meilleures pratiques en matière de sécurité du contrôle d’accès.  

Les méthodes d’installation pour installer et configurer l’agent Connected Machine requièrent que la méthode automatisée que vous utilisez dispose des autorisations d’administrateur sur les ordinateurs. Vous utilisez le compte root sur Linux et un membre du groupe Administrateurs local sur Windows.

Avant de commencer, consultez les [prérequis](overview.md#prerequisites) et assurez-vous que vous avez créé un rôle personnalisé correspondant aux autorisations requises.

## <a name="connecting-multiple-sql-server-instances-on-windows-using-azure-powershell"></a>Connexion de plusieurs instances SQL Server sur Windows à l’aide d’Azure PowerShell

[Azure PowerShell](/powershell/azure/install-az-ps) doit être installé sur chaque machine.

1. Créez le principal de service à l’aide de PowerShell et de la commande [`New-AzADServicePrincipal`](/powershell/module/az.resources/new-azadserviceprincipal). Veillez à stocker la sortie dans une variable. Dans le cas contraire, vous ne pourrez pas récupérer le mot de passe nécessaire ultérieurement.

    ```azurepowershell-interactive
    $sp = New-AzADServicePrincipal -DisplayName "Arc-for-servers" -Role <your custom role>
    $sp
    ```

    ```output
    Secret                : System.Security.SecureString
    ServicePrincipalNames : {ad9bcd79-be9c-45ab-abd8-80ca1654a7d1, https://Arc-for-servers}
    ApplicationId         : ad9bcd79-be9c-45ab-abd8-80ca1654a7d1
    ObjectType            : ServicePrincipal
    DisplayName           : Hybrid-RP
    Id                    : 5be92c87-01c4-42f5-bade-c1c10af87758
    Type                  :
    ```

   > [!NOTE]
   > Quand vous créez un principal de service, votre compte doit être Propriétaire ou Administrateur de l’accès utilisateur sur l’abonnement à utiliser pour l’intégration. Si vous n’avez pas les autorisations appropriées pour créer les attributions de rôles, le principal de service sera créé, mais il ne pourra pas intégrer de machines. Les instructions relatives à la création d’un rôle personnalisé sont fournies dans [Autorisations requises](overview.md#required-permissions).

2. Récupérez le mot de passe stocké dans la variable `$sp` :

   ```azurepowershell-interactive
   $credential = New-Object pscredential -ArgumentList "temp", $sp.Secret
   $credential.GetNetworkCredential().password
   ```
3. Récupérez la valeur de l’ID du locataire du principal de service :
 
   ```azurepowershell-interactive
   $tenantId= (Get-AzContext).Tenant.Id
   ```
4. Copiez et enregistrez le mot de passe, l’ID de l’application et l’ID du locataire en respectant les mesures de sécurité appropriées. Si vous oubliez ou perdez le mot de passe de votre principal de service, vous pouvez le réinitialiser à l’aide de la cmdlet [`New-AzADSpCredential`](/powershell/module/azurerm.resources/new-azurermadspcredential).

   > [!NOTE]
   > Veuillez noter qu’Azure Arc pour serveurs ne prend actuellement pas en charge la connexion avec un certificat. Par conséquent, un secret est nécessaire pour l’authentification du principal de service.

5. Téléchargez le script PowerShell depuis le portail en suivant les instructions fournies dans [Connecter votre SQL Server à Azure Arc](connect.md).

6. Ouvrez le script dans une instance administrateur de PowerShell ISE et remplacez les variables d’environnement suivantes par les valeurs générées au cours de la configuration du principal de service décrite précédemment. Ces variables sont vides au départ.

   ```azurepowershell-interactive
   $servicePrincipalAppId="{serviceprincipalAppID}"
   $servicePrincipalSecret="{serviceprincipalPassword}"
   $servicePrincipalTenantId="{serviceprincipalTenantId}"
   ```

7. Exécuter le script sur chaque machine cible

## <a name="connecting-multiple-sql-server-instances-on-linux-using-azure-cli"></a>Connexion de plusieurs instances SQL Server sur Linux à l’aide d’Azure CLI

[Azure CLI](/cli/azure/install-azure-cli) doit être installé sur chaque machine cible. Le script d’inscription se connecte automatiquement à Azure avec les informations d’identification du principal de service si elles sont fournies et si aucun autre utilisateur n’est déjà connecté. Procédez comme suit pour connecter des instances SQL Server sur plusieurs machines Linux.

1. Créez un principal de service à l’aide de la commande ['az ad sp create-for-rbac'](/cli/azure/ad/sp#az_ad_sp_create_for_rbac).

   ```azurecli-interactive
   az ad sp create-for-rbac --name <your service principal name> --role <your custom role name>
   ```

   ```output
   { "appId": "d2ff754a-e10a-4eb6-9cdc-ce6e7a4687db",
     "displayName": "Arc-for-servers",
     "name": "http://Arc-for-servers",
     "password": {SomeRandomlyGeneratedPassword}",
     "tenant": "2530e75f-673b-4841-8270-47ca6a34ef4f"
   }
   ```

   > [!NOTE]
   > Quand vous créez un principal de service, votre compte doit être Propriétaire ou Administrateur de l’accès utilisateur sur l’abonnement à utiliser pour l’intégration. Si vous n’avez pas les autorisations appropriées pour créer les attributions de rôles, le principal de service sera créé, mais il ne pourra pas intégrer de machines. Les instructions relatives à la création d’un rôle personnalisé sont fournies dans [Autorisations requises](overview.md#required-permissions).

2. Téléchargez le script de l’interpréteur de commandes Linux depuis le portail en suivant les instructions fournies dans [Connecter votre SQL Server à Azure Arc](connect.md).

3. Remplacez les variables suivantes dans le script à l’aide des valeurs retournées par la commande 'az ad sp create-for-rbac'. Ces variables sont vides au départ.

   ```bash
   servicePrincipalAppId="{serviceprincipalAppID}"
   servicePrincipalSecret="{serviceprincipalPassword}"
   servicePrincipalTenant="{serviceprincipalTenant}"
   ```

3. Exécuter le script sur chaque machine cible
 
   ```bash
   sudo chmod +x ./RegisterSqlServerArc.sh
   ./RegisterSqlServerArc.sh
   ```

## <a name="validate-successful-onboarding"></a>Vérifier la réussite de l’intégration

Une fois les instances SQL Server inscrites avec un serveur SQL doté d’Azure Arc (préversion), accédez au [portail Azure](https://aka.ms/azureportal) et affichez les ressources Azure Arc nouvellement créées. Une nouvelle ressource __Machine - Azure Arc__ s’affichera pour chaque machine connectée et une nouvelle ressource __SQL Server - Azure Arc__ s’affichera pour chaque instance SQL Server inscrite. 

![Intégration réussie](./media/join-at-scale/successful-onboard.png)

## <a name="next-steps"></a>Étapes suivantes

- Apprenez à gérer votre machine à l’aide d’[Azure Policy](/azure/governance/policy/overview), par exemple pour la [configuration invité](/azure/governance/policy/concepts/guest-configuration) des machines virtuelles, pour vérifier que l’ordinateur crée des rapports sur l’espace de travail Log Analytics prévu, pour activer l’analyse d’[Azure Monitor sur des machines virtuelles](/azure/azure-monitor/insights/vminsights-enable-policy) et bien plus encore.

- En savoir plus sur [l’agent Log Analytics](/azure/azure-monitor/platform/log-analytics-agent). L’agent Log Analytics pour Windows et Linux est nécessaire quand vous souhaitez superviser de manière proactive le système d’exploitation et les charges de travail en cours d’exécution sur la machine, gérer le système d’exploitation à l’aide de runbooks Automation ou de solutions comme Update Management ou utiliser d’autres services Azure tels qu’[Azure Security Center](/azure/security-center/security-center-intro).

- Découvrez comment [Configurer votre instance SQL Server pour le contrôle d’intégrité de l’environnement périodique à l’aide de SQL Assessment à la demande](assess.md)

- Découvrez comment [Configurer Advanced Data Security pour votre instance SQL Server](configure-advanced-data-security.md)