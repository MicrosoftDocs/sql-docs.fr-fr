---
description: Utilisation du pilote ODBC Visual FoxPro avec votre application Visual Basic
title: Utiliser le pilote ODBC de VFP FoxPro avec votre application Visual Basic | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- Visual FoxPro ODBC driver [ODBC], visual basic applications
- Visual Basic applications [ODBC]
- FoxPro ODBC driver [ODBC], visual basic applications
- Visual FoxPro data [ODBC], visual basic applications
ms.assetid: 5223ca23-5df6-4ebc-aa3b-70682ff27a8c
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 4bf121725c30f5f01ef4314c21f1dffbf34fcce8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100070394"
---
# <a name="using-the-vfp-foxpro-odbc-driver-with-your-visual-basic-application"></a>Utilisation du pilote ODBC Visual FoxPro avec votre application Visual Basic
Votre application Microsoft® Visual Basic® peut communiquer avec les données Visual FoxPro en créant un contrôle de données qui se connecte à une source de données Visual FoxPro.  
  
#### <a name="to-connect-to-visual-foxpro-data-using-the-data-control-in-visual-basic"></a>Pour vous connecter à des données Visual FoxPro à l’aide du contrôle de données dans Visual Basic  
  
1.  Créez une source de données nommée « test » qui se connecte à l’exemple de base de données TasTrade inclus dans Visual FoxPro. L’installation de Visual FoxPro par défaut place l’exemple de base de données TasTrade à l’emplacement suivant :  
  
    ```  
    c:\vfp\samples\mainsamp\data\tastrade.dbc  
    ```  
  
2.  Dans Visual Basic, créez un nouveau formulaire et placez-y une zone de texte et un contrôle de données.  
  
3.  Modifiez la propriété Connect du contrôle de données comme suit :  
  
    ```  
    ODBC;DATABASE=tastrade;DSN=test  
    ```  
  
4.  Modifiez la propriété TypeRecordset comme suit :  
  
    ```  
    2 - Snapshot  
    ```  
  
5.  Modifiez la propriété RecordSource comme suit :  
  
    ```  
    customer  
    ```  
  
6.  Remplacez la propriété DataSource de la zone de texte par le nom par défaut du contrôle de données par ce qui suit :  
  
    ```  
    data1  
    ```  
  
7.  Modifiez la propriété DataField de la zone de texte comme suit :  
  
    ```  
    customer_id  
    ```  
  
8.  Exécutez le formulaire et utilisez le contrôle Data pour ignorer les champs Customer ID de l’exemple de base de données TasTrade Visual FoxPro.
