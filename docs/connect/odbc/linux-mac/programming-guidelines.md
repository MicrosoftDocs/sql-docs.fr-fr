---
title: Instructions de programmation (pilote ODBC)
description: Les fonctionnalités de programmation de Microsoft ODBC Driver for SQL Server sur macOS et Linux reposent sur ODBC dans SQL Server Native Client.
ms.custom: ''
ms.date: 05/06/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
author: v-makouz
ms.author: v-daenge
ms.openlocfilehash: 6d602db9c189b6e7fce8b767b60204253ccb7f67
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464391"
---
# <a name="programming-guidelines"></a>Instructions de programmation

[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Les fonctionnalités de programmation de [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] sur macOS et Linux reposent sur ODBC dans [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)). [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client se base sur ODBC dans Windows Data Access Components ([Guide de référence du programmeur ODBC](../../../odbc/reference/odbc-programmer-s-reference.md)).  

Une application ODBC peut utiliser MARS (Multiple Active Result Sets) et d’autres fonctionnalités spécifiques [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] en incluant `/usr/local/include/msodbcsql.h` après avoir inclus les en-têtes unixODBC (`sql.h`, `sqlext.h`, `sqltypes.h` et `sqlucode.h`). Utilisez alors les mêmes noms symboliques pour les éléments spécifiques à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] que dans vos applications ODBC Windows.

## <a name="available-features"></a>Fonctionnalités disponibles  
Les sections suivantes de la documentation de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client pour ODBC ([SQL Server Native Client (ODBC)](../../../relational-databases/native-client/odbc/sql-server-native-client-odbc.md)) sont valides quand vous utilisez le pilote ODBC sur macOS et Linux :  

-   [Communication avec SQL Server (ODBC)](../../../relational-databases/native-client-odbc-communication/communicating-with-sql-server-odbc.md)  
-   [Prise en charge des connexions et délais de requête](../../../relational-databases/native-client/applications/using-connection-string-keywords-with-sql-server-native-client.md)  
-   [Curseurs](../../../relational-databases/native-client-odbc-cursors/using-cursors-odbc.md)  
-   [Améliorations des types de données date et heure (ODBC)](../../../relational-databases/native-client-odbc-date-time/date-and-time-improvements-odbc.md)  
-   [Exécution de requêtes (ODBC)](../../../relational-databases/native-client-odbc-queries/executing-queries-odbc.md)  
-   [Gestion des erreurs et des messages](../../../relational-databases/native-client-odbc-error-messages/handling-errors-and-messages.md)  
-   [Authentification Kerberos](../../../relational-databases/native-client/features/service-principal-name-spn-support-in-client-connections.md)  
-   [Types CLR volumineux définis par l’utilisateur (ODBC)](../../../relational-databases/native-client/odbc/large-clr-user-defined-types-odbc.md)  
-   [Exécution de transactions (ODBC) (à l’exception des transactions distribuées)](../../../relational-databases/native-client/odbc/performing-transactions-in-odbc.md)  
-   [Traitement des résultats (ODBC)](../../../relational-databases/native-client-odbc-results/processing-results-odbc.md)  
-   [Exécution de procédures stockées](../../../relational-databases/native-client-odbc-stored-procedures/running-stored-procedures.md)
-   [Prise en charge des colonnes éparses (ODBC)](../../../relational-databases/native-client/odbc/sparse-columns-support-odbc.md)
-   [Utilisation du chiffrement sans validation](../../../relational-databases/native-client/features/using-encryption-without-validation.md)
-   [Paramètres table](../../../relational-databases/native-client-odbc-table-valued-parameters/table-valued-parameters-odbc.md)
-   [API de commandes et données UTF-8 et UTF-16](../../../relational-databases/native-client/features/utf-16-support-in-sql-server-native-client-11-0.md)
-   [Utilisation des fonctions de catalogue](../../../relational-databases/native-client/odbc/using-catalog-functions.md)  

## <a name="unsupported-features"></a>Fonctions non prises en charge

Le fonctionnement correct des fonctionnalités suivantes dans cette version du pilote ODBC sur macOS et Linux n’a pas été vérifié :

-   Connexion de cluster de basculement
-   [Résolution d’adresses IP réseau transparente](../using-transparent-network-ip-resolution.md) (avant ODBC Driver 17)
-   [Suivi de pilote avancé](/archive/blogs/mattn/enabling-advanced-driver-tracing-for-the-sql-native-client-odbc-drivers)

Les fonctionnalités suivantes ne sont pas disponibles dans cette version du pilote ODBC sur macOS et Linux : 

-   Transactions distribuées (attribut SQL_ATTR_ENLIST_IN_DTC non pris en charge)  
-   Mise en miroir de bases de données  
-   FILESTREAM  
-   Profilage des performances du pilote ODBC, décrit dans [SQLSetConnectAttr](../../../relational-databases/native-client-odbc-api/sqlsetconnectattr.md) et attributs de connexion liés aux performances suivants :  
    -   SQL_COPT_SS_PERF_DATA  
    -   SQL_COPT_SS_PERF_DATA_LOG  
    -   SQL_COPT_SS_PERF_DATA_LOG_NOW  
    -   SQL_COPT_SS_PERF_QUERY  
    -   SQL_COPT_SS_PERF_QUERY_INTERVAL  
    -   SQL_COPT_SS_PERF_QUERY_LOG  
-   SQLBrowseConnect (avant la version 17.2)
-   Types d’intervalle C comme SQL_C_INTERVAL_YEAR_TO_MONTH (décrits dans [Identificateurs et descripteurs des types de données](../../../odbc/reference/appendixes/data-type-identifiers-and-descriptors.md))
-   Valeur SQL_CUR_USE_ODBC de l’attribut SQL_ATTR_ODBC_CURSORS de la fonction SQLSetConnectAttr.

## <a name="character-set-support"></a>Prise en charge du jeu de caractères

Pour ODBC Driver 13 et 13.1, les données SQLCHAR doivent être au format UTF-8. Aucun autre encodage n’est pris en charge.

Pour ODBC Driver 17, les données SQLCHAR dans un des jeux de caractères/encodages suivants sont prises en charge :

> [!NOTE]  
> En raison des différences de `iconv` dans `musl` et `glibc`, beaucoup de ces paramètres régionaux ne sont pas pris en charge sur Alpine Linux.
>
> Pour plus d’informations, consultez [Différences fonctionnelles par rapport à glibc](https://wiki.musl-libc.org/functional-differences-from-glibc.html)

|Nom|Description|
|-|-|
|UTF-8|Unicode|
|CP437|MS-DOS Latin US|
|CP850|MS-DOS Latin 1|
|CP874|Latin/thaï|
|CP932|Japonais, Shift-JIS|
|CP936|Chinois simplifié, GBK|
|CP949|Coréen, EUC-KR|
|CP950|Chinois traditionnel, Big5|
|CP1251|Cyrillique|
|CP1253|Grec|
|CP1256|Arabe|
|CP1257|Balte|
|CP1258|Vietnamien|
|ISO-8859-1 / CP1252|Latin-1|
|ISO-8859-2 / CP1250|Latin-2|
|ISO-8859-3|Latin-3|
|ISO-8859-4|Latin-4|
|ISO-8859-5|Latin/cyrillique|
|ISO-8859-6|Latin/arabe|
|ISO-8859-7|Latin/grec|
|ISO-8859-8 / CP1255|Hébreu|
|ISO-8859-9 / CP1254|Turc|
|ISO-8859-13|Latin-7|
|ISO-8859-15|Latin-9|

Au moment de la connexion, le pilote détecte les paramètres régionaux actuels du processus dans lequel il est chargé. S’il utilise un des encodages ci-dessus, le pilote s’en sert pour les données SQLCHAR (caractères étroits) ; sinon, il utilise par défaut l’encodage UTF-8. Dans la mesure où tous les processus démarrent dans les paramètres régionaux « C » par défaut (amenant le pilote à utiliser par défaut le format UTF-8), si une application doit utiliser un des encodages ci-dessus, elle doit recourir à la fonction **setlocale** pour définir les paramètres régionaux de façon appropriée avant la connexion, soit en spécifiant les paramètres régionaux souhaités explicitement, soit en utilisant une chaîne vide, par exemple `setlocale(LC_ALL, "")`, pour utiliser les paramètres régionaux de l’environnement.

Ainsi, dans un environnement Linux ou macOS standard où l’encodage est UTF-8, les utilisateurs d’ODBC Driver 17 qui effectuent une mise à niveau à partir de la version 13 ou 13.1 ne constatent aucune différence. Toutefois, les applications qui utilisent un codage non-UTF-8 dans la liste ci-dessus par le biais de `setlocale()` doivent recourir à cet encodage pour les données vers et depuis le pilote au lieu d’UTF-8.

Les données SQLWCHAR doivent être au format UTF-16LE (Little Endian).

Au moment de la liaison des paramètres d’entrée avec SQLBindParameter, si un type SQL à caractères étroits tel que SQL_VARCHAR est spécifié, le pilote convertit les données fournies depuis l’encodage client vers l’encodage [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] par défaut (en général, la page de codes 1252). Pour les paramètres de sortie, le pilote effectue une conversion depuis l’encodage spécifié dans les informations de classement associées aux données vers l’encodage client. Toutefois, une perte de données est possible : les caractères dans l’encodage source qui ne sont pas représentables dans l’encodage cible sont convertis en point d’interrogation (« ? »).

Pour éviter cette perte de données au moment de la liaison des paramètres d’entrée, spécifiez un type de caractère SQL Unicode, comme SQL_NVARCHAR. Dans ce cas, le pilote effectue une conversion depuis l’encodage client vers le format UTF-16, qui peut représenter tous les caractères Unicode. En outre, la colonne ou paramètre cible sur le serveur doit également être un type Unicode (**nchar**, **nvarchar**, **ntext**) ou doté d’un classement/encodage pouvant représenter tous les caractères des données sources d’origine. Pour éviter la perte de données avec les paramètres de sortie, spécifiez un type SQL Unicode et soit un type C Unicode (SQL_C_WCHAR), afin que le pilote retourne les données au format UTF-16, soit un type C à caractères étroits, et assurez-vous que l’encodage client peut représenter tous les caractères des données sources (ce qui est toujours possible avec le format UTF-8.)

Pour plus d’informations sur les classements et les codages, voir [Prise en charge d’Unicode et des classements](../../../relational-databases/collations/collation-and-unicode-support.md).

Il existe certaines différences de conversion d’encodage entre Windows et plusieurs versions de la bibliothèque iconv sur Linux et macOS. Les données texte dans la page de codes 1255 (hébreu) ont un seul point de code (0xCA) qui se comporte différemment au moment de la conversion en Unicode. Sur Windows, ce caractère est converti dans le point de code UTF-16 0x05BA. Sur macOS et Linux avec versions de libiconv antérieures à 1.15, il est converti dans le point de code 0x00CA. Sur Linux avec les bibliothèques iconv qui ne prennent pas en charge la révision 2003 de Big5/CP950 (nommée `BIG5-2003`), les caractères ajoutés à cette révision ne sont pas convertis correctement. Dans la page de codes 932 (japonais, Shift-JIS), le résultat du décodage de caractères qui ne sont pas initialement définis dans le standard d’encodage est également différent. Par exemple, l’octet 0x80 est converti en U+0080 sur Windows, mais peut devenir U+30FB sur Linux et macOS, suivant la version d’iconv.

Dans ODBC Driver 13 et 13.1, quand des caractères multioctets UTF-8 ou des substituts UTF-16 sont répartis entre les mémoires tampons SQLPutData, les données sont altérées. Utilisez des mémoires tampons pour diffuser en continu les SQLPutData qui ne se terminent pas dans des encodages de caractères partiels. Cette restriction a été supprimée avec ODBC Driver 17.

## <a name="openssl"></a><a name="bkmk-openssl"></a>OpenSSL
À partir de la version 17.4, le pilote charge OpenSSL de manière dynamique, ce qui lui permet de s’exécuter sur des systèmes dotés de la version 1.0 ou 1.1 sans avoir besoin de fichiers de pilote distincts. Lorsqu’il y a plusieurs versions d’OpenSSL, le pilote tente de charger la dernière version. Le pilote prend actuellement en charge OpenSSL 1.0.x et 1.1.x

> [!NOTE]  
> Un conflit potentiel peut se produire si l’application qui utilise le pilote (ou l’un de ses composants) est liée à une version différente d’OpenSSL ou la charge dynamiquement. S’il y a plusieurs versions d’OpenSSL sur le système et que l’application l’utilise, il est fortement recommandé de s’assurer que la version chargée par l’application et le pilote ne sont pas incompatibles, car les erreurs peuvent corrompre la mémoire et, par conséquent, ne pas forcément se manifester de manière évidente ou cohérente.

## <a name="alpine-linux"></a><a name="bkmk-alpine"></a>Alpine Linux
Au moment de la rédaction de cet article, la taille de la pile par défaut dans MUSL est de 128 Ko, ce qui est suffisant pour les fonctionnalités de base du pilote ODBC. Toutefois, en fonction de ce que fait l’application, cette limite peut être facilement dépassée, en particulier lorsque le pilote est appelé à partir de plusieurs threads. Il est recommandé de compiler une application ODBC sur Alpine Linux avec `-Wl,-z,stack-size=<VALUE IN BYTES>` pour augmenter la taille de la pile. Pour référence, la taille de la pile par défaut sur la plupart des systèmes GLIBC est de 2 Mo.

## <a name="additional-notes"></a>Remarques supplémentaires  

1.  Vous pouvez établir une connexion administrateur dédiée (DAC) à l’aide de l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] et de **port,hôte**. Un membre du rôle Sysadmin doit d’abord découvrir le port DAC. Consultez [Connexion de diagnostic pour les administrateurs de base de données](../../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md#dac-port) pour découvrir la procédure à suivre. Par exemple, si le port DAC était 33000, vous pourriez vous y connecter avec `sqlcmd` comme suit :  

    ```
    sqlcmd -U <user> -P <pwd> -S <host>,33000
    ```

    > [!NOTE]  
    > Les connexions DAC doivent utiliser l’authentification [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)].  
    
2.  Le Gestionnaire de pilotes UnixODBC retourne « Identificateur d’option/d’attribut non valide » pour tous les attributs d’instruction quand ils sont passés par le biais de SQLSetConnectAttr. Sur Windows, quand SQLSetConnectAttr reçoit une valeur d’attribut d’instruction, le pilote est amené à définir cette valeur sur toutes les instructions actives qui sont des enfants du handle de connexion.  

3.  Lorsque le pilote est utilisé avec des applications fortement multithread, la validation du descripteur d’unixODBC peut devenir un goulot d’étranglement de performances. Dans de tels scénarios, il est possible d’accroître significativement la performance en compilant unixODBC avec l’option `--enable-fastvalidate`. Toutefois, n’oubliez pas que ceci peut entraîner le blocage des applications qui transmettent des descripteurs non valides aux API ODBC au lieu de retourner des erreurs `SQL_INVALID_HANDLE`.

## <a name="see-also"></a>Voir aussi  
[Questions fréquentes (FAQ)](frequently-asked-questions-faq-for-odbc-linux.yml)

[Problèmes connus dans cette version du pilote](known-issues-in-this-version-of-the-driver.md)

[Notes de publication](release-notes-odbc-sql-server-linux-mac.md)
