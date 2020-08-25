---
title: Installer les packs d’administration SCOM
description: Procédez comme suit pour télécharger et installer les packs d’administration System Center Operations Manager (SCOM) pour SQL Server PDW. Les packs d’administration sont requis pour analyser les SQL Server PDW à partir de SCOM.
author: mzaman1
ms.prod: sql
ms.technology: data-warehouse
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: murshedz
ms.reviewer: martinle
ms.custom: seo-dt-2019
ms.openlocfilehash: 0db3a588dfabf290f2e095adafcd3331187af957
ms.sourcegitcommit: 7345e4f05d6c06e1bcd73747a4a47873b3f3251f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88766978"
---
# <a name="install-sql-server-operations-manager-scom-management-packs-for-analytics-platform-system"></a>Installer les packs d’administration SQL Server Operations Manager (SCOM) pour Analytics Platform System
Procédez comme suit pour télécharger et installer les packs d’administration System Center Operations Manager (SCOM) pour SQL Server PDW. Les packs d’administration sont requis pour analyser les SQL Server PDW à partir de SCOM.  
  
## <a name="before-you-begin"></a><a name="BeforeBegin"></a>Avant de commencer  
**Conditions préalables**  
  
System Center Operations Manager doit être installé et en cours d’exécution. SQL Server PDW 2012 requiert System Center Operations Manager 2007 R2, System Center Operations Manager 2012 ou System Center Operations Manager 2012 Service Pack 1.  
  
## <a name="step-1-download-the-management-packs"></a><a name="Step1"></a>Étape 1 : Télécharger les packs d’administration  
Pour la charge de travail PDW APS, téléchargez le [Pack d’administration System Center pour le Microsoft Analytics Platform System](https://go.microsoft.com/fwlink/?LinkId=396857).  
  
Pour la gestion de l’appliance, téléchargez le pack d’administration de base de l' [appliance SQL Server](/previous-versions/system-center/packs/gg602398(v=technet.10)).  
  
Pour les anciennes versions de PDW sans APS, téléchargez le[Pack d’analyse System Center pour l’Appliance Data Warehouse parallèle Microsoft SQL Server 2012](https://go.microsoft.com/fwlink/p/?LinkId=282661).  
  
<!-- MISSING LINKS - For the HDInsight workload, download the [System Center Management Pack for HDInsight](https://go.microsoft.com/fwlink/?LinkId=390208).  -->
  
## <a name="step-2-install-the-management-packs"></a><a name="Step2"></a>Étape 2 : installer les packs d’administration  
  
### <a name="install-the-sql-server-appliance-base-management-pack"></a>Installer le pack d’administration de base de l’appliance SQL Server  
  
1.  Pour exécuter l’installation, double-cliquez sur le pack d’administration de la base de SQL Server Appliance téléchargé.  
  
2.  Acceptez le contrat de licence, puis cliquez sur **suivant**.  
  
    ![Accepter le contrat de licence](./media/install-the-scom-management-packs/SCOM_licnse_agrmt.png "SCOM_licnse_agrmt")  
  
3.  Sélectionnez votre propre dossier d’installation ou utilisez le dossier d’installation du pack d’administration par défaut.  
  
    ![Sélectionner le dossier d'installation](./media/install-the-scom-management-packs/SCOM_licnse_agrmt2.png "SCOM_licnse_agrmt2")  
  
4.  Cliquez sur **Installer**.  
  
    ![Confirmez l'installation](./media/install-the-scom-management-packs/SCOM_licnse_agrmt3.png "SCOM_licnse_agrmt3")  
  
5.  Cliquez sur **Fermer**.  
  
    ![Cliquez sur Fermer](./media/install-the-scom-management-packs/SCOM_licnse_agrmt4.png "SCOM_licnse_agrmt4")  
  
### <a name="install-the-monitoring-pack-for-sql-server-pdw-appliance"></a>Installer le Pack de surveillance pour SQL Server PDW Appliance  
  
1.  Pour exécuter l’installation, double-cliquez sur le pack d’administration de SQL Server PDW Appliance téléchargé.  
  
2.  Acceptez le contrat de licence, puis cliquez sur **suivant**.  
  
    ![Acceptez le contrat de licence](./media/install-the-scom-management-packs/SCOM_licnse_agmtB.png "SCOM_licnse_agmtB")  
  
3.  Choisissez le répertoire qui contiendra les fichiers extraits. Le dossier d’installation par défaut du pack d’administration est affiché par défaut. Sélectionnez la valeur par défaut ou sélectionnez votre propre dossier d’installation.  
  
    ![Sélectionner le dossier d’installation](./media/install-the-scom-management-packs/SCOM_licnse_agmtB1.png "SCOM_licnse_agmtB1")  
  
4.  Cliquez sur **Installer**.  
  
    ![Confirmez l'installation](./media/install-the-scom-management-packs/SCOM_licnse_agmtB2.png "SCOM_licnse_agmtB2")  
  
5.  Cliquez sur **Fermer**.  
  
    ![Installation terminée](./media/install-the-scom-management-packs/SCOM_licnse_agmtB3.png "SCOM_licnse_agmtB3")  
  
## <a name="next-step"></a>étape suivante  
Maintenant que les packs d’administration sont installés, passez à l’étape suivante : [importez le pack d’administration SCOM pour PDW &#40;Analytics Platform System&#41;](import-the-scom-management-pack-for-pdw.md).  
  
<!-- MISSING LINKS ## See Also  
[Common Metadata Query Examples &#40;SQL Server PDW&#41;](../sqlpdw/common-metadata-query-examples-sql-server-pdw.md)  -->  
