---
description: Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche (Recherche SQL Server)
title: Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
ms.assetid: 29b4488e-4c6a-4bf0-a64d-19e2fdafa7ae
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.custom: seo-lt-2019
ms.openlocfilehash: 8362788a2bf1092e815a9df43c3b65298935d555
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344465"
---
# <a name="revert-word-breakers-used-by-search-to-previous-version-sql-server-search"></a>Rétablir la version précédente des analyseurs lexicaux utilisés par la recherche (Recherche SQL Server)

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

[!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] installe et active une version des analyseurs lexicaux et des générateurs de formes dérivées pour toutes les langues prises en charge par la recherche en texte intégral à l’exception du coréen. Cet article décrit comment passer de cette version de ces composants à la version précédente, ou de la version précédente à la nouvelle version.  
  
 Cet article ne couvre pas les langues suivantes :  
  
- **Anglais**. Pour rétablir ou restaurer les composants anglais, consultez [Modifier l'analyseur lexical utilisé pour l'anglais des États-Unis et l'anglais (R.U.)](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md).  
  
- **Danois, polonais et turc**. Les analyseurs lexicaux tiers pour le danois, le polonais et le turc qui étaient inclus avec les versions antérieures de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ont été remplacés par les composants [!INCLUDE[msCoName](../../includes/msconame-md.md)] .  
  
- **Tchèque et grec**. Il existe de nouveaux analyseurs lexicaux pour le tchèque et le grec. Les versions antérieures de la recherche en texte intégral de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ne prenaient pas en charge ces deux langues.  
  
- **Coréen**. L'analyseur lexical et le générateur de formes dérivées pour le coréen ne sont pas mis à niveau dans cette version.  
  
 Pour obtenir des informations générales sur les analyseurs lexicaux et générateurs de formes dérivées, consultez [Configurer et gérer les analyseurs lexicaux et générateurs de formes dérivées pour la recherche](../../relational-databases/search/configure-and-manage-word-breakers-and-stemmers-for-search.md).  
  
##  <a name="overview-of-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="overview"></a> Vue d'ensemble du rétablissement et de la restauration des analyseurs lexicaux et des générateurs de formes dérivées  
 Les instructions relatives au rétablissement et à la restauration des analyseurs lexicaux et des générateurs de formes dérivées dépendent de la langue. Le tableau suivant résume les trois ensembles d’actions qui peuvent être obligatoires pour rétablir la version antérieure des composants.  
  
|Fichier actuel|Fichier précédent|Nombre de langues affectées|Action pour les fichiers|Action pour les entrées de Registre|  
|------------------|-------------------|----------------------------------|----------------------|---------------------------------|  
|NaturalLanguage6.dll|NaturalLanguage6.dll|34|Obtenez et installez une version précédente de NaturalLanguage6.dll en remplaçant la version actuelle du fichier.|Aucune action requise.<br /><br /> Les clés de Registre et les valeurs n'ont pas changé pour cette version.|  
|(Autre nom de fichier)|NaturalLanguage6.dll|5|Obtenez et installez une version précédente de NaturalLanguage6.dll en remplaçant la version actuelle du fichier.|Modifiez un ensemble d'entrées de Registre pour spécifier la version précédente des composants.|  
|(Autre nom de fichier)|(Autre nom de fichier)|6|Aucune action requise.<br /><br /> [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] copie la version actuelle et la version précédente des composants dans le dossier Binn.|Modifiez un ensemble d'entrées de Registre pour spécifier la version précédente des composants.|  
  
> [!WARNING]  
>  Si vous remplacez la version actuelle du fichier NaturalLanguage6.dll par une version différente, le comportement de toutes les langues qui utilisent ce fichier est affecté.  
  
 Les fichiers décrits dans cet article sont des fichiers DLL installés dans le dossier `MSSQL\Binn` de l’instance [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]. Le chemin d'accès complet est généralement le suivant :  
  
 `C:\Program Files\Microsoft SQL Server\<instance>\MSSQL\Binn`  
  
##  <a name="languages-for-which-the-file-name-of-both-the-current-and-previous-word-breaker-is-naturallanguage6dll"></a><a name="nl6nl6"></a> Langues pour lesquelles le nom de fichier de l'analyseur lexical actuel et celui de l'analyseur lexical précédent est NaturalLanguage6.dll  
 Pour les langues répertoriées dans le tableau suivant, le nom de fichier de l'analyseur lexical actuel et celui de l'analyseur lexical précédent est NaturalLanguage6.dll. Pour rétablir ou restaurer ces composants, vous devez remplacer NaturalLanguage6.dll par une version différente du même fichier. Vous n'êtes pas obligé de modifier les entrées de Registre, car celles-ci n'ont pas changé pour cette version.  
  
> [!WARNING]  
>  Si vous remplacez la version actuelle du fichier NaturalLanguage6.dll par une version différente, le comportement de toutes les langues qui utilisent ce fichier est affecté.  
  
 **Liste des langues affectées**  
  
|Langage|Abréviation<br />utilisée dans le<br />Registre|LCID|  
|--------------|---------------------------------------|----------|  
|Bengali|`ben`|1093|  
|Bulgare|`bgr`|1026|  
|Catalan|`cat`|1027|  
|Espagnol|`esn`|3082|  
|Français|`fra`|1036|  
|Goudjrati|`guj`|1095|  
|Hébreu|`heb`|1037|  
|Hindi|`hin`|1081|  
|Croate|`hrv`|1050|  
|Indonésien|`ind`|1057|  
|Islandais|`isl`|1039|  
|Italien|`ita`|1040|  
|Kannada|`kan`|1099|  
|Lituanien|`lth`|1063|  
|Letton|`lvi`|1062|  
|Malayalam|`mal`|1100|  
|Marathi|`mar`|1102|  
|Malais|`msl`|1086|  
|Neutre|`Neutral`|0000|  
|Norvégien (bokmål)|`nor`|1044|  
|Pendjabi|`pan`|1094|  
|Portugais (Brésil)|`ptb`|1046|  
|Portugais|`ptg`|2070|  
|Roumain|`rom`|1048|  
|Slovaque|`sky`|1051|  
|Slovène|`slv`|1060|  
|Serbe - Cyrillique|`srb`|3098|  
|Serbe - Latin|`srl`|2074|  
|Suédois|`sve`|1053|  
|Tamoul|`tam`|1097|  
|Télougou|`tel`|1098|  
|Ukrainien|`ukr`|1058|  
|Ourdou|`urd`|1056|  
|Vietnamien|`vit`|1066|  
  
 Le tableau précédent est trié par ordre alphabétique selon la colonne Abréviation.  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="nl6nl6revert"></a> Pour rétablir les composants précédents  
  
1.  Accédez au dossier Binn décrit ci-dessus.  
  
2.  Sauvegardez la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll dans un autre emplacement.  
  
3.  Copiez la version précédente de NaturalLanguage6.dll du dossier Binn d'une instance de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou de [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] dans le dossier Binn de l'instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Cette modification affecte toutes les langues qui utilisent NaturalLanguage6.dll dans la version actuelle et la version précédente.  
  
4.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  

###  <a name="to-restore-the-current-components"></a><a name="nl6nl6restore"></a> Pour restaurer les composants actuels  
  
1.  Accédez à l'emplacement où vous avez sauvegardé la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll.  
  
2.  Copiez la version actuelle de NaturalLanguage6.dll à partir de l'emplacement de sauvegarde dans le dossier Binn de l'instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Cette modification affecte toutes les langues qui utilisent NaturalLanguage6.dll dans la version actuelle et la version précédente.  
  
3.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
##  <a name="languages-for-which-the-file-name-of-the-previous-word-breaker-only-is-naturallanguage6dll"></a><a name="newnl6"></a> Langues pour lesquelles le nom de fichier de l'analyseur lexical précédent est NaturalLanguage6.dll  
 Pour les langues répertoriées dans le tableau suivant, le nom de fichier de l'analyseur lexical précédent est différent de celui de la nouvelle version. Le nom de fichier précédent est NaturalLanguage6.dll. Pour rétablir la version précédente, vous devez remplacer la version actuelle de NaturalLanguage6.dll par une version précédente du même fichier. Vous devez également modifier un ensemble d'entrées de Registre pour spécifier la version précédente ou la version actuelle des composants.  
  
> [!WARNING]  
>  Si vous remplacez la version actuelle du fichier NaturalLanguage6.dll par une version différente, le comportement de toutes les langues qui utilisent ce fichier est affecté.  
  
 **Liste des langues affectées**  
  
|Langage|Abréviation<br />utilisée dans le<br />Registre|LCID|  
|--------------|---------------------------------------|----------|  
|Arabe|ara|1025|  
|Allemand|deu|1031|  
|Japonais|jpn|1041|  
|Néerlandais|nld|1043|  
|Russe|rus|1049|  
  
 Le tableau précédent est trié par ordre alphabétique selon la colonne Abréviation.  
  
 Utilisez les instructions suivantes avec la liste de valeurs dans la section [Noms de fichiers et valeurs de Registre pour rétablir et restaurer les analyseurs lexicaux et les générateurs de formes dérivées](#newnl6values).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnl6revert"></a> Pour rétablir les composants précédents  
  
1.  Accédez au dossier Binn décrit ci-dessus.  
  
2.  Ne supprimez pas les fichiers pour la version actuelle des composants du dossier Binn.  
  
3.  Sauvegardez la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll dans un autre emplacement.  
  
4.  Copiez la version précédente de NaturalLanguage6.dll du dossier Binn d’une instance de [!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] ou [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] dans le dossier Binn de la nouvelle instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)].  
  
    > [!WARNING]  
    >  Cette modification affecte toutes les langues qui utilisent NaturalLanguage6.dll dans la version actuelle et la version précédente.  
  
5.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
6.  Procédez comme suit pour ajouter de nouvelles clés pour les COM ClassID des interfaces précédentes de l'analyseur lexical et du générateur de formes dérivées pour la langue sélectionnée :  
  
    1.  Ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour l'analyseur lexical précédent.  
  
    2.  Remplacez les données (par défaut) de cette valeur de clé par le nom de fichier de l'analyseur lexical précédent figurant dans le tableau.  
  
    3.  Si la langue sélectionnée utilise un générateur de formes dérivées, ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour le générateur de formes dérivées précédent.  
  
    4.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez les données (par défaut) de cette valeur de clé par le nom de fichier du générateur de formes dérivées précédent figurant dans le tableau.  
  
7.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clé_langue>* représente l’abréviation de la langue utilisée dans le Registre, par exemple « fra » pour le français et « esn » pour l’espagnol.  
  
8.  Remplacez la valeur de la clé **WBreakerClass** par la valeur figurant dans le tableau pour l'analyseur lexical actuel.  
  
9. Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez la valeur de la clé **StemmerClass** par la valeur figurant dans le tableau pour le générateur de formes dérivées actuel.  
  
10. Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-current-components"></a><a name="newnl6restore"></a> Pour restaurer les composants actuels  
  
1.  Accédez à l'emplacement où vous avez sauvegardé la version [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] de NaturalLanguage6.dll.  
  
2.  Copiez la version actuelle de NaturalLanguage6.dll à partir de l'emplacement de sauvegarde dans le dossier Binn de l'instance [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] .  
  
    > [!WARNING]  
    >  Cette modification affecte toutes les langues qui utilisent NaturalLanguage6.dll dans la version actuelle et la version précédente.  
  
3.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
4.  Si les clés suivantes n'existent pas, procédez comme suit pour ajouter de nouvelles clés pour les COM ClassID des interfaces actuelles de l'analyseur lexical et du générateur de formes dérivées pour la langue sélectionnée :  
  
    1.  Ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour l'analyseur lexical actuel.  
  
    2.  Remplacez les données (par défaut) de cette valeur de clé par le nom de fichier de l'analyseur lexical actuel figurant dans le tableau.  
  
    3.  Si la langue sélectionnée utilise un générateur de formes dérivées, ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour le générateur de formes dérivées actuel.  
  
    4.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez les données (par défaut) de cette valeur de clé par le nom de fichier du générateur de formes dérivées actuel figurant dans le tableau.  
  
5.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clé_langue>* représente l’abréviation de la langue utilisée dans le Registre, par exemple « fra » pour le français et « esn » pour l’espagnol.  
  
6.  Remplacez la valeur de la clé **WBreakerClass** par la valeur figurant dans le tableau pour l'analyseur lexical précédent.  
  
7.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez la valeur de la clé **StemmerClass** par la valeur figurant dans le tableau pour le générateur de formes dérivées précédent.  
  
8.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnl6values"></a> Noms de fichiers et valeurs de Registre pour rétablir et restaurer les analyseurs lexicaux et les générateurs de formes dérivées  
 Utilisez la liste suivante de noms de fichiers et d'entrées de Registre avec les instructions dans la section précédente. Utilisez les valeurs précédentes pour rétablir la version précédente ou utilisez les valeurs actuelles pour restaurer la version actuelle des composants.  
  
 La liste suivante est triée par ordre alphabétique selon l'abréviation utilisée pour chaque langue.  
  
 **Arabe (ara), LCID 1025**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|7EFD3C7E-9E4B-4a93-9503-DECD74C0AC6D|483B0283-25DB-4c92-9C15-A65925CB95CE|  
|Nom de fichier précédent|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actuel|04b37e30-c9a9-4a7d-8f20-792fc87ddf71|None|  
|Nom de fichier actuel|MSWB7.dll|None|  
  
 **Allemand (deu), LCID 1031**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|45EACA36-DBE9-4e4a-A26D-5C201902346D|65170AE4-0AD2-4fa5-B3BA-7CD73E2DA825|  
|Nom de fichier précédent|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actuel|dfa00c33-bf19-482e-a791-3c785b0149b4|8a474d89-6e2f-419c-8dd5-9b50edc8c787|  
|Nom de fichier actuel|MSWB7.dll|MSWB7.dll|  
  
 **Japonais (jpn), LCID 1041**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|E1E8F15E-8BEC-45df-83BF-50FF84D0CAB5|3D5DF14F-649F-4cbc-853D-F18FEDE9CF5D|  
|Nom de fichier précédent|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actuel|04096682-6ece-4e9e-90c1-52d81f0422ed|None|  
|Nom de fichier actuel|MsWb70011.dll|None|  
  
 **Néerlandais (nld), LCID 1043**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|2C9F6BEB-C5B0-42b6-A5EE-84C24DC0D8EF|F7A465EE-13FB-409a-B878-195B420433AF|  
|Nom de fichier précédent|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actuel|69483c30-a9af-4552-8f84-a0796ad5285b|CF923CB5-1187-43ab-B053-3E44BED65FFA|  
|Nom de fichier actuel|MSWB7.dll|MSWB7.dll|  
  
 **Russe (rus), LCID 1049**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|2CB6CDA4-1C14-4392-A8EC-81EEF1F2E079|E06A0DDD-E81A-4e93-8A8D-F386C3A1B670|  
|Nom de fichier précédent|NaturalLanguage6.dll|NaturalLanguage6.dll|  
|CLSID actuel|aaa3d3bd-6de7-4317-91a0-d25e7d3babc3|d42c8b70-adeb-4b81-a52f-c09f24f77dfa|  
|Nom de fichier actuel|MSWB7.dll|MSWB7.dll|  
  
##  <a name="languages-for-which-neither-the-previous-nor-the-current-file-name-is-naturallanguage6dll"></a><a name="newnew"></a> Langues pour lesquelles ni le nom de fichier précédent ni le nom de fichier actuel n'est NaturalLanguage6.dll  
 Pour les langues dans le tableau suivant, les noms de fichier des analyseurs lexicaux et des générateurs de formes dérivées précédents sont différents des noms de fichier des nouvelles versions. Ni le nom de fichier précédent ni le nom de fichier actuel n'est NaturalLanguage6.dll. Vous n'êtes pas obligé de remplacer des fichiers, et ce car le programme d'installation de [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] copie la version actuelle et la version précédente des composants dans le dossier Binn. Toutefois, vous devez modifier un ensemble d'entrées de Registre pour spécifier la version précédente ou la version actuelle des composants.  
  
 **Liste des langues affectées**  
  
|Langage|Abréviation<br />utilisée dans le<br />Registre|LCID|  
|--------------|---------------------------------------|----------|  
|Chinois simplifié|chs|2052|  
|Chinois traditionnel|cht|1028|  
|Thaï|tha|1054|  
|Chinois traditionnel|zh-hk|3076|  
|Chinois traditionnel|zh-mo|5124|  
|Chinois (simplifié)|zh-sg|4100|  
  
 Le tableau précédent est trié par ordre alphabétique selon la colonne Abréviation.  
  
 Utilisez les instructions suivantes avec la liste de valeurs dans la section [Noms de fichiers et valeurs de Registre pour rétablir et restaurer les analyseurs lexicaux et les générateurs de formes dérivées](#newnewvalues).  
  
###  <a name="to-revert-to-the-previous-components"></a><a name="newnewrevert"></a> Pour rétablir les composants précédents  
  
1.  Ne supprimez pas les fichiers pour la version actuelle des composants du dossier Binn.  
  
2.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Procédez comme suit pour ajouter de nouvelles clés pour les COM ClassID des interfaces précédentes de l'analyseur lexical et du générateur de formes dérivées pour la langue sélectionnée :  
  
    1.  Ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour l'analyseur lexical précédent.  
  
    2.  Remplacez les données (par défaut) de cette valeur de clé par le nom de fichier de l'analyseur lexical précédent figurant dans le tableau.  
  
    3.  Si la langue sélectionnée utilise un générateur de formes dérivées, ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour le générateur de formes dérivées précédent.  
  
    4.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez les données (par défaut) de cette valeur de clé par le nom de fichier du générateur de formes dérivées précédent figurant dans le tableau.  
  
4.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clé_langue>* représente l’abréviation de la langue utilisée dans le Registre, par exemple « fra » pour le français et « esn » pour l’espagnol.  
  
5.  Remplacez la valeur de la clé **WBreakerClass** par la valeur figurant dans le tableau pour l'analyseur lexical actuel.  
  
6.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez la valeur de la clé **StemmerClass** par la valeur figurant dans le tableau pour le générateur de formes dérivées actuel.  
  
7.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="to-restore-the-previous-components"></a><a name="newnewrestore"></a> Pour restaurer les composants précédents  
  
1.  Ne supprimez pas les fichiers pour la version précédente des composants du dossier Binn.  
  
2.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\CLSID**.  
  
3.  Si les clés suivantes n'existent pas, procédez comme suit pour ajouter de nouvelles clés pour les COM ClassID des interfaces actuelles de l'analyseur lexical et du générateur de formes dérivées pour la langue sélectionnée :  
  
    1.  Ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour l'analyseur lexical actuel.  
  
    2.  Remplacez les données (par défaut) de cette valeur de clé par le nom de fichier de l'analyseur lexical actuel figurant dans le tableau.  
  
    3.  Si la langue sélectionnée utilise un générateur de formes dérivées, ajoutez une nouvelle clé avec la valeur figurant dans le tableau pour le générateur de formes dérivées actuel.  
  
    4.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez les données (par défaut) de cette valeur de clé par le nom de fichier du générateur de formes dérivées actuel figurant dans le tableau.  
  
4.  Dans le Registre, accédez au nœud suivant : **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\<InstanceRoot>\MSSearch\Language\<language_key>** . *<clé_langue>* représente l’abréviation de la langue utilisée dans le Registre, par exemple « fra » pour le français et « esn » pour l’espagnol.  
  
5.  Remplacez la valeur de la clé **WBreakerClass** par la valeur figurant dans le tableau pour l'analyseur lexical précédent.  
  
6.  Si la langue sélectionnée utilise un générateur de formes dérivées, remplacez la valeur de la clé **StemmerClass** par la valeur figurant dans le tableau pour le générateur de formes dérivées précédent.  
  
7.  Redémarrez [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
###  <a name="file-names-and-registry-values-for-reverting-and-restoring-word-breakers-and-stemmers"></a><a name="newnewvalues"></a> Noms de fichiers et valeurs de Registre pour rétablir et restaurer les analyseurs lexicaux et les générateurs de formes dérivées  
 Utilisez la liste suivante de noms de fichiers et d'entrées de Registre avec les instructions dans la section précédente. Utilisez les valeurs précédentes pour rétablir la version précédente ou utilisez les valeurs actuelles pour restaurer la version actuelle des composants.  
  
 La liste suivante est triée par ordre alphabétique selon l'abréviation utilisée pour chaque langue.  
  
 **Chinois simplifié (chs), LCID 2052**  
  
|Composant|Analyseur lexical|  
|---------------|------------------|  
|CLSID précédent|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nom de fichier précédent|chsbrkr.dll|  
|CLSID actuel|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nom de fichier actuel|MsWb70804.dll|  
  
 **Chinois traditionnel (cht), LCID 1028**  
  
|Composant|Analyseur lexical|  
|---------------|------------------|  
|CLSID précédent|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nom de fichier précédent|chtbrkr.dll|  
|CLSID actuel|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nom de fichier actuel|MsWb70404.dll|  
  
 **Thaï (tha), LCID 1054**  
  
|Composant|Analyseur lexical|Générateur de formes dérivées|  
|---------------|------------------|-------------|  
|CLSID précédent|CCA22CF4-59FE-11D1-BBFF-00C04FB97FDA|CEDC01C7-59FE-11D1-BBFF-00C04FB97FDA|  
|Nom de fichier précédent|Thawbrkr.dll|Thawbrkr.dll|  
|CLSID actuel|F70C0935-6E9F-4ef1-9F06-7876536DB900|None|  
|Nom de fichier actuel|MsWb7001e.dll|None|  
  
 **Chinois traditionnel (zh-hk), LCID 3076**  
  
|Composant|Analyseur lexical|  
|---------------|------------------|  
|CLSID précédent|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nom de fichier précédent|chtbrkr.dll|  
|CLSID actuel|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nom de fichier actuel|MsWb70404.dll|  
  
 **Chinois traditionnel (zh-mo), LCID 5124**  
  
|Composant|Analyseur lexical|  
|---------------|------------------|  
|CLSID précédent|1680E7C3-9430-4A51-9B82-1E7E7AEE5258|  
|Nom de fichier précédent|chtbrkr.dll|  
|CLSID actuel|E9B1DF65-08F1-438b-8277-EF462B23A792|  
|Nom de fichier actuel|MsWb70404.dll|  
  
 **Chinois simplifié (zh-sg), LCID 4100**  
  
|Composant|Analyseur lexical|  
|---------------|------------------|  
|CLSID précédent|12CE94A0-DEFB-11D2-B31D-00600893A857|  
|Nom de fichier précédent|chsbrkr.dll|  
|CLSID actuel|E0831C90-BAB0-4ca5-B9BD-EA254B538DAC|  
|Nom de fichier actuel|MsWb70804.dll|  
  
## <a name="see-also"></a>Voir aussi  
 [Modifier l’analyseur lexical utilisé pour l’anglais des États-Unis et l’anglais du Royaume-Uni](../../relational-databases/search/change-the-word-breaker-used-for-us-english-and-uk-english.md)   
 [Changements de comportement de la recherche en texte intégral](./full-text-search.md)
