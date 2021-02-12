---
description: Bien démarrer avec la console SSMA pour MySQL (MySQLToSQL)
title: Prise en main avec SSMA pour la console MySQL (MySQLToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- MySQL Console, launching console
- MySQL Console, output conventions
ms.assetid: 218d502c-059f-4d48-9aea-61e553d74303
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 4c4a7e894052ec9799039d45ff5bb9e2aa29a23b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100078210"
---
# <a name="getting-started-with-ssma-for-mysql-console-mysqltosql"></a>Bien démarrer avec la console SSMA pour MySQL (MySQLToSQL)
Cette section décrit la procédure de lancement et de prise en main de l’application de console MySQL. Les conventions utilisées dans une fenêtre de sortie de console SSMA standard sont également répertoriées.  
  
## <a name="launching-ssma-console"></a>Lancement de la console SSMA  
Pour démarrer l’application de console SSMA, procédez comme suit :  
  
1.  Accédez à **Démarrer** et pointez sur **tous les programmes**.  
  
2.  Cliquez sur le raccourci **d’invite de commandes Assistant Migration SQL Server pour MySQL** .  
  
    Elle affiche le menu de l’utilisation de la console SSMA et `(/? Help)` , pour vous aider à prendre en main l’application console.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procédure d’utilisation de la console SSMA  
Une fois que la console est correctement lancée sur votre système Windows, vous pouvez utiliser les étapes suivantes pour y travailler :  
  
1.  Configurez la console SSMA à l’aide des fichiers de script. Pour plus d’informations sur cette section, consultez [création de fichiers de Script &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-script-files-mysqltosql.md) .  
  
2.  [Création de fichiers de valeurs de variables &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-variable-value-files-mysqltosql.md)  
  
3.  [Création des fichiers de connexion au serveur &#40;MySQLToSQL&#41;](../../ssma/mysql/creating-the-server-connection-files-mysqltosql.md)  
  
4.  [Exécution de la console SSMA &#40;MySQLToSQL&#41;](../../ssma/mysql/executing-the-ssma-console-mysqltosql.md) en fonction des besoins de votre projet  
  
Fonctionnalités supplémentaires :  
  
1.  [Sécurisation des mots de passe](managing-passwords-mysqltosql.md) et exportation/importation sur d’autres ordinateurs Windows  
  
2.  [Générez des rapports](generating-reports-mysqltosql.md) pour afficher les rapports de sortie XML détaillés pour l’évaluation des/conversion et de la migration des données. Des rapports d’erreurs détaillés peuvent également être générés pour les commandes d’actualisation et de synchronisation.  
  
## <a name="ssma-console-output-conventions"></a>Conventions de sortie de la console SSMA  
Lors de l’exécution des commandes de script SSMA et des options, le programme de console affiche les résultats et les messages (informations, erreurs, etc.) à l’utilisateur sur la console ou, si nécessaire, redirige vers un fichier de sortie XML. Chaque type de message dans la sortie est signifié par une couleur unique. Par exemple, le message texte en blanc indique les commandes du fichier de script. la couleur verte représente une invite pour les entrées utilisateur, et ainsi de suite.  
  
![Capture d’écran montrant un exemple de sortie de la console MySQL de la console SSMA.](../../ssma/mysql/media/ssmaconsoleoutput_mysql.jpg "SSMAConsoleOutput_MySQL")  
  
Interprétation des couleurs de la sortie de la console dans le tableau suivant :  
  
|Couleur|Description|  
|---------|---------------|  
|Rouge|Erreur irrécupérable lors de l’exécution|  
|Gris|Date et heure, message à l’utilisateur|  
|White|Commandes de fichier de script, type de message|  
|Yellow|Avertissement|  
|Vert|Demander une entrée utilisateur|  
|Cyan|Début, fin et résultat d’une opération|  
  
## <a name="see-also"></a>Voir aussi  
[Installation de SSMA pour MySQL](installing-ssma-for-mysql-mysqltosql.md)  
  
