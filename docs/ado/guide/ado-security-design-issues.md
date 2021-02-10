---
description: Fonctionnalités de conception de la sécurité ADO
title: Problèmes de conception de la sécurité ADO | Microsoft Docs
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, security
ms.assetid: 86b83a38-efdf-4831-a6d5-7e470d517d1c
author: rothja
ms.author: jroth
ms.openlocfilehash: c992b58b6b84f79efa62c63353e3b8f1efcc333d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030497"
---
# <a name="ado-security-design-features"></a>Fonctionnalités de conception de la sécurité ADO
Les sections suivantes décrivent les fonctionnalités de conception de sécurité de ActiveX Data Objects (ADO) 2,8 et versions ultérieures. Ces modifications ont été apportées dans ADO 2,8 pour améliorer la sécurité. ADO 6,0, qui est inclus dans Windows DAC 6,0 dans Windows Vista, est fonctionnellement équivalent à ADO 2,8, qui était inclus dans MDAC 2,8 dans Windows XP et Windows Server 2003. Cette rubrique fournit des informations sur la façon de sécuriser au mieux vos applications dans ADO 2,8 ou une version ultérieure.

> [!IMPORTANT]
>  Si vous mettez à jour votre application à partir d’une version antérieure d’ADO, il est recommandé de tester votre application mise à jour sur un ordinateur autre que de production avant de la déployer sur les clients. De cette façon, vous pouvez vous assurer que vous avez pris connaissance des problèmes de compatibilité avant de déployer votre application mise à jour.

## <a name="internet-explorer-file-access-scenarios"></a>Scénarios d’accès aux fichiers dans Internet Explorer
 Les fonctionnalités suivantes ont une incidence sur le fonctionnement d’ADO 2,8 et versions ultérieures lorsqu’il est utilisé dans des pages Web de script dans Internet Explorer.

### <a name="revised-and-improved-security-warning-message-box-now-used-to-alert-users"></a>Boîte de message d’avertissement de sécurité révisée et améliorée maintenant utilisée pour alerter les utilisateurs
 Pour ADO 2,7 et versions antérieures, le message d’avertissement suivant apparaît lorsqu’une page Web avec script tente d’exécuter du code ADO à partir d’un fournisseur non approuvé :

```console
This page accesses data on another domain. Do you want to allow this? To
avoid this message in Internet Explorer, you can add a secure Web site to
your Trusted Sites zone on the Security tab of the Internet Options dialog
box.
```

 Pour ADO 2,8 et versions ultérieures, le message précédent n’apparaît plus. Au lieu de cela, le message suivant apparaît dans ce contexte :

```console
This Website uses a data provider that may be unsafe. If you trust the
Website, click OK, otherwise click Cancel.
```

 Le message précédent permet à l’utilisateur de prendre une décision informée, tout en connaissant les conséquences pour les deux choix :

-   Si l’utilisateur approuve le site, cliquez sur OK pour autoriser l’exécution et l’exécution dans la fenêtre du navigateur de tout le code sécurisé sur disque (toutes les méthodes et propriétés ADO avec les exceptions des API accessibles sur le disque décrites plus loin dans cette rubrique).

-   Si l’utilisateur n’approuve pas le site, cliquez sur Annuler pour bloquer l’exécution et l’exécution dans son intégralité du code ADO pour l’accès aux données.

### <a name="disk-accessible-code-limited-now-to-trusted-sites"></a>Code accessible sur disque limité maintenant aux sites de confiance
 Des modifications de conception supplémentaires ont été apportées dans ADO 2,8, qui restreignent spécifiquement la capacité d’un ensemble limité d’API, ce qui peut exposer le potentiel de lecture ou d’écriture dans les fichiers sur l’ordinateur local. Voici les méthodes d’API qui ont été encore plus limitées en matière de sécurité lors de l’exécution d’Internet Explorer :

-   Pour l’objet ADO **Stream** , si les méthodes [LoadFromFile](../reference/ado-api/loadfromfile-method-ado.md) ou [SaveToFile](../reference/ado-api/savetofile-method.md) sont utilisées.

-   Pour l’objet **Recordset** ADO, si la méthode [Save](../reference/ado-api/save-method.md) ou la méthode [Open](../reference/ado-api/open-method-ado-recordset.md) , par exemple quand l’option **adCmdFile** est définie ou si le [fournisseur de persistance Microsoft OLE DB (MSPersist)](./appendixes/microsoft-ole-db-persistence-provider-ado-service-provider.md) est utilisé.

 Pour ces ensembles limités de fonctions potentiellement accessibles sur disque, le comportement suivant se produit pour ADO 2,8 et versions ultérieures, si un code qui utilise ces méthodes est exécuté dans Internet Explorer :

-   Si le site qui a fourni le code a été ajouté précédemment à la liste de la zone sites de confiance, le code s’exécute dans le navigateur et l’accès est accordé aux fichiers locaux.

-   Si le site n’apparaît pas dans la liste zone de sites de confiance, le code est bloqué et l’accès aux fichiers locaux est refusé.

    > [!NOTE]
    >  Dans ADO 2,8 et versions ultérieures, l’utilisateur n’est pas averti ou n’est pas invité à ajouter des sites à la liste des zones de sites de confiance. Par conséquent, la gestion de la liste des sites de confiance est la responsabilité de ceux qui déploient ou prennent en charge des applications basées sur le site Web qui requièrent l’accès au système de fichiers local.

### <a name="access-blocked-to-the-activecommand-property-on-recordset-objects"></a>Accès bloqué à la propriété ActiveCommand sur les objets Recordset
 En cas d’exécution dans Internet Explorer, ADO 2,8 bloque désormais l’accès à la propriété [ActiveCommand](../reference/ado-api/activecommand-property-ado.md) pour un objet **Recordset** actif et retourne une erreur. L’erreur se produit même si la page provient d’un site Web inscrit dans la liste des sites de confiance.

### <a name="changes-in-handling-for-ole-db-providers-and-integrated-security"></a>Modifications de la gestion des fournisseurs de OLE DB et de la sécurité intégrée
 Lors de la révision d’ADO 2,7 et des versions antérieures pour des problèmes potentiels de sécurité et des préoccupations, le scénario suivant a été découvert :

 Dans certains cas, OLE DB fournisseurs qui prennent en charge la propriété Integrated Security [DBPROP_AUTH_INTEGRATED](/previous-versions/windows/desktop/ms712973(v=vs.85)) peuvent autoriser des pages Web scriptées à réutiliser l’objet de connexion ADO pour se connecter involontairement à d’autres serveurs à l’aide des informations d’identification de connexion actuelles des utilisateurs. Pour éviter cela, ADO 2,8 et versions ultérieures gèrent les fournisseurs de OLE DB en fonction de la façon dont ils ont choisi de fournir ou de ne pas fournir de sécurité intégrée.

 Pour les pages Web qui sont chargées à partir de sites répertoriés dans la liste zone de sites de confiance, le tableau suivant explique comment ADO 2,8 et versions ultérieures gèrent les connexions ADO dans chaque cas.

|Paramètres IE pour l’authentification de l’utilisateur, ouverture de session|Le fournisseur prend en charge la « sécurité intégrée » et l’UID et le PWD sont spécifiés (SQLOLEDB)|Le fournisseur ne prend pas en charge la « sécurité intégrée » (JOLT, MSDASQL, MSPersist)|Le fournisseur prend en charge la « sécurité intégrée » et est défini sur SSPI (aucun UID/PWD n’est spécifié)|
|------------------------------------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
|Ouverture de session automatique avec le nom d’utilisateur et le mot de passe actuels|Autoriser la connexion|Autoriser la connexion|Autoriser la connexion|
|Demander le nom d’utilisateur et le mot de passe|Autoriser la connexion|Échec de la connexion|Échec de la connexion|
|Ouverture de session automatique uniquement dans la zone Intranet|Autoriser la connexion|Demander à l’utilisateur un avertissement de sécurité|Demander à l’utilisateur un avertissement de sécurité|
|Ouverture de session anonyme|Autoriser la connexion|Échec de la connexion|Échec de la connexion|

 Dans le cas où un avertissement de sécurité s’affiche à présent, la boîte de message informe les utilisateurs :

```console
This Website is using your identity to access a data source. If you trust this Website, click OK, otherwise click Cancel.
```

 Le message précédent permet à l’utilisateur de prendre une décision plus éclairée et de continuer en conséquence.

> [!NOTE]
>  Pour les sites non approuvés (c’est-à-dire les sites qui ne sont pas répertoriés dans la liste zone de sites de confiance), si le fournisseur est également non approuvé (comme indiqué plus haut dans cette section), l’utilisateur peut voir deux avertissements de sécurité dans une ligne, un avertissement concernant le fournisseur unsafe et un deuxième avertissement concernant la tentative d’utilisation de son identité. Si l’utilisateur clique sur OK jusqu’au premier avertissement, les paramètres d’Internet Explorer et le code de comportement de la réponse décrits dans le tableau précédent sont exécutés.

## <a name="controlling-whether-password-text-is-returned-in-ado-connection-strings"></a>Contrôler si le texte du mot de passe est retourné dans les chaînes de connexion ADO
 Lorsque vous essayez d’extraire la valeur de la propriété [ConnectionString](../reference/ado-api/connectionstring-property-ado.md) sur un objet de **connexion** ADO, les événements suivants se produisent :

1.  Si la connexion est ouverte, un appel d’initialisation est ensuite effectué sur le fournisseur de OLE DB sous-jacent pour recevoir la chaîne de connexion.

2.  En fonction du paramètre du fournisseur OLE DB de la propriété [DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO](/previous-versions/windows/desktop/ms714905(v=vs.85)) , les mots de passe sont inclus avec d’autres informations de chaîne de connexion retournées.

 Par exemple, si la propriété dynamique ADO Connection **Persist Security Info** a la valeur **true**, les informations de mot de passe sont incluses dans la chaîne de connexion retournée. Sinon, si le fournisseur sous-jacent a défini la propriété sur **false** (par exemple, avec le fournisseur SQLOLEDB), les informations de mot de passe sont omises dans la chaîne de connexion retournée.

 Si vous utilisez des fournisseurs tiers (autrement dit, non-Microsoft) OLE DB des fournisseurs avec votre code d’application ADO, vous pouvez vérifier comment la propriété **DBPROP_AUTH_PERSIST_SENSITIVE_AUTHINFO** est implémentée pour déterminer si l’inclusion d’informations de mot de passe avec des chaînes de connexion ADO est autorisée.

## <a name="checking-for-non-file-devices-when-loading-and-saving-recordsets-or-streams"></a>Recherche de périphériques non-fichiers lors du chargement et de l’enregistrement des jeux d’enregistrements ou des flux
 Pour ADO 2,7 et versions antérieures, les opérations d’entrée/sortie de fichier telles que [Open](../reference/ado-api/open-method-ado-recordset.md) et [Save](../reference/ado-api/save-method.md) qui étaient utilisées pour lire et écrire des données basées sur des fichiers pouvaient, dans certains cas, permettre l’utilisation d’une URL ou d’un nom de fichier qui spécifiait un type de fichier qui n’est pas basé sur le disque. Par exemple, LPT1, COM2, PRN.TXT, aux peuvent être utilisés comme alias pour l’entrée/sortie entre les imprimantes et les périphériques auxiliaires sur le système en utilisant certains

 Pour ADO 2,8 et versions ultérieures, cette fonctionnalité a été mise à jour. Pour ouvrir et enregistrer des objets **Recordset** et **Stream** , ADO effectue désormais un contrôle de type de fichier pour s’assurer que l’appareil d’entrée ou de sortie spécifié dans une URL ou un nom de fichier est un fichier réel.

> [!NOTE]
>  La vérification des types de fichiers, comme décrit dans cette section, s’applique uniquement à Windows 2000 et versions ultérieures. Elle ne s’applique pas aux situations où ADO 2,8 ou version ultérieure s’exécute sous des versions antérieures de Windows, telles que Windows 98.