---
title: Installation des composants SSMA sur SQL Server (OracleToSQL) | Microsoft Docs
description: Découvrez comment installer le pack d’extension SSMA et les fournisseurs Oracle sur l’ordinateur qui exécute SQL Server pour prendre en charge la conversion de bases de données Oracle.
ms.prod: sql
ms.custom: ''
ms.date: 11/16/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Installing the extension pack
- SQL Server Database Objects
ms.assetid: 33070e5f-4e39-4b70-ae81-b8af6e4983c5
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 81d3f542e64030dbf6a0bf70067307222bef5a8b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344215"
---
# <a name="installing-ssma-components-on-sql-server-oracletosql"></a>Installation des composants SSMA sur SQL Server (OracleToSQL)

Outre l’installation de SSMA, vous devez également installer les composants sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants incluent le pack d’extension SSMA, qui prend en charge la migration des données et les fournisseurs Oracle pour activer la connectivité de serveur à serveur.

## <a name="ssma-for-oracle-extension-pack"></a>SSMA pour le pack d’extension Oracle

Le pack d’extension SSMA déploie des procédures stockées étendues et ajoute les bases de données **sysdb** et **ssmatesterdb** à l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Les procédures stockées étendues fournissent les fonctionnalités requises pour émuler les fonctionnalités et les behaiov d’Oracle, tandis que la base de données **sysdb** contient les tables et les procédures stockées nécessaires à la migration des données. La base de données **ssmatesterdb** contient les tables et les procédures requises par le composant testeur (le cas échéant).

En outre, lorsque vous migrez des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des travaux de l’agent lorsque le moteur de migration de données côté serveur est utilisé pour la migration des données.

### <a name="prerequisites"></a>Prérequis

Avant d’installer les composants SSMA pour Oracle Server sur, assurez-vous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que le système répond aux exigences suivantes :

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’instance est installée.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Le fournisseur OLE DB pour Oracle (si vous utilisez OLE DB) et la connectivité à la base de données Oracle que vous souhaitez migrer. Vous pouvez installer des fournisseurs à partir du support produit Oracle ou d’un site Web Oracle.
- Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser doit être en cours d’exécution pendant l’installation. Cette valeur est utilisée pour remplir une liste des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’Assistant installation. Vous pouvez désactiver le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser après l’installation.

  > [!NOTE]
  > Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser est en cours d’exécution, mais que vous ne voyez toujours pas la liste des instances dans le programme d’installation, vous devez débloquer le port UDP 1434. Vous pouvez utiliser le pare-feu Windows pour débloquer temporairement le port, ou vous pouvez désactiver temporairement le pare-feu Windows. Vous devrez peut-être également désactiver temporairement le logiciel antivirus. Veillez à activer les pare-feu et les logiciels antivirus après l’installation.

### <a name="installing-the-extension-pack"></a>Installation du pack d’extension

Vous pouvez installer le pack d’extension à tout moment avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Pour installer le pack d’extension, vous devez être membre du rôle de serveur **sysadmin** sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Pour installer le pack d’extension :

1. Copiez **SSMAforOracleExtensionPack_ *n*. msi** (où *n* est le numéro de Build) sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Double-cliquez sur **SSMAforOracleExtensionPack_ *n*. msi**.
3. Sur la page d’**accueil**, cliquez sur **Suivant**.
4. Sur la page **contrat de licence utilisateur final** , lisez le contrat de licence. Si vous acceptez, sélectionnez **l’option j’accepte le contrat** , puis cliquez sur **suivant**.
5. Dans la page **choisir le type d’installation** , sélectionnez **standard**.
6. Sur la page **prêt pour l’installation** , sélectionnez **installer**.
7. Dans la page **fin de la première étape de l’installation** , sélectionnez **suivant**.
  
   Une nouvelle boîte de dialogue s’affiche. Sélectionnez le type de Pack d’extension.
  
8. Sélectionnez le type d’installation souhaité, puis cliquez sur **suivant**.

   > [!IMPORTANT]
   > L’option Remote ne doit être utilisée que lors de l’installation du pack d’extension sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s’exécutant sur Linux ou lors du ciblage [!INCLUDE[ssAzureMi](../../includes/ssazuremi_md.md)] . [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] les installations s’exécutant sur Windows doivent toujours disposer d’un pack d’extension installé localement. [!INCLUDE[ssAzure](../../includes/ssazure_md.md)] et Azure Synapse Analytics ne prennent pas en charge le pack d’extension.

   Si vous installez le pack d’extension sur une [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] instance locale, la page suivante vous permettra de choisir une instance locale de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers laquelle vous allez migrer des schémas Oracle. Choisissez une instance dans la liste déroulante, puis sélectionnez **suivant**.

   L’instance par défaut porte le même nom que l’ordinateur. Les instances nommées sont suivies d’une barre oblique inverse et du nom de l’instance.

9. Sur la page connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.

   L’authentification Windows utilise vos informations d’identification Windows pour essayer de vous connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous sélectionnez authentification du serveur, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nom de connexion et un mot de passe.

10. L’étape suivante vous oblige à définir le mot de passe d’une clé principale qui sera utilisée pour chiffrer les données sensibles stockées dans la base de données du pack d’extension lors de la migration des données côté serveur. Fournissez un mot de passe fort, puis cliquez sur **suivant**.

11. Sur la page suivante, sélectionnez **installer les utilitaires base de données *n* et installer les bibliothèques d’extensions**, où *n* est le numéro de version. Si vous prévoyez d’utiliser la fonctionnalité testeur, activez la case à cocher **installer la base de données du testeur** , puis sélectionnez **suivant**.

    La base de données **sysdb** est créée avec les tables et les procédures stockées requises pour la migration des données (à l’aide du moteur de migration de données côté serveur) sont créées dans cette base de données.

    Si l’option **installer la base de données du testeur** est cochée, la base de données **ssmatesterdb** est créée.

12. Une fois l’installation terminée, une invite s’affiche pour vous demander si vous souhaitez installer la base de données des utilitaires sur une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionner **Oui**, puis sélectionner **suivant**, ou quitter l’Assistant, sélectionner **non** , puis **quitter**.

13. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] ou à l’aide de l' `sqlcmd` utilitaire, exécutez le script suivant pour activer le CLR :

    ```sql
    sp_configure 'clr enabled', 1
    GO
    RECONFIGURE
    GO
    ```

    Si le CLR n’est pas activé, vous recevrez l’erreur suivante lorsque SSMA se connecte à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] :

    > SSMA n’a pas pu récupérer les informations de version de l’assembly du pack d’extension. Réinstallez le pack d’extension sur le serveur de base de données.

### <a name="sql-server-database-objects"></a>SQL Server les objets de base de données

Une fois que vous avez installé le pack d’extension, une table **ssma_oracle. bcp _migration_packages** apparaît dans la base de données **sysdb** .

Chaque fois que vous migrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des données vers, SSMA crée un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] travail de l’agent. Ces travaux sont nommés **ssma_oracle package de migration de données {GUID}** et sont visibles dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nœud agent de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dans le dossier travaux.

Les procédures stockées étendues sont également ajoutées à la base de données **Master** :

- `xp_ora2ms_exec2`
- `xp_ora2ms_exec2_ex`
- `xp_ora2ms_versioninfo2`

## <a name="see-also"></a>Voir aussi

- [Installation de SSMA pour Oracle client](../../ssma/oracle/installing-ssma-for-oracle-client-oracletosql.md)
- [Migration de bases de données Oracle vers SQL Server](../../ssma/oracle/migrating-oracle-databases-to-sql-server-oracletosql.md)
