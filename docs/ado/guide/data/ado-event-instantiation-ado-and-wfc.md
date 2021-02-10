---
description: Instanciation des événements ADO ADO et WFC
title: 'Instanciation des événements ADO : ADO et WFC | Microsoft Docs'
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 02/15/2017
ms.reviewer: ''
ms.topic: conceptual
ms.assetid: 9ee4be21-657b-407a-afa4-0b27a6b096ce
author: rothja
ms.author: jroth
ms.openlocfilehash: b7ed585adcfc9542e3d396f2b0fd1a82d65cdcf4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028351"
---
# <a name="ado-event-instantiation-ado-and-wfc"></a>Instanciation des événements ADO ADO et WFC
ADO pour Windows Foundation classes (ADO/WFC) s’appuie sur le modèle d’événement ADO et présente une interface de programmation d’applications simplifiée. En général, ADO/WFC intercepte les événements ADO, consolide les paramètres d’événement dans une classe d’événements unique, puis appelle votre gestionnaire d’événements.  
  
### <a name="to-use-ado-events-in-adowfc"></a>Pour utiliser des événements ADO dans ADO/WFC  
  
1.  Définissez votre propre gestionnaire d’événements pour traiter un événement. Par exemple, si vous souhaitez traiter l’événement **ConnectComplete** dans la famille **ConnectionEvent** , vous pouvez utiliser ce code :  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    {  
        System.out.println("onConnectComplete:" + e);  
    }  
    ```  
  
2.  Définissez un objet gestionnaire pour représenter votre gestionnaire d’événements. L’objet de gestionnaire doit être de type de données **ConnectEventHandler** pour un événement de type **ConnectionEvent**, ou de type de données **RecordsetEventHandler** pour un événement de type **RecordsetEvent**. Par exemple, codez ce qui suit pour votre gestionnaire d’événements **ConnectComplete** :  
  
    ```  
    ConnectionEventHandler handler =   
        new ConnectionEventHandler(this, "onConnectComplete");  
    ```  
  
     Le premier argument du constructeur **ConnectionEventHandler** est une référence à la classe qui contient le gestionnaire d’événements nommé dans le deuxième argument.  
  
3.  Ajoutez votre gestionnaire d’événements à une liste de gestionnaires désignés pour traiter un type particulier d’événement. Utilisez la méthode avec un nom tel que **addon**_EventName_(*handler*).  
  
4.  ADO/WFC implémente en interne tous les gestionnaires d’événements ADO. Par conséquent, un événement provoqué par une opération de **connexion** ou **d’ensemble d’enregistrements** est intercepté par un gestionnaire d’événements ADO/WFC.  
  
     Le gestionnaire d’événements ADO/WFC transmet les paramètres **CONNECTIONEVENT** ADO dans une instance de la classe **CONNECTIONEVENT** ADO/WFC, ou des paramètres ADO **RecordsetEvent** dans une instance de la classe **RecordsetEvent** ADO/WFC. Ces classes ADO/WFC consolident les paramètres d’événement ADO ; autrement dit, chaque classe ADO/WFC contient un membre de données pour chaque paramètre unique dans toutes les méthodes **ConnectionEvent** ou **RecordsetEvent** ADO.  
  
5.  ADO/WFC appelle ensuite votre gestionnaire d’événements avec l’objet d’événement ADO/WFC. Par exemple, votre gestionnaire **onConnectComplete** a une signature similaire à celle-ci :  
  
    ```  
    public void onConnectComplete(Object sender,ConnectionEvent e)  
    ```  
  
     Le premier argument est le type d’objet qui a envoyé l’événement ([connexion](../../reference/ado-api/connection-object-ado.md) ou [Recordset](../../reference/ado-api/recordset-object-ado.md)), et le deuxième argument est l’objet d’événement ADO/WFC (**ConnectionEvent** ou **RecordsetEvent**).  
  
     La signature de votre gestionnaire d’événements est plus simple qu’un événement ADO. Toutefois, vous devez toujours comprendre le modèle d’événement ADO pour savoir quels paramètres s’appliquent à un événement et comment répondre.  
  
6.  Retournez de votre gestionnaire d’événements au gestionnaire ADO/WFC pour l’événement ADO. ADO/WFC copie les membres de données d’événement ADO/WFC pertinents vers les paramètres d’événement ADO, puis le gestionnaire d’événements ADO retourne.  
  
7.  Une fois le traitement terminé, supprimez votre gestionnaire de la liste des gestionnaires d’événements ADO/WFC. Utilisez la méthode avec un nom tel que **RemoveAt**_EventName_(*handler*).  
  
## <a name="see-also"></a>Voir aussi  
 [Résumé du gestionnaire d’événements ADO](./ado-event-handler-summary.md)   
 [Index de la syntaxe ADO-WFC](../../reference/ado-api/ado-wfc-syntax-index.md)   
 [Paramètres d’événement](./event-parameters.md)   
 [Fonctionnement conjoint des gestionnaires d’événements](./how-event-handlers-work-together.md)   
 [Types d’événements](./types-of-events.md)