---
title: Boîte de dialogue Erreurs de syntaxe SQL
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- vdt.dlgbox.sqlsyntaxerrorsencountered
- vdtsql.chm:69641
ms.assetid: bc9e5784-227e-4c5d-8084-24274fa6c14a
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.openlocfilehash: ead59dd19fc8de97c46bc2546c649f3779cfbd6a
ms.sourcegitcommit: f3321ed29d6d8725ba6378d207277a57cb5fe8c2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/06/2020
ms.locfileid: "86008181"
---
# <a name="sql-syntax-errors-encountered-dialog-box-visual-database-tools"></a>Erreurs de syntaxe SQL, boîte de dialogue (Visual Database Tools)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
Cette boîte de dialogue vous informe que le Concepteur ne peut pas analyser l'instruction SQL dans le volet SQL.  
  
Elle apparaît lorsque vous entrez ou modifiez une instruction SQL dans le volet SQL ; vous passez alors à un autre volet, vérifiez la requête ou tentez d'exécuter la requête, et l'une des conditions suivantes s'applique :  
  
-   L'instruction SQL est incomplète ou contient une ou plusieurs erreurs de syntaxe.  
  
-   L'instruction SQL est valide mais n'est pas prise en charge dans les volets graphiques (par exemple, une requête Union).  
  
-   L'instruction SQL est valide mais contient une syntaxe spécifique à la connexion de données que vous utilisez.  
  
> [!TIP]  
> Vous pouvez vérifier si une instruction est valide à l'aide du bouton **Vérifier la syntaxe SQL** situé dans la barre d'outils **Requête** .  
  
La boîte de dialogue affiche un message expliquant la raison pour laquelle l'instruction SQL ne peut pas être analysée. Cliquez sur **OK** pour continuer.  
  
## <a name="see-also"></a>Voir aussi  
[Rubriques de procédures relatives à la conception de requêtes et de vues &#40;Visual Database Tools&#41;](../../ssms/visual-db-tools/design-queries-and-views-how-to-topics-visual-database-tools.md)  
  
