---
description: Création d'une transformation synchrone à l'aide du composant Script
title: Création d’une transformation synchrone à l’aide du composant Script | Microsoft Docs
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: reference
dev_langs:
- VB
helpviewer_keywords:
- synchronous outputs [Integration Services]
- transformation components [Integration Services]
- Script component [Integration Services], transformation components
ms.assetid: aa1bee1a-ab06-44d8-9944-4bff03d73016
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 08c7e20120ef5ff746b495535843bc93a46fea16
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344896"
---
# <a name="creating-a-synchronous-transformation-with-the-script-component"></a>Création d'une transformation synchrone à l'aide du composant Script

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  Vous utilisez un composant de transformation dans le flux de données d'un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] pour modifier et analyser les données acheminées de la source à la destination. Une transformation à sorties synchrones traite chacune des lignes d'entrée lorsqu'elles traversent le composant. Une transformation à sorties asynchrones patiente jusqu'à ce qu'elle ait reçu toutes les lignes d'entrée pour achever son traitement. Cette rubrique décrit une transformation synchrone. Pour plus d’informations sur les transformations asynchrones, consultez [Création d’une transformation asynchrone à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md). Pour plus d’informations sur la différence entre les composants synchrones et asynchrones, consultez [Présentation des transformations synchrones et asynchrones](../../integration-services/understanding-synchronous-and-asynchronous-transformations.md).  
  
 Pour une vue d’ensemble du composant Script, consultez [Extension du flux de données avec le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/extending-the-data-flow-with-the-script-component.md).  
  
 Le composant Script et le code d'infrastructure qu'il génère simplifient considérablement le processus qui consiste à développer un composant de flux de données personnalisé. Toutefois, pour comprendre le fonctionnement du composant Script, il peut être utile de lire les étapes que vous devez suivre pour développer un composant de flux de données personnalisé dans la section [Développement d’un composant de flux de données personnalisé](../../integration-services/extending-packages-custom-objects/data-flow/developing-a-custom-data-flow-component.md) et plus particulièrement dans [Développement d’un composant de transformation personnalisé à sorties synchrones](../../integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md).  
  
## <a name="getting-started-with-a-synchronous-transformation-component"></a>Mise en route d'un composant de transformation synchrone  
 Lorsque vous ajoutez un composant Script au volet Flux de données du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)], la boîte de dialogue **Sélectionner le type de composant de script** s’ouvre et vous invite à sélectionner un type de composant Source, Destination ou Transformation. Dans cette boîte de dialogue, sélectionnez **Transformation**.  
  
## <a name="configuring-a-synchronous-transformation-component-in-metadata-design-mode"></a>Configuration d'un composant de transformation synchrone en mode Création des métadonnées  
 Après avoir sélectionné l’option de création d’un composant de transformation, configurez le composant dans l’**Éditeur de transformation de script**. Pour plus d’informations, consultez [Configuration du composant Script dans l’éditeur de composant de script](../../integration-services/extending-packages-scripting/data-flow-script-component/configuring-the-script-component-in-the-script-component-editor.md).  
  
 Pour définir le langage de script du composant Script, vous définissez la propriété **ScriptLanguage** dans la page **Script** de l’**Éditeur de transformation de script**.  
  
> [!NOTE]  
>  Pour définir le langage de script par défaut du composant Script, utilisez l’option **Langage de script** dans la page **Général** de la boîte de dialogue **Options**. Pour plus d’informations, consultez [Page Général](../general-page-of-integration-services-designers-options.md).  
  
 Un composant de transformation de flux de données possède une entrée et prend en charge une ou plusieurs sorties. La configuration de l’entrée et des sorties du composant est l’une des étapes à exécuter en mode Création des métadonnées, à l’aide de l’**Éditeur de transformation de script**, avant d’écrire le script personnalisé.  
  
### <a name="configuring-input-columns"></a>Configuration de colonnes d'entrée  
 Un composant de transformation a une entrée.  
  
 Dans la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, la liste des colonnes affiche les colonnes disponibles de la sortie du composant en amont du flux de données. Sélectionnez les colonnes à transformer ou à passer. Marquez toutes les colonnes que vous souhaitez transformer sur place comme accessibles en lecture/écriture.  
  
 Pour plus d’informations sur la page **Colonnes d’entrée** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Colonnes d’entrée&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="configuring-inputs-outputs-and-output-columns"></a>Configuration des entrées, des sorties et des colonnes de sortie  
 Un composant de transformation prend en charge une ou plusieurs sorties.  
  
 Dans la page **Entrées et sorties** de l’**Éditeur de transformation de script**, vous pouvez noter qu’une seule sortie a été créée, mais qu’elle ne contient aucune colonne. Dans cette page de l'éditeur, vous pouvez avoir besoin ou envie de configurer les éléments suivants.  
  
-   Créez une ou plusieurs sorties supplémentaires, telles qu'une sortie d'erreur simulée pour les lignes qui contiennent des valeurs inattendues. Les boutons **Ajouter une sortie** et **Supprimer une sortie** permettent de gérer les sorties du composant de transformation synchrone. Toutes les lignes d'entrée sont dirigées vers toutes les sorties disponibles sauf si vous indiquez que vous envisagez de rediriger chaque ligne vers une sortie ou une autre. Pour indiquer que vous envisagez de rediriger des lignes, vous spécifiez une valeur entière non nulle pour la propriété **ExclusionGroup** des sorties. La valeur entière spécifique entrée dans **ExclusionGroup** pour identifier les sorties n’est pas significative, mais vous devez utiliser le même entier de manière cohérente pour le groupe spécifié de sorties.  
  
    > [!NOTE]  
    >  Vous pouvez également utiliser une valeur de propriété **ExclusionGroup** non nulle avec une sortie unique lorsque vous ne souhaitez pas générer des sorties pour toutes les lignes. Toutefois, dans ce cas, vous devez appeler explicitement la méthode **DirectRowTo\<outputbuffer>** pour chaque ligne que vous souhaitez envoyer à la sortie.  
  
-   Attribuez un plus nom descriptif à l'entrée et aux sorties. Le composant Script utilise ces noms pour générer les propriétés d'accesseur typées qui vous permettent de référencer l'entrée et les sorties dans le script.  
  
-   Laissez des colonnes inchangées pour les transformations synchrones. En général, une transformation synchrone n'ajoute pas de colonnes au flux de données. Les données sont modifiées sur place dans le tampon, et le tampon est transmis au composant suivant dans le flux de données. Le cas échéant, vous n'avez pas à ajouter et configurer explicitement des colonnes de sortie sur les sorties de la transformation. Les sorties apparaissent dans l'éditeur sans aucune colonne définie explicitement.  
  
-   Ajoutez de nouvelles colonnes aux sorties d'erreur simulées pour les erreurs au niveau des lignes. Normalement, plusieurs sorties dans le même **ExclusionGroup** possèdent le même jeu de colonnes de sortie. Toutefois, si vous créez une sortie d'erreur simulée, vous pouvez ajouter plus de colonnes pour contenir des informations d'erreur. Pour plus d’informations sur la manière dont le moteur de flux de données traite les lignes d’erreur, consultez [Utilisation de sorties d’erreur dans un composant de flux de données](../../integration-services/extending-packages-custom-objects/data-flow/using-error-outputs-in-a-data-flow-component.md). Notez que, dans le composant Script, vous devez écrire votre propre code pour remplir les colonnes supplémentaires à l'aide des informations d'erreur appropriées. Pour plus d’informations, consultez [Simulation d’une sortie d’erreur pour le composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-examples/simulating-an-error-output-for-the-script-component.md).  
  
 Pour plus d’informations sur la page **Entrées et sorties** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Entrées et sorties&#41;](../data-flow/transformations/script-component.md).  
  
### <a name="adding-variables"></a>Ajout de variables  
 Si vous voulez utiliser des variables existantes dans votre script, vous pouvez les ajouter dans les champs de propriété **ReadOnlyVariables** et **ReadWriteVariables** de la page **Script** de l’**Éditeur de transformation de script**.  
  
 Lorsque vous ajoutez plusieurs variables dans les champs de propriété, séparez les noms de variables par des virgules. Vous pouvez également sélectionner plusieurs variables en cliquant sur le bouton de sélection ( **...** ) en regard des champs de propriété **ReadOnlyVariables** et **ReadWriteVariables**, puis en sélectionnant les variables dans la boîte de dialogue **Sélectionner des variables**.  
  
 Pour obtenir des informations générales sur l’utilisation de variables avec le composant Script, consultez [Utilisation de variables dans le composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/using-variables-in-the-script-component.md).  
  
 Pour plus d’informations sur la page **Script** de l’**Éditeur de transformation de script**, consultez [Éditeur de transformation de script &#40;page Script&#41;](../data-flow/transformations/script-component.md).  
  
## <a name="scripting-a-synchronous-transformation-component-in-code-design-mode"></a>Script d'un composant de transformation synchrone en mode Création du code  
 Après avoir configuré les métadonnées du composant, vous pouvez écrire votre script personnalisé. Dans l’**Éditeur de transformation de script**, dans la page **Script**, cliquez sur **Modifier le script** pour ouvrir l’environnement de développement intégré [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)] Tools for Applications (VSTA) où vous pouvez ajouter votre script personnalisé. Le langage de script utilisé varie selon que vous avez sélectionné [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic ou [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual C# comme langage de script pour la propriété **ScriptLanguage** dans la page **Script**.  
  
 Pour obtenir des informations importantes concernant tous les types de composants créés à l’aide du composant Script, consultez [Codage et débogage du composant Script](../../integration-services/extending-packages-scripting/data-flow-script-component/coding-and-debugging-the-script-component.md).  
  
### <a name="understanding-the-auto-generated-code"></a>Fonctionnement du code généré automatiquement  
 Lorsque vous ouvrez l’environnement de développement intégré VSTA après avoir créé et configuré un composant de transformation, la classe **ScriptMain** modifiable apparaît dans l’éditeur de code avec un stub pour la méthode **ProcessInputRow**. La classe **ScriptMain** est l’emplacement où vous allez écrire votre code personnalisé et **ProcessInputRow** est la méthode la plus importante d’un composant de transformation.  
  
 Si vous ouvrez la fenêtre **Explorateur de projets** dans VSTA, vous constatez que le composant Script a également généré des éléments de projet **BufferWrapper** et **ComponentWrapper** en lecture seule. La classe **ScriptMain** hérite de la classe **UserComponent** dans l’élément de projet **ComponentWrapper**.  
  
 Au moment de l’exécution, le moteur de flux de données appelle la méthode **ProcessInput** dans la classe **UserComponent**, qui remplace la méthode <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent.ProcessInput%2A> de la classe parente <xref:Microsoft.SqlServer.Dts.Pipeline.ScriptComponent>. La méthode **ProcessInput** parcourt à son tour les lignes du tampon d’entrée et appelle la méthode **ProcessInputRow** une fois pour chaque ligne.  
  
### <a name="writing-your-custom-code"></a>Écriture du code personnalisé  
 Un composant de transformation à sorties synchrones constitue le plus simple à écrire de tous les composants de flux de données. Par exemple, l'exemple à sortie unique présenté ultérieurement dans cette rubrique se compose du code personnalisé suivant :  
  
```vb  
Row.City = UCase(Row.City)  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
```  
  
 Pour achever la création d’un composant de transformation synchrone personnalisé, vous devez utiliser la méthode **ProcessInputRow** remplacée pour transformer les données dans chaque ligne du tampon d’entrée. Le moteur de flux de données passe ce tampon, lorsqu'il est complet, au composant suivant dans le flux de données.  
  
 Selon vos besoins, vous voudrez également écrire le script dans les méthodes **PreExecute** et **PostExecute** disponibles dans la classe **ScriptMain** pour effectuer tout traitement préliminaire ou final.  
  
### <a name="working-with-multiple-outputs"></a>Utilisation de plusieurs sorties  
 La direction de lignes d'entrée vers l'une de deux sorties possibles ne requiert pas beaucoup plus de code personnalisé que le scénario de la sortie unique présenté précédemment. Par exemple, l'exemple à deux sorties présenté ultérieurement dans cette rubrique se compose du code personnalisé suivant :  
  
```vb  
Row.City = UCase(Row.City)  
If Row.City = "REDMOND" Then  
    Row.DirectRowToMyRedmondAddresses()  
Else  
    Row.DirectRowToMyOtherAddresses()  
End If  
```  
  
```csharp  
Row.City = (Row.City).ToUpper();  
  
if (Row.City=="REDMOND")  
{  
    Row.DirectRowToMyRedmondAddresses();  
}  
else  
{  
    Row.DirectRowToMyOtherAddresses();  
}  
```  
  
 Dans cet exemple, le composant Script génère les méthodes **DirectRowTo\<OutputBufferX>** pour vous, en fonction des noms des sorties que vous avez configurées. Vous pouvez utiliser un code similaire pour diriger des lignes d'erreur vers une sortie d'erreur simulée.  
  
## <a name="examples"></a>Exemples  
 Ces exemples présentent le code personnalisé requis dans la classe **ScriptMain** pour créer un composant de transformation synchrone.  
  
> [!NOTE]  
>  Ces exemples utilisent la table **Person.Address** de l’exemple de base de données **AdventureWorks** et passent ses première et quatrième colonnes (à savoir les colonnes **intAddressID** et **nvarchar(30)City**) dans le flux de données. Les mêmes données sont utilisées dans les exemples de source, transformation et destination de cette section. Des conditions préalables et des hypothèses supplémentaires sont documentées pour chaque exemple.  
  
### <a name="single-output-synchronous-transformation-example"></a>Exemple de transformation synchrone à sortie unique  
 Cet exemple présente un composant de transformation synchrone à sortie unique. Cette transformation passe la colonne **AddressID** et convertit la colonne **City** en majuscules.  
  
 Si vous souhaitez exécuter cet exemple de code, vous devez configurer le package et le composant comme suit :  
  
1.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que transformation.  
  
2.  Connectez la sortie d'une source ou d'une autre transformation au nouveau composant de transformation dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Cette sortie doit fournir les données de la table **Person.Address** de l’exemple de base de données **AdventureWorks** qui contient les colonnes **AddressID** et **City**.  
  
3.  Ouvrez l' **Éditeur de transformation de script**. Dans la page **Colonnes d’entrée**, sélectionnez les colonnes **AddressID** et **City**. Marquez la colonne **City** comme accessible en lecture/écriture.  
  
4.  Dans la page **Entrées et sorties**, renommez l’entrée et la sortie en leur attribuant des noms plus descriptifs, comme **MyAddressInput** et **MyAddressOutput**. Notez que la propriété **SynchronousInputID** de la sortie correspond à la propriété **ID** de l’entrée. Par conséquent, vous n'avez pas à ajouter et configurer des colonnes de sortie.  
  
5.  Dans la page **Script** , cliquez sur **Modifier le script** , puis entrez le script suivant. Ensuite, fermez l'environnement de développement de script et l' **Éditeur de transformation de script**.  
  
6.  Créez et configurez un composant de destination qui attend les colonnes **AddressID** et **City**, comme une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ou l’exemple de composant de destination présenté dans [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Connectez ensuite la sortie de la transformation au composant de destination. Vous pouvez créer une table de destination en exécutant la commande [!INCLUDE[tsql](../../includes/tsql-md.md)] suivante dans la base de données **AdventureWorks** :  
  
    ```sql
    CREATE TABLE [Person].[Address2]([AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL)  
    ```  
  
7.  Exécutez l’exemple.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
{  
    public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
    }  
  
}  
```  
  
### <a name="two-output-synchronous-transformation-example"></a>Exemple de transformation synchrone à deux sorties  
 Cet exemple présente un composant de transformation synchrone à deux sorties. Cette transformation passe la colonne **AddressID** et convertit la colonne **City** en majuscules. Si le nom de la ville est Redmond, l'exemple dirige la ligne vers une seule sortie ; il dirige toutes les autres lignes vers une autre sortie.  
  
 Si vous souhaitez exécuter cet exemple de code, vous devez configurer le package et le composant comme suit :  
  
1.  Ajoutez un nouveau composant Script à l'aire du concepteur de flux de données et configurez-le en tant que transformation.  
  
2.  Connectez la sortie d'une source ou d'une autre transformation au nouveau composant de transformation dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)]. Cette sortie doit fournir les données de la table **Person.Address** de l’exemple de base de données **AdventureWorks** qui contient au moins les colonnes **AddressID** et **City**.  
  
3.  Ouvrez l' **Éditeur de transformation de script**. Dans la page **Colonnes d’entrée**, sélectionnez les colonnes **AddressID** et **City**. Marquez la colonne **City** comme accessible en lecture/écriture.  
  
4.  Dans la page **Entrées et sorties**, créez une seconde sortie. Après avoir ajouté la nouvelle sortie, assurez-vous de définir sa propriété **SynchronousInputID** sur la propriété **ID** de l’entrée. Cette propriété est déjà définie sur la première sortie, créée par défaut. Pour chaque de sortie, définissez la propriété **ExclusionGroup** sur la même valeur différente de zéro pour indiquer que vous fractionnerez les lignes d’entrée entre deux sorties mutuellement exclusives. Il n'est pas nécessaire d'ajouter des colonnes de sortie aux sorties.  
  
5.  Renommez l’entrée et les sorties en leur attribuant des noms plus descriptifs, tels que **MyAddressInput**, **MyRedmondAddresses** et **MyOtherAddresses**.  
  
6.  Dans la page **Script** , cliquez sur **Modifier le script** , puis entrez le script suivant. Ensuite, fermez l'environnement de développement de script et l' **Éditeur de transformation de script**.  
  
7.  Créez et configurez deux composants de destination qui attendent les colonnes **AddressID** et **City**, comme une destination [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], une destination de fichier plat ou l’exemple de composant de destination présenté dans [Création d’une destination à l’aide du composant Script](../../integration-services/extending-packages-scripting-data-flow-script-component-types/creating-a-destination-with-the-script-component.md). Puis connectez chacune des sorties de la transformation à l'un des composants de destination. Vous pouvez créer des tables de destination en exécutant une commande [!INCLUDE[tsql](../../includes/tsql-md.md)] similaire à la suivante (avec des noms de tables uniques) dans la base de données **AdventureWorks** :  
  
    ```sql
    CREATE TABLE [Person].[Address2](  
        [AddressID] [int] NOT NULL,  
        [City] [nvarchar](30) NOT NULL  
    ```  
  
8.  Exécutez l’exemple.  
  
```vb  
Public Class ScriptMain  
    Inherits UserComponent  
  
    Public Overrides Sub MyAddressInput_ProcessInputRow(ByVal Row As MyAddressInputBuffer)  
  
        Row.City = UCase(Row.City)  
  
        If Row.City = "REDMOND" Then  
            Row.DirectRowToMyRedmondAddresses()  
        Else  
            Row.DirectRowToMyOtherAddresses()  
        End If  
  
    End Sub  
  
End Class  
```  
  
```csharp  
public class ScriptMain:  
    UserComponent  
  
public override void MyAddressInput_ProcessInputRow(MyAddressInputBuffer Row)  
    {  
  
        Row.City = (Row.City).ToUpper();  
  
        if (Row.City == "REDMOND")  
        {  
            Row.DirectRowToMyRedmondAddresses();  
        }  
        else  
        {  
            Row.DirectRowToMyOtherAddresses();  
        }  
  
    }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Présentation des transformations synchrones et asynchrones](~/integration-services/understanding-synchronous-and-asynchronous-transformations.md)  
 [Création d’une transformation asynchrone à l’aide du composant Script](~/integration-services/extending-packages-scripting-data-flow-script-component-types/creating-an-asynchronous-transformation-with-the-script-component.md)  
 [Développement d’un composant de transformation personnalisé avec des sorties synchrones](~/integration-services/extending-packages-custom-objects-data-flow-types/developing-a-custom-transformation-component-with-synchronous-outputs.md)
