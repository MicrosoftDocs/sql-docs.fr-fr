---
title: 'Tutoriel Python : Créer des caractéristiques de données'
titleSuffix: SQL machine learning
description: Dans la troisième des cinq parties de ce tutoriel, vous ajouterez des calculs aux procédures stockées à utiliser dans des modèles Machine Learning Python avec SQL Machine Learning.
ms.prod: sql
ms.technology: machine-learning
ms.date: 07/29/2020
ms.topic: tutorial
author: dphansen
ms.author: davidph
ms.custom: seo-lt-2019
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||>=azuresqldb-mi-current'
ms.openlocfilehash: 630a5db712e3bd6e288c0ac12ca767a07c3ac4ef
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339708"
---
# <a name="python-tutorial-create-data-features-using-t-sql"></a>Tutoriel Python : Créer des caractéristiques de données à l’aide de T-SQL
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

Dans cette troisième des cinq parties du tutoriel, vous allez découvrir comment créer des caractéristiques à partir de données brutes en utilisant une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)]. Ensuite, vous appellerez cette fonction à partir d’une procédure stockée SQL pour créer une table qui contient les valeurs des caractéristiques.

Ce processus de *caractérisation* à partir des données brutes peut être une étape essentielle pour la modélisation de l’analytique avancée.

Dans cet article, vous allez :

> [!div class="checklist"]
> + Modifier une fonction personnalisée pour calculer la distance du trajet
> + Enregistrer les fonctionnalités à l’aide d’une autre fonction personnalisée

Dans la [première partie](python-taxi-classification-introduction.md), vous avez installé les prérequis et restauré l’exemple de base de données.

Dans la [deuxième partie](python-taxi-classification-explore-data.md), vous avez étudié les exemples de données et généré des tracés.

Dans la [quatrième partie](python-taxi-classification-train-model.md), vous chargez les modules et appelez les fonctions nécessaires pour créer et entraîner le modèle à l’aide d’une procédure stockée SQL Server.

Dans la [cinquième partie](python-taxi-classification-deploy-model.md), vous apprendrez à rendre opérationnels les modèles que vous avez formés et enregistrés dans la quatrième partie.

## <a name="define-the-function"></a>Définir la fonction

Les valeurs de distance indiquées dans les données d’origine sont basées sur la distance signalée au compteur, et ne représente pas nécessairement la distance géographique ou la distance parcourue. Ainsi, vous devrez calculer la distance directe entre les lieux de prise en charge et de dépose, en utilisant les coordonnées disponibles dans le dataset source « NYC Taxi ». Vous pouvez pour cela utiliser la [formule de Haversine](https://en.wikipedia.org/wiki/Haversine_formula) dans une fonction [!INCLUDE[tsql](../../includes/tsql-md.md)] personnalisée.

Vous allez utiliser une fonction T-SQL personnalisée, _fnCalculateDistance_, pour calculer la distance à l’aide de la formule de Haversine, et utiliser une seconde fonction T-SQL personnalisée, _fnEngineerFeatures_, pour créer une table contenant toutes les caractéristiques.

### <a name="calculate-trip-distance-using-fncalculatedistance"></a>Calcul de la distance des trajets à l’aide de fnCalculateDistance

1. La fonction _fnCalculateDistance_ est incluse dans l’exemple de base de données. Prenez le temps de passer le code en revue.
  
2. Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], développez **Programmabilité**, **Fonctions** puis **Fonctions scalaires**.
   Cliquez avec le bouton droit sur _fnCalculateDistance_, puis sélectionnez **Modifier** pour ouvrir le script [!INCLUDE[tsql](../../includes/tsql-md.md)] dans une nouvelle fenêtre de requête.
  
   ```sql
   CREATE FUNCTION [dbo].[fnCalculateDistance] (@Lat1 float, @Long1 float, @Lat2 float, @Long2 float)
   -- User-defined function that calculates the direct distance between two geographical coordinates
   RETURNS float
   AS
   BEGIN
     DECLARE @distance decimal(28, 10)
     -- Convert to radians
     SET @Lat1 = @Lat1 / 57.2958
     SET @Long1 = @Long1 / 57.2958
     SET @Lat2 = @Lat2 / 57.2958
     SET @Long2 = @Long2 / 57.2958
     -- Calculate distance
     SET @distance = (SIN(@Lat1) * SIN(@Lat2)) + (COS(@Lat1) * COS(@Lat2) * COS(@Long2 - @Long1))
     --Convert to miles
     IF @distance <> 0
     BEGIN
       SET @distance = 3958.75 * ATAN(SQRT(1 - POWER(@distance, 2)) / @distance);
     END
     RETURN @distance
   END
   GO
   ```

**Remarques :**

+ La fonction est une fonction scalaire qui retourne une valeur de données unique d’un type prédéfini.
+ Elle accepte des valeurs de latitude et de longitude comme entrées, obtenues à partir des lieux de prise en charge et de dépose des passagers. La formule de Haversine convertit les emplacements en radians et utilise ces valeurs pour calculer la distance directe en miles entre ces deux emplacements.

Pour ajouter la valeur calculée à une table qui peut être utilisée pour l’apprentissage du modèle, vous allez utiliser une autre fonction, _fnEngineerFeatures_.

### <a name="save-the-features-using-_fnengineerfeatures_"></a>Enregistrer les caractéristiques à l’aide de _fnEngineerFeatures_

1. Prenez une minute pour examiner le code de la fonction T-SQL personnalisée, _fnEngineerFeatures_, qui se trouve dans l’exemple de base de données.
  
   Il s’agit d’une fonction table qui prend plusieurs colonnes comme entrées et retourne une table avec plusieurs colonnes de caractéristiques.  Le rôle de cette fonction consiste à créer un ensemble de caractéristiques servant à générer un modèle. La fonction _fnEngineerFeatures_ appelle la fonction T-SQL créée précédemment, _fnCalculateDistance_, pour obtenir la distance directe entre les lieux de prise en charge et de dépose des passagers.
  
   ```sql
   CREATE FUNCTION [dbo].[fnEngineerFeatures] (
   @passenger_count int = 0,
   @trip_distance float = 0,
   @trip_time_in_secs int = 0,
   @pickup_latitude float = 0,
   @pickup_longitude float = 0,
   @dropoff_latitude float = 0,
   @dropoff_longitude float = 0)
   RETURNS TABLE
   AS
     RETURN
     (
     -- Add the SELECT statement with parameter references here
     SELECT
       @passenger_count AS passenger_count,
       @trip_distance AS trip_distance,
       @trip_time_in_secs AS trip_time_in_secs,
       [dbo].[fnCalculateDistance](@pickup_latitude, @pickup_longitude, @dropoff_latitude, @dropoff_longitude) AS direct_distance
     )
   GO
   ```
  
2. Pour vérifier que cette fonction fonctionne, vous pouvez l’utiliser pour calculer la distance géographique pour les trajets où la distance au compteur était égale à 0, mais où les lieux de prise en charge et de dépose étaient différents.
  
   ```sql
       SELECT tipped, fare_amount, passenger_count,(trip_time_in_secs/60) as TripMinutes,
       trip_distance, pickup_datetime, dropoff_datetime,
       dbo.fnCalculateDistance(pickup_latitude, pickup_longitude,  dropoff_latitude, dropoff_longitude) AS direct_distance
       FROM nyctaxi_sample
       WHERE pickup_longitude != dropoff_longitude and pickup_latitude != dropoff_latitude and trip_distance = 0
       ORDER BY trip_time_in_secs DESC
   ```
  
   Comme vous pouvez le voir, la distance signalée par le compteur ne correspond pas toujours à la distance géographique. C’est pourquoi l’ingénierie des caractéristiques est importante.

Dans la partie suivante, vous découvrirez comment utiliser ces caractéristiques de données pour créer et entraîner un modèle Machine Learning avec Python.

## <a name="next-steps"></a>Étapes suivantes

Dans cet article, vous découvrirez comment :

> [!div class="checklist"]
> + Modifier une fonction personnalisée pour calculer la distance du trajet
> + Enregistrer les fonctionnalités à l’aide d’une autre fonction personnalisée

> [!div class="nextstepaction"]
> [Tutoriel Python : Entraîner et enregistrer un modèle Python à l’aide de T-SQL](python-taxi-classification-train-model.md)