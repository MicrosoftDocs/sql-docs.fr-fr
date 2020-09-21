---
title: Journal HTTP Report Server | Microsoft Docs
description: Découvrez comment activer le journal HTTP Report Server après avoir installé Reporting Services. Cette fonctionnalité journalise chaque requête et réponse HTTP gérée par un serveur de rapports.
ms.date: 06/12/2019
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-server
ms.topic: conceptual
helpviewer_keywords:
- HTTP [Reporting Services]
ms.assetid: 6cc433b7-165c-4b16-9034-79256dd6735f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: c1f270bb3f26cada3944979ee32019737dcb13e3
ms.sourcegitcommit: df1f0f2dfb9452f16471e740273cd1478ff3100c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87392075"
---
# <a name="report-server-http-log"></a>Journal HTTP Report Server
  Le fichier journal HTTP Report Server garde un enregistrement de chaque requête et réponse HTTP gérée par le serveur de rapports. Dans la mesure où les erreurs de dépassement de capacité et de délai d'attente des requêtes n'atteignent pas le serveur de rapports, elles ne sont pas enregistrées dans le fichier journal.  
  
 La journalisation HTTP n'est pas activée par défaut. Vous devez modifier le fichier de configuration ReportingServicesService.exe pour utiliser cette fonctionnalité dans votre installation.  
  
## <a name="viewing-log-information"></a>Affichage des informations des journaux  
 Le journal est un fichier texte ASCII. Vous pouvez utiliser n'importe quel éditeur de texte pour afficher le fichier. Le fichier journal HTTP Report Server est équivalent au fichier journal étendu W3C des services IIS (Internet Information Services) ; il utilise des champs semblables afin que vous puissiez vous servir des visionneuses de fichiers journaux IIS existantes pour lire le fichier journal HTTP du serveur de rapports. Le tableau suivant fournit des informations supplémentaires sur le fichier journal HTTP :  
  
|informations relatives au fichier journal|Description|  
|-|-|  
|Nom de fichier|Par défaut, le nom de fichier est ReportServerService_HTTP_\<timestamp>.log. Vous pouvez personnaliser le préfixe du nom de fichier en modifiant l'attribut HttpTraceFileName dans le fichier ReportingServicesService.exe.config. L'horodateur est basé sur l'heure UTC (Coordinated Universal Time).|  
|Emplacement du fichier|Le fichier se trouve à l’emplacement \Microsoft SQL Server\\ *\<SQL Server Instance>* \Reporting Services\LogFiles.|  
|Format de fichier|Le fichier est au format EN-US. Il s'agit d'un fichier texte ASCII.|  
|Création et rétention du fichier|Le journal HTTP est créé une fois que vous l'avez activé dans le fichier de configuration, que vous avez redémarré le service, et que le serveur de rapports a géré une requête HTTP. Si vous configurez les paramètres mais que le fichier journal ne s'affiche pas, ouvrez un rapport ou démarrez une application du serveur de rapports (par exemple le portail web) afin de générer une requête HTTP pour créer le fichier.<br /><br /> Une nouvelle instance du fichier journal est créée chaque fois que le service redémarre et que la requête HTTP qui en résulte est envoyée au serveur de rapports.<br /><br /> Par défaut, les journaux des traces sont limités à 32 mégaoctets et sont supprimés après 14 jours.|  
  
## <a name="configuration-settings-for-report-server-http-log"></a>Paramètres de configuration du journal HTTP Report Server  
 Pour configurer le journal HTTP Report Server, utilisez le Bloc-notes afin de modifier le fichier ReportingServicesService.exe.config. Le fichier de configuration se trouve dans le dossier \Program Files\Microsoft SQL Server\MSSQL.n\Reporting Services\ReportServer\Bin.  
  
 Pour activer le serveur HTTP, vous devez ajouter **http:4** à la section RStrace du fichier ReportingServicesService.exe.config. Toutes les autres entrées du fichier journal HTTP sont facultatives. L'exemple suivant inclut tous les paramètres afin que vous puissiez coller l'intégralité de la section sur la section RStrace, et supprimer ensuite les paramètres dont vous n'avez pas besoin.
  
```  
   <RStrace>  
         <add name="FileName" value="ReportServerService_" />  
         <add name="FileSizeLimitMb" value="32" />  
         <add name="KeepFilesForDays" value="14" />  
         <add name="Prefix" value="tid, time" />  
         <add name="TraceListeners" value="debugwindow, file" />  
         <add name="TraceFileMode" value="unique" />  
         <add name="HttpTraceFileName" value="ReportServerService_HTTP_" />  
         <add name="HttpTraceSwitches" value="date,time,clientip,username,serverip,serverport,host,method,uristem,uriquery,protocolstatus,bytesreceived,timetaken,protocolversion,useragent,cookiereceived,cookiesent,referrer" />  
         <add name="Components" value="all:3,http:4" />  
   </RStrace>  
```  
  
## <a name="log-file-fields"></a>Champs du fichier journal  
 Le tableau suivant décrit les champs disponibles dans le journal. La liste des champs est configurable ; vous pouvez spécifier quels sont les champs à inclure via le paramètre de configuration **HTTPTraceSwitches** . La colonne **Default** spécifie si le champ doit être inclus automatiquement dans le fichier journal, si vous ne spécifiez pas **HTTPTraceSwitches**.  
  
|Champ|Description|Default|  
|-----------|-----------------|-------------|  
|HttpTraceFileName|Cette valeur est facultative. La valeur par défaut est ReportServerServiceHTTP_. Vous pouvez spécifier une autre valeur si vous souhaitez utiliser une convention d'affectation de noms de fichiers distincte (pour inclure le nom du serveur lorsque vous enregistrez les fichiers journaux dans un emplacement central, par exemple).|Oui|  
|HTTPTraceSwitches|Cette valeur est facultative. Si vous le spécifiez, vous pouvez configurer les champs du fichier journal en utilisant le format délimité par des virgules.|Non|  
|Date|Date à laquelle l'activité s'est produite.|Non|  
|Temps|Heure à laquelle l'activité s'est produite.|Non|  
|ClientIp|Adresse IP du client qui accède au serveur de rapports.|Oui|  
|UserName|Nom de l'utilisateur qui a accédé au serveur de rapports.|Non|  
|ServerPort|Numéro de port utilisé pour la connexion.|Non|  
|Host|Contenu de l'en-tête de l'hôte.|Non|  
|Méthode|Action ou méthode SOAP appelée à partir du client.|Oui|  
|UriStem|Ressource ayant fait l'objet d'un accès.|Oui|  
|UriQuery|Requête utilisée pour accéder à la ressource.|Non|  
|ProtocolStatus|Code d’état HTTP.|Oui|  
|BytesReceived|Nombre d'octets reçus par le serveur.|Non|  
|TimeTaken|Délai écoulé (en millisecondes) entre le moment où HTTP.SYS retourne les données de requête et le moment où le serveur termine le dernier envoi, sans inclure la durée de transmission réseau.|Non|  
|ProtocolVersion|Version de protocole utilisée par le client.|Non|  
|UserAgent|Type de navigateur utilisé par le client.|Non|  
|CookieReceived|Contenu du cookie reçu par le serveur.|Non|  
|CookieSent|Contenu du cookie envoyé par le serveur.|Non|  
|Referrer|Site précédent visité par le client.|Non|  
  
## <a name="see-also"></a>Voir aussi  
 [Report Server Service Trace Log](../../reporting-services/report-server/report-server-service-trace-log.md)   
 [Fichiers journaux et sources de Reporting Services](../../reporting-services/report-server/reporting-services-log-files-and-sources.md)   
 [Guide de référence des erreurs et des événements &#40;Reporting Services&#41;](../../reporting-services/troubleshooting/errors-and-events-reference-reporting-services.md)  
  