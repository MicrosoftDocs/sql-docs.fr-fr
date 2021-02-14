---
description: Système de gestion de versions dans la documentation SQL
title: Système de gestion de versions dans la documentation SQL
ms.date: 08/12/2020
ms.prod: sql
ms.technology: release-landing
ms.topic: conceptual
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||=azuresqldb-mi-current||=azure-sqldw-latest||>=aps-pdw-2016||>=sql-server-linux-2017||>=sql-server-2016
ms.openlocfilehash: 56ef939790de90d02add427f14f8817df885383f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100079750"
---
# <a name="versioning-system-for-sql-documentation"></a>Système de gestion de versions dans la documentation SQL

[!INCLUDE[includes_appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Cet article explique notre _système de gestion de versions_ dans la documentation SQL. Le système de gestion de versions connaît les produits et leurs versions. Le système vous permet de choisir le produit et la version qui vous intéressent. Le système affiche ensuite la documentation appropriée.

## <a name="applies-to-products"></a>Produits concernés (S’APPLIQUE À)

Le titre de la plupart des articles SQL Server est suivi de la mention **S’APPLIQUE À**. Vient ensuite sur la même ligne une liste pratique de _produits_ SQL avec des indicateurs montrant si l’article est pertinent pour les produits. Par exemple, le produit SQL Server peut être indiqué comme pertinent, tandis qu’Azure SQL Database peut être indiqué comme non pertinent pour l’article.

La ligne **S’APPLIQUE À** ignore les _versions_ des produits. Nous nous efforçons d’éviter les incohérences entre la ligne **S’APPLIQUE À** et l’aspect « produits » des configurations de notre système de gestion de versions.

## <a name="history-of-separate-file-sets"></a>Historique des jeux de fichiers distincts

Pour SQL Server 2014 et versions antérieures, chaque version a sa propre copie distincte complète des fichiers de documentation. Par exemple, la documentation relative à SQL Server 2014 a commencé sous la forme d’une copie de la documentation de SQL Server 2012. La copie 2014 a ensuite été modifiée pendant le cycle de développement du produit.

Cette ancienne approche signifiait que si un défaut était découvert dans la documentation 2014, il pouvait également être présent dans les documentations 2012 et 2008. Cela compliquait la résolution des défauts et la maintenance générale.

## <a name="multiple-versions-in-the-same-files"></a>Plusieurs versions dans les mêmes fichiers

Pour cette raison et d’autres, les fichiers de documentation pour SQL Server 2016 concernent également les versions 2017, 2019 et probablement \<vNext\>. Cette consolidation se veut pratique, car nous attribuons maintenant des _monikers de gestion de versions_ à nos fichiers de documentation SQL Server. Les monikers de gestion de versions sont assignés, ou explicitement incorporés, dès lors que le degré de granularité le justifie pour chaque fichier de documentation donné.

## <a name="versioning-control-in-the-ui"></a>Contrôle de gestion de versions dans l’interface utilisateur

Quand vous consultez un article de documentation SQL à l’aide de notre site web :::no-loc text="Docs":::, le moniker de gestion de versions actuellement choisi est visible au-dessus de la table des matières. Le contrôle est une liste déroulante.

![media_versioning-control-10-sql-server-2017.png](media/versioning-control-10-sql-server-2017.png)

Si vous souhaitez consulter la documentation relative à une autre version de SQL Server, cliquez sur la flèche de développement située à la fin du moniker de la version actuelle. Cliquez ensuite pour choisir la combinaison de produit et de version qui vous intéresse. Quand vous cliquez sur une autre version, la documentation affichée change immédiatement pour montrer les différences relatives à la nouvelle version choisie. La documentation alors affichée peut contenir, ou non, des changements ; les deux cas sont courants.

![media_versioning-control-20-expanded.png](media/versioning-control-20-expanded.png)

### <a name="https-parameter-no-loc-textview"></a>Paramètre HTTPs :::no-loc text="view=":::

Chaque article dont l’adresse web commence par `https://docs.microsoft.com/sql/` a un paramètre nommé `?view=` ajouté à son adresse. La valeur de ce paramètre est le code de moniker de gestion de versions.

Le _code_ de moniker dans l’adresse `https` correspond toujours au _nom_ de moniker affiché dans le contrôle de gestion de versions.

## <a name="products-not-editions"></a>Prise en compte des produits, pas des éditions

### <a name="editions"></a>Éditions

Dans les années 1990 jusqu’aux années 2000, Microsoft SQL Server n’avait qu’un seul produit. Il existait plusieurs _éditions_ de chaque version de SQL Server, telles que les éditions _Developer_ et _Enterprise_ de SQL Server 2008. Les éditions représentaient des ensembles de fonctionnalités légèrement différents, mais le produit principal était le même. Les nouvelles versions de SQL Server peuvent toujours avoir plusieurs éditions.

### <a name="products"></a>Products

Avec l’essor plus récent du cloud computing et de Microsoft Azure, Microsoft a publié son produit Azure SQL Database. Bien que le produit local SQL Server traditionnel et le produit Azure SQL Database partagent une grande quantité de code, ce sont deux produits véritablement distincts.

Pour SQL, les monikers de gestion de versions font la distinction entre les produits, mais pas entre les éditions.

#### <a name="azure-cloud-sql-products"></a>Produits SQL cloud Azure

Presque tous les articles dont l’adresse web commence par `https://docs.microsoft.com/sql/` s’appliquent à au moins une version du produit nommé SQL Server. Un grand sous-ensemble de ces articles s’applique également à un ou plusieurs de nos produits de service SQL hébergés dans notre cloud Azure. Parmi ces produits cloud SQL, citons Azure SQL Database.

Naturellement, le produit Azure SQL Database n’a qu’une seule version. Presque tous les articles qui s’appliquent à Azure SQL Database, mais pas à SQL Server, ont une adresse web commençant par `https://docs.microsoft.com/azure/sql-database/`.

## <a name="scenarios-of-version-filtering"></a>Scénarios de filtrage de version

Le système de gestion de versions repose sur le filtrage de tout le contenu de documentation qui ne s’applique pas au moniker actuellement actif. Chaque fois que vous choisissez un moniker de gestion de versions différent, le jeu de contenu qui est masqué change. Le filtrage masque le contenu aux niveaux suivants :

- Sections ou phrases dans un article.
- Entrées d’articles dans la table des matières.

Les scénarios suivants décrivent les effets du choix d’un autre moniker.

### <a name="scenario-1-within-the-current-article"></a>Scénario 1 : Dans l’article actuel

Le scénario suivant porte sur les sections de votre article actuel :

1. Le moniker de gestion de versions actuel est **SQL Server 2017**.
2. Vous lisez une section qui décrit une fonctionnalité qui a été ajoutée pour la première fois à la version 2017 de SQL Server.
3. Vous définissez le moniker sur **SQL Server 2016**.
4. Vous remarquez que la section que vous étiez en train de lire a disparu.
5. Vous définissez le moniker sur **SQL Server 2019**.
6. Vous remarquez que la section 2017 que vous étiez en train de lire réapparaît.

Dans le scénario précédent, la section relative à la nouvelle fonctionnalité 2017 est probablement marquée avec une _plage de monikers_ qui comprend le code de moniker suivant :

- `>=sql-server-2017`

Quand le moniker **SQL Server 2019** a été choisi, le système de gestion de versions a réalisé que 2019 est supérieur ou égal à 2017 et a donc affiché la section.

### <a name="scenario-2-click-a-link-to-a-hidden-article"></a>Scénario 2 : Cliquer sur un lien vers un article masqué

Le scénario rare suivant explique ce qui se passe si vous cliquez sur un lien vers un article qui est masqué dans la table des matières. En bref, le lien fonctionne :

1. Le moniker de gestion de versions actuel est **SQL Server 2017**.
2. Dans l’article actuel :::no-loc text="A":::, vous cliquez sur un lien vers un article :::no-loc text="B"::: qui s’applique uniquement à SQL Server 2016.
    - Avant le clic, l’entrée de l’article :::no-loc text="B"::: est masqué dans la table des matières.
3. Après le clic, l’article :::no-loc text="B"::: s’affiche.
    - L’affichage de l’article :::no-loc text="B"::: force le contrôle de gestion de versions à basculer vers le moniker **SQL Server 2016**.
    - Le moniker d’origine **SQL Server 2017** a dû être abandonné. Cet abandon provoque l’affichage d’un message d’information en haut de la page web. Le [message](#anchor-message-unavailable-for-moniker) explique que le moniker actuel a dû être basculé pour s’adapter au nouvel article :::no-loc text="B":::.

### <a name="scenario-3-navigate-to-an-https-address"></a>Scénario 3 : Accéder à une adresse https

L’article suivant a été ajouté pour SQL Server 2017. Il décrit les fonctionnalités qui ont été ajoutées à SQL Server dans la version 2017. La plupart ou la totalité de ces nouvelles fonctionnalités font également partie de la version 2019. Voici les attributs de l’article.

| Attribut | Valeur |
| :-------- | :---- |
| Intitulé | Nouveautés de SQL Server 2017 |
| Plage de monikers | `=">= sql-server-2017"` |
| Adresse `https` | `https://docs.microsoft.com/sql/sql-server/what-s-new-in-sql-server-2017` |
| &nbsp; | &nbsp; |

À partir de l’adresse `https` de base, le tableau suivant explique ce qui se produit quand le paramètre `?view=` est ajouté par l’utilisateur, en fonction de différentes valeurs.

| Valeur de `?view=` | Comportement de la navigation depuis l’adresse `https` |
| :---------------- | :------------------------------ |
| _(Aucun paramètre.)_ | Le système de gestion de versions essaie sa valeur de moniker par défaut. En règle générale, nous définissons cette dernière sur la version non-préversion la plus récente de SQL Server.<br/><br/>La valeur par défaut SQL Server 2017 ou 2019 satisfait à l’attribut `>= sql-server-2017`.<br/><br/>Le système ajoute le paramètre à l’adresse `https`, peut-être sous la forme `?view=sql-server-2017`.<br/>Le contrôle de liste déroulante de gestion de versions est ensuite défini sur le nom du moniker correspondant. |
| `sql-server-2016` | Le système de gestion de versions se rend compte que la plage de monikers de l’article n’inclut pas la version 2016.<br/><br/>Le système choisit alors l’un des monikers qui satisfont à la plage.<br/><br/>Ensuite, comme dans le cas de la version 2016 , le paramètre `?view=` est ajouté et le nom du contrôle correspond à la valeur du paramètre. |
| `sql-server-2017` | Le système de gestion de versions comprend que la valeur du paramètre est incluse dans la plage de monikers de l’article.<br/><br/>Le contrôle de gestion de versions est défini pour correspondre à la valeur du paramètre. |
| `sql-server-2019` | Identique au cas de la valeur `sql-server-2017`, sauf que le paramètre et le contrôle sont définis sur 2019. |
| &nbsp; | &nbsp; |

### <a name="all-sql---hide-nothing-special-moniker"></a><a name="anchor-allsql-hidenothing"></a> Moniker spécial All SQL - Hide nothing (Tout SQL - Ne rien masquer)

Il existe un nom de produit de moniker spécial, **All SQL** (Tout SQL), dont l’unique version est **Hide nothing** (Ne rien masquer). Ce moniker est utilisé à des fins de test interne de certaines modifications. S’il est utilisé par un client, ce moniker est plus susceptible d’induire en erreur que d’informer.

Certains articles contiennent des informations relatives à plusieurs versions de SQL Server. Chaque moniker normal masque les sections avec version qui pourraient afficher des informations inexactes, confuses ou contradictoires pour la version du moniker. Le moniker **All SQL** (Tout SQL) spécial affichant toutes les sections avec version, l’affichage d’informations inexactes pourrait passer inaperçu.

## <a name="message-the-requested-page-is-not-available-for-moniker"></a><a name="anchor-message-unavailable-for-moniker"></a> Message : La page demandée n’est pas disponible pour \<moniker\>

Le scénario suivant entraîne l’affichage d’un message d’information en haut de la page web :::no-loc text="Docs"::: :

1. Le moniker de gestion de versions est **SQL Server 2017**.
2. Vous lisez un article qui concerne SQL Server 2017.
    - L’article _ne concerne pas_ le produit Azure SQL Database.
3. Vous tentez de définir le moniker sur **Azure SQL Database - current** (Azure SQL Database - version actuelle).
4. Vous voyez que votre tentative a été rejetée et un message s’affiche.

À la fin de ce scénario, le message d’information suivant s’affiche en haut de la page web Docs :

> La page demandée n’est pas disponible pour Azure SQL Database - current (Azure SQL Database - version actuelle). Vous avez été redirigé vers la dernière version du produit pour laquelle cette page est disponible.

La version _la plus récente_ peut exclure les versions qui ne sont pas encore entièrement publiées et qui sont en _préversion_.

![media_versioning-control-30-viewfallbackfrom.png](media/versioning-control-30-viewfallbackfrom.png)

## <a name="previous-versions-of-sql-server"></a>Versions antérieures de SQL Server

Le système de gestion de versions est entièrement implémenté pour SQL Server versions 2016 et ultérieures.

- _Versions 2012 et antérieures :_ &nbsp; le système de gestion de versions n’est pas utilisé pour SQL Server versions 2012 et antérieures.
    - Le moniker spécial **SQL Server - older** (SQL Server - version antérieure) est destiné à masquer presque tous les articles. Les rares exceptions sont quelques articles dont peuvent avoir besoin les clients de versions antérieures.
    - [Versions antérieures de SQL Server, 2012-2005](./previous-versions-sql-server.md)

- _2014 :_ &nbsp; le système de gestion de versions est partiellement implémenté pour SQL Server 2014. Vous pouvez choisir SQL Server 2014 dans le contrôle de gestion de versions ; cela fonctionne. Toutefois, en interne, les fichiers pour 2014 sont dédiés uniquement à 2014, de la même façon que les fichiers pour 2008 sont dédiés uniquement à 2008.
    - [Documentation SQL Server 2014 hors connexion](./sql-server-offline-documentation.md)

- _Versions 2016 et ultérieures :_ &nbsp; le système de gestion de versions est entièrement implémenté pour SQL Server versions 2016 et ultérieures.
    - [Bienvenue dans la documentation SQL Server versions 2016 et ultérieures](./index.yml?preserve-view=true&view=sql-server-2016)
    - [Documentation SQL Server 2016 hors connexion](sql-server-offline-documentation.md)

## <a name="see-also"></a>Voir aussi

[Versions antérieures de SQL Server, 2014-2005](./previous-versions-sql-server.md)  
[Guide de navigation dans la documentation de SQL Server](sql-docs-navigation-guide.md)  
[Guide pratique pour contribuer à la documentation SQL Server](sql-server-docs-contribute.md)