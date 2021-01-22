---
title: Planifier l’attestation du Service Guardian hôte
description: Planifiez l’attestation à l’aide du Service Guardian hôte pour Always Encrypted avec enclaves sécurisées dans SQL Server.
ms.custom: ''
ms.date: 01/15/2021
ms.prod: sql
ms.reviewer: vanto
ms.technology: security
ms.topic: conceptual
author: rpsqrd
ms.author: ryanpu
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c4c80a51370de62410367b1225fd85e3ffe7f261
ms.sourcegitcommit: 8ca4b1398e090337ded64840bcb8d6c92d65c29e
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98534798"
---
# <a name="plan-for-host-guardian-service-attestation"></a>Planifier l’attestation du Service Guardian hôte

[!INCLUDE [sqlserver2019-windows-only](../../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Quand vous utilisez [Always Encrypted avec enclaves sécurisées](always-encrypted-enclaves.md), vous devez vérifier que l’application cliente communique avec une enclave digne de confiance au sein du processus [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Pour une enclave de sécurité basée sur la virtualisation (VBS), cela implique de vérifier que le code à l’intérieur de l’enclave est valide et que l’ordinateur hébergeant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est digne de confiance. L’attestation à distance réalise cet objectif en introduisant un tiers qui peut valider l’identité (et éventuellement la configuration) de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Avant que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] puisse utiliser une enclave pour exécuter une requête, il doit fournir des informations au service d’attestation concernant son environnement d’exploitation pour obtenir un certificat d’intégrité. Ce certificat d’intégrité est ensuite envoyé au client, qui peut vérifier son authenticité de façon indépendante auprès du service d’attestation. Une fois que le client approuve le certificat d’intégrité, il sait qu’il communique avec une enclave VBS digne de confiance et émet la requête qui va utiliser cette enclave.

Le rôle Service Guardian hôte dans Windows Server 2019 fournit des fonctionnalités d’attestation à distance pour Always Encrypted avec des enclaves VBS.
Cet article vous guide tout au long des décisions de prédéploiement et des exigences pour utiliser Always Encrypted avec des enclaves VBS et l’attestation du Service Guardian hôte.

## <a name="architecture-overview"></a>Vue d’ensemble de l’architecture

Le Service Guardian hôte est un service web en cluster qui s’exécute sur Windows Server 2019.
Dans un déploiement classique, il y aura 1 à 3 serveurs Service Guardian hôte, au moins un ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et un ordinateur exécutant une application cliente ou des outils, comme SQL Server Management Studio.
Comme le Service Guardian hôte est chargé de déterminer quels ordinateurs exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sont dignes de confiance, il nécessite une isolation à la fois physique et logique de l’instance [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] qu’il protège.
Si les mêmes administrateurs ont accès au Service Guardian hôte et à un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ils pourraient configurer le service d’attestation pour permettre à un ordinateur malveillant d’exécuter [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], ce qui leur permettrait de compromettre l’enclave VBS.

### <a name="hgs-domain"></a>Domaine SGH

Le programme d’installation du Service Guardian hôte va créer automatiquement un nouveau domaine Active Directory pour les serveurs Service Guardian hôte, les ressources du cluster de basculement et les comptes d’administrateur.

L’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] n’a pas besoin d’être dans à un domaine, mais si c’est le cas, il doit s’agir d’un domaine différent de celui utilisé par le serveur SGH.

### <a name="high-availability"></a>Haute disponibilité

La fonctionnalité Service Guardian hôte installe et configure automatiquement un cluster de basculement.
Dans un environnement de production, il est recommandé d’utiliser trois serveurs Service Guardian hôte pour la haute disponibilité. Pour plus d’informations sur la façon dont le quorum du cluster est déterminé et sur les configurations alternatives, notamment des clusters à deux nœuds avec un témoin externe, reportez-vous à la [documentation du cluster de basculement](/windows-server/failover-clustering/manage-cluster-quorum).

Un stockage partagé n’est pas nécessaire entre les nœuds du Service Guardian hôte. Une copie de la base de données d’attestation est stockée sur chaque serveur Service Guardian hôte et est automatiquement répliquée sur le réseau par le service de cluster.

### <a name="network-connectivity"></a>Connectivité réseau

Le client SQL et l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] doivent pouvoir communiquer avec SGH sur HTTP.
Configurez SGH avec un certificat TLS pour chiffrer toutes les communications entre le client SQL et SGH, ainsi qu’entre l’ordinateur SQL Server et SGH.
Cette configuration vous protège contre les attaques de l’intercepteur et garantit que vous communiquez avec le bon serveur SGH.

Les serveurs Service Guardian hôte nécessitent une connectivité entre chaque nœud du cluster pour garantir la synchronisation de la base de données du service d’attestation. C’est une bonne pratique pour le cluster de basculement que de connecter les nœuds du Service Guardian hôte sur un réseau donné pour la communication du cluster et d’utiliser un réseau distinct pour la communication des autres clients avec le Service Guardian hôte.

### <a name="attestation-modes"></a>Modes d’attestation

Quand un ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] tente d’attester avec le Service Guardian hôte, il commence par demander au Service Guardian hôte comment il doit attester.
Le Service Guardian hôte prend en charge deux modes d’attestation avec [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] :

| Mode d’attestation | Explication |
| ---------------- | ------- |
| Module de plateforme sécurisée (TPM) | L’attestation de module TPM fournit l’assurance la plus forte quant à l’identité et à l’intégrité de l’ordinateur qui atteste avec le Service Guardian hôte. Elle nécessite que TPM version 2.0 soit installé sur les ordinateurs exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Chaque puce TPM contient une identité unique et immuable (une paire de clés de type EK - Endorsement Key) qui peut être utilisée pour identifier un ordinateur particulier. Les modules TPM mesurent également le processus de démarrage de l’ordinateur, en stockant les hachages des mesures sensibles pour la sécurité dans les registres de contrôle de plateforme qui peuvent être lus mais pas modifiés par le système d’exploitation. Ces mesures sont utilisées lors de l’attestation pour fournir une preuve de chiffrement qu’un ordinateur est bien dans la configuration de sécurité où il prétend être. |
| Clé hôte | L’attestation de clé hôte est une forme d’attestation plus simple qui vérifie seulement l’identité d’un ordinateur en utilisant une paire de clés asymétriques. La clé privée est stockée sur l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et la clé publique est fournie au Service Guardian hôte. La configuration de la sécurité de l’ordinateur n’est pas mesurée et une puce TPM 2.0 n’est pas nécessaire sur l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Il est important de protéger la clé privée installée sur l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], car toute personne qui obtiendrait cette clé peut emprunter l’identité d’un ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] légitime et l’enclave VBS s’exécutant dans [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. |

En général, nous faisons les recommandations suivantes :

- Pour les **serveurs de production physiques**, nous recommandons d’utiliser l’attestation TPM pour les garanties supplémentaires qu’elle fournit.
- Pour les **serveurs de production virtuels**, nous recommandons d’utiliser l’attestation de clé d’hôte dans la mesure où la plupart des machines virtuelles ne disposent ni de modules TPM virtuels ni d’un démarrage sécurisé. Si vous utilisez une machine virtuelle bénéficiant d’une sécurité renforcée comme une [machine virtuelle locale dotée d’une protection maximale](/windows-server/security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms-top-node), vous pouvez opter pour le mode TPM. Dans tous les déploiements virtualisés, le processus d’attestation analyse uniquement votre environnement de machine virtuelle (pas la plateforme de virtualisation sous la machine virtuelle).
- Dans les **scénarios de développement/test**, nous recommandons d’utiliser l’attestation de clé d’hôte car elle est plus facile à configurer.

### <a name="trust-model"></a>Modèle d’approbation

Dans le modèle d’approbation d’enclave VBS, les requêtes et les données chiffrées sont évaluées dans une enclave logicielle pour la protéger du système d’exploitation hôte.
L’accès à cette enclave est protégé par l’hyperviseur, de la même façon que deux machines virtuelles exécutées sur le même ordinateur ne peuvent pas accéder à la mémoire l’une de l’autre.
Pour qu’un client puisse faire confiance à une instance légitime de VBS, vous devez utiliser l’attestation de module TPM qui établit une racine matérielle de l’approbation sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
Les mesures de module TPM capturées lors du processus de démarrage incluent la clé d’identité unique de l’instance de VBS, ce qui garantit que le certificat d’intégrité est valide seulement sur cet ordinateur.
En outre, quand un module TPM est disponible sur un ordinateur exécutant VBS, la partie privée de la clé d’identité VBS est protégée par le module TPM, empêchant toute tentative d’emprunt d’identité de cette instance de VBS.

Le démarrage sécurisé est obligatoire avec l’attestation TPM pour vérifier que l’interface UEFI a chargé un chargeur de démarrage légitime signé par Microsoft et qu’aucun rootkit n’a intercepté le processus de démarrage de l’hyperviseur.
De plus, un appareil IOMMU est nécessaire par défaut pour garantir qu’aucun appareil matériel disposant d’un accès direct à la mémoire ne peut inspecter ni modifier la mémoire de l’enclave.

Ces protections supposent que l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est une machine physique.
Si vous virtualisez l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], vous ne pouvez plus garantir que la mémoire de la machine virtuelle est protégée d’une inspection effectuée par l’hyperviseur ou par l’administrateur de l’hyperviseur. Par exemple, un administrateur de l’hyperviseur pourrait vider la mémoire de la machine virtuelle et accéder à la version en texte clair de la requête et des données de l’enclave.
De façon similaire, même si la machine virtuelle dispose d’un module TPM virtuel, elle peut mesurer seulement l’état et l’intégrité du système d’exploitation et de l’environnement de démarrage de la machine virtuelle.
Elle ne peut pas mesurer l’état de l’hyperviseur contrôlant la machine virtuelle.

Cependant, même quand [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] est virtualisé, l’enclave est néanmoins toujours protégée contre les attaques provenant du système d’exploitation de la machine virtuelle.
Si vous faites confiance à votre hyperviseur ou à votre fournisseur cloud, et que vous vous inquiétez principalement des attaques d’un administrateur de base de données et d’un administrateur du système d’exploitation sur des données sensibles, un [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] virtualisé peut répondre à vos besoins.

De même, l’attestation de clé d’hôte reste valable dans les situations où un module TPM 2.0 n’est pas installé sur l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] ou dans des scénarios de développement/test où la sécurité n’est pas primordiale.
Vous pouvez néanmoins toujours utiliser la plupart des fonctionnalités de sécurité mentionnées ci-dessus, notamment le démarrage sécurisé et un module TPM 1.2, pour mieux protéger VBS et le système d’exploitation comme un tout.
Cependant, étant donné qu’il n’existe aucun moyen pour le Service Guardian hôte de vérifier que ces paramètres sont activés sur l’ordinateur avec l’attestation de clé hôte, le client n’est pas sûr que l’hôte utilise toutes les protections disponibles.

## <a name="prerequisites"></a>Prérequis

### <a name="hgs-server-prerequisites"></a>Prérequis pour le serveur SGH

Le ou les ordinateurs qui exécutent le rôle Service Guardian hôte doivent remplir les conditions suivantes :

| Composant | Condition requise |
| --------- | ----------- |
| Système d’exploitation | Windows Server 2019 Édition Standard ou Datacenter |
| UC | 2 cœurs (minimum), 4 cœurs (recommandé) |
| RAM | 8 Go (minimum) |
| Cartes réseau | 2 cartes réseau avec adresses IP statiques recommandées (1 pour le trafic du cluster, 1 pour SGH) |

Le Service Guardian hôte est un rôle très lié au processeur, en raison du nombre d’actions qui nécessitent un chiffrement et un déchiffrement.
L’utilisation de processeurs modernes avec des fonctionnalités d’accélération du chiffrement améliore les performances du Service Guardian hôte.
Les exigences de stockage pour les données d’attestation sont peu importantes : elles sont comprises dans une plage de 10 Ko à 1 Mo pour chaque ordinateur effectuant les attestations.

Ne joignez pas le ou les ordinateurs du Service Guardian hôte à un domaine avant de commencer.

### <a name="ssnoversion-md-computer-prerequisites"></a>Prérequis pour l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]

Le ou les ordinateurs exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] doivent répondre à la [Configuration requise pour l’installation de SQL Server](../../../sql-server/install/hardware-and-software-requirements-for-installing-sql-server.md) et à la [Configuration matérielle requise pour Hyper-V](/virtualization/hyper-v-on-windows/reference/hyper-v-requirements#hardware-requirements).

Ces conditions requises incluent :

- [!INCLUDE [sssqlv15-md](../../../includes/sssqlv15-md.md)] ou ultérieur
- Windows 10 Entreprise version 1809 ou ultérieure , ou Windows Server 2019 Édition Datacenter. Les autres éditions de Windows 10 et de Windows Server ne prennent pas en charge l’attestation avec le Service Guardian hôte.
- Prise en charge du processeur pour les technologies de virtualisation :
  - Intel VT-x avec Extended Page Tables.
  - AMD-V avec Rapid Virtualization Indexing.
  - Si vous exécutez [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] sur une machine virtuelle, l’hyperviseur et le processeur physique doivent offrir des fonctionnalités de virtualisation imbriquées. Pour plus d’informations sur les garanties lors de l’exécution d’enclaves VBS dans une machine virtuelle, consultez la section [Modèle d’approbation](#trust-model).
    - Sur Hyper-V 2016 ou ultérieur, [activez les extensions de virtualisation imbriquée sur le processeur de la machine virtuelle](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization#configure-nested-virtualization).
    - Dans Azure, sélectionnez une taille de machine virtuelle qui prend en charge la virtualisation imbriquée. Toutes les machines virtuelles de la série v3 prennent en charge la virtualisation imbriquée, notamment Dv3 et Ev3. Voir [Créer une machine virtuelle Azure compatible avec l’imbrication](/azure/virtual-machines/windows/nested-virtualization#create-a-nesting-capable-azure-vm).
    - Sur VMware vSphere 6.7 ou ultérieur, activez la prise en charge de la sécurité basée sur la virtualisation pour la machine virtuelle, comme le décrit la [documentation VMware](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.vm_admin.doc/GUID-C2E78F3E-9DE2-44DB-9B0A-11440800AADD.html).
    - D’autres hyperviseurs et clouds publics peuvent prendre en charge les fonctionnalités de virtualisation imbriquées qui permettent aussi l’utilisation d’Always Encrypted avec enclaves VBS. Consultez les instructions relatives à la compatibilité et à la configuration dans la documentation de votre solution de virtualisation.
- Si vous envisagez d’utiliser l’attestation de module TPM, vous aurez besoin d’une puce TPM 2.0 révision 1.16 prêt à être utilisée sur le serveur. À ce stade, l’attestation de Service Guardian hôte ne fonctionne pas avec les puces TPM 2.0 révision 1.38. De plus, le module TPM doit avoir un certificat de paire de clés de type EK (Endorsement Key) valide.

## <a name="roles-and-responsibilities-when-configuring-attestation-with-hgs"></a>Rôles et responsabilités lors de la configuration de l’attestation avec SGH

La configuration de l’attestation avec SGH implique la configuration de composants de différents types : SGH, des ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], des instances [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et des applications qui déclenchent l’attestation des enclaves. La configuration des composants de chacun de ces types est effectuée par les utilisateurs qui disposent de l’un des rôles suivants :

- Administrateur SGH : déploie SGH, inscrit les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auprès de SGH et partage l’URL d’attestation SGH avec les administrateurs de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et les administrateurs de l’application cliente.
- Administrateur de l’ordinateur [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] : installe les composants du client d’attestation, active VBS sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], fournit à l’administrateur SGH les informations nécessaires à l’inscription des ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] auprès de SGH, configure l’URL d’attestation sur les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et vérifie que les ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] peuvent attester correctement avec SGH.
- Administrateur de la base de données : configure les enclaves sécurisées dans les instances [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)].
- Administrateur d’application : configure l’application avec l’URL d’attestation obtenue auprès de l’administrateur SGH.

Dans les environnements de production (qui gèrent des données sensibles réelles), il est important que votre organisation adhère à la séparation des rôles lors de la configuration de l’attestation, selon laquelle chaque rôle est assumé par une personne différente. En effet, si l’objectif de déploiement d’Always Encrypted au sein de votre organisation est de réduire la surface d’attaque en veillant à ce que les administrateurs d’ordinateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et les administrateurs de bases de données ne puissent pas accéder à des données sensibles, les administrateurs [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] et les administrateurs de bases de données ne doivent pas contrôler les serveurs SGH.

## <a name="devtest-environment-considerations"></a>Considérations relatives aux environnements de développement/test

Si vous utilisez Always Encrypted avec enclaves VBS dans un environnement de développement ou de test, et que vous n’avez pas besoin d’une haute disponibilité ou d’une protection renforcée de l’ordinateur exécutant [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)], vous pouvez faire une ou plusieurs des concessions suivantes pour un déploiement simplifié :

- Déployez un seul nœud de Service Guardian hôte. Même si le Service Guardian hôte installe un cluster de basculement, vous n’avez pas besoin d’ajouter des nœuds supplémentaires si la haute disponibilité n’est pas un problème.
- Utilisez le mode de clé hôte au lieu du mode TPM pour une expérience d’installation plus simple.
- Virtualisez le Service Guardian hôte et/ou [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] pour économiser les ressources physiques.
- Exécutez SSMS ou d’autres outils pour configurer Always Encrypted avec enclaves sécurisées sur le même ordinateur que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]. Cela permet de conserver les clés principales de colonne sur le même ordinateur que [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)] : ne le faites donc pas dans un environnement de production.

## <a name="next-steps"></a>Étapes suivantes

- [Déployer le Service Guardian hôte pour [!INCLUDE [ssnoversion-md](../../../includes/ssnoversion-md.md)]](./always-encrypted-enclaves-host-guardian-service-deploy.md)