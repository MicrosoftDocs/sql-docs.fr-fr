---
title: Installation des composants SSMA sur SQL Server (MySQLToSql) | Microsoft Docs
description: Installez les composants sur le serveur qui exécute SQL Server pour prendre en charge la conversion de base de données MySQL avec SSMA, y compris le pack d’extension SSMA et les fournisseurs MySQL.
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- SSMA extension pack, Installation
ms.assetid: 6772d0c5-258f-4d7b-afb0-b5f810e71af1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 48c111ab342d7410031851a5635fa73fe143b96b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070194"
---
# <a name="installing-ssma-components-on-sql-server-mysqltosql"></a>Installation des composants SSMA sur SQL Server (MySQLToSql)

Outre l’installation de SSMA, vous devez également installer les composants sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Ces composants incluent le pack d’extension SSMA, qui prend en charge la migration des données et les fournisseurs MySQL pour activer la connectivité de serveur à serveur.

## <a name="ssma-for-mysql-extension-pack"></a>SSMA pour MySQL Pack d’extension

Le pack d’extension SSMA ajoute une base de données, **sysdb**, à l’instance spécifiée de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Cette base de données contient les tables et les procédures stockées nécessaires à la migration des données.

En outre, lorsque vous migrez des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , SSMA crée [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] des travaux de l’agent, lorsque le moteur de migration de données côté serveur est utilisé pour la migration des données.

### <a name="prerequisites"></a>Prérequis

Avant d’installer les composants du serveur SSMA pour MySQL sur, assurez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] -vous que l’ordinateur remplit les conditions suivantes :

- [!INCLUDE[msCoName](../../includes/msconame_md.md)] Windows Installer 3,1 ou une version ultérieure.
- La [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort_md.md)] version 4.7.2 ou une version ultérieure. Vous pouvez l’obtenir à partir du [Centre de développement .NET Framework](https://go.microsoft.com/fwlink/?LinkId=48882).
- Le fournisseur client MySQL et la connectivité à la base de données MySQL que vous souhaitez migrer. Vous pouvez installer des fournisseurs à partir du média du produit MySQL ou du site Web MySQL.
- Le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser doit être en cours d’exécution pendant l’installation. Cette valeur est utilisée pour remplir une liste des instances de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans l’Assistant installation. Vous pouvez désactiver le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser après l’installation.  

  > [!NOTE]
  > Si le [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Service Browser est en cours d’exécution, mais que vous ne voyez toujours pas la liste des instances dans le programme d’installation, vous devez débloquer le port UDP 1434. Vous pouvez utiliser le pare-feu Windows pour débloquer temporairement le port, ou vous pouvez désactiver temporairement le pare-feu Windows. Vous devrez peut-être également désactiver temporairement le logiciel antivirus. Veillez à activer les pare-feu et les logiciels antivirus après l’installation.

### <a name="installing-the-extension-pack"></a>Installation du pack d’extension

Vous pouvez installer le pack d’extension à tout moment avant de migrer des données vers [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

> [!IMPORTANT]
> Pour installer le pack d’extension, vous devez être membre du rôle de serveur **sysadmin** sur l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .

Pour installer le pack d’extension :

1. Copiez SSMA pour **SSMAforMySQLExtensionPack_ *n*. msi**, où *n* est le numéro de build, sur l’ordinateur qui exécute [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .
2. Double-cliquez sur **SSMAforMySQLExtensionPack_ *n*. msi**.
3. Dans la boîte de dialogue **Bienvenue** , cliquez sur **suivant**.
4. Dans la boîte de dialogue **contrat de licence utilisateur final** , lisez le contrat de licence. Si vous acceptez, sélectionnez l’option **J’accepte le contrat** , puis cliquez sur **suivant**.
5. Dans la boîte de dialogue **choisir le type d’installation** , cliquez sur par **défaut**.
6. Dans la boîte de dialogue **prêt pour l’installation** , cliquez sur **installer**.
7. Dans la boîte **de dialogue la première étape de l’installation est terminée** , cliquez sur **suivant**.

   Une nouvelle boîte de dialogue s’affiche, dans laquelle vous sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour l’installation du pack d’extension.
  
8. Sélectionnez l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] dans laquelle vous allez migrer les schémas MySQL, puis cliquez sur **suivant**.
  
   L’instance par défaut porte le même nom que l’ordinateur. Les instances nommées sont suivies d’une barre oblique inverse et du nom de l’instance.

9. Sur la page connexion, sélectionnez la méthode d’authentification, puis cliquez sur **suivant**.
  
    L’authentification Windows utilise vos informations d’identification Windows pour essayer de se connecter à l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Si vous sélectionnez authentification du serveur, vous devez entrer un [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] nom de connexion et un mot de passe.

10. L’étape suivante vous oblige à définir le mot de passe d’une clé principale qui sera utilisée pour chiffrer les données sensibles stockées dans la base de données du pack d’extension lors de la migration des données côté serveur. Fournissez un mot de passe fort, puis cliquez sur **suivant**.

11. Dans la boîte de dialogue suivante, sélectionnez **installer la base de données des utilitaires *n* et installer les bibliothèques d’extensions**, où *n* est le numéro de version, puis cliquez sur **suivant**.

    La base de données **sysdb** est créée avec les tables et les procédures stockées requises pour la migration des données (à l’aide du moteur de migration de données côté serveur) sont créées dans cette base de données.

12. Pour installer les utilitaires sur une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] , sélectionnez **Oui**, puis cliquez sur **suivant**. Ou, pour quitter l’Assistant, cliquez sur **non**.

## <a name="see-also"></a>Voir également

- [Installation de SSMA pour MySQL client](../../ssma/mysql/installing-ssma-for-mysql-client-mysqltosql.md)
- [Migration de bases de données MySQL vers SQL Server Azure SQL Database](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)
