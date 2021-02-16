---
description: Gestionnaire de connexions du cache
title: Gestionnaire de connexions du cache | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.cacheconnection.f1
helpviewer_keywords:
- Cache connection manager
ms.assetid: bdc92038-3720-4795-8a5c-79b963f2c952
author: chugugrace
ms.author: chugu
ms.openlocfilehash: fba6ab0ff6c58228f702983b6338c2bd9965e5e7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100338443"
---
# <a name="cache-connection-manager"></a>Gestionnaire de connexions du cache

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Le gestionnaire de connexions du cache lit des données à partir de la transformation du cache ou d'un fichier cache (.caw) et peut enregistrer les données dans un fichier cache. Les données sont toujours stockées en mémoire que vous configuriez ou non le gestionnaire de connexions du cache pour utiliser un fichier cache.  
  
 La transformation du cache écrit des données provenant d'une source de données connectée dans le flux de données dans un gestionnaire de connexions du cache. La transformation de recherche dans un package effectue des recherches sur les données.  
  
> [!NOTE]  
>  Le gestionnaire de connexions du cache ne prend pas en charge les types de données de l'objet BLOB (Binary Large Object) DT_TEXT, DT_NTEXT et DT_IMAGE. Si le dataset de référence contient un type de données d'objet BLOB, le composant échoue lorsque vous exécutez le package. Vous pouvez utiliser l' **Éditeur du gestionnaire de connexions du cache** pour modifier des types de données de colonne. Pour plus d’informations, consultez [Éditeur du gestionnaire de connexions du cache]().  
  
> [!NOTE]  
>  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="configuration-of-the-cache-connection-manager"></a>Configuration du gestionnaire de connexions du cache  
 Vous pouvez configurer le gestionnaire de connexions du cache comme suit :  
  
-   Indiquez s'il faut utiliser ou non un fichier cache.  
  
     Si vous configurez le gestionnaire de connexions du cache pour utiliser un fichier cache, il effectuera l'une des actions suivantes :  
  
    -   Il enregistrera les données dans le fichier lorsqu'une transformation du cache est configurée pour écrire des données depuis une source de données dans le flux de données dans le gestionnaire de connexions du cache.  
  
    -   Il lira des données à partir du fichier cache.  
  
     Pour plus d'informations, consultez [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Modifiez les métadonnées des colonnes stockées dans le cache.  
  
-   Mettez à jour le nom du fichier cache au moment de l’exécution en utilisant une expression pour définir la propriété ConnectionString. Pour plus d’informations, consultez [Expressions de propriété dans des packages](../../integration-services/expressions/use-property-expressions-in-packages.md).  
  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] ou par programmation.  
  
 Pour plus d’informations sur la configuration d’un gestionnaire de connexions par programmation, consultez <xref:Microsoft.SqlServer.Dts.Runtime.ConnectionManager> et [Ajout de connexions par programmation](../../integration-services/building-packages-programmatically/adding-connections-programmatically.md).  
  
## <a name="cache-connection-manager-editor"></a>Éditeur du gestionnaire de connexions du cache
  Le gestionnaire de connexions du cache lit un dataset de référence à partir de la transformation de cache ou d'un fichier cache (.caw) et peut enregistrer les données dans un fichier cache. Les données sont toujours stockées en mémoire.  
  
> [!NOTE]  
>  Le gestionnaire de connexions du cache ne prend pas en charge les types de données de l'objet BLOB (Binary Large Object) DT_TEXT, DT_NTEXT et DT_IMAGE. Si le dataset de référence contient un type de données d'objet BLOB, le composant échoue lorsque vous exécutez le package. Vous pouvez utiliser l' **Éditeur du gestionnaire de connexions du cache** pour modifier des types de données de colonne.  
  
 La transformation de recherche effectue des recherches sur le dataset de référence.  
  
 La boîte de dialogue **Éditeur du gestionnaire de connexions du cache** inclut les onglets suivants :  
  
###  <a name="general-tab"></a><a name="generaltab"></a> Onglet Général  
 Utilisez l’onglet **Général** de la boîte de dialogue **Éditeur du gestionnaire de connexions du cache** pour indiquer s’il faut lire le cache depuis un fichier ou l’enregistrer dans un fichier.  
  
#### <a name="options"></a>Options  
 **Nom du gestionnaire de connexions**  
 Fournit un nom unique pour la connexion de fichiers cache dans le flux de travail. Le nom fourni sera affiché dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] .  
  
 **Description**  
 Décrit la connexion. Il est recommandé de décrire la connexion selon son objectif, de sorte que les packages soient correctement documentés et plus faciles à gérer.  
  
 **Utiliser un cache de fichier**  
 Indiquez s'il faut utiliser ou non un fichier cache.  
  
> [!NOTE]  
>  Le niveau de protection du package ne s'applique pas au fichier cache. Si le fichier cache contient des informations sensibles, utilisez une liste de contrôle d'accès (ACL) pour restreindre l'accès à l'emplacement ou au dossier dans lequel vous stockez le fichier. Vous devez autoriser l'accès à certains comptes uniquement. Pour plus d’informations, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
 Si vous configurez le gestionnaire de connexions du cache pour utiliser un fichier cache, il effectuera l'une des actions suivantes :  
  
-   Il enregistrera les données dans le fichier lorsqu'une transformation du cache est configurée pour écrire des données depuis une source de données dans le flux de données dans le gestionnaire de connexions du cache. Pour plus d'informations, consultez [Cache Transform](../../integration-services/data-flow/transformations/cache-transform.md).  
  
-   Il lira des données à partir du fichier cache.  
  
 **Nom de fichier**  
 Tapez le chemin d'accès et le nom du fichier cache.  
  
 **Parcourir**  
 Recherchez le fichier cache.  
  
 **Actualiser les métadonnées**  
 Supprimez les métadonnées de colonne dans le gestionnaire de connexions du cache et remplissez à nouveau celui-ci avec les métadonnées de colonne d'un fichier cache sélectionné.  
  
###  <a name="columns-tab"></a><a name="columnstab"></a> Onglet Colonnes  
 Utilisez l'onglet **Colonnes** de la boîte de dialogue **Éditeur du gestionnaire de connexions du cache** pour configurer les propriétés de chaque colonne dans le cache.  
  
#### <a name="options"></a>Options  
 **Colonne**  
 Spécifiez le nom de la colonne.  
  
 **Position d'index**  
 Spécifiez quelles colonnes sont des colonnes d'index en indiquant la position d'index de chaque colonne. L'index est une collection composée d'une ou de plusieurs colonnes.  
  
 Pour les colonnes qui ne sont pas des index, la position d'index est 0.  
  
 Pour les colonnes d'index, la position d'index est un nombre séquentiel, positif. Ce nombre indique l'ordre dans lequel la transformation de recherche compare des lignes dans le dataset de référence aux lignes de la source de données d'entrée. La colonne comportant la plupart des valeurs doit avoir la position d'index la plus faible.  
  
> [!NOTE]  
>  Lorsque la transformation de recherche est configurée pour utiliser un gestionnaire de connexions du cache, seules les colonnes d'index dans le dataset de référence peuvent être mappées avec des colonnes d'entrée. En outre, toutes les colonnes d'index doivent être mappées.  
  
 **Type**  
 Spécifiez le type de données de la colonne.  
  
 **Longueur**  
 Indique le type de données de la colonne. Si le type de données le permet, vous pouvez mettre à jour **Longueur**.  
  
 **Précision**  
 Spécifie la précision pour certains types de données de colonne. La précision est le nombre de chiffres qui composent un nombre. Si le type de données le permet, vous pouvez mettre à jour **Précision**.  
  
 **Mise à l’échelle**  
 Spécifie l'échelle pour certains types de données de colonne. L'échelle est le nombre de chiffres à droite du séparateur décimal dans un nombre. Si le type de données le permet, vous pouvez mettre à jour **Échelle**.  
  
 **Page de codes**  
 Spécifie la page de codes pour le type de colonne. Si le type de données le permet, vous pouvez mettre à jour **Page de codes**.  
  
## <a name="related-tasks"></a>Tâches associées  
 [Implémenter une transformation de recherche en mode Cache complet à l'aide du gestionnaire de connexions du cache](lookup-transformation-full-cache-mode-cache-connection-manager.md)  
  
