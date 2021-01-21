---
title: Création d’un composant au moment de la conception d’éléments de rapport personnalisé | Microsoft Docs
description: Découvrez comment créer un composant design-time d’élément de rapport personnalisé. Ce composant fournit une aire de conception activée qui accepte les opérations de glisser-déplacer.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: 323fd58a-a462-4c48-b188-77ebc0b4212e
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 237d056f8b42570fcab375330a9eb3172eb11758
ms.sourcegitcommit: d8cdbb719916805037a9167ac4e964abb89c3909
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/20/2021
ms.locfileid: "98596963"
---
# <a name="creating-a-custom-report-item-design-time-component"></a>Création d'un composant au moment de la conception d'élément de rapport personnalisé
  Un composant au moment de la conception d'élément de rapport personnalisé est un contrôle qui peut être utilisé dans l'environnement du Concepteur de rapports Visual Studio. Le composant au moment de la conception d'élément de rapport personnalisé fournit une aire de conception activée qui prend en charge les opérations de glisser-déplacer et l'intégration avec l'Explorateur de propriétés [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], tout en fournissant des éditeurs de propriété personnalisée.  
  
 Avec un composant au moment de la conception d'élément de rapport personnalisé, l'utilisateur peut positionner un élément de rapport personnalisé sur un rapport dans l'environnement de conception, définir des propriétés de données personnalisées sur l'élément de rapport personnalisé, puis enregistrer cet élément en tant que partie du projet de rapport.  
  
 Les propriétés définies à l'aide du composant au moment de la conception dans l'environnement de développement sont sérialisées et désérialisées par l'environnement de conception hôte, puis stockées comme éléments dans le fichier RDL (Report Definition Language). Lorsque le rapport est exécuté par le processeur de rapports, les propriétés définies à l'aide du composant au moment de la conception sont passées par le processeur de rapports à un composant d'exécution d'élément de rapport personnalisé, qui génère l'élément de rapport personnalisé et le repasse au processeur de rapports.  
  
> [!NOTE]
>  Le composant au moment de la conception d’élément de rapport personnalisé est implémenté en tant que composant [!INCLUDE[msCoName](../../includes/msconame-md.md)][!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)]. Ce document décrit les détails d'implémentation spécifiques au composant au moment de la conception d'élément de rapport personnalisé. Pour plus d’informations sur le développement de composants à l’aide de [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez [Composants dans Visual Studio](https://go.microsoft.com/fwlink/?LinkId=116576) dans MSDN Library.  
  
 Pour un exemple d’élément de rapport personnalisé totalement implémenté, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="implementing-a-design-time-component"></a>Implémentation d'un composant au moment de la conception  
 La classe principale d’un composant au moment de la conception d’élément de rapport personnalisé est héritée de la classe **Microsoft.ReportDesigner.CustomReportItemDesigner**. En plus des attributs standard utilisés pour un contrôle [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], votre classe de composant doit définir un attribut **CustomReportItem**. Cet attribut doit correspondre au nom de l'élément de rapport personnalisé tel que défini dans le fichier reportserver.config. Pour obtenir une liste des attributs [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)], consultez la section Attributs dans la documentation du SDK du [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)].  
  
 L'exemple de code suivant illustre l'application d'attributs à un contrôle au moment de la conception d'élément de rapport personnalisé :  
  
```csharp  
namespace PolygonsCRI  
{  
    [LocalizedName("Polygons")]  
    [Editor(typeof(CustomEditor), typeof(ComponentEditor))]  
        [ToolboxBitmap(typeof(PolygonsDesigner),"Polygons.ico")]  
        [CustomReportItem("Polygons")]  
  
    public class PolygonsDesigner : CustomReportItemDesigner  
    {  
...  
```  
  
### <a name="initializing-the-component"></a>Initialisation du composant  
 Pour passer des propriétés spécifiées par l'utilisateur pour un élément de rapport personnalisé, une classe <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> doit être utilisée. Votre implémentation de la classe **CustomReportItemDesigner** doit se substituer à la méthode **InitializeNewComponent** pour créer une instance de la classe <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> de votre composant et lui attribuer les valeurs par défaut.  
  
 L’exemple de code suivant illustre la substitution de la méthode **CustomReportItemDesigner.InitializeNewComponent** par une classe de composant au moment de la conception d’élément de rapport personnalisé pour initialiser la classe <xref:Microsoft.ReportingServices.RdlObjectModel.CustomData> du composant :  
  
```csharp  
public override void InitializeNewComponent()  
        {  
            CustomData = new CustomData();  
            CustomData.DataRowHierarchy = new DataHierarchy();  
  
            // Shape grouping  
            CustomData.DataRowHierarchy.DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].Group.Name = Name + "_Shape";  
            CustomData.DataRowHierarchy.DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Point grouping  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers.Add(new DataMember());  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group = new Group();  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.Name = Name + "_Point";  
            CustomData.DataRowHierarchy.DataMembers[0].DataMembers[0].Group.GroupExpressions.Add(new ReportExpression());  
  
            // Static column  
            CustomData.DataColumnHierarchy = new DataHierarchy();  
            CustomData.DataColumnHierarchy.DataMembers.Add(new DataMember());  
  
            // Points  
            IList<IList<DataValue>> dataValues = new List<IList<DataValue>>();  
            CustomData.DataRows.Add(dataValues);  
            CustomData.DataRows[0].Add(new List<DataValue>());  
            CustomData.DataRows[0][0].Add(NewDataValue("X", ""));  
            CustomData.DataRows[0][0].Add(NewDataValue("Y", ""));  
        }  
```  
  
### <a name="modifying-component-properties"></a>Modification des propriétés du composant  
 Vous pouvez modifier les propriétés **CustomData** de plusieurs façons dans l’environnement de conception. Vous pouvez modifier toute propriété exposée par le composant au moment de la conception et marquée avec l'attribut <xref:System.ComponentModel.BrowsableAttribute>, à l'aide de l'Explorateur de propriétés [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]. Vous pouvez également modifier des propriétés en faisant glisser des éléments vers l’aire de conception de l’élément de rapport personnalisé ou en cliquant avec le bouton droit sur le contrôle dans l’environnement de conception et en sélectionnant **Propriétés** dans le menu contextuel pour afficher une fenêtre de propriétés personnalisées.  
  
 L’exemple de code suivant montre une propriété **Microsoft.ReportDesigner.CustomReportItemDesigner.CustomData** dont l’attribut <xref:System.ComponentModel.BrowsableAttribute> est appliqué :  
  
```csharp  
[Browsable(true), Category("Data")]  
public string DataSetName  
{  
      get  
      {  
         return CustomData.DataSetName;  
      }  
      set  
      {  
         CustomData.DataSetName = value;  
      }  
   }  
  
```  
  
 Vous pouvez fournir votre composant au moment de la conception avec une boîte de dialogue d'éditeur de propriétés personnalisées. L'implémentation d'éditeur de propriétés personnalisées doit hériter de la classe <xref:System.ComponentModel.ComponentEditor> et doit créer une instance d'une boîte de dialogue destinée à l'édition de propriétés.  
  
 L'exemple suivant montre l'implémentation d'une classe qui hérite de <xref:System.ComponentModel.ComponentEditor> et ouvre une boîte de dialogue d'éditeur de propriétés personnalisées :  
  
```csharp  
internal sealed class CustomEditor : ComponentEditor  
{  
   public override bool EditComponent(  
      ITypeDescriptorContext context, object component)  
    {  
     PolygonsDesigner designer = (PolygonsDesigner)component;  
     PolygonProperties dialog = new PolygonProperties();  
     dialog.m_designerComponent = designer;  
     DialogResult result = dialog.ShowDialog();  
     if (result == DialogResult.OK)  
     {  
        designer.Invalidate();  
        designer.ChangeService().OnComponentChanged(designer, null, null, null);  
        return true;  
     }  
     else  
        return false;  
    }  
}  
```  
  
 Votre boîte de dialogue d'éditeur de propriétés personnalisées peut appeler l'Éditeur d'expressions du Concepteur de rapports. Dans l'exemple suivant, l'Éditeur d'expressions est appelé lorsque l'utilisateur sélectionne le premier élément dans la zone de liste déroulante :  
  
```csharp  
private void EditableCombo_SelectedIndexChanged(object sender,   
    EventArgs e)  
{  
   ComboBox combo = (ComboBox)sender;  
   if (combo.SelectedIndex == 0 && m_launchEditor)  
   {  
      m_launchEditor = false;  
      ExpressionEditor editor = new ExpressionEditor();  
      string newValue;  
      newValue = (string)editor.EditValue(null, m_designerComponent.Site, m_oldComboValue);  
      combo.Items[0] = newValue;  
   }  
}  
  
```  
  
### <a name="using-designer-verbs"></a>Utilisation des verbes de concepteur  
 Un verbe de concepteur est une commande de menu liée à un gestionnaire d'événements. Vous pouvez ajouter des verbes de concepteur qui apparaîtront dans le menu contextuel d'un composant lorsque votre contrôle au moment de l'exécution d'élément de rapport personnalisé est utilisé dans l'environnement de conception. Vous pouvez retourner la liste des verbes de concepteur disponibles de votre composant d’exécution à l’aide de la propriété **Verbs**.  
  
 L'exemple de code suivant illustre l'ajout d'un verbe de concepteur et d'un gestionnaire d'événements au <xref:System.ComponentModel.Design.DesignerVerbCollection>, ainsi que le code du gestionnaire d'événements :  
  
```csharp  
public override DesignerVerbCollection Verbs  
{  
    get  
    {  
        if (m_verbs == null)  
        {  
            m_verbs = new DesignerVerbCollection();  
            m_verbs.Add(new DesignerVerb("Proportional Scaling", new EventHandler(OnProportionalScaling)));  
         m_verbs[0].Checked = (GetCustomProperty("poly:Proportional") == bool.TrueString);  
        }  
  
        return m_verbs;  
    }  
}  
  
private void OnProportionalScaling(object sender, EventArgs e)  
{  
   bool proportional = !  
        (GetCustomProperty("poly:Proportional") == bool.TrueString);  
   m_verbs[0].Checked = proportional;  
   SetCustomProperty("poly:Proportional", proportional.ToString());  
   ChangeService().OnComponentChanged(this, null, null, null);  
   Invalidate();  
}  
```  
  
### <a name="using-adornments"></a>Utilisation d'ornements  
 Les classes d’éléments de rapports personnalisés peuvent également implémenter une classe **Microsoft.ReportDesigner.Design.Adornment**. Un ornement permet au contrôle d'élément de rapport personnalisé de fournir des zones à l'extérieur du rectangle principal de l'aire de conception. Ces zones permettent de gérer les événements de l'interface utilisateur, tels que les clics de souris et les opérations de glisser-déplacer. La classe **Adornment** définie dans l’espace de noms [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] **Microsoft.ReportDesigner** est une implémentation directe de la classe <xref:System.Windows.Forms.Design.Behavior.Adorner> dans Windows Forms. Pour obtenir la documentation complète de la classe **Adorner**, consultez [Vue d’ensemble du service de comportement](/previous-versions/ms171826(v=vs.140)) dans MSDN Library. Pour un exemple de code qui implémente une classe **Microsoft.ReportDesigner.Design.Adornment**, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
 Pour plus d'informations sur la programmation et l'utilisation de Windows Forms dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)], consultez les rubriques suivantes dans MSDN Library :  
  
-   Attributs au moment de la conception pour les composants  
  
-   Composants dans [!INCLUDE[vsprvs](../../includes/vsprvs-md.md)]  
  
-   Procédure pas à pas : Création d’un contrôle Windows Forms qui tire parti des fonctionnalités au moment de la conception de Visual Studio  
  
## <a name="see-also"></a>Voir aussi  
 [Architecture des éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [Création d’un composant d’exécution d’éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-run-time-component.md)   
 [Bibliothèques de classes d’éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [Procédure : déployer un élément de rapport personnalisé](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
