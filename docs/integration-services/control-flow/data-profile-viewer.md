---
description: Visionneuse du profil des données
title: Visionneuse du profil des données | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.dataprofileviewer.f1
helpviewer_keywords:
- Data Profile Viewer [Integration Services]
- Data Profiling task [Integration Services], output viewer
ms.assetid: b9043428-ce26-45bb-910c-588d07579565
author: chugugrace
ms.author: chugu
ms.openlocfilehash: aaa5d2276908feccd86aac75012703e093b861d3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352134"
---
# <a name="data-profile-viewer"></a>Visionneuse du profil des données

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  L'affichage et l'analyse des profils des données sont les étapes suivantes du processus de profilage des données. Pour afficher ces profils, vous devez avoir exécuté la tâche de profilage des données dans un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] et avoir calculé les profils des données. Pour plus d’informations sur la configuration et l’exécution des tâches de profilage des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
> [!IMPORTANT]  
>  Le fichier de sortie peut contenir des données sensibles qui concernent votre base de données et les données qu'elle contient. Pour obtenir des suggestions sur la manière de sécuriser davantage ce fichier, consultez [Accéder aux fichiers utilisés par des packages](../../integration-services/security/security-overview-integration-services.md#files).  
  
## <a name="data-profiles"></a>Profils de données  
 Pour afficher les profils des données, vous configurez la tâche de profilage des données de manière à envoyer sa sortie à un fichier, puis vous utilisez la visionneuse du profil des données autonome. Pour ouvrir la visionneuse du profil des données, effectuez l'une des actions suivantes :  
  
-   Cliquez avec le bouton droit sur la tâche de **profilage des données** dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , puis cliquez sur **Modifier**. Cliquez sur **Ouvrir la visionneuse de profil** dans la page **Général** de **l’Éditeur de tâche de profilage de données**.  
  
-   Dans le dossier, *\<drive>* :\Program Files (x86) | Program Files\Microsoft SQL Server\110\DTS\Binn, exécutez DataProfileViewer.exe.  
  
 La visionneuse utilise plusieurs volets pour afficher les profils demandés et les résultats calculés avec, en option, des détails et une fonction d'exploration vers le bas :  
  
 Volet **Profils**  
 Le volet **Profils** affiche les profils demandés dans la tâche de profilage des données. Pour afficher les résultats calculés pour le profil, sélectionnez le profil dans le volet **Profils** . Les résultats apparaîtront dans les autres volets de la visionneuse.  
  
 Volet **Résultats**  
 Le volet **Résultats** résume les résultats calculés du profil sur une seule ligne. Par exemple, si vous demandez un **Profil de distribution de longueurs de colonne**, cette ligne inclut la longueur minimale et la longueur maximale, ainsi que le nombre de lignes. Pour la plupart des profils, vous pouvez sélectionner cette ligne dans le volet **Résultats** pour afficher d’autres détails dans le volet **Détails** facultatif.  
  
 Volet **Détails**  
 Pour la plupart des types de profils, le volet **Détails** affiche des informations supplémentaires à propos des résultats du profil sélectionnés dans le volet **Résultats** . Par exemple, si vous demandez un **Profil de distribution de longueurs de colonne**, le volet **Détails** affiche chaque longueur de colonne qui a été trouvée. Le volet affiche aussi le nombre et le pourcentage de lignes dans lesquelles la valeur de colonne est égale à cette longueur de colonne.  
  
 Pour les trois types de profils qui sont calculés sur plusieurs colonnes (Profil de clé candidate, Profil de dépendance fonctionnelle et Profil d’inclusion de valeur), le volet **Détails** affiche les violations de la relation attendue. Par exemple, si vous demandez un Profil de clé candidate, le volet Détails affiche les valeurs dupliquées qui enfreignent l'unicité de la clé candidate.  
  
 Si la source de données utilisée pour calculer le profil est disponible, vous pouvez double-cliquer sur une ligne dans le volet **Détails** pour consulter les lignes correspondantes de données dans le volet **Exploration vers le bas** .  
  
 Volet **Exploration vers le bas**  
 Vous pouvez double-cliquer sur une ligne dans le volet **Détails** pour consulter les lignes correspondantes de données dans le volet **Exploration vers le bas** quand les conditions suivantes sont vérifiées :  
  
-   La source de données utilisée pour calculer le profil est disponible.  
  
-   Vous êtes autorisé à afficher les données.  
  
 Pour se connecter à la base de données source lors d'une requête d'exploration vers le bas, la visionneuse du profil des données utilise l'authentification Windows et les informations d'identification de l'utilisateur actuel. La visionneuse du profil des données n'utilise pas les informations de connexion stockées dans le package qui a exécuté la tâche de profilage des données.  
  
> [!IMPORTANT]  
>  La fonction d'exploration vers le bas disponible dans la visionneuse du profil des données envoie des requêtes actives à la source de données d'origine. Ces requêtes peuvent avoir un impact négatif sur les performances du serveur.  
>   
>  Si vous descendez dans la hiérarchie à partir d'un fichier de sortie qui n'a pas été créé récemment, les requêtes d'exploration risquent de retourner un autre ensemble de lignes que celui à partir duquel la sortie d'origine a été calculée.  
  
 Pour plus d’informations sur l’interface utilisateur de la visionneuse du profil des données, consultez [Aide F1 de la visionneuse du profil des données]().  
  
## <a name="data-profile-viewer-f1-help"></a>Aide F1 de la Visionneuse du profil des données
  Utilisez la Visionneuse du profil des données pour afficher la sortie de la tâche de profilage des données.  
  
 Pour plus d’informations sur le mode d’utilisation de la Visionneuse du profil des données, consultez [Visionneuse du profil des données](../../integration-services/control-flow/data-profile-viewer.md). Pour plus d’informations sur l’utilisation de la tâche de profilage des données, qui crée la sortie de profil que vous analysez dans la Visionneuse du profil des données, consultez [Configuration de la tâche de profilage des données](../../integration-services/control-flow/setup-of-the-data-profiling-task.md).  
  
### <a name="static-options"></a>Options statiques  
 **Ouvrir**  
 Cliquez pour accéder au fichier enregistré qui contient la sortie de la tâche de profilage des données.  
  
 Volet **Profils**  
 Développez l’arborescence du volet **Profils** pour consulter les profils inclus dans la sortie. Sélectionnez un profil afin d'afficher les résultats de ce profil.  
  
 Volet **Message**  
 Affiche des messages d'état.  
  
 Volet **Exploration vers le bas**  
 Affiche les lignes de données qui correspondent à une valeur dans la sortie, si la source de données utilisée par la tâche de profilage des données est disponible.  
  
 Par exemple, si vous affichez la sortie d’un profil de distribution de valeurs de colonne pour une colonne des états américains, le volet **Distribution de valeurs** peut contenir une ligne pour « WA ». Double-cliquez sur la ligne dans le volet **Distribution de valeurs** pour consulter les lignes de données pour lesquelles la valeur de la colonne d’état est « WA » dans le volet d’exploration.  
  
### <a name="dynamic-options"></a>Options dynamiques  
  
#### <a name="profile-type--column-length-distribution-profile"></a>Type de profil = Profil de distribution de longueurs de colonne  
  
##### <a name="column-length-distribution-profile---column-pane"></a>Profil de distribution de longueurs de colonne - Volet \<column>  
 **Longueur minimale**  
 Affiche la longueur minimale des valeurs de cette colonne.  
  
 **Longueur maximale**  
 Affiche la longueur maximale des valeurs de cette colonne.  
  
 **Ignorer les espaces de début**  
 Indique si ce profil a été calculé avec une valeur **IgnoreLeadingSpaces** True ou False. Cette propriété a été définie dans la page **Demandes de profil** de l’éditeur de tâche de profilage de données.  
  
 **Ignorez les espaces de fin**  
 Indique si ce profil a été calculé avec une valeur **IgnoreTrailingSpaces** True ou False. Cette propriété a été définie dans la page **Demandes de profil** de l’éditeur de tâche de profilage de données.  
  
 **Nombre de lignes**  
 Affiche le nombre de lignes présentes dans la table ou la vue.  
  
##### <a name="detailed-length-distribution-pane"></a>Volet de distribution de longueurs détaillé  
 **Longueur**  
 Affiche les longueurs de colonne trouvées dans la colonne profilée.  
  
 **Count**  
 Affiche le nombre de lignes dans lesquelles la valeur de la colonne profilée a la longueur affichée dans la colonne **Longueur** .  
  
 **Percentage**  
 Affiche le pourcentage de lignes dans lesquelles la valeur de la colonne profilée a la longueur affichée dans la colonne **Longueur** .  
  
#### <a name="profile-type--column-null-ratio-profile"></a>Type de profil = Profil de ratio de colonne Null  
  
##### <a name="column-null-ratio-profile---column-pane"></a>Profil de ratio de colonne Null - Volet \<column>  
 **Nombre null**  
 Affiche le nombre de lignes dans lesquelles la colonne profilée possède une valeur NULL.  
  
 **Pourcentage null**  
 Affiche le pourcentage de lignes dans lesquelles la colonne profilée possède une valeur NULL.  
  
 **Nombre de lignes**  
 Affiche le nombre de lignes présentes dans la table ou la vue.  
  
#### <a name="profile-type--column-pattern-profile"></a>Type de profil = Profil de modèle de colonne  
  
##### <a name="column-pattern-profile---column-pane"></a>Profil de motif de colonne - Volet \<column>  
 **Nombre de lignes**  
 Affiche le nombre de lignes présentes dans la table ou la vue.  
  
##### <a name="pattern-distribution-pane"></a>Volet Distribution de modèles  
 **Modèle**  
 Affiche les modèles calculés pour la colonne profilée.  
  
 **Percentage**  
 Affiche le pourcentage de lignes dont les valeurs correspondent au modèle affiché dans la colonne **Modèle** .  
  
#### <a name="profile-type--column-statistics-profile"></a>Type de profil = Profil de statistiques de colonnes  
  
##### <a name="column-statistics-profile---column-pane"></a>Profil de statistiques de colonnes - Volet \<column>  
 **Minimum**  
 Affiche la valeur minimale trouvée dans la colonne profilée.  
  
 **Maximum**  
 Affiche la valeur maximale trouvée dans la colonne profilée.  
  
 **Mean**  
 Affiche la moyenne des valeurs trouvées dans la colonne profilée.  
  
 **Écart type**  
 Affiche l'écart type des valeurs trouvées dans la colonne profilée.  
  
#### <a name="profile-type--column-value-distribution-profile"></a>Type de profil = Profil de distribution de valeurs de colonne  
  
##### <a name="column-value-distribution-profile---column-pane"></a>Profil de distribution de valeurs de colonne - Volet \<column>  
 **Nombre de valeurs distinctes**  
 Affiche le nombre de valeurs distinctes trouvées dans la colonne profilée.  
  
 **Nombre de lignes**  
 Affiche le nombre de lignes présentes dans la table ou la vue.  
  
##### <a name="detailed-value-distribution-pane"></a>Volet de distribution de valeurs détaillé  
 **Valeur**  
 Affiche les valeurs distinctes trouvées dans la colonne profilée.  
  
 **Count**  
 Affiche le nombre de lignes dans lesquelles la colonne profilée a la valeur affichée dans la colonne **Valeur** .  
  
 **Percentage**  
 Affiche le pourcentage de lignes dans lesquelles la colonne profilée a la valeur affichée dans la colonne **Valeur** .  
  
#### <a name="profile-type--candidate-key-profile"></a>Type de profil = Profil de clé candidate  
  
##### <a name="candidate-key-profile---table-pane"></a>Profil de clé candidate - Volet \<table>  
 **Colonnes clés**  
 Affiche les colonnes sélectionnées pour le profilage en tant que clé candidate.  
  
 **Puissance de la clé**  
 Affiche la puissance (sous forme de pourcentage) de la colonne ou de la combinaison de colonnes clés candidates. Une puissance de clé inférieure à 100 % indique qu'il existe des valeurs dupliquées.  
  
##### <a name="key-violations-pane"></a>Volet Violations de clé  
 **\<column1>, \<column2>, etc.**  
 Affiche les valeurs dupliquées qui ont été détectées dans la colonne profilée.  
  
 **Count**  
 Affiche le nombre de lignes dans lesquelles la colonne spécifiée possède la valeur affichée dans la première colonne.  
  
#### <a name="profile-type--functional-dependency-profile"></a>Type de profil = Profil de dépendance fonctionnelle  
  
##### <a name="functional-dependency-profile-pane"></a>Volet Profil de dépendance fonctionnelle  
 **Colonnes déterminantes**  
 Affiche les colonnes sélectionnées comme colonnes déterminantes. Dans l'exemple où le même code postal américain doit systématiquement posséder le même état, le code postal est la colonne déterminante.  
  
 **Colonnes dépendantes**  
 Affiche les colonnes sélectionnées comme colonnes dépendantes. Dans l'exemple où le même code postal d'état américain doit systématiquement posséder le même état, l'état est la colonne déterminante.  
  
 **Puissance de la dépendance fonctionnelle**  
 Affiche la puissance (sous forme de pourcentage) de la dépendance fonctionnelle entre les colonnes. Une puissance de clé inférieure à 100% indique qu'il existe des situations où la valeur déterminante ne détermine pas la valeur dépendante. Dans l'exemple où le même code postal d'état américain doit systématiquement posséder le même état, cela indique probablement que certaines valeurs d'état ne sont pas valides.  
  
##### <a name="functional-dependency-violations-pane"></a>Volet Violations de la dépendance fonctionnelle  
  
> [!NOTE]  
>  Un pourcentage élevé de valeurs erronées dans les données peut générer des résultats inattendus d'un profil de dépendance fonctionnelle. Par exemple, 90 % des lignes ont une valeur État de « WI » pour une valeur Code postal de « 98052 ». Le profil signale les lignes qui contiennent la valeur d'état correcte de « WA » en tant que violations.  
  
 **\<determinant column name>**  
 Affiche la valeur de la colonne déterminante ou de la combinaison de colonnes dans cette instance d'une violation de dépendance fonctionnelle.  
  
 **\<dependent column name>**  
 Affiche la valeur de la colonne dépendante dans cette instance d'une violation de dépendance fonctionnelle.  
  
 **Nombre de supports**  
 Affiche le nombre de lignes dans lesquelles la valeur de colonne déterminante détermine la colonne dépendante.  
  
 **Nombre de violations**  
 Affiche le nombre de lignes dans lesquelles la valeur de colonne déterminante ne détermine pas la colonne dépendante. (Il s’agit des lignes sur lesquelles la valeur dépendante est la valeur affichée dans la colonne **\<dependent column name>** .)  
  
 **Pourcentage de supports**  
 Affiche le pourcentage de lignes dans lesquelles la colonne déterminante détermine la colonne dépendante.  
  
#### <a name="profile-type--value-inclusion-profile"></a>Type de profil = Profil d'inclusion de valeur  
  
##### <a name="value-inclusion-profile-pane"></a>Volet du profil d'inclusion de valeur  
 **Colonnes côté sous-ensemble**  
 Affiche la colonne ou la combinaison de colonnes qui ont été profilées pour déterminer si elles figurent dans les colonnes du sur-ensemble.  
  
 **Colonnes côté sur-ensemble**  
 Affiche la colonne ou la combinaison de colonnes qui ont été profilées pour déterminer si elles incluent les valeurs dans les colonnes du sous-ensemble.  
  
 **Puissance d'inclusion**  
 Affiche la puissance (sous forme de pourcentage) du chevauchement entre les colonnes. Une puissance de clé inférieure à 100 % indique que dans certains cas, la valeur du sous-ensemble est introuvable parmi les valeurs du sur-ensemble.  
  
##### <a name="inclusion-violations-pane"></a>Volet des violations d'inclusion  
 **\<column1>, \<column2>, etc.**  
 Affiche les valeurs de la colonne ou des colonnes du sous-ensemble qui étaient introuvables dans la colonne ou les colonnes du sur-ensemble.  
  
 **Count**  
 Affiche le nombre de lignes dans lesquelles la colonne spécifiée possède la valeur affichée dans la première colonne.  
