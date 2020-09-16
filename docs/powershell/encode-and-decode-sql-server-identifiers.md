---
title: Encoder et décoder des identificateurs SQL Server | Microsoft Docs
description: Certains caractères qui peuvent s’afficher dans les identificateurs délimités de SQL Server ne sont pas pris en charge dans les chemins d’accès Windows PowerShell. Découvrez comment les inclure en les représentant avec leurs valeurs hexadécimales.
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: sql-server-powershell
ms.topic: conceptual
ms.assetid: bb9fe0d3-e432-42d3-b324-64dc908b544a
author: markingmyname
ms.author: maghan
ms.openlocfilehash: 51e4caad8209f84a1ab0f1db054431c37cd10598
ms.sourcegitcommit: a9f16d7819ed0e2b7ad8f4a7d4d2397437b2bbb2
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88714057"
---
# <a name="encode-and-decode-sql-server-identifiers"></a>Encoder et décoder des identificateurs SQL Server
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

Les identificateurs délimités SQL Server contiennent parfois des caractères non pris en charge dans les chemins Windows PowerShell. Vous pouvez spécifier ces caractères en encodant leurs valeurs hexadécimales.  

> [!NOTE]
> Il existe deux modules SQL Server PowerShell : **SqlServer** et **SQLPS**. Le module **SQLPS** fait partie de l’installation de SQL Server (à des fins de compatibilité descendante), mais il n’est plus mis à jour. Le module PowerShell le plus récent est **SqlServer**. Le module **SqlServer** contient les versions mises à jour des applets de commande disponibles dans **SQLPS**, ainsi que de nouvelles applets de commande pour prendre en charge les dernières fonctionnalités SQL.  
> Des versions précédentes du module **SqlServer** *étaient* fournies avec SQL Server Management Studio (SSMS), mais uniquement avec les versions 16.x de SSMS. Pour utiliser PowerShell avec SSMS 17.0 et ultérieur, vous devez installer le module **SqlServer** à partir de PowerShell Gallery.
> Pour installer le module **SqlServer**, consultez [Installer SQL Server PowerShell](download-sql-server-ps-module.md).
  
  
Les caractères qui ne sont pas pris en charge dans les noms de chemins d'accès Windows PowerShell peuvent être représentés, ou codés, sous la forme du caractère « % » suivi de la valeur hexadécimale pour le modèle binaire qui représente le caractère, comme dans « **%** xx ». Le codage peut toujours être utilisé pour gérer des caractères qui ne sont pas pris en charge dans les chemins d'accès Windows PowerShell.  
  
 L’applet de commande **Encode-SqlName** prend comme entrée un identificateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] . Elle génère une chaîne contenant tous les caractères qui ne sont pas pris en charge par le langage Windows PowerShell codés avec « % xx ». L’applet de commande **Decode-SqlName** prend comme entrée un identificateur [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] encodé et retourne l’identificateur d’origine.  
  
##  <a name="limitations-and-restrictions"></a><a name="LimitationsRestrictions"></a> Limitations et restrictions  
 Les applets de commande **Encode-Sqlname** et **Decode-Sqlname** encodent ou décodent uniquement les caractères autorisés dans les identificateurs délimités SQL Server, mais ne sont pas prises en charge dans les chemins PowerShell. Voici les caractères encodés par **Encode-SqlName** et décodés par **Decode-SqlName** :  
  
|||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|-|  
|**Caractère**|\ |/|:|%|\<|>|*|?|[|]|&#124;|  
|**Encodage hexadécimal**|%5C|%2F|%3A|%25|%3C|%3E|%2A|%3F|%5B|%5D|%7C|  
  
##  <a name="encoding-an-identifier"></a><a name="EncodeIdent"></a> Encodage d'un identificateur  
 **Pour encoder un identificateur SQL Server dans un chemin d'accès PowerShell**  
  
-   Utilisez l'une des deux méthodes suivantes pour encoder un identificateur SQL Server :  
  
    -   Spécifiez le code hexadécimal du caractère non pris en charge à l'aide de la syntaxe %XX, où XX est le code hexadécimal.  
  
    -   Passez l’identificateur en tant que chaîne entre guillemets à l’applet de commande **Encode-Sqlname**  
  
### <a name="examples-encoding"></a>Exemples (encodage)  
 Cet exemple spécifie la version encodée du caractère « : » (%3A) :  
  
```  
Set-Location Table%3ATest  
```  
  
 Vous pouvez également utiliser **Encode-SqlName** pour générer un nom pris en charge par Windows PowerShell :  
  
```  
Set-Location (Encode-SqlName "Table:Test")  
```  
  
##  <a name="decoding-an-identifier"></a><a name="DecodeIdent"></a> Décodage d'un identificateur  
 **Pour décoder un identificateur SQL Server à partir d'un chemin d'accès PowerShell**  
  
 Utilisez l’applet de commande **Decode-Sqlname** pour remplacer les encodages hexadécimaux par les caractères représentés par l’encodage.  
  
### <a name="examples-decoding"></a>Exemples (décodage)  
 Cet exemple retourne « Table:Test » :  
  
```  
Decode-SqlName "Table%3ATest"  
```  
  
## <a name="see-also"></a>Voir aussi  
 [Identificateurs SQL Server dans PowerShell](sql-server-identifiers-in-powershell.md)   
 [fournisseur PowerShell SQL Server](sql-server-powershell-provider.md)   
 [SQL Server PowerShell](sql-server-powershell.md)  
  
  
