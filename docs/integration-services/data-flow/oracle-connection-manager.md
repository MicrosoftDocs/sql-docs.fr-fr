---
description: Gestionnaire de connexions Oracle
title: Gestionnaire de connexions Oracle | Microsoft Docs
ms.custom: ''
ms.date: 08/14/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 258cce05d25b02b046c9fc4e79bd031d6b27fc22
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343249"
---
# <a name="oracle-connection-manager"></a>Gestionnaire de connexions Oracle

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Un gestionnaire de connexions Oracle permet à un package d’extraire des données à partir de bases de données Oracle et de charger des données dans des bases de données Oracle.

La propriété **ConnectionManagerType** du gestionnaire de connexions Oracle a pour valeur **ORACLE**.

## <a name="configuring-the-oracle-connection-manager"></a>Configuration du gestionnaire de connexions Oracle

Les modifications apportées à la configuration du gestionnaire de connexions Oracle seront résolues par Integration Services au moment de l’exécution. Utilisez la boîte de dialogue **Éditeur du gestionnaire de connexions Oracle** pour ajouter une connexion à une source de données Oracle.

![Gestionnaire de connexions](media/oracle-connection-manager.png)

### <a name="options"></a>Options

#### <a name="connection-manager-information"></a>Informations sur le gestionnaire de connexions

Entrez les informations sur la connexion Oracle.

**Nom**

Entrez un nom pour la connexion Oracle. Le nom par défaut est Gestionnaire de connexions Oracle. 

**Description** 

Entrez la description de la connexion. Cette entrée est facultative.

**Nom du service TNS**

Entrez le nom de la base de données Oracle avec laquelle vous travaillez. Le nom du service TNS peut être :

- Le nom du descripteur de connexion défini dans le fichier tnsnames.ora qui se trouve dans le dossier admin du client Oracle.

- Format EzConnect :[//]hôte[:port][/nom_service]

Pour plus d'informations, consultez votre documentation Oracle.

#### <a name="connection-manager-logging"></a>Journalisation du gestionnaire de connexions

Sélectionnez l’une des options ci-dessous :

- **Utiliser l’authentification Windows** : sélectionnez cette option pour utiliser l’authentification Windows.

- **Utiliser l’authentification Oracle** : sélectionnez cette option pour utiliser l’authentification de base de données Oracle. Si vous utilisez cette authentification, entrez vos informations d’identification Oracle comme suit :  
    **Nom d’utilisateur** : tapez le nom d’utilisateur utilisé pour se connecter à la base de données Oracle.  
    **Mot de passe** : tapez le mot de passe de base de données Oracle de l’utilisateur mentionné dans le champ Nom d’utilisateur.

> [!NOTE]
>
>L’authentification Windows n’est pas prise en charge pour Oracle Server 18C.

**Tester la connexion**

Cliquez sur **Tester la connexion** pour vérifier si les informations fournies sont correctes. Vous recevrez le message **Le test de la connexion a réussi** si les informations entrées permettent de se connecter à la base de données Oracle.

### <a name="custom-properties"></a>Propriétés personnalisées

Le gestionnaire de connexions Oracle propose les propriétés de gestionnaire de connexions personnalisées suivantes :

- **EnableDetailedTracing** : non utilisée.

- **OracleHome** : spécifiez le nom d’Oracle Home 32 bits ou le dossier à utiliser par le connecteur. (facultatif)

- **OracleHome64** : spécifiez le nom d’Oracle Home 64 bits ou le dossier à utiliser par le connecteur lors de l’exécution en mode 64 bits. (facultatif)

Les propriétés personnalisées ne sont pas listées dans l’éditeur du gestionnaire de connexions Oracle. Pour définir les propriétés **OracleHome** et **OracleHome64**

1. Dans la zone Gestionnaire de connexions, cliquez avec le bouton droit sur le gestionnaire de connexions Oracle que vous utilisez et sélectionnez **Propriétés**.

2. Dans le volet **Propriétés**, affectez à la propriété **OracleHome** ou **OracleHome64** le chemin complet du répertoire de démarrage Oracle.

## <a name="next-steps"></a>Étapes suivantes

- Configurer la [Source Oracle](oracle-source.md).
- Configurer la [Destination Oracle](oracle-destination.md).
- Si vous avez des questions, rendez-vous sur [TechCommunity](https://aka.ms/AA5u35j).
