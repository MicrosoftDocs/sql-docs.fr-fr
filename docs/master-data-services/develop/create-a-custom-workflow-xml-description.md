---
description: Créer un flux de travail personnalisé - Description du code XML
title: Description personnalisée XML de flux de travail
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: mds
ms.reviewer: ''
ms.technology: master-data-services
ms.topic: reference
ms.assetid: e267e5f4-38bb-466d-82e8-871eabeec07e
author: lrtoyou1223
ms.author: lle
ms.openlocfilehash: 416670f703ba40a54650a2dfc28b2f508c202439
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350140"
---
# <a name="create-a-custom-workflow---xml-description"></a>Créer un flux de travail personnalisé - Description du code XML

[!INCLUDE [SQL Server Windows Only - ASDBMI ](../../includes/applies-to-version/sql-windows-only-asdbmi.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)] , la méthode [Microsoft. MasterDataServices. WorkflowTypeExtender. IWorkflowTypeExtender. StartWorkflow *](/previous-versions/sql/sql-server-2016/hh759009(v=sql.130)) est appelée par SQL Server Service d’intégration de flux de travail MDS au démarrage d’un flux de travail. Cette méthode reçoit les métadonnées et les données relatives à l'élément qui a déclenché la règle d'entreprise de flux de travail sous la forme d'un bloc XML. Pour découvrir un exemple de code qui implémente un gestionnaire de flux de travail, consultez [Exemple de flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md).  
  
 L'exemple suivant montre à quoi ressemble le code XML envoyé au gestionnaire de flux de travail :  
  
```scr  
<ExternalAction>  
  <Type>TEST</Type>  
  <SendData>1</SendData>  
  <Server_URL>This is my test!</Server_URL>  
  <Action_ID>Test Workflow</Action_ID>  
  <Model_ID>5</Model_ID>  
  <Model_Name>Customer</Model_Name>  
  <Entity_ID>34</Entity_ID>  
  <Entity_Name>Customer</Entity_Name>  
  <Version_ID>8</Version_ID>  
  <MemberType_ID>1</MemberType_ID>  
  <Member_ID>12</Member_ID>  
  <MemberData>  
    <ID>12</ID>  
    <Version_ID>8</Version_ID>  
    <ValidationStatus_ID>3</ValidationStatus_ID>  
    <ChangeTrackingMask>0</ChangeTrackingMask>  
    <EnterDTM>2011-02-25T20:16:36.650</EnterDTM>  
    <EnterUserID>2</EnterUserID>  
    <EnterUserName>MyUserName</EnterUserName>  
    <EnterUserMuid>EEF91D48-B673-4D83-B95F-5A363C11DE91</EnterUserMuid>  
    <EnterVersionId>8</EnterVersionId>  
    <EnterVersionName>VERSION_1</EnterVersionName>  
    <EnterVersionMuid>52B788C2-2750-4651-9DB0-2CB05A88AA5A</EnterVersionMuid>  
    <LastChgDTM>2011-02-25T20:16:36.650</LastChgDTM>  
    <LastChgUserID>2</LastChgUserID>  
    <LastChgUserName>MyUserName</LastChgUserName>  
    <LastChgUserMuid>EEF91D48-B673-4D83-B95F-5A363C11DE91</LastChgUserMuid>  
    <LastChgVersionId>8</LastChgVersionId>  
    <LastChgVersionName>VERSION_1</LastChgVersionName>  
    <LastChgVersionMuid>52B788C2-2750-4651-9DB0-2CB05A88AA5A</LastChgVersionMuid>  
    <Name>Test Customer</Name>  
    <Code>TC</Code>  
  </MemberData>  
</ExternalAction>  
```  
  
 Le tableau suivant décrit quelques-unes des balises contenues dans ce code XML :  
  
|Tag|Description|  
|---------|-----------------|  
|\<Type>|Texte que vous avez entré dans la zone de texte **Type de flux de travail** dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)] pour identifier quels assemblys personnalisés de flux de travail doivent être chargés.|  
|\<SendData>|Valeur booléenne contrôlée par la case à cocher **Inclure les données de membre dans le message** dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)]. La valeur 1 signifie que la \<MemberData> section est envoyée ; sinon, la \<MemberData> section n’est pas envoyée.|  
|<Server_URL>|Texte que vous avez entré dans la zone de texte **Site du flux de travail** dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
|<Action_ID>|Texte que vous avez entré dans la zone de texte **Nom du flux de travail** dans [!INCLUDE[ssMDSmdm](../../includes/ssmdsmdm-md.md)].|  
|\<MemberData>|Contient les données du membre qui a déclenché l'action de flux de travail. Cela est inclus uniquement si la valeur de \<SendData> est 1.|  
|\<Enter*xxx*>|Cet ensemble de balises contient des métadonnées sur la création du membre, comme sa date de création et la personne qui l'a créé.|  
|\<LastChg*xxx*>|Cet ensemble de balises contient des métadonnées sur la dernière modification apportée au membre, comme la date de la modification et la personne qui l'a effectuée.|  
|\<Name>|Premier attribut du membre qui a été modifié. Cet exemple de membre contient uniquement des attributs Name et Code.|  
|\<Code>|Attribut suivant du membre qui a été modifié. Si cet exemple de membre contenait plus d'attributs, ils suivraient celui-ci.|  
  
## <a name="see-also"></a>Voir aussi  
 [Créer un &#40;de flux de travail personnalisé Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-master-data-services.md)   
 [Exemple de flux de travail personnalisé &#40;Master Data Services&#41;](../../master-data-services/develop/create-a-custom-workflow-example.md)  
  
  
