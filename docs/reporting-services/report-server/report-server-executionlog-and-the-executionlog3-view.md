---
title: Journal d’exécution du serveur de rapports et vue ExecutionLog3 | Microsoft Docs
description: Apprenez-en davantage sur le journal d’exécution du serveur de rapports dans Reporting Services, qui contient des informations sur les rapports sur les serveurs en mode natif ou dans une batterie de serveurs SharePoint.
ms.date: 03/01/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- logs [Reporting Services], execution
- execution logs [Reporting Services]
ms.assetid: a7ead67d-1404-4e67-97e7-4c7b0d942070
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 28d083e053e31c1fcce26e233ba22211e211a993
ms.sourcegitcommit: 1f826eb3f73bd4d94bc9638b9cdd60991a2e2fa0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/12/2021
ms.locfileid: "98125584"
---
# <a name="report-server-executionlog-and-the-executionlog3-view"></a>Journal des exécutions du serveur de rapports et vue ExecutionLog3
  Le journal des exécutions du serveur de rapports [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)]contient des informations sur les rapports qui sont exécutés sur le serveur ou sur plusieurs serveurs dans le cadre d’un déploiement évolutif en mode natif ou sur une batterie de serveurs SharePoint. Vous pouvez l'utiliser pour connaître la fréquence de demande d'un rapport, les formats de sortie les plus utilisés et le nombre de millisecondes de traitement consacré à chaque phrase du traitement. Le journal contient des informations sur le temps passé pour l'exécution d'une requête de dataset dans un rapport et le temps passé pour le traitement des données. Si vous êtes administrateur de serveur de rapports, vous pouvez passer en revue les informations du journal, identifier les tâches longues et faire des suggestions aux auteurs de rapports pour améliorer des zones du rapport (dataset ou traitement).  
  
 Les serveurs de rapports configurés pour le mode SharePoint peuvent également utiliser les journaux ULS de SharePoint. Pour plus d’informations, consultez [Activer des événements Reporting Services pour le journal des traces SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)  
  
##  <a name="viewing-log-information"></a><a name="bkmk_top"></a> Affichage des informations des journaux  
 Le serveur de rapports consigne les données sur l'exécution des rapports dans une table de base de données interne. Les informations de la table sont fournies par les vues de SQL Server.  
  
 Le journal d'exécution des rapports est stocké dans la base de données du serveur de rapports nommée par défaut **ReportServer**. Les vues SQL fournissent les informations associées au journal d'exécution. Les vues « 2 » et « 3 » ont été ajoutées dans les dernières versions et contiennent de nouveaux champs ou des champs avec des noms plus conviviaux que dans les versions précédentes. Les anciennes vues sont toujours présentes dans le produit de sorte que les applications personnalisées qui dépendent d'elles ne sont pas impactées. Si vous n'avez pas de dépendance sur une vue plus ancienne, par exemple ExecutionLog, il est recommandé d'utiliser la vue la plus récente, soit ExecutionLog **3**.  
  
 Dans cette rubrique :  
  
-   [Paramètres de configuration d'un serveur de rapports en mode SharePoint](#bkmk_sharepoint)  
  
-   [Paramètres de configuration d'un serveur de rapports en mode natif](#bkmk_native)  
  
-   [Champs de journalisation (ExecutionLog3)](#bkmk_executionlog3)  
  
-   [Champ AdditionalInfo](#bkmk_additionalinfo)  
  
-   [Champs de journalisation (ExecutionLog2)](#bkmk_executionlog2)  
  
-   [Champs de journalisation (ExecutionLog)](#bkmk_executionlog)  
  
##  <a name="configuration-settings-for-a-sharepoint-mode-report-server"></a><a name="bkmk_sharepoint"></a> Paramètres de configuration d'un serveur de rapports en mode SharePoint  
 Vous pouvez activer ou désactiver la journalisation de l'exécution des rapports dans les paramètres système d'une application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] .  
  
 Par défaut, les entrées de journal sont conservées pendant 60 jours. Au-delà de cette date, les entrées sont supprimées à 02:00 tous les jours. Dans une installation déjà rodée, seuls 60 jours d'informations sont disponibles à tout moment.  
  
 Vous ne pouvez pas définir de limites pour le nombre de lignes ou le type d'entrées enregistrées.  
  
 **Pour activer la journalisation des exécutions :**  
  
1.  Depuis l'Administration centrale de SharePoint, dans le groupe **Gestion des applications** , cliquez sur **Gérer les applications de service** .  
  
2.  Cliquez sur le nom de l'application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] à configurer.  
  
3.  Cliquez sur **Paramètres système**.  
  
4.  Sélectionnez **Activer la journalisation des exécutions** dans la section **Enregistrement** .  
  
5.  Cliquez sur **OK**.  
  
 **Pour activer la journalisation verbose :**  
  
 Vous devez activer la journalisation comme décrit dans les étapes précédentes, puis effectuer les opérations suivantes :  
  
1.  Dans la page **Paramètres système** de votre application de service [!INCLUDE[ssRSnoversion](../../includes/ssrsnoversion-md.md)] , recherchez la section **Défini par l’utilisateur** .  
  
2.  Modifiez **ExecutionLogLevel** en **verbose**. Ce champ est un champ d'entrée de texte et les deux valeurs possibles sont **verbose** et **normal**.  
  
##  <a name="configuration-settings-for-a-native-mode-report-server"></a><a name="bkmk_native"></a> Paramètres de configuration d'un serveur de rapports en mode natif  
 Vous pouvez activer ou désactiver la journalisation de l'exécution de rapports dans la page Propriétés du serveur de SQL Server Management Studio. **EnableExecutionLogging** est une propriété avancée.  
  
 Par défaut, les entrées de journal sont conservées pendant 60 jours. Au-delà de cette date, les entrées sont supprimées à 02:00 tous les jours. Dans une installation déjà rodée, seuls 60 jours d'informations sont disponibles à tout moment.  
  
 Vous ne pouvez pas définir de limites pour le nombre de lignes ou le type d'entrées enregistrées.  
  
 **Pour activer la journalisation des exécutions :**  
  
1.  Ouvrez SQL Server Management Studio avec des privilèges d'administrateur. Par exemple, cliquez avec le bouton droit sur l’icône Management Studio et cliquez sur « Exécuter en tant qu’administrateur ».  
  
2.  Connectez-vous au serveur de rapports souhaité.  
  
3.  Cliquez avec le bouton droit sur le nom du serveur, puis cliquez sur **Propriétés**. Si l'option Propriétés est désactivée, vérifiez que vous avez ouvert SQL Server Management Studio avec des privilèges d'administrateur.  
  
4.  Cliquez sur la page **Journalisation** .  
  
5.  Sélectionnez **Activer la journalisation de l'exécution des rapports**.  
  
 **Pour activer la journalisation verbose :**  
  
 Vous devez activer la journalisation comme décrit dans les étapes précédentes, puis effectuer les opérations suivantes :  
  
1.  Dans la boîte de dialogue **Propriétés du serveur** , cliquez sur la page **Avancé** .  
  
2.  Dans la section **Défini par l’utilisateur** , modifiez **ExecutionLogLevel** sur **verbose**. Ce champ est un champ d'entrée de texte et les deux valeurs possibles sont **verbose** et **normal**.  
  
##  <a name="log-fields-executionlog3"></a><a name="bkmk_executionlog3"></a> Champs de journalisation (ExecutionLog3)  
 Dans cette vue, un nœud de diagnostic de performances supplémentaire est ajouté dans la colonne **AdditionalInfo** basée sur XML. La colonne AdditionalInfo contient une structure XML comportant de 1 à plusieurs champs d'informations supplémentaires. Voici un exemple d'instruction Transact SQL pour extraire des lignes de la vue ExecutionLog3. L'exemple suppose que la base de données du serveur de rapports est nommée **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog3 order by TimeStart DESC  
```  
  
 La table suivante décrit les données qui sont capturées dans le journal d'exécution des rapports :  
  
|Colonne|Description|  
|------------|-----------------|  
|InstanceName|Nom de l'instance du serveur de rapports qui a géré la demande. Si votre environnement inclut plusieurs serveurs de rapports, vous pouvez analyser la distribution InstanceName pour surveiller et déterminer si votre programme d'équilibrage de la charge réseau distribue les requêtes entre les différents serveurs de rapports comme prévu.|  
|ItemPath|Chemin de stockage d'un rapport ou d'un élément de rapport|  
|UserName|Identificateur de l'utilisateur.|  
|ExecutionID|Identificateur interne associé à une requête. Les requêtes sur les mêmes sessions utilisateur partagent le même ID d'exécution.|  
|RequestType|Valeurs possibles :<br /><br /> Interactive<br /><br /> Abonnement<br /><br /> <br /><br /> L'analyse des données de journal filtrées par RequestType=Subscription et triées par TimeStart peut identifier des périodes d'utilisation importante des abonnements ; si vous le souhaitez, il est par la suite possible de modifier l'heure de certains abonnements aux rapports.|  
|Format|Format de rendu.|  
|Paramètres|Valeurs des paramètres utilisées pour une exécution de rapport.|  
|ItemAction|Valeurs possibles :<br /><br /> Rendu<br /><br /> Trier<br /><br /> BookMarkNavigation<br /><br /> DocumentNavigation<br /><br /> GetDocumentMap<br /><br /> Findstring<br /><br /> Execute<br /><br /> RenderEdit|  
|TimeStart|Heures de début et de fin qui indiquent la durée d'un traitement de rapport.|  
|TimeEnd||  
|TimeDataRetrieval|Nombre de millisecondes passées pour la récupération des données.|  
|TimeProcessing|Nombre de millisecondes passées pour le traitement du rapport.|  
|TimeRendering|Nombre de millisecondes passées pour le rendu du rapport.|  
|Source|Source d'exécution du rapport. Valeurs possibles :<br /><br /> En direct<br /><br /> Cache : indique une exécution mise en cache ; par exemple, les requêtes de jeu de données ne sont pas exécutées en direct.<br /><br /> Instantané<br /><br /> Historique<br /><br /> AdHoc : indique soit un rapport d’extraction basé sur un modèle généré dynamiquement, soit un rapport du Générateur de rapports affiché en aperçu dans un client utilisant le serveur de rapports pour le traitement et le rendu.<br /><br /> Session : indique une requête de suivi dans une session déjà établie.  Par exemple, la requête initiale concerne l'affichage de la page 1 et la requête de suivi concerne l'exportation vers Excel avec l'état de la session active.<br /><br /> Rdce :  indique une extension RDCE (Report Definition Customization Extension). Une extension RDCE personnalisée peut personnaliser dynamiquement une définition de rapport avant qu'elle ne soit passée au moteur de traitement lors de l'exécution.|  
|Statut|État (soit rsSuccess, soit un code d'erreur. En cas de plusieurs erreurs, seule la première est enregistrée).|  
|ByteCount|Taille en octets des rapports rendus.|  
|RowCount|Nombre de lignes retournées par les requêtes.|  
|AdditionalInfo|Conteneur de propriétés XML contenant des informations supplémentaires sur l'exécution. Le contenu peut être différent pour chaque ligne.|  
  
##  <a name="the-additionalinfo-field"></a><a name="bkmk_additionalinfo"></a> Champ AdditionalInfo  
 Le champ AdditionalInfo est un conteneur de propriétés ou une structure XML contenant des informations supplémentaires sur l'exécution. Le contenu peut être différent pour chaque ligne dans le journal.  
  
 Voici plusieurs exemples de contenu du champ AddtionalInfo pour la journalisation standard et commentée :  
  
 **Exemple de journalisation standard d'AddtionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>147</ConnectionOpenTime>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>642</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>63</ExecuteReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>157</TotalTimeDataRetrieval>  
          <ExecuteReaderTime>60</ExecuteReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 **Exemple de journalisation commentée d'AdditionalInfo**  
  
```  
<AdditionalInfo>  
  <ProcessingEngine>2</ProcessingEngine>  
  <ScalabilityTime>  
    <Pagination>0</Pagination>  
    <Processing>0</Processing>  
  </ScalabilityTime>  
  <EstimatedMemoryUsageKB>  
    <Pagination>0</Pagination>  
    <Processing>6</Processing>  
  </EstimatedMemoryUsageKB>  
  <DataExtension>  
    <SQL>1</SQL>  
  </DataExtension>  
  <Connections>  
    <Connection>  
      <ConnectionOpenTime>127</ConnectionOpenTime>  
      <DataSource>  
        <Name>DataSource1</Name>  
        <DataExtension>SQL</DataExtension>  
      </DataSource>  
      <DataSets>  
        <DataSet>  
          <Name>DataSet1</Name>  
          <RowsRead>16</RowsRead>  
          <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>33</ExecuteReaderTime>  
          <DataReaderMappingTime>30</DataReaderMappingTime>  
          <DisposeDataReaderTime>1</DisposeDataReaderTime>  
        </DataSet>  
        <DataSet>  
          <Name>DataSet2</Name>  
          <RowsRead>3</RowsRead>  
          <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
          <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
          <ExecuteReaderTime>1</ExecuteReaderTime>  
          <DataReaderMappingTime>0</DataReaderMappingTime>  
          <DisposeDataReaderTime>0</DisposeDataReaderTime>  
        </DataSet>  
      </DataSets>  
    </Connection>  
  </Connections>  
</AdditionalInfo>  
  
```  
  
 Le tableau ci-après présente une partie des valeurs que vous voyez dans le champ AdditionalInfo :  
  
-   **ProcessingEngine**  
  
     1=SQL Server 2005, 2=Nouveau moteur de traitement à la demande. Si la plupart de vos rapports affichent toujours la valeur 1, vous pouvez envisager de les reconcevoir afin qu'ils utilisent le nouveau moteur de traitement à la demande, plus efficace.  
  
     `<ProcessingEngine>2</ProcessingEngine>`  
  
-   **ScalabilityTime**  
  
     Nombre de millisecondes passées pour exécuter des opérations d'échelle dans le moteur de traitement. La valeur 0 indique qu'aucune période de temps supplémentaire n'a été passée sur les opérations d'échelle et indique également que la demande ne présente pas une mémoire insuffisante.  
  
    ```  
    <ScalabilityTime>  
        <Processing>0</Processing>  
    </ScalabilityTime>  
    ```  
  
-   **EstimatedMemoryUsageKB**  
  
     Estimation de la quantité maximale de mémoire, en kilo-octets, consommée par chaque composant pendant une requête particulière.  
  
    ```  
    <EstimatedMemoryUsageKB>  
        <Processing>38</Processing>  
    </EstimatedMemoryUsageKB>  
    ```  
  
-   **DataExtension**  
  
     Types d'extensions de données ou de sources de données utilisées dans le rapport. Le nombre représente le nombre d'occurrences de la source de données spécifique.  
  
    ```  
    <DataExtension>  
       <DAX>2</DAX>  
    </DataExtension>  
    ```  
  
-   **ExternalImages**  
  
     Cette valeur est exprimée en millisecondes. Ces données peuvent être utilisées pour diagnostiquer les problèmes de performances. Le temps nécessaire pour récupérer des images d'un serveur web externe peut ralentir l'exécution globale des rapports.  
  
    ```  
    <ExternalImages>  
        <Count>3</Count>  
        <ByteCount>9268</ByteCount>  
        <ResourceFetchTime>9</ResourceFetchTime>  
    </ExternalImages>  
    ```  
  
-   **Connexions**  
  
     Structure à plusieurs niveaux  
  
    ```  
    <Connections>  
        <Connection>  
          <ConnectionOpenTime>127</ConnectionOpenTime>  
          <DataSource>  
            <Name>DataSource1</Name>  
            <DataExtension>SQL</DataExtension>  
          </DataSource>  
          <DataSets>  
            <DataSet>  
              <Name>DataSet1</Name>  
              <RowsRead>16</RowsRead>  
              <TotalTimeDataRetrieval>655</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>94</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>33</ExecuteReaderTime>  
              <DataReaderMappingTime>30</DataReaderMappingTime>  
              <DisposeDataReaderTime>1</DisposeDataReaderTime>  
            </DataSet>  
            <DataSet>  
              <Name>DataSet2</Name>  
              <RowsRead>3</RowsRead>  
              <TotalTimeDataRetrieval>16</TotalTimeDataRetrieval>  
              <QueryPrepareAndExecutionTime>2</QueryPrepareAndExecutionTime>  
              <ExecuteReaderTime>1</ExecuteReaderTime>  
              <DataReaderMappingTime>0</DataReaderMappingTime>  
              <DisposeDataReaderTime>0</DisposeDataReaderTime>  
            </DataSet>  
          </DataSets>  
        </Connection>  
    </Connections>  
  
    ```  
  
##  <a name="log-fields-executionlog2"></a><a name="bkmk_executionlog2"></a> Champs de journalisation (ExecutionLog2)  
 Cette vue ajoute de nouveaux champs et contient des champs renommés. Voici un exemple d'instruction Transact SQL pour extraire des lignes de la vue ExecutionLog2. L'exemple suppose que la base de données du serveur de rapports est nommée **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog2 order by TimeStart DESC  
```  
  
 La table suivante décrit les données qui sont capturées dans le journal d'exécution des rapports :  
  
|Colonne|Description|  
|------------|------------------------------------------------------------|  
|InstanceName|Nom de l'instance du serveur de rapports qui a géré la demande.|  
|ReportPath|Structure du chemin d'accès au rapport. Un rapport enregistré dans le dossier racine, comme « test », a un ReportPath « /test ».<br /><br /> Pour un rapport nommé « test » qui est enregistré dans le dossier « samples », le ReportPath est « /Samples/test/ »|  
|UserName|Identificateur de l'utilisateur.|  
|ExecutionID||  
|RequestType|Type de demande (utilisateur ou système).|  
|Format|Format de rendu.|  
|Paramètres|Valeurs des paramètres utilisées pour une exécution de rapport.|  
|ReportAction|Valeurs possibles : Render, Sort, BookMarkNavigation, DocumentNavigation, GetDocumentMap, Findstring|  
|TimeStart|Heure de début qui indique la durée d'un traitement de rapport.|
|TimeEnd|Heure de fin qui indique la durée d'un traitement de rapport.|
|TimeDataRetrieval|Nombre de millisecondes passées pour la récupération des données.|
|TimeProcessing|Nombre de millisecondes passées pour le traitement du rapport.|
|TimeRendering|Nombre de millisecondes passées pour le rendu du rapport.|
|Source|Source de l'exécution du rapport (1=Direct, 2=Cache, 3=Instantané, 4=Historique).|  
|Statut|État (soit rsSuccess, soit un code d'erreur. En cas de plusieurs erreurs, seule la première est enregistrée).|  
|ByteCount|Taille en octets des rapports rendus.|  
|RowCount|Nombre de lignes retournées par les requêtes.|  
|AdditionalInfo|Conteneur de propriétés XML contenant des informations supplémentaires sur l'exécution.|  
  
##  <a name="log-fields-executionlog"></a><a name="bkmk_executionlog"></a> Champs de journalisation (ExecutionLog)  
 Voici un exemple d'instruction Transact SQL pour extraire des lignes de la vue ExecutionLog. L'exemple suppose que la base de données du serveur de rapports est nommée **ReportServer**:  
  
```  
Use ReportServer  
select * from ExecutionLog order by TimeStart DESC  
  
```  
  
 La table suivante décrit les données qui sont capturées dans le journal d'exécution des rapports :  
  
|Colonne|Description|  
|------------|-----------------|  
|InstanceName|Nom de l'instance du serveur de rapports qui a géré la demande.|  
|ReportID|Identificateur du rapport.|  
|UserName|Identificateur de l'utilisateur.|  
|RequestType|Valeurs possibles :<br /><br /> True = Demande d'abonnement<br /><br /> False= Demande interactive|  
|Format|Format de rendu.|  
|Paramètres|Valeurs des paramètres utilisées pour une exécution de rapport.|  
|TimeStart|Heures de début et de fin qui indiquent la durée d'un traitement de rapport.|  
|TimeEnd||  
|TimeDataRetrieval|Nombre de millisecondes consacré à la récupération des données, au traitement du rapport et au rendu du rapport.|  
|TimeProcessing||  
|TimeRendering||  
|Source|Source d'exécution du rapport. Valeurs possibles : (1=Actif, 2=Cache, 3=Instantané, 4=Historique, 5=Adhoc, 6=Session, 7=RDCE).|  
|Statut|Valeurs possibles : rsSuccess, rsProcessingAborted, ou un code d'erreur. Si plusieurs erreurs se produisent, seule la première erreur est enregistrée.|  
|ByteCount|Taille en octets des rapports rendus.|  
|RowCount|Nombre de lignes retournées par les requêtes.|  
  
## <a name="see-also"></a>Voir aussi  
 [Activer des événements Reporting Services pour le journal des traces SharePoint &#40;ULS&#41;](../../reporting-services/report-server/turn-on-reporting-services-events-for-the-sharepoint-trace-log-uls.md)   
 [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Guide de référence des erreurs et des événements &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  
  
