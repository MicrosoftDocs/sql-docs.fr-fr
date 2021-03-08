---
title: Connexion avec sqlcmd
description: Découvrez comment utiliser l’utilitaire sqlcmd avec Microsoft ODBC Driver for SQL Server sur Linux et macOS.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 216f78615ca049d3e97134cb14831d9a5e6afd32
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837360"
---
# <a name="connecting-with-sqlcmd"></a>Connexion avec sqlcmd
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

L’utilitaire [sqlcmd](../../../tools/sqlcmd-utility.md) est disponible dans [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur Linux et macOS.
  
Les commandes suivantes montrent respectivement comment utiliser l’authentification Windows (Kerberos) l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] :
  
```console
sqlcmd -E -Sxxx.xxx.xxx.xxx  
sqlcmd -Sxxx.xxx.xxx.xxx -Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>Options disponibles

Dans la version actuelle, les options suivantes sont disponibles :  
  
- -? Afficher l’utilisation de `sqlcmd`.  
  
- -a Demander une taille de paquet.  
  
- -b Arrêter le traitement par lots en cas d’erreur.  
  
- -c *marque_fin_lot* Spécifier la marque de fin de lot.  
  
- -C Faire confiance au certificat de serveur.  

- -d *nom_base_de_données* Émettre une instruction `USE` *nom_base_de_données* au démarrage de `sqlcmd`.  

- -D Fait en sorte que la valeur passée à l’option -S de `sqlcmd` soit interprétée comme un nom de source de données (DSN). Pour plus d’informations, consultez « Prise en charge du nom de source de données dans `sqlcmd` et `bcp` » à la fin de cette rubrique.  
  
- -e Écrire des scripts d’entrée sur le périphérique de sortie standard (stdout).

- -E Utiliser la connexion approuvée (authentification intégrée). Pour savoir comment établir des connexions approuvées qui utilisent l’authentification intégrée à partir d’un client Linux ou macOS, consultez [Utiliser l’authentification intégrée](using-integrated-authentication.md).

- -f codepage | i:codepage[,o:codepage] | o:codepage[,i:codepage] Spécifie les pages de codes d’entrée et de sortie. Le numéro de page de codes est une valeur numérique qui spécifie une page de codes Linux installée.
(disponible depuis 17.5.1.1)

- -G Ce commutateur est utilisé par le client durant la connexion à SQL Database ou à Azure Synapse Analytics pour faire en sorte que l’utilisateur soit authentifié à l’aide de l’authentification Azure Active Directory. Cette option définit la variable de script sqlcmd SQLCMDUSEAAD = true. Le commutateur -G nécessite au moins la version sqlcmd 17.6. Pour déterminer votre version, exécutez sqlcmd -?.

> [!IMPORTANT]
> L’option `-G` s’applique uniquement à Azure SQL Database et à Azure Synapse Analytics.
> L’authentification interactive AAD n’est actuellement prise en charge ni sur Linux ni sur macOS. L’authentification intégrée AAD requiert [Microsoft ODBC Driver 17 pour SQL Server](../download-odbc-driver-for-sql-server.md) version 17.6.1 ou ultérieure et un [environnement Kerberos correctement configuré](using-integrated-authentication.md#configure-kerberos).

- -h *nombre_de_lignes*  Spécifier le nombre de lignes à imprimer entre les en-têtes de colonnes.  
  
- -H Spécifier un nom de station de travail.  
  
- -i *input_file*[,*input_file*[,...]] Identifier le fichier contenant un traitement d'instructions SQL ou des procédures stockées.  
  
- -I Affecter la valeur ON à l’option de connexion `SET QUOTED_IDENTIFIER`.  
  
- -k Supprimer ou remplacer des caractères de contrôle.  
  
- **-K**_application\_intent_  
Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. La seule valeur actuellement prise en charge est **ReadOnly**. Si vous ne spécifiez pas **-K**, `sqlcmd` ne prend pas en charge la connectivité à un réplica secondaire dans un groupe de disponibilité AlwaysOn. Pour plus d’informations, consultez [Prise en charge par le pilote ODBC pour Linux et macOS de la haute disponibilité et de la reprise d’activité](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> L’option **-K** n’est pas prise en charge dans le CTP pour SUSE Linux. Vous pouvez toutefois spécifier le mot clé **ApplicationIntent=ReadOnly** dans un fichier DSN passé à `sqlcmd`. Pour plus d’informations, consultez « Prise en charge du nom de source de données dans `sqlcmd` et `bcp` » à la fin de cette rubrique.  
  
- -l *timeout* spécifie le nombre de secondes au terme duquel une connexion `sqlcmd` expire quand vous tentez de vous connecter à un serveur.

- -m *niveau_erreur* Déterminer les messages d’erreur envoyés à stdout.  
  
- **-M**_multisubnet\_failover_  
Spécifiez toujours **-M** en cas de connexion à l’écouteur de groupe de disponibilité d’un groupe de disponibilité [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)] ou d’une instance de cluster de basculement [!INCLUDE[ssSQL11](../../../includes/sssql11-md.md)]. **-M** accélère la détection des basculements et la connexion au serveur (actuellement) actif. Si vous ne spécifiez pas l’option **-M** , **-M** est désactivé. Pour plus d’informations sur [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)], consultez [Pilote ODBC sur Linux et macOS pour la haute disponibilité et la récupération d’urgence](odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> L’option **-M** n’est pas prise en charge dans le CTP pour SUSE Linux. Vous pouvez toutefois spécifier le mot clé **MultiSubnetFailover=Yes** dans un fichier DSN passé à `sqlcmd`. Pour plus d’informations, consultez « Prise en charge du nom de source de données dans `sqlcmd` et `bcp` » à la fin de cette rubrique.  
  
- -N Chiffrer la connexion.  
  
- -o *fichier_sortie* Identifier le fichier qui reçoit le sortie de `sqlcmd`.  
  
- -p Imprimer des statistiques de performances pour chaque jeu de résultats.  
  
- -P Spécifier un mot de passe utilisateur.  
  
- -q *requête_ligne_de_commande* Exécute une requête au démarrage de `sqlcmd`, mais ne quitte pas au terme de l’exécution de la requête.  

- -Q *requête_ligne_de_commande* Exécuter une requête au démarrage de `sqlcmd`. `sqlcmd` s’arrête quand la requête est terminée.  

- -r Redirige les messages d’erreur vers stderr.

- -R Fait en sorte que le pilote utilise les paramètres régionaux du client pour convertir les données de devise et de date et d’heure en données caractères. Uniquement actuellement uniquement la mise en forme en_US (anglais américain).
  
- -s *caractère_séparation_colonnes*  Spécifier le caractère de séparation des colonnes.  

- -S [*protocol*:] *server*[ **,** _port_]  
Spécifier l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à laquelle il faudra se connecter ou, si -D est utilisé, un nom de source de données. Le pilote ODBC sur Linux et macOS impose d’utiliser -S. Il est à noter que le protocole **tcp** est le seul protocole valide.  
  
- -t *délai_expiration_requête* Spécifier le nombre de secondes accordées pour l’exécution d’une commande (ou une instruction SQL).  
  
- -u Spécifier que fichier_sortie est stocké au format Unicode, quel que soit le format de fichier_entrée.  
  
- -U *ID_connexion* Spécifier un ID de connexion utilisateur.  
  
- -V *niveau_gravité_erreur* Contrôler le niveau de gravité utilisé pour définir la variable ERRORLEVEL.  
  
- -w *largeur_colonne* Spécifier la largeur d’écran de la sortie.  
  
- -W Supprimer les espaces à droite d’une colonne.  
  
- -x Désactiver la substitution de variable.  
  
- -X Désactiver les commandes, le script de démarrage et les variables d’environnement.  
  
- -y *largeur_affichage_type_longueur_variable* Définir la variable de script `sqlcmd` `SQLCMDMAXFIXEDTYPEWIDTH`.
  
- -Y *largeur_affichage_type_longueur_fixe* Définir la variable de script `sqlcmd` `SQLCMDMAXVARTYPEWIDTH`.

- -z *mot_de_passe* Modifier le mot de passe.  
  
- -Z *mot_de_passe* Modifier le mot de passe et quitter.  


## <a name="available-commands"></a>Commandes disponibles

Dans la version actuelle, les commandes suivantes sont disponibles :  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*count*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>Options non disponibles
Dans la version actuelle, les options suivantes ne sont pas disponibles :  

- -A Se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec une connexion administrateur dédiée (DAC). Pour plus d’informations sur la façon d’établir une connexion administrateur dédiée, consultez [Recommandations en matière de programmation](programming-guidelines.md).  
  
- -L Répertorier les serveurs configurés localement et le nom des serveurs diffusant sur le réseau.  
  
- -v Créer une variable de script `sqlcmd` pouvant être utilisée dans un script `sqlcmd`.  
  
Vous pouvez utiliser la méthode alternative suivante : Placez les paramètres dans un même fichier, que vous pouvez ensuite ajouter à un autre fichier. Cela vous aidera à utiliser un fichier de paramètres pour remplacer les valeurs. Par exemple, créer un fichier nommé `a.sql` (le fichier de paramètres) avec le contenu suivant :
  
```console
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
```
  
Ensuite, créer un fichier nommé `b.sql` avec les paramètres de remplacement :  
  
```sql
    select $(ColumnName) from $(TableName)  
```

En ligne de commande, combinez `a.sql` et `b.sql` en `c.sql` avec les commandes suivantes :  
  
```console
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
```
  
Exécutez `sqlcmd` et utilisez `c.sql` comme fichier d’entrée :  
  
```console
    sqlcmd -S<...> -P<..> -U<..> -I c.sql  
```

## <a name="unavailable-commands"></a>Commandes non disponibles

Dans la version actuelle, les commandes suivantes ne sont pas disponibles :  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>Prise en charge du nom de source de données dans sqlcmd et bcp

Vous pouvez indiquer un nom de source de données (DSN) plutôt qu’un nom de serveur dans l’option **sqlcmd** ou **bcp** `-S` (ou la commande **sqlcmd** :Connect) si vous spécifiez `-D`. `-D` provoque **sqlcmd** ou **bcp** pour se connecter au serveur spécifié dans le DSN dans l’option `-S`.  
  
Les noms de sources de données système sont stockés dans le fichier `odbc.ini` du répertoire ODBC SysConfigDir (`/etc/odbc.ini` sur les installations standard). Les noms de sources de données utilisateur sont stockés dans `.odbc.ini` dans le répertoire de base de l’utilisateur (`~/.odbc.ini`).

Sur les systèmes Windows, les noms de source de données Utilisateur et Système sont stockés dans le Registre et gérés par le biais d’odbcad32.exe. Les noms de source de données Fichier ne sont pas pris en charge par bcp et sqlcmd.

Consultez [Mots clés et attributs de chaîne de connexion et DSN](../dsn-connection-string-attribute.md) pour la liste des entrées prises en charge par le pilote.

Dans un nom de source de données, seule l’entrée DRIVER est nécessaire, mais pour établir une connexion à un serveur distant, `sqlcmd` ou `bcp` a besoin d’une valeur dans l’élément SERVER. Si l’élément SERVER est vide ou absent dans le nom de source de données, `sqlcmd` et `bcp` tenteront de se connecter à l’instance par défaut sur le système local.

Lors de l’utilisation de bcp sur les systèmes Windows, [!INCLUDE [sssql17-md](../../../includes/sssql17-md.md)] et versions antérieures exigent le pilote SQL Native Client 11 (sqlncli11.dll), tandis que [!INCLUDE [sssql19-md](../../../includes/sssql19-md.md)] et versions ultérieures exigent le pilote Microsoft ODBC Driver 17 for SQL Server (msodbcsql17.dll).  

Si vous spécifiez la même option dans le nom de source de données et sur la ligne de commande `sqlcmd` ou `bcp`, l’option de ligne de commande remplace la valeur utilisée dans le nom de source de données. Par exemple, si le nom de source de données comporte une entrée DATABASE et que la ligne de commande `sqlcmd` inclut **-d**, la valeur passée à **-d** est utilisée. Si vous spécifiez **Trusted_Connection=yes** dans le nom de source de données, l’authentification Kerberos est utilisée et le nom d’utilisateur ( **-U**) et le mot de passe ( **-P**), s’ils sont fournis, sont ignorés.

Vous pouvez modifier les scripts qui appellent `isql` pour qu’ils utilisent `sqlcmd` en définissant l’alias suivant : `alias isql="sqlcmd -D"`.  

## <a name="see-also"></a>Voir aussi  
[Connexion avec **bcp**](connecting-with-bcp.md)  
[Notes de publication](release-notes-tools.md)
