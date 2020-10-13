---
description: Annexe-1 (DB2ToSQL)
title: Annexe-1 (DB2ToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: c6a30367-d56f-4fcc-8920-c6a6b0335a67
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 9b1d9148572823e8cd274fe2812e46b2a84320a3
ms.sourcegitcommit: a41e1f4199785a2b8019a419a1f3dcdc15571044
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91988155"
---
# <a name="appendix---1-db2tosql"></a>Annexe-1 (DB2ToSQL)
Affichage rapide des options de ligne de commande de la console SSMA :  
  
|SL. Non.|Basculer|Requis ?|Argument de commutateur|Valeurs autorisées|  
|-----------|----------|-------------|-------------------|--------------------|  
|1|-s/script|Oui|scriptfile|Nom de fichier XML valide.<br /><br />Fichier de définition de script de console.|  
|2|-v/variable|Non|variablevaluefile|Nom de fichier XML valide.<br /><br />Si des variables sont utilisées dans un fichier de script, ce fichier doit être spécifié.|  
|3|-c/ServerConnection|Non|serverconnectionfile|Nom de fichier XML valide.<br /><br />Ce fichier contient des informations de connexion au serveur.|  
|4|-x/XMLOutput|Non|xmloutputfile|Cette option indique la sortie de la console au format XML. Si cette option n’est pas spécifiée, la sortie par défaut est au format texte.<br /><br />Si xmloutputfile n’est pas spécifié, la sortie XML est dirigée vers STDOUT.<br /><br />Xmloutputfile est le nom du fichier dans lequel la sortie de la console est écrite au format XML.|  
|5|-l/log|Non|logfile|Nom de fichier valide.|  
|6|-e/projectenvironment|Non|projectenvironmentfolder|Nom de dossier valide contenant les fichiers d’environnement de projet SSMA.|  
|7|-p/SecurePassword|Non|-a/Add {<server_id> [,... n] &#124; All}-c&#124;ServerConnection <Server-Connection-File> [-v&#124;variable <variable-value-file>] [-o/Overwrite]<br /><br />or<br /><br />-a/Add {<server_id> [,... n] &#124; tous les fichiers de script}-s&#124;<> de fichier de script [-v&#124;variable <variable-valeur-fichier>] [-o/Overwrite]<br /><br />-r/supprimer {<server_id> [,... n] &#124; tout}<br /><br />-l/liste<br /><br />-e/export {<Server-ID> [,... n] &#124; tous} <fichier de mot de passe chiffré><br /><br />-i/import {<Server-ID> [,... n] &#124; tous} <fichier de mot de passe chiffré>|S’il est spécifié, cette option ne doit pas être combinée avec d’autres options.<br /><br />Server-ID : ID unique fourni pour un serveur {String}<br /><br />Server-Connection-file : fichier de définition de serveur (serverconnectionfile ou scriptfile).<br /><br />variable-value-file : il s’agit d’un fichier de définition de variable et il est utilisé dans le fichier de connexion de serveur.<br /><br />encrypted-password-file : il s’agit d’un fichier de mots de passe de serveur chiffré à l’aide d’une phrase secrète spécifiée par l’utilisateur.|  
|8|-?|Non|Non applicable|Non applicable|  
  
## <a name="see-also"></a>Voir aussi  
[Exécution de la console SSMA](./executing-the-ssma-console-db2tosql.md)  
