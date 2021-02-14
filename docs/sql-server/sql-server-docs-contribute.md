---
description: Guide pratique pour contribuer à la documentation SQL Server
title: Guide pratique pour contribuer à la documentation SQL Server | Microsoft Docs
ms.date: 08/13/2018
ms.prod: sql
ms.technology: release-landing
ms.reviewer: ''
ms.custom: ''
ms.topic: conceptual
author: MikeRayMSFT
ms.author: mikeray
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || >= sql-server-linux-2017'
ms.openlocfilehash: a0f3f665421c04208af1123e659aadcfe513d407
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080300"
---
# <a name="how-to-contribute-to-sql-server-documentation"></a>Guide pratique pour contribuer à la documentation SQL Server

[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md.md](../includes/appliesto-ss-asdb-asdw-pdw-md.md)]

Tout le monde peut apporter sa contribution à la documentation SQL Server. C’est-à-dire corriger des fautes de frappe, suggérer de meilleures explications et améliorer la précision technique. Cet article explique comment commencer à contribuer à du contenu et comment fonctionne la procédure.

Il existe deux flux de travail principaux que vous pouvez utiliser pour apporter votre contribution :

|Workflow|Description|
|---|---|
| [Modifier dans votre navigateur](#githubui) | Recommandé pour les modifications mineures et rapides d’un article. |
| [Modifier localement avec des outils](#tools) | Recommandé pour les modifications plus complexes, les modifications impliquant plusieurs articles et les contributions fréquentes à docs.microsoft.com. |

Toutes les contributions publiques sont examinées par l’équipe du contenu SQL afin de vérifier leur exactitude technique et leur cohérence. 

## <a name="edit-in-your-browser"></a><a id="githubui"></a> Modifier dans votre navigateur

Vous pouvez apporter des modifications de base au contenu SQL Server dans votre navigateur, puis les envoyer à Microsoft. Pour plus d’informations, consultez la [vue d’ensemble du guide du contributeur Microsoft Docs](/contribute/#quick-edits-to-existing-documents). 

Les étapes suivantes montrent la procédure à suivre : 

1. Dans la page sur laquelle vous souhaitez laisser des commentaires, sélectionnez le lien **Edit** (Modifier) en haut à droite.
1. Dans la page suivante, sélectionnez l’icône en forme de **crayon** en haut à droite.
1. Dans la page suivante, dans la fenêtre de texte **Edit file** (Modifier le fichier), apportez vos modifications directement dans le texte.
    Si vous avez besoin d’aide pour mettre en forme le texte que vous ajoutez ou celui que vous changez, consultez l’[aide-mémoire sur Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).
1. Une fois que vous avez apporté vos modifications, sous **Commit changes** (Valider les changements) :
    1. Dans la première zone de texte, entrez une brève description du changement que vous venez d’apporter.
    1. Dans la zone **Add an optional extended description** (Ajouter une description étendue facultative), entrez une brève explication de votre changement.
1. Sélectionnez **Propose file change** (Proposer un changement de fichier).
1. Dans la page **Comparing changes** (Comparaison des changements), sélectionnez **Create pull request** (Créer une demande de tirage). 
1. Dans la page **Open a pull request** (Ouvrir une demande de tirage), sélectionnez **Create pull request** (Créer une demande de tirage). 

L’image GIF suivante montre le processus de bout en bout pour soumettre des changements dans votre navigateur :

![Modifier la documentation SQL](media/sql-server-docs-navigation-guide/edit-sql-docs.gif)

## <a name="edit-locally-with-tools"></a><a id="tools"></a> Modifier localement avec des outils

Une autre option de modification consiste à dupliquer le dépôt **sql-docs** ou **azure-docs**, puis à le cloner localement sur votre ordinateur. Vous pouvez ensuite utiliser un éditeur Markdown et un client Git pour soumettre les modifications. Ce flux de travail est approprié pour les modifications plus complexes ou qui impliquent plusieurs fichiers. Il est également adapté pour les contributeurs réguliers de docs.microsoft.com.

Pour apporter votre contribution à l’aide de cette méthode, consultez les articles suivants :

- [Créer un compte GitHub](/contribute/get-started-setup-github)
- [Installer des outils de création de contenu](/contribute/get-started-setup-tools)
- [Configurer localement un dépôt Git](/contribute/get-started-setup-local)
- [Apporter sa contribution à l’aide d’outils](/contribute/how-to-write-workflows-major)

Si vous envoyez une demande de tirage avec d’importants changements à apporter à la documentation, un commentaire s’affiche dans GitHub vous demandant d’envoyer un **contrat de licence de contribution** (CLA, Contribution License Agreement) en ligne. Pour que votre demande de tirage soit acceptée, vous devez au préalable compléter le formulaire en ligne.

## <a name="recognition"></a>Reconnaissance

Si vos changements sont acceptés, vous êtes reconnu comme contributeur en haut de l’article.

![Reconnaissance de la contribution au contenu](./media/sql-server-docs-contribute/contribution-recognition.png)

## <a name="sql-docs-overview"></a>Vue d’ensemble de sql-docs

Cette section fournit des conseils supplémentaires sur l’utilisation du dépôt **sql-docs**.

> [!IMPORTANT]
> Les informations de cette section sont spécifiques à **sql-docs**. Si vous modifiez un article SQL dans la documentation Azure, consultez [le fichier Lisez-moi du dépôt azure-docs sur GitHub](https://github.com/MicrosoftDocs/azure-docs/blob/master/README.md).

Le dépôt [sql-docs](https://github.com/MicrosoftDocs/sql-docs) utilise plusieurs dossiers standard pour organiser le contenu.

| Dossier | Description |
|---|---|
| [docs](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs) | Contient tout le contenu SQL Server publié. Les sous-dossiers organisent de manière logique les différents domaines du contenu. |
| [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes) | Contient les fichiers include. Ces fichiers sont des blocs de contenu qui peuvent être inclus dans une ou plusieurs autres rubriques. |
| **./media** | Chaque dossier peut avoir un sous-dossier **media** pour les images des articles. Le dossier **media** comprend à son tour des sous-dossiers portant le même nom que les rubriques dans lesquelles l’image apparaît. Les images doivent être des fichiers .png avec toutes les lettres en minuscules et sans espaces. |
| **TOC.MD** | Fichier de table des matières. Chaque sous-dossier a la possibilité d’utiliser un fichier TOC.MD. |

#### <a name="applies-to-includes"></a>Fichiers include Applies-to

Chaque article SQL Server contient un fichier include **applies-to** après le titre. Cela indique les domaines ou les versions de SQL Server auxquels l’article s’applique.

Prenons l’exemple Markdown suivant qui extrait le fichier include **appliesto-ss-asdb-asdw-pdw-md.md**.

```Markdown
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
```

Cette opération ajoute le texte suivant en haut de l’article :

![Texte « S’applique à »](./media/sql-server-docs-contribute/applies-to.png)

Pour trouver le fichier include applies-to approprié à votre article, utilisez les conseils suivants :

- Pour une liste de fichiers include couramment utilisés, consultez [Version SQL Server et fichiers include applies-to](applies-to-includes.md).
- Consultez d’autres articles qui traitent de la même fonction ou d’une tâche associée. Si vous modifiez cet article, vous pouvez copier le texte Markdown du lien du fichier include applies-to (vous pouvez annuler la modification sans la soumettre).
- Recherchez les fichiers contenant le texte « applies-to » dans le répertoire [docs/includes](https://github.com/MicrosoftDocs/sql-docs/tree/live/docs/includes). Vous pouvez utiliser le bouton **Find** (Trouver) dans GitHub pour appliquer rapidement un filtre. Cliquez sur le fichier pour voir comment il est restitué.
- Faites attention à la convention de nommage. Si le nom contient des x, ce sont généralement des espaces réservés indiquant l’absence de prise en charge d’un service. Par exemple, **appliesto-xx-xxxx-asdw-xxx-md.md** indique la prise en charge d’Azure Synapse Analytics uniquement, car seul **asdw** est indiqué clairement, tandis que les autres champs contiennent des x.
- Certains fichiers include spécifient un numéro de version, comme **tsql-appliesto-ss2017-xxxx-xxxx-xxx-md.md**. Utilisez ces fichiers include seulement si vous savez que la fonctionnalité a été introduite avec une version spécifique de SQL Server.

## <a name="contributor-resources"></a>Ressources pour les contributeurs

- [Guide des contributeurs pour docs.microsoft.com](/contribute/)
- [Guide de style de Microsoft](/teamblog/style-guide)
- [Principes de base de Markdown](https://help.github.com/articles/getting-started-with-writing-and-formatting-on-github/)

> [!TIP]
> Si vous avez des commentaires sur le produit plutôt que sur la documentation, [fournissez vos commentaires sur le produit SQL Server ici](https://feedback.azure.com/forums/908035-sql-server).

## <a name="next-steps"></a>Étapes suivantes

Explorez le [dépôt sql-docs](https://github.com/MicrosoftDocs/sql-docs) sur GitHub.

Recherchez un article, envoyez une modification et aidez la Communauté SQL Server. 

Merci !