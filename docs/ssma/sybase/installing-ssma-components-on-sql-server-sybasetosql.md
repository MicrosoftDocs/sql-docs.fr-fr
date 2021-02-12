---
description: Installation des composants SSMA sur SQL Server (SybaseToSQL)
title: Installation des composants SSMA sur SQL Server (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/14/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 5ad9e12c-2cdb-4dd2-8703-05a23242d19d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 82f5d7eef228326f0dba9b194b87bffce4bb5eff
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078230"
---
# <a name="installing-ssma-components-on-sql-server-sybasetosql"></a>Installation des composants SSMA sur SQL Server (SybaseToSQL)

Outre l’installation de SSMA, pour l’utilisation de la migration des données côté serveur, vous devez également installer les composants sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants incluent le pack d’extension SSMA, qui prend en charge la migration des données et les fournisseurs Sybase pour activer la connectivité de serveur à serveur.

## <a name="ssma-for-sybase-extension-pack"></a>SSMA pour le pack d’extension Sybase

Le pack d’extension SSMA ajoute les bases de données, **sysdb** et **ssmatesterdb_syb**, à l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . La base de données **sysdb** contient les tables et les procédures stockées nécessaires à la migration des données. La base de données **ssmatester_syb** contient le **ssma_sybase_utilities** de schéma, dans lequel les objets (tables, déclencheurs, vues) utilisés par le composant testeur SSMA sont créés.

En outre, lorsque vous migrez des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des travaux de l’agent lorsque le moteur de migration de données côté serveur est utilisé pour la migration des données.

### <a name="prerequisites"></a>Prérequis

Avant d’installer les composants SSMA pour Sybase Server sur, assurez-vous [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] que le système répond aux exigences suivantes :

- [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] l’instance est installée.
- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Le fournisseur Sybase OLE DB/ADO.Net/ODBC et la connectivité au serveur de base de données SAP ASE qui contient les bases de données que vous souhaitez migrer. Vous pouvez installer des fournisseurs à partir du support produit SAP ASE. Pour plus d’informations sur la connectivité, consultez [connexion à Sybase ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/connecting-to-sybase-ase-sybasetosql.md).
- Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser doit être en cours d’exécution pendant l’installation. Cette valeur est utilisée pour remplir une liste des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’Assistant installation. Vous pouvez désactiver le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser après l’installation.

  > [!NOTE]
  > Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser est en cours d’exécution, mais que vous ne voyez toujours pas la liste des instances dans le programme d’installation, vous devez débloquer le port UDP 1434. Vous pouvez utiliser le pare-feu Windows pour débloquer temporairement le port, ou vous pouvez désactiver temporairement le pare-feu Windows. Vous devrez peut-être également désactiver temporairement le logiciel antivirus. Veillez à activer les pare-feu et les logiciels antivirus après l’installation.

### <a name="installing-the-extension-pack"></a>Installation du pack d’extension

Vous pouvez installer le pack d’extension à tout moment avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Pour installer le pack d’extension, vous devez être membre du rôle de serveur sysadmin sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Pour installer le pack d’extension :

1. Copiez **SSMAforSybaseExtensionPack_ *n*. msi**, où *n* est le numéro de build, sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Double-cliquez sur **SSMAforSybaseExtensionPack_ *n*. msi**.
3. Sur la page d’**accueil**, cliquez sur **Suivant**.
4. Sur la page **contrat de licence utilisateur final** , lisez le contrat de licence. Si vous acceptez, sélectionnez l’option **J’accepte le contrat** , puis cliquez sur **suivant**.
5. Dans la page **choisir le type d’installation** , cliquez sur par **défaut**.
6. Dans la page **Prêt pour l'installation**, cliquez sur **Installer**.
7. Dans la page **fin de la première étape de l’installation** , cliquez sur **suivant**.

   Une nouvelle boîte de dialogue s’affiche, dans laquelle vous sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l’installation du pack d’extension.

8. Sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous allez migrer les bases de données SAP ASE, puis cliquez sur **suivant**.

   L’instance par défaut porte le même nom que l’ordinateur. Les instances nommées sont suivies d’une barre oblique inverse et du nom de l’instance.

9. Sur la page connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.

   L’authentification Windows utilise vos informations d’identification Windows pour essayer de se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous sélectionnez authentification du serveur, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nom de connexion et un mot de passe.

10. L’étape suivante vous oblige à définir le mot de passe d’une clé principale qui sera utilisée pour chiffrer les données sensibles stockées dans la base de données du pack d’extension lors de la migration des données côté serveur. Fournissez un mot de passe fort, puis cliquez sur **suivant**.

11. Sur la page suivante, sélectionnez **installer les utilitaires base de données *n* et installer les bibliothèques d’extensions**, où *n* est le numéro de version. Si vous prévoyez d’utiliser la fonctionnalité testeur, activez la case à cocher **installer la base de données du testeur** , puis sélectionnez **suivant**.

    La base de données **sysdb** est créée avec les tables et les procédures stockées requises pour la migration des données (à l’aide du moteur de migration de données côté serveur) sont créées dans cette base de données.

    Si l’option **installer la base de données du testeur** est cochée, la base de données **ssmatesterdb_syb** sera créée.

12. Une fois l’installation terminée, une invite s’affiche pour vous demander si vous souhaitez installer la base de données des utilitaires sur une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionner **Oui**, puis sélectionner **suivant**, ou quitter l’Assistant, sélectionner **non** , puis **quitter**.

### <a name="sql-server-database-objects"></a>SQL Server les objets de base de données

Après avoir installé le pack d’extension, vous verrez une table **ssma_syb. bcp _migration_packages** dans la base de données **sysdb** . Vous verrez également les procédures stockées suivantes :

- `bcp_clean_migration_data`
- `bcp_ensure_message_table`
- `bcp_insert_new_message`
- `bcp_post_process`
- `bcp_read_new_migration_messages`
- `bcp_save_migration_package`
- `bcp_smart_truncate`
- `bcp_start_migration_process`
- `get_jobstep_info`
- `stop_agent_process`

Chaque fois que vous migrez des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crée un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] travail de l’agent. Ces travaux sont nommés **ssma_syb package de migration de données {GUID}** et sont visibles dans le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nœud agent de [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] dans le dossier travaux.  

## <a name="sybase-providers"></a>Fournisseurs Sybase

Lorsque vous utilisez la migration de données côté serveur pour déplacer des données de SAP ASE vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , les données sont migrées directement entre SAP ASE et [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Il ne passe pas par SSMA, car cela ralentirait la migration des données.

### <a name="installing-the-sybase-providers"></a>Installation des fournisseurs Sybase

Les instructions suivantes fournissent les étapes d’installation de base pour l’installation des fournisseurs Sybase. Les instructions exactes varient en fonction de la version du programme d’installation de Sybase.

> [!IMPORTANT]
> Avant d’exécuter le programme d’installation, vérifiez que vous ne respectez pas vos contrats de licence.

1. Exécutez le programme d’installation de Sybase ASE.
2. Sélectionnez installation personnalisée.
3. Dans la page sélection de fonctionnalités, sélectionnez les fournisseurs de données ODBC, OLE DB et ADO.NET.
4. Vérifiez les fonctionnalités sélectionnées, puis cliquez sur **Terminer** pour installer le fournisseur de données.

## <a name="see-also"></a>Voir également

- [Installation de SSMA pour Sybase client](../../ssma/sybase/installing-ssma-for-sybase-client-sybasetosql.md)
- [Migration de bases de données Sybase ASE vers SQL Server Azure SQL Database](../../ssma/sybase/migrating-sybase-ase-databases-to-sql-server-azure-sql-db-sybasetosql.md)
