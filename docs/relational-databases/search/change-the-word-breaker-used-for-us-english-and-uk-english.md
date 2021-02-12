---
description: Modifier l'analyseur lexical utilisé pour l'anglais des États-Unis et l'anglais (R.U.)
title: Modifier l’analyseur lexical utilisé pour l’anglais des États-Unis et l’anglais (R.U.) | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 6b5d2177-db98-47f5-b32e-4b80a2f74ffe
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: de2501c3af1fcc24aed29dc66da455d7e2ff1f2b
ms.sourcegitcommit: 10ae200635b9e8554e6bc6f658125e1a80d4d5ae
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99589289"
---
# <a name="change-the-word-breaker-used-for-us-english-and-uk-english"></a>Modifier l'analyseur lexical utilisé pour l'anglais des États-Unis et l'anglais (R.U.)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  À partir de [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)], le programme d’installation installe une nouvelle version de l’analyseur lexical et du générateur de formes dérivées pour la langue anglaise, en remplaçant la version précédente de ces composants. Pour plus d’informations sur la modification du comportement des nouveaux composants, consultez [Changements de comportement pour la recherche en texte intégral](./full-text-search.md). Cette rubrique décrit comment passer de la nouvelle version de ces composants à la version précédente, ou de la version précédente à la nouvelle version. Pour les installations de cluster, ces modifications doivent être apportées sur tous les nœuds principaux et passifs.  
  
 Certaines versions précédentes de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] utilisaient des analyseurs lexicaux différents représentés par des CLSID différents pour l’anglais des États-Unis (LCID 1033) et l’anglais du Royaume-Uni (LCID 2057). Dans cette version, les deux LCID utilisent les mêmes composants avec le même CLSID, comme indiqué dans le tableau suivant :  
  
|LCID|Analyseur lexical installé par les versions précédentes<br /><br /> version 12.0.6828.0|Générateur de formes dérivées installé par les versions précédentes|Analyseur lexical installé par cette version<br /><br /> version 14.0.4999.1038|Générateur de formes dérivées installé par cette version|  
|----------|-------------------------------------------------------------------------|--------------------------------------------|-----------------------------------------------------------------------|---------------------------------------|  
|1033<br />(Anglais des États-Unis)|188D6CC5-CB03-4C01-912E-47D21295D77E|EEED4C20-7F1B-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
|2057<br />(Anglais du Royaume-Uni)|173C97E2-AEBE-437C-9445-01B237ABF2F6|D99F7670-7F1A-11CE-BE57-00AA0051FE20|9faed859-0b30-4434-ae65-412e14a16fb8|e1e5ef84-c4a6-4e50-8188-99aef3de2659|  
  
 Les composants décrits dans cette rubrique sont des fichiers DLL installés dans le dossier `MSSQL\Binn` de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] . Le chemin complet est généralement `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`.  
  
 Pour plus d’informations sur les analyseurs lexicaux et générateurs de formes dérivées, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
## <a name="switching-from-the-current-english-word-breaker-to-the-previous-english-word-breakers"></a>Basculement de l'analyseur lexical anglais actuel vers les analyseurs lexicaux anglais précédents  
  
#### <a name="to-switch-from-the-current-version-of-the-us-english-word-breaker-to-the-previous-version"></a>Pour basculer de la version actuelle de l'analyseur lexical anglais des États-Unis vers la version précédente  
  
1.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Procédez comme suit pour ajouter de nouvelles clés pour les COM ClassID des interfaces précédentes de l'analyseur lexical et du générateur de formes dérivées anglais des États-Unis pour LCID 1033 :  
  
    1.  Ajoutez une nouvelle clé avec la valeur **{188D6CC5-CB03-4C01-912E-47D21295D77E}** pour l’analyseur lexical précédent.  
  
    2.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **langwrbk.dll**.  
  
    3.  Ajoutez une nouvelle clé avec la valeur **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}** pour le générateur de formes dérivées précédent.  
  
    4.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **infosoft.dll**.  
  
3.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<RacineInstance\>\MSSearch\Language\enu**.  
  
4.  Mettez à jour la valeur de clé **WBreakerClass** en la remplaçant par **{188D6CC5-CB03-4C01-912E-47D21295D77E}**.  
  
5.  Mettez à jour la valeur de clé **StemmerClass** en la remplaçant par **{EEED4C20-7F1B-11CE-BE57-00AA0051FE20}**.  
  
6.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

#### <a name="to-switch-from-the-current-version-of-the-uk-english-word-breaker-to-the-previous-version"></a>Pour basculer de la version actuelle de l'analyseur lexical anglais (R.U.) vers la version précédente  
  
1.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Procédez comme suit pour ajouter une nouvelle clé pour les COM ClassID des interfaces précédentes de l'analyseur lexical et du générateur de formes dérivées anglais (R.U.) pour LCID 2057 :  
  
    1.  Ajoutez une nouvelle clé avec la valeur **{173C97E2-AEBE-437C-9445-01B237ABF2F6}** pour l’analyseur lexical précédent.  
  
    2.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **langwrbk.dll**.  
  
    3.  Ajoutez une nouvelle clé avec la valeur **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}** pour le générateur de formes dérivées précédent.  
  
    4.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **infosoft.dll**.  
  
3.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<racine_instance\>\MSSearch\Language\eng**.  
  
4.  Mettez à jour la valeur de clé **WBreakerClass** en la remplaçant par **{173C97E2-AEBE-437C-9445-01B237ABF2F6}**.  
  
5.  Mettez à jour la valeur de clé **StemmerClass** en la remplaçant par **{D99F7670-7F1A-11CE-BE57-00AA0051FE20}**.  
  
6.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="switching-back-from-the-previous-english-word-breakers-to-the-current-english-word-breaker"></a>Rebasculement des analyseurs lexicaux anglais précédents vers l'analyseur lexical anglais actuel  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-us-english-word-breaker-to-the-current-version"></a>Pour rebasculer de la version précédente de l'analyseur lexical anglais des États-Unis vers la version actuelle  
  
1.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Si les clés suivantes n'existent pas, procédez comme suit pour ajouter une nouvelle clé pour les COM ClassID des interfaces actuelles de l'analyseur lexical et du générateur de formes dérivées anglais des États-Unis pour LCID 1033 :  
  
    1.  Ajoutez une nouvelle clé dont la valeur est **{9faed859-0b30-4434-ae65-412e14a16fb8}** pour l’analyseur lexical actuel.  
  
    2.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **MsWb7.dll**.  
  
    3.  Ajoutez une nouvelle clé dont la valeur est **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** pour le générateur de formes dérivées actuel.  
  
    4.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **MsWb7.dll**.  
  
3.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<racine_instance\>\MSSearch\Language\eng**.  
  
4.  Mettez à jour la valeur de clé **WBreakerClass** en la définissant sur **{9faed859-0b30-4434-ae65-412e14a16fb8}**.  
  
5.  Mettez à jour la valeur de clé **StemmerClass** en la définissant sur **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**.  
  
6.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
#### <a name="to-switch-back-from-the-previous-version-of-the-uk-english-word-breaker-to-the-current-version"></a>Pour rebasculer de la version précédente de l'analyseur lexical anglais (R.U.) vers la version actuelle  
  
1.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<InstanceRoot\>\MSSearch\CLSID**.  
  
2.  Si les clés suivantes n'existent pas, procédez comme suit pour ajouter une nouvelle clé pour les COM ClassID des interfaces actuelles de l'analyseur lexical et du générateur de formes dérivées anglais (R.U.) pour LCID 2057 :  
  
    1.  Ajoutez une nouvelle clé dont la valeur est **{9faed859-0b30-4434-ae65-412e14a16fb8}** pour l’analyseur lexical actuel.  
  
    2.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **MsWb7.dll**.  
  
    3.  Ajoutez une nouvelle clé dont la valeur est **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}** pour le générateur de formes dérivées actuel.  
  
    4.  Mettez à jour les données (valeurs par défaut) de cette valeur de clé en la définissant sur **MsWb7.dll**.  
  
3.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\\<racine_instance\>\MSSearch\Language\eng**.  
  
4.  Mettez à jour la valeur de clé **WBreakerClass** en la définissant sur **{9faed859-0b30-4434-ae65-412e14a16fb8}**.  
  
5.  Mettez à jour la valeur de clé **StemmerClass** en la définissant sur **{e1e5ef84-c4a6-4e50-8188-99aef3de2659}**.  
  
6.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
## <a name="see-also"></a>Voir aussi  
 [Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche](../../relational-databases/search/revert-the-word-breakers-used-by-search-to-the-previous-version.md)   
 [Changements de comportement de la recherche en texte intégral](./full-text-search.md)  
  
