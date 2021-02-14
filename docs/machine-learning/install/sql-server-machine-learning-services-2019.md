---
title: Modifications de l’isolation pour Windows
description: Cet article décrit les modifications apportées au mécanisme d’isolation dans Machine Learning Services dans SQL Server 2019 sur Windows. Ces modifications affectent SQLRUserGroup, les règles de pare-feu, l’autorisation de fichier et l’authentification implicite.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 03/05/2020
ms.topic: how-to
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15'
ms.openlocfilehash: ace298ff0e69ef3e3ee964c15d894280d6ef1b99
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100082375"
---
# <a name="sql-server-2019-on-windows-isolation-changes-for-machine-learning-services"></a>SQL Server 2019 sur Windows : Modifications de l’isolation dans Machine Learning Services
[!INCLUDE [SQL Server 2019 - Windows only](../../includes/applies-to-version/sqlserver2019-windows-only.md)]

Cet article décrit les modifications apportées au mécanisme d’isolation dans Machine Learning Services dans SQL Server 2019 sur Windows. Ces modifications affectent **SQLRUserGroup**, les règles de pare-feu, l’autorisation de fichier et l’authentification implicite.

Pour plus d’informations, découvrez comment installer [SQL Server Machine Learning Services sur Windows](sql-machine-learning-services-windows-install.md).

## <a name="changes-to-isolation-mechanism"></a>Modifications apportées au mécanisme d’isolation

Sur Windows, le programme d’installation de SQL Server 2019 modifie le mécanisme d’isolation pour les processus externes. Cette modification remplace les comptes professionnels locaux par des éléments [AppContainers](/windows/desktop/secauthz/appcontainer-isolation), une technologie d’isolation destinée aux applications clientes s’exécutant sur Windows. 

Aucune action spécifique n’est requise de la part de l’administrateur suite à la modification. Sur un serveur nouveau ou mis à niveau, tous les scripts externes et le code exécuté à partir de [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) respectent automatiquement le nouveau modèle d’isolation. 

En résumé, les principales différences dans cette version sont les suivantes :

+ Les comptes d’utilisateurs locaux sous **Groupe d’utilisateurs restreints SQL (SQLRUserGroup)** ne sont plus créés ou utilisés pour exécuter des processus externes. Les éléments AppContainers les remplacent.
+ L’appartenance à **SQLRUserGroup** a changé. Au lieu de plusieurs comptes d’utilisateur locaux, l’appartenance se compose uniquement du compte de service SQL Server Launchpad. Les processus R et Python s’exécutent désormais sous l’identité du service Launchpad, isolée par le biais des AppContainers.

Bien que le modèle d’isolation ait changé, l’assistant d’installation et les paramètres de ligne de commande restent les mêmes dans SQL Server 2019. Pour obtenir de l’aide pour l’installation, consultez [Installer SQL Server Machine Learning Services](sql-machine-learning-services-windows-install.md).

## <a name="about-appcontainer-isolation"></a>À propos de l’isolation AppContainer

Dans les versions précédentes, **SQLRUserGroup** contenait un pool de comptes d’utilisateurs Windows locaux (MSSQLSERVER00-MSSQLSERVER20) utilisés pour isoler et exécuter les processus externes. Lorsqu’un processus externe était nécessaire, le service SQL Server Launchpad choisissait un compte disponible et l’utilisait pour exécuter un processus. 

Dans SQL Server 2019, le programme d’installation ne crée plus de comptes professionnels locaux. Au lieu de cela, l’isolation est obtenue via [AppContainers](/windows/desktop/secauthz/appcontainer-isolation). Au moment de l’exécution, lorsqu’un script incorporé ou du code est détecté dans une procédure stockée ou une requête, SQL Server appelle Launchpad avec une demande de lanceur spécifique à l’extension. Launchpad appelle l’environnement de runtime approprié dans un processus sous son identité et instancie un élément AppContainer pour l’héberger. Cette modification est utile, car la gestion des comptes et des mots de passe locaux n’est plus nécessaire. En outre, sur les installations où les comptes d’utilisateurs locaux sont interdits, la suppression de la dépendance de compte d’utilisateur local signifie que vous pouvez désormais utiliser cette fonctionnalité.

Tels qu’implémentés par SQL Server, les éléments AppContainers constituent un mécanisme interne. Bien que vous ne puissiez pas voir physiquement les éléments AppContainers dans Process Monitor, vous pouvez les retrouver dans les règles de pare-feu sortantes créées par le programme d’installation pour empêcher les processus d’effectuer des appels réseau.

## <a name="firewall-rules-created-by-setup"></a>Règles de pare-feu créées par le programme d’installation

Par défaut, SQL Server désactive les connexions sortantes en créant des règles de pare-feu. Par le passé, ces règles étaient basées sur des comptes d’utilisateur locaux : le programme d’installation créait une règle de trafic sortant pour **SQLRUserGroup** qui refusait l’accès réseau à ses membres (chaque compte professionnel était répertorié comme un principe local soumis à la règle). 

Dans le cadre de la migration vers les éléments AppContainers, il existe de nouvelles règles de pare-feu basées sur les SID AppContainer : une pour chacun des 20 éléments AppContainers créés par le programme d’installation SQL Server. Les conventions d’affectation de noms pour le nom de la règle de pare-feu sont **Bloquer l’accès réseau pour AppContainer-00 dans l’instance SQL Server MSSQLSERVER**, où 00 est le numéro de l’élément AppContainer (00-20 par défaut) et MSSQLSERVER est le nom de l’instance SQL Server. 

> [!Note]
> Si des appels réseau sont nécessaires, vous pouvez désactiver les règles de trafic sortant dans le pare-feu Windows.

<a name="file-permissions"></a>

## <a name="file-permissions"></a>Autorisations de fichier

Par défaut, les scripts Python et R externes n’ont qu’une autorisation d’accès en lecture à leurs répertoires de travail. 

Si vos scripts Python ou R ont besoin d’accéder à un autre répertoire, vous devez attribuer des autorisations de **Lecture et exécution** et/ou d’**Écriture** au compte d’utilisateur du service **NT Service\MSSQLLaunchpad** et à **TOUS LES PACKAGES d’APPLICATION** de ce répertoire.

Suivez les étapes ci-dessous pour accorder l’accès.

1. Dans l’Explorateur de fichiers, cliquez avec le bouton droit sur le dossier que vous souhaitez utiliser comme répertoire de travail, puis sélectionnez **Propriétés**.
1. Sélectionnez **Sécurité**, puis cliquez sur **Modifier...** pour changer les autorisations.
1. Cliquez sur **Ajouter...**
1. Vérifiez que le champ **À partir de cet emplacement** contient le nom de l’ordinateur local.
1. Entrez **TOUS LES PACKAGES d’APPLICATION** dans **Entrez les noms des objets à sélectionner**, puis cliquez sur **Vérifier les noms**. Cliquez sur **OK**.
1. Sélectionnez **Lecture et exécution** sous la colonne **Autoriser**.
1. Sélectionnez **Écriture** sous la colonne **Autoriser**, si vous souhaitez accorder des autorisations d’accès en écriture.
1. Cliquez sur **OK**, puis de nouveau sur **OK**.

### <a name="program-file-permissions"></a>Autorisations d’accès aux fichiers de programme

Comme pour les versions précédentes, le groupe **SQLRUserGroup** continue de fournir des autorisations de lecture et d’exécution sur les exécutables dans les répertoires SQL Server **Binn**, **R_SERVICES** et **PYTHON_SERVICES**. Dans cette version, le seul membre de **SQLRUserGroup** est le compte de service SQL Server Launchpad.  Lorsque le service Launchpad démarre un environnement d’exécution R ou Python, le processus s’exécute en tant que service LaunchPad.

## <a name="implied-authentication"></a>Authentification implicite

Comme précédemment, une configuration supplémentaire est toujours nécessaire pour *l’authentification implicite* dans les cas où un script ou du code doit se reconnecter à SQL Server via une authentification approuvée pour récupérer des données ou des ressources. La configuration supplémentaire implique la création d’une connexion de base de données pour **SQLRUserGroup**, dont le seul membre est désormais le seul compte de service SQL Server Launchpad alors qu’il y avait auparavant plusieurs comptes professionnels. Pour plus d’informations sur cette tâche, consultez [Ajouter SQLRUserGroup comme utilisateur de base de données](../security/create-a-login-for-sqlrusergroup.md).


## <a name="symbolic-link-created-by-setup"></a>Lien symbolique créé par le programme d’installation

Un lien symbolique est créé vers les éléments **R_SERVICES** et **PYTHON_SERVICES** actuels par défaut dans le cadre de l’installation de SQL Server. Si vous ne souhaitez pas créer ce lien, une autre solution consiste à accorder l’autorisation de lecture « Tous les packages d’application » à la hiérarchie menant au dossier.


## <a name="see-also"></a>Voir aussi

+ [Installer SQL Server Machine Learning Services sur Windows](sql-machine-learning-services-windows-install.md)
+ [Installer SQL Server Machine Learning Services sur Linux](../../linux/sql-server-linux-setup-machine-learning.md)