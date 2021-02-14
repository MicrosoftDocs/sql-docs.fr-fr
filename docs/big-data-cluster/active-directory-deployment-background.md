---
title: Déploiement de plusieurs clusters Big Data SQL Server dans un domaine Active Directory
titleSuffix: SQL Server Big Data Cluster
description: Découvrez comment déployer plusieurs clusters Big Data SQL Server dans un domaine Active Directory unique.
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a8b82b4a5802467841ca345ac24c83e1356a79f8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100040089"
---
# <a name="deploy-multiple-big-data-clusters-2019-in-the-same-active-directory-domain"></a>Déploiement de plusieurs [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans le même domaine Active Directory

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Cet article décrit les mises à jour apportées à SQL Server 2019 CU5 qui offrent la possibilité de déployer et d’intégrer plusieurs clusters Big Data SQL Server 2019 dans le même domaine Active Directory.

Avant SQL 2019 CU5, deux problèmes empêchaient le déploiement de plusieurs clusters Big Data dans un domaine AD.

- Conflit de noms entre les noms de principal du service et le domaine DNS
- Nom du principal du compte de domaine

## <a name="object-name-collisions"></a>Collisions de noms d’objets

### <a name="service-principal-names-spn-and-dns-domain-naming-conflict"></a>Conflit de noms entre les noms de principal du service (SPN) et le domaine DNS

Le nom de domaine fourni au moment du déploiement est utilisé comme domaine DNS AD. Cela signifie que les pods peuvent se connecter les uns aux autres dans le réseau interne à l’aide de ce domaine DNS. Les utilisateurs utilisent également ce domaine pour se connecter aux points de terminaison Clusters Big Data. Par conséquent, le nom du pod, du service ou du point de terminaison Kubernetes doit être qualifié avec ce domaine DNS AD pour tous les noms de principal du service (SPN) créés pour un service dans Clusters Big Data. Si un utilisateur déploie un deuxième cluster dans le domaine, les noms SPN générés possèderont le même nom de domaine complet (FQDN), car le nom des pods et du domaine DNS ne diffère pas entre les deux clusters. Prenons par exemple le cas d’un domaine DNS AD `contoso.local`. L’un des noms SPN générés pour le pool principal SQL Server dans le pod `master-0` serait `MSSQLSvc/master-0.contoso.local:1433`. Dans le deuxième cluster que l’utilisateur tente de déployer, le nom de pod pour `master-0` est le même et l’utilisateur fournit le même domaine DNS AD (``contoso.local``), ce qui aboutit à la même chaîne SPN. Active Directory interdit la création d’un nom SPN en conflit, entraînant ainsi un échec de déploiement pour le deuxième cluster.

### <a name="domain-account-principal-names"></a>Nom du principal du compte de domaine

Lors du déploiement d’un cluster Big Data avec un domaine Active Directory, plusieurs principaux de comptes sont générés pour les services qui s’exécutent dans le cluster. Il s’agit essentiellement de comptes d’utilisateur AD. Avant SQL 2019 CU5, le nom de ces comptes n’était pas unique d’un cluster à l’autre. Cet aspect se manifeste si l’on tente de créer le même nom de compte d’utilisateur pour un service donné de Clusters Big Data dans deux clusters différents. Le cluster déployé en deuxième entrera en conflit dans AD et ne pourra pas créer son compte.

## <a name="resolution-for-collisions"></a>Résolution des collisions

### <a name="solution-to-solve-the-problem-with-spns-and-dns-domain---sql-2019-cu5"></a>Solution pour résoudre le problème lié aux noms de principal de service et au domaine DNS – SQL 2019 CU5

Étant donné que les noms SPN doivent être différents entre deux clusters, le nom de domaine DNS transmis au moment du déploiement doit être différent. Vous pouvez spécifier différents noms DNS à l’aide du nouveau paramètre introduit dans le fichier de configuration de déploiement : `subdomain`. Si le sous-domaine diffère entre deux clusters et que la communication interne est possible sur ce sous-domaine, les noms SPN incluent le sous-domaine, atteignant ainsi l’unicité requise.

>[!NOTE]
>La valeur transmise par le biais du paramètre subdomain ne constitue pas un nouveau domaine AD, mais un domaine DNS utilisé en interne.

Reprenons par exemple le cas du nom SPN de pool principal SQL Server. Si le sous-domaine est `bdc`, le nom SPN précédent devient `MSSQLSvc/master-0.bdc.contoso.local:1433`.  

Il n’est pas obligatoire de personnaliser la valeur du nouveau paramètre subdomain dans la spécification de la configuration Active Directory. Par défaut, c’est le nom du cluster Big Data ou de l’espace de noms qui est utilisé pour calculer la valeur du paramètre subdomain. Lorsque les utilisateurs souhaitent remplacer le nom du sous-domaine, ils peuvent se servir du nouveau paramètre subdomain introduit dans la spécification de la configuration Active Directory.

### <a name="solution-to-solve-the-problem-regarding-account-names-uniqueness"></a>Solution pour résoudre le problème lié à l’unicité des noms de comptes

Pour mettre à jour les noms de compte selon un schéma qui garantit l’unicité, nous avons introduit le concept de préfixe de compte. Il s’agit d’une partie du nom de compte qui est unique d’un cluster à l’autre. La partie restante du nom de compte est constante pour un service donné. Le nouveau format du nom de compte est le suivant : `<prefix>-<name>-<podId>`. 

>[!NOTE]
>Active Directory impose que les noms de compte soient limités à 20 caractères. Le cluster Big Data doit en utiliser 8 pour distinguer les pods et les StatefulSet, ce qui laisse 12 caractères comme limite du préfixe de compte.

La personnalisation du nom de compte est facultative. Utilisez le paramètre `accountPrefix` dans la spécification de la configuration Active Directory. SQL Server 2019 CU5 introduit `accountPrefix` dans la spécification de la configuration. Par défaut, c’est le nom du sous-domaine qui est utilisé comme préfixe de compte. Si ce nom dépasse 12 caractères, le préfixe de compte est constitué de la sous-chaîne composée des 12 premiers caractères.

Le sous-domaine ne s’applique qu’au nom DNS. Le nouveau nom de compte d’utilisateur LDAP est donc `bdc-ldap@contoso.local`, et non `bdc-ldap@bdc.contoso.local`.

## <a name="semantics"></a>Sémantique

En résumé, voici la sémantique des paramètres ajoutés dans SQL 2019 CU5 pour plusieurs clusters dans un domaine :

### `subdomain`

- Champ facultatif
- Type de données : chaîne
- Définition : sous-domaine DNS unique à utiliser pour ce cluster Big Data. Cette valeur doit être différente pour chacun des clusters déployés dans le domaine Active Directory.  
- Valeur par défaut : lorsque aucune valeur n’est fournie, c’est le nom du cluster qui est utilisé comme valeur par défaut.
- Longueur maximale : 63 caractères par étiquette (à savoir chaque chaîne séparée par un point)
- Remarques : les noms DNS de point de terminaison doivent comporter le sous-domaine dans leur nom de domaine complet.

### `accountPrefix`

- Champ facultatif
- Type de données : chaîne
- Définition : préfixe unique pour les comptes AD que le cluster Big Data génère. Cette valeur doit être différente pour chacun des clusters déployés dans le domaine Active Directory.
- Valeur par défaut : lorsque aucune valeur n’est fournie, c’est le nom du sous-domaine qui est utilisé comme valeur par défaut. Lorsque le sous-domaine n’est pas indiqué, le nom du cluster sert de nom du sous-domaine, et donc est également hérité comme accountPrefix. Si le sous-domaine est précisé et qu’il s’agit d’un nom en plusieurs parties (avec un ou plusieurs points), l’utilisateur doit fournir un accountPrefix. 
- Longueur maximale : 12 caractères 

## <a name="impact-on-ad-domain-and-dns-server"></a>Impact sur le domaine AD et le serveur DNS 

Aucune modification n’est requise dans le domaine ou le contrôleur de domaine AD pour prendre en charge le déploiement de plusieurs clusters Big Data sur le même domaine Active Directory. Le sous-domaine DNS est automatiquement créé dans le serveur DNS lors de l’inscription du nom DNS des points de terminaison externes. 

## <a name="impact-on-setting-up-the-deployment-configuration-file-used-for-the-bdc-deployment"></a>Impact sur la configuration du fichier de configuration de déploiement utilisé pour le déploiement du cluster Big Data 

La section *activeDirectory* de la configuration du plan de contrôle *control.json* comporte deux nouveaux paramètres facultatifs : `subdomain` et `accountPrefix`. Donnez-leur des valeurs seulement si vous souhaitez remplacer le comportement par défaut, qui consiste à utiliser le nom de cluster pour chacun d’eux. Le nom du cluster correspond au nom de l’espace de noms.

Par ailleurs, vous pouvez utiliser les noms DNS de point de terminaison de votre choix, à condition qu’ils soient complets et n’entrent pas en conflit entre deux clusters Big Data déployés dans le même domaine. Il est possible d’opter pour la valeur du paramètre subdomain afin d’être sûr que les noms DNS soient différents entre les clusters.  Prenons par exemple le point de terminaison de passerelle. Si vous souhaitez utiliser le nom `gateway` pour le point de terminaison et l’inscrire automatiquement dans le serveur DNS dans le cadre du déploiement du cluster Big Data, choisissez `gateway.bdc1.contoso.local` comme nom DNS, `bdc1` correspondant au sous-domaine et `contoso.local` au nom de domaine DNS AD. `gateway-bdc1.contoso.local` ou encore simplement `gateway.contoso.local` sont d’autres valeurs acceptables.

## <a name="examples"></a>Exemples

Vous trouverez ci-dessous un exemple de configuration de la sécurité Active Directory, au cas où vous souhaiteriez remplacer subdomain et accountPrefix. 

```json
    "security": { 
        "activeDirectory": { 
            "ouDistinguishedName":"OU=contosoou,DC=contoso,DC=local", 
            "dnsIpAddresses": [ "10.10.10.10" ], 
            "domainControllerFullyQualifiedDns": [ "contoso-win2016-dc.contoso.local" ], 
            "domainDnsName":"contoso.local", 
            "subdomain": "bdc", 
            "accountPrefix": "myprefix", 
            "clusterAdmins": [ "contosoadmins" ], 
            "clusterUsers": [ "contosousers1", "contosousers2" ] 
        } 
    } 
  
```

Voici un exemple de spécification de point de terminaison pour les points de terminaison du plan de contrôle. Vous pouvez utiliser n’importe quelles valeurs comme noms DNS, à condition qu’ils soient uniques et complets :
  
```json
        "endpoints": [ 
            { 
                "serviceType": "NodePort", 
                "port": 30080, 
                "name": "Controller", 
                "dnsName": "control-bdc1.contoso.local" 
            }, 
            { 
                "serviceType": "NodePort", 
                "port": 30777, 
                "name": "ServiceProxy", 
                "dnsName": "monitor-bdc1.contoso.local" 
            } 
        ] 
  
```

## <a name="questions"></a>Questions

### <a name="do-you-need-to-create-separate-ous-for-different-clusters"></a>Est-il nécessaire de créer des UO distinctes pour les différents clusters ?

Ce n’est pas obligatoire, mais recommandé. Le fait de fournir des UO distinctes pour les différents clusters facilite la gestion des comptes d’utilisateurs générés.

### <a name="how-to-revert-back-to-the-pre-cu5-behavior-in-sql-2019"></a>Comment revenir au comportement d’avant CU5 dans SQL 2019 ?

Il peut exister des cas de figure dans lesquels la prise en compte du nouveau paramètre `subdomain` n’est pas possible, par exemple, si vous devez déployer une version antérieure à CU5 et que vous avez déjà mis à niveau [!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]. Même si cette situation est très improbable, vous pouvez définir le paramètre `useSubdomain` sur `false` dans la section Active Directory de `control.json` pour rétablir le comportement d’avant CU5.

Dans l’exemple suivant, `useSubdomain` est défini sur `false` pour ce cas de figure.

```console
azdata bdc config replace -c custom-prod-kubeadm/control.json -j "$.security.activeDirectory.useSubdomain=false" 
```

## <a name="next-steps"></a>Étapes suivantes

[Résolution des problèmes d’intégration Active Directory Clusters Big Data SQL Server](troubleshoot-active-directory.md)
