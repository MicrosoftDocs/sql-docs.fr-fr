---
title: Exemples d’expressions (Générateur de rapports) | Microsoft Docs
description: Contrôlez le contenu et l’apparence des rapports à l’aide des fonctions intégrées, du code personnalisé, des variables de rapport et de groupe, et des variables définies par l’utilisateur dans le Générateur de rapports.
ms.date: 04/06/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- page breaks [Reporting Services], expressions
- green-bar reports [Reporting Services]
- Visual Basic [Reporting Services]
- functions [Reporting Services], examples
- custom code [Reporting Services]
- appearance of reports
- formatting reports [Reporting Services], expressions
- show/hide [Reporting Services]
- parameters [Reporting Services], expressions
- visibility [Reporting Services], expressions
- page headers [Reporting Services]
- page footers [Reporting Services]
- dates [Reporting Services], expressions
- expressions [Reporting Services], examples
ms.assetid: 87ddb651-a1d0-4a42-8ea9-04dea3f6afa4
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 40a05ec1ed6b7ffa05132e0cefbd5157b7d3ce92
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596335"
---
# <a name="expression-examples-report-builder-and-ssrs"></a>Exemples d'expressions (Générateur de rapports et SSRS)
Les expressions sont fréquemment utilisées dans les rapports paginés [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] pour en contrôler le contenu et l’apparence. Les expressions sont écrites en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)], et peuvent utiliser des fonctions intégrées, du code personnalisé, des variables de rapport et de groupe, et des variables définies par l’utilisateur. Les expressions commencent par un signe égal (=). Pour plus d’informations sur l’éditeur d’expressions et les types de références que vous pouvez inclure, consultez [Utilisation d’expressions dans les rapports &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md), et [Ajouter une expression &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-an-expression-report-builder-and-ssrs.md).  
  
> [!IMPORTANT]  
>  Lorsque le sandboxing RDL est activé, seuls certains types et membres peuvent être utilisés dans le texte de l'expression au moment de la publication des rapports. Pour plus d’informations, consultez [Enable and Disable RDL Sandboxing](../../reporting-services/report-server-sharepoint/enable-and-disable-rdl-sandboxing.md).  
  
Cette rubrique propose des exemples d'expressions qu'il est possible d'utiliser pour des tâches courantes dans un rapport.  
  
-   [Fonctions Visual Basic](#VisualBasicFunctions) Exemples pour les fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] de date, de chaîne, de conversion et conditionnelles.  
  
-   [Fonctions de rapport](#ReportFunctions) Exemples pour les agrégats et autres fonctions de rapport intégrées.  
  
-   [Apparence des données d'un rapport](#AppearanceofReportData) Exemples pour la modification de l'apparence d'un rapport.  
  
-   [Propriétés](#Properties) Exemples pour la définition des propriétés d'élément de rapport permettant de contrôler le format ou la visibilité.  
  
-   [Paramètres](#Parameters) Exemples pour l'utilisation de paramètres dans une expression.  
  
-   [Code personnalisé](#CustomCode) Exemples de code personnalisé incorporé.  
  
Pour obtenir des exemples d'expressions pour des utilisations spécifiques, consultez les rubriques suivantes :  
  
-   [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)  
  
-   [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)  
  
-   [Filtres couramment utilisés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
-   [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md)  
  
Pour plus d’informations sur les expressions simples et complexes, l’endroit où vous pouvez utiliser des expressions et les types de références que vous pouvez inclure dans une expression, consultez les rubriques sous [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md). Pour plus d’informations sur le contexte dans lequel les expressions sont évaluées pour calculer des agrégats, consultez [Étendue des expressions pour les totaux, les agrégats et les collections intégrées &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-scope-for-totals-aggregates-and-built-in-collections.md).  
  
Pour apprendre à écrire des expressions qui utilisent plusieurs fonctions et opérateurs également utilisés par les exemples d'expressions de cette rubrique, mais dans le contexte de la rédaction d'un rapport, consultez [Tutorial: Introducing Expressions](../../reporting-services/tutorial-introducing-expressions.md).  

  
## <a name="functions"></a>Fonctions  
 Dans un rapport, beaucoup d'expressions contiennent des fonctions. Vous pouvez mettre en forme des données, appliquer une logique et accéder aux métadonnées du rapport en utilisant ces fonctions. Vous pouvez écrire des expressions qui utilisent des fonctions de la bibliothèque runtime [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] et des fonctions des espaces de noms <xref:System.Convert> et <xref:System.Math>. Vous pouvez ajouter des références à des fonctions issues d'autres assemblys ou du code personnalisé. Vous pouvez également utiliser des classes à partir de [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], y compris <xref:System.Text.RegularExpressions>.  
  
##  <a name="visual-basic-functions"></a><a name="VisualBasicFunctions"></a> Fonctions Visual Basic  
 Vous pouvez utiliser des fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] pour manipuler les données affichées dans des zones de texte ou utilisées pour des paramètres, des propriétés et d'autres zones du rapport. Cette section fournit des exemples décrivant certaines de ces fonctions. Pour plus d'informations, consultez [Membres de la bibliothèque runtime Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) sur MSDN.  
  
 Le [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] offre de nombreuses options de formats personnalisés, par exemple pour des formats de date spécifiques. Pour plus d'informations, consultez [Mise en forme des types](/previous-versions/) sur MSDN.  
  
### <a name="math-functions"></a>Fonctions mathématiques  
  
-   La fonction **Round** permet d'arrondir des nombres à l'entier le plus proche. L'expression suivante arrondit 1,3 à 1 :  
  
    ```  
    = Round(1.3)  
    ```  
  
     Vous pouvez également écrire une expression pour arrondir une valeur à un multiple de votre choix, de manière semblable à la fonction **MRound** dans Excel. Multipliez la valeur par un facteur qui crée un entier, arrondissez le nombre, puis divisez-le par le même facteur. Par exemple, pour arrondir 1,3 au multiple le plus proche de 0,2 (1,4), utilisez l'expression suivante :  
  
    ```  
    = Round(1.3*5)/5  
    ```  
  
###  <a name="date-functions"></a><a name="DateFunctions"></a> Fonctions de date  
  
-   La fonction **Today** fournit la date actuelle. Cette expression peut être utilisée dans une zone de texte pour afficher la date sur le rapport ou bien, dans un paramètre pour filtrer les données basées sur la date actuelle.  
  
    ```  
    =Today()  
    ```  
  
-   Utilisez la fonction **DateInterval** pour extraire une partie spécifique d’une date. Voici différents paramètres **DateInterval** valides :

    -   DateInterval.Second
    -   DateInterval.Minute
    -   DateInterval.Hour
    -   DateInterval.Weekday
    -   DateInterval.Day
    -   DateInterval.DayOfYear
    -   DateInterval.WeekOfYear
    -   DateInterval.Month
    -   DateInterval.Quarter
    -   DateInterval.Year

    Par exemple, cette expression affiche le numéro de la semaine dans l’année en cours pour la date du jour :
  
    ```  
    =DatePart(DateInterval.WeekOfYear, today()) 
    ```  
  
-   La fonction **DateAdd** est utile pour fournir une plage de dates basées sur un seul paramètre. L'expression ci-dessous indique une date qui se situe six mois après la date provenant du paramètre nommé *StartDate*.  
  
    ```  
    =DateAdd(DateInterval.Month, 6, Parameters!StartDate.Value)  
    ```  
  
-   La fonction **Year** affiche l'année pour une date particulière. Vous pouvez l'utiliser pour grouper des dates ou pour afficher l'année en tant que libellé d'un ensemble de dates. Cette expression affiche l'année pour un groupe donné de dates de commande client. La fonction **Month** et d'autres fonctions peuvent également être utilisées pour manipuler des dates. Pour plus d’informations, consultez la documentation de [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)].  
  
    ```  
    =Year(Fields!OrderDate.Value)  
    ```  
  
-   Vous pouvez combiner des fonctions dans une expression pour personnaliser le format. L'expression suivante modifie le format d'une date écrite sous la forme mois-jour-année en mois-semaine-numéro de semaine. Par exemple, 12/23/2009 est remplacé par December Week 3 (Décembre Semaine 3) :  
  
    ```  
    =Format(Fields!MyDate.Value, "MMMM") & " Week " &   
    (Int(DateDiff("d", DateSerial(Year(Fields!MyDate.Value),   
    Month(Fields!MyDate.Value),1), Fields!FullDateAlternateKey.Value)/7)+1).ToString  
    ```  
  
     Lorsque cette expression est utilisée comme champ calculé dans un dataset, vous pouvez vous en servir dans un graphique pour agréger les valeurs par semaine dans chaque mois.  
  
-   L'expression suivante affiche la valeur SellStartDate au format MMM-YY. Le champ SellStartDate comprend des données de type datetime.  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "MMM-yy")  
    ```  
  
-   L'expression suivante affiche la valeur SellStartDate au format dd/MM/yyyy. Le champ SellStartDate comprend des données de type datetime.  
  
    ```  
    =FORMAT(Fields!SellStartDate.Value, "dd/MM/yyyy")  
    ```  
  
-   La fonction **CDate** convertit la valeur en date. La fonction **Now** retourne une valeur de date contenant la date et l'heure actuelles en fonction de votre système. **DateDiff** retourne une valeur longue spécifiant le nombre d'intervalles de temps entre deux valeurs de date.  
  
     L'exemple suivant affiche la date de début de l'année en cours  
  
    ```  
    =DateAdd(DateInterval.Year,DateDiff(DateInterval.Year,CDate("01/01/1900"),Now()),CDate("01/01/1900"))  
    ```  
  
-   L'exemple suivant affiche la date de début du mois précédent en fonction du mois actuel.  
  
    ```  
    =DateAdd(DateInterval.Month,DateDiff(DateInterval.Month,CDate("01/01/1900"),Now())-1,CDate("01/01/1900"))  
    ```  
  
-   L'expression suivante génère des années d'intervalle entre SellStartDate et LastReceiptDate. Ces champs sont compris dans deux datasets différents, DataSet1 et DataSet2. La [fonction First &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-first-function.md), qui est une fonction d’agrégation, renvoie la première valeur de SellStartDate dans DataSet1 et la première valeur de LastReceiptDate dans DataSet2.  
  
    ```  
    =DATEDIFF("yyyy", First(Fields!SellStartDate.Value, "DataSet1"), First(Fields!LastReceiptDate.Value, "DataSet2"))  
    ```  
  
-   La fonction **DatePart** renvoie une valeur entière contenant le composant spécifié d'une valeur donnée de Date. L'expression suivante renvoie l'année de la première valeur de SellStartDate dans DataSet1. L'étendue du dataset est spécifiée, car le rapport contient plusieurs datasets.  
  
    ```  
    =Datepart("yyyy", First(Fields!SellStartDate.Value, "DataSet1"))  
  
    ```  
  
-   La fonction **DateSerial** renvoie une valeur Date représentant une année, un mois et un jour spécifiés, avec l'heure définie sur minuit. L'exemple suivant affiche la date de fin du mois précédent en fonction du mois actuel.  
  
    ```  
    =DateSerial(Year(Now()), Month(Now()), "1").AddDays(-1)  
    ```  
  
-   Les expressions suivantes affichent plusieurs dates basées sur une valeur de paramètre de date sélectionnée par l'utilisateur.  
  
|Description de l'exemple|Exemple|  
|-------------------------|-------------|  
|hier|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-1)`|  
|Il y a deux jours|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value)-2)`|  
|Il y a un mois|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-1,Day(Parameters!TodaysDate.Value))`|  
|Il y a deux mois|`=DateSerial(Year(Parameters!TodaysDate.Value),Month(Parameters!TodaysDate.Value)-2,Day(Parameters!TodaysDate.Value))`|  
|Il y a un an|`=DateSerial(Year(Parameters!TodaysDate.Value)-1,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
|Il y a deux ans|`=DateSerial(Year(Parameters!TodaysDate.Value)-2,Month(Parameters!TodaysDate.Value),Day(Parameters!TodaysDate.Value))`|  
  
###  <a name="string-functions"></a><a name="StringFunctions"></a> Fonctions de chaîne  
  
-   Vous pouvez combiner plusieurs champs en utilisant des opérateurs de concaténation et des constantes [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] . L'expression suivante retourne deux champs, chacun sur une ligne séparée dans la même zone de texte :  
  
    ```  
    =Fields!FirstName.Value & vbCrLf & Fields!LastName.Value   
    ```  
  
-   Vous pouvez mettre en forme des dates et des nombres dans une chaîne avec la fonction **Format** . L'expression suivante affiche les valeurs des paramètres *StartDate* et *EndDate* dans un format de date longue :  
  
    ```  
    =Format(Parameters!StartDate.Value, "D") & " through " &  Format(Parameters!EndDate.Value, "D")    
    ```  
  
     Si la zone de texte contient uniquement une date ou un nombre, utilisez plutôt la propriété Format de la zone de texte pour appliquer la mise en forme, plutôt que la fonction **Format** dans la zone de texte.  
  
-   Les fonctions **Right**, **Len** et **InStr** sont utiles pour renvoyer une sous-chaîne, par exemple en réduisant *DOMAINE*\\*nom d’utilisateur* au seul nom d’utilisateur. L’expression suivante retourne la partie de la chaîne à droite de la barre oblique inverse (\\) à partir d’un paramètre nommé *User*:  
  
    ```  
    =Right(Parameters!User.Value, Len(Parameters!User.Value) - InStr(Parameters!User.Value, "\"))  
    ```  
  
     L’expression suivante retourne la même valeur que précédemment, en utilisant des membres de la classe [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.String> au lieu des fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] :  
  
    ```  
    =Parameters!User.Value.Substring(Parameters!User.Value.IndexOf("\")+1, Parameters!User.Value.Length-Parameters!User.Value.IndexOf("\")-1)  
    ```  
  
-   Vous pouvez afficher les valeurs sélectionnées à partir d'un paramètre à valeurs multiples. L'exemple suivant utilise la fonction **Join** pour concaténer les valeurs sélectionnées du paramètre *MySelection* en une chaîne unique qu'il est possible de définir en tant qu'expression de la valeur d'une zone de texte dans un élément de rapport :  
  
    ```  
    = Join(Parameters!MySelection.Value)  
    ```  
  
     L'exemple suivant donne le même résultat que l'exemple ci-dessus et affiche une chaîne de texte avant la liste de valeurs sélectionnées.  
  
    ```  
    ="Report for " & JOIN(Parameters!MySelection.Value, " & ")  
  
    ```  
  
-   Les fonctions **Regex** de l’espace de noms [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.Text.RegularExpressions> sont utiles si vous souhaitez modifier le format de chaînes existantes (par exemple, le format d’un numéro de téléphone). L’expression suivante utilise la fonction **Replace** pour modifier dans un champ le format d’un numéro de téléphone à dix chiffres et le faire passer du format "*nnn*-*nnn*-*nnnn*" au format "(*nnn*) *nnn*-*nnnn*" :  
  
    ```  
    =System.Text.RegularExpressions.Regex.Replace(Fields!Phone.Value, "(\d{3})[ -.]*(\d{3})[ -.]*(\d{4})", "($1) $2-$3")  
    ```  
  
    > [!NOTE]  
    >  Vérifiez que la valeur de Fields!Phone.Value n’a pas d’espaces supplémentaires et est de type <xref:System.String>.  
  
### <a name="lookup"></a>Recherche  
  
-   En spécifiant un champ clé, vous pouvez utiliser la fonction **Lookup** pour récupérer une valeur à partir d’un dataset pour une relation un-à-un, par exemple une paire clé-valeur. L’expression suivante affiche le nom de produit d’un dataset (« Product »), compte tenu de l’identificateur de produit qui doit être mis en correspondance :  
  
    ```  
    =Lookup(Fields!PID.Value, Fields!ProductID.Value, Fields!ProductName.Value, "Product")  
    ```  
  
### <a name="lookupset"></a>LookupSet  
  
-   En spécifiant un champ clé, vous pouvez utiliser la fonction **LookupSet** pour récupérer un jeu de valeurs à partir d’un dataset pour une relation un-à-plusieurs. Par exemple, une personne peut avoir plusieurs numéros de téléphone. Dans l'exemple suivant, supposons que le dataset PhoneList contient un identificateur de personne et un numéro de téléphone sur chaque ligne. **LookupSet** retourne un tableau de valeurs. L'expression suivante combine les valeurs de retour dans une chaîne unique et affiche la liste des numéros de téléphone de la personne spécifiée par ContactID :  
  
    ```  
    =Join(LookupSet(Fields!ContactID.Value, Fields!PersonID.Value, Fields!PhoneNumber.Value, "PhoneList"),",")  
    ```  
  
###  <a name="conversion-functions"></a><a name="ConversionFunctions"></a> Fonctions de conversion  
 Vous pouvez utiliser des fonctions [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] pour convertir un champ en un type de données différent de celui qu'il avait au départ. Les fonctions de conversion peuvent être utilisées pour convertir le type de données par défaut d'un champ en type qui sera nécessaire pour certains calculs ou pour combiner du texte.  
  
-   L’expression suivante convertit la constante 500 en type Decimal pour la comparer à un type de données money [!INCLUDE[tsql](../../includes/tsql-md.md)] dans le champ Value d’une expression de filtre.  
  
    ```  
    =CDec(500)  
    ```  
  
-   L'expression ci-dessous affiche le nombre de valeurs sélectionnées pour le paramètre à valeurs multiples *MySelection*.  
  
    ```  
    =CStr(Parameters!MySelection.Count)  
    ```  
  
###  <a name="decision-functions"></a><a name="DecisionFunctions"></a> Fonctions de décision  
  
-   La fonction **Iif** retourne une valeur sur les deux possibles, laquelle dépend du fait que l'expression est vraie ou fausse. L’expression ci-dessous utilise la fonction **Iif** pour retourner la valeur booléenne **True** si la valeur de `LineTotal` est supérieure à 100. Sinon, elle retourne **False**:  
  
    ```  
    =IIF(Fields!LineTotal.Value > 100, True, False)  
    ```  
  
-   Vous pouvez utiliser des fonctions **IIF** (également appelées « IIF imbriqués ») pour retourner une valeur sur les trois possibles, laquelle dépend de la valeur de `PctComplete`. L'expression ci-dessous peut être placée dans la couleur de remplissage d'une zone de texte afin de changer la couleur d'arrière-plan selon la valeur contenue dans la zone de texte.  
  
    ```  
    =IIF(Fields!PctComplete.Value >= 10, "Green", IIF(Fields!PctComplete.Value >= 1, "Blue", "Red"))  
    ```  
  
     Les valeurs supérieures ou égales à 10 s'affichent avec un arrière-plan vert, celles qui sont comprises entre 1 et 9 avec un arrière-plan bleu et les valeurs inférieures à 1 avec un arrière-plan rouge.  
  
-   Une autre méthode pour obtenir la même fonctionnalité consiste à utiliser la fonction **Switch** . La fonction **Switch** est utile lorsque vous avez au moins trois conditions à tester. La fonction **Switch** retourne la valeur associée à la première expression d'une série qui remplit la condition :  
  
    ```  
    =Switch(Fields!PctComplete.Value >= 10, "Green", Fields!PctComplete.Value >= 1, "Blue", Fields!PctComplete.Value = 1, "Yellow", Fields!PctComplete.Value <= 0, "Red")  
    ```  
  
     Les valeurs supérieures ou égales à 10 s'affichent avec un arrière-plan vert, celles qui sont comprises entre 1 et 9 avec un arrière-plan bleu, les valeurs égales à 1 s'affichent avec un arrière-plan jaune et celles qui sont égales ou inférieures à 0 avec un arrière-plan rouge.  
  
-   L'expression ci-dessous teste la valeur du champ `ImportantDate` et retourne la valeur « Red » si elle a plus d'une semaine et la valeur « Blue » dans le cas contraire. Cette expression peut être utilisée pour contrôler la propriété Color d'une zone de texte dans un élément de rapport :  
  
    ```  
    =IIF(DateDiff("d",Fields!ImportantDate.Value, Now())>7,"Red","Blue")  
    ```  
  
-   L’expression ci-dessous teste la valeur du champ `PhoneNumber` et retourne « No Value » si la valeur est **null** (**Nothing** en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]) ou le numéro de téléphone dans le cas contraire. Cette expression peut être utilisée pour contrôler la valeur d'une zone de texte dans un élément de rapport.  
  
    ```  
    =IIF(Fields!PhoneNumber.Value Is Nothing,"No Value",Fields!PhoneNumber.Value)  
    ```  
  
-   L’expression ci-dessous teste la valeur du champ `Department` et retourne le nom d’un sous-rapport ou **null** (**Nothing** en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]). Cette expression peut être utilisée pour des sous-rapports d'extraction conditionnelle.  
  
    ```  
    =IIF(Fields!Department.Value = "Development", "EmployeeReport", Nothing)  
    ```  
  
-   L'expression ci-dessous teste si la valeur d'un champ est Null. Cette expression peut être utilisée pour contrôler la propriété **Hidden** d'un élément de rapport de type image. Dans l'exemple suivant, l'image spécifiée par le champ [LargePhoto] s'affiche uniquement si la valeur du champ n'est pas Null.  
  
    ```  
    =IIF(IsNothing(Fields!LargePhoto.Value),True,False)  
    ```  
  
-   La fonction **MonthName** retourne une valeur de chaîne contenant le nom du mois spécifié. L'exemple suivant affiche NA dans le champ Mois lorsque le champ contient la valeur 0.  
  
    ```  
    IIF(Fields!Month.Value=0,"NA",MonthName(IIF(Fields!Month.Value=0,1,Fields!Month.Value)))  
  
    ```  
  
##  <a name="report-functions"></a><a name="ReportFunctions"></a> Fonctions de rapport  
 Dans une expression, vous pouvez ajouter une référence à des fonctions supplémentaires de rapport qui manipulent les données dans un rapport. Cette section fournit des exemples pour deux de ces fonctions. Pour plus d’informations sur les fonctions et les exemples de rapport, consultez [Informations de référence sur les fonctions d’agrégation &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/report-builder-functions-aggregate-functions-reference.md).  
  
###  <a name="sum"></a><a name="Sum"></a> Sum  
  
-   La fonction **Sum** peut additionner les valeurs dans un groupe ou une région de données. Cette fonction peut être utile dans l'en-tête ou le pied de page d'un groupe. L'expression suivante affiche la somme des données dans la région de données ou le groupe Order :  
  
    ```  
    =Sum(Fields!LineTotal.Value, "Order")  
    ```  
  
-   Vous pouvez également utiliser la fonction **Sum** pour effectuer des calculs d'agrégats conditionnels. Par exemple, si un dataset contient un champ nommé State dont les valeurs possibles sont Not Started, Started et Finished, l'expression suivante, lorsqu'elle est placée dans un en-tête de groupe, calcule la somme uniquement pour la valeur Finished :  
  
    ```  
    =Sum(IIF(Fields!State.Value = "Finished", 1, 0))  
    ```  
  
###  <a name="rownumber"></a><a name="RowNumber"></a> RowNumber  
  
-   La fonction **RowNumber** , quand elle est utilisée dans une zone de texte au sein d'une région de données, affiche le numéro de ligne de chaque instance de la zone de texte dans laquelle l'expression apparaît. Cette fonction peut être utile pour numéroter les lignes dans un tableau. Elle peut également servir à l'accomplissement de tâches plus complexes, telles que l'insertion de sauts de page en fonction du nombre de lignes. Pour plus d'informations, consultez [Sauts de page](#PageBreaks) , plus loin dans cette rubrique.  
  
     L'étendue que vous spécifiez pour **RowNumber** détermine quand commence la renumérotation. Le mot clé **Nothing** indique que la fonction démarrera à compter à la première ligne dans la région de données la plus à l'extérieur. Pour commencer à compter dans des régions de données imbriquées, utilisez le nom de la région de données. Pour commencer à compter dans un groupe, utilisez le nom du groupe.  
  
    ```  
    =RowNumber(Nothing)  
    ```  
  
##  <a name="appearance-of-report-data"></a><a name="AppearanceofReportData"></a> Apparence des données d'un rapport  
 Vous pouvez recourir à des expressions pour intervenir sur la façon dont les données apparaissent sur un rapport. Par exemple, vous pouvez afficher les valeurs de deux champs dans une seule zone de texte, afficher des informations sur le rapport ou modifier la façon dont les sauts de page sont insérés dans le rapport.  
  
###  <a name="page-headers-and-footers"></a><a name="PageHeadersandFooters"></a> En-têtes et pieds de page  
 Lors de la conception d'un rapport, vous pouvez choisir d'afficher son nom et les numéros des pages dans le pied de page. Pour ce faire, vous pouvez utiliser les expressions suivantes :  
  
-   L'expression suivante fournit le nom du rapport ainsi que les date et heure de son exécution. Elle peut être placée dans une zone de texte du pied de page du rapport ou dans le corps du rapport. Le format des date et heure est déterminé par la chaîne de mise en forme [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] pour la date courte :  
  
    ```  
    =Globals.ReportName & ", dated " & Format(Globals.ExecutionTime, "d")  
    ```  
  
-   L'expression ci-dessous, placée dans une zone de texte du pied de page d'un rapport, indique le numéro de la page actuelle ainsi que le nombre total de pages du rapport :  
  
    ```  
    =Globals.PageNumber & " of " & Globals.TotalPages  
    ```  
  
 Les exemples suivants décrivent comment afficher la première et la dernière valeur d'une page dans l'en-tête de la page, comme dans le contenu d'un annuaire. Cet exemple suppose l'existence d'une région de données qui contient une zone de texte appelée `LastName`.  
  
-   L'expression suivante, placée dans une zone de texte sur la partie gauche de l'en-tête de la page, fournit la première valeur de la zone de texte `LastName` sur la page :  
  
    ```  
    =First(ReportItems("LastName").Value)  
    ```  
  
-   L'expression suivante, placée dans une zone de texte sur la partie droite de l'en-tête de la page, fournit la dernière valeur de la zone de texte `LastName` sur la page :  
  
    ```  
    =Last(ReportItems("LastName").Value)  
    ```  
  
 L'exemple suivant décrit comment afficher un total pour une page. Cet exemple suppose l'existence d'une région de données qui contient une zone de texte appelée `Cost`.  
  
-   L'expression suivante, placée dans l'en-tête ou le pied de page, fournit la somme des valeurs dans la zone de texte `Cost` pour la page :  
  
    ```  
    =Sum(ReportItems("Cost").Value)  
    ```  
  
> [!NOTE]  
>  Vous ne pouvez faire référence qu'à un seul élément de rapport par expression dans un en-tête ou un pied de page. Vous pouvez aussi faire référence au nom de la zone de texte, mais pas à l'expression de données elle-même, dans les expressions d'en-tête et de pied de page.  
  
###  <a name="page-breaks"></a><a name="PageBreaks"></a> Sauts de page  
 Dans certains rapports, vous pouvez souhaiter placer un saut de page à la fin d'un nombre spécifié de lignes à la place, ou en plus, de groupes ou d'éléments de rapport. Pour ce faire, créez un groupe qui contient les groupes ou enregistrements de détails qui vous intéressent, ajoutez un saut de page au groupe, puis ajoutez une expression de groupe pour regrouper par un nombre spécifié de lignes.  
  
-   L'expression suivante, quand elle est placée dans l'expression de groupe, affecte un nombre à chaque ensemble de 25 lignes. Quand un saut de page est défini pour le groupe, cette expression insère un saut de page toutes les 25 lignes.  
  
    ```  
    =Ceiling(RowNumber(Nothing)/25)  
    ```  
  
     Pour permettre à l'utilisateur de définir une valeur pour le nombre de lignes par page, créez un paramètre nommé `RowsPerPage` et basez l'expression de groupe sur le paramètre, comme le montre l'expression suivante :  
  
    ```  
    =Ceiling(RowNumber(Nothing)/Parameters!RowsPerPage.Value)  
    ```  
  
     Pour plus d’informations sur la définition de sauts de page pour un groupe, consultez [Ajouter un saut de page &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-page-break-report-builder-and-ssrs.md).  
  
##  <a name="properties"></a><a name="Properties"></a> Propriétés  
 Les expressions ne sont pas uniquement utilisées pour afficher des données dans des zones de texte. Elles peuvent également servir à modifier la manière dont les propriétés sont appliquées aux éléments de rapport. Vous pouvez modifier les informations de style d'un élément de rapport ou bien modifier sa visibilité.  
  
###  <a name="formatting"></a><a name="Formatting"></a> Mise en forme  
  
-   L’expression suivante, quand elle est utilisée dans la propriété Color d’une zone de texte, modifie la couleur du texte en fonction de la valeur du champ `Profit` :  
  
    ```  
    =Iif(Fields!Profit.Value < 0, "Red", "Black")  
    ```  
  
     Vous pouvez également utiliser la variable objet [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]`Me`. Cette variable constitue un autre moyen de faire référence à la valeur d'une zone de texte.  
  
     `=Iif(Me.Value < 0, "Red", "Black")`  
  
-   L’expression suivante, quand elle est utilisée dans la propriété BackgroundColor d’un élément de rapport dans une région de données, fait alterner la couleur d’arrière-plan de chaque ligne entre vert pâle et blanc :  
  
    ```  
    =Iif(RowNumber(Nothing) Mod 2, "PaleGreen", "White")  
    ```  
  
     Si vous utilisez une expression pour une étendue spécifique, vous devrez peut-être indiquer le dataset de la fonction d'agrégation :  
  
    ```  
    =Iif(RowNumber("Employees") Mod 2, "PaleGreen", "White")  
    ```  
  
> [!NOTE]  
>  Les couleurs disponibles proviennent de l’énumération [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] KnownColor.  
  
### <a name="chart-colors"></a>Couleurs du graphique  
 Pour spécifier les couleurs d'un graphique à base de formes, vous pouvez utiliser du code personnalisé pour contrôler l'ordre dans lequel les couleurs sont mappées aux valeurs de point de données. Cela vous permet d'utiliser des couleurs cohérentes dans le cadre de plusieurs graphiques possédant les mêmes groupes de catégories. Pour plus d’informations, consultez [Spécifier des couleurs cohérentes pour plusieurs graphiques à base de formes &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/specify-consistent-colors-across-multiple-shape-charts-report-builder-and-ssrs.md).  
  
###  <a name="visibility"></a><a name="Visibility"></a> Visibilité  
 Vous pouvez également afficher et masquer des éléments dans un rapport en utilisant les propriétés de visibilité de l'élément de rapport. Dans une région de données comme un tableau, vous pouvez initialement masquer les lignes de détails en fonction de la valeur d'une expression.  
  
-   L'expression suivante, lorsqu'elle est utilisée pour la visibilité initiale des lignes de détails dans un groupe, affiche les lignes de détails de toutes les ventes supérieures à 90 pour cent dans le champ `PctQuota` :  
  
    ```  
    =Iif(Fields!PctQuota.Value>.9, False, True)  
    ```  
  
-   L’expression suivante, quand elle est définie dans la propriété Hidden d’une table, affiche la table uniquement si elle compte plus de 12 lignes :  
  
    ```  
    =IIF(CountRows()>12,false,true)  
    ```  
  
-   L'expression suivante, lorsqu'elle est définie dans la propriété **Hidden** d'une colonne, affiche la colonne uniquement si le champ existe dans le dataset du rapport après que les données ont été récupérées de la source de données :  
  
    ```  
    =IIF(Fields!Column_1.IsMissing, true, false)  
    ```  
  
###  <a name="urls"></a><a name="Hyperlinks"></a> URL  
 Vous pouvez personnaliser des URL à l'aide de données de rapport, mais aussi contrôler de manière conditionnelle si les URL sont ajoutées en tant qu'action pour une zone de texte.  
  
-   L'expression ci-dessous, lorsqu'elle est utilisée en tant qu'action pour une zone de texte, génère une URL personnalisée qui spécifie le champ de dataset `EmployeeID` comme un paramètre d'URL.  
  
    ```  
    ="https://adventure-works/MyInfo?ID=" & Fields!EmployeeID.Value  
    ```  
  
     Pour plus d’informations, consultez [Ajouter un lien hypertexte à une URL &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/add-a-hyperlink-to-a-url-report-builder-and-ssrs.md).  
  
-   L'expression ci-dessous contrôle de manière conditionnelle si une URL doit être ajoutée à une zone de texte. Cette expression dépend d'un paramètre nommé `IncludeURLs` qui permet à un utilisateur de décider s'il faut inclure des URL actives dans un rapport. Cette expression est définie en tant qu'action pour une zone de texte. En affectant au paramètre la valeur False, puis en consultant le rapport, vous pouvez exporter le rapport vers Microsoft Excel sans liens hypertexte.  
  
    ```  
    =IIF(Parameters!IncludeURLs.Value,"https://adventure-works.com/productcatalog",Nothing)  
    ```  
  
##  <a name="report-data"></a><a name="ReportData"></a> Données des rapports  
 Les expressions peuvent être utilisées pour manipuler les données utilisées dans les rapports. Vous pouvez faire référence à des paramètres et à d'autres informations de rapport. Vous pouvez même modifier la requête utilisée pour extraire les données du rapport.  
  
###  <a name="parameters"></a><a name="Parameters"></a> Paramètres  
 Vous pouvez utiliser des expressions dans un paramètre pour faire varier la valeur par défaut du paramètre. Par exemple, vous pouvez utiliser un paramètre pour filtrer les données d'un utilisateur en particulier en fonction de l'ID utilisateur utilisé pour exécuter le rapport.  
  
-   L'expression ci-dessous, lorsqu'elle est utilisée comme valeur par défaut pour un paramètre, collecte l'ID utilisateur de la personne exécutant le rapport :  
  
    ```  
    =User!UserID  
    ```  
  
-   Pour faire référence à un paramètre dans un paramètre de requête, une expression de filtre, une zone de texte ou une autre zone du rapport, utilisez la collection globale **Parameters** . Cet exemple suppose que le paramètre est appelé *Department*:  
  
    ```  
    =Parameters!Department.Value  
    ```  
  
-   Des paramètres peuvent être créés dans un rapport, mais définis comme étant masqués. Lorsque le rapport s'exécute sur le serveur de rapports, le paramètre n'apparaît pas dans la barre d'outils et le lecteur du rapport ne peut pas modifier la valeur par défaut. Vous pouvez utiliser un paramètre masqué auquel est affectée une valeur par défaut en tant que constante personnalisée. Vous pouvez utiliser cette valeur dans n'importe quelle expression, y compris une expression de champ. L'expression suivante identifie le champ spécifié par la valeur de paramètre par défaut pour le paramètre nommé *ParameterField*:  
  
    ```  
    =Fields(Parameters!ParameterField.Value).Value  
    ```  
  
##  <a name="custom-code"></a><a name="CustomCode"></a> Code personnalisé  
 L'usage de code personnalisé au sein d'un rapport est possible. Le code personnalisé est soit incorporé au rapport, soit stocké dans un assembly personnalisé utilisé au sein du rapport. Pour plus d’informations sur le code personnalisé, consultez [Code personnalisé et références d’assembly dans les expressions du Concepteur de rapports &#40;SSRS&#41;](../../reporting-services/report-design/custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs.md).  
  
### <a name="using-group-variables-for-custom-aggregation"></a>Utilisation de variables de groupe pour une agrégation personnalisée  
 Vous pouvez initialiser la valeur pour une variable de groupe qui est locale à une étendue de groupe particulière, puis inclure une référence à cette variable dans les expressions. L'une des façons d'utiliser une variable de groupe avec du code personnalisé consiste à implémenter un agrégat personnalisé. Pour plus d'informations, consultez [Using Group Variables in Reporting Services 2008 for Custom Aggregation](/archive/blogs/robertbruckner/)(en anglais).  
  
 Pour plus d’informations sur les variables, consultez [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md).  
  
## <a name="suppressing-null-or-zero-values-at-run-time"></a>Suppression de valeurs Null ou zéro au moment de l'exécution  
 Certaines valeurs d'une expression peuvent prendre la valeur Null ou une valeur indéfinie au moment du traitement du rapport. Des erreurs d’exécution peuvent alors se produire et provoquer l’affichage de **#Erreur** au lieu de l’expression évaluée dans la zone de texte. La fonction **IIF** est particulièrement sensible à ce comportement, car, contrairement à une instruction If-Then-Else, chaque partie de l’instruction **IIF** est évaluée (y compris les appels de fonction) avant d’être passée à la routine de test **true** ou **false**. L’instruction `=IIF(Fields!Sales.Value is NOTHING, 0, Fields!Sales.Value)` génère **#Erreur** dans le rapport rendu si `Fields!Sales.Value` a la valeur NOTHING.  
  
 Pour éviter de vous trouver dans cette situation, utilisez l'une des stratégies qui suivent :  
  
-   Affectez au numérateur la valeur 0 et au dénominateur la valeur 1 si la valeur du champ B est 0 ou indéfinie ; sinon, affectez au numérateur la valeur du champ A et au dénominateur celle du champ B.  
  
    ```  
    =IIF(Field!B.Value=0, 0, Field!A.Value / IIF(Field!B.Value =0, 1, Field!B.Value))  
    ```  
  
-   Utilisez une fonction de code personnalisé pour retourner la valeur de l'expression. L'exemple ci-dessous retourne la différence, exprimée en pourcentage, entre une valeur actuelle et une valeur précédente. Il peut être utilisé pour calculer la différence entre deux valeurs consécutives quelconques et gère le cas limite de la première comparaison (quand il n’y a aucune valeur précédente) et les cas où la valeur précédente ou la valeur actuelle est **null** (**Nothing** en [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]).  
  
    ```  
    Public Function GetDeltaPercentage(ByVal PreviousValue, ByVal CurrentValue) As Object  
        If IsNothing(PreviousValue) OR IsNothing(CurrentValue) Then  
            Return Nothing  
        Else if PreviousValue = 0 OR CurrentValue = 0 Then  
            Return Nothing  
        Else   
            Return (CurrentValue - PreviousValue) / CurrentValue  
        End If  
    End Function  
    ```  
  
     L’expression suivante montre comment appeler ce code personnalisé à partir d’une zone de texte, pour le conteneur « ColumnGroupByYear » (groupe ou région de données).  
  
    ```  
    =Code.GetDeltaPercentage(Previous(Sum(Fields!Sales.Value),"ColumnGroupByYear"), Sum(Fields!Sales.Value))  
    ```  
  
     Cela permet d'éviter les exceptions d'exécution. Vous pouvez maintenant utiliser une expression comme `=IIF(Me.Value < 0, "red", "black")` dans la propriété **Color** de la zone de texte pour afficher de manière conditionnelle un texte selon que les valeurs sont supérieures ou inférieures à 0.  
  
## <a name="see-also"></a>Voir aussi  
 [Exemples d’équations de filtre &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/filter-equation-examples-report-builder-and-ssrs.md)   
 [Exemples d’expressions de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/group-expression-examples-report-builder-and-ssrs.md)   
 [Utilisation d’expressions dans les rapports &#40;Générateur de rapport et SSRS&#41;](../../reporting-services/report-design/expression-uses-in-reports-report-builder-and-ssrs.md)   
 [Expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expressions-report-builder-and-ssrs.md)   
 [Filtres couramment utilisés &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/commonly-used-filters-report-builder-and-ssrs.md)  
  
