---
title: Virtualiser des données externes
description: Cette page détaille les étapes d’utilisation de l’Assistant Création d’une table externe pour des sources de données ODBC.
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mikeray
ms.date: 12/13/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: polybase
monikerRange: '>= sql-server-ver15'
ms.metadata: seo-lt-2019
ms.openlocfilehash: 85365639565eaf42ca1bf23825016a08ef8f50bf
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100081340"
---
# <a name="use-the-external-table-wizard-with-odbc-data-sources"></a>Utiliser l’Assistant Table externe avec des sources de données ODBC

Un des principaux scénarios pour SQL Server 2019 est la possibilité de virtualiser des données. Ce processus permet aux données de rester à leur emplacement d’origine. Vous pouvez *virtualiser* les données dans une instance de SQL Server, ce qui vous permet de les interroger comme n’importe quelle autre table dans SQL Server. Ce processus réduit la nécessité de recourir à des processus ETL. Il est possible grâce à l’utilisation de connecteurs PolyBase. Pour plus d’informations sur la virtualisation des données, consultez [Bien démarrer avec PolyBase](polybase-guide.md).

Cette vidéo fournit une introduction à la virtualisation des données :

> [!VIDEO https://channel9.msdn.com/Shows/Data-Exposed/Introducing-Data-Virtualization/player?WT.mc_id=dataexposed-c9-niner]


## <a name="start-the-external-table-wizard"></a>Démarrer l’Assistant Table externe

Connectez-vous à l’instance principale en utilisant l’adresse IP/numéro de port du point de terminaison **sql-server-master** obtenu à l’aide de la commande [**azdata cluster endpoints list**](../../big-data-cluster/deployment-guidance.md#endpoints). Développez votre nœud **Bases de données** dans l’Explorateur d’objets. Sélectionnez ensuite l’une des bases de données où vous voulez virtualiser les données à partir d’une instance de SQL Server existante. Cliquez avec le bouton droit sur la base de données, puis sélectionnez **Créer une table externe** pour démarrer l’Assistant Virtualisation de données. Vous pouvez également démarrer l’Assistant Virtualisation de données à partir de la palette de commandes. Utilisez Ctrl+Maj+P dans Windows ou Cmd+Maj+P avec un Mac.

![Assistant Virtualisation de données](media/data-virtualization/virtualize-data-wizard.png)
## <a name="select-a-data-source"></a>Sélectionner une source de données

Si vous avez démarré l’Assistant à partir de l’une des bases de données, la zone de liste déroulante de destination est remplie automatiquement. Vous avez également la possibilité d’entrer ou de changer la base de données de destination dans cette page. Les types de sources de données externes pris en charge par l’Assistant sont SQL Server, Oracle, MongoDB et Teradata.

> [!NOTE]
>SQL Server est mis en surbrillance par défaut.


![Sélectionner une source de données](media/data-virtualization/select-data-source.png)

Sélectionnez **Suivant** pour continuer.

## <a name="create-a-database-master-key"></a>Créer une clé principale de base de données

Lors de cette étape, vous allez créer une clé principale de base de données. La création d’une clé principale est obligatoire. Une clé principale sécurise les informations d’identification utilisées par une source de données externe. Choisissez un mot de passe fort pour votre clé principale. Sauvegardez également la clé principale à l’aide de **BACKUP MASTER KEY**. Conservez la sauvegarde en lieu sûr, en dehors de votre lieu de travail.

![Créer une clé principale de base de données](media/data-virtualization/virtualize-data-master-key.png)

> [!IMPORTANT]
> Si vous disposez déjà d’une clé principale de base de données, cette étape est ignorée automatiquement.

## <a name="enter-external-data-source-credentials"></a>Entrer les informations d’identification de la source de données externe

Lors de cette étape, entrez votre source de données externe et les détails des informations d’identification pour créer un objet de source de données externe. Les informations d’identification permettent à l’objet de base de données de se connecter à la source de données. Entrez un nom pour la source de données externe, par exemple Test. Fournissez des détails sur la connexion SQL Server à la source de données externe. Entrez le **Nom du serveur** et le **Nom de la base de données** où vous souhaitez créer votre source de données externe.

L’étape suivante consiste à configurer les informations d’identification. Entrez un nom pour les informations d’identification. Ce nom correspond aux informations d’identification incluses dans l’étendue de la base de données qui servent à stocker en toute sécurité les informations de connexion pour la source de données externe que vous créez. par exemple `TestCred`. Entrez un nom d’utilisateur et un mot de passe pour la connexion à la source de données.

![Capture d’écran montrant l’étape 3 : Créer une connexion à votre source de données.](media/data-virtualization/data-source-credentials.png)

## <a name="external-data-table-mapping"></a>Mappage de tables de données externes

Dans la page suivante, sélectionnez les tables pour lesquelles vous souhaitez créer des vues externes. Quand vous sélectionnez des bases de données parentes, les tables enfants sont également incluses. Une fois les tables sélectionnées, une table de mappage apparaît à droite. Ici, vous pouvez modifier les types. Vous pouvez également changer le nom de la table externe sélectionnée.

![Capture d’écran montrant l’étape 4 : Mapper vos objets de source de données à votre table externe.](media/data-virtualization/data-table-map.png)

> [!NOTE]
>Pour changer la vue du mappage, double-cliquez sur une autre table sélectionnée.

> [!IMPORTANT]
>Le type « photo » n’est pas pris en charge par l’outil Table externe. Si vous créez une vue externe contenant un type de photo, une erreur s’affiche après la création de la table, mais la table est quand même créée.

## <a name="summary"></a>Résumé

Cette étape fournit un récapitulatif de vos sélections. Elle indique le nom des informations d’identification incluses dans l’étendue de la base de données et les objets de source de données externe créés dans la base de données de destination. Sélectionnez **Générer un script** pour générer un script T-SQL de la syntaxe utilisée pour créer la source de données externe. Sélectionnez **Créer** pour créer l’objet de source de données externe.

![Écran Récapitulatif](media/data-virtualization/virtualize-data-summary.png)

Si vous sélectionnez **Créer**, l’objet de source de données externe créé dans la base de données de destination apparaît.

![Sources de données externes](media/data-virtualization/external-data-sources.png)

Si vous sélectionnez **Générer un script**, la requête T-SQL qui est générée pour créer l’objet de source de données externe apparaît.

![Générer un script](media/data-virtualization/generated-script.png)

> [!NOTE]
> L’option **Générer un script** doit normalement être visible seulement dans la dernière page de l’Assistant. Actuellement, elle apparaît dans toutes les pages.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur les clusters Big Data SQL Server et les scénarios associés, consultez [Présentation des clusters Big Data SQL Server](../../big-data-cluster/big-data-cluster-overview.md).
