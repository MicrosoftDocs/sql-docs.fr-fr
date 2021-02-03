---
description: MSSQLSERVER_824
title: MSSQLSERVER_824 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 824 (Database Engine error)
ms.assetid: 2aa22246-2712-4fdb-9744-36e7e6f3175e
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 031a478a281538b096d0900ad80fa34741182269
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180306"
---
# <a name="mssqlserver_824"></a>MSSQLSERVER_824
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|824|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|B_HARDSSERR|  
|Texte du message|SQL Server a détecté une erreur d'E/S logique et relative à la cohérence. L'erreur %ls s'est produite pendant une opération de %S_MSG de la page %S_PGID dans la base de données avec l'ID %d au niveau du décalage %#016I64x dans le fichier '%ls'.  Vous trouverez peut-être plus de détails dans les messages supplémentaires qui figurent dans le journal des erreurs et le journal des événements système de SQL Server.|  
  
## <a name="symptom"></a>Symptôme  


Vous pouvez rencontrer le message d’erreur suivant dans le journal des erreurs de SQL Server ou dans le journal des événements des applications Windows en cas d’échec d’une vérification de cohérence logique après la lecture ou l’écriture d’une page de base de données :
 
``` 
2009-11-02 15:46:42.90 spid51      Error: 824, Severity: 24, State: 2.
2009-11-02 15:46:42.90 spid51      SQL Server detected a logical consistency-based I/O error: incorrect pageid (expected 1:43686; actual 0:0). It occurred during a read of page (1:43686) in database ID 23 at offset 0x0000001554c000 in file 'H:\MSSQL.SQL2008\MSSQL\DATA\my_db.mdf'.  Additional messages in the SQL Server error log or system event log may provide more detail. This is a severe error condition that threatens database integrity and must be corrected immediately. Complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```
 
Si une application rencontre ce message lors de l’interrogation ou de la modification des données, le message d’erreur est renvoyé à l’application et la connexion à la base de données est interrompue. 
  
## <a name="cause"></a>Cause
Cette erreur indique que Windows considère que la page est correctement lue à partir du disque alors que [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] signale qu'il y a un problème avec la page. Cette erreur est similaire à l’erreur 823, à la différence qu’elle n’a pas été détectée par Windows. Cela indique généralement un problème dans le sous-système d’E/S, comme un lecteur de disque défaillant, des problèmes avec le microprogramme d’un disque, un pilote de périphérique défectueux, etc. Pour plus d’informations sur les erreurs d’E/S, consultez [Concepts de base des E/S Microsoft SQL Server, chapitre 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).  

SQL Server utilise des API Windows (par exemple, ReadFile, WriteFile, ReadFileScatter, WriteFileGather]) pour effectuer les opérations d’E/S. À l’issue de ces opérations d’E/S, SQL Server vérifie s’il existe des conditions d’erreur en lien avec ces appels d’API. Si ces appels d’API échouent avec une erreur du système d’exploitation, SQL Server signale l’erreur 823. Il peut arriver que l’appel de l’API Windows aboutisse, mais que les données transférées par l’opération d’E/S aient rencontré un problème de cohérence logique. Ces problèmes de cohérence logique sont signalés par l’erreur 824.
 
L’erreur 824 contient les informations suivantes :

- Fichier de base de données sur lequel l’opération d’E/S est effectuée
- Décalage par rapport au fichier où l’opération d’E/S a été tentée
- Base de données à laquelle ce fichier appartient
- Numéro de page impliqué dans l’opération d’E/S
- L’opération était-elle une opération de lecture ou d’écriture
- Détails sur la vérification de cohérence logique qui a échoué [type de vérification, valeur réelle et valeur attendue utilisées pour cette vérification]
 
Ces vérifications de cohérence logique sont des contrôles d’intégrité supplémentaires effectués par SQL Server pour garantir que certains aspects clés des données impliquées dans le transfert de données ont été maintenus au cours de l’opération d’E/S. Les vérifications incluent Somme de contrôle, Page endommagée, Transfert rapide, ID de page incorrect, Lecture obsolète, Échec de l’audit de page. La nature des vérifications effectuées varie en fonction des différentes options de configuration au niveau de la base de données et du serveur. 
 
Le message d’erreur 824 indique généralement l’existence d’un problème au niveau du système de stockage sous-jacent, du matériel ou d’un pilote qui se trouve dans le chemin de la demande d’E/S. Vous pouvez rencontrer cette erreur en cas d’incohérences dans le système de fichiers ou si le fichier de base de données est endommagé.

## <a name="resolution"></a>Résolution  

Si vous rencontrez l’erreur 824, vous pouvez essayer les solutions suivantes : 

- Examinez la table [suspect_pages](../backup-restore/manage-the-suspect-pages-table-sql-server.md) dans msdb pour vérifier si d’autres pages (dans la même base de données ou dans d’autres bases de données) rencontrent ce problème.
- Vérifiez la cohérence des bases de données situées sur le même volume (celui indiqué dans le message 824) à l’aide de la commande DBCC CHECKDB. Si vous trouvez des incohérences à partir de la commande DBCC CHECKDB, aidez-vous des conseils fournis dans l’article de la Base de connaissances [Comment résoudre les erreurs de cohérence de base de données signalées par DBCC CHECKB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check).
- Si la base de données qui rencontre ces erreurs 824 n’a pas l’option de base de données PAGE_VERIFY CHECKSUM activée, activez-la immédiatement. Les erreurs 824 peuvent se produire pour d’autres raisons qu’une erreur de somme de contrôle, mais CHECKSUM offre la meilleure option pour vérifier la cohérence de la page après qu’elle a été écrite sur le disque.
- Vérifiez si les journaux des événements Windows contiennent des erreurs ou des messages signalés par le système d’exploitation, un dispositif de stockage ou un pilote de périphérique. Si ces erreurs sont liées à cette erreur de quelque manière que ce soit, corrigez-les en premier. Par exemple, outre le message 824, il se peut aussi que vous notiez la présence d’un événement de type « Le pilote a détecté une erreur de contrôleur sur \Device\Harddisk4\DR4 » signalé par la source Disque dans le journal des événements. Dans ce cas, vous devez déterminer si ce fichier est présent sur ce dispositif, puis commencez par corriger ces erreurs de disque.
- Utilisez l’utilitaire [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) pour déterminer si ces erreurs 824 peuvent être reproduites en dehors des demandes d’E/S normales de SQL Server. SQLIOSim étant fourni avec SQL Server 2008, il n’est pas nécessaire de le télécharger séparément pour cette version ou une version ultérieure.
- Vérifiez auprès du fournisseur de matériel ou du fabricant de l’appareil que :
   - Les dispositifs matériels et la configuration sont conformes aux [conditions d’E/S de SQL Server](https://support.microsoft.com/help/967576/microsoft-sql-server-database-engine-input-output-requirements).
   - Les pilotes de périphérique et les autres composants logiciels de prise en charge de tous les appareils situés dans le chemin d’E/S sont à jour.
- Si le fournisseur de matériel ou le fabricant de l’appareil vous a fourni des utilitaires de diagnostic, utilisez-les pour évaluer l’intégrité du système d’E/S.
- Déterminez si le chemin de ces demandes d’E/S contient des pilotes de filtre sujets à des problèmes.
   - Vérifiez s’il existe des mises à jour pour ces pilotes de filtre
   - Vérifiez si ces pilotes de filtre peuvent être supprimés ou désactivés et si cela fait disparaître le problème à l’origine de l’erreur 824
- S'il ne s'agit pas d'un problème de matériel et qu'une sauvegarde saine est disponible, restaurez la base de données à partir de cette sauvegarde.  

## <a name="see-also"></a>Voir aussi  
[Gérer la table suspect_pages &#40;SQL Server&#41;](~/relational-databases/backup-restore/manage-the-suspect-pages-table-sql-server.md)  
  
