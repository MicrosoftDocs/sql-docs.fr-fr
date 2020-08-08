---
title: Prise en main avec SSMA pour la console Oracle (OracleToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Console, Console Output Conventions
- Oracle Console, Launching Console
ms.assetid: 667a5e4a-6848-4973-a72d-1287f64718ac
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 57170c17538ccd997c5bc4d2e12ab53914b3727c
ms.sourcegitcommit: e8f6c51d4702c0046aec1394109bc0503ca182f0
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87934888"
---
# <a name="getting-started-with-ssma--for-oracle-console-oracletosql"></a>Bien démarrer avec la console SSMA pour Oracle (OracleToSQL)
Cette section décrit la procédure de lancement et de prise en main de l’application console Oracle. Les conventions utilisées dans une fenêtre de sortie de console SSMA standard sont également répertoriées.  
  
## <a name="launching-ssma-console"></a>Lancement de la console SSMA  
Pour démarrer l’application de console SSMA, procédez comme suit :  
  
1.  Accédez à **Démarrer** et pointez sur **tous les programmes**.  
  
2.  Cliquez sur le raccourci ** [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] d’invite de commandes Assistant Migration pour Oracle** .  
  
    Elle affiche le menu de l’utilisation de la console SSMA et `(/? Help)` , pour vous aider à prendre en main l’application console.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procédure d’utilisation de la console SSMA  
Une fois que la console est correctement lancée sur votre système Windows, vous pouvez utiliser les étapes suivantes pour y travailler :  
  
1.  Configurez la console SSMA à l’aide des fichiers de script. Pour plus d’informations sur cette section, consultez [création de fichiers de Script &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md) .  
  
2.  [Création de fichiers de valeurs de variables &#40;OracleToSQL&#41;](../../ssma/oracle/creating-variable-value-files-oracletosql.md)  
  
3.  [Création des fichiers de connexion au serveur &#40;OracleToSQL&#41;](../../ssma/oracle/creating-the-server-connection-files-oracletosql.md)  
  
4.  [Exécution de la console SSMA &#40;OracleToSQL&#41;](../../ssma/oracle/executing-the-ssma-console-oracletosql.md) en fonction des besoins de votre projet  
  
Fonctionnalités supplémentaires :  
  
1.  [Spécifiez un mot de passe](managing-passwords-oracletosql.md) et exportez-le sur d’autres ordinateurs Windows  
  
2.  [Générez des rapports](generating-reports-oracletosql.md) pour afficher les rapports de sortie XML détaillés pour l’évaluation des/conversion et de la migration des données. Des rapports d’erreurs détaillés peuvent également être générés pour les commandes d’actualisation et de synchronisation.  
  
## <a name="ssma-console-output-conventions"></a>Conventions de sortie de la console SSMA  
Lors de l’exécution des commandes de script SSMA et des options, le programme de console affiche les résultats et les messages (informations, erreurs, etc.) à l’utilisateur sur la console ou, si nécessaire, redirige vers un fichier de sortie XML. Chaque type de message dans la sortie est signifié par une couleur unique. Par exemple, le message texte en blanc indique les commandes du fichier de script. la couleur verte représente une invite pour les entrées utilisateur, et ainsi de suite.  
  
![Output_Oracle de console SSMA](../../ssma/db2/media/ssmaconsoleoutput_oracle.jpg "Output_Oracle de console SSMA")  
  
Interprétation des couleurs de la sortie de la console dans le tableau suivant :  
  
|Couleur|Description|  
|---------|---------------|  
|Rouge|Erreur irrécupérable lors de l’exécution|  
|Gris|Date et heure, message à l’utilisateur|  
|White|Commandes de fichier de script, type de message|  
|Jaune|Avertissement|  
|Vert|Demander une entrée utilisateur|  
|Cyan|Début, fin et résultat d’une opération|  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour Oracle](installing-ssma-for-oracle-oracletosql.md)  
  
