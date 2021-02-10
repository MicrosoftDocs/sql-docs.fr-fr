---
description: Prise en main avec SSMA pour Access console (AccessToSQL)
title: Prise en main avec SSMA pour Access console (AccessToSQL) | Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8585ec16-7e0a-483a-b250-adab9b9232a3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d39f130c2b2ff8edd95fb52a78b7865b5c6dc868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044509"
---
# <a name="getting-started-with-ssma-for-access-console-accesstosql"></a>Prise en main avec SSMA pour Access console (AccessToSQL)
Cette section décrit la procédure de lancement et de prise en main de l’application console Access. Les conventions utilisées dans une fenêtre de sortie de console SSMA standard sont également répertoriées.  
  
## <a name="launching-ssma-console"></a>Lancement de la console SSMA  
Pour démarrer l’application de console SSMA, procédez comme suit :  
  
1.  Accédez à **Démarrer** et pointez sur **tous les programmes**.  
  
2.  Cliquez sur le raccourci **d’invite de commandes Assistant Migration SQL Server pour accéder** à.  
  
    Elle affiche le menu de l’utilisation de la console SSMA et `(/? Help)` , pour vous aider à prendre en main l’application console.  
  
## <a name="procedure-for-using-the-ssma-console"></a>Procédure d’utilisation de la console SSMA  
Une fois que la console est correctement lancée sur votre système Windows, vous pouvez utiliser les étapes suivantes pour y travailler :  
  
1.  Configurez la console SSMA à l’aide des fichiers de script. Pour plus d’informations sur cette section, consultez [création de fichiers de Script &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md).  
  
2.  [Création de fichiers de valeurs de variables &#40;AccessToSQL&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md)  
  
3.  [Création des fichiers de connexion au serveur &#40;AccessToSQL&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
4.  [Exécution de la console SSMA &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md) en fonction des besoins de votre projet  
  
Fonctionnalités supplémentaires :  
  
1.  [Spécifiez un mot de passe](managing-passwords-accesstosql.md) et exportez-le sur d’autres ordinateurs Windows  
  
2.  [Générez des rapports](generating-reports-accesstosql.md) pour afficher les rapports de sortie XML détaillés pour l’évaluation des/conversion et de la migration des données. Des rapports d’erreurs détaillés peuvent également être générés pour les commandes d’actualisation et de synchronisation.  
  
## <a name="ssma-console-output-conventions"></a>Conventions de sortie de la console SSMA  
Lors de l’exécution des commandes de script SSMA et des options, le programme de console affiche les résultats et les messages (informations, erreurs, etc.) à l’utilisateur sur la console ou, si nécessaire, redirige vers un fichier de sortie XML. Chaque type de message dans la sortie est signifié par une couleur unique. Par exemple, le message texte en blanc indique les commandes du fichier de script. la couleur verte représente une invite pour les entrées utilisateur, et ainsi de suite.  
  
![Sortie de console SSMA](../../ssma/access/media/ssmaconsoleoutput.jpg "Sortie de console SSMA")  
  
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
[Installation de Assistant Migration SQL Server pour l’accès](installing-sql-server-migration-assistant-for-access-accesstosql.md)  
  
