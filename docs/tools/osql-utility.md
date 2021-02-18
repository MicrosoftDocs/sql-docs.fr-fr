---
title: Utilitaire osql
description: Dans SQL Server, l’utilitaire osql vous permet d’entrer des instructions Transact-SQL, des procédures système et des fichiers de script. Pour communiquer avec le serveur, osql utilise ODBC.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- statements [SQL Server], command prompt
- QUIT command
- Transact-SQL statements, command prompt
- EXIT command
- operating systems [SQL Server], commands
- osql utility [SQL Server]
- stored procedures [SQL Server], command prompt
- scripts [SQL Server], command prompt
- RESET command
- GO command
- command prompt utilities [SQL Server], osql
- CTRL+C command
ms.assetid: cf530d9e-0609-4528-8975-ab8e08e40b9a
author: markingmyname
ms.author: maghan
ms.manageR: jroth
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/16/2017
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017'
ms.openlocfilehash: 41c47f76dd0a4b81d1ccc337b65bf1c8c157b770
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335985"
---
# <a name="osql-utility"></a>Utilitaire osql

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

L'utilitaire **osql** permet de spécifier des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] , des procédures système, ainsi que des fichiers de script. Pour communiquer avec le serveur, cet utilitaire fait appel à ODBC.  
  
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une version future de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Évitez de recourir à ce composant dans un nouveau travail de développement et planifiez la modification des applications qui l'utilisent actuellement. Utilisez plutôt **sqlcmd** . Pour plus d'informations, consultez [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
osql  
[-?] |  
[-L] |  
[  
  {  
     {-Ulogin_id [-Ppassword]} | -E }  
     [-Sserver_name[\instance_name]] [-Hwksta_name] [-ddb_name]  
     [-ltime_out] [-ttime_out] [-hheaders]  
     [-scol_separator] [-wcolumn_width] [-apacket_size]  
     [-e] [-I] [-D data_source_name]  
     [-ccmd_end] [-q "query"] [-Q"query"]  
     [-n] [-merror_level] [-r {0 | 1}]  
     [-iinput_file] [-ooutput_file] [-p]  
     [-b] [-u] [-R] [-O]  
]  
```  
  
## <a name="arguments"></a>Arguments  
 **-?**  
 Affiche un résumé de la syntaxe des commutateurs **osql** .  
  
 **-L**  
 Répertorie tous les serveurs configurés localement et les noms des serveurs émettant sur le réseau.  
  
> [!NOTE]  
>  En raison de la nature de la diffusion sur les réseaux, **osql** risque de ne pas recevoir de réponse de tous les serveurs dans les délais impartis. Par conséquent, la liste des serveurs retournée peut varier à chaque invocation de cette option.  
  
 **-U** _login_id_  
 ID de connexion de l'utilisateur. Les ID de connexion respectent la casse.  
  
 **-P** _password_  
 Spécifie le mot de passe pour l'utilisateur. Si l’option **-P** n’est pas utilisée, **osql** invite à entrer un mot de passe. Si l’option **-P** est utilisée à la fin de la ligne de commande sans spécifier de mot de passe, **osql** emploie le mot de passe par défaut (NULL).  
  
> [!IMPORTANT]  
>  N'utilisez pas de mot de passe vide. Utilisez un mot de passe fort. Pour plus d’informations, consultez [Strong Passwords](../relational-databases/security/strong-passwords.md).  
  
 Les mots de passe respectent la casse.  
  
 La variable d'environnement OSQLPASSWORD permet de définir un mot de passe par défaut pour la session en cours. Par conséquent, il est inutile de programmer spécifiquement le mot de passe dans des fichiers de commandes.  
  
 Si vous ne spécifiez pas de mot de passe avec l’option **-P** , **osql** commence par rechercher la variable OSQLPASSWORD. Si aucune valeur n'est définie, **osql** utilise le mot de passe par défaut (NULL). L'exemple suivant définit la variable OSQLPASSWORD dans une invite de commandes et accède ensuite à l'utilitaire **osql** :  
  
```  
C:\>SET OSQLPASSWORD=abracadabra  
C:\>osql   
```  
  
> [!IMPORTANT]  
>  Pour masquer votre mot de passe, ne spécifiez pas l’option **-P** avec l’option **-U** . À la place, appuyez sur Entrée après avoir spécifié **osql** avec l’option **-U** et d’autres commutateurs (ne spécifiez pas **-P**). **osql** vous demandera d’entrer un mot de passe. Cette méthode garantit le masquage de votre mot de passe lors de son entrée.  
  
 **-E**  
 Utilise une connexion approuvée au lieu de demander un mot de passe.  
  
 **-S** _server\_name_[ **\\** _instance\_name_]  
 Spécifie l'instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] à laquelle établir une connexion. Spécifiez *server_name* pour vous connecter à l’instance par défaut du [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur ce serveur. Spécifiez _server\_name_ **\\** _instance\_name_ pour vous connecter à une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur ce serveur. Si aucun serveur n'est spécifié, **osql** se connecte à l'instance par défaut de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] sur l'ordinateur local. Cette option est indispensable lorsque vous exécutez **osql** à partir d'un ordinateur distant connecté au réseau.  
  
 **-H** _wksta_name_  
 Nom d'une station de travail. Le nom de la station de travail est stocké dans **sysprocesses.hostname** et est affiché par **sp_who**. Si cette option n'est pas spécifiée, le nom d'ordinateur en cours est utilisé par défaut.  
  
 **-d** _db_name_  
 Émet une instruction USE *db_name* au démarrage d’ **osql**.  
  
 **-l** _time_out_  
 Spécifie le nombre de secondes avant expiration du délai de connexion à **osql** . Le délai d’attente par défaut pour la connexion à **osql** est de huit secondes.  
  
 **-t** _time_out_  
 Spécifie le nombre de secondes accordées pour l'exécution d'une commande. Si aucune valeur *time_out* n’est spécifiée, les commandes n’ont pas de délai d’expiration.  
  
 **-h** _headers_  
 Spécifie le nombre de lignes à imprimer entre les en-têtes de colonne. Par défaut, les en-têtes ne sont imprimés qu'une fois pour chaque jeu de résultats d'une requête. Utilisez -1 pour indiquer qu'aucun titre ne sera imprimé. Si vous utilisez -1, ne laissez aucun espace entre le paramètre et sa valeur ( **-h-1**, et non **-h -1**).  
  
 **-s** _col_separator_  
 Spécification du caractère de séparation des colonnes, qui est par défaut un espace. Pour utiliser des caractères qui présentent une signification particulière pour le système d'exploitation (par exemple, | ; & < >), mettez-les entre guillemets doubles (").  
  
 **-w** _column_width_  
 Permet à l'utilisateur de définir la largeur d'écran des sorties. La valeur par défaut est de 80 caractères. Lorsqu'une ligne de sortie a atteint la largeur d'écran maximale, elle est scindée en plusieurs lignes.  
  
 **-a** _packet_size_  
 Spécifie le taille des paquets. Les valeurs correctes pour *packet_size* sont comprises entre 512 et 65535. La valeur **osql** par défaut est la valeur par défaut du serveur. Une plus grande taille de paquet permet d'améliorer les performances lors de l'exécution de scripts plus volumineux, où la quantité d'instructions SQL entre les commandes GO est substantielle. [!INCLUDE[msCoName](../includes/msconame-md.md)] indiquent que la valeur 8192 représente généralement le réglage le plus rapide pour les opérations de copie en bloc. Une taille de paquet supérieure peut être demandée, mais **osql** prend la valeur par défaut du serveur si la requête ne peut pas être satisfaite.  
  
 **-e**  
 Retourne les données d'entrée.  
  
 **-I**  
 Active l'option de connexion QUOTED_IDENTIFIER.  
  
 **-D** _data_source_name_  
 Établit la connexion à une source de données ODBC définie à l'aide du pilote ODBC de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. La connexion **osql** utilise les options spécifiées dans la source de données.  
  
> [!NOTE]  
>  Cette option ne fonctionne pas avec les sources de données définies pour les autres pilotes.  
  
 **-c** _cmd_end_  
 Spécifie l'indicateur de fin de commande. Par défaut, il faut entrer la commande GO sur une ligne isolée pour terminer une commande et la soumettre à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Si vous changez d'indicateur de fin de commande, n'utilisez ni les mots réservés [!INCLUDE[tsql](../includes/tsql-md.md)] ni les caractères ayant une signification particulière pour le système d'exploitation, qu'ils soient ou non précédés d'une barre oblique inverse  
  
 **-q "** _query_ **"**  
 Exécute une requête au démarrage d’ **osql** , mais ne quitte pas **osql** à l’issue de la requête. (Notez que la requête ne doit pas comporter d'instruction GO). Si vous exécutez une requête à partir d'un fichier de commandes, vous pouvez utiliser %variable ou %variable d'environnement%. Par exemple :  
  
```  
SET table=sys.objects  
osql -E -q "select name, object_id from %table%"  
```  
  
 Placez le nom de la requête entre guillemets doubles et tout élément imbriqué dans la requête entre guillemets simples.  
  
 **-Q"** _query_ **"**  
 Exécute une requête, puis quitte immédiatement **osql**. Placez le nom de la requête entre guillemets doubles et tout élément imbriqué dans la requête entre guillemets simples.  
  
 **-n**  
 Supprime la numérotation et le symbole de ligne de commande (>) des lignes d'entrée.  
  
 **-m** _error_level_  
 Personnalise l'affichage des messages d'erreur. Le numéro du message, son état et son niveau d'erreur sont affichés pour les erreurs atteignant ou dépassant le niveau de gravité indiqué. Aucune information n'est affichée pour les erreurs d'une gravité inférieure au niveau indiqué. Utilisez **-1** pour afficher tous les en-têtes retournés avec les messages, même s’il s’agit de messages d’information. Si vous utilisez **-1**, ne laissez aucun espace entre le paramètre et sa valeur ( **-m-1**, et non **-m -1**).  
  
 **-r** { **0**| **1**}  
 Redirige la sortie des messages à l’écran (**stderr**). Si vous n'indiquez aucun paramètre ou si vous spécifiez la valeur **0**, seuls les messages d'erreur de gravité égale ou supérieure à 11 sont redirigés. Si vous indiquez la valeur **1**, tous les messages émis (y compris les messages d’impression) sont redirigés.  
  
 **-i** _input_file_  
 Identifie le fichier contenant un traitement d'instructions SQL ou des procédures stockées. L’opérateur de comparaison inférieur à ( **\<** ) peut être utilisé à la place de **-i**.  
  
 **-o** _output_file_  
 Identifie le fichier recevant une sortie de **osql**. L’opérateur de comparaison supérieur à ( **>** ) peut être utilisé à la place de **-o**.  
  
 Si *input_file* n’est pas au format Unicode et si **-u** n’est pas spécifié, *output_file* est enregistré au format OEM. Si *input_file* est au format Unicode ou si **-u** est spécifié, *output_file* est stocké au format Unicode.  
  
 **-p**  
 Affiche les statistiques sur les performances.  
  
 **-b**  
 Spécifie que **osql** prend fin et retourne une valeur DOS ERRORLEVEL quand une erreur se produit. La valeur retournée à la variable DOS ERRORLEVEL est 1 lorsque le message d'erreur de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] possède une gravité égale ou supérieure à 11, sinon la valeur retournée est 0. [!INCLUDE[msCoName](../includes/msconame-md.md)] Les fichiers de commande MS-DOS peuvent tester la valeur de DOS ERRORLEVEL et traiter l’erreur d’une manière appropriée.  
  
 **-u**  
 Spécifie qu’ *output_file* est stocké au format Unicode, quel que soit le format d’ *input_file*.  
  
 **-R**  
 Spécifie l’utilisation par le pilote ODBC de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] des paramètres du client pour convertir monnaie, date et heure en données caractères.  
  
 **-O**  
 Spécifie que certaines fonctionnalités d’ **osql** sont désactivées pour assurer la continuité avec des versions antérieures d’ **isql**. Les fonctionnalités suivantes sont désactivées :  
  
-   Traitement de lot EOF  
  
-   Mise à l'échelle automatique de la largeur de la console  
  
-   Messages larges  
  
 Il attribue aussi la valeur par défaut -1 à DOS ERRORLEVEL.  
  
> [!NOTE]  
>  Les options **-n**, **-O** et **-D** ne sont plus prises en charge par **osql**.  
  
## <a name="remarks"></a>Notes  
 L’utilitaire **osql** doit être exécuté directement à partir du système d’exploitation à l’aide des options respectant la casse énumérées ici. Une fois **osql** démarré, il accepte les instructions SQL et les envoie de manière interactive à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les résultats sont mis en forme et affichés à l’écran (**stdout**). Pour quitter **osql**, utilisez QUIT ou EXIT.  
  
 Si vous ne spécifiez pas de nom d’utilisateur quand vous démarrez **osql**, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] vérifie les variables d’environnement et les utilise, par exemple, **osqluser=(** _user_ **)** ou **osqlserver=(** _server_ **)** . Si aucune variable d'environnement n'est définie, le nom d'utilisateur du poste de travail est utilisé. Si vous n'indiquez pas de serveur, le nom du poste de travail est utilisé.  
  
 Si aucune des options **-U** et **-P** n’est utilisée, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] tente de se connecter à l’aide du mode d’authentification [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows. L'authentification est basée sur le compte [!INCLUDE[msCoName](../includes/msconame-md.md)] Windows de l'utilisateur exécutant **osql**.  
  
 L'utilitaire **osql** utilise l'API ODBC. Cet utilitaire emploie les paramètres par défaut du pilote ODBC [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] pour les options de connexion ISO de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Pour plus d'informations, consultez « Effets des options ANSI ».  
  
> [!NOTE]  
>  L’utilitaire **osql** ne prend pas en charge les types de données CLR définis par l’utilisateur. Pour traiter ces types de données, vous devez employer l'utilitaire **sqlcmd** . Pour plus d'informations, consultez [sqlcmd Utility](../tools/sqlcmd-utility.md).  
  
## <a name="osql-commands"></a>Commandes OSQL  
 En plus des instructions [!INCLUDE[tsql](../includes/tsql-md.md)] dans **osql**, les commandes ci-dessous sont également disponibles.  
  
|Commande|Description|  
|-------------|-----------------|  
|GO|Exécute toutes les commandes entrées après le dernier GO.|  
|RESET|Efface toutes les instructions que vous avez entrées.|  
|QUIT ou EXIT( )|Quitte **osql**.|  
|CTRL+C|Termine une requête sans quitter **osql**.|  
  
> [!NOTE]  
>  Les commandes !! et ED ne sont plus prises en charge par **osql**.  
  
 Les indicateurs de fin de commande, GO (par défaut), RESET EXIT, QUIT et CTRL+C ne sont reconnus que s’ils apparaissent au début d’une ligne immédiatement après l’invite **osql** .  
  
 GO indique la fin d'un traitement et l'exécution des commandes [!INCLUDE[tsql](../includes/tsql-md.md)] placées dans le cache. Quand vous appuyez sur ENTRÉE à la fin d'une ligne d'entrée, **osql** place les instructions de cette ligne dans le cache. Lorsque vous appuyez sur ENTRÉE après avoir tapé GO, toutes les instructions en cache sont envoyées en tant que traitement à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)].  
  
 L'utilitaire **osql** actuel fonctionne comme si une instruction GO implicite terminait chaque script exécuté, de sorte que toutes les instructions du script sont exécutées.  
  
 Pour terminer une commande, tapez une ligne commençant par un indicateur de fin de commande. Vous pouvez faire suivre cet indicateur de fin de commande d'un nombre entier pour indiquer le nombre d'exécutions de la commande. Par exemple, pour exécuter cette commande 100 fois, entrez :  
  
```  
SELECT x = 1  
GO 100  
```  
  
 Les résultats sont imprimés une fois à la fin de l'exécution. **osql** n'accepte pas plus de 1 000 caractères par ligne. Les instructions de grande taille doivent être scindées en plusieurs lignes.  
  
 La fonction de rappel de commande de Windows peut servir à rappeler et à modifier des instructions **osql** . Le tampon de requête en cours peut être vidé en tapant RESET.  
  
 Lors de l'exécution de procédures stockées, **osql** imprime une ligne vide entre chaque jeu de résultats d'un lot. En outre, le message « 0 ligne affectée » ne s'affiche pas lorsqu'il ne concerne pas l'instruction exécutée.  
  
## <a name="using-osql-interactively"></a>Utilisation interactive de osql  
 Pour utiliser **osql** en mode interactif, entrez la commande **osql** (et les options désirées) après l’invite de commandes du système d’exploitation.  
  
 Vous pouvez exécuter dans **osql** la requête contenue dans un fichier (tel que Stores.qry) en entrant une commande de ce type :  
  
```  
osql -E -i stores.qry  
```  
  
 Vous pouvez lire un fichier contenant une requête (tel que Titles.qry) et rediriger les résultats vers un autre fichier en entrant une commande de ce type :  
  
```  
osql -E -i titles.qry -o titles.res  
```  
  
> [!IMPORTANT]  
>  Quand cela est possible, utilisez l’option **-E**(connexion approuvée).  
  
 Si vous utilisez **osql** en mode interactif, vous pouvez lire un fichier du système d’exploitation dans le tampon de commandes en entrant **:r**_nom\_fichier_. Cette opération adresse le script SQL qui se trouve dans *nom_fichier* directement au serveur en un seul traitement.  
  
> [!NOTE]  
>  Quand **osql** est utilisé, [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] traite le délimiteur de lot « GO » comme une erreur de syntaxe s’il apparaît dans un fichier de script SQL.  
  
## <a name="inserting-comments"></a>Insertion de commentaires  
 Vous pouvez inclure des commentaires dans une instruction Transact-SQL soumise à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] par **osql**. Deux styles de commentaires sont autorisés : `--` et `/*...*/`.  
  
## <a name="using-exit-to-return-results-in-osql"></a>Utilisation d'EXIT pour retourner des résultats dans osql  
 Vous pouvez utiliser le résultat d'une instruction SELECT comme valeur retournée par **osql**. S'il est numérique, la dernière colonne de la dernière ligne de résultats est convertie en entier de 4 octets (entier long). MS-DOS transmet l'octet de poids faible au processus parent ou au niveau erreur du système d'exploitation. Windows transmet la totalité de l'entier de 4 octets. La syntaxe est :  
  
```  
EXIT ( < query > )  
```  
  
 Par exemple :  
  
```  
EXIT(SELECT @@ROWCOUNT)  
```  
  
 Vous pouvez également inclure le paramètre EXIT dans un fichier de commandes. Par exemple :  
  
```  
osql -E -Q "EXIT(SELECT COUNT(*) FROM '%1')"  
```  
  
 L’utilitaire **osql** transmet au serveur toutes les informations placées entre parenthèses **()** telles qu’elles ont été entrées. Si une procédure système stockée sélectionne un ensemble et retourne une valeur, seule la sélection est retournée. L’instruction **()** sans information entre parenthèses exécute toutes les commandes qui la précèdent dans le traitement, puis quitte l’utilitaire sans retourner de valeur.  
  
 Il existe quatre formats de sortie :  
  
-   EXIT  
  
> [!NOTE]  
>  N'exécute pas le traitement ; ferme immédiatement l'utilitaire et ne retourne aucune valeur.  
  
-   EXIT **()**  
  
> [!NOTE]  
>  Exécute le traitement, puis quitte sans retourner de valeur.  
  
-   EXIT **(** _requête_ **)**  
  
> [!NOTE]  
>  Exécute le traitement, y compris la requête, puis quitte en retournant les résultats de la requête.  
  
-   RAISERROR avec une gravité de 127  
  
> [!NOTE]  
>  Si RAISERROR est utilisé dans un script **osql** et qu'une erreur de gravité 127 se produit, l'exécution d’ **osql** se termine et l'ID du message est retourné au client. Par exemple :  
  
```  
RAISERROR(50001, 10, 127)  
```  
  
 Cette erreur entraîne l'arrêt de l'exécution du script **osql** et envoie le message 50001 au client.  
  
 Les valeurs retournées comprises entre -1 et -99 sont réservées à [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. **osql** définit les valeurs suivantes :  
  
-   -100  
  
     Erreur rencontrée avant la sélection d'une valeur retournée.  
  
-   -101  
  
     Aucune ligne trouvée lors de la sélection d'une valeur retournée.  
  
-   -102  
  
     Erreur de conversion survenue lors de la sélection d'une valeur retournée.  
  
## <a name="displaying-money-and-smallmoney-data-types"></a>Affichage des types de données money et smallmoney  
 **osql** affiche les types de données **money** et **smallmoney** avec deux décimales, bien que [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] les stocke en interne avec quatre décimales. Prenons cet exemple :  
  
```  
SELECT CAST(CAST(10.3496 AS money) AS decimal(6, 4))  
GO  
```  
  
 Cette instruction produit le résultat `10.3496`, ce qui indique que la valeur est bien stockée avec toutes ses décimales.  
  
## <a name="see-also"></a>Voir aussi  
 [Commentaire &#40;MDX&#41;](../mdx/comment-mdx.md)   
 [-- &#40;Comment&#41; &#40;MDX&#41;](../mdx/comment-mdx-operator-reference.md)   
 [CAST et CONVERT &#40;Transact-SQL&#41;](../t-sql/functions/cast-and-convert-transact-sql.md)   
 [RAISERROR &#40;Transact-SQL&#41;](../t-sql/language-elements/raiserror-transact-sql.md)  
  
  
