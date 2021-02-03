---
description: MSSQLSERVER_832
title: MSSQLSERVER_832
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 832 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: bba8712825dc3ef14617ad4e8d8f7312b479e936
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 01/30/2021
ms.locfileid: "99180296"
---
# <a name="mssqlserver_832"></a>MSSQLSERVER_832
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>Détails

|Attribut|Valeur|
|---|---|
|Nom du produit|SQL Server|
|ID de l’événement|832|
|Source de l’événement|MSSQLSERVER|
|Composant|SQLEngine|
|Nom symbolique|B_CONSTPAGECHANGED|
|Texte du message|Une page qui aurait dû être constante a changé (somme de contrôle attendue : \<expected value>, somme de contrôle réelle : \<actual value>, base de données \<dbid>, fichier \'<filename>, page \<pageno>). Cela indique généralement une défaillance de la mémoire ou tout autre dommage au niveau du matériel ou du système d'exploitation.|
||

## <a name="explanation"></a>Explication

Un facteur externe a entraîné la modification d’une page de base de données en dehors du code normal du moteur [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est utilisé pour modifier les pages de base de données.  Ce facteur externe peut être :  

- Un thread s’exécutant dans le processus [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui génère des écritures incorrectes dans une page de base de données. Dans ce cas, on parle souvent de « scribbler ».
- Un problème lié au matériel ou au système d’exploitation, lors duquel la mémoire qui est allouée à la page de la base de données est incorrecte ou endommagée  

Lorsque [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] détecte ce problème, l’erreur 832 est générée.

## <a name="user-action"></a>Action requise

Pour déterminer la cause de l’erreur, envisagez les options suivantes :

- Vous devez exécuter toutes les vérifications habituelles du matériel ou du système afin de détecter tout problème lié à la mémoire, au processeur ou autre composant matériel. Vérifiez que tous les pilotes système, les mises à jour du système d’exploitation et les mises à jour matérielles ont été appliqués au système. Envisagez d’exécuter des diagnostics de fabrication de matériel, y compris des tests liés à la mémoire.
- Parmi les DLL « externes » chargées dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)], évaluez lesquelles sont susceptibles de provoquer ce problème, notamment les procédures stockées étendues, les objets COM ou d’autres DLL qui peuvent modifier à tort la mémoire [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] qui est réservée aux pages de base de données.  

À chaque fois que vous voyez cette erreur, vous devez immédiatement exécuter `DBCC CHECKDB` sur la base de données référencée par le \<dbid> dans le message d’erreur.

## <a name="more-information"></a>Informations complémentaires

Cette erreur est détectée par la tâche en arrière-plan, souvent appelée LazyWriter (la « commande » associée à cette tâche s’affiche sous le nom de LAZY WRITER). Par conséquent, cette erreur n’est pas retournée à une application cliente. L’erreur est écrite dans le Journal des événements de l’application Windows, avec EventID=832.  

Seules les pages qui ne sont pas modifiées dans le cache (ou « compromises ») sont vérifiées. C’est la raison pour laquelle le message utilise le terme « constant », car la page n’a jamais été modifiée depuis qu’elle a été lue à partir du disque. De plus, la page a été lue sous sa forme « propre » à partir du disque, car elle a une valeur de somme de contrôle et n’a pas rencontré d’échec de somme de contrôle (Msg 824). Toutefois, la page peut avoir été modifiée après cette erreur, puis avoir été écrite sur le disque avec une modification incorrecte. Si cette erreur se produit, une nouvelle somme de contrôle est calculée sur la base de toutes les modifications, avant d’être écrite sur le disque. Par conséquent, la page peut être endommagée sur le disque, sans qu’une lecture ultérieure ne déclenche l’échec de la somme de contrôle. Il est important d’exécuter `DBCC CHECKDB` sur toutes les bases de données référencées par cette erreur.  

Il est possible que même `DBCC CHECKDB` ne signale pas d’erreur pour une page dans cet état après son écriture sur le disque. Cela est dû au fait que la modification incorrecte peut se trouver là où la page ne contient pas de données ni d’informations importantes sur la structure de la page ou de la ligne. Cela peut être aussi dû à des modifications de données que CHECKDB ne peut pas détecter.  

Pour plus d’informations sur Msg 832, consultez le livre blanc [SQL Server I/O Basics, Chapter 2](/previous-versions/sql/sql-server-2005/administrator/cc917726(v=technet.10)).