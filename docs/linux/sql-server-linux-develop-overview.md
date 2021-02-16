---
title: Développer des applications pour SQL Server sur Linux
description: Vous pouvez créer des applications qui se connectent à et utilisent SQL Server sur Linux à partir de différents langages de programmation et de frameworks web populaires.
author: VanMSFT
ms.author: vanto
ms.date: 11/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 758cb738-b018-465b-9ab0-59a24b892e66
ms.openlocfilehash: 01655b948afa721365b7ea7adebd1e22f8f6f851
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100273080"
---
# <a name="how-to-get-started-developing-applications-for-sql-server-on-linux"></a>Comment commencer à développer des applications pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous pouvez créer des applications qui se connectent à et utilisent SQL Server sur Linux à partir de différents langages de programmation tels que C#, Java, Node.js, PHP, Python, Ruby et C++. Vous pouvez également utiliser des frameworks web populaires et des frameworks ORM (Object Relational Mapping).

> [!VIDEO https://channel9.msdn.com/events/Connect/2017/T153/player]

> [!TIP]
> Ces mêmes options de développement vous permettent également de cibler SQL Server sur d’autres plateformes. Les applications peuvent cibler SQL Server s’exécutant en local ou dans le cloud, sur Linux, Windows ou Docker sur macOS. Vous pouvez également cibler Azure SQL Database et Azure Synapse Analytics.

## <a name="try-the-tutorials"></a>Essayer les tutoriels

La meilleure façon de commencer et de créer des applications avec SQL Server est d’essayer par vous-même.

- Accédez à [Prise en main des tutoriels](https://aka.ms/sqldev).
- Sélectionnez votre plateforme de langage et de développement.
- Essayez les exemples de code.

> [!TIP]
> Si vous souhaitez développer pour SQL Server sur Docker, jetez un coup d’œil aux tutoriels **macOS**.

## <a name="create-new-applications"></a>Créez des applications

Si vous créez une application, jetez un coup d’œil à la liste des [bibliothèques de connectivité](sql-server-linux-develop-connectivity-libraries.md) pour obtenir un résumé des connecteurs et des frameworks populaires disponibles pour différents langages de programmation.

## <a name="use-existing-applications"></a>Utilisez les applications existantes

Si vous avez une application de base de données existante, vous pouvez simplement modifier sa chaîne de connexion pour cibler SQL Server sur Linux. Veillez à consulter les [problèmes connus](sql-server-linux-release-notes.md) dans SQL Server sur Linux.

## <a name="use-existing-sql-tools-on-windows-with-sql-server-on-linux"></a>Utilisez les outils SQL existants sur Windows avec SQL Server sur Linux

Les outils qui s’exécutent actuellement sur Windows, tels que SSMS, SSDT et PowerShell, fonctionnent également avec SQL Server sur Linux. Bien qu’ils ne s’exécutent pas en mode natif sur Linux, vous pouvez toujours gérer les instances SQL Server distantes sur Linux. 

Pour plus d'informations, consultez les rubriques ci-dessous :

- [SQL Server Management Studio (SSMS)](sql-server-linux-manage-ssms.md)
- [Outils SQL Server Data Tools (SSDT)](sql-server-linux-develop-use-ssdt.md)
- [SQL PowerShell](sql-server-linux-manage-powershell.md)

> [!Note]
> Veillez à utiliser les dernières versions de ces outils pour une expérience optimale.

## <a name="use-new-sql-tools-for-linux"></a>Utilisez les nouveaux outils SQL pour Linux

Vous pouvez utiliser la nouvelle [extension mssql](https://aka.ms/mssql-marketplace) pour [Visual Studio Code](https://code.visualstudio.com) sur Linux, macOS et Windows. Pour une procédure pas à pas, consultez le tutoriel suivant :

- [Utiliser Visual Studio Code](../tools/visual-studio-code/sql-server-develop-use-vscode.md)

Vous pouvez également utiliser de nouveaux outils en ligne de commande qui sont natifs pour Linux. Ces outils incluent les éléments suivants :

- [sqlcmd](../tools/sqlcmd-utility.md)
- [bcp](sql-server-linux-migrate-bcp.md)
- [mssql-conf](sql-server-linux-configure-mssql-conf.md)

## <a name="next-steps"></a>Étapes suivantes

Pour commencer, installez SQL Server sur Linux à l’aide de l’un des outils de démarrage rapide suivants :

- [Installer sur Red Hat Enterprise Linux](quickstart-install-connect-red-hat.md)
- [Installer sur SUSE Linux Enterprise Server](quickstart-install-connect-suse.md)
- [Installer sur Ubuntu](quickstart-install-connect-ubuntu.md)
- [Exécuter sur Docker](quickstart-install-connect-ubuntu.md)