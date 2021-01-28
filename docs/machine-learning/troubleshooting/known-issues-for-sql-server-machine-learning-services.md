---
title: Problèmes connus pour Python et R
description: Cet article décrit les problèmes connus et les limitations des composants Python et R fournis dans SQL Server Machine Learning Services et SQL Server R 2016 R Services.
ms.prod: sql
ms.technology: machine-learning-services
ms.date: 10/13/2020
ms.topic: troubleshooting
author: dphansen
ms.author: davidph
ms.custom: contperf-fy20q4
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15'
ms.openlocfilehash: 51d729bcdb25900af855e7429e953ee44dc1388b
ms.sourcegitcommit: 108bc8e576a116b261c1cc8e4f55d0e0713d402c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/25/2021
ms.locfileid: "98766301"
---
# <a name="known-issues-in-sql-server-machine-learning-services"></a>Problèmes connus dans SQL Server Machine Learning Services
[!INCLUDE [SQL Server 2016 and later](../../includes/applies-to-version/sqlserver2016.md)]

Cet article décrit les problèmes connus et les limitations des composants Python et R fournis dans [SQL Server Machine Learning Services](../sql-server-machine-learning-services.md) et [SQL Server R 2016 R Services](../r/sql-server-r-services.md).

## <a name="setup-and-configuration-issues"></a>Problèmes d’installation et de configuration

Pour obtenir une description des processus liés à l’installation initiale et à la configuration, consultez [Installer SQL Server Machine Learning Services](../install/sql-machine-learning-services-windows-install.md). Vous y trouverez des informations sur les mises à niveau, l’installation côte à côte et l’installation de nouveaux composants R ou Python.

### <a name="1-inconsistent-results-in-mkl-computations-due-to-missing-environment-variable"></a>1. Résultats incohérents dans les calculs MKL en raison d’une variable d’environnement manquante

**S’applique à :** Binaires R_SERVER version 9.0, 9.1, 9.2 ou 9.3.

R_SERVER utilise la bibliothèque Intel Math Kernel Library (MKL). Pour les calculs impliquant MKL, les résultats peuvent manquer de cohérence en l’absence d’une variable d’environnement dans votre système. 

Définissez la variable d’environnement `'MKL_CBWR'=AUTO` pour assurer une reproductibilité numérique conditionnelle dans R_SERVER. Pour plus d’informations, consultez [Introduction to Conditional Numerical Reproducibility (CNR)](https://software.intel.com/articles/introduction-to-the-conditional-numerical-reproducibility-cnr).

**Solution de contournement**

1. Dans le Panneau de configuration, cliquez sur **Système et sécurité** > **Système** > **Paramètres système avancés** > **Variables d’environnement**.

2. Créez une variable Utilisateur ou Système. 

   + Nommez la variable « MKL_CBWR ».
   + Définissez « Valeur de la variable » sur « AUTO. ».

3. Redémarrez R_SERVER. Sur SQL Server, vous pouvez redémarrer le service SQL Server Launchpad.

> [!NOTE]
> Si vous exécutez SQL Server 2019 sur Linux, modifiez ou créez *.bash_profile* dans votre répertoire racine utilisateur en ajoutant la ligne `export MKL_CBWR="AUTO"`. Exécutez ce fichier en saisissant `source .bash_profile` à l’invite de commandes bash. Redémarrez R_SERVER en tapant `Sys.getenv()` à l’invite de commandes R.

### <a name="2-r-script-runtime-error-sql-server-2017-cu5-cu7-regression"></a>2. Erreur d’exécution de script R (régression dans les mises à jour cumulatives CU5-CU7 de SQL Server 2017)

Pour SQL Server 2017, dans les mises à jour cumulatives 5 à 7, il existe une régression dans le fichier **rlauncher.config**, où le chemin du fichier du répertoire temporaire comporte un espace. Cette régression a été corrigée dans les mises à jour cumulatives CU8.

L’erreur qui s’affiche pendant l’exécution du script R comprend les messages suivants :

> *Impossible de communiquer avec le runtime pour le script « R ». Vérifiez les conditions requises du runtime « R ».*
>
> Message(s) STDERR provenant du script externe : 
>
> *Erreur irrécupérable : impossible de créer « R_TempDir »*

**Solution de contournement**

Appliquez les mises à jour cumulatives CU8 dès qu’elles sont disponibles. Sinon, recréez **rlauncher.config** en exécutant **registerrext** avec uninstall/install dans une invite de commandes avec élévation de privilèges. 

```cmd
<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /uninstall /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>

<SQLInstancePath>\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRExt.exe /install /sqlbinnpath:<SQLInstanceBinnPath> /userpoolsize:0 /instance:<SQLInstanceName>
```

L’exemple suivant montre les commandes avec l’installation de l’instance par défaut « MSSQL14. MSSQLSERVER » dans le répertoire C:\Program Files\Microsoft SQL Server\" :

```cmd
"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /uninstall /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER

"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\R_SERVICES\library\RevoScaleR\rxLibs\x64\RegisterRext.exe" /install /sqlbinnpath:"C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\MSSQL\Binn" /userpoolsize:0 /instance:MSSQLSERVER
```

### <a name="3-unable-to-install-sql-server-machine-learning-features-on-a-domain-controller"></a>3. Impossible d’installer les fonctionnalités de Machine Learning de SQL Server sur un contrôleur de domaine

Si vous essayez d’installer SQL Server 2016 R Services ou SQL Server Machine Learning Services sur un contrôleur de domaine, l’installation échoue avec les erreurs suivantes :

> *Une erreur s’est produite lors du processus d’installation du composant*
>
> *Impossible de trouver le groupe avec l’identité*
>
> *Code d’erreur du composant : 0x80131509*

L’échec se produit, car sur un contrôleur de domaine, le service ne peut pas créer les 20 comptes locaux nécessaires à l’exécution du Machine Learning. En général, nous vous déconseillons d’installer SQL Server sur un contrôleur de domaine. Pour plus d’informations, consultez [Bulletin de support 2032911](https://support.microsoft.com/help/2032911/you-may-encounter-problems-when-installing-sql-server-on-a-domain-cont).

### <a name="4-install-the-latest-service-release-to-ensure-compatibility-with-microsoft-r-client"></a>4. Installer la dernière version du service pour garantir la compatibilité avec Microsoft R Client

Si vous installez la dernière version de Microsoft R Client et que vous vous en servez pour exécuter R sur SQL Server dans un contexte de calcul distant, il est possible que vous obteniez une erreur de ce type :

> *Vous exécutez la version 9.x.x de Microsoft R Client sur votre ordinateur, qui n’est pas compatible avec la version 8.x.x de Microsoft R Server. Téléchargez et installez une version compatible.*

SQL Server 2016 exige que les bibliothèques R du client correspondent exactement à celles du serveur. La restriction a été retirée pour les versions postérieures à R Server 9.0.1. Cependant, si vous rencontrez cette erreur, vérifiez la version des bibliothèques R utilisée par votre client et le serveur et, si nécessaire, mettez à jour le client pour que sa version corresponde à celle du serveur.

La version de R qui est installée avec SQL Server R Services est mise à jour chaque fois qu’une version du service SQL Server est installée. Pour toujours avoir les versions les plus récentes des composants R, veillez à installer tous les Service Packs.

Pour assurer la compatibilité avec Microsoft R Client 9.0.0, installez les mises à jour qui sont décrites dans cet [article de support](https://support.microsoft.com/kb/3210262).

Pour éviter tout problème avec les packages R, vous pouvez aussi mettre à niveau la version des bibliothèques R installées sur le serveur en modifiant votre contrat de maintenance de façon à adhérer à la politique de support du cycle de vie moderne, comme décrit dans [la section suivante](#bkmk_sqlbindr). Dans ce cas, la version de R installée avec SQL Server est mise à jour selon la même planification que celle utilisée pour les mises à jour de Machine Learning Server (anciennement Microsoft R Server).

**S’applique à :** SQL Server 2016 R Services, avec R Server version 9.0.0 ou antérieure

### <a name="5-r-components-missing-from-cu3-setup"></a>5. Composants R absents du programme d’installation de CU3

Un nombre limité de machines virtuelles Azure a été provisionné sans les fichiers d’installation R qui doivent être inclus avec SQL Server. Le problème concerne les machines virtuelles provisionnées entre le 05/01/2018 et le 23/01/2018. Ce problème peut aussi affecter les installations locales si vous avez appliqué la mise à jour CU3 pour SQL Server 2017 entre le 05/01/2018 et le 23/01/2018.

Une version du service comprenant la bonne version des fichiers d’installation R a été mise à disposition.

+ [Package de mises à jour cumulatives 3 pour SQL Server 2017 KB4052987](https://www.microsoft.com/download/details.aspx?id=56128).

Pour installer les composants et réparer SQL Server 2017 CU3, vous devez désinstaller CU3 et réinstaller la version mise à jour :

1. Téléchargez le fichier d’installation mis à jour de CU3, qui comprend les programmes d’installation de R.
2. Désinstallez CU3. Dans le panneau de configuration, recherchez **Désinstaller une mise à jour**, puis sélectionnez « Correctif logiciel 3015 pour SQL Server 2017 (KB4052987) (64 bits) ». Effectuez les étapes de désinstallation.
3. Réinstallez la mise à jour CU3 en double-cliquant sur la mise à jour KB4052987 que vous venez de télécharger : `SQLServer2017-KB4052987-x64.exe`. Suivez les instructions d'installation.

### <a name="6-unable-to-install-python-components-in-offline-installations-of-sql-server-2017-ctp-20-or-later"></a>6. Impossible d’installer les composants Python dans les installations hors connexion de SQL Server 2017 CTP 2.0 ou version ultérieure

Si vous installez une version préliminaire de SQL Server 2017 sur un ordinateur sans accès Internet, il est possible que le programme d’installation n’affiche pas la page qui demande l’emplacement des composants Python téléchargés. En pareil cas, vous pouvez installer la fonctionnalité Machine Learning Services, mais pas les composants Python.

Ce problème est résolu dans la version publiée. De même, cette limitation ne s’applique pas aux composants R.

**S’applique à :** SQL Server 2017 avec Python

### <a name="warning-of-incompatible-version-when-you-connect-to-an-older-version-of-sql-server-r-services-from-a-client-by-using-sssqlv14_md"></a><a name="bkmk_sqlbindr"></a> Avertissement d’incompatibilité de version quand vous vous connectez à une ancienne version de SQL Server R Services à partir d’un client en utilisant [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)]

Quand vous exécutez du code R dans un contexte de calcul SQL Server 2016, l’erreur suivante risque de s’afficher :

> *Vous exécutez la version 9.0.0 de Microsoft R Client sur votre ordinateur, ce qui est incompatible avec la version 8.0.3 de Microsoft R Server. Téléchargez et installez une version compatible.*

Ce message s’affiche si l’un des deux énoncés suivants est vrai,

+ Vous avez installé R Server (autonome) sur un ordinateur client à l’aide de l’Assistant Installation de [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)].
+ Vous avez installé Microsoft R Server à l’aide du [programme Windows Installer distinct](/machine-learning-server/install/r-server-install-windows).

Pour faire en sorte que le serveur et le client utilisent la même version, vous devrez peut-être avoir recours à la _liaison_, prise en charge pour Microsoft R Server 9.0 et les versions ultérieures, afin de mettre à niveau les composants R dans les instances SQL Server 2016. Pour déterminer si une prise en charge est disponible pour les mises à niveau de votre version de R Services, consultez [Mettre à niveau une instance de R Services à l’aide de SqlBindR.exe](../install/upgrade-r-and-python.md).

**S’applique à :** SQL Server 2016 R Services, avec R Server version 9.0.0 ou antérieure

### <a name="7-setup-for-sql-server-2016-service-releases-might-fail-to-install-newer-versions-of-r-components"></a>7. La configuration des versions de services de SQL Server 2016 peut échouer à installer de nouvelles versions des composants R

Si vous installez une mise à jour cumulative ou un Service Pack pour SQL Server 2016 sur un ordinateur qui n’est pas connecté à Internet, l’Assistant Installation risque de ne pas pouvoir afficher l’invite permettant de mettre à jour les composants R à l’aide des fichiers CAB téléchargés. Cet échec se produit généralement quand plusieurs composants ont été installés en même temps que le moteur de base de données.

Pour contourner ce problème, vous pouvez installer la version du service depuis la ligne de commande et spécifier l’argument `MRCACHEDIRECTORY`, comme dans cet exemple, qui installe les mises à jour CU1 :

`C:\<path to installation media>\SQLServer2016-KB3164674-x64.exe /Action=Patch /IACCEPTROPENLICENSETERMS /MRCACHEDIRECTORY=<path to CU1 CAB files>`

Pour obtenir les derniers programmes d’installation, consultez [Installer les composants Machine Learning sans accès à Internet](../install/sql-ml-component-install-without-internet-access.md).

**S’applique à :** SQL Server 2016 R Services, avec R Server version 9.0.0 ou antérieure

### <a name="8-launchpad-services-fails-to-start-if-the-version-is-different-from-the-r-version"></a>8. Les services Launchpad ne parviennent pas à démarrer si la version est différente de la version de R

Si vous installez SQL Server R Services séparément du moteur de base de données, et que les versions de build sont différentes, l’erreur suivante risque de figurer dans le journal des événements système :

> *Le service SQL Server Launchpad n’a pas pu démarrer en raison de l’erreur suivante : Le service n’a pas répondu assez vite à la demande de lancement ou de contrôle.*

Par exemple, cette erreur peut se produire si vous installez le moteur de base de données en utilisant la version publiée, appliquez un correctif pour mettre à niveau le moteur de base de données, puis ajoutez le composant R Services en utilisant la version publiée.

Pour éviter ce problème, utilisez un utilitaire comme le Gestionnaire de fichiers pour comparer les versions de Launchpad. exe à la version des fichiers binaires SQL, par exemple sqldk.dll. Tous les composants doivent avoir le même numéro de version. Si vous mettez à niveau un composant, veillez à appliquer la même mise à niveau à tous les autres composants installés.

Recherchez Launchpad dans le dossier `Binn` de l’instance. Par exemple, dans une installation par défaut de SQL Server 2016, le chemin peut être `C:\Program Files\Microsoft SQL Server\MSSQL.13.InstanceNameMSSQL\Binn`. 

### <a name="9-remote-compute-contexts-are-blocked-by-a-firewall-in-sql-server-instances-that-are-running-on-azure-virtual-machines"></a>9. Les contextes de calcul à distance sont bloqués par un pare-feu dans les instances SQL Server qui s’exécutant sur des machines virtuelles Azure

Si vous avez installé [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] sur une machine virtuelle Azure, vous risquez de ne pas pouvoir utiliser les contextes de calcul qui nécessite l’utilisation de l’espace de travail de la machine virtuelle. Cela est dû au fait que, par défaut, le pare-feu de la machine virtuelle Azure comporte une règle qui bloque l’accès réseau pour les comptes d’utilisateurs R locaux.

Pour contourner ce problème, sur la machine virtuelle Azure, ouvrez **Pare-feu Windows avec sécurité avancée**, sélectionnez **Règles de trafic sortant**, puis désactivez la règle suivante : **Bloquer l’accès réseau pour les comptes d’utilisateurs locaux R dans l’instance SQL Server MSSQLSERVER**. Vous pouvez laisser la règle activée, mais faites passer la propriété de sécurité à **Autoriser si sécurisé**.

### <a name="10-implied-authentication-in-sqlexpress"></a>10. Authentification implicite dans SQLEXPRESS

Quand vous exécutez des tâches R à partir d’une station de travail de science des données distante en utilisant l’authentification Windows intégrée, SQL Server utilise *l’authentification implicite* pour générer les appels ODBC locaux dont peut avoir besoin le script. Toutefois, cette fonctionnalité ne marchait pas dans la version RTM de SQL Server Express Edition.

Pour résoudre ce problème, nous vous recommandons d’effectuer la mise à niveau vers une version ultérieure du service.

Si la mise à niveau n’est pas possible, en guise de solution de contournement, utilisez une connexion SQL pour exécuter les tâches R à distance qui peuvent nécessiter des appels ODBC incorporés.

**S’applique à :** SQL Server 2016 R Services Express Edition

### <a name="11-performance-limits-when-libraries-used-by-sql-server-are-called-from-other-tools"></a>11. Limites de performances quand des bibliothèques utilisées par SQL Server sont appelées à partir d’autres outils

Il est possible d’appeler les bibliothèques Machine Learning qui sont installées pour SQL Server à partir d’une application externe, comme RGui. C’est peut-être la façon la plus pratique d’effectuer certaines tâches, comme installer de nouveaux packages ou exécuter des tests ad hoc sur des exemples de code très courts. Cependant, en dehors de SQL Server, les performances peuvent être limitées.

Par exemple, même si vous utilisez SQL Server Enterprise Edition, R s’exécute en mode monothread quand vous exécutez votre code R à l’aide d’outils externes. Pour bénéficier de bonnes performances dans SQL Server, lancez une connexion SQL Server et utilisez [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) pour appeler le runtime de script externe.

De manière générale, évitez d’appeler les bibliothèques Machine Learning qui sont utilisées par SQL Server à partir d’outils externes. Si vous avez besoin de déboguer du code R ou Python, il est généralement plus facile de le faire en dehors de SQL Server. Pour obtenir les bibliothèques qui se trouvent dans SQL Server, vous pouvez installer Microsoft R Client ou [SQL Server 2017 Machine Learning Server (autonome)](../install/sql-machine-learning-standalone-windows-install.md).

### <a name="12-sql-server-data-tools-does-not-support-permissions-required-by-external-scripts"></a>12. SQL Server Data Tools ne prend pas en charge les autorisations nécessaires aux scripts externes

Quand vous utilisez Visual Studio ou SQL Server Data Tools pour publier un projet de base de données, si un principal a des autorisations spécifiques pour l’exécution de scripts externes, vous pouvez obtenir une erreur semblable à celle-ci :

> *Modèle TSQL : Erreur détectée lors de la rétroconception de la base de données. L’autorisation n’a pas été reconnue et n’a pas été importée.*

À l’heure actuelle, le modèle DACPAC ne prend pas en charge les autorisations utilisées par R Services ou Machine Learning Services, comme GRANT ANY EXTERNAL SCRIPT ou EXECUTE ANY EXTERNAL SCRIPT. Ce problème sera résolu dans une version ultérieure.

Pour contourner ce problème, exécutez les instructions GRANT supplémentaires dans un script de post-déploiement.

### <a name="13-external-script-execution-is-throttled-due-to-resource-governance-default-values"></a>13. L’exécution des scripts externes est limitée en raison des valeurs par défaut de gouvernance des ressources

Dans Enterprise Edition, vous pouvez utiliser des pools de ressources pour gérer les processus de script externe. Dans les premières builds de certaines versions, la quantité maximale de mémoire qui pouvait être allouée aux processus R était de 20 %. Par conséquent, si le serveur possédait 32 Go de RAM, les fichiers exécutables R (RTerm.exe et BxlServer.exe) pouvaient utiliser au maximum 6,4 Go dans une seule requête.

Si vous rencontrez des limitations de ressources, vérifiez la valeur par défaut actuelle. Si la valeur de 20 % est insuffisante, consultez la documentation de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] pour savoir comment la modifier.

**S’applique à :** SQL Server 2016 R Services, Entreprise Edition

### <a name="14-error-when-using-sp_execute_external_script-without-libcso-on-linux"></a>14. Erreur lors de l’utilisation de `sp_execute_external_script` sans `libc++.so` sur Linux

Sur une machines Linux propre sur laquelle `libc++.so` n’est pas installé, l’exécution d’une requête `sp_execute_external_script` (SPEES) avec Java ou un langage externe échoue, car `commonlauncher.so` ne peut pas charger `libc++.so`.

Par exemple :

```sql
EXECUTE sp_execute_external_script @language = N'Java'
    , @script = N'JavaTestPackage.PassThrough'
    , @parallel = 0
    , @input_data_1 = N'select 1'
WITH RESULT SETS((col1 INT NOT NULL))
GO
```

Cette requête échoue avec un message similaire au suivant :

```text
Msg 39012, Level 16, State 14, Line 0

Unable to communicate with the runtime for 'Java' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Java' runtime.
```

Les journaux `mssql-launchpadd` présentent un message d’erreur similaire au suivant :

```text
Oct 18 14:03:21 sqlextmls launchpadd[57471]: [launchpad] 2019/10/18 14:03:21 WARNING: PopulateLauncher failed: Library /opt/mssql-extensibility/lib/commonlauncher.so not loaded. Error: libc++.so.1: cannot open shared object file: No such file or directory
```

**Solution de contournement**

Vous pouvez essayer l’une des solutions de contournement suivantes :

1. Copiez `libc++*` de `/opt/mssql/lib` vers le chemin système par défaut `/lib64`

1. Ajoutez les entrées suivantes à `/var/opt/mssql/mssql.conf` pour exposer le chemin :

   ```text
   [extensibility]
   readabledirectories = /opt/mssql
   ```

**S’applique à :** SQL Server 2019 sur Linux

### <a name="15-installation-or-upgrade-error-on-fips-enabled-servers"></a>15. Erreur d’installation ou de mise à niveau sur les serveurs compatibles FIPS

Si vous installez SQL Server 2019 avec la fonctionnalité **Machine Learning Services et extensions de langage** ou que vous mettez à niveau l’instance SQL Server sur un serveur compatible [FIPS (Federal Information Processing Standard)](/windows/security/threat-protection/security-policy-settings/system-cryptography-use-fips-compliant-algorithms-for-encryption-hashing-and-signing), vous recevrez l’erreur suivante :

> *Une erreur s’est produite lors de l’installation de la fonctionnalité d’extensibilité. Message d’erreur : Échec de la création AppContainer. Message d’erreur : NONE. État : Cette implémentation ne fait pas partie des algorithmes de chiffrement compatibles FIPS de la plateforme Windows.*

**Solution de contournement**

Désactivez FIPS avant l’installation de SQL Server 2019 avec la fonctionnalité **Machine Learning Services et extensions de langage** ou la mise à niveau de l’instance SQL Server. Une fois l’installation ou la mise à niveau terminée, vous pouvez réactiver FIPS.

**S’applique à :** SQL Server 2019

## <a name="r-script-execution-issues"></a>Problèmes d’exécution des scripts R

Cette section présente des problèmes connus spécifiques à l’exécution de R sur SQL Server ainsi que certains problèmes liés aux bibliothèques et outils R publiés par Microsoft, dont RevoScaleR.

D’autres problèmes connus susceptibles d’affecter les solutions R sont consultables sur le site [Machine Learning Server](/machine-learning-server/resources-known-issues).

### <a name="1-access-denied-warning-when-executing-r-scripts-on-sql-server-in-a-non-default-location"></a>1. Avertissement d’accès refusé pour l’exécution de scripts R sur SQL Server à un emplacement autre que celui par défaut

Si l’instance de SQL Server a été installée à un emplacement autre que celui par défaut, par exemple en dehors du dossier `Program Files`, l’avertissement ACCESS_DENIED est levé quand vous essayez d’exécuter des scripts qui installent un package. Par exemple :

> *Dans `normalizePath(path.expand(path), winslash, mustWork)` : path[2]="~ExternalLibraries/R/8/1": Accès refusé*

La raison en est qu’une fonction R tente de lire le chemin, mais échoue si le groupe d’utilisateurs intégré **SQLRUserGroup** ne dispose pas d’un accès en lecture. L’avertissement qui est levé ne bloque pas l’exécution du script R actuel, mais l’avertissement risque de réapparaître à plusieurs reprises chaque fois que l’utilisateur exécute un autre script R.

Si vous avez installé SQL Server à l’emplacement par défaut, cette erreur ne se produit pas, car tous les utilisateurs Windows ont des autorisations de lecture sur le dossier `Program Files`.

Ce problème sera résolu dans une prochaine version du service. Pour contourner ce problème, attribuez au groupe **SQLRUserGroup** un accès en lecture pour tous les dossiers parents de `ExternalLibraries`.

### <a name="2-serialization-error-between-old-and-new-versions-of-revoscaler"></a>2. Erreur de sérialisation entre les anciennes et nouvelles versions de RevoScaleR

Quand vous transmettez un modèle utilisant un format sérialisé à une instance SQL Server distante, vous pouvez obtenir l’erreur suivante : 

> *Error in memDecompress(data, type = decompress) internal error -3 in memDecompress(2).*

Cette erreur est levée si vous avez enregistré le modèle à l’aide d’une version récente de la fonction de sérialisation, [rxSerializeModel](/machine-learning-server/r-reference/revoscaler/rxserializemodel), mais que l’instance SQL Server dans laquelle vous désérialisez le modèle est dotée d’une version plus ancienne des API RevoScaleR, à partir de SQL Server 2017 CU2 ou version antérieure.

Pour contourner ce problème, vous pouvez mettre à niveau l’instance SQL Server 2017 vers CU3 ou une version ultérieure.

L’erreur ne s’affiche pas si la version de l’API est la même ou si vous déplacez un modèle enregistré avec une fonction de sérialisation plus ancienne sur un serveur qui utilise une version plus récente de l’API de sérialisation.

Autrement dit, veillez à utiliser la même version de RevoScaleR pour les opérations de sérialisation et de désérialisation.

### <a name="3-real-time-scoring-does-not-correctly-handle-the-_learningrate_-parameter-in-tree-and-forest-models"></a>3. Le scoring en temps réel ne gère pas correctement le paramètre _learningRate_ dans les modèles d’arbre et de forêt

Si vous créez un modèle à l’aide d’une méthode d’arbre de décision ou de forêt d’arbres décisionnels et que vous spécifiez le taux d’apprentissage, les résultats obtenus en utilisant `sp_rxpredict` ou la fonction SQL `PREDICT` peuvent être différents de ceux obtenus avec `rxPredict`.

Cela est dû à une erreur dans l’API qui traite les modèles sérialisés, et cela se limite au paramètre `learningRate` : par exemple, dans [rxBTrees](/machine-learning-server/r-reference/revoscaler/rxbtrees) ou

Ce problème sera résolu dans une prochaine version du service.

### <a name="4-limitations-on-processor-affinity-for-r-jobs"></a>4. Limitations sur l’affinité du processeur pour les tâches R

Dans la build initiale de SQL Server 2016, vous pouviez définir l’affinité de processeur uniquement pour les processeurs du premier groupe de processeurs (K-Group). Par exemple, si le serveur est une machine à deux sockets avec deux K-Groups, seuls les processeurs du premier K-Group sont utilisés pour les processus R. La même restriction s’applique quand vous configurez la gouvernance des ressources pour les travaux de script R.

Ce problème est résolu dans le Service Pack 1 de SQL Server 2016. Nous vous recommandons d’effectuer une mise à niveau vers la dernière version du service.

**S’applique à :** SQL Server 2016 R Services version RTM

### <a name="5-changes-to-column-types-cannot-be-performed-when-reading-data-in-a-sql-server-compute-context"></a>5. Les modifications des types de colonne ne peuvent pas être effectuées lors de la lecture de données dans un contexte de calcul SQL Server

Si votre contexte de calcul est défini sur l’instance SQL Server, vous ne pouvez pas utiliser l’argument _colClasses_ (ou d’autres arguments similaires) pour modifier le type de données des colonnes dans votre code R.

Par exemple, l’instruction suivante génère une erreur si la colonne CRSDepTimeStr n’est pas déjà un entier :

```R
data <- RxSqlServerData(
  sqlQuery = "SELECT CRSDepTimeStr, ArrDelay  FROM AirlineDemoSmall", 
  connectionString = connectionString, 
  colClasses = c(CRSDepTimeStr = "integer"))
```

Pour contourner ce problème, vous pouvez réécrire la requête SQL pour utiliser CAST ou CONVERT et présenter les données à R en utilisant le bon type de données. En général, quand vous travaillez avec des données et que vous utilisez SQL, les performances sont meilleures que lorsque vous modifiez les données dans le code R.

**S’applique à :** SQL Server 2016 R Services

### <a name="6-limits-on-size-of-serialized-models"></a>6. Taille limite des modèles sérialisés

Quand vous enregistrez un modèle dans une table SQL Server, vous devez sérialiser le modèle et l’enregistrer dans un format binaire. En théorie, la taille de stockage maximale d’un modèle avec cette méthode est de 2 Go, ce qui correspond à la taille maximale des colonnes varbinary dans SQL Server.

Si vous avez besoin d’utiliser des modèles plus volumineux, voici les solutions de contournement qui s’offrent à vous :

+ Prenez les mesures nécessaires pour réduire la taille de votre modèle. Certains packages R open source ajoutent une grande quantité d’informations dans l’objet de modèle dont la plupart peuvent être supprimées pour le déploiement. 
+ Utilisez la sélection de composant pour supprimer les colonnes inutiles.
+ Si vous utilisez un algorithme open source, envisagez une implémentation similaire utilisant l’algorithme correspondant dans MicrosoftML ou RevoScaleR. Ces packages ont été optimisés pour les scénarios de déploiement.
+ Une fois que le modèle a été rationalisé et que la taille a été réduite en suivant les étapes précédentes, voyez si la fonction [memCompress](https://www.rdocumentation.org/packages/base/versions/3.4.1/topics/memCompress) en R de base peut être utilisée pour réduire la taille du modèle avant de le transmettre à SQL Server. Cette option est à privilégier quand le modèle est proche de la limite de 2 Go.
+ Pour les modèles plus volumineux, vous pouvez utiliser la fonctionnalité [FileTable](../../relational-databases/blob/filetables-sql-server.md) de SQL Server pour stocker les modèles, au lieu d’utiliser une colonne varbinary.

    Pour utiliser des FileTables, vous devez ajouter une exception de pare-feu, car les données stockées dans les FileTables sont gérées par le pilote du système de fichiers Filestream dans SQL Server, et les règles de pare-feu par défaut bloquent l’accès aux fichiers réseau. Pour plus d’informations, consultez [Activer les conditions préalables pour les FileTables](../../relational-databases/blob/enable-the-prerequisites-for-filetable.md).

    Une fois que vous avez activé FileTable, pour écrire le modèle, vous obtenez un chemin de SQL via l’API FileTable, puis vous écrivez le modèle à cet emplacement à partir de votre code. Quand vous avez besoin de lire le modèle, vous obtenez le chemin de SQL, puis vous appelez le modèle en utilisant le chemin à partir de votre script. Pour plus d’informations, consultez [Accéder aux FileTables avec des API d’entrée-sortie de fichier](../../relational-databases/blob/access-filetables-with-file-input-output-apis.md).

### <a name="7-avoid-clearing-workspaces-when-you-execute-r-code-in-a-ssnoversion-compute-context"></a>7. Éviter l’effacement d’espaces de travail pendant l’exécution de code R dans un contexte de calcul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]

Si vous utilisez une commande R pour effacer votre espace de travail d’objets pendant que vous exécutez du code R dans un contexte de calcul [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], ou si vous effacez l’espace de travail dans un script R appelé avec [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md), vous pouvez obtenir cette erreur : *workspace object revoScriptConnection not found*

`revoScriptConnection` est un objet dans l’espace de travail R qui contient des informations sur une session R appelée à partir de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Toutefois, si votre code R inclut une commande pour effacer l’espace de travail (comme `rm(list=ls()))`) toutes les informations sur la session et les autres objets dans l’espace de travail R sont également effacés.

Pour contourner ce problème, évitez l’effacement global de variables et d’autres objets pendant que vous exécutez du code R dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Bien que l’effacement de l’espace de travail soit courant dans la console R, cela peut avoir des conséquences imprévues.

+ Pour supprimer des variables spécifiques, utilisez la fonction R `remove` : par exemple, `remove('name1', 'name2', ...)`
+ Si plusieurs variables sont à supprimer, enregistrez les noms des variables temporaires dans une liste et effectuez un nettoyage périodique.

### <a name="8-restrictions-on-data-that-can-be-provided-as-input-to-an-r-script"></a>8. Restrictions sur les données qui peuvent être fournies comme entrée à un script R

Dans un script R, vous ne pouvez pas utiliser les types de résultats de requête suivants :

- Données issues d’une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] qui référence des colonnes à chiffrement intégral.
  
- Données issues d’une requête [!INCLUDE[tsql](../../includes/tsql-md.md)] qui référence des colonnes masquées.
  
     Si vous avez besoin d’utiliser des données masquées dans un script R, une solution de contournement possible consiste à effectuer une copie des données dans une table temporaire et à utiliser ces données à la place.

### <a name="9-use-of-strings-as-factors-can-lead-to-performance-degradation"></a>9. L’utilisation de chaînes en tant que facteurs peut occasionner une détérioration des performances

L’utilisation de variables de type chaîne en tant que facteurs peut grandement augmenter la quantité de mémoire utilisée pour les opérations R. Il s’agit d’un problème largement connu de R, et de nombreux articles traitent de ce sujet. Par exemple, consultez l’article de John Mount intitulé [Factors are not first-class citizens in R, sur le site R-bloggers)](https://www.r-bloggers.com/factors-are-not-first-class-citizens-in-r/) ou l’article de Roger Peng intitulé [stringsAsFactors: An unauthorized biography](https://simplystatistics.org/2015/07/24/stringsasfactors-an-unauthorized-biography/). 

Même si le problème n’est pas propre à SQL Server, il peut fortement affecter les performances d’exécution du code R dans SQL Server. Les chaînes sont généralement stockées en tant que varchar ou nvarchar, et si une colonne de données de type chaîne contient beaucoup de valeurs uniques, le processus de conversion interne de ces valeurs en entiers puis en chaînes par R peut même entraîner des erreurs d’allocation de mémoire.

Si vous n’avez pas absolument besoin d’un type de données String pour d’autres opérations, le mappage des valeurs de chaîne à un type de données numérique (entier) dans le cadre de la préparation des données serait bénéfique du point de vue des performances et de mise à l’échelle.

Pour obtenir des informations complémentaires sur ce problème et autres conseils, consultez [Performances pour R services – Optimisation des données](../r/r-and-data-optimization-r-services.md).

### <a name="10-arguments-varstokeep-and-varstodrop-are-not-supported-for-sql-server-data-sources"></a>10. Les arguments *varsToKeep* et *varsToDrop* ne sont pas pris en charge pour les sources de données SQL Server

Quand vous utilisez la fonction rxDataStep pour écrire des résultats dans une table, l’utilisation de *varsToKeep* et de *varsToDrop* offre un moyen pratique de spécifier les colonnes à inclure ou à exclure dans le cadre de l’opération. Cependant, ces arguments ne sont pas pris en charge pour les sources de données SQL Server.

### <a name="11-limited-support-for-sql-data-types-in-sp_execute_external_script"></a>11. Prise en charge limitée des types de données SQL dans sp\_execute\_external\_script

Parmi les types de données qui sont pris en charge dans SQL, certains ne peuvent pas être utilisés en R. Pour contourner ce problème, envisagez de caster le type de données non pris en charge dans un type de données pris en charge avant de transmettre les données à sp\_execute\_external\_script.

Pour plus d’informations, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

### <a name="12-possible-string-corruption-using-unicode-strings-in-varchar-columns"></a>12. Corruption possible de chaînes quand des chaînes unicode sont utilisées dans des colonnes varchar

La transmission de données unicode de colonnes varchar de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers R/Python peut occasionner une corruption de chaînes. En effet, l’encodage de ces chaînes unicode de classements [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] peut ne pas correspondre à l’encodage UTF-8 par défaut utilisé avec R/Python. 

Pour envoyer des données de type chaîne non ASCII de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] vers R/Python, utilisez l’encodage UTF-8 (disponible dans [!INCLUDE[sql-server-2019](../../includes/sssqlv15-md.md)]) ou utilisez le type nvarchar.

### <a name="13-only-one-value-of-type-raw-can-be-returned-from-sp_execute_external_script"></a>13. Une seule valeur de type `raw` peut être retournée par `sp_execute_external_script`

Quand un type de données binaire (type de données **raw** R) est retourné par R, la valeur doit être envoyée dans la trame de données de sortie.

Avec les types de données autres que **raw**, vous pouvez retourner des valeurs de paramètres avec les résultats de la procédure stockée en ajoutant le mot clé OUTPUT. Pour plus d’informations, consultez [Paramètres](../../relational-databases/stored-procedures/parameters.md).

Si vous voulez utiliser plusieurs jeux de sortie contenant des valeurs de type **raw**, la solution de contournement consiste à effectuer plusieurs appels de la procédure stockée ou de renvoyer les jeux de résultats à [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] à l’aide d’ODBC.

### <a name="14-loss-of-precision"></a>14. Perte de précision

[!INCLUDE[tsql](../../includes/tsql-md.md)] et R prennent en charge divers types de données. De ce fait, les types de données numériques peuvent subir une perte de précision pendant la conversion.

Pour plus d’informations sur la conversion implicite de types de données, consultez [Types de données et bibliothèques R](../r/r-libraries-and-data-types.md).

### <a name="15-variable-scoping-error-when-you-use-the-transformfunc-parameter"></a>15. Erreur de définition d’étendue de variable quand le paramètre transformFunc est utilisé

Pour transformer des données en cours de modélisation, vous pouvez transmettre un argument *transfoumFunc* dans une fonction comme `rxLinmod` ou `rxLogit`. Cependant, les appels de fonction imbriqués peuvent entraîner des erreurs de définition d’étendue dans le contexte de calcul SQL Server, même si les appels fonctionnent correctement dans le contexte de calcul local.

> *L’exemple de jeu de données pour l’analyse n’a pas de variables*

Par exemple, supposez que vous avez défini deux fonctions, `f` et `g`, dans votre environnement global local, et que `g` appelle `f`. Dans les appels distribués ou distants impliquant `g`, l’appel à `g` peut échouer avec cette erreur, car `f` est introuvable, même si vous avez transmis à la fois `f` et `g` à l’appel distant.

Si vous rencontrez ce problème, vous pouvez le résoudre en incorporant la définition de `f` dans votre définition de `g`, n’importe où avant l’appel habituel de `g` par `f`.

Par exemple :

```R
f <- function(x) { 2*x * 3 }
g <- function(y) {
              a <- 10 * y
               f(a)
}
```

Pour éviter cette erreur, réécrivez la définition comme suit :

```R
g <- function(y){
              f <- function(x) { 2*x +3}
              a <- 10 * y
              f(a)
}
```

### <a name="16-data-import-and-manipulation-using-revoscaler"></a>16. Importation et manipulation de données à l’aide de RevoScaleR

Quand les colonnes **varchar** sont lues à partir d’une base de données, les espaces blancs sont supprimés. Pour éviter cela, placez les chaînes entre caractères autres qu’un espace.

Quand des fonctions comme `rxDataStep` sont utilisées pour créer des tables de base de données comportant des colonnes **varchar**, la largeur des colonnes est estimée à partir d’un échantillon des données. Si la largeur peut varier, il peut être nécessaire de remplir toutes les chaînes afin qu’elles aient une longueur commune.

L’utilisation d’une transformation pour modifier le type de données d’une variable n’est pas prise en charge quand des appels répétés à `rxImport` ou `rxTextToXdf` sont effectués pour importer et ajouter des lignes, combinant plusieurs fichiers d’entrée dans un fichier .xdf unique.

### <a name="17-limited-support-for-rxexec"></a>17. Prise en charge limitée de rxExec

Dans SQL Server 2016, la fonction `rxExec` fournie par le package RevoScaleR peut être utilisée uniquement en mode monothread.

### <a name="18-increase-the-maximum-parameter-size-to-support-rxgetvarinfo"></a>18. Accroître la taille maximale des paramètres pour prendre en charge rxGetVarInfo

Si vous utilisez des jeux de données avec un très grand nombre de variables (par exemple, plus de 40 000), définissez l’indicateur `max-ppsize` quand vous commencer à utiliser des fonctions `rxGetVarInfo` avec R. L’indicateur `max-ppsize` spécifie la taille maximale de la pile de protection du pointeur.

Si vous utilisez la console R (par exemple, dans RGui.exe ou RTerm.exe), vous pouvez définir la valeur de _max-ppsize_ sur 500000 en tapant :

```R
R --max-ppsize=500000
```

### <a name="19-issues-with-the-rxdtree-function"></a>19. Problèmes liés à la fonction rxDTree

La fonction `rxDTree` ne prend pas en charge actuellement les transformations dans les formules. En particulier, l’utilisation de la syntaxe `F()` pour créer des facteurs à la volée n’est pas prise en charge. Cependant, les données numériques sont automatiquement placées dans un conteneur.

Les facteurs ordonnés sont traités de la même façon que les facteurs dans toutes les fonctions d’analyse RevoScaleR, sauf `rxDTree`.

### <a name="20-datatable-as-an-outputdataset-in-r"></a>20. Data.table en tant que OutputDataSet en R

L’utilisation de `data.table` en tant que `OutputDataSet` en R n’est pas prise en charge dans SQL Server 2017 avec la mise à jour cumulative 13 (CU13) et les versions antérieures. Le message suivant peut s’afficher :

``` text
Msg 39004, Level 16, State 20, Line 2
A 'R' script error occurred during execution of 
'sp_execute_external_script' with HRESULT 0x80004004.
Msg 39019, Level 16, State 2, Line 2
An external script error occurred: 
Error in alloc.col(newx) : 
  Internal error: length of names (0) is not length of dt (11)
Calls: data.frame ... as.data.frame -> as.data.frame.data.table -> copy -> alloc.col

Error in execution.  Check the output for more information.
Error in eval(expr, envir, enclos) : 
  Error in execution.  Check the output for more information.
Calls: source -> withVisible -> eval -> eval -> .Call
Execution halted
```

L’utilisation de `data.table` en tant que `OutputDataSet` en R est prise en charge dans SQL Server 2017 avec la mise à jour cumulative 14 (CU14) et les versions ultérieures.

### <a name="21-running-a-long-script-fails-while-installing-a-library"></a>21. L’exécution d’un script long échoue pendant l’installation d’une bibliothèque

Le fait d’exécuter une session de script externe de longue durée pendant que le dbo tente en parallèle d’installer une bibliothèque sur une autre base de données peut mettre fin au script.

Par exemple, l’exécution de ce script externe sur le maître :

```sql
USE MASTER
DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(max) = N'Sys.sleep(100)'
DECLARE @input_data_1 nvarchar(max) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1 with result sets none
go
```

Pendant que le dbo installe en parallèle une bibliothèque dans LibraryManagementFunctional :

```sql
USE [LibraryManagementFunctional]
go

CREATE EXTERNAL LIBRARY [RODBC] FROM (CONTENT = N'/home/ani/var/opt/mssql/data/RODBC_1.3-16.tar.gz') WITH (LANGUAGE = 'R')
go

DECLARE @language nvarchar(1) = N'R'
DECLARE @script nvarchar(14) = N'library(RODBC)'
DECLARE @input_data_1 nvarchar(8) = N'select 1'
EXEC sp_execute_external_script @language = @language, @script = @script, @input_data_1 = @input_data_1
go
```

Le précédent script externe de longue durée exécuté sur le maître se termine avec le message d’erreur suivant :

> *Une erreur de script « R » s’est produite lors de l’exécution de « sp_execute_external_script » avec HRESULT 0x800704d4.*

**Solution de contournement**

N’installez pas la bibliothèque parallèlement à l’exécution de requête de longue durée. Ou réexécutez la requête de longue durée une fois l’installation terminée.

**S’applique à :** SQL Server 2019 sur les clusters Big Data & Linux uniquement.

### <a name="22-sql-server-stops-responding-when-executing-r-scripts-containing-parallel-execution"></a>22. SQL Server ne répond plus lors de l’exécution de scripts R contenant une exécution parallèle

SQL Server 2019 contient une régression qui affecte les scripts R qui utilisent l’exécution en parallèle. Les exemples incluent l’utilisation de `rxExec` avec le contexte de calcul `RxLocalPar` et les scripts qui utilisent le package parallèle. Ce problème est dû au fait que le package parallèle rencontre des erreurs lors de l’écriture sur l’appareil null lors de l’exécution dans SQL Server.

**S’applique à :** SQL Server 2019.

### <a name="23-precision-loss-for-moneynumericdecimalbigint-data-types"></a>23. Perte de précision pour les types de données monnaie/numérique/décimal/bigint

L’exécution d’un script R avec `sp_execute_external_script` autorise les données d’entrée de type monnaie, numérique, décimal et bigint. Toutefois, étant donné qu’elles sont converties dans le type numérique de R, elles subissent une perte de précision pour les valeurs très élevées ou comportant des décimales.

+ **monnaie** : Les valeurs de centimes sont parfois imprécises, auquel cas un avertissement est émis : *Avertissement : Impossible de représenter avec précision les valeurs de centimes*.  
+ **numérique/décimal** : `sp_execute_external_script` avec un script R ne prend pas en charge la totalité des types de données et modifie les derniers chiffres décimaux, en particulier ceux qui comportent une fraction.
+ **bigint** : R prend en charge les entiers dans la limite de 53 bits, au-delà de laquelle il commence à afficher une perte de précision.

## <a name="python-script-execution-issues"></a>Problèmes d’exécution des scripts Python

Cette section présente des problèmes connus spécifiques à l’exécution de Python sur SQL Server ainsi que certains problèmes liés aux packages Python publiés par Microsoft, dont [revoscalepy](/r-server/python-reference/revoscalepy/revoscalepy-package) et [microsoftml](/r-server/python-reference/microsoftml/microsoftml-package).

### <a name="1-call-to-pretrained-model-fails-if-path-to-model-is-too-long"></a>1. L’appel au modèle pré-entraîné échoue si le chemin du modèle est trop long

Si vous avez installé les modèles pré-entraînés dans l’une des premières versions de SQL Server 2017, le chemin complet du fichier de modèle entraîné est peut être trop long pour que Python puisse le lire. Cette limitation sera résolue dans une version ultérieure du service.

Il est possible de contourner ce problème de plusieurs façons :

+ Quand vous installez les modèles pré-entraînés, choisissez un emplacement personnalisé.
+ Si possible, installez l’instance SQL Server dans un chemin d’installation personnalisé plus court, par exemple C:\SQL\MSSQL14.MSSQLSERVER.
+ Utilisez l’utilitaire Windows [Fsutil](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc788097(v=ws.11)) pour créer un lien physique qui mappe le fichier de modèle à un chemin plus court.
+ Procédez à une mise à jour vers la dernière version du service.

### <a name="2-error-when-saving-serialized-model-to-sql-server"></a>2. Erreur pendant l’enregistrement d’un modèle sérialisé dans SQL Server

Quand vous transmettez un modèle à une instance SQL Server distante et que vous tentez de lire le modèle binaire en utilisant la fonction `rx_unserialize` dans [revoscalepy](/machine-learning-server/python-reference/revoscalepy/revoscalepy-package), vous pouvez obtenir l’erreur suivante : 

> *NameError: name 'rx_unserialize_model' is not defined*

Cette erreur est levée si vous avez enregistré le modèle à l’aide d’une version récente de la fonction de sérialisation, mais que l’instance SQL Server dans laquelle vous désérialisez le modèle ne reconnaît pas l’API de sérialisation.

Pour résoudre ce problème, mettez à niveau l’instance SQL Server 2017 vers CU3 ou une version ultérieure.

### <a name="3-failure-to-initialize-a-varbinary-variable-causes-an-error-in-bxlserver"></a>3. L’échec de l’initialisation d’une variable varbinary provoque une erreur dans BxlServer

Si vous exécutez du code Python dans SQL Server en utilisant `sp_execute_external_script`et que le code contient des variables de sortie de type varbinary(max), varchar(max) ou de types similaires, la variable doit être initialisée ou définie dans votre script. À défaut, le composant d’échange de données, BxlServer, lève une erreur et cesse de fonctionner.

Cette limitation sera corrigée dans une prochaine version du service. Pour contourner ce problème, vérifiez que la variable est initialisée dans le script Python. Vous pouvez utiliser n’importe quelle valeur valide, comme dans les exemples suivants :

```sql
declare @b varbinary(max);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N'b = 0x0'
  , @params = N'@b varbinary(max) OUTPUT'
  , @b = @b OUTPUT;
go
```

```sql
declare @b varchar(30);
exec sp_execute_external_script
  @language = N'Python'
  , @script = N' b = ""  '
  , @params = N'@b varchar(30) OUTPUT'
  , @b = @b OUTPUT;
go
```

### <a name="4-telemetry-warning-on-successful-execution-of-python-code"></a>4. Affichage d’un avertissement de télémétrie après l’exécution réussie du code Python

À partir de SQL Server 2017 CU2, le message suivant peut s’afficher même si le code Python s’exécute correctement :

> *Message(s) STDERR provenant du script externe :* 
>  *~PYTHON_SERVICES\lib\site-packages\revoscalepy\utils\RxTelemetryLogger*
> *SyntaxWarning : telemetry_state est utilisé avant la déclaration globale*

Ce problème a été résolu dans SQL Server 2017 avec la mise à jour cumulative 3 (CU3). 

### <a name="5-numeric-decimal-and-money-data-types-not-supported"></a>5. Types de données numérique, décimal et monétaire non pris en charge

À partir de SQL Server 2017 avec la mise à jour cumulative 12 (CU12), les types de données numérique, décimal et monétaire dans WITH RESULT SETS ne sont pas pris en charge quand Python est utilisé avec `sp_execute_external_script`. Les messages suivants peuvent s’afficher :

> *[Code : 39004, État SQL : S1000]  Une erreur de script « Python » s’est produite lors de l’exécution de « sp_execute_external_script » avec HRESULT 0x80004004.*
>
> *[Code : 39019, État SQL : S1000]  Une erreur de script externe s’est produite :*
>
> *Erreur SqlSatelliteCall : Type non pris en charge dans le schéma de sortie. Types pris en charge : bit, smallint, int, datetime, smallmoney, real et float ; char, varchar sont partiellement pris en charge.*

Ce problème a été résolu dans SQL Server 2017 avec la mise à jour cumulative 14 (CU14).

### <a name="6-bad-interpreter-error-when-installing-python-packages-with-pip-on-linux"></a>6. Interpréteur incorrect entraînant une erreur pendant l’installation de packages Python avec pip sur Linux 

Dans SQL Server 2019, si vous essayez d’utiliser **pip**. Par exemple :

```bash
/opt/mssql/mlservices/runtime/python/bin/pip -h
```

Vous obtenez cette erreur :

> *bash: /opt/mssql/mlservices/runtime/python/bin/pip: /opt/microsoft/mlserver/9.4.7/bin/python/python: bad interpreter: Pas de fichier ou de répertoire correspondant*

**Solution de contournement**

Installez **pip** à partir de [PyPA (Python Package Authority)](https://www.pypa.io) :

```bash
wget 'https://bootstrap.pypa.io/get-pip.py' 
/opt/mssql/mlservices/bin/python/python ./get-pip.py 
```

**Recommandation**

Consultez [Installer des packages Python avec sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md).

**S’applique à :** SQL Server 2019 sur Linux

### <a name="7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows"></a>7. Impossible d’installer des packages Python à l’aide de pip après avoir installé SQL Server 2019 sur Windows

Après avoir installé SQL Server 2019 sur Windows, toute tentative d’installation d’un package Python via **pip** à partir d’une ligne de commande DOS aboutit à un échec. Par exemple :

```bash
pip install quantfolio
```

L’erreur suivante est retournée :

> *pip is configured with locations that require TLS/SSL, however the ssl module in Python is not available.*

Il s’agit d’un problème spécifique au package Anaconda. Il sera corrigé dans une prochaine version du service.

**Solution de contournement**

Copiez les fichiers suivants :

+ `libssl-1_1-x64.dll`
+ `libcrypto-1_1-x64.dll`

du dossier <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\Library\bin`

vers le dossier <br>
`C:\Program Files\Microsoft SQL Server\MSSSQL15.MSSQLSERVER\PYTHON_SERVICES\DLLs`

Ouvrez ensuite une nouvelle invite d’interpréteur de commandes DOS.

**S’applique à :** SQL Server 2019 sur Windows

### <a name="8-error-when-using-sp_execute_external_script-without-libcaboso-on-linux"></a>8. Erreur lors de l’utilisation de `sp_execute_external_script` sans `libc++abo.so` sur Linux

Sur une machine Linux propre sur laquelle `libc++abi.so` n’est pas installé, l’exécution d’une requête `sp_execute_external_script` (SPEES) échoue avec l’erreur « Fichier ou répertoire introuvable ».

Par exemple :

```text
EXEC sp_execute_external_script
    @language = N'Python'
    , @script = N'
OutputDataSet = InputDataSet'
    , @input_data_1 = N'select 1'
    , @input_data_1_name = N'InputDataSet'
    , @output_data_1_name = N'OutputDataSet'
    WITH RESULT SETS (([output] int not null));
Msg 39012, Level 16, State 14, Line 0
Unable to communicate with the runtime for 'Python' script for request id: 94257840-1704-45E8-83D2-2F74AEB46CF7. Please check the requirements of 'Python' runtime.
STDERR message(s) from external script:

Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.

SqlSatelliteCall error: Failed to load library /opt/mssql-extensibility/lib/sqlsatellite.so with error libc++abi.so.1: cannot open shared object file: No such file or directory.
STDOUT message(s) from external script:
SqlSatelliteCall function failed. Please see the console output for more information.
Traceback (most recent call last):
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/computecontext/RxInSqlServer.py", line 605, in rx_sql_satellite_call
    rx_native_call("SqlSatelliteCall", params)
  File "/opt/mssql/mlservices/libraries/PythonServer/revoscalepy/RxSerializable.py", line 375, in rx_native_call
    ret = px_call(functionname, params)
RuntimeError: revoscalepy function failed.
Total execution time: 00:01:00.387
```

**Solution de contournement**

Exécutez la commande suivante :

```bash
sudo cp /opt/mssql/lib/libc++abi.so.1 /opt/mssql-extensibility/lib/
```

**S’applique à :** SQL Server 2019 sur Linux

### <a name="9-cannot-install-tensorflow-package-using-sqlmlutils"></a>9. Impossible d’installer le package **TensorFlow** avec **sqlmlutils**

Le package [sqlmlutils](../package-management/install-additional-python-packages-on-sql-server.md) permet d’installer des packages Python dans SQL Server 2019. Vous devez télécharger, installer et mettre à jour [Microsoft Visual C++ 2015-2019 Redistributable (x64)](https://visualstudio.microsoft.com/downloads/). Cependant, il n’est pas possible d’installer le package **TensorFlow** à l’aide de sqlmlutils, car il dépend d’une version plus récente de numpy que la version installée dans SQL Server. Or, numpy est un package système préinstallé que sqlmlutils ne peut pas mettre à jour lorsqu’il tente d’installer TensorFlow.

**Solution de contournement**

Dans une invite de commandes en mode administrateur, exécutez la commande suivante, en remplaçant « MSSQLSERVER » par le nom de votre instance SQL :

   ```cmd
   "C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\PYTHON_SERVICES\python.exe" -m pip install --upgrade tensorflow
   ```

   Si vous obtenez une erreur « TLS/SSL », consultez [7. Impossible d’installer des packages Python à l’aide de pip](#7-unable-to-install-python-packages-using-pip-after-installing-sql-server-2019-on-windows) plus haut dans cet article.

**S’applique à :** SQL Server 2019 sur Windows

## <a name="revolution-r-enterprise-and-microsoft-r-open"></a>Revolution R Enterprise et Microsoft R Open

Cette section liste des problèmes propres aux outils de connectivité, de développement et de performances R fournis par Revolution Analytics. Ces outils ont été fournis dans des préversions antérieures de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].

En général, nous recommandons de désinstaller ces versions précédentes et d’installer la dernière version de SQL Server ou Microsoft R Server.

### <a name="1-revolution-r-enterprise-is-not-supported"></a>1. Revolution R Enterprise n’est pas pris en charge

L’installation de Revolution R Enterprise côte à côte avec n’importe quelle version de [!INCLUDE[rsql_productname_md](../../includes/rsql-productname-md.md)] n’est pas prise en charge.

Si vous avez déjà une licence Revolution R Enterprise, vous devez la placer sur un ordinateur autre que celui de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et de la station de travail dont vous voulez vous servir pour vous connecter à l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].

Certaines préversions de [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)] comportaient un environnement de développement R pour Windows qui avait été créé par Revolution Analytics. Cet outil n’est plus fourni et n’est pas pris en charge.

Pour assurer la compatibilité avec [!INCLUDE[rsql_productname](../../includes/rsql-productname-md.md)], nous vous recommandons d’installer plutôt Microsoft R Client. [Outils R pour Visual Studio](https://marketplace.visualstudio.com/items?itemName=MikhailArkhipov007.RTVS2019) et [Visual Studio Code](https://code.visualstudio.com/) prennent aussi en charge les solutions Microsoft R.

### <a name="2-compatibility-issues-with-sqlite-odbc-driver-and-revoscaler"></a>2. Problèmes de compatibilité avec le pilote ODBC de SQLite et RevoScaleR

La révision 0.92 du pilote ODBC SQLite est incompatible avec RevoScaleR. Les révisions 0.88-0.91, 0.93 et ultérieures sont connues pour être compatibles.

## <a name="next-steps"></a>Étapes suivantes

[Résolution des problèmes de Machine Learning dans SQL Server](machine-learning-troubleshooting-overview.md)