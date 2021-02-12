---
title: Déployer SQL Server sur une machine virtuelle Azure
description: Ce tutoriel montre comment créer SQL Server sur des machines virtuelles Azure.
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, maghan
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: c5cffc4964e7a91b3b63da56e43e01db41bbe8a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048629"
---
# <a name="create-sql-server-on-azure-virtual-machines-using-azure-data-studio"></a>Créer SQL Server sur des machines virtuelles Azure à l’aide d’Azure Data Studio

Vous pouvez créer une machine virtuelle SQL en utilisant Azure Data Studio par le biais de l’Assistant Déploiement et des notebooks.

## <a name="pre-requisites"></a>Conditions préalables

- [Azure Data Studio](download-azure-data-studio.md) est installé
- Un compte et un abonnement Azure actifs. Si vous n’en avez pas, [créez un compte gratuit](https://azure.microsoft.com/free/).

## <a name="use-the-deployment-wizard"></a>Utiliser l’Assistant Déploiement

Effectuez les étapes suivantes pour utiliser l’Assistant Déploiement, qui vous aidera à configurer les paramètres requis dans une interface utilisateur simple.

Tout d’abord, recherchez et sélectionnez Machine virtuelle Azure SQL dans l’Assistant Déploiement.

1. Dans Azure Data Studio, sélectionnez la viewlet Connexions dans le volet de navigation gauche.

2. Sélectionnez le bouton **...** en haut du panneau Connexions, puis choisissez **Nouveau déploiement**.

3. Dans l’Assistant Déploiement, sélectionnez la vignette **Machine virtuelle Azure SQL** et cochez la case d’acceptation des conditions générales.

4. Si vous y êtes invité, installez les outils nécessaires, puis sélectionnez le bouton **Sélectionner** en bas.

Ensuite, entrez tous les paramètres requis dans l’Assistant Machine virtuelle Azure SQL.

1. Connectez-vous à votre compte Azure si ce n’est déjà fait. Vous pouvez actualiser votre connexion si vous avez des problèmes dans cette page de l’Assistant.

2. Sélectionnez l’abonnement, le groupe de ressources et la région souhaités. Sélectionnez ensuite **Suivant**.

3. Entrez un nom de machine virtuelle unique et vos informations d’identification (nom d’utilisateur et mot de passe).

4. Sélectionnez l’image, la référence SKU et la version de votre choix, puis sélectionnez la taille de machine virtuelle souhaitée. Vous pouvez en savoir plus sur les [tailles de machine virtuelle disponibles](/azure/virtual-machines/sizes) pour vous aider à choisir. Sélectionnez ensuite **Suivant**.

5. Sélectionnez un réseau virtuel existant dans la liste déroulante, ou cochez la case **Nouveau réseau virtuel** pour entrer un nom de nouveau réseau virtuel.

6. Procédez de la même façon pour sélectionner ou créer un sous-réseau et une adresse IP publique.

7. Si vous souhaitez vous connecter à votre machine virtuelle par le biais de Bureau à distance (RDP), cochez la case **Activer le port d’entrée RDP**. Sélectionnez ensuite **Suivant**.

8. Entrez les paramètres SQL Server de votre choix. Pour tester cette expérience, nous vous recommandons de définir la connectivité SQL sur **Publique (Internet)** , d’entrer le port 1433 et d’activer l’authentification SQL avec votre nom d’utilisateur et votre mot de passe préférés. Sélectionnez ensuite **Suivant**.

9. Passez en revue les paramètres que vous avez entrés, puis sélectionnez **Exécuter un script sur un notebook**.

Une fois le notebook ouvert, vous pouvez examiner le contenu et le code, et apporter des modifications si vous le souhaitez. Toutefois, il n’est pas recommandé d’apporter des modifications, car cela peut entraîner des erreurs de validation.

La dernière étape consiste à sélectionner **Tout exécuter** pour exécuter toutes les cellules du notebook. Une fois cette opération terminée, les éléments suivants devraient être créés et en cours d’exécution :

- Une machine virtuelle Azure
- Une machine virtuelle SQL
- Un réseau virtuel, un sous-réseau et une adresse IP publique
- Un groupe de sécurité réseau et une interface réseau
- Accès à RDP dans la machine virtuelle
- Accédez à diverses fonctionnalités de gestion SQL Server pour contrôler facilement la planification des sauvegardes, la planification des mises à jour correctives, l’édition SQL Server et les licences, les configurations de performances de stockage et bien plus encore.

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la façon de migrer vos données vers la nouvelle machine virtuelle SQL Server, consultez l’article suivant.

> [!div class="nextstepaction"]
> [Migrer une base de données SQL Server vers SQL Server dans une machine virtuelle Azure](/azure/azure-sql/virtual-machines/windows/migrate-to-vm-from-sql-server)

Pour en savoir plus sur l’utilisation de SQL Server dans Azure, consultez [Présentation de SQL Server sur les machines virtuelles](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) Azure et le [Forum Aux Questions](/azure/azure-sql/virtual-machines/windows/frequently-asked-questions-faq) associé.