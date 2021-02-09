---
title: Utilitaire bcp
description: L’utilitaire bcp (programme de copie en bloc) copie les données en bloc entre une instance de SQL Server et un fichier de données dans un format spécifié par l’utilisateur.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- bcp utility [SQL Server]
- exporting data
- row exporting [SQL Server]
- copying data [SQL Server], bcp utility
- command prompt utilities [SQL Server], bcp
- tables [SQL Server], importing data
- column importing [SQL Server]
- bcp utility [SQL Server], command options
- file exporting [SQL Server]
- bulk copy [SQL Server]
- bcp utility [SQL Server], about bcp utility
- tables [SQL Server], exporting data
- row importing [SQL Server]
- importing data, bcp utility
- file importing [SQL Server]
- column exporting [SQL Server]
ms.assetid: c0af54f5-ca4a-4995-a3a4-0ce39c30ec38
author: markingmyname
ms.author: maghan
ms.reviewer: v-daenge
ms.custom: seo-lt-2019
ms.date: 09/11/2020
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 9c732f7be3a25752c16e8e09e483097c8b9cac18
ms.sourcegitcommit: 5dcbe4abbe2339292961370c1d8ca3affa625f72
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536410"
---
# <a name="bcp-utility"></a>Utilitaire bcp

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

> Pour utiliser bcp sur Linux, consultez [Installer sqlcmd et bcp sur Linux](../linux/sql-server-linux-setup-tools.md).
>
> Pour plus d’informations sur l’utilisation de bcp avec Azure Synapse Analytics, consultez [Charger des données avec bcp](/azure/sql-data-warehouse/sql-data-warehouse-load-with-bcp).

L’utilitaire **b** ulk **c** opy **p** rogram (**bcp**) copie en bloc des données entre une instance de [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] et un fichier de données dans un format spécifié par l’utilisateur. L’utilitaire **bcp** permet d’importer un grand nombre de nouvelles lignes dans des tables [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] ou d’exporter des données de tables dans des fichiers de données. Sauf lorsqu’il est utilisé avec l’option **queryout** , l’utilitaire ne nécessite aucune connaissance de [!INCLUDE[tsql](../includes/tsql-md.md)]. Pour importer des données dans une table, vous devez utiliser un fichier de format créé pour cette table ou comprendre la structure de la table et les types de données valides pour ses colonnes.  

![Icône de lien vers une rubrique](../database-engine/configure-windows/media/topic-link.gif "Icône du lien de rubrique") Pour plus d’informations sur les conventions de syntaxe utilisées pour **bcp**, consultez [Conventions de la syntaxe Transact-SQL &#40;Transact-SQL&#41;](../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md).  

> [!NOTE]
> Si vous utilisez **bcp** pour sauvegarder vos données, créez un fichier de format pour enregistrer le format de données. Les fichiers de données **bcp** **n’incluent pas** de schéma ni d’informations de format, ce qui fait que si une table ou une vue est supprimée et si vous n’avez pas de fichier de format, il se peut que vous ne soyez pas en mesure d’importer les données.

## <a name="download-the-latest-version-of-bcp-utility"></a>Télécharger la dernière version de l’utilitaire bcp

**[![Télécharger bcp pour x64](../ssdt/media/download.png) Télécharger les utilitaires de ligne de commande Microsoft 15 pour SQL Server (x64)](https://go.microsoft.com/fwlink/?linkid=2142258)**
<br>**[![Télécharger bcp pour x86](../ssdt/media/download.png) Télécharger les utilitaires de ligne de commande Microsoft 15 pour SQL Server (x86)](https://go.microsoft.com/fwlink/?linkid=2142257)**

Les outils en ligne de commande sont en disponibilité générale (GA), mais ils sont publiés avec le package d’installation de [!INCLUDE[sql-server-2019](../includes/sssql19-md.md)].

### <a name="version-information"></a>Informations sur la version

Numéro de version : 15.0.2 <br>
Numéro de build : 15.0.2000.5<br>
Date de publication : 11 septembre 2020

La nouvelle version de SQLCMD prend en charge l’authentification Azure AD, y compris la prise en charge de MFA (Multi-Factor Authentication) pour les fonctionnalités SQL Database, Azure Synapse Analytics et Always Encrypted.
Le nouveau BCP prend en charge l’authentification Azure AD, notamment la prise en charge de MFA pour SQL Database et Azure Synapse Analytics.

### <a name="system-requirements"></a>Configuration requise

Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008, Windows Server 2008 R2, Windows Server 2008 R2 SP1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Server 2019

Ce composant nécessite [Windows Installer 4.5](https://windows-installer.soft32.com/) et [Microsoft ODBC Driver 17 for SQL Server](../connect/odbc/download-odbc-driver-for-sql-server.md).

Pour vérifier la version de BCP, exécutez la commande `bcp /v` et vérifiez que 15.0.2000.5 ou une version ultérieure est utilisée.

<table><th>Syntaxe</th><tr><td><pre>
bcp [<a href="#db_name">database_name.</a>] <a href="#schema">schema</a>.{<a href="#tbl_name">table_name</a> | <a href="#vw_name">view_name</a> | <a href="#query">"query"</a>}
    {<a href="#in">in</a> <a href="#data_file">data_file</a> | <a href="#out">out</a> <a href="#data_file">data_file</a> | <a href="#qry_out">queryout</a> <a href="#data_file">data_file</a> | <a href="#format">format</a> <a href="#format">nul</a>}
<a>                                                                                                         </a>
    [<a href="#a">-a packet_size</a>]
    [<a href="#b">-b batch_size</a>]
    [<a href="#c">-c</a>]
    [<a href="#C">-C { ACP | OEM | RAW | code_page } </a>]
    [<a href="#d">-d database_name</a>]
    [<a href="#D">-D</a>]
    [<a href="#e">-e err_file</a>]
    [<a href="#E">-E</a>]
    [<a href="#f">-f format_file</a>]
    [<a href="#F">-F first_row</a>]
    [<a href="#G">-G Azure Active Directory Authentication</a>]
    [<a href="#h">-h"hint [,...n]"</a>]
    [<a href="#i">-i input_file</a>]
    [<a href="#k">-k</a>]
    [<a href="#K">-K application_intent</a>]
    [<a href="#l">-l login_timeout</a>]
    [<a href="#L">-L last_row</a>]
    [<a href="#m">-m max_errors</a>]
    [<a href="#n">-n</a>]
    [<a href="#N">-N</a>]
    [<a href="#o">-o output_file</a>]
    [<a href="#P">-P password</a>]
    [<a href="#q">-q</a>]
    [<a href="#r">-r row_term</a>]
    [<a href="#R">-R</a>]
    [<a href="#S">-S [server_name[\instance_name]</a>]
    [<a href="#t">-t field_term</a>]
    [<a href="#T">-T</a>]
    [<a href="#U">-U login_id</a>]
    [<a href="#v">-v</a>]
    [<a href="#V">-V (80 | 90 | 100 | 110 | 120 | 130 ) </a>]
    [<a href="#w">-w</a>]
    [<a href="#x">-x</a>]
</pre></td></tr></table>

## <a name="arguments"></a>Arguments

 _**data\_file**_<a name="data_file"></a>  
 Chemin d'accès complet du fichier de données. Lors de l'importation en bloc de données vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le fichier de données contient les données à copier dans la table ou vue spécifiée. Lors de l'exportation en bloc de données à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], le fichier de données contient les données provenant de la table ou vue. Le chemin d'accès peut compter entre 1 et 255 caractères. Le fichier de données peut contenir jusqu’à 2^63 - 1 lignes.  

 _**database\_name**_<a name="db_name"></a>  
 Nom de la base de données qui contient la table ou la vue spécifiée. Sans autre indication, il s'agit de la base de données par défaut de l'utilisateur.  

 Vous pouvez aussi spécifier explicitement le nom de la base de données avec **-d**.  

 **dans** *data_file* \| **hors** *data_file* \| **queryout** *data_file* \| **format nul**  
 Direction de la copie en bloc :  
  
- **in**<a name="in"></a> copie à partir d’un fichier dans une table ou une vue de la base de données.  
  
- **out**<a name="out"></a> copie dans un fichier à partir d’une table ou d’une vue de la base de données. Si vous spécifiez un fichier existant, il est remplacé. Lors de l’extraction des données, l’utilitaire **bcp** représente une chaîne vide comme une chaîne Null, et une chaîne Null comme une chaîne vide.  
  
- **queryout**<a name="qry_out"></a> copie à partir d’une requête et doit être spécifié uniquement lors d’une copie de données en bloc à partir d’une requête.  
  
- **format**<a name="format"></a> crée un fichier de format basé sur l’option spécifiée ( **-n**, **-c**, **-w** ou **-N**) et les délimiteurs de table ou de vue. Lors de la copie en bloc de données, la commande **bcp** peut faire référence à un fichier de format, ce qui évite d’avoir à ressaisir les informations de format de manière interactive. L’option **format** nécessite l’option **-f** ; la création d’un fichier de format XML nécessite aussi l’option **-x** . Pour plus d’informations, consultez [Créer un fichier de format &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md). Vous devez spécifier **nul** comme valeur (**format nul**).  
  
 _**owner**_<a name="schema"></a>  
 Nom du propriétaire de la table ou de la vue. *owner* est facultatif si l'utilisateur qui effectue l'opération est le propriétaire de la table ou de la vue. Si la valeur de *owner* n’est pas spécifiée et si l’utilisateur effectuant l’opération ne possède pas la table ou la vue spécifiée, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] retourne un message d’erreur et l’opération est annulée.  
  
**"** _**query**_ **"** <a name="query"></a> est une requête [!INCLUDE[tsql](../includes/tsql-md.md)] qui retourne un jeu de résultats. Si la requête retourne plusieurs jeux de résultats, seul le premier jeu de résultats est copié dans le fichier de données ; les jeux de résultats suivants sont ignorés. Placez le nom de la requête entre guillemets doubles et tout élément imbriqué dans la requête entre guillemets simples. **queryout** doit également être spécifié uniquement lors d’une copie de données en bloc à partir d’une requête.  
  
 La requête peut référencer une procédure stockée du moment que toutes les tables référencées dans la procédure stockée existent préalablement à l'exécution de l'instruction bcp. Par exemple, si la procédure stockée génère une table temp, l’instruction **bcp** échoue parce que la table temp est uniquement disponible au moment de l’exécution du programme et pas au moment de l’exécution de l’instruction. Dans ce cas, envisagez d’insérer les résultats de la procédure stockée dans une table, puis d’utiliser **bcp** pour copier les données de la table dans un fichier de données.  
  
 _**table\_name**_<a name="tbl_name"></a>  
 Nom de la table de destination lors de l’importation de données dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) ou nom de la table source lors de l’exportation de données à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**).  
  
 _**view\_name**_<a name="vw_name"></a>   
 Nom de la vue de destination lors de la copie de données vers [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**in**) ou nom de la vue source lors de la copie de données à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] (**out**). Seules les vues dont toutes les colonnes référencent la même table peuvent être utilisées comme vues de destination. Pour plus d’informations sur les restrictions relatives à la copie des données dans les vues, consultez [INSERT &#40;Transact-SQL&#41;](../t-sql/statements/insert-transact-sql.md).  
  
 **-a** _**packet\_size**_<a name="a"></a>  
 Spécifie le nombre d’octets, par paquet réseau, envoyés depuis/vers le serveur. Vous pouvez définir une option de configuration du serveur au moyen de [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] (ou de la procédure stockée système **sp_configure** ). Toutefois, la configuration du serveur peut être modifiée individuellement à l'aide de cette option. *packet_size* peut être compris entre 4 096 octets et 65 535 octets. Sa valeur par défaut est 4 096.  
  
 L'augmentation de la taille des paquets peut améliorer les performances des opérations de copie en bloc. S'il est demandé une taille de paquet plus importante alors que cela n'est pas possible, la valeur par défaut est utilisée. Les statistiques de performance générées par **bcp** indiquent la taille de paquet utilisée.  
  
 **-b** _**batch\_size**_<a name="b"></a>  
 Nombre de lignes par lot de données importées. Chaque lot est importé et consigné dans un journal comme transaction distincte important le lot complet avant d'être validée. Par défaut, toutes les lignes du fichier de données sont importées comme un lot. Pour distribuer les lignes entre les différents lots, spécifiez une valeur de *batch_size* inférieure au nombre de lignes du fichier de données. Si la transaction d'un lot échoue, seules les insertions du lot actif sont restaurées. Les lots dont l'importation a été effectuée par des transactions validées ne sont pas affectés par une défaillance ultérieure.  
  
 N’utilisez pas cette option avec l’option **-h "** ROWS_PER_BATCH **=** _bb_ **"** .  
 
 **-c**<a name="c"></a>  
 Effectue l'opération en utilisant un type de données caractères. Cette option n’affiche aucune invite pour aucun champ, mais utilise le type de données **char** comme type de stockage, n’ajoute pas de préfixe et emploie **\t** (tabulation) comme séparateur de champ et **\r\n** (nouvelle ligne) comme indicateur de fin de ligne. **-c** n’est pas compatible avec **-w**.  
  
 Pour plus d’informations, consultez [Utiliser le format caractère pour importer ou exporter des données &#40;SQL Server&#41;](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md).  
  
 **-C** { **ACP** \| **OEM** \| **RAW** \| *code_page* }<a name="C"></a>   
 Indique la page de codes des données dans le fichier. L’utilisation de *code_page* n’est justifiée que si les données contiennent des colonnes de type **char**, **varchar** ou **text** dont les valeurs de caractères sont supérieures à 127 ou inférieures à 32.  
  
> [!NOTE]
> Nous recommandons de spécifier un nom de classement pour chaque colonne dans un fichier de format, sauf lorsque vous souhaitez que l’option 65001 soit prioritaire sur la spécification de page de codes/classement
  
|Valeur de la page de codes|Description|  
|---------------------|-----------------|  
|ACP|[!INCLUDE[vcpransi](../includes/vcpransi-md.md)]/Microsoft Windows (ISO 1252).|  
|OEM|Page de codes par défaut du client. Il s’agit de la page de codes par défaut qui est utilisée si **-C** n’est pas spécifié.|  
|RAW|Aucune conversion d'une page de codes vers une autre n'a lieu. Il s'agit de l'option la plus rapide car aucune conversion n'a lieu.|  
|*code_page*|Numéro spécifique de la page de codes, par exemple 850.<br /><br /> Les versions antérieures à la version 13 ([!INCLUDE[sssql15-md](../includes/sssql16-md.md)]) ne prennent pas en charge la page de codes 65001 (encodage UTF-8). La version 13 et les versions ultérieures peuvent importer l’encodage UTF-8 pour les versions antérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].|  
|||
  
 **-d** _**database\_name**_<a name="d"></a>   
 Spécifie la base de données à laquelle se connecter. Par défaut, bcp.exe se connecte à la base de données par défaut de l'utilisateur. Si -d database_name et un nom en trois parties (database_name.schema.table, passé comme premier paramètre à bcp.exe) sont spécifiés, une erreur se produit, car vous ne pouvez pas spécifier deux fois le nom de la base de données. Si *database_name* commence par un trait d’union (-) ou une barre oblique (/), n’ajoutez pas d’espace entre **-d** et le nom de la base de données.  

**-D**<a name="D"></a>  
Fait en sorte que la valeur passée à l’option `bcp` `-S` soit interprétée comme un nom de source de données (DSN). Un nom de source de données peut être utilisé pour incorporer des options de pilote afin de simplifier les lignes de commande, appliquer des options de pilote qui ne sont pas accessibles autrement à partir de la ligne de commande (telles que MultiSubnetFailover), ou pour aider à protéger les informations d’identification sensibles contre la détection en tant qu’arguments de ligne de commande. Pour plus d’informations, consultez _Prise en charge du nom de source de données dans sqlcmd et bcp_ dans [Connexion avec sqlcmd](../connect/odbc/linux-mac/connecting-with-sqlcmd.md).


 **-e** _**err\_file**_<a name="e"></a>  
 Spécifie le chemin complet d’un fichier d’erreur utilisé pour stocker les lignes que l’utilitaire **bcp** ne peut pas transférer du fichier vers la base de données. Les messages d’erreur de la commande **bcp** sont transmis à la station de travail de l’utilisateur. Si cette option est omise, aucun fichier d'erreur n'est créé.  
  
 Si *err_file* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-e** et la valeur *err_file* .  
  
**-E**<a name="E"></a>

Indique que la ou les valeurs d'identité figurant dans le fichier de données importé doivent être utilisées dans la colonne d'identité. Si **-E** n’est pas spécifié, les valeurs d’identité de cette colonne figurant dans le fichier de données importé ne sont pas prises en compte et [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] attribue automatiquement des valeurs uniques, basées sur la valeur initiale et sur un incrément spécifié durant la création de la table.  Pour plus d’informations, consultez [DBCC CHECKIDENT](../t-sql/database-console-commands/dbcc-checkident-transact-sql.md).
  
 Si le fichier de données ne contient pas de valeurs pour la colonne d'identité de la table ou de la vue, utilisez un fichier de format pour spécifier que la colonne d'identité ne doit pas être prise en compte lors de l'importation des données ; auquel cas [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] attribue automatiquement des valeurs uniques à la colonne.
  
 L’option **-E** n’exige aucune autorisation spéciale. Pour plus d’informations, consultez la section[Notes](#remarks), plus loin dans cette rubrique.  
   
 **-f** _**format\_file**_<a name="f"></a>  
 Spécifie le chemin complet au fichier de format. La signification de cette option dépend de l'environnement d'utilisation :  
  
- Si **-f** est utilisé avec l’option **format** , le fichier de format spécifié ( *format_file* ) est créé pour la table ou la vue spécifiée. Pour créer un fichier de format XML, spécifiez également l’option **-x** . Pour plus d’informations, consultez [Créer un fichier de format &#40;SQL Server&#41;](../relational-databases/import-export/create-a-format-file-sql-server.md).  
  
- Utilisé avec l’option **in** ou **out** , **-f** nécessite un fichier de format existant.  
  
    > [!NOTE]
    > L’utilisation d’un fichier de format avec l’option **in** ou **out** est facultative. En l’absence de l’option **-f** , si **-n**, **-c**, **-w** ou **-N** n’est pas spécifiée, la commande vous invite à fournir des informations de format et vous permet d’enregistrer vos réponses dans un fichier de format (dont le nom de fichier par défaut est Bcp.fmt).
  
 Si *format_file* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-f** et la valeur *format_file* .  
  
**-F** _**first\_row**_<a name="F"></a>  
 Spécifie le numéro de la première ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données. Ce paramètre nécessite une valeur supérieure à (>) 0 mais inférieure (<) ou égale au (=) nombre total de lignes. En l'absence de ce paramètre, la valeur par défaut est la première ligne du fichier.  
  
 *first_row* peut être un entier positif avec une valeur maximale de 2^63-1. **-F** *first_row* est basé sur 1.  

**-G**<a name="G"></a>

 Ce commutateur est utilisé par le client durant la connexion à Azure SQL Database ou à Azure Synapse Analytics pour faire en sorte que l’utilisateur soit authentifié à l’aide de l’authentification Azure Active Directory. Le commutateur -G nécessite la [version 14.0.3008.27 ou ultérieure](https://go.microsoft.com/fwlink/?LinkID=825643). Pour déterminer votre version, exécutez bcp -v. Pour plus d’informations, consultez [Utiliser l’authentification Azure Active Directory pour l’authentification auprès de SQL Database ou Azure Synapse Analytics](/azure/sql-database/sql-database-aad-authentication). 

> [!IMPORTANT]
> L’option **-G** s’applique uniquement à Azure SQL Database et à Azure Data Warehouse.
> L’authentification intégrée et interactive AAD n’est actuellement pas prise en charge sur Linux ou macOS.

> [!TIP]
> Pour vérifier si votre version de BCP comprend la prise en charge de type d’authentification Azure Active Directory (AAD), tapez **BCP--** (BCP \<space>\<dash>\<dash>) et vérifiez que -G apparait dans la liste des arguments disponibles.

- **Nom d’utilisateur et mot de passe Azure Active Directory :** 

    Lorsque vous souhaitez utiliser un nom d’utilisateur Azure Active Directory et le mot de passe, vous pouvez fournir l’option **- G** et utiliser également le nom d’utilisateur et le mot de passe en fournissant les options **-U** et **-P** . 

    L’exemple suivant exporte des données à l’aide du nom d’utilisateur et du mot de passe Azure AD, où user et password sont des informations d’identification AAD. L’exemple exporte la table `bcptest` de la base de données `testdb` à partir d’Azure Server `aadserver.database.windows.net` et stocke les données dans le fichier `c:\last\data1.dat` :

    ```cmd
    bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

    L’exemple suivant importe des données à l’aide du nom d’utilisateur et du mot de passe Azure AD, où user et password sont des informations d’identification AAD. L’exemple importe des données à partir du fichier `c:\last\data1.dat` dans la table `bcptest` pour la base de données `testdb` sur le serveur Azure `aadserver.database.windows.net` à l’aide du nom d’utilisateur et du mot de passe Azure AD :

    ```cmd
    bcp bcptest in "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com -P xxxxx
    ```

- **Intégrée à Azure Active Directory**

    Pour l’authentification intégrée à Azure Active Directory, spécifiez l’option **-G** sans nom d’utilisateur ni mot de passe. Cette configuration suppose que le compte d’utilisateur Windows actuel (le compte sous lequel s’exécute la commande bcp) est fédéré avec Azure AD :

    L’exemple suivant exporte des données à l’aide d’un compte intégré à Azure AD. L’exemple exporte la table `bcptest` de la base de données `testdb` avec Azure AD Integrated à partir d’Azure Server `aadserver.database.windows.net` et stocke les données dans le fichier `c:\last\data2.dat` :

    ```cmd
    bcp bcptest out "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

    L’exemple suivant importe des données à l’aide de l’authentification Azure AD Integrated. L’exemple importe des données à partir du fichier `c:\last\data2.txt` dans la table `bcptest` pour la base de données `testdb` sur le serveur Azure `aadserver.database.windows.net` à l’aide de l’authentification Azure AD Integrated :

    ```cmd
    bcp bcptest in "c:\last\data2.dat" -S aadserver.database.windows.net -d testdb -G -c -t
    ```

- **Azure Active Directory Interactive**  

   L’authentification interactive Azure AD pour Azure SQL Database et Azure Synapse Analytics vous permet d’utiliser une méthode interactive qui prend en charge l’authentification multifacteur. Pour plus d’informations, consultez [Authentification interactive Active Directory](../ssdt/azure-active-directory.md#active-directory-interactive-authentication).

   L’authentification Azure AD interactive requiert **bcp** [version 15.0.1000.34](#download-the-latest-version-of-bcp-utility) ou version ultérieure, ainsi que [ODBC version 17.2 ou ultérieure](../connect/odbc/download-odbc-driver-for-sql-server.md).  

   Pour activer l’authentification interactive, fournissez l’option -G avec le nom d’utilisateur (-U) uniquement, sans mot de passe.

   L’exemple suivant exporte des données à l’aide du mode interactif Azure AD indiquant le nom d’utilisateur où l’utilisateur représente un compte AAD. C’est le même exemple que celui utilisé dans la section précédente : *Nom d’utilisateur et mot de passe Azure Active Directory*.  

   Le mode interactif nécessite que vous entriez un mot de passe manuellement ou, pour les comptes sur lesquels l’authentification multifacteur est activée, effectuez votre méthode d’authentification MFA configurée.

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U alice@aadtest.onmicrosoft.com
   ```

   Si un utilisateur Azure AD est un domaine fédéré avec un compte Windows, le nom d’utilisateur requis sur la ligne de commande contient son compte de domaine (par exemple, joe@contoso.com voir ci-dessous) :

   ```cmd
   bcp bcptest out "c:\last\data1.dat" -c -t -S aadserver.database.windows.net -d testdb -G -U joe@contoso.com
   ```

   Si des utilisateurs invités existent dans un Azure AD spécifique et font partie d’un groupe qui existe dans SQL Database et qui dispose d’autorisations de base de données pour exécuter la commande bcp, leur alias d’utilisateur invité est utilisé (par exemple, *keith0@adventureworks.com* ).
  
**-h** _**"load hints**_[ ,... *n*] **"** <a name="h"></a> Spécifie le ou les indicateurs à utiliser lors de l’importation en bloc de données vers une table ou une vue.  
  
* **ORDER**(**_colonne_[ASC | DESC] [** , **..._n_])**  
Ordre de tri des données dans le fichier de données. Les performances de l'importation en bloc sont améliorées si les données importées sont triées en fonction de l'index cluster de la table, le cas échéant. Si le fichier de données est trié dans un ordre différent, c'est-à-dire dans un ordre autre que celui d'une clé d'index cluster, ou s'il n'existe pas d'index cluster dans la table, l'option ORDER est ignorée. Les noms de colonnes fournis doivent être des noms de colonnes valides dans la table de destination. Par défaut, **bcp** considère que le fichier de données n’est pas ordonné. Pour une importation en bloc optimisée, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] valide également le fait que les données importées sont triées.  
  
* **ROWS_PER_BATCH** **=** _**bb**_  
Nombre de lignes de données par lot ( *bb*). Utilisée quand **-b** n’est pas spécifié, cette option provoque l’envoi au serveur de la totalité du fichier de données au cours d’une transaction unique. Le serveur optimise le chargement en bloc en fonction de la valeur de *bb*. Par défaut, ROWS_PER_BATCH est inconnu.  
  
* **KILOBYTES_PER_BATCH** **=** _**cc**_  
Nombre approximatif de kilo-octets (Ko) de données par lot ( *cc*). Par défaut, KILOBYTES_PER_BATCH est inconnu.  
  
* **TABLOCK**  
Spécifie qu’un verrou de niveau table d’une mise à jour en bloc est obtenu pour la durée de l’opération de chargement en bloc ; sinon, un verrou de niveau ligne est obtenu. Cette option augmente sensiblement les performances car le maintien d'un verrou pour la durée de la seule opération de copie réduit la contention de verrouillage de la table. Une table peut être chargée simultanément par plusieurs clients à condition qu’elle ne comporte pas d’index et que **TABLOCK** soit spécifié. Par défaut, le comportement de verrouillage est déterminé par l’option de table **table lock on bulkload**.  
  
  > [!NOTE]
  > Si la table cible est un index cluster columnstore, l’indicateur TABLOCK n’est pas requis pour le chargement par plusieurs clients simultanés, car chaque thread simultané a reçu un rowgroup distinct au sein de l’index et y charge les données. Pour plus d’informations, reportez-vous aux rubriques sur les concepts de l’index columnstore.
  
  **CHECK_CONSTRAINTS**  
  Spécifie que toutes les contraintes sur la table ou vue cible doivent être vérifiées pendant l'opération d'importation en bloc. Sans l’option CHECK_CONSTRAINTS, les contraintes CHECK et FOREIGN KEY sont ignorées. Après l’opération, les contraintes sur la table sont marquées comme non approuvées.  
  
  > [!NOTE]
  > Les contraintes UNIQUE, PRIMARY KEY et NOT NULL sont toujours appliquées.
  
  À un point donné, vous devez vérifier les contraintes sur toute la table. Si la table n'était pas vide avant l'opération d'importation en bloc, le coût de la revalidation de la contrainte peut dépasser celui de l'application des contraintes CHECK aux données incrémentielles. Par conséquent, nous vous recommandons d'activer le contrôle de contrainte pendant une importation en bloc incrémentielle.  
  
  Il peut notamment convenir de désactiver les contraintes (comportement par défaut) si les données d'entrée contiennent des lignes qui violent des contraintes. Lorsque les contraintes CHECK sont désactivées, vous pouvez importer les données et utiliser ensuite des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] pour supprimer les données non valides.  
  
  > [!NOTE]
  > **bcp** applique désormais une validation des données et des contrôles de données qui peuvent entraîner l’échec de scripts existants si ceux-ci sont exécutés sur des données non valides dans un fichier de données.

  > [!NOTE]
  > Le commutateur **-m** *max_errors* n’applique pas le contrôle de contrainte.
  
* **FIRE_TRIGGERS**  
Spécifié avec l’argument **in** , n’importe quel déclencheur d’insertion sur la table de destination s’exécute pendant l’opération de copie en bloc. Si FIRE_TRIGGERS n'est pas spécifié, aucun déclencheur d'insertion ne s'exécute. FIRE_TRIGGERS est ignoré pour les arguments **out**, **queryout** et **format** .  
  
**-i** _**input\_file**_<a name="i"></a>  
Spécifie le nom d’un fichier réponse contenant les réponses aux questions d’invite de commandes pour chaque champ de données quand une copie en bloc est effectuée en mode interactif ( **-n**, **-c**, **-w** ou **-N** non spécifié).  
  
Si *input_file* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-i** et la valeur *input_file* .  
  
**-k**<a name="k"></a>  
Pendant l’opération, les colonnes vides doivent conserver une valeur NULL et les colonnes insérées ne doivent pas prendre de valeur par défaut. Pour plus d’informations, consultez [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md).  
  
**-K** _**application\_intent**_<a name="K"></a>   
Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. La seule valeur possible est **ReadOnly**(lecture seule). Si **-K** n’est pas spécifié, l’utilitaire bcp ne prend pas en charge la connectivité à un réplica secondaire dans un groupe de disponibilité Always On. Pour plus d’informations, consultez [Secondaires actifs : Réplicas secondaires accessibles en lecture &#40;groupes de disponibilité AlwaysOn&#41;](../database-engine/availability-groups/windows/active-secondaries-readable-secondary-replicas-always-on-availability-groups.md).  
  
**-l** _**login\_timeout**_<a name="l"></a>  
Spécifie un délai de connexion. L’option -l spécifie le nombre de secondes au terme duquel une connexion de SQL Server expire quand vous tentez de vous connecter à un serveur. Par défaut, le délai d'expiration de la connexion est de 15 secondes. Le délai de connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n’est pas numérique ou n’est pas comprise dans cette plage, bcp génère un message d’erreur. Une valeur 0 spécifie un délai d’expiration infini.
  
**-L** _**last\_row**_<a name="L"></a>  
Spécifie le numéro de la dernière ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données. Ce paramètre nécessite une valeur supérieure à (>) 0 mais inférieure (<) ou égale au (=) numéro de la dernière ligne. En l'absence de ce paramètre, la valeur par défaut est la dernière ligne du fichier.  
  
*last_row* peut être un entier positif avec une valeur maximale de 2^63-1.  
  
**-m** _**max\_errors**_<a name="m"></a>  
Spécifie le nombre maximal d’erreurs de syntaxe toléré avant l’annulation de l’opération **bcp** . Une erreur de syntaxe implique une erreur de conversion de données vers le type de données cible. Le total *max_errors* exclut les erreurs pouvant être détectées uniquement sur le serveur, telles que des violations de contrainte.  
  
 Une ligne ne pouvant pas être copiée par l’utilitaire **bcp** est ignorée et est comptabilisée comme une erreur. Si cette option est omise, sa valeur par défaut est 10.  
  
> [!NOTE]
> L’option **-m** ne s’applique pas non plus à la conversion des types de données **money** ou **bigint** .
  
**-n**<a name="n"></a>  
Effectue la copie en bloc en faisant appel aux types de données par défaut des données (ceux de la base de données). Cette option n'affiche aucune invite pour aucun champ, mais utilise les valeurs par défaut.  
  
Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md).  
  
**-N**<a name="N"></a>  
Copie en bloc en faisant appel aux types de données natifs (base de données) des données non caractères, ainsi qu'au type Unicode pour les données caractères. Cette option, qui remplace avantageusement **-w** , sert à transférer des données d’une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vers une autre à l’aide d’un fichier de données. Elle n'affiche aucune invite pour aucun champ. Utilisez-la pour transférer des données comportant des caractères ANSI étendus et conserver tous les avantages des performances du mode natif.  
  
 Pour plus d’informations, consultez [Utiliser le format natif Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md).  
  
 Si vous exportez puis importez des données dans le même schéma de la table à l’aide de bcp.exe avec **-N**, un avertissement de troncation peut s’afficher s’il existe une colonne de type caractère non Unicode, d’une longueur fixe (par exemple, **char(10)** ).  
  
 Vous pouvez ignorer cet avertissement. Une façon de résoudre cet avertissement consiste à utiliser **-n** au lieu de **-N**.  
  
 **-o** _**output\_file**_<a name="o"></a>  
 Spécifie le nom d’un fichier recevant la sortie redirigée à partir de l’invite de commandes.  
  
 Si *output_file* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-o** et la valeur *output_file* .  
  
 **-P** _**password**_<a name="P"></a>  
 Spécifie le mot de passe de l’ID de connexion. Si cette option n’est pas utilisée, la commande **bcp** invite à entrer un mot de passe. Si vous utilisez cette option à la fin de l’invite de commandes sans spécifier de mot de passe, **bcp** emploie le mot de passe par défaut (NULL).  
  
> [!IMPORTANT]
> [!INCLUDE[ssNoteStrongPass](../includes/ssnotestrongpass-md.md)]
  
 Pour masquer votre mot de passe, ne spécifiez pas l’option **-P** avec l’option **-U** . À la place, après avoir spécifié **bcp** avec l’option **-U** et d’autres commutateurs (ne spécifiez pas **-P**), appuyez sur Entrée ; la commande vous demande alors d’entrer un mot de passe. Cette méthode garantit le masquage de votre mot de passe lors de son entrée.  
  
 Si *password* commence par un trait d’union (-) ou une barre oblique (/), n’ajoutez pas d’espace entre **-P** et la valeur *password* .  
  
 **-q**<a name="q"></a>  
 Exécute l’instruction SET QUOTED_IDENTIFIERS ON dans la connexion entre l’utilitaire **bcp** et une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Utilisez cette option pour spécifier un nom de base de données, de propriétaire, de table ou de vue contenant un espace ou un guillemet simple. Mettez entre guillemets doubles (" ") les trois parties du nom de la table ou de la vue.  
  
 Pour spécifier un nom de base de données comportant un espace ou un guillemet simple, vous devez utiliser l’option **-q** .  
  
 **-q** ne s’applique pas aux valeurs transmises à **-d**.  
  
 Pour plus d’informations, consultez la section [Notes](#remarks), plus loin dans cette rubrique.  
  
 **-r** _**row\_term**_<a name="r"></a>  
 Spécifie l’indicateur de fin de ligne. Par défaut, il s’agit du caractère de saut de ligne ( **\n** ). Utilisez ce paramètre pour remplacer l'indicateur de fin de ligne par défaut. Pour plus d’informations, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Si vous spécifiez l'indicateur de fin de ligne en notation hexadécimale dans une commande bcp.exe, la valeur sera tronquée à 0x00. Par exemple, si vous spécifiez 0x410041, 0x41 sera utilisé.  
  
 Si *row_term* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-r** et la valeur *row_term* .  
  
 **-R**<a name="R"></a>  
 Spécifie que les données de type devise, date et heure sont copiées en bloc dans [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] en utilisant le format régional défini par les paramètres régionaux de l'ordinateur client. Par défaut, les paramètres régionaux sont ignorés.  
  
 **-S** _**server\_name**_ [\\_**instance\_name**_]<a name="S"></a> Spécifie l’instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec laquelle la connexion sera établie. Si aucun serveur n’est spécifié, l’utilitaire **bcp** se connecte à l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur l’ordinateur local. Cette option est requise lorsqu’une commande **bcp** est exécutée depuis un ordinateur distant sur le réseau ou sur une instance nommée locale. Pour se connecter à l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur un serveur, spécifiez uniquement *server_name*. Pour vous connecter à une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], spécifiez _server\_name_ **\\** _instance\_name_.  
  
 **-t** _**field\_term**_<a name="t"></a>  
 Spécifie l’indicateur de fin de champ. Par défaut, il s’agit du caractère de tabulation ( **\t** ). Utilisez ce paramètre pour remplacer l'indicateur de fin de champ par défaut. Pour plus d’informations, consultez [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md).  
  
 Si vous spécifiez l'indicateur de fin de champ en notation hexadécimale dans une commande bcp.exe, la valeur sera tronquée à 0x00. Par exemple, si vous spécifiez 0x410041, 0x41 sera utilisé.  
  
 Si *field_term* commence par un trait d’union (-) ou une barre oblique (/), n’incluez pas d’espace entre **-t** et la valeur *field_term* .  
  
 **-T**<a name="T"></a>  
 Spécifie que l'utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] avec une connexion approuvée qui utilise la sécurité intégrée. Les informations d’identification de sécurité de l’utilisateur réseau, *login_id* et *password* , ne sont pas requises. Si **-T** n’est pas spécifié, vous devez indiquer **-U** et **-P** pour vous connecter.

> [!IMPORTANT]
> Quand l’utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] via une connexion approuvée utilisant la sécurité intégrée, utilisez l’option **-T** (connexion approuvée) à la place de la combinaison *user name* et *password* . Lorsque l’utilitaire **bcp** se connecte à SQL Database ou à Azure Synapse Analytics à l’aide de l’authentification Windows, ou lorsque l’authentification Azure Active Directory n’est pas prise en charge. Utilisez les options **-U** et **-P** . 
  
 **-U** _**login\_id**_<a name="U"></a>  
 Spécifie l'ID de connexion utilisé pour une connexion à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
> [!IMPORTANT]
> Quand l’utilitaire **bcp** se connecte à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] via une connexion approuvée utilisant la sécurité intégrée, utilisez l’option **-T** (connexion approuvée) à la place de la combinaison *user name* et *password* . Lorsque l’utilitaire **bcp** se connecte à SQL Database ou à Azure Synapse Analytics à l’aide de l’authentification Windows, ou lorsque l’authentification Azure Active Directory n’est pas prise en charge. Utilisez les options **-U** et **-P** .
  
 **-v**<a name="v"></a>  
 Indique le numéro de version et le copyright de l’utilitaire **bcp** .  
  
 **-V** (**80** \| **90** \| **100** \| **110** \| **120** \| **130**)<a name="V"></a>  
 Copie en bloc en faisant appel aux types de données d'une version antérieure de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Cette option ne pose aucune question pour aucun champ, mais utilise les valeurs par défaut.  
  
 **80** = [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)]  
  
 **90** = [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]  
  
 **100** = [!INCLUDE[ssKatmai](../includes/sskatmai-md.md)] et [!INCLUDE[ssKilimanjaro](../includes/sskilimanjaro-md.md)]  
  
 **110** = [!INCLUDE[ssSQL11](../includes/sssql11-md.md)]  
  
 **120** = [!INCLUDE[ssSQL14](../includes/sssql14-md.md)]  
  
 **130** = [!INCLUDE[sssql15-md](../includes/sssql16-md.md)]  
  
 Par exemple, pour générer des données pour les types non pris en charge par [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)], mais introduits dans les versions ultérieures de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilisez l'option -V80.  
  
 Pour plus d’informations, consultez [Importer des données au format natif et caractère à partir de versions antérieures de SQL Server](../relational-databases/import-export/import-native-and-character-format-data-from-earlier-versions-of-sql-server.md).  
  
 **-w**<a name="w"></a>  
 Copie en bloc en utilisant les caractères Unicode. Cette option n’affiche aucune invite pour aucun champ, mais utilise le type de données **nchar** comme type de stockage, n’ajoute pas de préfixe et emploie **\t** (tabulation) comme séparateur de champs et **\n** (caractère de saut de ligne) comme indicateur de fin de ligne. **-w** n’est pas compatible avec **-c**.  
  
 Pour plus d’informations, consultez [Utiliser le format caractère Unicode pour importer ou exporter des données &#40;SQL Server&#41;](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md).  
  
 **-x**<a name="x"></a>  
 Utilisé avec les options **format** et **-f** *format_file*, génère un fichier au format XML à la place du fichier au format non-XML par défaut. L’option **-x** ne fonctionne pas lors de l’importation ou de l’exportation de données. Elle génère une erreur si elle est utilisée sans **format** et **-f** *format_file*.  

## <a name="remarks"></a>Notes<a name="remarks"></a>

- Le client **bcp** 13.0 est installé quand vous installez les outils [!INCLUDE[msCoName](../includes/msconame-md.md)] [!INCLUDE[sssql19-md.md](../includes/sssql19-md.md)]. Si les outils sont installés pour plusieurs versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], en fonction de la valeur de la variable d’environnement PATH, vous pouvez utiliser le client **bcp** antérieur au lieu du client **bcp** 13.0. Cette variable d'environnement définit l'ensemble de répertoires utilisés par Windows pour rechercher des fichiers exécutables. Pour savoir quelle version vous utilisez, exécutez la commande **bcp /v** ou **bcp -v** à l’invite de commandes Windows. Pour plus d’informations sur la définition du chemin de commande dans la variable d’environnement PATH, consultez [Variables d’environnement](/windows/win32/shell/user-environment-variables) ou recherchez Variables d’environnement dans l’aide de Windows.

    Pour vous assurer que vous exécutez la version la plus récente de l’utilitaire bcp, vous devez supprimer toutes les anciennes versions de cet utilitaire.

    Pour déterminer l’emplacement d’installation de toutes les versions de l’utilitaire bcp, tapez la commande suivante dans l’invite de commandes :

    ```cmd
    where bcp.exe
    ```

- L’utilitaire bcp peut également être téléchargé séparément depuis le [Microsoft SQL Server 2016 Feature Pack](https://www.microsoft.com/download/details.aspx?id=56833).  Sélectionnez `ENU\x64\MsSqlCmdLnUtils.msi` ou `ENU\x86\MsSqlCmdLnUtils.msi`.

- Les fichiers de format XML ne sont pris en charge que si les outils [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sont installés conjointement avec [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] Native Client.

- Pour savoir où trouver l’utilitaire **bcp** ou comment l’exécuter, et pour connaître les conventions syntaxiques des utilitaires d’invite de commandes, consultez [Référence de service d’invite de commande &#40;moteur de base de données&#41;](../tools/command-prompt-utility-reference-database-engine.md).

- Pour plus d’informations sur la préparation des données en vue d’une importation ou d’une exportation en bloc, consultez [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md).

- Pour savoir à quel moment les opérations d’insertion de ligne effectuées par l’importation en bloc sont consignées dans le journal des transactions, consultez [Conditions requises pour une journalisation minimale dans l’importation en bloc](../relational-databases/import-export/prerequisites-for-minimal-logging-in-bulk-import.md).

- [Utilisation de caractères spéciaux supplémentaires](/windows-server/administration/windows-commands/set_1#remarks)

    Les caractères <, >, |, & et ^ sont des caractères d’interpréteur de commandes spéciaux. Ils doivent être précédés du caractère d’échappement (^) ou placés entre guillemets quand ils sont utilisés dans une chaîne (par exemple, « StringContaining&Symbol »). Si vous placez une chaîne contenant un caractère spécial entre guillemets, les guillemets sont définis comme faisant partie de la valeur de la variable d’environnement.

## <a name="native-data-file-support"></a>Prise en charge de fichier de données natif

 Dans [!INCLUDE[ssnoversion](../includes/ssnoversion-md.md)], l’utilitaire **bcp** prend en charge les fichiers de données natifs compatibles avec les versions de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à partir de [!INCLUDE[ssVersion2000](../includes/ssversion2000-md.md)] et ultérieur.

## <a name="computed-columns-and-timestamp-columns"></a>Colonnes calculées et colonnes horodateur

 Les valeurs dans le fichier de données importé pour des colonnes calculées ou **timestamp** sont ignorées, et [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] attribue automatiquement des valeurs. Si le fichier de données ne contient pas de valeurs pour les colonnes calculées ou **timestamp** de la table, utilisez un fichier de format pour spécifier que les colonnes calculées ou **timestamp** de la table ne doivent pas être prises en compte lors de l’importation des données ; auquel cas, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] attribue automatiquement des valeurs à la colonne.  
  
 Les colonnes calculées et **timestamp** sont copiées en bloc de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vers un fichier de données comme d’ordinaire.  

## <a name="specifying-identifiers-that-contain-spaces-or-quotation-marks"></a>Spécification d'identificateurs contenant des espaces ou des guillemets

 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] peuvent inclure des caractères tels que des espaces et des guillemets incorporés. De tels identificateurs doivent être traités de la manière suivante :  
  
- Quand vous spécifiez, à l'invite de commandes, un identificateur ou un nom de fichier comportant un espace ou une apostrophe, mettez cet identificateur entre guillemets doubles (" ").  

    Par exemple, la commande `bcp out` suivante crée un fichier de données nommé `Currency Types.dat`:  

    ```cmd
    bcp AdventureWorks2012.Sales.Currency out "Currency Types.dat" -T -c  
    ```
  
- Pour spécifier un nom de base de données comportant un espace ou un guillemet simple, vous devez utiliser l’option **-q** .  
  
- Pour les noms de propriétaires, de tables ou de vues incorporant des espaces ou des guillemets simples, vous pouvez :  
  
    - Spécifier l’option **-q** , ou  
  
    - Placer le nom de propriétaire, de table ou de vue entre crochets ([]), à l'intérieur des guillemets simples.  

## <a name="data-validation"></a>Validation des données

 **bcp** applique désormais une validation des données et des contrôles de données qui peuvent entraîner l’échec de scripts existants si ceux-ci sont exécutés sur des données non valides dans un fichier de données. Par exemple, **bcp** vérifie maintenant que :  
  
- Les représentations en mode natif des types de données float ou real sont valides.  
  
- les données Unicode comportent un nombre d'octets pair.  
  
 Les types de données non valides qui pouvaient être importées dans les versions précédentes de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] risquent de ne pas pouvoir être chargées désormais ; tandis que dans les versions précédentes, l'échec ne se produisait que lorsqu'un client tentait d'accéder aux données non valides. La validation supplémentaire réduit les risques d’incidents lors de l’interrogation des données après un chargement en bloc.  

## <a name="bulk-exporting-or-importing-sqlxml-documents"></a>Exportation et importation en bloc de documents SQLXML

Pour exporter ou importer en bloc des données SQLXML, utilisez l'un des types de données ci-dessous dans votre fichier de format.  
  
|Type de données|Résultat|  
|---------------|------------|  
|SQLCHAR ou SQLVARYCHAR|Les données sont envoyées dans la page de codes du client ou dans la page de codes impliquée par le classement. L’effet est le même que si vous définissiez le commutateur **-c** sans spécifier de fichier de format.|  
|SQLNCHAR ou SQLNVARCHAR|Les données sont envoyées au format Unicode. L’effet est le même que si vous définissiez le commutateur **-w** sans spécifier de fichier de format.|  
|SQLBINARY ou SQLVARYBIN|Les données sont envoyées sans être converties.|  

## <a name="permissions"></a>Autorisations

 Une opération **bcp out** nécessite l’autorisation SELECT sur la table source.  
  
 Une opération **bcp in** nécessite au minimum des autorisations SELECT/INSERT sur la table cible. En outre, l'autorisation ALTER TABLE est requise si l'une des conditions suivantes est vraie :  
  
- Les contraintes existent et l'indicateur CHECK_CONSTRAINTS n'est pas spécifié.  
  
    > [!NOTE]
    > La désactivation des contraintes est le comportement par défaut. Pour activer les contraintes explicitement, utilisez l’option **-h** avec l’indicateur CHECK_CONSTRAINTS.
  
- Les déclencheurs existent et l'indicateur FIRE_TRIGGER n'est pas spécifié.  
  
    > [!NOTE]
    > Par défaut, les déclencheurs ne sont pas activés. Pour activer les déclencheurs explicitement, utilisez l’option **-h** avec l’indicateur FIRE_TRIGGERS.
  
- Vous devez utiliser l’option **-E** pour importer des valeurs d’identité à partir d’un fichier de données.  
  
> [!NOTE]
> L'autorisation ALTER TABLE obligatoire sur la table cible était nouvelle dans [!INCLUDE[ssVersion2005](../includes/ssversion2005-md.md)]. Les scripts **bcp** qui n’appliquent pas de déclencheurs et de contrôles de contrainte risquent d’échouer si le compte d’utilisateur ne possède pas d’autorisation de table ALTER pour la table cible.

## <a name="character-mode--c-and-native-mode--n-best-practices"></a>Meilleures pratiques relatives au mode caractère (-c) et au mode natif (-n)

 Cette section contient des recommandations pour le mode caractère (-c) et le mode natif (-n).  
  
- (Administrateur/Utilisateur) Lorsque cela est possible, utilisez le format natif (-n) pour éviter le problème de séparateur. Utilisez le format natif pour exporter et importer à l'aide de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Exportez les données à partir de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à l'aide de l'option -c ou -w si les données doivent être importées dans une base de données non-[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
- (Administrateur) Vérifiez les données à l'aide de BCP OUT. Par exemple, lorsque vous utilisez BCP OUT, BCP IN, puis BCP OUT vérifiez que les données sont correctement exportées et que les valeurs de terminateurs ne sont pas utilisées comme parties des valeurs de données. Remplacez les terminateurs par défaut (en utilisant les options -t et -r) par des valeurs hexadécimales aléatoires afin d’éviter les conflits entre les valeurs de terminateurs et les valeurs de données.  
  
- (Utilisateur) Utilisez un terminateur long et unique (n'importe quelle séquence d'octets ou de caractères) pour minimiser les risques de conflits avec la valeur de chaîne actuelle. Cette tâche peut être réalisée à l'aide des options -t et -r.  

## <a name="examples"></a>Exemples

Cette section contient les exemples suivants :

R. Identifier la version de l’utilitaire **bcp**

B. Copie de lignes de table dans un fichier de données (avec une connexion approuvée)

C. Copie de lignes de table dans un fichier de données (avec l'authentification en mode mixte)

D. Copie de données depuis un fichier dans une table

E. Copie d'une colonne spécifique dans un fichier de données

F. Copie d'une ligne spécifique dans un fichier de données

G. Copie de données d'une requête dans un fichier de données

H. Création de fichiers de format

I. Utilisation d’un fichier de format pour une importation en bloc avec **bcp**

J. Spécification d’une page de codes

### <a name="example-test-conditions"></a>**Exemples de conditions de test**

Les exemples ci-dessous utilisent l’exemple de base de données `WideWorldImporters` pour SQL Server (à partir de 2016) et Azure SQL Database.  `WideWorldImporters` peut être téléchargé depuis [https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/wide-world-importers-v1.0).  Consultez [RESTORE (Transact-SQL)](../t-sql/statements/restore-statements-transact-sql.md) pour que la syntaxe restaure la base de données.  Sauf mention spécifique contraire, ces exemples partent du principe que vous utilisez l’authentification Windows et que vous disposez d’une connexion approuvée à l’instance du serveur sur laquelle vous exécutez la commande **bcp** .  Un répertoire nommé `D:\BCP` sera utilisé dans de nombreux exemples.

Le script ci-dessous crée une copie vide de la table `WideWorldImporters.Warehouse.StockItemTransactions` , puis ajoute une contrainte de clé primaire.  Exécuter le script T-SQL suivant dans SQL Server Management Studio (SSMS)

```sql  
USE WideWorldImporters;  
GO  

SET NOCOUNT ON;

IF NOT EXISTS (SELECT * FROM sys.tables WHERE name = 'Warehouse.StockItemTransactions_bcp')
BEGIN
    SELECT * INTO WideWorldImporters.Warehouse.StockItemTransactions_bcp
    FROM WideWorldImporters.Warehouse.StockItemTransactions  
    WHERE 1 = 2;  

    ALTER TABLE Warehouse.StockItemTransactions_bcp 
    ADD CONSTRAINT PK_Warehouse_StockItemTransactions_bcp PRIMARY KEY NONCLUSTERED 
    (StockItemTransactionID ASC);
END
```

> [!NOTE]
> Tronquez la table `StockItemTransactions_bcp` autant que nécessaire.
>
> TRUNCATE TABLE WideWorldImporters.Warehouse.StockItemTransactions_bcp;

### <a name="a--identify-bcp-utility-version"></a>R.  Identifier la version de l’utilitaire **bcp**

À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp -v
```
  
### <a name="b-copying-table-rows-into-a-data-file-with-a-trusted-connection"></a>B. Copie de lignes de table dans un fichier de données (avec une connexion approuvée)

Les exemples suivants illustrent l’utilisation de l’option **out** sur la table `WideWorldImporters.Warehouse.StockItemTransactions`.

- **De base** Cet exemple crée un fichier de données nommé `StockItemTransactions_character.bcp` et y copie les données de table au format **caractère**.

  À partir d'une invite de commandes, entrez la commande suivante :

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Développé** Cet exemple crée un fichier de données nommé `StockItemTransactions_native.bcp` et y copie les données de table à l’aide du format **natif**.  L’exemple spécifie également le nombre maximum d’erreurs de syntaxe, un fichier d’erreurs et un fichier de sortie.

    À partir d'une invite de commandes, entrez la commande suivante :

    ```cmd
    bcp WideWorldImporters.Warehouse.StockItemTransactions OUT D:\BCP\StockItemTransactions_native.bcp -m 1 -n -e D:\BCP\Error_out.log -o D:\BCP\Output_out.log -S -T
    ```

Passez en revue `Error_out.log` et `Output_out.log`.  `Error_out.log` doit être laissé vide.  Comparez les tailles de fichiers entre `StockItemTransactions_character.bcp` et `StockItemTransactions_native.bcp`.

### <a name="c-copying-table-rows-into-a-data-file-with-mixed-mode-authentication"></a>C. Copie de lignes de table dans un fichier de données (avec l'authentification en mode mixte)

L’exemple suivant illustre l’option **out** sur la table `WideWorldImporters.Warehouse.StockItemTransactions` .  Cet exemple crée un fichier de données nommé `StockItemTransactions_character.bcp` et y copie les données de table au format **caractère** .  
  
 L’exemple part du principe que vous utilisez l’authentification en mode mixte ; vous devez utiliser le commutateur **-U** pour spécifier votre ID de connexion. De même, à moins que vous vous connectiez à l’instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur l’ordinateur local, utilisez le commutateur **-S** pour spécifier le nom du système et, éventuellement, un nom d’instance.  

À partir d'une invite de commandes, entrez la commande suivante : \(Le système demande votre mot de passe.\)

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions out D:\BCP\StockItemTransactions_character.bcp -c -U<login_id> -S<server_name\instance_name>
```

### <a name="d-copying-data-from-a-file-to-a-table"></a>D. Copie de données depuis un fichier dans une table

Les exemples suivants illustrent l’option **in** sur la table `WideWorldImporters.Warehouse.StockItemTransactions_bcp` , à l’aide de fichiers créés précédemment.

- **De base** Cet exemple utilise le fichier de données `StockItemTransactions_character.bcp` créé précédemment.

  À partir d'une invite de commandes, entrez la commande suivante :

  ```cmd
  bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_character.bcp -c -T
  ```

- **Développé** Cet exemple utilise le fichier de données `StockItemTransactions_native.bcp` créé précédemment.  En outre, l’exemple utilise l’indicateur **TABLOCK**, spécifie la taille de lot, le nombre maximum d’erreurs de syntaxe, un fichier d’erreurs et un fichier de sortie.
  
À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp IN D:\BCP\StockItemTransactions_native.bcp -b 5000 -h "TABLOCK" -m 1 -n -e D:\BCP\Error_in.log -o D:\BCP\Output_in.log -S -T
```

  Passez en revue `Error_in.log` et `Output_in.log`.

### <a name="e-copying-a-specific-column-into-a-data-file"></a>E. Copie d'une colonne spécifique dans un fichier de données

Pour copier une colonne spécifique, vous pouvez utiliser l’option **queryout** .  L'exemple suivant copie uniquement la colonne `StockItemTransactionID` de la table `Warehouse.StockItemTransactions` dans un fichier de données.
  
À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp "SELECT StockItemTransactionID FROM WideWorldImporters.Warehouse.StockItemTransactions WITH (NOLOCK)" queryout D:\BCP\StockItemTransactionID_c.bcp -c -T
```

### <a name="f-copying-a-specific-row-into-a-data-file"></a>F. Copie d'une ligne spécifique dans un fichier de données

Pour copier une ligne spécifique, vous pouvez utiliser l’option **queryout** . L’exemple suivant copie uniquement la ligne de la personne nommée `Amy Trefl` de la table `WideWorldImporters.Application.People` dans un fichier de données `Amy_Trefl_c.bcp`.  Remarque : le commutateur **-d** sert à identifier la base de données.
  
À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp "SELECT * from Application.People WHERE FullName = 'Amy Trefl'" queryout D:\BCP\Amy_Trefl_c.bcp -d WideWorldImporters -c -T
```

### <a name="g-copying-data-from-a-query-to-a-data-file"></a>G. Copie de données d'une requête dans un fichier de données

Pour copier le jeu de résultats d’une instruction Transact-SQL dans un fichier de données, utilisez l’option **queryout** .  L’exemple suivant copie les noms de la table `WideWorldImporters.Application.People` , triée en fonction du nom, dans le fichier de données `People.txt` .  Remarque : le commutateur **-t** est utilisé pour créer un fichier délimité par des virgules.

À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp "SELECT FullName, PreferredName FROM WideWorldImporters.Application.People ORDER BY FullName" queryout D:\BCP\People.txt -t, -c -T
```

### <a name="h-creating-format-files"></a>H. Création de fichiers de format

L’exemple suivant crée trois fichiers de format distincts pour la table `Warehouse.StockItemTransactions` dans la base de données `WideWorldImporters` .  Passez en revue le contenu de chaque fichier créé.

À partir d’une invite de commandes, entrez les commandes suivantes :

```cmd
REM non-XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.fmt -c -T 

REM non-XML native format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_n.fmt -n -T

REM XML character format
bcp WideWorldImporters.Warehouse.StockItemTransactions format nul -f D:\BCP\StockItemTransactions_c.xml -x -c -T
```

> [!NOTE]
> Pour utiliser le commutateur **-x** , vous devez utiliser un client **bcp** 9.0. Pour plus d’informations sur l’utilisation du client **bcp** 9.0, consultez la section [Notes](#remarks).
  
 Pour plus d’informations consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../relational-databases/import-export/non-xml-format-files-sql-server.md) et [Fichiers de format XML &#40;SQL Server&#41;](../relational-databases/import-export/xml-format-files-sql-server.md).
  
### <a name="i-using-a-format-file-to-bulk-import-with-bcp"></a>I. Utilisation d'un fichier de format pour une importation en bloc avec bcp

Pour utiliser un fichier de format précédemment créé lors de l’importation de données dans une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilisez le commutateur **-f** avec l’option **in** .  Par exemple, la commande suivante copie en bloc le contenu d’un fichier de données, `StockItemTransactions_character.bcp`, dans une copie de la table `Warehouse.StockItemTransactions_bcp` en utilisant le fichier de format précédemment créé, `StockItemTransactions_c.xml`.  Remarque : le commutateur **-L** est utilisé pour importer uniquement les 100 premiers enregistrements.

À partir d'une invite de commandes, entrez la commande suivante :

```cmd
bcp WideWorldImporters.Warehouse.StockItemTransactions_bcp in D:\BCP\StockItemTransactions_character.bcp -L 100 -f D:\BCP\StockItemTransactions_c.xml -T
```

> [!NOTE]
> Les fichiers de format s'avèrent utiles lorsque les champs des fichiers de données diffèrent des colonnes de table ; par exemple, par leur nombre, leur ordre ou leurs types de données. Pour plus d’informations, consultez [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md).  

### <a name="j-specifying-a-code-page"></a>J. Spécification d’une page de codes

L’exemple d’extrait de code suivant illustre l’importation bcp lors de la spécification d’une page de codes 65001 :

```cmd
bcp.exe MyTable in "D:\data.csv" -T -c -C 65001 -t , ...  
```

## <a name="additional-examples"></a>Autres exemples

|Les rubriques suivantes contiennent des exemples de l’utilisation de bcp : |
|---|
|Formats de données pour l'importation en bloc ou l'exportation en bloc (SQL Server)<br />&emsp;&#9679;&emsp;[Utiliser le format natif pour importer ou exporter des données (SQL Server)](../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère pour importer ou exporter des données (SQL Server)](../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format natif Unicode pour importer ou exporter des données (SQL Server)](../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser le format caractère Unicode pour importer ou exporter des données (SQL Server)](../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)<br /><br />[Spécifier des indicateurs de fin de champ et de fin de ligne (SQL Server)](../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)<br /><br />[Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l'importation en bloc (SQL Server)](../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)<br /><br />[Conserver des valeurs d'identité lors de l'importation de données en bloc (SQL Server)](../relational-databases/import-export/keep-identity-values-when-bulk-importing-data-sql-server.md)<br /><br />Fichiers de format pour l’importation ou l’exportation de données (SQL Server)<br />&emsp;&#9679;&emsp;[Créer un fichier de format (SQL Server)](../relational-databases/import-export/create-a-format-file-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour importer des données en bloc (SQL Server)](../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour ignorer un champ de données (SQL Server)](../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)<br />&emsp;&#9679;&emsp;[Utiliser un fichier de format pour mapper les colonnes d’une table aux champs d’un fichier de données (SQL Server)](../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)<br /><br />[Exemples d'importation et d'exportation en bloc de documents XML (SQL Server)](../relational-databases/import-export/examples-of-bulk-import-and-export-of-xml-documents-sql-server.md)<br /><p>  </p>|

## <a name="considerations-and-limitations"></a>Observations et limitations

- L’utilitaire BCP présente une limitation que le message d’erreur affiche uniquement des caractères de 512 octets. Seuls les 512 premiers octets du message d’erreur s’affichent.

## <a name="next-steps"></a>Étapes suivantes

- [Préparer des données en vue d’une exportation ou d’une importation en bloc &#40;SQL Server&#41;](../relational-databases/import-export/prepare-data-for-bulk-export-or-import-sql-server.md)
- [BULK INSERT &#40;Transact-SQL&#41;](../t-sql/statements/bulk-insert-transact-sql.md)
- [OPENROWSET &#40;Transact-SQL&#41;](../t-sql/functions/openrowset-transact-sql.md)
- [SET QUOTED_IDENTIFIER &#40;Transact-SQL&#41;](../t-sql/statements/set-quoted-identifier-transact-sql.md)
- [sp_configure &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)
- [sp_tableoption &#40;Transact-SQL&#41;](../relational-databases/system-stored-procedures/sp-tableoption-transact-sql.md)
- [Fichiers de format pour l’importation ou l’exportation de données &#40;SQL Server&#41;](../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)

[!INCLUDE[get-help-options](../includes/paragraph-content/get-help-options.md)]
