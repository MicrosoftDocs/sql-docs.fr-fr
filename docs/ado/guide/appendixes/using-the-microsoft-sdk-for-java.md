---
description: Utilisation du SDK Microsoft pour Java
title: Utilisation du kit de développement logiciel (SDK) Microsoft pour Java | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/08/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- Java (Microsoft SDK for)
- Microsoft SDK for Java [ADO]
ms.assetid: 2d7cb5b5-8307-49dd-b07e-c07069bb1626
author: rothja
ms.author: jroth
ms.openlocfilehash: 13a518a3cbf7c4d9f83da5810e340406147f86f2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028829"
---
# <a name="using-the-microsoft-sdk-for-java"></a>Utilisation du SDK Microsoft pour Java

> [!IMPORTANT]
> Microsoft a supprimé la prise en charge de Visual J++ en janvier 2004.

Le kit de développement logiciel (SDK) Microsoft pour Java est le kit de développement de l’environnement Microsoft Internet Explorer. Des outils, des informations et des exemples sont fournis pour vous aider à développer des programmes et des applets Java basés sur JDK 1,1 et sur la machine virtuelle Microsoft Win32 (Microsoft VM). Le kit de développement logiciel (SDK) Microsoft pour Java n’est pas lié à Microsoft Visual J++. Pour télécharger ce kit de développement logiciel (SDK), cliquez ici.  
  
 L’utilitaire Jactivex.exe génère des classes à partir d’une bibliothèque de types, mais ne peut être appelé que sur la ligne de commande. Cette fonctionnalité n’est pas intégrée à l’environnement de développement Visual J++. Contrairement aux classes générées par l’Assistant bibliothèque de types Java, vous pouvez effectuer un pas à pas détaillé dans les wrappers de classe créés par le kit de développement logiciel (SDK). Cela est utile pour déboguer la façon dont votre code utilise les classes wrapper ADO.  
  
 Ce mécanisme lit la bibliothèque de types ADO et génère des classes que vous pouvez instancier dans votre application. Il génère ces classes à l’emplacement suivant : \\<répertoire Windows \> \Java\trustlib\msado15.  
  
 La création d’une application ADO en Java à l’aide du kit de développement logiciel (SDK) Microsoft pour Java est fondamentalement identique, du point de vue du code source, à l’utilisation de l’Assistant bibliothèque de types Java. Pour obtenir un exemple de code, consultez [wrappers de classe Java ADO](./ado-java-class-wrappers.md). La seule différence réelle réside dans la façon dont vous générez les classes wrapper en premier lieu, comme illustré dans les étapes suivantes.  
  
### <a name="to-create-an-ado-project-with-the-microsoft-sdk-for-java"></a>Pour créer un projet ADO avec le kit de développement logiciel (SDK) Microsoft pour Java  
  
1.  Exécutez la commande suivante à l’invite de commandes. Vous devez définir le chemin d’accès pour inclure le répertoire bin du kit de développement logiciel (SDK) Microsoft pour Java, ou exécuter la commande à partir de cet emplacement. En règle générale, le kit de développement logiciel (SDK) Microsoft pour Java est installé au même emplacement que Visual Studio. Il s’agit d’une instruction de commande unique.  
  
    ```java
    \<path to DevStudio>\<path to Java SDK>\bin\JactiveX.exe /javatlb "C:\program files\common files\system\ado\msado15.dll"  
    ```  
  
2.  Exécutez la commande suivante pour compiler les classes générées. Le commutateur/g : t active la génération de symboles de débogage pour que vous puissiez tracer dans le. Symboles Java. Supprimez-le pour les versions release.  
  
    ```java
    jvc /g:t c:\<windows>\Java\trustlib\msado15\*.Java  
    ```  
  
3.  Pour utiliser ces fichiers, ouvrez votre projet dans Visual J++. Dans le menu **projet** , choisissez **Ajouter au projet**. Sélectionnez **fichiers**, puis ajoutez tout. Fichiers JAVA générés dans le répertoire trustlib\msado15 pour votre projet.  
  
## <a name="see-also"></a>Voir aussi  
 [Wrappers de classe Java ADO](./ado-java-class-wrappers.md)