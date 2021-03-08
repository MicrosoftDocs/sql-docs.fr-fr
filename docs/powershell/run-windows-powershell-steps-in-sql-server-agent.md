---
title: Utiliser Windows PowerShell dans les étapes de travail de l'Agent SQL Server
description: Découvrez comment exécuter des étapes Windows PowerShell dans un travail de l’Agent SQL Server.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: seo-lt-2019
ms.date: 03/16/2017
ms.openlocfilehash: 8029d18dee3dd49342c3c029fc78992900394ed4
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101839399"
---
# <a name="run-windows-powershell-steps-in-sql-server-agent"></a>Utiliser Windows PowerShell dans les étapes de travail de l'Agent SQL Server

[!INCLUDE[appliesto-ss-xxxx-asdw-pdw-md](../includes/appliesto-ss-xxxx-asdw-pdw-md.md)]

Utilisez SQL Server Agent pour exécuter des scripts SQL Server PowerShell à des heures planifiées.

[!INCLUDE[sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

[!INCLUDE[sql-server-powershell-no-sqlps](../includes/sql-server-powershell-no-sqlps.md)]

## <a name="to-run-powershell-from-sql-server-agent"></a>Pour exécuter PowerShell à partir de SQL Server Agent

Il y a plusieurs types d'étapes de travail SQL Server Agent. Chaque type est associé à un sous-système qui implémente un environnement spécifique, tel qu'un agent de réplication ou un environnement d'invite de commandes. Vous pouvez coder les scripts Windows PowerShell, puis utiliser SQL Server Agent pour les inclure dans les travaux qui s'exécutent à des heures planifiées ou en réponse à des événements SQL Server. Les scripts Windows PowerShell peuvent être exécutés à l'aide d'une étape de travail d'invite de commandes ou d'une étape de travail PowerShell.

- Utilisez une étape du travail PowerShell pour que le sous-système SQL Server Agent exécute l’utilitaire **sqlps**, qui lance PowerShell et importe le module **sqlps**. Si vous exécutez SQL Server 2019 ou une version ultérieure, nous vous recommandons d’utiliser le module **[SqlServer](sql-server-powershell.md#sql-server-agent)** dans votre étape de travail SQL Agent.

- Utilisez une étape du travail d’invite de commandes pour exécuter PowerShell.exe et spécifiez un script qui importe le module **sqlps** .

### <a name="caution-about-memory-consumption"></a><a name="LimitationsRestrictions"></a> Avertissement concernant la consommation de mémoire

Chaque étape de travail SQL Server Agent qui exécute PowerShell avec le module **sqlps** lance un processus qui consomme environ **20 Mo** de mémoire. L'exécution simultanée d'un grand nombre d'étapes de travail Windows PowerShell peut nuire aux performances.

## <a name="create-a-powershell-job-step"></a><a name="PShellJob"></a> Créer une étape de travail PowerShell

### <a name="to-create-a-powershell-job-step"></a>Pour créer une étape de travail PowerShell

1. Développez **SQL Server Agent**, créez un nouveau travail ou cliquez avec le bouton de droite sur un travail existant, puis cliquez sur **Propriétés**. Pour plus d'informations sur la création d'un travail, consultez [Création de travaux](../ssms/agent/create-jobs.md).

2. Dans la boîte de dialogue **Propriétés du travail**, sélectionnez la page **Étapes**, puis **Nouveau**.

3. Dans la boîte de dialogue **Nouvelle étape du travail** , tapez un **nom d'étape** de travail.

4. Dans la liste **Type**, sélectionnez **PowerShell**.

5. Dans la liste **Exécuter en tant que** , sélectionnez le compte proxy avec les informations d'identification que le travail utilisera.

6. Dans la zone **Commande** , tapez la syntaxe du script PowerShell qui sera exécuté pour l'étape de travail. Vous pouvez aussi sélectionner **Ouvrir** et sélectionner un fichier contenant la syntaxe du script.

7. Sélectionnez la page **Avancé** pour paramétrer les options suivantes pour l'étape de travail : l'action à exécuter si l'étape de travail échoue ou réussit, le nombre de tentatives d'exécution de l'étape de travail que doit effectuer SQL Server Agent et la fréquence de ces tentatives.

## <a name="create-a-command-prompt-job-step"></a><a name="CmdExecJob"></a> Créer une étape de travail d'invite de commandes

### <a name="to-create-a-cmdexec-job-step"></a>Pour créer une étape de travail CmdExec

1. Développez **SQL Server Agent**, créez un nouveau travail ou cliquez avec le bouton de droite sur un travail existant, puis cliquez sur **Propriétés**. Pour plus d'informations sur la création d'un travail, consultez [Création de travaux](../ssms/agent/create-jobs.md).

2. Dans la boîte de dialogue **Propriétés du travail**, sélectionnez la page **Étapes**, puis **Nouveau**.

3. Dans la boîte de dialogue **Nouvelle étape du travail** , tapez un **nom d'étape** de travail.

4. Dans la liste **Type** , choisissez **Système d’exploitation (CmdExec)** .

5. Dans la liste **Exécuter en tant que** , sélectionnez le compte proxy avec les informations d'identification que doit utiliser le travail. Par défaut, les étapes de travail CmdExec s'exécutent dans le contexte du compte de service SQL Server Agent.

6. Dans la zone **Traiter le code de sortie d'une commande réussie** , entrez une valeur comprise entre 0 et 999999.

7. Dans la zone **Commande** , entrez powershell.exe avec des paramètres spécifiant le script PowerShell à exécuter.

8. sélectionnez la page **Avancé** pour définir les options d'étape de travail, telles que l'action à exécuter lorsque l'étape de travail aboutit ou échoue, le nombre de tentatives d'exécution de l'étape de travail que doit effectuer SQL Server Agent et le fichier dans lequel SQL Server Agent peut écrire la sortie de l'étape de travail. Seuls les membres du rôle de serveur fixe **sysadmin** peuvent écrire une sortie d'étape de travail dans un fichier du système d'exploitation.

## <a name="see-also"></a>Voir aussi

- [SQL Server PowerShell](sql-server-powershell.md)
- [SQL Server Agent avec PowerShell](sql-server-powershell.md#sql-server-agent)