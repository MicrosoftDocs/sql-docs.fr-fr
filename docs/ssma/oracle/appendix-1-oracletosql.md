---
description: Annexe - 1 (OracleToSQL)
title: Annexe-1 (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: e01f8be5-ce68-4c9f-bd13-d65e73a16470
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: a57da78a0d73d0b5e2bcd7ccd3e514ce37789f3a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058884"
---
# <a name="appendix---1-oracletosql"></a>Annexe - 1 (OracleToSQL)
Affichage rapide des options de ligne de commande de la console SSMA :  
  
|SL. Non.|Commutateur|Nécessaire ?|Argument de commutateur|Valeurs autorisées|  
|-----------|----------|-------------|-------------------|--------------------|  
|1|-s/script|Oui|scriptfile|Nom de fichier XML valide.<br /><br />Fichier de définition de script de console.|  
|2|-v/variable|Non|variablevaluefile|Nom de fichier XML valide.<br /><br />Si des variables sont utilisées dans un fichier de script, ce fichier doit être spécifié.|  
|3|-c/ServerConnection|Non|serverconnectionfile|Nom de fichier XML valide.<br /><br />Ce fichier contient des informations de connexion au serveur.|  
|4|-x/XMLOutput|Non|xmloutputfile|Cette option indique la sortie de la console au format XML. Si cette option n’est pas spécifiée, la sortie par défaut est au format texte.<br /><br />Si xmloutputfile n’est pas spécifié, la sortie XML est dirigée vers `STDOUT` .<br /><br />Xmloutputfile est le nom du fichier dans lequel la sortie de la console est écrite au format XML.|  
|5|-l/log|Non|logfile|Nom de fichier valide.|  
|6|-e/projectenvironment|Non|projectenvironmentfolder|Nom de dossier valide contenant les fichiers d’environnement de projet SSMA.|  
|7|-p/SecurePassword|Non|-a/Add {<server_id> [,... n] &#124; All}-c&#124;ServerConnection <Server-Connection-File> [-v&#124;variable <variable-value-file>] [-o/Overwrite]<br /><br />or<br /><br />-a/Add {<server_id> [,... n] &#124; tous les fichiers de script}-s&#124;<> de fichier de script [-v&#124;variable <variable-valeur-fichier>] [-o/Overwrite]<br /><br />-r/supprimer {<server_id> [,... n] &#124; tout}<br /><br />-l/liste<br /><br />-e/export {<Server-ID> [,... n] &#124; tous} <fichier de mot de passe chiffré><br /><br />-i/import {<Server-ID> [,... n] &#124; tous} <fichier de mot de passe chiffré>|S’il est spécifié, cette option ne doit pas être combinée avec d’autres options.<br /><br />Server-ID : ID unique fourni pour un serveur {String}<br /><br />Server-Connection-file : fichier de définition de serveur (serverconnectionfile ou scriptfile).<br /><br />variable-value-file : il s’agit d’un fichier de définition de variable et il est utilisé dans le fichier de connexion de serveur.<br /><br />encrypted-password-file : il s’agit d’un fichier de mots de passe de serveur chiffré à l’aide d’une phrase secrète spécifiée par l’utilisateur.|  
|8|-?|Non|Non applicable|Non applicable|  
  
## <a name="see-also"></a>Voir aussi  
[Exécution de la console SSMA (Oracle)](./executing-the-ssma-console-oracletosql.md)  
