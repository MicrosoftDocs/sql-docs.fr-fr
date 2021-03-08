---
title: Connexion avec bcp
description: Découvrez comment utiliser l’utilitaire bcp avec Microsoft ODBC Driver for SQL Server sur Linux et macOS.
ms.custom: ''
ms.date: 02/24/2021
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- bcp
ms.assetid: 3eca5717-e50f-40db-be16-a1cebbdfee70
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f8bff2d5d9892d709885d0888e36ca042aa72242
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837393"
---
# <a name="connecting-with-bcp"></a>Connexion avec bcp
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

L’utilitaire [bcp](../../../tools/bcp-utility.md) est disponible dans [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur Linux et macOS. Cette page décrit les différences par rapport à la version Windows de `bcp`.
  
- La marque de fin de champ est une tabulation (« \t  »).  
  
- La marque de fin de ligne est un saut de ligne (« \n »).  
  
- Le mode caractère est le format préféré des fichiers au format `bcp` et des fichiers de données qui ne contiennent pas de caractères étendus.  
  
> [!NOTE]  
> Une barre oblique inverse « \\ » dans un argument de ligne de commande doit être placée entre guillemets ou précédée d’un caractère d’échappement. Par exemple, pour spécifier un saut de ligne en tant que marque de fin de ligne personnalisée, vous devez utiliser l»un des mécanismes suivants :  
>   
> -   -r\\\n  
> -   -r"\n"  
> -   -r'\n'  
  
Voici un exemple d’appel de commande de l’utilitaire `bcp` pour copier des lignes de table dans un fichier texte :  
  
```  
bcp AdventureWorks2008R2.Person.Address out test.dat -Usa -Pxxxx -Sxxx.xxx.xxx.xxx  
```  
  
## <a name="available-options"></a>Options disponibles
Dans la version actuelle, la syntaxe et les options suivantes sont disponibles :  

[_database_ **.** ]_schema_ **.** _table_ **in** _data\_file_ | **out** _data\_file_

- -a *packet_size*  
Spécifie le nombre d’octets, par paquet réseau, envoyés depuis/vers le serveur.  
  
- -b *batch_size*  
Nombre de lignes par lot de données importées.  
  
- -c  
Utilise un type de données caractères.  
  
- -d *database_name*  
Spécifie la base de données à laquelle se connecter.  
  
- -d  
Fait en sorte que la valeur passée à l’option -S de `bcp` soit interprétée comme un nom de source de données (DSN). Pour plus d’informations, consultez « Prise en charge du nom de source de données dans sqlcmd et bcp » dans [Connexion avec sqlcmd](connecting-with-sqlcmd.md).  
  
- -e *error_file* Spécifie le chemin complet d’un fichier d’erreur utilisé pour stocker les lignes que l’utilitaire `bcp` n’a pas pu transférer du fichier à la base de données.  
  
- -E  
Utilise une ou plusieurs valeurs d’identité figurant dans le fichier de données importé pour la colonne d’identité.  
  
- -f *format_file*  
Spécifie le chemin complet au fichier de format.  
  
- -F *first_row*  
Spécifie le numéro de la première ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données.  

- -G  
Ce commutateur est utilisé par le client durant la connexion à Azure SQL Database ou à Azure Synapse Analytics pour faire en sorte que l’utilisateur soit authentifié à l’aide de l’authentification Azure Active Directory. Le commutateur -G nécessite au moins la version bcp 17.6. Pour déterminer votre version, exécutez bcp -v.

> [!IMPORTANT]
> L’option `-G` s’applique uniquement à Azure SQL Database et à Azure Synapse Analytics.
> L’authentification interactive AAD n’est actuellement prise en charge ni sur Linux ni sur macOS. L’authentification intégrée AAD requiert [Microsoft ODBC Driver 17 pour SQL Server](../download-odbc-driver-for-sql-server.md) version 17.6.1 ou ultérieure et un [environnement Kerberos correctement configuré](using-integrated-authentication.md#configure-kerberos).

- -k  
Pendant l’opération, les colonnes vides doivent conserver une valeur NULL et les colonnes insérées ne doivent pas prendre de valeur par défaut.  
  
- -l  
Spécifie un délai de connexion. L’option -l spécifie le nombre de secondes au terme duquel une connexion de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] expire quand vous tentez de vous connecter à un serveur. Par défaut, le délai d'expiration de la connexion est de 15 secondes. Le délai de connexion doit être un nombre compris entre 0 et 65534. Si la valeur fournie n'est pas numérique ou n'est pas comprise dans cet intervalle, `bcp` génère un message d'erreur. Une valeur 0 spécifie un délai d’expiration infini.
  
- -L *last_row*  
Spécifie le numéro de la dernière ligne à exporter à partir d’une table ou à importer à partir d’un fichier de données.  
  
- -m *max_errors*  
Spécifie le nombre maximal d'erreurs de syntaxe tolérées avant l'annulation de l'opération `bcp`.  
  
- -n  
Utilise les types de données (de la base de données) natifs pour effectuer l’opération de copie en bloc.  
  
- -P *password*  
Spécifie le mot de passe de l’ID de connexion.  
  
- -q  
Exécute l'instruction SET QUOTED_IDENTIFIERS ON dans la connexion entre l'utilitaire `bcp` et une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -r *row_terminator*  
Spécifie l’indicateur de fin de ligne.  
  
- -R  
Spécifie que les données de type devise, date et heure sont copiées en bloc dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en utilisant le format régional défini par les paramètres régionaux de l'ordinateur client.  
  
- -S *server*  
Spécifie le nom de l’instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] à laquelle se connecter, ou si -D est utilisé, un DSN.  
  
- -t *field_terminator*  
Spécifie l’indicateur de fin de champ.  
  
- -T  
Spécifie que l’utilitaire `bcp` se connecte à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] avec une connexion approuvée (sécurité intégrée).  
  
- -U *login_id*  
Spécifie l'ID de connexion utilisé pour une connexion à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -v  
Indique le numéro de version et le copyright de l'utilitaire `bcp`.  
  
- -w  
Utilise des caractères Unicode pour effectuer l’opération de copie en bloc.  
  
Dans cette version, les caractères Latin-1 et UTF-16 sont pris en charge.  
  
## <a name="unavailable-options"></a>Options non disponibles
Dans la version actuelle, la syntaxe et les options suivantes sont indisponibles :  

- -c  
Indique la page de codes des données dans le fichier.  
  
- -h *hint*  
Spécifie les indications utilisées pendant l’importation en bloc des données dans une table ou une vue.  
  
- -i *input_file*  
Spécifie le nom d’un fichier réponse.  
  
- -n  
Utilise les types de données (base de données) natifs pour les données non-caractères et les caractères Unicode pour les données caractères.  
  
- -o *output_file*  
Spécifie le nom d’un fichier recevant la sortie redirigée à partir de l’invite de commandes.  
  
- -V (80 | 90 | 100)  
Utilise les types de données d’une version antérieure de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
  
- -X  
Utilisé avec les options format et -f formal_file, génère un fichier au format XML à la place du fichier au format non-XML par défaut.  
  
## <a name="see-also"></a>Voir aussi

[Connexion avec **sqlcmd**](connecting-with-sqlcmd.md)  
[Notes de publication](release-notes-tools.md)