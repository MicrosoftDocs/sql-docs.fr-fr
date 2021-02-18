---
title: Partenaires pour la haute disponibilité et la reprise d’activité de SQL Server
description: Listes de partenaires tiers proposant des solutions de haute disponibilité et récupération d’urgence pour les services SQL Server.
ms.topic: conceptual
ms.custom: seo-dt-2019
ms.date: 09/17/2017
ms.prod: sql
ms.technology: release-landing
ms.author: mikeray
author: MikeRayMSFT
ms.openlocfilehash: 7c6a58d336de824aaad46b9e67d4f491fe96fc37
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100339363"
---
# <a name="sql-server-high-availability-and-disaster-recovery-partners"></a>Partenaires pour la haute disponibilité et la récupération d’urgence de SQL Server
[!INCLUDE[sqlserver](../includes/applies-to-version/sqlserver.md)]
Pour fournir la haute disponibilité et la récupération d’urgence pour vos services SQL Server, vous pouvez choisir parmi un large éventail d’outils de pointe.  Cet article présente les sociétés partenaires de Microsoft offrant des solutions de haute disponibilité et de récupération d’urgence qui prennent en charge Microsoft SQL Server.

## <a name="high-availability-and-disaster-recovery-partners"></a>Partenaires pour la haute disponibilité et la récupération d’urgence

| Partenaire | Description | Liens | 
| --- | --- | --- |
|![Azure][5] |**Azure Site Recovery**<br>Site Recovery réplique les charges de travail en cours d’exécution sur des machines virtuelles ou des serveurs physiques afin de les garder à disposition dans un emplacement secondaire si le site principal n’est pas disponible. Vous pouvez répliquer et basculer des machines virtuelles SQL Server à partir du centre de données local vers Azure ou vers un autre centre de données local, ou d’un centre de données Azure vers un autre centre de données Azure.<br><br> Éditions Standard et Enterprise de SQL Server 2008 R2 - SQL Server 2016|[Site web][azure_website]<br>[Place de marché][azure_marketplace]<br>[Feuille de données][azure_datasheet]<br>[Twitter][azure_twitter]<br>[Vidéo][azure_youtube]|
|![DH2i][2] |**DH2i**<br>DxEnterprise est un logiciel de disponibilité intelligente pour Windows, Linux et Docker permettant d’obtenir des temps d’arrêt, planifiés ou non, aussi proches que possible de zéro, de faire des économies importantes, de simplifier considérablement la gestion et de bénéficier d’une consolidation à la fois physique et logique.<br><br>SQL Server 2005+, Windows Server 2008 R2+, Ubuntu 16+, RHEL 7+, CentOS 7+|[Site web][dh2i_website]<br>[Feuille de données][dh2i_datasheet]<br>[Twitter][dh2i_twitter]<br>[Vidéo][dh2i_youtube]|
|![Logo HPE][4] |**HPE Serviceguard**<br>Protégez vos charges de travail SQL Server 2017 critiques sur Linux &reg; contre les temps d’indisponibilité planifiés ou non, dus à une multitude de défaillances des infrastructures et des applications dans des environnements physiques et virtuels à n’importe quelle distance, avec HPE Serviceguard pour Linux (SGLX). HPE SGLX A.12.20.00 (et versions ultérieures) offre des options de monitoring et de récupération contextuelles pour l’instance du cluster de basculement et les charges de travail SQL Server des groupes de disponibilité Always On. Optimisez la disponibilité avec HPE SGLX sans compromettre l’intégrité des données et les performances.<br><br>SQL Server 2017 sur Linux - RedHat 7.3, 7.4, SUSE 12 SP2, SP3|[Site web][hpe_website]<br>[Feuille de données][hpe]<br>[Télécharger l’évaluation][hpe_download]<br>[Blog][hpe_download]<br>[Twitter][hpe_twitter]
|![IDERA][3]|**IDERA**<br>SQL Safe Backup est une solution de sauvegarde et de récupération à hautes performances pour SQL Server, qui permet des économies en réduisant le temps de sauvegarde des bases de données et la taille des fichiers de sauvegarde, et en fournissant un accès instantané en lecture et en écriture aux bases de données dans les fichiers de sauvegarde.<br><br>Microsoft SQL Server : 2005 SP1 ou ultérieur, 2008, 2008 R2, 2012, 2014, 2016 ; toutes les éditions |[Site web][idera_website]|
|![NEC][7]|**NEC**<br>ExpressCluster est une solution complète et entièrement automatisée de récupération d’urgence et de haute disponibilité contre les principales défaillances (matérielles, logicielles, de réseau et de site) pour SQL Server et les applications associées s’exécutant sur des machines physiques ou virtuelles dans des environnements locaux ou cloud.<br><br>Microsoft SQL Server : version 2005 ou ultérieure ; toutes les éditions |[Site web][necec_website]<br>[Feuille de données][necec_datasheet]<br>[Vidéo][necec_youtube]<br>[Télécharger][necec_download]|
|![Portworx][6] |**Portworx**<br>Portworx est la solution pour les conteneurs avec état s’exécutant en production. Avec Portworx, les utilisateurs peuvent gérer toutes les bases de données ou services avec état sur n’importe quelle infrastructure utilisant n’importe quel planificateur de conteneur, notamment Kubernetes, Mesosphere DC/OS et Docker Swarm. Portworx résout les cinq problèmes les plus courants rencontrés par les équipes DevOps lors de l’exécution de bases de données en conteneur et d’autres services avec état en production : persistance, haute disponibilité, automatisation des données, prise en charge de plusieurs magasins de données et infrastructures, et sécurité.<br><br>SQL Server 2017 sur Docker |[Site web][portworx_website]<br>[Documentation][portworx_docs]<br>[Vidéo][portworx_youtube]|
|![SIOS][8] |**SIOS**<br>SIOS Technology offre des solutions de haute disponibilité et de récupération d’urgence pour SQL Server sous Windows et sous Linux. Le clustering SANless SIOS dispense d’utiliser un réseau SAN de stockage partagé et offre toute la flexibilité nécessaire pour protéger les applications les plus importantes dans les configurations physiques, virtuelles, cloud et cloud hybride des environnements, qu’ils soient multisites ou non.<br><br>Ajoutez SIOS DataKeeper à votre environnement de clustering de basculement Windows Server pour créer une ressource de volume SANless qui remplace le stockage partagé classique, ce qui facilite l’exécution de WSFC dans Azure.<br><br>SIOS Protection Suite est une solution de clustering entièrement flexible qui protège des applications Linux critiques comme SQL Server, SAP, HANA, Oracle et bien d’autres.|[Site web][sios_website]<br>[Feuille de données][sios_datasheet]<br>[Twitter][sios_twitter]<br>[Place de marché][sios_marketplace]<br>[Vidéo][sios_youtube]|
|![Veeam][1] |**Veeam**<br>Veeam Backup & Replication est une solution de sauvegarde et de disponibilité puissante, facile à utiliser et abordable. Elle permet la récupération rapide, flexible et fiable des applications virtualisées et des données, en rassemblant la sauvegarde et la réplication des machines virtuelles dans une même solution logicielle. Veeam Backup & Replication a reçu plusieurs récompenses pour sa prise en charge des environnements virtuels VMware vSphere et Microsoft Hyper-V.<br><br>SQL Server 2005 SP4 - SQL Server 2016 sur Windows |[Site web][veeam_website]<br>[Feuille de données][veeam_datasheet]<br>[Twitter][veeam_twitter]<br>[Vidéo][veeam_youtube]|

## <a name="next-steps"></a>Étapes suivantes
Pour plus d’informations sur les autres partenaires, consultez les [partenaires de supervision][mon_partners], les [partenaires de gestion][management_partners] et les [partenaires de développement][dev_partners].

<!--Image references-->
[1]: ./media/partner-hadr-sql-server/Veeam-green-logo.png
[2]: ./media/partner-hadr-sql-server/dh2i-logo.png
[3]: ./media/partner-hadr-sql-server/idera-logo.png
[4]: ./media/partner-hadr-sql-server/hpe.png
[5]: ./media/partner-hadr-sql-server/azure-logo.png
[6]: ./media/partner-hadr-sql-server/portworx-logo.png
[7]: ./media/partner-hadr-sql-server/nec-logo.png
[8]: ./media/partner-hadr-sql-server/sios-logo.png

<!--Article links-->
[mon_partners]: ./partner-monitor-sql-server.md
[management_partners]: ./partner-management-sql-server.md
[dev_partners]: ./partner-dev-sql-server.md

<!--Website links -->
[veeam_website]:https://www.veeam.com/
[dh2i_website]:https://dh2i.com
[idera_website]:https://www.idera.com/productssolutions/sqlserver
[hpe_website]: https://www.hpe.com/us/en/product-catalog/detail/pip.376220.html
[azure_website]: /azure/site-recovery/site-recovery-sql
[necec_website]: https://www.necam.com/ExpressCluster/
[portworx_website]: https://portworx.com/
[sios_website]: https://us.sios.com/

<!--Get Started Links-->

<!--Datasheet Links-->
[veeam_datasheet]:https://www.veeam.com/veeam_backup_9_5_datasheet_en_ds.pdf
[dh2i_datasheet]:https://dh2i.com/wp-content/uploads/DxE-Win-QuickFacts.pdf
[hpe]:https://www.hpe.com/h20195/v2/default.aspx?cc=us&lc=en&oid=376220
[necec_datasheet]: https://www.necam.com/docs/?id=0d9ef7a7-f935-4909-b6bb-20a47b3
[azure_datasheet]: /azure/site-recovery/vmware-physical-azure-support-matrix
[sios_datasheet]: https://us.sios.com/solutions/high-availability-cluster-software-cloud/

<!--Marketplace Links -->
[azure_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps?search=site%20recovery&page=1
[sios_marketplace]: https://azuremarketplace.microsoft.com/marketplace/apps/sios_datakeeper.sios-datakeeper-8
<!--Press links-->
<!--[veeam_press]:-->

<!--YouTube links-->
[veeam_youtube]:https://www.youtube.com/user/YouVeeam
[dh2i_youtube]:https://www.youtube.com/user/dh2icompany 
[idera_youtube]:https://www.idera.com/resourcecentral/videos/sql-safe-overview
[azure_youtube]: https://mva.microsoft.com/en-US/training-courses/is-your-lack-of-a-disaster-recovery-site-keeping-you-up-at-night-8680?l=oF7YrFH1_7504984382
[necec_youtube]: https://www.youtube.com/watch?v=9La3Cw1Q1Jk
[portworx_youtube]: https://www.youtube.com/channel/UCSexpvQ9esSRgiS_Q9_3mLQ
[sios_youtube]: https://www.youtube.com/watch?v=U3M44gJNWQE

<!--Twitter links-->
[veeam_twitter]:https://twitter.com/veeam
[dh2i_twitter]:https://twitter.com/dh2i
[hpe_twitter]:https://twitter.com/hpe
[azure_twitter]:https://twitter.com/hashtag/azuresiterecovery
[sios_twitter]:https://www.twitter.com/SIOSTech

<!--Docs links>-->
[portworx_docs]: https://docs.portworx.com/

<!--Download links-->
[hpe_download]: https://h20392.www2.hpe.com/portal/swdepot/displayProductInfo.do?productNumber=SGLX-DEMO
[necec_download]: https://www.necam.com/ExpressCluster/30daytrial/
<!--Blog links-->
[hpe_blog]: https://community.hpe.com/t5/Servers-The-Right-Compute/SQL-Server-for-Linux-Is-Here-and-A-New-Chapter-for-Mission/ba-p/6977571#.WiHWW0xFwUE