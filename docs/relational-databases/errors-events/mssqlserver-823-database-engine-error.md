---
title: Erreur 823 de MSSQLSERVER | mssqlserver_823
description: Description et solutions courantes de l’erreur 823 de Microsoft SQL Server (mssqlserver_823). Il s’agit d’une grave condition d’erreur de niveau système qui menace l’intégrité de la base de données et qui doit être traitée immédiatement.
ms.custom: ''
ms.date: 01/27/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 823 (Database Engine error)
ms.assetid: 0d9fce3c-3772-46ce-a7a3-4f4988dc6cae
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: 7ed748bdf748aa51813d11348859bc769525ed35
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99197899"
---
# <a name="mssqlserver-error-823"></a>Erreur 823 de MSSQLSERVER
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>Détails  
  
| Attribut | Valeur |  
| :-------- | :---- |  
|Nom du produit|SQL Server|  
|ID de l’événement|823|  
|Source de l’événement|MSSQLSERVER|  
|Composant|SQLEngine|  
|Nom symbolique|B_HARDERR|  
|Texte du message|Le système d'exploitation a retourné l'erreur %ls à SQL Server pendant une opération de %S_MSG au niveau du décalage %#016I64x dans le fichier '%ls'. D'autres messages peuvent fournir davantage d'informations dans le journal des erreurs SQL Server et le journal des événements système. Il s'agit d'une condition d'erreur sévère de niveau système qui met en péril l'intégrité de la base de données et qui doit être corrigée immédiatement. Effectuez une vérification complète de la cohérence de la base de données (DBCC CHECKDB). Cette erreur peut avoir de nombreuses causes ; pour plus d'informations, consultez la documentation en ligne de SQL Server.|  
  
## <a name="explanation"></a>Explication  
SQL Server utilise des API Windows (par exemple, [ReadFile](/windows/win32/api/fileapi/nf-fileapi-readfile), [WriteFile](/windows/win32/api/fileapi/nf-fileapi-writefile), [ReadFileScatter](/windows/win32/api/fileapi/nf-fileapi-readfilescatter), [WriteFileGather](/windows/win32/api/fileapi/nf-fileapi-writefilegather)) pour effectuer des opérations d’E/S de fichiers. À l’issue de ces opérations d’E/S, SQL Server vérifie s’il existe des conditions d’erreur en lien avec ces appels d’API. Si les appels d’API échouent avec une erreur du système d’exploitation, SQL Server signale l’erreur 823.

 Le message d’erreur 823 contient les indications suivantes :
 - Fichier de base de données sur lequel l’opération d’E/S a été effectuée
 - Décalage au sein du fichier où l’opération d’E/S a été tentée. Il s’agit du décalage de fichier physique par rapport au début du fichier. En divisant ce nombre par 8192, vous obtenez le numéro de la page logique qui est affectée par l’erreur.
 - Nature de l’opération d’E/S (demande de lecture ou d’écriture)
 - Code d’erreur du système d’exploitation et description de l’erreur entre parenthèses
 

**Erreur du système d’exploitation :** un appel d’API Windows de lecture ou d’écriture échoue et SQL Server rencontre une erreur du système d’exploitation liée à l’appel d’API Windows. Le message suivant est un exemple d’erreur 823 :

```
Error: 823, Severity: 24, State: 2.
2010-03-06 22:41:19.55 spid58 The operating system returned error 1117 (The request could not be performed because of an I/O device error.) to SQL Server during a read at offset 0x0000002d460000 in file 'e:\program files\Microsoft SQL Server\mssql\data\mydb.MDF'. Additional messages in the SQL Server error log and system event log may provide more detail. This is a severe, system-level error condition that threatens database integrity and must be corrected immediately. It is recommended to complete a full database consistency check (DBCC CHECKDB). This error can be caused by many factors; for more information, see SQL Server Books Online.
```

Il est possible que vous constatiez la présence d’erreurs résultant de l’exécution de instruction DBCC CHECKDB sur la base de données associée au fichier indiqué dans le message d’erreur. Vous pouvez exécuter l’instruction DBCC CHECKDB quand vous voyez une erreur 823. Si l’instruction DBCC CHECKDB ne signale pas d’erreur, il est probable que vous rencontrez un problème système intermittent ou un problème de disque.

Quand vous utilisez l’indicateur de trace 818, des informations de diagnostic supplémentaires peuvent être écrites dans le fichier journal des erreurs de SQL Server pour les erreurs 823.
Pour plus d’informations, consultez [KB 826433 : Diagnostics SQL Server supplémentaires ajoutés pour détecter les problèmes d’E/S non signalés](https://support.microsoft.com/help/826433/sql-server-diagnostics-added-to-detect-unreported-i-o-problems-due-to)


## <a name="cause"></a>Cause
Le message d’erreur 823 indique généralement l’existence d’un problème au niveau du système de stockage sous-jacent, du matériel ou d’un pilote qui se trouve dans le chemin de la demande d’E/S. Vous pouvez rencontrer cette erreur en cas d’incohérences dans le système de fichiers ou si le fichier de base de données est endommagé. Dans le cas d’une lecture de fichier, SQL Server aura déjà retenté la demande de lecture quatre fois avant de retourner l’erreur 823. Si la nouvelle tentative réussit, la requête n’échoue pas, mais le message [825](mssqlserver-825-database-engine-error.md) est écrit dans le journal des erreurs (ERRORLOG) et le journal des événements.

## <a name="user-action"></a>Action de l'utilisateur  
 - Examinez la table [suspect_pages](../system-tables/suspect-pages-transact-sql.md) dans MSDB pour vérifier si d’autres pages rencontrent ce problème (dans la même base de données ou dans d’autres bases de données).
 - Vérifiez la cohérence des bases de données situées sur le même volume (celui indiqué dans le message 823) à l’aide de la commande DBCC CHECKDB. Si vous trouvez des incohérences à partir de la commande DBCC CHECKDB, aidez-vous des conseils fournis dans [Comment résoudre les erreurs de cohérence de base de données signalées par DBCC CHECKB](https://support.microsoft.com/help/2015748/how-to-troubleshoot-database-consistency-errors-reported-by-dbcc-check). 
 - Vérifiez si les journaux des événements Windows contiennent des erreurs ou des messages signalés par le système d’exploitation, un dispositif de stockage ou un pilote de périphérique. S’ils sont liés à cette erreur de quelque manière que ce soit, corrigez ces erreurs en premier. Par exemple, outre le message 823, il se peut aussi que vous notiez la présence d’un événement de type « Le pilote a détecté une erreur de contrôleur sur \Device\Harddisk4\DR4 » signalé par la source Disque dans le journal des événements. Dans ce cas, vous devez déterminer si ce fichier est présent sur ce dispositif, puis commencez par corriger ces erreurs de disque.
 - Utilisez l’utilitaire [SQLIOSim](https://support.microsoft.com/help/231619/how-to-use-the-sqliosim-utility-to-simulate-sql-server-activity-on-a-d) pour déterminer si ces erreurs 823 peuvent être reproduites en dehors des demandes d’E/S normales de SQL Server. L’utilitaire SQLIOSim étant fourni avec SQL Server 2008 et les versions ultérieures, il n’est pas nécessaire de le télécharger séparément. Il se trouve généralement dans le dossier `C:\Program Files\Microsoft SQL Server\MSSQLxx.MSSQLSERVER\MSSQL\Binn`.
 - Vérifiez auprès du fournisseur de matériel ou du fabricant du dispositif que
   - Les dispositifs matériels et la configuration sont conformes aux conditions d’E/S de SQL Server
   - Les pilotes de périphérique et les autres composants logiciels de prise en charge de tous les dispositifs situés dans le chemin d’E/S sont à jour
 - Si le fournisseur de matériel ou le fabriquant de l’unité vous a fourni des utilitaires de diagnostic, servez-vous-en pour évaluer l’intégrité du système d’E/S
 - Déterminez si le chemin de ces demandes d’E/S contient des [pilotes de filtre](https://support.microsoft.com/help/2454053/use-of-system-filter-drivers-can-lead-to-sql-server-database-engine-pe) sujets à des problèmes.
   - Vérifiez s’il existe des mises à jour pour ces pilotes de filtre
   - Vérifiez si ces pilotes de filtre peuvent être supprimés ou désactivés et si cela fait disparaître le problème à l’origine de l’erreur 823  
