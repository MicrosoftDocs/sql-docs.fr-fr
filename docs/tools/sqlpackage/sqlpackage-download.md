---
title: Télécharger et installer sqlpackage
description: Télécharger et installer sqlpackage pour Windows, macOS ou Linux
ms.custom: tools|sos
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan; sstein
ms.date: 03/10/2021
ms.openlocfilehash: 2077800356a065ae4650cf6ab97ea45c3c537b8c
ms.sourcegitcommit: 81ee3cd57526d255de93afb84186074a3fb9885f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102622765"
---
# <a name="download-and-install-sqlpackage"></a>Télécharger et installer sqlpackage

sqlpackage s’exécute sur Windows, macOS et Linux.

Téléchargez et installez la dernière version de .NET Framework et les versions préliminaires de macOS et de Linux :

|Plateforme|Téléchargement|Date de publication|Version|Build
|:---|:---|:---|:---|:---|
|[Windows](#get-sqlpackage-for-windows)|[Programme d’installation MSI](https://go.microsoft.com/fwlink/?linkid=2157201)|10 mars 2021| 18,7 | 15.0.5084.2 |
|[macOS .NET Core](#get-sqlpackage-net-core-for-macos) |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157203)|10 mars 2021| 18,7| 15.0.5084.2 |
|[Linux .NET Core](#get-sqlpackage-net-core-for-linux) |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157202)|10 mars 2021| 18,7| 15.0.5084.2 |
|[Windows .NET Core](#get-sqlpackage-net-core-for-windows) |[Fichier zip](https://go.microsoft.com/fwlink/?linkid=2157302)|10 mars 2021| 18,7| 15.0.5084.2 |

Pour plus d’informations sur la dernière version, consultez les [notes de publication](release-notes-sqlpackage.md). Pour télécharger des langues supplémentaires, consultez la section [Langues disponibles](#available-languages).


Un lien persistant ([https://aka.ms/sqlpackage-linux](https://aka.ms/sqlpackage-linux)) est disponible et pointe vers la version actuelle de sqlpackage pour [.NET Core Linux](#get-sqlpackage-net-core-for-linux), qui peut être utilisé dans l’automatisation des environnements avec le sqlpackage le plus récent.

## <a name="dacfx"></a>DacFx
SqlPackage est une interface de ligne de commande pour l’infrastructure DacFx, exposant certaines API DacFx publiques. DacServices ([Microsoft. SqlServer. DAC](/dotnet/api/microsoft.sqlserver.dac.dacservices)) est un mécanisme associé pour intégrer le déploiement de base de données à votre pipeline d’application.  L’API DacServices est disponible dans un package via NuGet,[Microsoft.SqlServer.DACFx](https://www.NuGet.org/packages/Microsoft.SqlServer.DACFx).  La version actuelle de DacFx est 150.4897.1.

L’installation du package NuGet via l’interface CLI .NET s’effectue à l’aide de cette commande :

```cmd
dotnet add package Microsoft.SqlServer.DACFx
```

>[!NOTE]
> Des packages NuGet supplémentaires ont été publiés sous le nom DacFx, « Microsoft.SqlServer.DacFx.x64 » et « Microsoft.SqlServer.DacFx.x86 ». La prise en charge des deux plateformes est traitée avec le package « Microsoft.SqlServer.DACFx ». De nouvelles références doivent être apportées à ce package, et pas aux variantes x64 ou x86.

## <a name="get-sqlpackage-for-windows"></a>Obtenir sqlpackage pour Windows

Cette version de sqlpackage comprend une expérience du programme d’installation Windows standard et un fichier .zip : 

1. Téléchargez et exécutez le [programme d’installation DacFramework.msi pour Windows](https://go.microsoft.com/fwlink/?linkid=2157201).
2. Ouvrez une nouvelle fenêtre d’invite de commandes et exécutez sqlpackage.exe
    - sqlpackage est installé dans le dossier ```C:\Program Files\Microsoft SQL Server\150\DAC\bin```

## <a name="get-sqlpackage-net-core-for-windows"></a>Obtenir le sqlpackage .NET Core pour Windows

1. Télécharger [sqlpackage pour Windows](https://go.microsoft.com/fwlink/?linkid=2157302).
2. Pour extraire le fichier, cliquez avec le bouton droit sur le fichier dans l’Explorateur Windows, puis sélectionnez l’option « Extraire tout... » et sélectionnez le répertoire cible.
3. Ouvrez une nouvelle fenêtre de terminal et exécutez une commande cd sur l’emplacement où sqlpackage a été extrait :

   ```cmd
   > sqlpackage
   ```

## <a name="get-sqlpackage-net-core-for-macos"></a>Obtenir le sqlpackage .NET Core pour macOS

1. Téléchargez [sqlpackage pour macOS](https://go.microsoft.com/fwlink/?linkid=2157203).
2. Pour extraire le fichier et lancer sqlpackage, ouvrez une nouvelle fenêtre de Terminal et tapez les commandes suivantes :

   ```bash
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-osx-<version string>.zip -d ~/sqlpackage 
   $ echo 'export PATH="$PATH:~/sqlpackage"' >> ~/.bash_profile
   $ source ~/.bash_profile
   $ sqlpackage
   ```

   > [!NOTE]
   > Les paramètres de sécurité peuvent nécessiter une modification pour exécuter sqlpackage sur macOS. Utilisez les commandes suivantes pour interagir avec Gatekeeper à partir de la ligne de commande.

   **Avant d’exécuter sqlpackage :**
   ```bash
   $ sudo spctl --master-disable
   ```

   **Après avoir exécuté sqlpackage :**
   ```bash
   $ sudo spctl --master-enable
   ```

## <a name="get-sqlpackage-net-core-for-linux"></a>Obtenir le sqlpackage .NET Core pour Linux

1. Téléchargez [sqlpackage pour Linux](https://go.microsoft.com/fwlink/?linkid=2157202) en utilisant un des programmes d’installation ou l’archive tar.gz.
2. Pour extraire le fichier et lancer sqlpackage, ouvrez une nouvelle fenêtre de Terminal et tapez les commandes suivantes :

   ```bash
   $ cd ~
   $ mkdir sqlpackage
   $ unzip ~/Downloads/sqlpackage-linux-<version string>.zip -d ~/sqlpackage 
   $ echo "export PATH=\"\$PATH:$HOME/sqlpackage\"" >> ~/.bashrc
   $ chmod a+x ~/sqlpackage/sqlpackage
   $ source ~/.bashrc
   $ sqlpackage
   ```

   > [!NOTE]
   > Sur Debian, Red Hat et Ubuntu, vous pouvez avoir des dépendances manquantes. Utilisez les commandes suivantes pour installer ces dépendances selon votre version de Linux :

   **Debian :**

   ```bash
   $ sudo apt-get install libunwind8
   ```

   **Redhat :**

   ```bash
   $ yum install libunwind
   $ yum install libicu
   ```

   **Ubuntu :**

   ```bash
   $ sudo apt-get install libunwind8

   # install the libicu library based on the Ubuntu version
   $ sudo apt-get install libicu52      # for 14.x
   $ sudo apt-get install libicu55      # for 16.x
   $ sudo apt-get install libicu57      # for 17.x
   $ sudo apt-get install libicu60      # for 18.x
   $ sudo apt-get install libicu60      # for 20.x
   ```

## <a name="uninstall-sqlpackage"></a>Désinstaller sqlpackage

Si vous avez installé sqlpackage à l’aide du programme d’installation Windows, alors désinstallez-le de la même manière que n’importe quelle application Windows.

Si vous avez installé sqlpackage avec un fichier .zip ou une autre archive, supprimez les fichiers.

## <a name="supported-operating-systems"></a>Systèmes d’exploitation pris en charge

SqlPackage s’exécute sur Windows, macOS et Linux et est construit à l’aide de .NET Core 3.1.  Les [exigences pour le système d’exploitation .NET Core 3.1](https://github.com/dotnet/core/blob/master/release-notes/3.1/3.1-supported-os.md) s’appliquent à sqlpackage.

### <a name="windows-x64"></a>Windows (x64)

- Windows 10 (1607+)
- Windows 8.1
- Windows 7 SP1
- Ordinateur Windows Server principal
- Windows Server 2012 R2
- Windows Server 2016
- Windows Server 2019

### <a name="macos"></a>macOS

- macOS 10.15 « Catalina »
- macOS 10.14 « Mojave »
- macOS 10.13 « High Sierra »

### <a name="linux-x64"></a>Linux (x64)

- Red Hat Enterprise Linux 7+
- SUSE Linux Enterprise Server v12 SP2+
- Ubuntu 16.04, 18.04, 20.04

## <a name="available-languages"></a>Langues disponibles

Vous pouvez installer cette version de sqlpackage dans les langues suivantes :

sqlpackage Windows :  
[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2157201&clcid=0x40a)

sqlpackage .NET Core Windows :  
[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2157302&clcid=0x40a)

sqlpackage .NET Core macOS :  
[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2157203&clcid=0x40a)

sqlpackage .NET Core Linux :  
[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2157202&clcid=0x40a)


## <a name="next-steps"></a>Étapes suivantes

- En savoir plus sur [sqlpackage](sqlpackage.md)

[Déclaration de confidentialité Microsoft](https://go.microsoft.com/fwlink/?LinkId=521839)