---
description: Désinstaller le Générateur de rapports
title: Désinstaller le Générateur de rapports | Microsoft Docs
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.topic: conceptual
ms.assetid: 009538c6-4941-4393-b14b-9144cffdbdaf
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 3f473c53a8e0a04603a391300fb2fc26dbd8210e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100077480"
---
# <a name="uninstall-report-builder"></a>Désinstaller le Générateur de rapports

Vous pouvez désinstaller la version autonome du Générateur de rapports à partir du Panneau de configuration ou de la ligne de commande.

La désinstallation du Générateur de rapports à partir de la ligne de commande utilise une syntaxe identique à celle que vous utilisez pour installer le Générateur de rapports, à la différence près que vous utilisez l'option /x à la place de l'option /i. Les lignes de commande de désinstallation peuvent également inclure l'option /quiet et d'autres options standard. Si le package Windows Installer du Générateur de rapports (ReportBuilder3_x86.msi) a été supprimé, la ligne de commande ne permet pas de désinstaller facilement le Générateur de rapports. Pour en savoir plus sur la suppression du Générateur de rapports à l’aide de son GUID, consultez la documentation du programme msiexec dans [Options de ligne de commande](/windows/desktop/Msi/command-line-options).  

Si les dossiers utilisés par le Générateur de rapports incluent des fichiers personnalisés, les dossiers et fichiers sont conservés lors de la suppression du Générateur de rapports. Seuls les fichiers du Générateur de rapports sont supprimés.  

### <a name="to-uninstall-report-builder-from-the-control-panel"></a>Pour désinstaller le Générateur de rapports à partir du panneau de configuration

1.  Dans le menu **Démarrer** , cliquez sur **Panneau de configuration**.  
  
2.  Dans le Panneau de configuration, cliquez sur **Programmes et fonctionnalités**.  
  
3.  Recherchez Générateur de rapports [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQL Server dans la liste **Nom**, puis cliquez dessus.  
  
4.  Cliquez sur **Désinstaller**.  
  
5.  Si vous êtes invité à confirmer la désinstallation du Générateur de rapports, cliquez sur **Oui**.  
  
### <a name="to-uninstall-report-builder-from-the-command-line"></a>Pour désinstaller le Générateur de rapports à partir de la ligne de commande  
  
1.  Dans le menu **Démarrer** , cliquez sur **Exécuter**.  
  
2.  Dans la zone de texte **Ouvrir** , tapez **cmd**.  
  
3.  Dans la fenêtre d'invite de commandes, naviguez jusqu'au dossier avec ReportBuilder3_x86.msi.  
  
4.  Tapez une ligne de commande de base, par exemple :  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v install.log`  
  
 Pour inclure l'enregistrement, utilisez une ligne de commande telle que :  
  
 `msiexec /x ReportBuilder3_x86.msi /quiet /l*v c:\junk\install.log`  
  
5.  Appuyez sur **Entrée**.  

## <a name="next-steps"></a>Étapes suivantes

[Installer le générateur de rapports](../../reporting-services/install-windows/install-report-builder.md)  

D’autres questions ? [Essayez de poser une question dans le forum Reporting Services](https://go.microsoft.com/fwlink/?LinkId=620231)
