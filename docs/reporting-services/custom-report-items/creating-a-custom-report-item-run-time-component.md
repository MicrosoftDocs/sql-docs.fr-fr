---
title: Création d’un composant d’exécution d’éléments de rapport personnalisé | Microsoft Docs
description: Découvrez comment créer un composant run-time d’élément de rapport personnalisé et comment définir les propriétés de ce composant dans l’environnement de conception.
ms.date: 03/14/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: custom-report-items
ms.topic: reference
helpviewer_keywords:
- custom report items, creating
ms.assetid: b3e15a4a-98f8-4dbb-b847-bbcb20327051
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 1c6d70bba220624cd43ca1ced80cbe89ba936460
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100061944"
---
# <a name="creating-a-custom-report-item-run-time-component"></a>Création d'un composant d'exécution d'élément de rapport personnalisé
  Le composant d’exécution d’élément de rapport personnalisé est implémenté en tant que composant [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[dnprdnshort](../../includes/dnprdnshort-md.md)] à l’aide de tout langage Conforme CLS et est appelé par le processeur de rapports au moment de l’exécution. Les propriétés du composant d'exécution sont définies dans l'environnement de conception en modifiant le composant de conception d'élément de rapport personnalisé y correspondant.  
  
 Pour un exemple d’élément de rapport personnalisé totalement implémenté, consultez [SQL Server Reporting Services Product Samples](https://go.microsoft.com/fwlink/?LinkId=177889) (Exemples Reporting Services pour le produit SQL Server).  
  
## <a name="definition-and-instance-objects"></a>Objets d'instance et de définition  
 Avant d’implémenter un élément de rapport personnalisé, il est important de comprendre la différence entre *objets de définition* et *objets d’instance*. Les objets de définition fournissent la représentation RDL de l'élément de rapport personnalisé alors que les objets d'instance sont les versions évaluées des objets de définition. Il n'existe qu'un seul objet de définition pour chaque élément du rapport. Lorsque vous accédez aux propriétés d'un objet de définition qui contient des expressions, vous obtenez une chaîne d'expression non-évaluée. Les objets d'instance contiennent les versions évaluées des objets de définition et peuvent avoir une relation un-à-plusieurs avec l'objet de définition d'un élément. Par exemple, lorsque la région de données <xref:Microsoft.ReportingServices.OnDemandReportRendering.Tablix> d'un rapport contient un composant <xref:Microsoft.ReportingServices.OnDemandReportRendering.CustomReportItem> dans l'une de ses lignes de détails, un seul objet de définition y correspond. En revanche, plusieurs objets d'instance sont spécifiés, un pour chaque ligne que comporte la région de données.  
  
## <a name="implementing-the-icustomreportitem-interface"></a>Implémentation de l'interface ICustomReportItem  
 Pour créer un composant d’exécution **CustomReportItem**, vous devez implémenter l’interface <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem> définie dans le fichier Microsoft.ReportingServices.ProcessingCore.dll :  
  
```csharp  
namespace Microsoft.ReportingServices.OnDemandReportRendering  
{  
    public interface ICustomReportItem  
    {  
        void GenerateReportItemDefinition(CustomReportItem customReportItem);  
void EvaluateReportItemInstance(CustomReportItem customReportItem);  
    }  
}  
```  
  
 Une fois l'interface <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem> implémentée, deux stubs de méthode sont générés : <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A> et <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A>. La méthode <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.GenerateReportItemDefinition%2A> est appelée en premier et est utilisée pour définir des propriétés de définition et créer l'objet <xref:Microsoft.ReportingServices.OnDemandReportRendering.Image> qui contiendra à la fois les propriétés de définition et d'instance utilisées pour le rendu de l'élément. La méthode <xref:Microsoft.ReportingServices.OnDemandReportRendering.ICustomReportItem.EvaluateReportItemInstance%2A> est appelée une fois les objets de définition évalués. Elle fournit en outre les objets d'instance utilisés pour le rendu de l'élément.  
  
 L'exemple suivant illustre l'implémentation d'un élément de rapport personnalisé qui restitue le nom du contrôle sous la forme d'une image.  
  
```csharp  
namespace Microsoft.Samples.ReportingServices  
{  
    using System;  
    using System.Collections.Generic;  
    using System.Collections.Specialized;  
    using System.Drawing.Imaging;  
    using System.IO;  
    using System.Text;  
    using Microsoft.ReportingServices.OnDemandReportRendering;  
  
    public class PolygonsCustomReportItem : ICustomReportItem  
    {  
        #region ICustomReportItem Members  
  
        public void GenerateReportItemDefinition(CustomReportItem cri)  
        {  
            // Create the Image object that will be   
            // used to render the custom report item  
            cri.CreateCriImageDefinition();  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
        }  
  
        public void EvaluateReportItemInstance(CustomReportItem cri)  
        {  
            // Get the Image definition  
            Image polygonImage = (Image)cri.GeneratedReportItem;  
  
            // Create the image for the custom report item  
            polygonImage.ImageInstance.ImageData = DrawImage(cri);  
        }  
  
        #endregion  
  
        /// <summary>  
        /// Creates an image of the CustomReportItem's name  
        /// </summary>  
        private byte[] DrawImage(CustomReportItem customReportItem)  
        {  
            int width = 1;          // pixels  
            int height = 1;         // pixels  
            int resolution = 75;    // dpi  
  
            System.Drawing.Bitmap bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            System.Drawing.Graphics graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Get the Font for the Text  
            System.Drawing.Font font = new System.Drawing.Font(System.Drawing.FontFamily.GenericMonospace,  
                12, System.Drawing.FontStyle.Regular);  
  
            // Get the Brush for drawing the Text  
            System.Drawing.Brush brush = new System.Drawing.SolidBrush(System.Drawing.Color.LightGreen);  
  
            // Get the measurements for the image  
            System.Drawing.SizeF maxStringSize = graphics.MeasureString(customReportItem.Name, font);  
            width = (int)(maxStringSize.Width + 2 * font.GetHeight(resolution));  
            height = (int)(maxStringSize.Height + 2 * font.GetHeight(resolution));  
  
            bitmap.Dispose();  
            bitmap = new System.Drawing.Bitmap(width, height);  
            bitmap.SetResolution(resolution, resolution);  
  
            graphics.Dispose();  
            graphics = System.Drawing.Graphics.FromImage(bitmap);  
            graphics.PageUnit = System.Drawing.GraphicsUnit.Pixel;  
  
            // Draw the text  
            graphics.DrawString(customReportItem.Name, font, brush, font.GetHeight(resolution),   
                font.GetHeight(resolution));  
  
            // Create the byte array of the image data  
            MemoryStream memoryStream = new MemoryStream();  
            bitmap.Save(memoryStream, ImageFormat.Bmp);  
            memoryStream.Position = 0;  
            byte[] imageData = new byte[memoryStream.Length];  
            memoryStream.Read(imageData, 0, imageData.Length);  
  
            return imageData;  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Architecture des éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-architecture.md)   
 [Création d’un composant au moment de la conception d’éléments de rapport personnalisé](../../reporting-services/custom-report-items/creating-a-custom-report-item-design-time-component.md)   
 [Bibliothèques de classes d’éléments de rapports personnalisés](../../reporting-services/custom-report-items/custom-report-item-class-libraries.md)   
 [Procédure : déployer un élément de rapport personnalisé](../../reporting-services/custom-report-items/how-to-deploy-a-custom-report-item.md)  
  
  
