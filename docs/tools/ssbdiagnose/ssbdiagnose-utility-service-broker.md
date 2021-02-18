---
title: Utilitaire ssbdiagnose (Service Broker)
description: L’utilitaire ssbdiagnose signale des problèmes dans des conversations Service Broker ou dans la configuration des services Service Broker.
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- Service Broker, runtime reports
- Service Broker, command prompt utilities
- troubleshooting [Service Broker], conversations
- troubleshooting [Service Broker], configurations
- command prompt utilities [Service Broker]
- Service Broker, troubleshooting
- Service Broker, configuration reports
- Service Broker, tools
- troubleshooting [Service Broker], runtime
- conversations [Service Broker], troubleshooting
- troubleshooting [Service Broker], ssbdiagnose utility
- tools [Service Broker], ssbdiagnose
- Service Broker, ssbdiagnose utility
- ssbdiagnose
ms.assetid: 0c1636e8-a3db-438e-be4c-1ea40d1f4877
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.openlocfilehash: a5001821d5a396f65ee739341424a2eaae3dad78
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335894"
---
# <a name="ssbdiagnose-utility-service-broker"></a>Utilitaire ssbdiagnose (Service Broker)

[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]

L’utilitaire **ssbdiagnose** signale des problèmes rencontrés dans des conversations [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou dans la configuration des services [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Des vérifications de configuration peuvent réalisées pour deux services ou pour un seul. Les problèmes sont signalés soit dans la fenêtre d’invite de commandes par un texte explicite, soit dans un fichier XML mis en forme qui peut être redirigé vers un fichier ou un autre programme.

## <a name="syntax"></a>Syntaxe  
  
```  
  
ssbdiagnose   
[ [ -XML ]  
    [ -LEVEL { ERROR | WARNING | INFO } ]  
  [-IGNORE error_id ] [ ...n]  
    [ <baseconnectionoptions> ]  
  { <configurationreport> | <runtimereport> }  
]  
| -?  
  
<configurationreport> ::=  
    CONFIGURATION  
  { [ FROM SERVICE service_name  
      [ <fromconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
    [ TO SERVICE service_name[, broker_id ]  
      [ <toconnectionoptions> ]  
      [ MIRROR <mirrorconnectionoptions> ]  
    ]  
  }  
    ON CONTRACT contract_name  
  [ ENCRYPTION { ON | OFF | ANONYMOUS } ]  
  
<runtime_report> ::=  
    RUNTIME  
    [-SHOWEVENTS ]  
        [ -NEW  
         [ -ID { conversation_handle  
                | conversation_group_id  
                 | conversation_id  
                  }  
        ] [ ...n]  
        ]  
    [ -TIMEOUT timeout_interval ]  
    [ <runtimeconnectionoptions> ]  
  
<baseconnectionoptions> ::=  
  <connectionoptions>  
  
<fromconnectionoptions> ::=  
  <connectionoptions>  
  
<toconnectionoptions> ::=  
  <connectionoptions>  
  
<mirrorconnectionoptions> ::=  
  <connectionoptions>  
  
<runtimeconnectionoptions> ::=  
  [ CONNECT TO <connectionoptions> ] [ ...n]  
  
<connectionoptions> ::=  
    [ -E | { -U login_id [ -P password ] } ]  
  [ -S server_name[\instance_name] ]  
  [ -d database_name ]  
  [ -l login_timeout ]  
  
```  
  
## <a name="command-line-options"></a>Options de la ligne de commande  
 **-XML**  
 Spécifie que la sortie de **ssbdiagnose** doit être générée au format XML. Ce fichier peut être redirigé vers un fichier ou une autre application. Si l’option **-XML** n’est pas spécifiée, la sortie de **ssbdiagnose** se présente sous forme de texte explicite.  
  
 **-LEVEL** { **ERROR** | **WARNING** | **INFO**}  
 Spécifie le niveau des messages à signaler.  
  
 **ERROR**: signaler uniquement les messages d’erreur.  
  
 **WARNING**: signaler les messages d’erreur et d’avertissement.  
  
 **INFO**: signaler les messages d’erreur, d’avertissement et d’information.  
  
 Le paramètre par défaut est **WARNING**.  
  
 **-IGNORE** _error_id_  
 Spécifie que les erreurs ou les messages ayant l’ *ID_erreur* spécifié ne doivent pas être inclus dans les rapports. Vous pouvez spécifier **-IGNORE** plusieurs fois pour supprimer plusieurs ID de message.  
  
 **\<baseconnectionoptions>**  
 Spécifie les informations de connexion de base utilisées par **ssbdiagnose** quand les options de connexion ne sont pas incluses dans une clause spécifique. Les informations de connexion indiquées dans une clause spécifique remplacent celles figurant dans **baseconnectionoption** . Ceci est effectué individuellement pour chaque paramètre. Par exemple, si les options **-S** et **-d** sont spécifiées dans **baseconnetionoptions**, et si seule l’option **-d** est spécifiée dans **oconnetionoptions**, **ssbdiagnose** utilise -S de **baseconnetionoptions** et -d de **toconnetionoptions**.  
  
 **CONFIGURATION**  
 Demande un rapport des erreurs de configuration entre une paire de services [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou pour un service unique.  
  
 **FROM SERVICE** _service_name_  
 Spécifie le service qui lance les conversations.  
  
 **\<fromconnectionoptions>**  
 Spécifie les informations requises pour se connecter à la base de données contenant le service initiateur. Si **fromconnectionoptions** n’est pas spécifié, **ssbdiagnose** utilise les informations de connexion de **baseconnectionoptions** pour se connecter à la base de données de l’initiateur. Si **fromconnectionoptions** est spécifié, la base de données contenant le service initiateur doit y être indiquée. Si **fromconnectionoptions** n’est pas spécifié, **baseconnectionoptions** doit spécifier la base de données de l’initiateur.  
  
 **TO SERVICE** _service_name_[, *broker_id* ]  
 Spécifie le service qui est la cible des conversations.  
  
 *nom_service*: spécifie le nom du service cible.  
  
 *ID_broker*: spécifie l’ID [!INCLUDE[ssSB](../../includes/sssb-md.md)] qui identifie la base de données cible. *ID_broker* est un GUID. Vous pouvez exécuter la requête suivante dans la base de données cible pour le rechercher :  
  
```  
SELECT service_broker_guid  
FROM sys.databases  
WHERE database_id = DB_ID();  
```  
  
 **\<toconnectionoptions>**  
 Spécifie les informations requises pour connecter la base de données contenant le service cible. Si **toconnectionoptions** n’est pas spécifié, **ssbdiagnose** utilise les informations de connexion de **baseconnectionoptions** pour se connecter à la base de données cible.  
  
 **MIRROR**  
 Spécifie que le service [!INCLUDE[ssSB](../../includes/sssb-md.md)] associé est hébergé par une base de données miroir. **ssbdiagnose** vérifie si l’itinéraire du service est un itinéraire mis en miroir, où la clause MIRROR_ADDRESS est spécifiée sur CREATE ROUTE.  
  
 **\<mirrorconnectionoptions>**  
 Spécifie les informations requises pour se connecter à la base de données miroir. Si **mirrorconnectionoptions** n’est pas spécifié, **ssbdiagnose** utilise les informations de connexion de **baseconnectionoptions** pour se connecter à la base de données miroir.  
  
 **ON CONTRACT** _contract_name_  
 Demande que **ssbdiagnose** vérifie uniquement les configurations qui utilisent le contrat spécifié. Si ON CONTRACT n’est pas spécifié, **ssbdiagnose** effectue un rapport sur le contrat nommé DEFAULT.  
  
 **ENCRYPTION** { **ON** | **OFF** | **ANONYMOUS** }  
 Demande que la configuration du dialogue soit vérifiée pour le niveau de chiffrement spécifié :  
  
 **ON** : Paramètre par défaut. La sécurité de dialogue complète est configurée. Des certificats ont été déployés des deux côtés du dialogue, une liaison de service distant est présente et l'instruction GRANT SEND du service cible a spécifié l'utilisateur initiateur.  
  
 **OFF** : Aucune sécurité de dialogue n'est configurée. Aucun certificat n’a été déployé, aucune liaison de service distant n’a été créée, et l’instruction GRANT SEND du service initiateur a spécifié le rôle **public** .  
  
 **ANONYMOUS** : La sécurité de dialogue anonyme est configurée. Un certificat a été déployé, la liaison de service distant a spécifié la clause ANONYMOUS, et l’instruction GRANT SEND du service cible a spécifié le rôle **public** .  
  
 **RUNTIME**  
 Demande un rapport sur les problèmes à l'origine d'erreurs d'exécution pour une conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Si ni **-NEW** ni **-ID** n’est spécifiée, **ssbdiagnose** surveille toutes les conversations dans toutes les bases de données spécifiées dans les options de connexion. Si l’option **-NEW** ou **-ID** est spécifiée, **ssbdiagnose** génère la liste des ID spécifiés dans les paramètres.  
  
 Pendant son exécution, **ssbdiagnose** enregistre tous les événements [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] qui indiquent des erreurs d’exécution. Il enregistre les événements qui se produisent pour les ID spécifiés, ainsi que les événements au niveau système. Si des erreurs d’exécution sont rencontrées, **ssbdiagnose** exécute un rapport de configuration sur la configuration associée.  
  
 Par défaut, les erreurs d'exécution ne sont pas incluses dans le rapport de sortie, seuls les résultats de l'analyse de la configuration le sont. Utilisez **-SHOWEVENTS** pour inclure les erreurs d’exécution dans le rapport.  
  
 **-SHOWEVENTS**  
 Spécifie que **ssbdiagnose** doit signaler les événements [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] dans le cadre d’un rapport RUNTIME. Seuls les événements considérés comme des conditions d'erreur sont signalés. Par défaut, **ssbdiagnose** ne fait que surveiller les événements d’erreurs. Il ne les signale pas dans la sortie.  
  
 **-NEW**  
 Demande le contrôle d’exécution de la première conversation qui commence après le démarrage de **ssbdiagnose** .  
  
 **-ID**  
 Demande que les éléments de conversation spécifiés fassent l'objet d'une surveillance au moment de l'exécution. Vous pouvez spécifier **-ID** plusieurs fois.  
  
 Si vous spécifiez un descripteur de conversation, seuls les événements associés au point de terminaison de conversation associé sont signalés. Si vous spécifiez un ID de conversation, tous les événements de cette conversation et de ses points de terminaison initiateur et cible sont signalés. Si un ID de groupe de conversations est spécifié, tous les événements de toutes les conversations et de tous les points de terminaison du groupe de conversations sont signalés.  
  
 *conversation_handle*  
 Identificateur unique qui identifie un point de terminaison de conversation dans une application. Les descripteurs de conversation sont uniques à un point de terminaison d'une conversation, les points de terminaison initiateur et cible ont des descripteurs de conversation distincts.  
  
 Les descripteurs de conversation sont retournés aux applications par le paramètre *\@dialog_handle* de l’instruction **BEGIN DIALOG**, et la colonne **conversation_handle** dans le jeu de résultats d’une instruction **RECEIVE**.  
  
 Les descripteurs de conversation sont signalés dans la colonne **conversation_handle** des affichages catalogue **sys.transmission_queue** et **sys.conversation_endpoints** .  
  
 *conversation_group_id*  
 Identificateur unique qui identifie un groupe de conversations.  
  
 Les ID de groupe de conversations sont retournés aux applications par le paramètre *\@conversation_group_id* de l’instruction **GET CONVERSATION GROUP**, et la colonne **conversation_group_id** dans le jeu de résultats d’une instruction **RECEIVE**.  
  
 Les ID de groupe de conversations sont signalés dans les colonnes **conversation_group_id** des affichages catalogue **sys.conversation_groups** et **sys.conversation_endpoints** .  
  
 *conversation_id*  
 Identificateur unique qui identifie une conversation. Les ID de conversation sont les mêmes pour les points de terminaison initiateur et cible d'une conversation.  
  
 Les ID de conversation sont signalés dans la colonne **conversation_id** de l’affichage catalogue **sys.conversation_endpoints** .  
  
 **-TIMEOUT** _timeout_interval_  
 Spécifie la durée d’exécution du rapport **RUNTIME** en secondes. Si vous ne spécifiez pas **-TIMEOUT** , le rapport d’exécution s’exécute indéfiniment. **-TIMEOUT** est utilisé uniquement sur les rapports **RUNTIME** , et non sur les rapports **CONFIGURATION** . Utilisez Ctrl+C pour quitter **ssbdiagnose** si **-TIMEOUT** n’a pas été spécifié, ou pour terminer un rapport d’exécution avant **-** l’expiration du délai d’attente. *intervalle_délai_d’attente* doit être un nombre compris entre 1 et 2 147 483 647.  
  
 **\<runtimeconnectionoptions>**  
 Spécifie les informations de connexion pour les bases de données contenant les services associés aux éléments de conversation qui sont surveillés. Si tous les services se trouvent dans la même base de données, vous ne devez spécifier qu’une seule clause **CONNECT TO** . Si les services se trouvent dans des bases de données séparées, vous devez fournir une clause **CONNECT TO** pour chaque base de données. Si **runtimeconnectionoptions** n’est pas spécifié, **ssbdiagnose** utilise les informations de connexion de **baseconnectionoptions**.  
  
 **-E**  
 Ouvrir une connexion par le biais de l’authentification Windows à une instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] en utilisant votre compte Windows actuel comme ID de connexion. La connexion doit être membre du rôle serveur fixe **sysadmin** .  
  
 L'option -E ignore les paramètres d'utilisateur et de mot de passe des variables d'environnement SQLCMDUSER et SQLCMDPASSWORD.  
  
 Si ni l’option **-E** ni l’option **-U** n’est spécifiée, **ssbdiagnose** utilise la valeur de la variable d’environnement SQLCMDUSER. Si cette variable n’est pas définie non plus, **ssbdiagnose** utilise l’authentification Windows.  
  
 Si l’option **-E** est utilisée avec l’option **-U** ou **-P** , un message d’erreur est généré.  
  
 **-U** _login_id_  
 Ouvrir une connexion par le biais de l’authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] en utilisant l’ID de connexion spécifié. La connexion doit être membre du rôle serveur fixe **sysadmin** .  
  
 Si ni l’option **-E** ni l’option **-U** n’est spécifiée, **ssbdiagnose** utilise la valeur de la variable d’environnement SQLCMDUSER. Si cette variable n’est pas définie non plus, **ssbdiagnose** essaie de se connecter en utilisant le mode d’authentification Windows sur la base du compte Windows de l’utilisateur qui exécute **ssbdiagnose**.  
  
 Si l’option **-U** est utilisée avec l’option **-E** , un message d’erreur est généré. Si l’option **-U** est suivie de plusieurs arguments, un message d’erreur est généré et le programme se termine.  
  
 **-P** _password_  
 Spécifie le mot de passe de l’ID de connexion **-U** . Les mots de passe respectent la casse. Si vous utilisez l’option **-U** mais pas l’option **-P** , **ssbdiagnose** utilise la valeur de la variable d’environnement SQLCMDPASSWORD. Si cette variable n’est pas définie non plus, **ssbdiagnose** invite l’utilisateur à entrer un mot de passe.  
  
> [!IMPORTANT]  
>  Lorsque vous tapez une commande SET SQLCMDPASSWORD, votre mot de passe est visible par quiconque regarde votre moniteur.  
  
 Si l’option **-P** est spécifiée sans mot de passe, **ssbdiagnose** utilise le mot de passe par défaut (NULL).  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssNoteStrongPass](../../includes/ssnotestrongpass-md.md)]Pour plus d’informations, consultez [Mots de passe forts](../../relational-databases/security/strong-passwords.md).  
  
 L’invite de mot de passe s’affiche en imprimant l’invite de commande sur la console, comme suit : `Password:`  
  
 L'entrée de l'utilisateur est masquée, ce qui signifie que rien ne s'affiche et que le curseur reste immobile.  
  
 Si l’option **-P** est utilisée avec l’option **-E** , un message d’erreur est généré.  
  
 Si l’option **-P** est suivie de plusieurs arguments, un message d’erreur est généré.  
  
 **-S** _server_name_[\\*instance_name*]  
 Spécifie l’instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)] qui contient les services [!INCLUDE[ssSB](../../includes/sssb-md.md)] à analyser.  
  
 Spécifiez *nom_serveur* pour vous connecter à l’instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur ce serveur. Spécifiez _server\_name_ **\\** _instance\_name_ pour vous connecter à une instance nommée de [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur ce serveur. Si vous ne spécifiez pas l’option **-S** , **ssbdiagnose** utilise la valeur de la variable d’environnement SQLCMDSERVER. Si cette variable n’est pas définie non plus, **ssbdiagnose** se connecte à l’instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)] sur l’ordinateur local.  
  
 **-d** _database_name_  
 Spécifie la base de données qui contient les services [!INCLUDE[ssSB](../../includes/sssb-md.md)] à analyser. Si cette base de données n'existe pas, un message d'erreur est généré. Si vous ne spécifiez pas l’option **-d** , la valeur par défaut est la base de données spécifiée dans la propriété de base de données par défaut de votre connexion.  
  
 **-l** _login_timeout_  
 Spécifie le délai d'attente d'une tentative de connexion à un serveur (en secondes). Si vous ne spécifiez pas l’option **-l** , **ssbdiagnose** utilise la valeur définie pour la variable d’environnement SQLCMDLOGINTIMEOUT. Si cette variable n'est pas définie non plus, le délai d'attente par défaut est de trente secondes. Le délai d'attente de la connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n’est pas numérique ou n’est pas comprise dans cet intervalle, **ssbdiagnose** génère un message d’erreur. Une valeur de 0 spécifie un délai d'attente infini.  
  
 **-?**  
 Affiche l'aide de la ligne de commande.  
  
## <a name="remarks"></a>Notes  
 Utilisez **ssbdiagnose** pour effectuer les opérations suivantes :  
  
-   Confirmer qu'une application [!INCLUDE[ssSB](../../includes/sssb-md.md)] récemment configurée ne contient pas d'erreurs de configuration.  
  
-   Confirmer l'absence d'erreurs de configuration après modification de la configuration d'une application [!INCLUDE[ssSB](../../includes/sssb-md.md)] existante.  
  
-   Confirmer l'absence d'erreurs de configuration après qu'une base de données [!INCLUDE[ssSB](../../includes/sssb-md.md)] a été détachée puis rattachée à une nouvelle instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Rechercher d'éventuelles erreurs de configuration lorsque des messages ne sont pas transmis avec succès entre des services.  
  
-   Obtenir un rapport sur toute erreur se produisant dans un jeu d'éléments de conversation [!INCLUDE[ssSB](../../includes/sssb-md.md)] .  
  
## <a name="configuration-reporting"></a>Rapport de configuration  
 Pour analyser correctement la configuration utilisée par une conversation, exécutez un rapport de configuration **ssbdiagnose** utilisant les mêmes options que celles de la conversation. Si vous spécifiez pour **ssbdiagnose** un niveau d’options inférieur à celui de la conversation, **ssbdiagnose** risque de ne pas signaler les éléments requis par la conversation. Si vous spécifiez un niveau d’options supérieur pour **ssbdiagnose**, ce dernier risque de signaler des éléments de rapport qui ne sont pas requis par la conversation. Par exemple, une conversation entre deux services qui se trouvent dans la même base de données peut être exécutée avec ENCPRYPTION OFF. Si vous exécutez **ssbdiagnose** pour valider la configuration entre les deux services, mais utilisez le paramètre ENCRYPTION ON par défaut, **ssbdiagnose** signale qu’une clé principale est absente de la base de données. Or la conversation ne requiert pas de clé principale.  
  
 Le rapport de configuration de **ssbdiagnose** analyse un seul service [!INCLUDE[ssSB](../../includes/sssb-md.md)] ou une seule paire de services à chaque exécution. Pour générer un rapport sur plusieurs paires de services [!INCLUDE[ssSB](../../includes/sssb-md.md)] , générez un fichier de commande .cmd qui appelle **ssbdiagnose** plusieurs fois.  
  
## <a name="runtime-reporting"></a>Rapport d'exécution  
 Quand vous spécifiez -RUNTIME, **ssbdiagnose** recherche toutes les bases de données spécifiées dans **runtimeconnectionoptions** et **baseconnectionoptions** pour générer la liste des ID [!INCLUDE[ssSB](../../includes/sssb-md.md)] . Le contenu de liste d'ID générée dépend des options -NEW et –ID :  
  
-   Si vous ne spécifiez ni **-NEW** ni **-ID** , la liste inclut toutes les conversations pour toutes les bases de données spécifiées dans les options de connexion.  
  
-   Si vous spécifiez **-NEW** , **ssbdiagnose** inclut les éléments de la première conversation qui démarre après exécution de **ssbdiagnose** . Sont ainsi inclus l'ID de la conversation et les descripteurs de la conversation pour les points de terminaison initiateur et cible.  
  
-   Si vous spécifiez **-ID** avec un descripteur de conversation, seul ce descripteur est inclus dans la liste.  
  
-   Si vous spécifiez **-ID** avec un ID de conversation, l’ID de conversation et les descripteurs de ses deux points de terminaison de conversation sont ajoutés à la liste.  
  
-   Si vous spécifiez **-ID** avec un ID de groupe de conversations, tous les ID de conversation et les descripteurs de conversation de ce groupe sont ajoutés à la liste.  
  
 La liste n'inclut pas les éléments provenant de bases de données qui ne sont pas couvertes par les options de connexion. Par exemple, supposons que vous utilisez **-ID** pour spécifier un ID de conversation, mais que vous ne fournissez une clause **runtimeconnectionoptions** que pour la base de données initiateur et pas pour la base de données cible. **ssbdiagnose** n’inclut pas le descripteur de la conversation cible dans sa liste d’ID. Seuls l’ID de conversation et le descripteur de la conversation initiateur sont inclus.  
  
 **ssbdiagnose** analyse les événements [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] des bases de données couvertes par **runtimeconnectionoptions** et **baseconnectionoptions**. Il recherche des événements [!INCLUDE[ssSB](../../includes/sssb-md.md)] indiquant qu’une erreur a été rencontrée par une ou plusieurs ID [!INCLUDE[ssSB](../../includes/sssb-md.md)] dans la liste d’exécution. **ssbdiagnose** recherche également des événements d’erreur [!INCLUDE[ssSB](../../includes/sssb-md.md)] au niveau du système qui ne sont pas spécifiquement associés à un groupe de conversations.  
  
 Si **ssbdiagnose** détecte des erreurs de conversation, l’utilitaire tente de générer un rapport sur la cause première des événements en exécutant également un rapport de configuration. **ssbdiagnose** utilise les métadonnées des bases de données pour essayer de déterminer les instances, les ID [!INCLUDE[ssSB](../../includes/sssb-md.md)] , les bases de données, les services et les contrats utilisés par la conversation. Il exécute ensuite un rapport de configuration à l'aide de toutes les informations disponibles.  
  
 Par défaut, **ssbdiagnose** ne signale pas les événements d’erreur. Il signale seulement les problèmes sous-jacents identifiés au cours de la vérification de la configuration. Cela réduit la quantité d'informations signalée et vous aide à vous concentrer sur les problèmes de configuration sous-jacents. Vous pouvez spécifier **-SHOWEVENTS** pour afficher les événements d’erreur rencontrés par **ssbdiagnose**.  
  
## <a name="issues-reported-by-ssbdiagnose"></a>Problèmes signalés par ssbdiagnose  
 **ssbdiagnose** signale trois classes de problèmes. Dans le fichier de sortie XML, chaque catégorie de problème correspond à un type distinct de l'élément Issue. Ces trois types de problèmes signalés par **ssbdiagnose** sont les suivants :  
  
 **Diagnostic**  
 Signale un problème de configuration. Sont inclus les problèmes identifiés soit lors de l’exécution d’un rapport **CONFIGURATION** , soit pendant la phase de configuration d’un rapport **RUNTIME** . **ssbdiagnose** signale chaque problème de configuration une seule fois.  
  
 **Event**  
 Signale un événement [!INCLUDE[ssSqlProfiler](../../includes/sssqlprofiler-md.md)] qui indique qu’un problème a été rencontré par une conversation surveillée pendant un rapport **RUNTIME** . **ssbdiagnose** signale les événements chaque fois qu’ils sont générés. Les événements peuvent être signalés plusieurs fois si plusieurs conversations rencontrent le problème.  
  
 **Problème**  
 Signale un problème qui empêche **ssbdiagnose** d’effectuer une analyse de la configuration ou de surveiller des conversations.  
  
## <a name="sqlcmd-environment-variables"></a>Variables d'environnement sqlcmd  
 L’utilitaire **ssbdiagnose** prend en charge les variables d’environnement SQLCMDSERVER, SQLCMDUSER, SQLCMDPASSWORD et SQLCMDLOGINTIMOUT, qui sont également utilisées par l’utilitaire **sqlcmd** . Vous pouvez définir les variables d’environnement soit en utilisant l’invite de commandes SET, soit en utilisant la commande **setvar** dans des scripts [!INCLUDE[tsql](../../includes/tsql-md.md)] exécutés à l’aide de **sqlcmd**. Pour plus d’informations sur la façon d’utiliser **setvar** dans **sqlcmd**, consultez [Utiliser sqlcmd avec des variables de script](../../ssms/scripting/sqlcmd-use-with-scripting-variables.md).  
  
## <a name="permissions"></a>Autorisations  
 Dans chaque clause **connectionoptions** , la connexion spécifiée avec l’option **-E** ou **-U** doit être un membre du rôle serveur fixe **sysadmin** dans l’instance spécifiée dans **-S**.  
  
## <a name="examples"></a>Exemples  
 Cette section contient des exemples d’utilisation de **ssbdiagnose** à une invite de commandes.  
  
### <a name="a-checking-the-configuration-of-two-services-in-the-same-database"></a>R. Vérification de la configuration de deux services dans la même base de données  
 L'exemple suivant montre comment demander un rapport de configuration dans le contexte suivant :  
  
-   Le service initiateur et le service cible se trouvent dans la même base de données.  
  
-   La base de données se trouve dans l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   Les instances se trouvent sur l’ordinateur sur lequel **ssbdiagnose** est exécuté.  
  
 L’utilitaire **ssbdiagnose** signale la configuration qui utilise le contrat DEFAULT, car ON CONTRACT n’est pas spécifié.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target  
```  
  
### <a name="b-checking-the-configuration-of-two-services-on-separate-computers-that-use-one-login"></a>B. Vérification de la configuration de deux services sur des ordinateurs distincts qui utilisent une seule connexion  
 L'exemple suivant montre comment demander un rapport de configuration lorsque le service initiateur et le service cible se trouvent sur des ordinateurs distincts, mais sont accessibles en utilisant la même connexion via l'authentification Windows.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator -S InitiatorComputer -d InitiatorDatabase TO SERVICE /test/target -S TargetComputer -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="c-checking-the-configuration-of-two-services-on-separate-computers-that-use-separate-logins"></a>C. Vérification de la configuration de deux services sur des ordinateurs distincts qui utilisent des connexions distinctes  
 L'exemple suivant montre comment demander un rapport de configuration lorsque le service initiateur et le service cible se trouvent sur des ordinateurs distincts et que des connexions d'authentification [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] distinctes sont requises pour chaque instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
```  
ssbdiagnose CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -U InitiatorLogin -p !wEx23Dvb   
-d InitiatorDatabase TO SERVICE /test/target -S TargetComputer   
-U TargetLogin -p ER!49jiy -d TargetDatabase ON CONTRACT TestContract  
```  
  
### <a name="d-checking-mirrored-service-configurations-on-separate-computers-with-anonymous-encryption"></a>D. Vérification de la configuration de services mis en miroir sur des ordinateurs distincts avec le chiffrement anonyme  
 L'exemple suivant montre comment demander un rapport de configuration lorsque le service initiateur et le service cible se trouvent sur des ordinateurs distincts et que l'initiateur est mis en miroir sur une instance nommée. Ce rapport vérifie également que les services sont configurés pour utiliser le chiffrement anonyme.  
  
```  
ssbdiagnose -E CONFIGURATION FROM SERVICE /text/initiator   
-S InitiatorComputer -d InitiatorDatabase MIRROR   
-S MirrorComputer/MirrorInstance TO SERVICE /test/target   
-S TargetComputer -d TargetDatabase ON CONTRACT TestContract ENCRYPTION ANONYMOUS  
```  
  
### <a name="e-checking-the-configuration-of-two-contracts"></a>E. Vérification de la configuration de deux contrats  
 L'exemple suivant montre comment générer un fichier de commandes qui demande des rapports de configuration dans le contexte suivant :  
  
-   Le service initiateur et le service cible se trouvent dans la même base de données.  
  
-   La base de données se trouve dans l'instance par défaut du [!INCLUDE[ssDE](../../includes/ssde-md.md)].  
  
-   L’instance se trouve sur l’ordinateur sur lequel **ssbdiagnose** est exécuté.  
  
 À chaque exécution, **ssbdiagnose** signale la configuration d’un contrat différent entre les mêmes services.  
  
```  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target ON CONTRACT PayRaiseContract  
ssbdiagnose -E -d MyDatabase CONFIGURATION FROM SERVICE /test/initiator   
TO SERVICE /test/target ON CONTRACT PromotionContract  
```  
  
### <a name="f-monitor-the-status-of-a-specific-conversation-on-the-local-computer-with-a-time-out"></a>F. Surveillance du statut d'une conversation spécifique sur l'ordinateur local avec un délai d'attente  
 L’exemple suivant montre comment surveiller une conversation spécifique où le service initiateur et le service cible se trouvent dans la même base de données dans l’instance par défaut du même ordinateur exécutant **ssbdiagnose**. La valeur de l'intervalle d'arrêt est de 20 secondes.  
  
```  
ssbdiagnose -E -d TestDatabase RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D -TIMEOUT 20  
```  
  
### <a name="g-monitor-the-status-of-a-conversation-that-spans-two-computers"></a>G. Surveillance du statut d'une conversation qui s'étend sur deux ordinateurs  
 L'exemple suivant montre comment surveiller une conversation spécifique où le service initiateur et le service cible se trouvent sur des ordinateurs distincts.  
  
```  
ssbdiagnose RUNTIME -ID D68D77A9-B1CF-41BF-A5CE-279ABCAB140D   
-TIMEOUT 10 CONNECT TO -E -S InitiatorComputer/InitiatorInstance   
-d InitiatorDatabase CONNECT TO -E -S TargetComputer/TargetInstance   
-d TargetDatabase  
```  
  
### <a name="h-monitor-the-status-of-a-conversation-in-two-databases-in-the-same-instance"></a>H. Surveillance du statut d'une conversation dans deux bases de données dans la même instance  
 L'exemple suivant montre comment surveiller une conversation spécifique où le service initiateur et le service cible se trouvent dans des bases de données distinctes dans la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cet exemple utilise **baseconnectionoptions** pour spécifier l’instance et les informations de connexion, ainsi que deux clauses CONNECT TO pour spécifier les bases de données. -SHOWEVENTS est spécifié afin que tous les événements d'exécution soient inclus dans le rapport.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME -SHOWEVENTS   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="i-monitor-the-status-of-two-conversations-between-two-databases"></a>I. Surveillance du statut de deux conversations entre deux bases de données  
 L'exemple suivant montre comment surveiller deux conversations spécifiques où le service initiateur et le service cible se trouvent dans des bases de données distinctes dans la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cet exemple utilise **baseconnectionoptions** pour spécifier l’instance et les informations de connexion, ainsi que deux clauses CONNECT TO pour spécifier les bases de données.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-ID 5094d4a7-e38c-4c37-da37-1d58b1cb8455   
-ID 9b293be9-226b-4e22-e169-1d2c2c15be86 -TIMEOUT 10 CONNECT TO   
-d InitiatorDatabase CONNECT TO -d TargetDatabase  
```  
  
### <a name="j-monitor-the-status-of-all-conversations-between-two-databases"></a>J. Surveillance du statut de toutes les conversations entre deux bases de données  
 L'exemple suivant montre comment surveiller toutes les conversations entre deux bases de données dans la même instance du [!INCLUDE[ssDE](../../includes/ssde-md.md)]. Cet exemple utilise **baseconnectionoptions** pour spécifier l’instance et les informations de connexion, ainsi que deux clauses CONNECT TO pour spécifier les bases de données.  
  
```  
ssbdiagnose -E -S TestComputer/DevTestInstance RUNTIME   
-TIMEOUT 10 CONNECT TO -d InitiatorDatabase CONNECT TO   
-d TargetDatabase  
```  
  
### <a name="k-ignore-specific-errors"></a>K. Ignorer des erreurs spécifiques  
 L'exemple suivant montre comment ignorer les erreurs connues (303 et 304) détectées dans la configuration actuelle de l'activation dans un système test.  
  
```  
ssbdiagnose -IGNORE 303 -IGNORE 304 -E -d TestDatabase   
CONFIGURATION FROM SERVICE /test/initiator TO SERVICE /test/target   
ON CONTRACT TextContract  
```  
  
### <a name="l-redirecting-ssbdiagnose-xml-output"></a>L. Redirection de la sortie XML de ssbdiagnose  
 L’exemple suivant montre comment demander que **ssbdiagnose** génère sa sortie sous la forme d’un fichier XML redirigé vers un fichier. Le fichier TestDiag.xml peut ensuite être ouvert par une application pour analyser ou créer un rapport à partir des fichiers XML de **ssbdiagnose** . Vous pouvez également le consulter dans tout éditeur XML, tel que le bloc-notes XML.  
  
```  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target > c:\MyDiagnostics\TestDiag.xml  
```  
  
### <a name="m-using-an-environment-variable"></a>M. Utilisation d'une variable d'environnement  
 L’exemple suivant commence par définir la variable d’environnement SQLCMDSERVER pour qu’elle contienne le nom du serveur, puis exécute **ssbdiagnose** sans spécifier l’option **-S**.  
  
```  
SET SQLCMDSERVER=MyComputer  
ssbdiagnose -XML -E -d MyDatabase CONFIGURATION FROM SERVICE   
/test/initiator TO SERVICE /test/target  
```  
  
## <a name="see-also"></a>Voir aussi  
 [SQL Server Service Broker](../../database-engine/configure-windows/sql-server-service-broker.md)   
 [BEGIN DIALOG CONVERSATION &#40;Transact-SQL&#41;](../../t-sql/statements/begin-dialog-conversation-transact-sql.md)   
 [CREATE BROKER PRIORITY &#40;Transact-SQL&#41;](../../t-sql/statements/create-broker-priority-transact-sql.md)   
 [CREATE CERTIFICATE &#40;Transact-SQL&#41;](../../t-sql/statements/create-certificate-transact-sql.md)   
 [CREATE CONTRACT &#40;Transact-SQL&#41;](../../t-sql/statements/create-contract-transact-sql.md)   
 [CREATE ENDPOINT &#40;Transact-SQL&#41;](../../t-sql/statements/create-endpoint-transact-sql.md)   
 [CREATE MASTER KEY &#40;Transact-SQL&#41;](../../t-sql/statements/create-master-key-transact-sql.md)   
 [CREATE MESSAGE TYPE &#40;Transact-SQL&#41;](../../t-sql/statements/create-message-type-transact-sql.md)   
 [CREATE QUEUE &#40;Transact-SQL&#41;](../../t-sql/statements/create-queue-transact-sql.md)   
 [CREATE REMOTE SERVICE BINDING &#40;Transact-SQL&#41;](../../t-sql/statements/create-remote-service-binding-transact-sql.md)   
 [CREATE ROUTE &#40;Transact-SQL&#41;](../../t-sql/statements/create-route-transact-sql.md)   
 [CREATE SERVICE &#40;Transact-SQL&#41;](../../t-sql/statements/create-service-transact-sql.md)   
 [RECEIVE &#40;Transact-SQL&#41;](../../t-sql/statements/receive-transact-sql.md)   
 [sys.transmission_queue &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-transmission-queue-transact-sql.md)   
 [sys.conversation_endpoints &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-endpoints-transact-sql.md)   
 [sys.conversation_groups &#40;Transact-SQL&#41;](../../relational-databases/system-catalog-views/sys-conversation-groups-transact-sql.md)  
  
