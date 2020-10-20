---
description: Se connecter à une source de données SQL Server (Assistant Importation et Exportation SQL Server)
title: Se connecter à une source de données SQL Server (Assistant Importation et Exportation SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 06/29/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
ms.assetid: 386cedbb-fae5-45ce-9363-c4a417f80a2f
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 5238049681cfb8ec71ea27a444a4e41bf7c64703
ms.sourcegitcommit: cfa04a73b26312bf18d8f6296891679166e2754d
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/19/2020
ms.locfileid: "92196386"
---
# <a name="connect-to-a-sql-server-data-source-sql-server-import-and-export-wizard"></a>Se connecter à une source de données SQL Server (Assistant Importation et Exportation SQL Server)

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


Cette rubrique vous montre comment se connecter à une source de données **Microsoft SQL Server** à partir de la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant Importation et Exportation SQL Server. Vous pouvez utiliser plusieurs fournisseurs de données pour vous connecter à SQL Server.

> [!TIP]
> Si vous êtes sur un réseau doté de nombreux serveurs, il peut être plus facile d’entrer le nom du serveur plutôt que de développer la liste déroulante des serveurs. Si vous cliquez sur la liste déroulante, l’interrogation du réseau pour connaître tous les serveurs disponibles peut prendre beaucoup de temps, et les résultats peuvent même ne pas contenir le serveur que vous souhaitez.

## <a name="connect-to-sql-server-with-the-net-framework-data-provider-for-sql-server"></a>Se connecter à SQL Server avec le fournisseur de données .NET Framework pour SQL Server 
Après avoir sélectionné le **Fournisseur de données .NET Framework pour SQL Server** dans la page **Choisir une source de données** ou **Choisir une destination** de l’Assistant, la page affiche une liste groupée d’options pour le fournisseur. Nombre d’entre elles portent des noms inconnus et ont des paramètres inhabituels. Heureusement, pour vous connecter à une base de données d’entreprise, vous ne devez généralement fournir que quelques éléments d’information. Vous pouvez ignorer les valeurs par défaut des autres paramètres.

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes, que SQL Server soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

|Informations nécessaires|Propriété du fournisseur de données .NET Framework pour SQL Server|
|---|---|
|Authentication|**NotSpecified** par défaut (« Sécurité intégrée »), ou choisissez un autre mode d’authentification. L’authentification interactive Active Directory n’est pas prise en charge. |
|Nom du serveur|**Source de données**|
|Informations d’authentification (connexion)|**Sécurité intégrée** ; ou **ID d’utilisateur** et **Mot de passe**<br/>Si vous voulez afficher la liste déroulante des bases de données sur le serveur, vous devez d’abord fournir des informations de connexion valides.|
|Nom de la base de données|**Catalogue initial**|

![Connexion à SQL avec le fournisseur .NET](../../integration-services/import-export-data/media/connect-to-sql-with-net-provider.jpg)

### <a name="options-to-specify-net-framework-data-provider-for-sql-server"></a>Options à spécifier (Fournisseur de données .NET Framework pour SQL Server)

> [!NOTE]
> Les options de connexion de ce fournisseur de données sont les mêmes, que SQL Server soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

**Source de données**  
 Entrez le nom ou l’adresse IP du serveur source ou de destination, ou sélectionnez un serveur dans la liste déroulante.  
 
 Pour indiquer un port TCP non standard, entrez une virgule après l’adresse IP ou le nom du serveur, puis entrez le numéro de port.
 
 **Catalogue initial**  
 Entrez le nom de la base de données source ou de destination, ou sélectionnez une base de données dans la liste déroulante.  
  
 **Sécurité intégrée**  
 Spécifiez **True** pour établir la connexion par le biais de l’authentification intégrée de Windows (recommandé), ou **False** pour établir la connexion à l’aide de l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Si vous spécifiez **False**, vous devez entrer un ID d'utilisateur et un mot de passe. La valeur par défaut est **False**.  
  
 **ID d'utilisateur**  
 Entrez un nom d’utilisateur si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 **Mot de passe**  
 Entrez le mot de passe si vous utilisez l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

## <a name="connect-to-sql-server-with-the-odbc-driver-for-sql-server"></a>Se connecter à SQL Server avec le pilote ODBC pour SQL Server 
Les pilotes ODBC ne sont pas répertoriés dans la liste déroulante des sources de données. Pour vous connecter avec un pilote ODBC, commencez par sélectionner le **Fournisseur de données .NET Framework pour ODBC** comme source de données. Ce fournisseur agit comme un wrapper autour du pilote ODBC.

> [!TIP]
> **Récupérer le pilote le plus récent**. Téléchargez [Microsoft ODBC Driver for SQL Server](../../connect/odbc/download-odbc-driver-for-sql-server.md).

Voici l’écran générique que vous voyez immédiatement après avoir sélectionné le fournisseur de données .NET Framework pour ODBC.

![Connexion à SQL avec ODBC avant](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-before.jpg)

### <a name="options-to-specify-odbc-driver-for-sql-server"></a>Options à spécifier (pilote ODBC pour SQL Server)

> [!NOTE]
> Les options de connexion de ce pilote ODBC sont les mêmes, que SQL Server soit la source ou la destination. Autrement dit, les options que vous voyez sont identiques dans les pages **Choisir une source de données** et **Choisir une destination** de l’Assistant.

Pour vous connecter à SQL Server au moyen du pilote ODBC le plus récent, assemblez une chaîne de connexion qui inclut les paramètres suivants accompagnés de leur valeur. Le format d’une chaîne de connexion complète est donné immédiatement après la liste des paramètres.

> [!TIP]
> Obtenez de l’aide pour l’assemblage d’une chaîne de connexion correcte. Au lieu de produire une chaîne de connexion, vous pouvez fournir un nom de source de données (DSN, data source name) existant ou en créer un. Pour obtenir plus d’informations sur ces options, consultez [Se connecter à une source de données ODBC](../../integration-services/import-export-data/connect-to-an-odbc-data-source-sql-server-import-and-export-wizard.md).

**Driver**  
Nom du pilote ODBC. Le nom diffère selon les versions du pilote.

**Serveur**  
Nom du serveur SQL Server.

**Sauvegarde de la base de données**  
Nom de la base de données.  

**Trusted_Connection** ; ou **Uid** et **Pwd**  
Spécifiez **Trusted_Connection=Yes** pour vous connecter avec l’authentification intégrée de Windows, sinon spécifiez **Uid** (id d’utilisateur) et **Pwd** (mot de passe) pour vous connecter avec l’authentification SQL Server.

### <a name="connection-string-format"></a>Format de la chaîne de connexion
Voici le format d’une chaîne de connexion qui utilise l’authentification intégrée de Windows.

`Driver={ODBC Driver 13 for SQL Server};server=<server>;database=<database>;trusted_connection=Yes;`

Voici le format d’une chaîne de connexion qui utilise l’authentification SQL Server au lieu de l’authentification intégrée de Windows.

`Driver={ODBC Driver 13 for SQL Server};server=<server>;database=<database>;uid=<user id>;pwd=<password>;`

### <a name="enter-the-connection-string"></a>Entrer la chaîne de connexion
Indiquez la chaîne de connexion dans le champ **ConnectionString** ou entrez le nom de la source de données dans le champ **Dsn** de la page **Choisir une source de données** ou **Choisir une Destination**. Une fois que vous avez entré la chaîne de connexion, l’Assistant analyse cette chaîne et affiche les propriétés individuelles avec leur valeur dans la liste.

L’exemple suivant utilise cette chaîne de connexion.

`Driver={ODBC Driver 13 for SQL Server};server=localhost;database=WideWorldImporters;trusted_connection=Yes;`

Voici l’écran que vous voyez après avoir entré la chaîne de connexion.

![Connexion à SQL avec ODBC après](../../integration-services/import-export-data/media/connect-to-sql-with-odbc-after.jpg)

## <a name="connect-to-sql-server-with-the-microsoft-ole-db-provider-for-sql-server-or-sql-server-native-client"></a>Se connecter à SQL Server au moyen du fournisseur Microsoft OLE DB pour SQL Server ou de SQL Server Native Client

> [!IMPORTANT]
> Le fournisseur Microsoft OLE DB pour SQL Server et SQL Server Native Client ne sont pas pris en charge dans les versions de SQL Server postérieures à SQL Server 2012. Utilisez le pilote ODBC à la place. Pour plus d’informations sur la transition vers le pilote ODBC, consultez les billets de blog suivants.
>   -   [Microsoft s’aligne sur ODBC pour l’accès aux données relationnelles natives](/archive/blogs/sqlnativeclient/microsoft-is-aligning-with-odbc-for-native-relational-data-access)
>   -   [Présentation des nouveaux pilotes Microsoft ODBC pour SQL Server](/archive/blogs/sqlnativeclient/introducing-the-new-microsoft-odbc-drivers-for-sql-server)

## <a name="other-data-providers-and-more-info"></a>Autres fournisseurs de données et renseignements complémentaires
Pour plus d’informations sur la façon de se connecter à SQL Server au moyen d’un fournisseur de données qui n’est pas répertorié ici, consultez [Chaînes de connexion SQL Server](https://www.connectionstrings.com/sql-server/). Ce site tiers contient également des informations complémentaires sur les fournisseurs de données et les paramètres de connexion décrits dans cette page.

## <a name="see-also"></a>Voir aussi
[Choisir une source de données](../../integration-services/import-export-data/choose-a-data-source-sql-server-import-and-export-wizard.md)  
[Choisir une destination](../../integration-services/import-export-data/choose-a-destination-sql-server-import-and-export-wizard.md)