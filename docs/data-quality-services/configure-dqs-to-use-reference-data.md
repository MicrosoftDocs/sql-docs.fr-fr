---
description: Configurer DQS pour utiliser des données de référence
title: Configurer DQS pour utiliser des données de référence
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: data-quality-services
ms.reviewer: ''
ms.technology: data-quality-services
ms.topic: conceptual
f1_keywords:
- sql13.dqs.administration.rdsconfiguration.f1
- sql13.dqs.administration.configuration.createDirectRDS.f1
- sql13.dqs.admin.config.rds.f1
ms.assetid: fae745e7-57a7-4cbc-8979-56ea8e392e4e
author: swinarko
ms.author: sawinark
ms.openlocfilehash: 0ea7b94d092565827bbd0086f7f4d122ce219fac
ms.sourcegitcommit: c7f40918dc3ecdb0ed2ef5c237a3996cb4cd268d
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91725415"
---
# <a name="configure-dqs-to-use-reference-data"></a>Configurer DQS pour utiliser des données de référence

[!INCLUDE [SQL Server - Windows only ASDBMI  ](../includes/applies-to-version/sqlserver.md)]

  Cette rubrique explique comment configurer [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) afin d'utiliser des données de référence pour le nettoyage de vos données. Vous pouvez utiliser des données de référence à partir d’Azure Marketplace ou de fournisseurs de données de référence tiers en ligne directs.  

> [!IMPORTANT]
> Cet article mentionne des services de données de référence tiers qui étaient disponibles dans Azure DataMarket. DataMarket et Data Services, notamment les données d’adresse Melissa par exemple, ont été supprimés après le 31/12/2016. Par conséquent, vous ne pouvez plus exécuter les exemples de cet article avec les services spécifiés de DataMarket. Vous pouvez quand même utiliser les services de données de référence directement disponibles en ligne des fournisseurs de données de référence tiers.

## <a name="before-you-begin"></a>Avant de commencer  
  
###  <a name="prerequisites"></a><a name="Prerequisites"></a> Conditions préalables  
 Pour utiliser des données de référence de Marketplace, vous devez disposer d'une clé de compte Marketplace valide. Pour plus d’informations sur la création d’une clé de compte Marketplace, consultez [créer votre compte](/previous-versions/azure/ff717655(v=azure.100)) ( https://go.microsoft.com/fwlink/?LinkId=212936) . Vous pouvez également créer une clé de compte de la Place de marché à partir de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)]. Pour ce faire, cliquez sur **Configuration** sous **Administration** dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)], puis cliquez sur **Créer un ID de compte DataMarket** sous l'onglet **Données de référence**.  
  
###  <a name="security"></a><a name="Security"></a> Sécurité  
  
####  <a name="permissions"></a><a name="Permissions"></a> Autorisations  
 Vous devez disposer du rôle dqs_administrator sur la base de données DQS_MAIN pour configurer les paramètres du service de données de référence dans DQS.  
  
##  <a name="configure-dqs-to-use-reference-data-from-marketplace"></a><a name="Marketplace"></a> Configurer DQS pour utiliser des données de référence de Marketplace  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Exécutez l’Application Data Quality client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , sous **Administration**, cliquez sur **Configuration**.  
  
3.  Sous l'onglet **Données de référence** , sous la zone **Paramètres réseau** , tapez les valeurs appropriées dans les zones **Serveur proxy** et **Port** si vous ou votre organisation utilisez un serveur proxy pour vous connecter à Internet.  
  
4.  Spécifiez la clé de compte de la Place de marché dans la zone **ID de compte DataMarket**, puis cliquez sur l'icône **Valider l'ID de compte DataMarket** pour valider la clé de compte. Un message indiquant si la clé de compte Marketplace spécifiée est valide s'affiche.  
  
 Vous êtes maintenant prêt à utiliser, dans DQS, les services de données de référence Marketplace faisant l'objet d'un abonnement pour la clé de compte Marketplace spécifiée.  
  
##  <a name="configure-dqs-to-use-reference-data-from-direct-online-third-party-reference-data-providers"></a><a name="ThirdParty"></a> Configurer DQS pour utiliser des données de référence provenant de fournisseurs de données de référence tiers en ligne directs  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)][Exécutez l’Application Data Quality client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Dans l'écran d'accueil de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , sous **Administration**, cliquez sur **Configuration**.  
  
3.  Sous l'onglet **Données de référence** , sous la zone **Paramètres réseau** , tapez les valeurs appropriées dans les zones **Serveur proxy** et **Port** si vous ou votre organisation utilisez un serveur proxy pour vous connecter à Internet.  
  
4.  Dans la zone **Paramètres du service de données de référence tiers en ligne direct** , cliquez sur l'icône **Ajouter un nouveau fournisseur de services de données de référence** .  
  
5.  Dans la boîte de dialogue **Créer un fournisseur de services de données de référence tiers en ligne direct** , spécifiez les détails suivants :  
  
    1.  Dans la zone **Nom** , tapez le nom du nouveau fournisseur de services de données de référence direct.  
  
    2.  (Facultatif) Dans la zone **Description** , tapez la description du nouveau fournisseur de services de données de référence direct.  
  
    3.  Dans la zone **Catégorie** , tapez la catégorie des données fournies par le nouveau fournisseur de services de données de référence direct.  
  
    4.  Dans la zone Schéma, spécifiez le schéma qui définit la chaîne des champs (noms de colonnes) à utiliser à partir du fournisseur de services de données de référence direct. Un nom de champ ne doit pas contenir d'espace, et les champs doivent être séparés par des virgules. Par exemple : `FirstName, LastName, City, State`.  
  
    5.  Dans la zone **URI** , tapez l'URI du fournisseur de services de données de référence direct. Seuls les URI sécurisés (adresse commençant par « https:// ») sont autorisés dans DQS.  
  
    6.  Dans la zone **Taille de lot maximale** , tapez le nombre maximal d'enregistrements par lot qui seront envoyés au fournisseur de services de données de référence pour être nettoyés. Il est possible de spécifier au maximum 100 enregistrements par lot pour l'activité de nettoyage.  
  
    7.  Dans la zone **ID de compte** , tapez l'ID de compte de l'abonné avec le fournisseur de services de données de référence.  
  
6.  Cliquez sur **OK** pour enregistrer les données, puis fermez la boîte de dialogue **Créer un fournisseur de services de données de référence tiers en ligne direct** . Le fournisseur de données de référence tiers en ligne direct que vous venez d'ajouter devient disponible dans la grille des **Fournisseurs de services de données de référence** dans DQS.  
  
 Vous êtes maintenant prêt à utiliser, dans DQS, les services de données de référence à partir du fournisseur de données de référence tiers en ligne direct que vous venez de configurer.  
  
##  <a name="follow-up-after-configuring-dqs-to-use-reference-data"></a><a name="FollowUp"></a> Suivi : après avoir configuré DQS pour utiliser des données de référence  
 Vous devez maintenant mapper les domaines de base de connaissances requis aux données de référence disponibles auprès des fournisseurs de données que vous venez de configurer. Pour ce faire, consultez [Attacher un domaine ou un domaine composite à des données de référence](../data-quality-services/attach-domain-or-composite-domain-to-reference-data.md).  
  
