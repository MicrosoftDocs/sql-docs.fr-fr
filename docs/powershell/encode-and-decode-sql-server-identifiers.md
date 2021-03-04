---
title: Encoder et décoder des identificateurs SQL Server
description: Certains caractères qui peuvent s’afficher dans les identificateurs délimités de SQL Server ne sont pas pris en charge dans les chemins d’accès Windows PowerShell. Découvrez comment les inclure en les représentant avec leurs valeurs hexadécimales.
ms.prod: sql
ms.technology: sql-server-powershell
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: matteot, drskwier
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: 13b32a62791aaa932311223f4f648d8006eb33e7
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/04/2021
ms.locfileid: "101838033"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Encoder et décoder des identificateurs SQL Server

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les identificateurs délimités SQL Server contiennent parfois des caractères non pris en charge dans les chemins Windows PowerShell. Vous pouvez spécifier ces caractères en encodant leurs valeurs hexadécimales.

[!INCLUDE [sql-server-powershell-version](../includes/sql-server-powershell-version.md)]

Les caractères qui ne sont pas pris en charge dans les noms de chemins d'accès Windows PowerShell peuvent être représentés, ou codés, sous la forme du caractère « % » suivi de la valeur hexadécimale pour le modèle binaire qui représente le caractère, comme dans « **%** xx ». Le codage peut toujours être utilisé pour gérer des caractères qui ne sont pas pris en charge dans les chemins d'accès Windows PowerShell.

L’applet de commande **Encode-SqlName** prend comme entrée un identificateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Elle génère une chaîne contenant tous les caractères qui ne sont pas pris en charge par le langage Windows PowerShell codés avec « % xx ». L’applet de commande **Decode-SqlName** prend comme entrée un identificateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] encodé et retourne l’identificateur d’origine.  

## <a name="limitations-and-restrictions"></a>Limitations et restrictions

Les applets de commande **Encode-Sqlname** et **Decode-Sqlname** encodent ou décodent uniquement les caractères autorisés dans les identificateurs délimités SQL Server, mais ne sont pas prises en charge dans les chemins PowerShell. Voici les caractères encodés par **Encode-SqlName** et décodés par **Decode-SqlName** :

|||||||||||||
|-|-|-|-|-|-|-|-|-|-|-|-|
|**Caractère**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|**Encodage hexadécimal**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|

## <a name="encoding-an-identifier"></a>Encodage d'un identificateur  

### <a name="to-encode-a-sql-server-identifier-in-a-powershell-path"></a>Pour encoder un identificateur SQL Server dans un chemin d'accès PowerShell

- Utilisez l'une des deux méthodes suivantes pour encoder un identificateur SQL Server :
    - Spécifiez le code hexadécimal du caractère non pris en charge à l'aide de la syntaxe %XX, où XX est le code hexadécimal.
    - Passez l’identificateur en tant que chaîne entre guillemets à l’applet de commande **Encode-Sqlname**

### <a name="examples-encoding"></a>Exemples (encodage)

Cet exemple spécifie la version encodée du caractère « : » (%3A) :

```powershell
Set-Location Table%3ATest
```

Vous pouvez également utiliser **Encode-SqlName** pour générer un nom pris en charge par Windows PowerShell :

```powershell
Set-Location (Encode-SqlName "Table:Test")
```

## <a name="decoding-an-identifier"></a>Décodage d'un identificateur

### <a name="to-decode-a-sql-server-identifier-from-a-powershell-path"></a>Pour décoder un identificateur SQL Server à partir d'un chemin d'accès PowerShell

Utilisez l’applet de commande **Decode-Sqlname** pour remplacer les encodages hexadécimaux par les caractères représentés par l’encodage.

### <a name="examples-decoding"></a>Exemples (décodage)

Cet exemple retourne « Table:Test » :

```powershell
Decode-SqlName "Table%3ATest"
```

## <a name="see-also"></a>Voir aussi

- [Identificateurs SQL Server dans PowerShell](sql-server-identifiers-in-powershell.md)
- [Fournisseur SQL Server PowerShell](sql-server-powershell-provider.md)
- [SQL Server PowerShell](sql-server-powershell.md)  
