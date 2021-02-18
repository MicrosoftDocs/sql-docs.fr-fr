---
title: Utilitaire profiler
description: L’utilitaire Profiler lance l’outil SQL Server Profiler. Les arguments facultatifs vous permettent de contrôler le démarrage de l’application.
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: tools-other
ms.topic: conceptual
helpviewer_keywords:
- command prompt utilities [SQL Server], profiler90 utility
- profiler90 utility
- Profiler [SQL Server Profiler], starting
- SQL Server Profiler, starting
- starting SQL Server Profiler
ms.assetid: e91c30a9-0d29-4f84-bcb8-e8fb62afadda
author: markingmyname
ms.author: maghan
ms.openlocfilehash: ab40ea190dab072d7415ded1f399cfa4a4b78568
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100335943"
---
# <a name="profiler-utility"></a>Utilitaire profiler
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
  L’utilitaire **profiler** lance l’outil [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)] . Les arguments facultatifs répertoriés ci-dessous dans cette rubrique permettent de contrôler le démarrage de l'application.  
  
> [!NOTE]  
>  L’utilitaire **profiler** n’est pas destiné à générer des scripts de trace. Pour en savoir plus, voir [SQL Server Profiler](../tools/sql-server-profiler/sql-server-profiler.md).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
profiler  
    [ /? ] |  
[  
{  
{ /U login_id [ /P password ] }  
| /E  
}  
{[ /S sql_server_name ] | [ /A analysis_services_server_name ] }  
[ /D database ]  
[ /T "template_name" ]  
[ /B { "trace_table_name" } ]  
{ [/F "filename" ] | [ /O "filename" ] }  
[ /L locale_ID ]  
[ /M "MM-DD-YY hh:mm:ss" ]  
[ /R ]  
[ /Z file_size ]  
]  
```  
  
## <a name="arguments"></a>Arguments  
 **/?**  
 Affiche le résumé de la syntaxe des arguments de **profiler** .  
  
 **/U** *login_id*  
 ID de connexion de l'utilisateur pour l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Les ID de connexion respectent la casse.  
  
> [!NOTE]  
>  [!INCLUDE[ssNoteWinAuthentication](../includes/ssnotewinauthentication-md.md)].  
  
 **/P** *mot de passe*  
 Indique un mot de passe spécifié par l'utilisateur pour l'authentification [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] .  
  
 **/E**  
 Spécifie la connexion avec l'authentification Windows en utilisant les informations d'identification de l'utilisateur actuel.  
  
 **/S**  *sql_server_name*  
 Spécifie une instance de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]. Profiler se connecte automatiquement au serveur spécifié en utilisant les informations d’authentification spécifiées dans les commutateurs **/U** et **/P** ou dans le commutateur **/E** . Pour vous connecter à une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)], utilisez **/S** *sql_server_name*\\*instance_name*.  
  
 **/A**  *analysis_services_server_name*  
 Spécifie une instance d'Analysis Services. Profiler se connecte automatiquement au serveur spécifié en utilisant les informations d’authentification spécifiées dans les commutateurs **/U** et **/P** ou dans le commutateur **/E** . Pour vous connecter à une instance nommée de [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] , utilisez **/A** *analysis_services_server_name\instance_name*.  
  
 **/D** *base de données*  
 Spécifie le nom de la base de données à utiliser avec la connexion. Cette option sélectionne la base de données par défaut pour l'utilisateur spécifié si aucune base de données n'est spécifiée.  
  
 **/B "** *trace_table_name* **"**  
 Spécifie une table de trace à charger lors du démarrage du générateur de profils. Vous devez spécifier la base de données, l'utilisateur ou le schéma, ainsi que la table.  
  
 **/T"** *template_name* **"**  
 Spécifie le modèle qui sera chargé pour configurer la trace. Le nom du modèle doit être entre guillemets. Le nom du modèle doit être le répertoire de modèles système ou le répertoire de modèles utilisateur. Si deux modèles portant le même nom existent dans les deux répertoires, le modèle du répertoire système est chargé. Si aucun modèle n'existe sous le nom spécifié, le modèle standard est chargé. Notez que l’extension de fichier du modèle (.tdf) ne doit pas être spécifiée dans *template_name*. Par exemple :  
  
```  
/T "standard"  
```  
  
 **/F"** *nom de fichier* **"**  
 Spécifie le chemin et le nom d'un fichier de trace à charger lors du démarrage du générateur de profils. Le chemin complet et le nom de fichier doivent être entre guillemets. Cette option n’est pas utilisable avec **/O**.  
  
 **/O "** *nom de fichier*  **"**  
 Spécifie le chemin d'accès et le nom d'un fichier dans lequel les résultats de trace doivent être écrits. Le chemin complet et le nom de fichier doivent être entre guillemets. Cette option n’est pas utilisable avec **/F**.  
  
 **/L** *locale_ID*  
 Non disponible.  
  
 **/M "** *MM-DD-YY hh:mm:ss* **"**  
 Spécifie la date et l'heure auxquelles la trace doit s'arrêter. L'heure d'arrêt doit être indiquée entre guillemets. Spécifiez l'heure d'arrêt conformément aux paramètres du tableau ci-dessous :  
  
|Paramètre|Définition|  
|---------------|----------------|  
|MM|Mois à deux chiffres|  
|DD|Jour à deux chiffres|  
|YY|Année à deux chiffres|  
|hh|Heure à deux chiffres au format 24 heures|  
|MM|Minute à deux chiffres|  
|ss|Seconde à deux chiffres|  
  
> [!NOTE]  
>  Le format « MM-DD-YY hh:mm:ss » peut être utilisé uniquement si l’option **Utiliser des paramètres régionaux pour afficher les valeurs de date et d’heure** est activée dans [!INCLUDE[ssSqlProfiler](../includes/sssqlprofiler-md.md)]. Si cette option n'est pas activée, vous devez utiliser le format de date et heure « YYYY-MM-DD hh:mm:ss ».  
  
 **/R**  
 Permet la substitution du fichier de trace.  
  
 **/Z**  *file_size*  
 Spécifie la taille du fichier de trace, en mégaoctets (Mo). La taille par défaut est de 5 Mo. Si la substitution est activée, tous les fichiers de substitution sont limités à la valeur spécifiée dans cet argument.  
  
## <a name="remarks"></a>Notes  
 Pour démarrer une trace avec un modèle spécifique, utilisez les options **/S** et **/T** simultanément. Par exemple, pour démarrer une trace en utilisant le modèle Standard sur MyServer\MyInstance, entrez ce qui suit à l'invite de commandes :  
  
```  
profiler /S MyServer\MyInstance /T "Standard"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’utilitaire d’invite de commandes &#40;moteur de base de données&#41;](../tools/command-prompt-utility-reference-database-engine.md)  
  
  
