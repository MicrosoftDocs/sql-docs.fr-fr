---
title: Code personnalisé et références d’assembly dans les expressions du Concepteur de rapports | Microsoft Docs
description: Découvrez comment ajouter des références à du code personnalisé incorporé dans un rapport. Générez et enregistrez sur votre ordinateur et déployez sur le serveur de rapports dans le Générateur de rapports.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
helpviewer_keywords:
- items [Reporting Services], expressions
- data [Reporting Services], expressions
- expressions [Reporting Services], about expressions
- expressions [Reporting Services]
- SSRS, expressions
- formulas [Reporting Services]
- data manipulation [Reporting Services]
- SQL Server Reporting Services, expressions
ms.assetid: ae8a0166-2ccc-45f4-8d28-c150da7b73de
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 31e92b49186e79c78cbaa2d13859d8dd3abe5c5d
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596338"
---
# <a name="custom-code-and-assembly-references-in-expressions-in-report-designer-ssrs"></a>Code personnalisé et références d'assembly dans les expressions du Concepteur de rapports (SSRS)
  Vous pouvez ajouter des références à du code personnalisé incorporé dans un rapport ou à des assemblys personnalisés que vous générez et enregistrez sur votre ordinateur et déployez sur le serveur de rapports. L'incorporation de code convient dans le cas de constantes personnalisées, de fonctions complexes ou de fonctions utilisées plusieurs fois dans un même rapport. Quant aux assemblys personnalisés, il est préférable de les utiliser si vous voulez centraliser du code en un emplacement unique et le partager en vue de son utilisation par plusieurs rapports. Le code personnalisé peut inclure de nouvelles constantes personnalisées, variables, fonctions ou sous-routines. Vous pouvez inclure des références en lecture seule à des collections intégrées, telles que la collection de paramètres. Cependant, les fonctions personnalisées ne peuvent pas recevoir des ensembles de valeurs de données de rapport : les agrégations personnalisées ne sont notamment pas prises en charge.  
  
> [!IMPORTANT]  
>  Pour les calculs pour lesquels le temps est important qui sont évalués une fois au moment de l'exécution et qui doivent conserver la même valeur pendant le traitement du rapport, voyez s'il convient d'utiliser une variable de rapport ou une variable de groupe. Pour plus d’informations, consultez [Références à des collections de variables de rapport et de groupe &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-report-and-group-variables-references-report-builder.md).  
  
 Le Concepteur de rapports est l'environnement de création recommandé pour ajouter du code personnalisé à un rapport. Le Générateur de rapports prend en charge le traitement des rapports qui contiennent des expressions valides, ou qui incluent des références à des assemblys personnalisés sur un serveur de rapports. Le Générateur de rapports ne permet pas d'ajouter une référence à un assembly personnalisé.  
  
> [!NOTE]  
>  Sachez que la mise à niveau d'un serveur de rapports peut nécessiter des étapes supplémentaires lorsque les rapports dépendent d'assemblys personnalisés.  
  
> [!NOTE]  
>  [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]  
  
##  <a name="working-with-custom-code-in-report-builder"></a><a name="RB3"></a> Utilisation du code personnalisé dans le Générateur de rapports  
 Dans le Générateur de rapports, vous pouvez ouvrir un rapport d'un serveur de rapports qui inclut des références aux assemblys personnalisés. Par exemple, vous pouvez modifier les rapports qui sont créés et déployés à l'aide du Concepteur de rapports dans [!INCLUDE[ssBIDevStudioFull](../../includes/ssbidevstudiofull-md.md)]. Les assemblys personnalisés doivent être déployés sur le serveur de rapports.  
  
 Vous ne pouvez pas effectuer les opérations suivantes :  
  
1.  Ajouter des références ou des instances de membre de classe à un rapport.  
  
2.  Afficher l'aperçu d'un rapport avec des références aux assemblys personnalisés en mode local.  
  
##  <a name="including-references-to-commonly-used-functions"></a><a name="Common"></a> Intégration de références aux fonctions couramment utilisées  
 Utilisez la boîte de dialogue **Expression** pour consulter une liste classée par catégorie de fonctions courantes intégrées à [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]. Lorsque vous développez **Fonctions communes** et cliquez sur une catégorie, le volet **Élément** affiche la liste des fonctions que vous incluez dans une expression. Les fonctions courantes incluent des classes provenant des espaces de noms [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] <xref:System.Math> et <xref:System.Convert> ainsi que des fonctions de la bibliothèque d’exécutables [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)]. Pour plus de commodité, vous pouvez consulter les fonctions le plus communément utilisées dans la boîte de dialogue **Expression** , où elles sont répertoriées par catégorie : Texte, Date et heure, Math, Inspection, Flux de programme, Agrégat, Financier, Conversion et Divers. Les fonctions moins souvent utilisées n'apparaissent pas dans la liste, mais peuvent cependant être utilisées dans une expression.  
  
 Pour utiliser une fonction intégrée, double-cliquez sur son nom dans le volet Élément. Une description de la fonction s'affiche dans le volet Description et un exemple de l'appel de la fonction apparaît dans le volet d'exemple. Dans le volet du code, quand vous tapez le nom de la fonction suivi d’une parenthèse ouvrante **(** , l’aide d’IntelliSense affiche chaque syntaxe valide pour l’appel de la fonction. Par exemple, pour calculer la valeur maximale pour un champ nommé `Quantity` dans une table, ajoutez l'expression simple `=Max(` au volet du code, puis utilisez les balises actives pour consulter toutes les syntaxes valides possibles pour l'appel de la fonction. Pour compléter cet exemple, tapez `=Max(Fields!Quantity.Value)`.  
  
 Pour plus d’informations sur chaque fonction, consultez <xref:System.Math>, <xref:System.Convert>et [Membres de la bibliothèque runtime Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) sur MSDN.  
  
##  <a name="including-references-to-less-commonly-used-functions"></a><a name="NotCommon"></a> Intégration de références aux fonctions moins couramment utilisées  
 Pour inclure une référence à d’autres espaces de noms CLR moins couramment utilisés, vous devez utiliser une référence complète, par exemple <xref:System.Text.StringBuilder>. IntelliSense n'est pas pris en charge dans le volet du code de la boîte de dialogue **Expression** pour ces fonctions moins couramment utilisées.  
  
 Pour plus d'informations, consultez [Membres de la bibliothèque runtime Visual Basic](/dotnet/visual-basic/language-reference/runtime-library-members) sur MSDN.  
  
##  <a name="including-references-to-external-assemblies"></a><a name="External"></a> Intégration de références aux assemblys externes  
 Pour inclure une référence à une classe dans un assembly externe, vous devez identifier l'assembly pour le processeur de rapports. Utilisez la page **Références** de la boîte de dialogue **Propriétés du rapport** pour spécifier le nom complet de l'assembly à ajouter au rapport. Dans votre expression, vous devez utiliser le nom complet de la classe dans l'assembly. Les classes dans un assembly externe n'apparaissent pas dans la boîte de dialogue **Expression** ; vous devez fournir le nom correct de la classe. Un nom complet comprend l'espace de noms, le nom de la classe et le nom du membre.  
  
##  <a name="including-embedded-code"></a><a name="Embedded"></a> Intégration de code incorporé  
 Pour ajouter du code incorporé à un rapport, utilisez l'onglet Code de la boîte de dialogue **Propriétés du rapport** . Le bloc de code que vous créez peut contenir plusieurs méthodes. Les méthodes du code incorporé doivent être écrites en [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] et doivent être basées sur des instances. Le processeur de rapports ajoute automatiquement des références pour les espaces de noms System.Convert et System.Math. Utilisez la page **Références** de la boîte de dialogue **Propriétés du rapport** pour ajouter des références d'assembly supplémentaires. Pour plus d’informations, consultez [Ajouter une référence d’assembly à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md).  
  
 Les méthodes du code incorporé sont disponibles par l'intermédiaire d'un membre **Code** globalement défini. Pour y accéder, vous devez faire référence au membre **Code** et au nom de la méthode. L’exemple ci-dessous appelle la méthode **ToUSD**, qui convertit la valeur du champ `StandardCost` en une valeur exprimée en dollars :  
  
```  
=Code.ToUSD(Fields!StandardCost.Value)  
```  
  
 Pour faire référence à des collections intégrées dans votre code personnalisé, incluez une référence à l’objet **Report** intégré :  
  
```  
=Report.Parameters!Param1.Value  
```  
  
 Les exemples ci-dessous indiquent comment définir des variables et des constantes personnalisées.  
  
```  
Public Const MyNote = "Authored by Bob"  
Public Const NCopies As Int32 = 2  
Public Dim  MyVersion As String = "123.456"  
Public Dim MyDoubleVersion As Double = 123.456  
```  
  
 Bien que les constantes personnalisées n’apparaissent pas dans la catégorie **Constantes** de la boîte de dialogue **Expression** (qui affiche uniquement les constantes intégrées), vous pouvez leur ajouter des références à partir de n’importe quelle expression, comme le montrent les exemples suivants. Dans une expression, une constante personnalisée est traitée comme une valeur de type **Variant**.  
  
```  
=Code.MyNote  
=Code.NCopies  
=Code.MyVersion  
=Code.MyDoubleVersion  
```  
  
 L’exemple suivant inclut la référence au code et l’implémentation de code de la fonction **FixSpelling**, qui substitue le texte `"Bicycle"` à toutes les occurrences du texte « Bike » dans le champ `SubCategory` .  
  
 `=Code.FixSpelling(Fields!SubCategory.Value)`  
  
 Le code suivant, quand il est incorporé dans un bloc de code de définition de rapport, affiche une implémentation de la méthode **FixSpelling** . Cet exemple indique comment utiliser une référence complète à la classe [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] **StringBuilder**.  
  
```vb  
Public Function FixSpelling(ByVal s As String) As String  
   Dim strBuilder As New System.Text.StringBuilder(s)  
   If s.Contains("Bike") Then  
      strBuilder.Replace("Bike", "Bicycle")  
      Return strBuilder.ToString()  
      Else : Return s  
   End If  
End Function  
```  
  
 Pour plus d’informations sur les collections d’objets intégrées et leur initialisation, consultez [Références à des champs Globals et Users prédéfinis &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/built-in-collections-built-in-globals-and-users-references-report-builder.md) et [Initialisation d’objets Assembly personnalisés](../../reporting-services/custom-assemblies/initializing-custom-assembly-objects.md).  
  
##  <a name="including-references-to-parameters-from-code"></a><a name="Parameters"></a> Intégration de références à des paramètres de code  
 Vous pouvez référencer la collection globale de paramètres via du code personnalisé dans un bloc de code de la définition de rapport ou dans un assembly personnalisé que vous fournissez. La collection de paramètres est en lecture seule et ne possède aucun itérateur public. Vous ne pouvez pas utiliser de construction [!INCLUDE[vbprvb](../../includes/vbprvb-md.md)] **For Each** pour parcourir la collection. Vous devez connaître le nom du paramètre défini dans la définition de rapport pour pouvoir ensuite le référencer dans votre code. Cependant, vous pouvez effectuer une itération dans toutes les valeurs d’un paramètre à valeurs multiples.  
  
 Le tableau suivant présente des exemples du référencement de la collection `Parameters` intégrée à partir du code personnalisé :  
  
 **Transmission de l’intégralité de la collection globale de paramètres au code personnalisé.** Cette fonction retourne la valeur d’un paramètre de rapport spécifique *MyParameter*.  
  
 Référence dans une expression `=Code.DisplayAParameterValue(Parameters)`  
  
 Définition de code personnalisé  
  
```  
Public Function DisplayAParameterValue(ByVal parameters as Parameters) as Object  
Return parameters("MyParameter").Value  
End Function  
```  
  
 **Transmission d'un seul paramètre au code personnalisé.**  
  
 Référence dans une expression `=Code.ShowParametersValues(Parameters!DayOfTheWeek)`  
  
 Cet exemple retourne la valeur du paramètre transmis. Si le paramètre est un paramètre à valeurs multiples, la chaîne de retour est une concaténation de toutes les valeurs.  
  
 Définition de code personnalisé  
  
```  
Public Function ShowParameterValues(ByVal parameter as Parameter)  
 as String  
   Dim s as String   
   If parameter.IsMultiValue then  
      s = "Multivalue: "   
      For i as integer = 0 to parameter.Count-1  
         s = s + CStr(parameter.Value(i)) + " "   
      Next  
   Else  
      s = "Single value: " + CStr(parameter.Value)  
   End If  
   Return s  
End Function  
```  
  
##  <a name="including-references-to-code-from-custom-assemblies"></a><a name="Custom"></a> Intégration de références à du code d'assemblys personnalisés  
 Pour utiliser un assembly personnalisé dans un rapport, vous devez créer l'assembly, le rendre accessible au Concepteur de rapports, ajouter une référence à l'assembly dans le rapport et utiliser une expression dans le rapport qui fait référence aux méthodes contenues dans cet assembly. Lorsque le rapport est déployé sur le serveur de rapports, vous devez également déployer l'assembly personnalisé sur le serveur de rapports.  
  
 Pour plus d’informations sur la création d’un assembly personnalisé et sur la façon de le rendre disponible pour [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)], consultez [Utilisation d’assemblys personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md).  
  
 Pour faire référence à du code personnalisé dans une expression, vous devez appeler le membre d'une classe au sein de l'assembly. La procédure pour ce faire dépend du type de méthode, à savoir statique ou basée sur une instance. Les méthodes statiques d'un assembly personnalisé sont disponibles de façon globale dans le rapport. Elles sont accessibles dans les expressions en spécifiant l'espace de noms, la classe et le nom de la méthode. L'exemple de code ci-dessous appelle la méthode **ToGBP**, qui convertit la valeur en dollars du champ **StandardCost** en une valeur exprimée en livres sterling :  
  
```  
=CurrencyConversion.DollarCurrencyConversion.ToGBP(Fields!StandardCost.Value)  
```  
  
 Les méthodes basées sur des instances sont disponibles par l’intermédiaire d’un membre **Code** globalement défini. Vous pouvez y accéder en faisant référence au membre **Code** , suivi du nom de l'instance et de la méthode. L'exemple de code ci-dessous appelle la méthode **ToEUR**, qui convertit la valeur en dollars du champ **StandardCost** en une valeur exprimée en euros :  
  
```  
=Code.m_myDollarCoversion.ToEUR(Fields!StandardCost.Value)  
```  
  
> [!NOTE]  
>  Dans le Concepteur de rapports, un assembly personnalisé est chargé une fois et il n'est pas déchargé tant que vous ne fermez pas [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Si vous affichez un aperçu d'un rapport, modifiez un assembly personnalisé utilisé dans ce rapport, puis affichez à nouveau l'aperçu, vos modifications n'apparaissent pas dans ce deuxième aperçu. Pour recharger l'assembly, fermez et rouvrez [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] , puis affichez l'aperçu du rapport.  
  
 Pour plus d'informations sur l'accès à votre code, consultez [Accessing Custom Assemblies Through Expressions](../../reporting-services/custom-assemblies/accessing-custom-assemblies-through-expressions.md).  
  
##  <a name="passing-built-in-collections-into-custom-assemblies"></a><a name="collections"></a> Passage de collections intégrées dans les assemblys personnalisés  
 Si vous voulez transmettre des collections intégrées, telles que les collections *Globals* ou *Parameters* , dans un assembly personnalisé pour le traitement, vous devez ajouter une référence d’assembly dans votre projet de code au niveau de l’assembly qui définit les collections intégrées, et vous devez accéder à l’espace de noms correct. Selon que vous développez l'assembly personnalisé pour un rapport exécuté sur un serveur de rapports (rapport du serveur) ou un rapport exécuté localement dans une application .NET. (rapport local), l'assembly que vous devez référencer est différent. Voir les détails ci-dessous.  
  
-   **Espace de noms :** Microsoft.ReportingServices.ReportProcessing.ReportObjectModel  
  
-   **Assembly (rapport local) :** Microsoft.ReportingServices.ProcessingObjectModel.dll  
  
-   **Assembly (rapport de serveur) :** Microsoft.ReportViewer.ProcessingObjectModel.dll  
  
 Comme le contenu des collections *Fields* et *ReportItems* peut changer dynamiquement pendant l’exécution, vous ne devriez pas les conserver entre les appels à l’assembly personnalisé (par exemple, dans une variable membre). La même recommandation s'applique généralement à toutes les collections intégrées.  
  
## <a name="see-also"></a>Voir aussi  
 [Ajouter du code à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-code-to-a-report-ssrs.md)   
 [Utilisation d'assemblys personnalisés avec des rapports](../../reporting-services/custom-assemblies/using-custom-assemblies-with-reports.md)   
 [Ajouter une référence d’assembly à un rapport &#40;SSRS&#41;](../../reporting-services/report-design/add-an-assembly-reference-to-a-report-ssrs.md)   
 [Didacticiels sur Reporting Services &#40;SSRS&#41;](../../reporting-services/reporting-services-tutorials-ssrs.md)   
 [Exemples d’expressions &#40;Générateur de rapports et SSRS&#41;](../../reporting-services/report-design/expression-examples-report-builder-and-ssrs.md)   
 [Exemples de rapports (Générateur de rapports et SSRS)](https://go.microsoft.com/fwlink/?LinkId=198283)  
  
