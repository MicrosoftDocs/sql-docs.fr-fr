---
title: Gestion des certificats (Gestionnaire de configuration SQL Server)
description: Découvrez comment installer des certificats dans différentes configurations SQL Server. Les instances uniques, les clusters de basculement et les groupes de disponibilité Always On en sont des exemples.
ms.prod: sql
ms.prod_service: high-availability
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- connections [SQL Server], encrypted
- SSL [SQL Server]
- Secure Sockets Layer (SSL)
- encryption [SQL Server], connections
- cryptography [SQL Server], connections
- certificates [SQL Server], installing
- requesting encrypted connections
- installing certificates
- security [SQL Server], encryption
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: ''
ms.date: 01/12/2021
ms.openlocfilehash: 9d6eebebb411c4d3939de8dd09b094b245efc1d2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100355341"
---
# <a name="certificate-management-sql-server-configuration-manager"></a>Gestion des certificats (Gestionnaire de configuration SQL Server)

[!INCLUDE [sql-windows-only](../../includes/applies-to-version/sql-windows-only.md)]

Cette rubrique décrit comment déployer et gérer des certificats sur votre topologie de groupe de disponibilité ou cluster de basculement SQL Server Always On.

Les certificats SSL/TLS sont largement utilisés pour sécuriser l’accès à SQL Server. Avec les versions antérieures de SQL Server, les organisations avec de grandes infrastructures SQL Server devaient consacrer d’importants efforts à la gestion de leur infrastructure de certificats SQL Server, souvent en développant des scripts et en exécutant des commandes manuelles. Avec SQL Server 2019, la gestion des certificats est intégrée au Gestionnaire de configuration SQL Server, ce qui simplifie les tâches courantes telles que les suivantes : 

* Affichage et validation des certificats installés dans une instance SQL Server. 
* Identification des certificats arrivant à expiration. 
* Déploiement de certificats sur plusieurs machines d’un groupe de disponibilité Always On à partir du nœud contenant le réplica principal. 
* Déploiement de certificats sur plusieurs machines participant à une instance de cluster de basculement Always On à partir du nœud actif.

> [!NOTE]
> Vous pouvez utiliser la gestion des certificats dans le Gestionnaire de configuration SQL Server avec des versions inférieures de SQL Server, à partir de SQL Server 2008.

##  <a name="to-install-a-certificate-for-a-single-sql-server-instance"></a><a name="provision-single-server-cert"></a> Pour installer un certificat pour une seule instance SQL Server  

::: moniker range=">=sql-server-ver15"
1. Dans le Gestionnaire de configuration SQL Server, dans le volet de la console, développez **Configuration du réseau SQL Server**.  

2. Cliquez avec le bouton droit sur **Protocoles pour** *&lt;nom de l’instance&gt;* , puis sélectionnez **Propriétés**.  

3. Choisissez l’onglet **Certificat**, puis sélectionnez **Importer**.  

4. Sélectionnez **Parcourir**, puis le fichier de certificat.  

5. Sélectionnez **Suivant** pour valider le certificat. Si aucune erreur ne se produit, sélectionnez **Suivant** pour importer le certificat dans l’instance locale.  
::: moniker-end

::: moniker range="<= sql-server-2017"
1. Dans le Gestionnaire de configuration SQL Server, dans le volet de la console, développez **Configuration du réseau SQL Server**.  

2. Cliquez avec le bouton droit sur **Protocoles pour** *&lt;nom de l’instance&gt;* , puis sélectionnez **Propriétés**.  

3. Sélectionnez un certificat dans le menu déroulant **Certificat**, puis sélectionnez **Appliquer**.  

4. Sélectionnez **OK**. 
::: moniker-end

##  <a name="to-install-a-certificate-in-a-failover-cluster-instance-configuration"></a><a name="provision-failover-cluster-cert"></a> Pour installer un certificat dans une configuration d’instance de cluster de basculement  
  
1. Dans le Gestionnaire de configuration SQL Server, dans le volet de la console, développez **Configuration du réseau SQL Server**.
  
2. Cliquez avec le bouton droit sur **Protocoles pour** *&lt;nom de l’instance&gt;* , puis choisissez **Propriétés**. 

3. Choisissez l’onglet **Certificat**, puis sélectionnez **Importer**.

4. Sélectionnez le type de certificat et indiquez si vous voulez procéder à l’importation uniquement pour le nœud actuel ou pour chaque nœud de cluster.

5. Dans le cas d’une installation pour un nœud unique, choisissez **Parcourir**, puis le fichier de certificat. Passez ensuite à l’étape 8.

6. Dans le cas de l’installation d’un certificat pour chaque nœud, sélectionnez **Suivant** pour lister les nœuds propriétaires possibles. Les propriétaires possibles de l’instance de cluster de basculement actuelle sont présélectionnés.

7. Choisissez **Suivant** pour sélectionner le certificat à importer.

8. Entrez le mot de passe quand vous y êtes invité. Recherchez les éventuels avertissements ou erreurs après la validation.

9. Sélectionnez **Suivant** pour importer les certificats sélectionnés.

> [!NOTE]
> Effectuez ces étapes dans le nœud actif de l’instance de cluster de basculement Always On. L’utilisateur doit disposer des autorisations d’administrateur sur tous les nœuds de cluster.

##  <a name="to-install-a-certificate-in-an-always-on-availability-group-configuration"></a><a name="provision-availability-group-cert"></a>Pour installer un certificat dans une configuration de groupe de disponibilité Always On  
  
1. Dans le Gestionnaire de configuration SQL Server, dans le volet de la console, développez **Configuration du réseau SQL Server**.
  
2. Cliquez avec le bouton droit sur **Protocoles pour** *&lt;nom de l’instance&gt;* , puis sélectionnez **Propriétés**.  
  
3. Choisissez l’onglet **Certificat**, puis sélectionnez **Importer**.  
  
4. Choisissez le type de certificat et sélectionnez **Suivant** pour sélectionner dans la liste des groupes de disponibilité connus.  

5. Sélectionnez **Suivant** pour choisir des certificats pour chaque nœud de réplica. Les certificats doivent avoir un nom de fichier qui correspond au nom NetBIOS des nœuds.

6. Sélectionnez **Suivant** pour importer le certificat sur chaque nœud.


> [!NOTE]
> Effectuez ces étapes à partir du nœud contenant le réplica principal du groupe de disponibilité. L’utilisateur doit disposer des autorisations d’administrateur sur tous les nœuds de cluster.

