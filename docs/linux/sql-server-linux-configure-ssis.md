---
title: Configurer SSIS sur Linux avec ssis-conf
description: Cet article explique comment configurer des packages SQL Server Integration Services (SSIS) sur Linux avec le l’utilitaire ssi-conf.
author: lrtoyou1223
ms.author: lle
ms.reviewer: maghan
ms.date: 10/02/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.openlocfilehash: da546bddb3b7b45753b224539a546f96663704c5
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100348189"
---
# <a name="configure-sql-server-integration-services-on-linux-with-ssis-conf"></a>Configurer SQL Server Integration Services sur Linux avec ssis-conf

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Vous exécutez le script de configuration `ssis-conf` lorsque vous installez SQL Server Integration Services (SSIS) pour Red Hat Enterprise Linux et Ubuntu. Pour plus d’informations sur l’installation de SSIS, consultez [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md).

Vous pouvez également utiliser l'utilitaire `ssis-conf` pour configurer les propriétés suivantes :

| Commande | Description |
|-------------|---------------------------------------------------------------------|
| set-edition | Définir l’édition de SQL Server                                       |
| télémétrie   | Activer ou désactiver le service de télémétrie SQL Server Integration Services |
| configurer       | Initialiser et configurer Microsoft SQL Server Integration Services      |
|||

## <a name="run-ssis-conf"></a>Exécuter ssis-conf

Les exemples de cet article exécutent `ssis-conf` en spécifiant le chemin complet : `/opt/ssis/bin/ssis-conf`. Si vous accédez à cet emplacement avant d’exécuter `ssis-conf`, vous pouvez exécuter l’utilitaire dans le contexte du répertoire actif : `./ssis-conf`.

Veillez à exécuter les commandes décrites dans cet article avec des privilèges racine. Par exemple, exécutez `sudo /opt/ssis/bin/ssis-conf setup` et pas `/opt/ssis/bin/ssis-conf setup`.

Pour exécuter ces commandes avec des invites dans la langue de votre choix, vous pouvez spécifier des paramètres régionaux. Par exemple, pour recevoir des invites en chinois, exécutez la commande suivante : `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="use-set-edition-to-set-the-edition-of-sql-server-integration-services"></a>Utilisez set-edition pour définir l’édition de SQL Server Integration Services

L’édition de SSIS est alignée sur l’édition de SQL Server.

Entrez la commande suivante : `$ sudo /opt/ssis/bin/ssis-conf set-edition`.

Une fois que vous avez entré la commande, l’invite suivante s’affiche :

```
Choose an edition of SQL Server:

1) Evaluation (free, no production use rights, 180-day limit)

2) Developer (free, no production use rights)

3) Express (free)

4) Web (PAID)

5) Standard (PAID)

6) Enterprise (PAID)

7) Enterprise Core (PAID)

8) I bought a license through a retail sales channel and have a product key to enter.

Details about editions can be found at https://go.microsoft.com/fwlink/?LinkId=852748&clcid=0x409.

Use of PAID editions of this software requires separate licensing through a Microsoft Volume Licensing program.

By choosing a PAID edition, you are verifying that you have the appropriate number of licenses in place to install and run this software.

Enter your edition (1-8):
```

Si vous entrez une valeur comprise entre 1 et 7, le système configure une édition gratuite ou PAYANTE. Si vous entrez 8, l’utilitaire vous invite à entrer la clé du produit que vous avez acheté :

```
Enter the 25-character product key:
```

## <a name="use-telemetry-to-configure-customer-feedback"></a>Utiliser la télémétrie pour configurer les commentaires des clients

La commande `telemetry` détermine si SSIS envoie des commentaires à Microsoft.

Pour les éditions gratuites (c’est-à-dire les éditions Express, Développeur et Évaluation), le service de télémétrie est toujours activé. Si vous disposez d’une édition gratuite, vous ne pouvez pas utiliser la commande `telemetry` pour désactiver la télémétrie.

Entrez la commande suivante : `$ sudo /opt/ssis/bin/ssis-conf telemetry`.

Pour les éditions PAYANTES, une fois que vous avez entré la commande, l’invite suivante s’affiche :

```
Send feature usage data to Microsoft. Feature usage data includes information about your hardware configuration and how you use SQL Server Integration Services.

[Yes/No]:
```

Si vous sélectionnez **Oui**, le service de télémétrie est activé et commence à s’exécuter. Le service démarre automatiquement après chaque démarrage. Si vous sélectionnez **Non**, le service de télémétrie s’arrête et est désactivé.

## <a name="use-setup-to-initialize-and-set-up-microsoft-sql-server-integration-services"></a>Utiliser la configuration pour initialiser et configurer Microsoft SQL Server Integration Services

Utilisez la commande `setup` chaque fois que vous installez SSIS.

Entrez la commande suivante : `sudo /opt/ssis/bin/ssis-conf setup`.

L’utilitaire vous invite à accuser réception ou à fournir des valeurs pour les éléments suivants :
-   Licence du produit
-   Contrat CLUF
-   Service de télémétrie
-   La langue utilisée par Integration Services

Pour exécuter la commande `setup` avec des invites dans la langue de votre choix, vous pouvez spécifier des paramètres régionaux. Par exemple, pour recevoir des invites en chinois, exécutez la commande suivante : `sudo LC_ALL=zh_CN.UTF-8 /opt/ssis/bin/ssis-conf setup`.

## <a name="ssisconf-format"></a>format ssis.conf

Le fichier `/var/opt/ssis/ssis.conf` suivant fournit un exemple pour chaque paramètre.

Pour SQL Server, vous pouvez modifier les paramètres système en modifiant les valeurs dans le fichier `mssql.conf`. Pour SSIS, vous *ne pouvez pas* modifier les paramètres système en modifiant les valeurs dans le fichier `ssis.conf`. Le fichier `ssis.conf` affiche uniquement les résultats de la configuration. Si vous souhaitez modifier les paramètres de SSIS, vous pouvez supprimer le fichier `ssis.conf` et réexécuter la commande `setup`.

Voici un exemple de fichier `ssis.conf`. Chaque champ correspond au résultat d’une étape de configuration.

```
[LICENSE]
                       
registered = Y        
                       
pid = enterprisecore  
                       
[EULA]
                       
accepteula = Y        
                       
[TELEMETRY]
                       
enabled = Y           
                       
[language]
                       
lcid = 2052
```

## <a name="related-content-about-ssis-on-linux"></a>Contenu connexe relatif à SSIS sur Linux
-   [Extraire, transformer et charger des données sur Linux avec SSIS](sql-server-linux-migrate-ssis.md)
-   [Installer SQL Server Integration Services (SSIS) sur Linux](sql-server-linux-setup-ssis.md)
-   [Limitations et problèmes connus pour SSIS sur Linux](sql-server-linux-ssis-known-issues.md)
-   [Planifier l’exécution du package SQL Server Integration Services sur Linux avec cron](sql-server-linux-schedule-ssis-packages.md)
