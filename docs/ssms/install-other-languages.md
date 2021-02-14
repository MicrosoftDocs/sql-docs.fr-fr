---
title: Installer des versions dans d’autres langues que l’anglais
description: Installez des versions de SQL Server Management Studio (SSMS) dans d’autres langues que l’anglais. Cet article s’applique à SSMS 17.x.
ms.prod: sql
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.custom: seo-lt-2019
ms.date: 04/25/2019
ms.openlocfilehash: 8a70ef94e5491a807414abbd69b1f8ef71b9ca7d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064894"
---
# <a name="install-non-english-language-versions-of-sql-server-management-studio-ssms"></a>Installer des versions de SQL Server Management Studio (SSMS) dans d’autres langues que l’anglais

SSMS est disponible dans plusieurs langues, mais le programme d’installation de SSMS bloque l’installation sur les ordinateurs dont les paramètres régionaux système ne correspondent pas à la langue de SSMS.

> [!NOTE]
> Cet article s’applique à SSMS 17.x. Pour SSMS 18.x, le bloc sur le paramétrage des langues multiples a été supprimé et vous pouvez maintenant, par exemple, installer SSMS en allemand sur une version française de Windows. Si la langue du système d’exploitation ne correspond pas à la langue de SSMS, modifiez la langue sous **Outils** > **Options** > **Paramètres internationaux**, sinon SSMS affiche l’interface utilisateur en anglais.

Les instructions suivantes dépendent de la version de Windows. Celles qui sont présentées ici valent pour Windows 10.

## <a name="install-non-english-ssms-on-a-computer-running-an-english-operating-system-os"></a>Installer SSMS dans une autre langue que l’anglais sur un ordinateur doté d’un système d’exploitation (OS) en anglais

1. Installez le module linguistique de Windows associé à la langue que SSMS devra utiliser :
   - **Paramètres** > **Heure et langue** > **Région et langue** > **Ajouter une langue**
2. Définissez maintenant les paramètres régionaux système afin d’utiliser le module linguistique installé à l’étape précédente en cliquant sur la langue que vous venez d’installer, puis sélectionnez **Définir comme valeur par défaut**. (Après l’installation de SSMS, vous pourrez sélectionner à nouveau l’anglais dans les paramètres régionaux système.)
3. Lorsque votre système d’exploitation fonctionne dans la langue souhaitée, installez la langue SSMS que vous souhaitez. La première fois que vous installez une nouvelle langue SSMS, utilisez le package complet. Vous pourrez utiliser le package de mise à niveau pour les installations suivantes.
4. Exécutez SSMS ; il devrait s’afficher dans la langue installée à l’étape précédente.
5. Sélectionnez à nouveau l’anglais dans les paramètres régionaux système de votre ordinateur.

## <a name="install-ssms-in-a-language-other-than-the-language-of-the-installed-os"></a>Installer SSMS dans une langue autre que celle du système d’exploitation installé

1. Installez le module linguistique de Windows associé à la langue que SSMS devra utiliser :
   - **Paramètres** > **Heure et langue** > **Région et langue** > **Ajouter une langue**
2. Définissez maintenant les paramètres régionaux système afin d’utiliser le module linguistique installé à l’étape précédente en cliquant sur la langue que vous venez d’installer, puis sélectionnez **Définir comme valeur par défaut**.
3. Lorsque votre système d’exploitation fonctionne dans la langue souhaitée, installez la langue SSMS que vous souhaitez. La première fois que vous installez une nouvelle langue SSMS, utilisez le package complet. Vous pourrez utiliser le package de mise à niveau pour les installations suivantes.
4. Pour chaque langue à installer qui ne correspond pas à la langue de la première version de SSMS installée, installez le module linguistique Visual Studio 2015 Shell (isolé) correspondant :
   - Accédez à [https://connect.microsoft.com/VisualStudio/ExtendVS](https://connect.microsoft.com/VisualStudio/ExtendVS) (vous devrez peut-être vous connecter et effectuer la procédure de *connexion d’inscription*).
   - Téléchargez le module linguistique Visual Studio 2015 Shell (isolé) souhaité, et installez-le.

   > [!IMPORTANT]
   > Suivez les étapes précédentes pour installer le module linguistique Visual Studio 2015 Shell isolé ; n’utilisez pas le lien **Obtenir des langues supplémentaires** qui se trouve sur **Outils** | **Options** | **Paramètres internationaux**.

5. Exécutez SSMS et sélectionnez la langue que vous souhaitez utiliser dans :
   - **Outils** | **Options** | **Paramètres internationaux**
6. Fermez et redémarrez SSMS.

## <a name="next-steps"></a>Étapes suivantes

- [Tutoriel : SQL Server Management Studio](./quickstarts/ssms-connect-query-sql-server.md)