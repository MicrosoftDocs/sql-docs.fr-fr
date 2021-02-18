---
title: 'Didacticiel R : Explorer et visualiser des données'
titleSuffix: SQL machine learning
description: Explorez les exemples de données et générez des tracés en préparation de l’utilisation de la classification binaire dans R avec SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 10/15/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2016||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 8e2e8994f92bfb96fa66c2c6a08c7db198902b45
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354213"
---
# <a name="r-tutorial-explore-and-visualize-data"></a>Didacticiel R : Explorer et visualiser des données
[!INCLUDE [SQL Server 2016 SQL MI](../../includes/applies-to-version/sqlserver2016-asdbmi.md)]

Dans la deuxième des cinq parties de notre tutoriel, vous explorez les exemples de données et générez des tracés. Plus tard, vous allez apprendre à sérialiser des objets de graphiques dans Python, puis à désérialiser ces objets et à créer des tracés.

Dans la deuxième partie de ce tutoriel en cinq parties, vous allez examiner les exemples de données, puis générer des tracés à l’aide des fonctions génériques `barplot` et `hist` en base R.

L’objectif principal de cet article est d’illustrer comment appeler des fonctions R à partir de [!INCLUDE[tsql](../../includes/tsql-md.md)] dans des procédures stockées et enregistrer les résultats dans des formats de fichiers d’application :

+ Créez une procédure stockée à l’aide de `barplot` pour générer un tracé R en tant que données varbinary. Utilisez **bcp** pour exporter le flux binaire vers un fichier image.
+ Créez une procédure stockée à l’aide de `hist` pour générer un tracé, en enregistrant les résultats au format JPG et PDF.

> [!NOTE]
> Étant donné que la visualisation est un outil puissant pour la compréhension de la forme et de la distribution des données, R propose une gamme de fonctions et de packages pour générer des histogrammes, des nuages de points, des surfaces et d’autres graphs d’exploration de données. R crée généralement des images à l’aide d’un appareil R pour la sortie graphique, que vous pouvez capturer et stocker en tant que type de données **varbinary** pour le rendu dans l’application. Vous pouvez également enregistrer les images dans l’un des formats de fichiers de prise en charge (.JPG,.PDF, etc.).

Dans cet article, vous allez :

> [!div class="checklist"]
> + Examiner l’exemple de données
> + Créer des tracés à l’aide de R en T-SQL
> + Émettre des tracés dans plusieurs formats de fichiers

Dans la [première partie](r-taxi-classification-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [troisième partie](r-taxi-classification-create-features.md), vous apprendrez à créer des fonctionnalités à partir de données brutes à l’aide d’une fonction Transact-SQL. Ensuite, vous appellerez cette fonction à partir d’une procédure stockée pour créer une table qui contient les valeurs des caractéristiques.

Dans la [quatrième partie](r-taxi-classification-train-model.md), vous chargez les modules et appelez les fonctions nécessaires pour créer et entraîner le modèle à l’aide d’une procédure stockée SQL Server.

Dans la [cinquième partie](r-taxi-classification-deploy-model.md), vous apprendrez à rendre opérationnels les modèles que vous avez formés et enregistrés dans la quatrième partie.

## <a name="review-the-data"></a>Examiner les données

Le développement d’une solution de science des données comprend généralement l’exploration et la visualisation des données. Tout d’abord, prenez une minute pour examiner les exemples de données, si ce n’est déjà fait.

Dans le jeu de données public d’origine, les identificateurs de taxis et les enregistrements de trajets ont été fournis dans des fichiers distincts. Toutefois, pour simplifier l’utilisation des exemples de données, les deux jeux de données d’origine ont été joints sur les colonnes _medallion_, _hack\_license_ et _pickup\_datetime_.  Les enregistrements ont aussi été échantillonnés pour obtenir seulement 1 % du nombre d’enregistrements d’origine. Le dataset échantillonné obtenu compte 1 703 957 lignes et 23 colonnes.

**Identificateurs de taxis**
  
+ La colonne _medallion_ représente le numéro identifiant unique du taxi.
  
+ La colonne _hack\_license_ contient le numéro de licence du conducteur du taxi (anonyme).
  
**Enregistrements de trajets et de prix**
  
+ Chaque enregistrement de trajet comprend les lieux de prise en charge et de dépose, ainsi que la durée et la distance du trajet.
  
+ Chaque enregistrement de prix inclut des informations telles que le type de paiement, le montant total du paiement et le montant du pourboire.
  
+ Les trois dernières colonnes peuvent être utilisées pour différentes tâches d’apprentissage automatique. La colonne _tip\_amount_ contient des valeurs numériques continues et peut être utilisée comme colonne **label** pour l’analyse de régression. La colonne _tipped_ contient seulement des valeurs oui/non. Elle sert à la classification binaire. La colonne _tip\_class_ a plusieurs **étiquettes de classes**, et peut donc être utilisée comme étiquette pour les tâches de classification multiclasse.
  
  Cette procédure pas à pas ne montre que la tâche de classification binaire. Si vous le souhaitez, vous pouvez essayer de créer des modèles pour les autres deux tâches d’apprentissage automatique, la régression et la classification multiclasse.
  
+ Les valeurs utilisées pour les colonnes d’étiquettes sont basées sur la colonne _tip\_amount_, à l’aide de ces règles d’entreprise :
  
  |Nom de la colonne dérivée|Règle|
  |-|-|
  |tipped|Si tip_amount > 0, tipped = 1, sinon tipped = 0|
  |tip_class|Classe 0 : tip_amount = 0 $<br /><br />Classe 1 : tip_amount > 0 $ et tip_amount <= 5 $<br /><br />Classe 2 : tip_amount > 5 $ et tip_amount <= 10 $<br /><br />Classe 3 : tip_amount > 10 $ et tip_amount <= 20 $<br /><br />Classe 4 : tip_amount > 20 $|

## <a name="create-plots-using-r-in-t-sql"></a>Créer des tracés à l’aide de R en T-SQL

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
> [!IMPORTANT]
> À compter de SQL Server 2019, le mécanisme d’isolation vous oblige à accorder les autorisations appropriées au répertoire dans lequel le fichier de traçage est stocké. Pour savoir comment définir ces autorisations, consultez la [section Autorisations de fichiers dans SQL Server 2019 sur Windows : Modifications de l’isolation dans Machine Learning Services](../install/sql-server-machine-learning-services-2019.md#file-permissions).
::: moniker-end

Pour créer le tracé, utilisez la fonction R `barplot`. Cette étape trace un histogramme basé sur les données d’une requête [!INCLUDE[tsql](../../includes/tsql-md.md)]. Vous pouvez wrapper cette fonction dans la procédure stockée **RPlotHistogram**.

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données **NYCTaxi_Sample**, et sélectionnez **Nouvelle requête**. Sinon, dans Azure Data Studio, vous pouvez également sélectionner **Nouveau notebook** dans le menu **Fichier**, puis vous connecter à la base de données.

2. Collez le script suivant pour créer une procédure stockée qui trace l’histogramme. Cet exemple se nomme **RPlotHistogram**.

   ```sql
   CREATE PROCEDURE [dbo].[RPlotHistogram]
   AS
   BEGIN
     SET NOCOUNT ON;
     DECLARE @query nvarchar(max) =  
     N'SELECT tipped FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
                                        @script = N'  
      image_file = tempfile();  
      jpeg(filename = image_file);  
      #Plot histogram  
      barplot(table(InputDataSet$tipped), main = "Tip Histogram", col="lightgreen", xlab="Tipped or not", ylab = "Counts", space=0)
      dev.off();  
      OutputDataSet <- data.frame(data=readBin(file(image_file, "rb"), what=raw(), n=1e6));  
      ',  
      @input_data_1 = @query  
      WITH RESULT SETS ((plot varbinary(max)));  
   END
   GO
   ```

Les points clés à retenir dans ce script sont les suivants :
  
+ La variable `@query` définit le texte de requête (`'SELECT tipped FROM nyctaxi_sample'`), qui est transmis au script R comme argument de la variable d’entrée du script, `@input_data_1`. Pour les scripts R qui s’exécutent en tant que processus externes, vous devez disposer d’un mappage un-à-un entre les entrées de votre script, et les entrées de la procédure stockée système [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) qui démarre la session R sur SQL Server.
  
+ Dans le script R, une variable (`image_file`) est définie pour stocker l’image.

+ La fonction `barplot` est appelée pour générer le tracé.
  
+ L’appareil R est défini sur **off** (désactivé), car vous exécutez cette commande en tant que script externe dans SQL Server. Généralement, en R, quand vous émettez une commande principale de traçage, R ouvre une fenêtre graphique appelée *device* (périphérique). Vous pouvez désactiver l’appareil si vous écrivez dans un fichier ou si vous gérez la sortie d’une autre façon.
  
+ L’objet graphique R est sérialisé en data.frame R pour la sortie.

### <a name="execute-the-stored-procedure-and-use-bcp-to-export-binary-data-to-an-image-file"></a>Exécuter la procédure stockée et utiliser bcp pour exporter des données binaires dans un fichier image

La procédure stockée retourne l’image sous forme de flux de données varbinary qui, évidemment, ne peut pas être affiché directement. Toutefois, vous pouvez utiliser l’utilitaire **bcp** pour obtenir les données varbinary et les enregistrer en tant que fichier image sur un ordinateur client.
  
1. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], exécutez la commande suivante :
  
    ```sql
    EXEC [dbo].[RPlotHistogram]
    ```
  
    **Résultats**
    
    *tracé* *0xFFD8FFE000104A4649...*
  
2. Ouvrez une invite de commandes PowerShell et exécutez la commande suivante, en fournissant le nom d’instance, le nom de base de données, le nom d’utilisateur et les informations d’identification appropriés en tant qu’arguments. Pour ceux qui utilisent des identités Windows, vous pouvez remplacer **-U** et **-P** par **-T**.
  
   ```powershell
   bcp "exec RPlotHistogram" queryout "plot.jpg" -S <SQL Server instance name> -d  NYCTaxi_Sample  -U <user name> -P <password> -T
   ```

   > [!NOTE]
   > Les commutateurs de commande bcp respectent la casse.
  
3. Si la connexion réussit, vous serez invité à entrer davantage d’informations sur le format du fichier graphique.

   Appuyez sur Entrée à chaque invite pour accepter les valeurs par défaut, à l’exception de ces modifications :

   + Pour **prefix-length of field plot**, tapez 0.
  
   + Type **Y** si vous souhaitez enregistrer les paramètres de sortie pour une réutilisation ultérieure.
  
   ```text
   Enter the file storage type of field plot [varbinary(max)]: 
   Enter prefix-length of field plot [8]: 0
   Enter length of field plot [0]:
   Enter field terminator [none]:
  
   Do you want to save this format information in a file? [Y/n]
   Host filename [bcp.fmt]:
   ```
  
   **Résultats**

   ```text
   Starting copy...
   1 rows copied.
   Network packet size (bytes): 4096
   Clock Time (ms.) Total     : 3922   Average : (0.25 rows per sec.)
   ```

   > [!TIP]
   > Si vous enregistrez les informations de format dans un fichier (bcp.fmt), l’utilitaire **bcp** génère une définition de format que vous pouvez appliquer ultérieurement à des commandes similaires sans avoir à entrer d’options de format de fichier graphique. Pour utiliser le fichier de format, ajoutez `-f bcp.fmt` à la fin d’une ligne de commande, après l’argument de mot de passe.
  
4. Le fichier de sortie est créé dans le même répertoire que celui où vous avez exécuté la commande PowerShell. Pour afficher le tracé, il suffit d’ouvrir le fichier plot.jpg.
  
   ![voyages en taxi avec et sans conseils](media/rsql-devtut-tippedornot.jpg "voyages en taxi avec et sans conseils")  
  
## <a name="create-a-stored-procedure-using-hist"></a>Créer une procédure stockée avec `hist`

En général, les scientifiques des données génèrent plusieurs visualisations de données, pour obtenir des informations sur les données de différentes perspectives. Dans cet exemple, vous allez créer une procédure stockée appelée **RPlotHist** pour écrire des histogrammes, des nuages de points et d’autres graphiques R au format .JPG et .PDF.

Cette procédure stockée utilise la fonction `hist` pour créer l’histogramme, en exportant les données binaires dans des formats populaires tels que .JPG, .PDF et .PNG.

1. Dans [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)], dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données **NYCTaxi_Sample**, et sélectionnez **Nouvelle requête**.

2. Collez le script suivant pour créer une procédure stockée qui trace l’histogramme. Cet exemple est nommé **RPlotHist**.
  
   ```sql
   CREATE PROCEDURE [dbo].[RPlotHist]  
   AS  
   BEGIN  
     SET NOCOUNT ON;  
     DECLARE @query nvarchar(max) =  
     N'SELECT cast(tipped as int) as tipped, tip_amount, fare_amount FROM [dbo].[nyctaxi_sample]'  
     EXECUTE sp_execute_external_script @language = N'R',  
     @script = N'  
      # Set output directory for files and check for existing files with same names   
       mainDir <- ''C:\\temp\\plots''  
       dir.create(mainDir, recursive = TRUE, showWarnings = FALSE)  
       setwd(mainDir);  
       print("Creating output plot files:", quote=FALSE)
       
       # Open a jpeg file and output histogram of tipped variable in that file.  
       dest_filename = tempfile(pattern = ''rHistogram_Tipped_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.jpg'',sep="")  
       print(dest_filename, quote=FALSE);  
       jpeg(filename=dest_filename);  
       hist(InputDataSet$tipped, col = ''lightgreen'', xlab=''Tipped'',   
           ylab = ''Counts'', main = ''Histogram, Tipped'');  
        dev.off();  

       # Open a pdf file and output histograms of tip amount and fare amount.   
       # Outputs two plots in one row  
       dest_filename = tempfile(pattern = ''rHistograms_Tip_and_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=7);  
       par(mfrow=c(1,2));  
       hist(InputDataSet$tip_amount, col = ''lightgreen'',   
           xlab=''Tip amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram, Tip amount'', xlim = c(0,40), 100);  
       hist(InputDataSet$fare_amount, col = ''lightgreen'',   
           xlab=''Fare amount ($)'',   
           ylab = ''Counts'',   
           main = ''Histogram,   
           Fare amount'',   
           xlim = c(0,100), 100);  
       dev.off();  
  
       # Open a pdf file and output an xyplot of tip amount vs. fare amount using lattice;  
       # Only 10,000 sampled observations are plotted here, otherwise file is large.  
       dest_filename = tempfile(pattern = ''rXYPlots_Tip_vs_Fare_Amount_'', tmpdir = mainDir)  
       dest_filename = paste(dest_filename, ''.pdf'',sep="")  
       print(dest_filename, quote=FALSE);  
       pdf(file=dest_filename, height=4, width=4);  
       plot(tip_amount ~ fare_amount,   
           data = InputDataSet[sample(nrow(InputDataSet), 10000), ],   
           ylim = c(0,50),   
           xlim = c(0,150),   
           cex=.5,   
           pch=19,   
           col=''darkgreen'',    
           main = ''Tip amount by Fare amount'',   
           xlab=''Fare Amount ($)'',   
           ylab = ''Tip Amount ($)'');   
       dev.off();',  
     @input_data_1 = @query  
   END
   ```
  
Les points clés à retenir dans ce script sont les suivants :

+ Le résultat de la requête SELECT dans la procédure stockée est stocké dans la trame de données R par défaut, `InputDataSet`. Vous pouvez ensuite appeler différentes fonctions de traçage R pour générer les fichiers graphiques proprement dits. La plupart du script R incorporé représente des options pour ces fonctions graphiques, telles que `plot` ou `hist`.
  
+ L’appareil R est défini sur **off** (désactivé), car vous exécutez cette commande en tant que script externe dans SQL Server. Généralement, en R, quand vous émettez une commande principale de traçage, R ouvre une fenêtre graphique appelée *device* (périphérique). Vous pouvez désactiver l’appareil si vous écrivez dans un fichier ou si vous gérez la sortie d’une autre façon.
  
+ Tous les fichiers sont enregistrés dans le dossier local C:\temp\Plots. Le dossier de destination est défini par les arguments fournis au script R dans le cadre de la procédure stockée. Pour exporter les fichiers vers un autre dossier, modifiez la valeur de la variable `mainDir` dans le script R incorporé dans la procédure stockée. Vous pouvez également modifier le script pour générer des formats différents, plusieurs fichiers, et ainsi de suite.

### <a name="execute-the-stored-procedure"></a>Exécuter la procédure stockée

Exécutez l’instruction suivante pour exporter des données de tracé binaires au format de fichier JPEG et PDF.

```sql
EXEC RPlotHist
```

**Résultats**

```text
STDOUT message(s) from external script:
[1] Creating output plot files:[1] C:\temp\plots\rHistogram_Tipped_18887f6265d4.jpg[1] 

C:\temp\plots\rHistograms_Tip_and_Fare_Amount_1888441e542c.pdf[1]

C:\temp\plots\rXYPlots_Tip_vs_Fare_Amount_18887c9d517b.pdf
```

Les nombres dans les noms de fichiers sont générés de manière aléatoire pour s’assurer que vous ne recevez pas d’erreur lors de la tentative d’écriture dans un fichier existant.

### <a name="view-output"></a>Voir la sortie

Pour voir le tracé, ouvrez le dossier de destination et examinez les fichiers qui ont été créés par le code R dans la procédure stockée.Ouvrez le dossier de destination et examinez les fichiers qui ont été créés par le code R dans la procédure stockée.

1. Accédez au dossier indiqué dans le message STDOUT (dans cet exemple, il s’agit de C:\temp\plots\)

2. Ouvrez `rHistogram_Tipped.jpg` pour afficher le nombre de trajets pour lesquels un pourboire a été donné par rapport à ceux qui n’ont reçu aucun pourboire (cet histogramme est semblable à celui que vous avez généré à l’étape précédente).

3. Ouvrez `rHistograms_Tip_and_Fare_Amount.pdf` pour afficher la répartition des montants des pourboires, en fonction des montants des prix.

   ![histogramme qui indique tip_amount et fare_amount](media/rsql-devtut-tipamtfareamt.PNG "histogramme qui indique tip_amount et fare_amount")

4. Ouvrez `rXYPlots_Tip_vs_Fare_Amount.pdf` pour afficher un nuage de points avec les prix sur l’axe des abscisses et le montant des pourboires sur l’axe y.

   ![montant des pourboires tracé par rapport au montant des prix](media/rsql-devtut-tipamtbyfareamt.PNG "montant des pourboires tracé par rapport au montant des prix")

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :

> [!div class="checklist"]
> + Passer en revue des exemples de données
> + Tracés créés à l’aide de R en T-SQL
> + Émettre des tracés dans plusieurs formats de fichiers

> [!div class="nextstepaction"]
> [Didacticiel R : Créer des caractéristiques de données](r-taxi-classification-create-features.md)