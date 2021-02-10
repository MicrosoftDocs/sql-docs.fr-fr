---
description: Création de fichiers de script (SybaseToSQL)
title: Création de fichiers de script (SybaseToSQL) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Configuring Settings
- Sybase Console,Script Commands
- Sybase Console,Script File Validation
- Sybase Console,Server Connection Parameters
ms.assetid: e6baf106-abbd-4200-b3de-33b4b4f1b294
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b3f417752ef7de6ab5c4fef077eb9771d7454996
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100057194"
---
# <a name="creating-script-files-sybasetosql"></a>Création de fichiers de script (SybaseToSQL)
La première étape avant le lancement de l’application de console SSMA consiste à créer le fichier de script et, si nécessaire, à créer le fichier de valeur de variable et le fichier de connexion au serveur.  
  
Le fichier de script peut être divisé en trois sections, à savoir :  
  
1.  **Configuration :** Permet à l’utilisateur de définir les paramètres de configuration de l’application console.  
  
2.  **serveurs :** Permet à l’utilisateur de définir les définitions de serveur source/cible. Il peut également s’agir d’un fichier de connexion au serveur distinct.  
  
3.  **script-commandes :** Permet à l’utilisateur d’exécuter des commandes de workflow SSMA.  
  
Chaque section est décrite en détail ci-dessous :  
  
## <a name="configuring-sybase-console-settings"></a>Configuration des paramètres de la console Sybase  
Les configurations d’un script s’affichent dans le fichier de script de la console.  
  
Si l’un des éléments est spécifié dans le nœud de configuration, ils sont définis en tant que paramètres globaux, c’est-à-dire qu’ils s’appliquent à toutes les commandes de script. Ces éléments de configuration peuvent également être définis dans chaque commande de la section script-Command si l’utilisateur souhaite remplacer le paramètre global.  
  
Les options configurables par l’utilisateur sont les suivantes :  
  
1.  **Fournisseur de fenêtre Sortie :** Si l’attribut Suppress-messages a la valeur « true », les messages spécifiques à la commande ne sont pas affichés sur la console. La description des attributs est indiquée ci-dessous :  
  
    -   destination : spécifie si la sortie doit être imprimée dans un fichier ou stdout. Il s’agit de la valeur false par défaut.  
  
    -   nom-fichier : chemin d’accès du fichier (facultatif).  
  
    -   supprimer-messages : supprime les messages sur la console. Il s’agit de la valeur par défaut « false ».  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <output-window  
  
        suppress-messages="<true/false>"   (optional)  
  
        destination="<file/stdout>"        (optional)  
  
        file-name="<file-name>"            (optional)  
  
       />  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <output-window  
  
         suppress-messages="<true/false>"   (optional)  
  
         destination="<file/stdout>"        (optional)  
  
         file-name="<file-name>"            (optional)  
  
       />  
  
    </...All commands...>  
    ```  
  
2.  **Fournisseur de connexion pour la migration de données :** Spécifie le serveur source/cible à prendre en compte pour la migration des données.  Source-use-Last-Used indique que le dernier serveur source utilisé est utilisé pour la migration des données. De la même façon, la cible-use-Last-Used indique que le dernier serveur cible utilisé est utilisé pour la migration des données. L’utilisateur peut également spécifier le serveur (source ou cible) à l’aide des attributs serveur source ou serveur cible.  
  
    Un seul attribut spécifié peut être utilisé, par exemple :  
  
    -   source-use-Last-Used = "true" (valeur par défaut) ou source-Server = "source_servername"  
  
    -   cible-use-Last-Used = "true" (valeur par défaut) ou target-Server = "target_servername"  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <data-migration-connection   source-use-last-used="true"  
  
                                   target-server="<target-server-unique-name>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <migrate-data>  
  
      <data-migration-connection   source-server="<source-server-unique-name>"  
  
                                   target-use-last-used="true"/>  
  
    </migrate-data>  
    ```  
  
3.  **Fenêtre contextuelle entrée d’utilisateur :** Cela permet de gérer les erreurs, lorsque les objets sont chargés à partir de la base de données. L’utilisateur fournit les modes de saisie et, en cas d’erreur, la console se présente comme l’indique l’utilisateur.  
  
    Les modes sont les suivants :  
  
    -   **ask-User-** Demande à l’utilisateur de continuer (« oui ») ou d’erreur (« non »).  
  
    -   **erreur :** La console affiche une erreur et interrompt l’exécution.  
  
    -   **Continuer-** La console se poursuit avec l’exécution.  
  
    Le mode par défaut est **Error**.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!-- Connect to target database -->  
  
    <connect-target-database server="<target-server-unique-name>">  
  
      <user-input-popup mode="<ask-user/continue/error>"/>  
  
    </connect-target-database>  
    ```  
  
4.  **Fournisseur de reconnexion :** Cela permet à l’utilisateur de définir les paramètres de reconnexion en cas d’échec de la connexion. Cette valeur peut être définie pour les serveurs source et cible.  
  
    Les modes de reconnexion sont les suivants :  
  
    -   reconnexion-to-Last-Used-Server : si la connexion n’est pas active, elle tente de se reconnecter au dernier serveur utilisé au plus 5 fois.  
  
    -   Generate-a-error : si la connexion n’est pas active, une erreur est générée.  
  
    Le mode par défaut est **Generate-a-Error**.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <reconnect-manager  on-source-reconnect="<reconnect-to-last-used-server/generate-an-error>"  
  
                          on-target-reconnect="<reconnect-to-last-used-server/generate-an-error>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <!--synchronization-->  
  
    <synchronize-target>  
  
      <reconnect-manager on-target-reconnect="reconnect-to-last-used-server"/>  
  
    </synchronize-target>  
    ```  
    *or*  
  
    ```xml  
    <!--data migration-->  
  
    <migrate-data server="<target-server-unique-name>">  
  
      <reconnect-manager  
  
        on-source-reconnect="reconnect-to-last-used-server"  
  
        on-target-reconnect="generate-an-error"/>  
  
    </migrate-data>  
    ```  
  
5.  **Fournisseur de remplacement de convertisseur :** Cela permet à l’utilisateur de gérer les objets qui sont déjà présents dans la métabase cible. Les actions possibles sont les suivantes :  
  
    -   erreur : la console affiche une erreur et interrompt l’exécution.  
  
    -   remplacement : remplace les valeurs d’objet existantes. Cette action est effectuée par défaut.  
  
    -   ignorer : la console ignore les objets qui existent déjà dans la base de données  
  
    -   Ask-User : invite l’utilisateur à entrer une valeur (« oui »/« non »)  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <convert-schema object-name="<object-name>">  
  
      <object-overwrite action="<error/skip/overwrite/ask-user>"/>  
  
    </convert-schema>  
    ```  
  
6.  **Fournisseur de composants requis en échec :** Cela permet à l’utilisateur de gérer les composants requis pour le traitement d’une commande. Par défaut, le mode strict est « false ». Si la valeur est « true », une exception est générée pour ne pas satisfaire aux conditions préalables.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <prerequisites strict-mode="<true/false>"/>  
  
    </output-providers>  
    ```  
  
7.  **Arrêter l’opération :** Pendant la mi-opération, si l’utilisateur veut arrêter l’opération, la touche de raccourci **« Ctrl + C »** peut être utilisée. SSMA pour la console Sybase attend la fin de l’opération et met fin à l’exécution de la console.  
  
    Si l’utilisateur veut arrêter immédiatement l’exécution, alors la touche **Ctrl + C** peut être appuyée pour terminer brutalement l’application console SSMA  
  
8.  **Fournisseur de progression :** Indique la progression de chaque commande de console. Elle est désactivée par défaut. Les attributs de rapport de progression comprennent :  
  
    -   arrêt  
  
    -   toutes les-1%  
  
    -   toutes les 2%  
  
    -   toutes les 5%  
  
    -   toutes les 10%  
  
    -   toutes les 20%  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <progress-reporting enable="<true/false>"           (optional)  
  
                          report-messages="<true/false>"  (optional)  
  
                          report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off" (optional)/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <progress-reporting  
  
        enable="<true/false>"          (optional)  
  
        report-messages="<true/false>" (optional)  
  
        report-progress="every-1%/every-2%/every-5%/every-10%/every-20%/off (optional)/>  
  
    </...All commands...>  
    ```  
  
9. **Commentaires du journal :** Définit le niveau de détail du journal. Cela correspond à l’option toutes les catégories dans l’interface utilisateur. Par défaut, le niveau de détail du journal est « erreur ».  
  
    Les options au niveau du journal sont les suivantes :  
  
    -   Fatal-erreur : seuls les messages d’erreur fatale sont journalisés.  
  
    -   erreur : seuls les messages d’erreur et d’erreur fatale sont journalisés.  
  
    -   AVERTISSEMENT : tous les niveaux, à l’exception des messages de débogage et d’informations, sont journalisés.  
  
    -   info : tous les niveaux, à l’exception des messages de débogage, sont journalisés.  
  
    -   débogage : tous les niveaux de messages journalisés.  
  
    > [!NOTE]  
    > Les messages obligatoires sont journalisés à n’importe quel niveau.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </output-providers>  
    ```  
    *or*  
  
    ```xml  
    <...All commands...>  
  
      <log-verbosity level="fatal-error/error/warning/info/debug"/>  
  
    </...All commands...>  
    ```  
  
10. **Remplacer le mot de passe chiffré :** Si la valeur est « true », le mot de passe en texte clair spécifié dans la section de définition de serveur du fichier de connexion au serveur ou dans le fichier de script, remplace le mot de passe chiffré stocké dans le stockage protégé s’il existe. Si aucun mot de passe n’est spécifié en texte clair, l’utilisateur est invité à entrer le mot de passe.  
  
    Ici, deux cas se produisent :  
  
    1.  Si l’option de remplacement a la **valeur false**, l’ordre de recherche est protégé fichier de script de stockage-fichier de &gt; connexion au serveur-utilisateur d' &gt; &gt; invite.  
  
    2.  Si l’option de remplacement a la **valeur true**, l’ordre de recherche sera fichier de script-fichier de &gt; connexion du serveur- &gt; inviter l’utilisateur.  
  
    **Exemple :**  
  
    ```xml  
    <output-providers>  
  
      <encrypted-password override="<true/false>"/>  
  
    </output-providers>  
    ```  
  
L’option non configurable est la suivante :  
  
-   **Nombre maximal de tentatives de reconnexion :** Lorsqu’une connexion établie expire ou s’interrompt en raison d’une défaillance du réseau, le serveur doit être reconnecté. Les tentatives de reconnexion sont autorisées à un maximum de **5** tentatives après laquelle, la console effectue automatiquement la reconnexion. La fonctionnalité de reconnexion automatique réduit votre effort de réexécution du script.  
  
## <a name="server-connection-parameters"></a>Paramètres de connexion au serveur  
Les paramètres de connexion au serveur peuvent être définis dans le fichier de script ou dans le fichier de connexion au serveur. Pour plus d’informations, reportez-vous à la section [création de fichiers de connexion au serveur &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md) .  
  
## <a name="script-commands"></a>Commandes de script  
Le fichier de script contient une séquence de commandes de flux de travail de migration au format XML. L’application console SSMA traite la migration dans l’ordre des commandes figurant dans le fichier de script.  
  
Par exemple, une migration de données classique d’une table spécifique dans une base de données Sybase suit la hiérarchie de : Database- &gt; Schema- &gt; table.  
  
Lorsque toutes les commandes du fichier de script sont exécutées avec succès, l’application de console SSMA s’arrête et retourne le contrôle à l’utilisateur. Le contenu d’un fichier de script est plus ou moins statique avec des informations variables contenues dans des [fichiers de valeurs de variables](creating-variable-value-files-sybasetosql.md) ou dans une section distincte du fichier de script pour les valeurs de variables.  
  
**Exemple :**  
  
```xml  
<!--Sample of script file commands -->  
  
<ssma-script-file>  
  
  <script-commands>  
  
    <create-new-project project-folder="<project-folder>"  
  
                        project-name="<project-name>"  
  
                        overwrite-if-exists="<true/false>"/>  
  
    <connect-source-database server="<source-server-unique-name>"/>  
  
    <save-project/>  
  
    <close-project/>  
  
  </script-commands>  
  
</ssma-script-file>  
```  
Les modèles constitués de 3 fichiers de script (pour l’exécution de différents scénarios), d’un fichier de valeurs de variable et d’un fichier de connexion au serveur sont fournis dans le dossier exemple de scripts de console du répertoire du produit :  
  
-   AssessmentReportGenerationSample.xml  
  
-   ConversionAndDataMigrationSample.xml  
  
-   SqlStatementConversionSample.xml  
  
-   VariableValueFileSample.xml  
  
-   ServersConnectionFileSample.xml  
  
Vous pouvez exécuter les modèles (fichiers) après avoir modifié les paramètres affichés ici pour des critères de pertinence.  
  
La liste complète des commandes de script se trouve dans [exécution de la console SSMA &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md)  
  
## <a name="script-file-validation"></a>Validation du fichier de script  
L’utilisateur peut facilement valider son fichier de script par rapport à la **2ssconsolescriptschema. xsd du fichier de** définition de schéma disponible dans le dossier « schemas ».  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante de l’utilisation de la console consiste à [créer des fichiers de valeurs de variables &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md).  
  
## <a name="see-also"></a>Voir aussi  
[Création de fichiers de valeurs de variables &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
  
