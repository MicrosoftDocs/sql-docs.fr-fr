---
description: Obtenir des données de diagnostic après un incident de SQL Server Management Studio (SSMS)
title: Résolution d’un problème dans lequel un système cesse de répondre ou se bloque avec SSMS
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
ms.assetid: c28ffa44-7b8b-4efa-b755-c7a3b1c11ce4
author: markingmyname
ms.author: maghan
ms.reviewer: drskwier, sstein
ms.custom: seo-lt-2019
ms.date: 09/18/2019
ms.openlocfilehash: 5e4b7e4926265a1d6358b95bba4fae7b63ae016f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066054"
---
# <a name="get-diagnostic-data-after-a-sql-server-management-studio-ssms-crash"></a>Obtenir des données de diagnostic après un incident de SQL Server Management Studio (SSMS)

[!INCLUDE[Applies to](../../includes/appliesto-ss-asdb-asdw-xxx-md.md)]

## <a name="get-full-memory-dump-after-an-unresponsive-system-or-crash"></a>Obtenir une image mémoire complète lorsqu’un système cesse de répondre ou se bloque

Obtenez une image mémoire complète de SQL Server Management Studio (SSMS) lorsque le système cesse de répondre ou se bloque.

Pour capturer des informations de diagnostic pour détecter un problème dans lequel SSMS cesse de répondre ou se bloque, suivez les étapes ci-dessous.

1. Télécharger [ProcDump](/sysinternals/downloads/procdump).

2. Décompressez le téléchargement dans un dossier.

3. Ouvrez une invite de commandes et exécutez la commande suivante.

    ```console
    <PathToProcDumpFolder>\procdump.exe -e -h -ma -w ssms.exe
    ```

    S’il vous invite à accepter un contrat de licence, sélectionnez *Accepter*.

4. Démarrez SSMS, s’il n’a pas déjà démarré.

5. Reproduisez le problème.

6. Le texte doit apparaître dans l’invite de commandes sur l’écriture du fichier de sauvegarde, attendez la fin.

7. Créez un nouveau dossier et copiez le fichier *.dmp qui est écrit dans ce dossier.

8. Copiez les fichiers suivants dans le même dossier.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Compressez le dossier.

## <a name="get-full-memory-dump-for-an-outofmemoryexception"></a>Obtenir une image mémoire complète pour une OutOfMemoryException

Obtenez une image mémoire complète de SSMS quand il lève une OutOfMemoryException.

Vous pouvez obtenir une image mémoire complète avec n’importe quelle exception managée.

Pour capturer des informations de diagnostic afin de résoudre une OutOfMemoryException à partir du SSMS, suivez les étapes ci-dessous.

1. Télécharger [ProcDump](/sysinternals/downloads/procdump).

2. Décompressez le téléchargement dans un dossier.

3. Ouvrez une invite de commandes et exécutez la commande suivante.

    ```console
    <PathToProcDumpFolder>\procdump.exe -e 1 -f System.OutOfMemoryException -ma -w ssms.exe
    ```

    S’il vous invite à accepter un contrat de licence, sélectionnez *Accepter*.

4. Démarrez SSMS (SQL Server Management Studio) s’il n’est pas déjà démarré.

5. Reproduisez le problème.

6. Le texte doit apparaître dans l’invite de commandes sur l’écriture du fichier de sauvegarde, attendez la fin.

7. Créez un nouveau dossier et copiez le fichier *.dmp qui est écrit dans ce dossier.

8. Copiez les fichiers suivants dans le même dossier.

    "C:\Windows\Microsoft.NET\Framework\v4.0.30319\mscordacwks.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\SOS.dll"  "C:\Windows\Microsoft.NET\Framework\v4.0.30319\clr.dll"

9. Compresser le dossier.