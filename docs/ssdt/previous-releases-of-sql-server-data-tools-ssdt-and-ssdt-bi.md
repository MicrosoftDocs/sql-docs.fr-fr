---
title: Versions antérieures de SQL Server Data Tools (SSDT)
description: Découvrez quelles versions de SSDT et SSDT-BI fonctionnent avec les versions de Visual Studio. Consultez comment installer différentes versions de SSDT et SSDT-BI.
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: 5d32e301-0f44-4916-b0db-76e8322c0ab7
author: dzsquared
ms.author: drskwier
ms.reviewer: maghan
ms.custom: seo-lt-2019
ms.date: 06/17/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||=azuresqldb-mi-current'
ms.openlocfilehash: 3e2a3e607b561ede827652f61032b90fc7efa336
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100353989"
---
# <a name="previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi"></a>Versions antérieures de SQL Server Data Tools (SSDT et SSDT-BI)

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SQL Server Data Tools (SSDT) fournit des modèles de projet et des aires de conception permettant de créer des types de contenu SQL Server (bases de données relationnelles, modèles Analysis Services, rapports Reporting Services et packages Integration Services).

SSDT offre une compatibilité descendante, ce qui signifie que vous pouvez toujours utiliser [la version de SSDT la plus récente](download-sql-server-data-tools-ssdt.md) pour concevoir et déployer des bases de données, des modèles, des rapports et des packages qui s’exécutent sur des versions plus anciennes de SQL Server.

Historiquement, le shell Visual Studio utilisé pour créer des types de contenu SQL Server a été publié sous différents noms, notamment **SQL Server Data Tools**, **SQL Server Data Tools - Business Intelligence** et **Business Intelligence Development Studio**. Les versions précédentes étaient fournies avec des ensembles distincts de modèles de projet. Pour obtenir tous les modèles de projet regroupés dans un SSDT, vous devez disposer de [la version la plus récente](download-sql-server-data-tools-ssdt.md). Sinon, vous devez probablement installer plusieurs versions précédentes pour obtenir tous les modèles utilisés dans SQL Server. Un seul shell est installé par version de Visual Studio ; l’installation d’un deuxième SSDT ajoute simplement les modèles manquants.

## <a name="previous-ssdt-releases"></a>Versions précédentes de SSDT

Téléchargez les versions précédentes de SSDT en sélectionnant le lien de téléchargement de la section associée.

| Version de SSDT | Version Visual Studio |
|--------------|-----------------------|
| [15.8](#ssdt-for-visual-studio-vs-2017) | 2017 |
| [17.4](#ssdt-for-visual-studio-vs-2015) | 2015 |
| [16.5](#ssdt-for-visual-studio-vs-2013) | 2013 |
| [11.1.50727.1](#ssdt-for-visual-studio-vs-2012) | 2012 |

### <a name="ssdt-for-visual-studio-vs-2017"></a>SSDT pour Visual Studio (VS) 2017

**[Télécharger SSDT pour Visual Studio 2017 (15.8)](https://go.microsoft.com/fwlink/?linkid=2124319)**

Cette version de **SSDT pour Visual Studio 2017** peut être installée dans les langues suivantes :

[Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x804) | [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x404) | [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x409) | [Français](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40c) | [Allemand](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x407) | [Italien](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x410) | [Japonais](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x411) | [Coréen](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x412) | [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x416) | [Russe](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x419) | [Espagnol](https://go.microsoft.com/fwlink/?linkid=2124319&clcid=0x40a)

### <a name="ssdt-for-visual-studio-vs-2015"></a>SSDT pour Visual Studio (VS) 2015

Pour installer cette version de SSDT, vous devez télécharger une image ISO. Le fichier ISO est un fichier auto-extractible qui contient tous les composants nécessaires à SSDT et qui peut être téléchargé à l'aide d'un gestionnaire de téléchargement redémarrable, ce qui est utile en cas de bande passante réseau limitée ou peu fiable. Une fois téléchargée, l’image ISO peut être montée comme lecteur.

Étapes d'installation :

1. **[Télécharger SSDT pour Visual Studio 2015 (17.4)](https://go.microsoft.com/fwlink/?linkid=2132817)** .

2. Ouvrez l’image ISO.

3. Exécutez le fichier *SSDTSetup.exe*.

    ![Image ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Cette version de **SSDT pour Visual Studio 2015** peut être installée dans les langages suivants :

| Langage | Code de hachage SHA256 |
|----------|-------------|
| [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Français](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Allemand](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italien](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japonais](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coréen](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russe](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Espagnol](https://go.microsoft.com/fwlink/?linkid=2132817&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2013"></a>SSDT pour Visual Studio (VS) 2013

Pour installer cette version de SSDT, vous devez télécharger une image ISO. Le fichier ISO est un fichier auto-extractible qui contient tous les composants nécessaires à SSDT et qui peut être téléchargé à l'aide d'un gestionnaire de téléchargement redémarrable, ce qui est utile en cas de bande passante réseau limitée ou peu fiable. Une fois téléchargée, l’image ISO peut être montée comme lecteur.

Étapes d'installation :

1. **[Télécharger SSDT pour Visual Studio 2013 (16.5)](https://go.microsoft.com/fwlink/?linkid=832312)**

2. Ouvrez l’image ISO.

3. Exécutez le fichier *SSDTSetup.exe*.

    ![Image ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Cette version de **SSDT pour Visual Studio 2013** peut être installée dans les langages suivants :

| Langage | Code de hachage SHA256 |
|----------|-------------|
| [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x804) | 79A958B122DDEC2857F1F4B9F0272A6BD2106BB17B4DA94CC68CACFCDDC16EAE |
| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x404) | 8F9661883F2D2D28D6928AE66242DB465DBB25696EDFE0348D222421CEAB000A |
| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x409) | 7727BA227A9E49C2DFCC1E9F2A10924CC472E3425653DC7DE8E4B830712B302E |
| [Français](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40c) | 2DF6655819F604E8D20A396CA9FDFEE279C5A9E50776FFC143A5BA4C3E2F1849 |
| [Allemand](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x407) | 5C44502DEE8B31675D0B10B4CE8CA6F5D96A2692CC498B9859A77C24F30EF870 |
| [Italien](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x410) | 6A616F6E3A1C7DD52457FB8C8692E5C1C551032FF46AD8C5112CF9364F17C631 |
| [Japonais](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x411) | 1244A5EADF673E4BD36E9967A2008F65CA2A1D40E8E7DD4D17640A6FC1EACA9A |
| [Coréen](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x412) | 6E0E057A853F54AB87F3F8984954F590FCCD3BE2EC2139F02EFA085FEA6D3E61 |
| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x416) | 24122C092464B299F41A7644814F375F0CC2786877BCAE0282221FF8D73BD100 |
| [Russe](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x419) | 2CDE208C241C3F13D2EC37294C10503C7A9D1222ED33F6FE54849169F30BE697 |
| [Espagnol](https://go.microsoft.com/fwlink/?linkid=832312&clcid=0x40a) | BD12844E6F0A0ECFD5815D01EDFB8018CE9B4A1044DE8DF96AC740D85E800FD6 |

### <a name="ssdt-for-visual-studio-vs-2012"></a>SSDT pour Visual Studio (VS) 2012

Pour installer cette version de SSDT, vous devez télécharger une image ISO. Le fichier ISO est un fichier auto-extractible qui contient tous les composants nécessaires à SSDT et qui peut être téléchargé à l'aide d'un gestionnaire de téléchargement redémarrable, ce qui est utile en cas de bande passante réseau limitée ou peu fiable. Une fois téléchargée, l’image ISO peut être montée comme lecteur.

Étapes d'installation :

1. **[Télécharger SSDT pour Visual Studio 2012 (11.1.50727.1)](https://go.microsoft.com/fwlink/?linkid=518814)**

2. Ouvrez l’image ISO.

3. Exécutez le fichier *SSDTSetup.exe*.

    ![Image ISO](media/previous-releases-of-sql-server-data-tools-ssdt-and-ssdt-bi/iso-image.png)

Cette version de **SSDT pour Visual Studio 2012** peut être installée dans les langages suivants :

| Langage | Code de hachage SHA256 |
|----------|-------------|
| [Chinois (simplifié)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x804) | 78F20325648CFF49D9C58A26186E0AC199D104B3CF634E5663B4B262BEC69C07 |
| [Chinois (traditionnel)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x404) | A2722380AF2EE1E8BB52B4FA54A61BAB411E5E5FD5B050108F81ED23DC87366D |
| [Anglais (États-Unis)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x409) | 748EF78D3F9CC6FE360432C378EA690DE51AEB2C747E87D43E08448D26F93A91 |
| [Français](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40c) | 40E6504169BF618EDC7EB5B62D1215DC96726D6EEC3CA8EC3EEB49044E4B6FB7 |
| [Allemand](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x407) | C45C974E6B8F9611BA2AC1EE90C5C507992BCE5693BF46F6C7C138591ED6A123 |
| [Italien](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x410) | C1B20CDB41C7B1B5DE76A71F9A091A6FDF5186B12F6AA269DA6338B3CB2D91A6 |
| [Japonais](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x411) | BF56958B904C1C5F28084BD0B16928B6CBFEC83EB3F0C913EC364FA335241943 |
| [Coréen](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x412) | 9023B0656785C357A67E39EB76A2806B923C2BD17342D7226A8ADA384A9F4E28 |
| [Portugais (Brésil)](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x416) | ACAD9FE03729E289ECE821D92C56CFB1D7FCCEA8423ABF1E164BF3C67ABEFEFB |
| [Russe](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x419) | E2191D787BA833DF4A85B064C5373DC44099E76214FBF9505728702D4D6B83F0 |
| [Espagnol](https://go.microsoft.com/fwlink/?linkid=518814&clcid=0x40a) | 6D81FB572A7003C54C29D2ACF076D2CED4A1CA80F329BFF9D41A806920D64EEE |

> [!Note]
> SSDT prend en charge les deux dernières versions de Visual Studio. Avec la version Visual Studio 2019, les versions de SSDT pour Visual Studio 2015 et versions antérieures ne sont plus mises à jour. SSDT pour Visual Studio 2010 n’est plus disponible. Pour plus d’informations, consultez la section *FAQ* de [ce billet de blog de l’équipe SSDT](/archive/blogs/ssdt/sql-server-data-tools-17-0-rc-and-ssdt-in-vs2017).

## <a name="sql-bi-analysis-services-reporting-services-integration-services"></a>SQL BI : Analysis Services, Reporting Services, Integration services

Les modèles BI sont utilisés pour créer des modèles SSAS, des rapports SSRS et des packages SSIS. Les concepteurs BI sont liés à des versions spécifiques de SQL Server. Pour utiliser les nouvelles fonctionnalités BI, installez un concepteur BI d’une version plus récente.

## <a name="bi-designers"></a>Concepteurs BI

[Télécharger SSDT-BI pour Visual Studio 2013](https://www.microsoft.com/download/details.aspx?id=42313) (SQL Server 2014, SQL Server 2012, SQL Server 2008 et 2008 R2)

[Télécharger SSDT-BI pour Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843) (SQL Server 2014, SQL Server 2012, SQL Server 2008 et 2008 R2)

BIDS (Business Intelligence Development Studio) est installé via le programme d’installation de SQL Server. Aucun téléchargement web (QL Server 2008 et 2008 R2).

Pour SQL Server 2012 ou 2014, vous pouvez utiliser **SSDT-BI pour Visual Studio 2012** ou **SSDT-BI fou Visual Studio 2013**. La seule différence entre les deux est la version de Visual Studio.

## <a name="next-steps"></a>Étapes suivantes

- [Télécharger la dernière version de SQL Server Data Tools](../ssdt/download-sql-server-data-tools-ssdt.md)
- [Notes de publication SQL Server Data Tools (SSDT)](release-notes-ssdt.md)
- [Télécharger SQL Server Management Studio (SSMS)](../ssms/download-sql-server-management-studio-ssms.md)
- [Télécharger Azure Data Studio](../azure-data-studio/download-azure-data-studio.md)
- [Outils et utilitaires SQL](../tools/overview-sql-tools.md)