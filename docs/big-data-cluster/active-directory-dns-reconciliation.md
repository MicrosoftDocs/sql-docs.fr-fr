---
title: Rapprochement entre les noms DNS Active Directory et Kubernetes dans les déploiements Clusters Big Data
description: Configurer le rapprochement DNS pour Cluster Big Data SQL Server en mode Active Directory
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: a20afc81637eba30427b7b1062738b2d81ae67a0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100047879"
---
# <a name="active-directory-and-kubernetes-dns-reconciliation-in-big-data-clusters-deployments"></a>Rapprochement entre les noms DNS Active Directory et Kubernetes dans les déploiements Clusters Big Data

Cet article décrit quelques problématiques et solutions relatives à la gestion de l’intégration Active Directory lors du déploiement de clusters Big Data.

## <a name="overview"></a>Vue d’ensemble

Lorsque le cluster Big Data n’est pas déployé avec l’intégration Active Directory, les résolutions DNS internes s’appuient sur le service [Kubernetes CoreDNS](https://kubernetes.io/docs/tasks/administer-cluster/coredns/). Kubernetes utilise un domaine interne du type `<namespace>.svc.cluster.local`. Il crée des enregistrements A (recherche directe) et PTR (recherche inversée) dans le serveur DNS avec les noms de ce domaine.

Lorsque le mode de Active Directory est activé, en revanche, un nouveau domaine entre en jeu avec son propre ensemble de serveurs DNS. Lors de la résolution de noms interne, cela peut entraîner une confusion concernant l’ensemble de serveurs DNS à atteindre pour les recherches directes et inversées.

## <a name="challenges"></a>Défis

* Lors du déploiement de nouveaux pods Kubernetes, les entrées DNS doivent être ajoutées dans les deux ensembles de serveurs DNS. Kubernetes prend en charge l’enregistrement des entrées dans son service CoreDNS. Le flux de travail de déploiement de clusters Big Data, lui, est chargé d’ajouter les entrées requises dans les serveurs DNS du contrôleur de domaine Active Directory. De même, en cas de suppression d’un cluster Big Data, le flux de travail doit veiller à ce que ces entrées soient supprimées.
* Les serveurs DNS Active Directory sont extérieurs au cluster Kubernetes. Cependant, le cluster Big Data possède son propre espace IP dans Kubernetes et ne peut pas créer d’enregistrements pour cet espace IP sur un serveur DNS externe, car cet espace IP n’est pas visible en dehors des limites du cluster.
* Lorsque des événements de basculement se produisent au sein du cluster Kubernetes, les enregistrements des serveurs DNS AD doivent également être mis à jour.
* Les noms du service Kubernetes comme ceux des pods doivent être adressables au moyen de recherches de nom de domaine AD, ce qui crée une difficulté supplémentaire dans les noms DNS Active Directory. En effet, un nom de service peut correspondre à plusieurs adresses IP de pods.
* L’enregistrement des retards de propagation et de réplication des mises à jour dans les serveurs DNS Active Directory de l’organisation peut être volumineux et échapper au contrôle des flux de travail de gestion Clusters Big Data, ce qui risque d’avoir un impact sur la fonctionnalité Clusters Big Data juste après le déploiement et le basculement. Kubernetes CoreDNS est plus rapide et plus efficace en raison de sa localité.

## <a name="solution"></a>Solution

Pour contourner les problématiques mentionnées plus haut, la solution implémentée dans Clusters Big Data implique un nouveau service CoreDNS interne, géré dans l’espace de noms Clusters Big Data. Il s’agit du seul service DNS auquel s’adresseront les pods de l’espace de noms Clusters Big Data pour les résolutions de noms. La complexité liée aux domaines multiples est cachée derrière le nouveau service CoreDNS.

Par exemple, dans le diagramme suivant, les pods utilisent le serveur CoreDNS Clusters Big Data pour résoudre les noms. Ils n’interagissent directement ni avec le serveur Kubernetes CoreDNS, ni avec le serveur DNS AD. 

:::image type="content" source="media/active-directory-dns-reconciliation/bdc-ad-dns-reconciliation.png" alt-text="Les pods se connectent au serveur CoreDNS dans leur propre espace de noms":::

Voici quelques aspects de l’implémentation qui clarifient le fonctionnement de cette conception dans Clusters Big Data :

### <a name="centralized-management-of-multiple-domains"></a>Gestion centralisée de plusieurs domaines

La complexité des processus de recherches de noms reste cachée derrière le service DNS interne de manière centralisée, ce qui évite aux différents pods d’avoir à gérer plusieurs domaines et simplifie la conception.

### <a name="no-records-for-internal-pods-in-external-dns-servers"></a>Aucun enregistrement pour les pods internes des serveurs DNS externes

En conséquence de ce principe de conception, Clusters Big Data n’a pas à créer ni à gérer les enregistrements A et PTR des pods qui se trouvent dans l’espace IP Kubernetes des serveurs DNS externes.

### <a name="no-duplication-of-records"></a>Aucune duplication des enregistrements

Les enregistrements DNS internes ne sont stockés que dans Kubernetes CoreDNS. Le service CoreDNS interne Clusters Big Data effectue une réécriture et un transfert computationnels des requêtes DNS vers Kubernetes CoreDNS.

### <a name="computational-rewriting"></a>Réécriture computationnelle

Dans la mesure où il ne stocke pas d’enregistrements, Clusters Big Data est responsable de la traduction des requêtes de recherche directe entrantes, passant des noms du domaine AD à ceux du domaine Kubernetes. Ensuite, il transmet cette requête à Kubernetes CoreDNS.
Par exemple, une requête entrante pour `compute-0-0.contoso.local` est réécrite en `compute-0-0.compute-0-svc.contoso.svc.cluster.local`. Cette demande est transférée à Kubernetes CoreDNS.
Dans le cas de recherches inversées, la demande est transmise avec des adresses IP internes, car elles sont destinées à Kubernetes CoreDNS. Ensuite, elle réécrit la réponse avec le nom de domaine AD avant de répondre au client.

### <a name="simplicity-in-pod-configurations"></a>Simplicité dans les configurations de pods

Le fait que seul le nom CoreDNS Clusters Big Data interne soit référencé dans /etc/resolv.conf pour tous les pods simplifie la vue réseau à partir des pods. La complexité est cachée dans le service CoreDNS interne.

### <a name="static-and-reliable-ip-address-for-dns-service"></a>Adresse IP statique et fiable pour le service DNS

Le service CoreDNS déployé par Clusters Big Data comporte une adresse IP interne statique inscrite, accessible à partir de tous les pods, ce qui évite d’avoir à mettre à jour les valeurs de /etc/resolv.conf.

### <a name="service-load-balance-management-is-retained-by-kubernetes"></a>Gestion de l’équilibrage de charge de service conservée par Kubernetes

Les recherches effectuées pour des services plutôt que des pods individuels restent adressées à Kubernetes CoreDNS. Ainsi, Clusters Big Data n’est pas responsable de l’implémentation de l’équilibrage de charge spécialement pour le domaine AD.

Par exemple, si une demande de recherche directe est fournie pour `compute-0-svc.contoso.local`, elle est réécrite en ` compute-0-svc.contoso.svc.cluster`.local. Elle sera transférée à Kubernetes CoreDNS, où se produira l’équilibrage de charge. La réponse sera l’adresse IP de l’une des instances de pools de calcul multiples (réplicas de pods).

### <a name="scalability"></a>Extensibilité

Dans la mesure où Clusters Big Data ne stocke pas d’enregistrements, le service CoreDNS Clusters Big Data interne peut être mis à l’échelle sans rétention d’état ni réplication d’enregistrements sur plusieurs réplicas. Si les enregistrements DNS sont stockés dans Clusters Big Data, la réplication de l’état sur tous les pods doit également être prise en charge par Clusters Big Data.

### <a name="externally-visible-service-entries-stay-in-ad-dns"></a>Maintien des entrées de service visibles de l’extérieur dans le nom DNS AD

En ce qui concerne les points de terminaison de service qui doivent être accessibles aux clients en dehors du cluster Kubernetes, les entrées DNS sont créées dans le serveur DNS AD lors du déploiement Clusters Big Data. L’utilisateur entre les noms DNS servant de base à l’inscription à travers les profils de configuration du déploiement.

### <a name="self-deprovisioning"></a>Déprovisionnement automatique

Une fois le cluster Big Data supprimé et lors de son déprovisionnement, il n’existe aucun travail dynamique supplémentaire à effectuer pour supprimer les entrées DNS. Les seules entrées des noms DNS Active Directory distants qui doivent être nettoyées concernent les services externes ; leur nombre est statique. Les entrées DNS internes seront automatiquement supprimées avec le cluster.

## <a name="next-steps"></a>Étapes suivantes

- [Déploiement de clusters Big Data SQL Server en mode Active Directory](active-directory-deploy.md)
- [Gestion de l’accès à un cluster Big Data en mode Active Directory](active-directory-objects.md)
- [Déploiement de plusieurs clusters Big Data SQL Server Clusters dans le même domaine Active Directory](active-directory-deployment-background.md)
