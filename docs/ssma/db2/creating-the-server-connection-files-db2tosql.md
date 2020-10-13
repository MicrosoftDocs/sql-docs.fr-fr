---
description: Création des fichiers de connexion au serveur (DB2ToSQL)
title: Création des fichiers de connexion au serveur (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 07/14/2020
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 685419f6-8606-462c-be12-8bace45bede6
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 754f9002dd5b9e8f9111dce59bc81417c704e40d
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984915"
---
# <a name="creating-the-server-connection-files-db2tosql"></a>Création des fichiers de connexion au serveur (DB2ToSQL)

Les informations sur le serveur peuvent être spécifiées dans la section serveurs du fichier de script ou dans un fichier de connexion au serveur distinct. Le paramètre de ligne de commande du fichier de connexion au serveur est, `-c <serverconnectionfile>` . Si le même ID de serveur est présent dans le fichier de script et le fichier de connexion au serveur, la définition du serveur dans le fichier de script est prise en compte.

**Exemple : 1**

```xml
<!-- Sample of server connection file commands -->
<db2 name="<source-server-unique-name>">
  <standard-mode>

    <connection-provider value="OleDB Provider" />

    <!-- Defines server manager to connect.
         Available manager attribute values
           • zOs - DB2 for z/OS
           • LUW - DB2 for Linux, Unix and Windows
           • DBi - DB2 for i -->
    <database-manager value="$Db2Manager$" />

    <server-name value="$Db2HostName$" />

    <port value="$Db2Port$" />

    <initial-catalog value="$Db2Instance$" />

    <user-id value="$Db2UserName$" />

    <password value="$Db2Password$"/>

  </standard-mode>
</db2>
```

```xml
<sql-server name="<target-server-unique-name>">
  <sql-server-authentication>

    <server value="<server-name>" />

    <database value="<database-name>" />
  
    <user-id value="<user-name>"/>
  
    <password value="<password>"/>

  </sql-server-authentication>
</sql-server>
```

## <a name="next-step"></a>étape suivante

L’étape suivante de l’utilisation de la console est l' [exécution de la console SSMA &#40;DB2ToSQL&#41;](../../ssma/db2/executing-the-ssma-console-db2tosql.md)

## <a name="see-also"></a>Voir aussi

- [Exécution de la console SSMA](./executing-the-ssma-console-db2tosql.md)