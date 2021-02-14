---
title: Connexion à MySQL (MySQLToSQL) | Microsoft Docs
description: Découvrez comment vous connecter à une base de données iMySQL cible pour migrer une base de données MySQL. SSMA obtient des métadonnées sur les bases de données dans Azure SQL Database.
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Connecting to MySQL, MySQL permission
- Connecting to MySQL,reconnecting
ms.assetid: 084c7020-f729-4f91-90e0-143f85fa68d1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d7f3f46a58fccafa68164d9335c66df10c5b7487
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068894"
---
# <a name="connecting-to-mysql-mysqltosql"></a>Connexion à MySQL (MySQLToSQL)
Pour migrer des bases de données MySQL vers SQL Server ou SQL Azure, vous devez vous connecter à la base de données MySQL que vous souhaitez migrer. Quand vous vous connectez, SSMA obtient des métadonnées sur tous les schémas MySQL, puis les affiche dans le volet Explorateur de métadonnées MySQL. SSMA stocke les informations relatives au serveur de base de données, mais ne stocke pas les mots de passe.  
  
Votre connexion à la base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données.  
  
Les métadonnées relatives à la base de données MySQL ne sont pas automatiquement mises à jour. Au lieu de cela, si vous souhaitez mettre à jour les métadonnées dans l’Explorateur de métadonnées MySQL, vous devez les mettre à jour manuellement. Pour plus d’informations, consultez la section « actualisation des métadonnées MySQL » plus loin dans cette rubrique.  
  
## <a name="required-mysql-permissions"></a>Autorisations MySQL requises  
Le compte utilisé pour se connecter à la base de données MySQL doit disposer au minimum d’autorisations de **connexion** . Cela permet à SSMA d’obtenir des métadonnées à partir de schémas appartenant à l’utilisateur qui se connecte. Pour obtenir les métadonnées des objets dans d’autres schémas, puis convertir les objets dans ces schémas, le compte doit disposer des autorisations suivantes :  
  
-   Privilèges’SHOW’sur les objets de base de données  
  
-   Privilège « SELECT » sur « Information_schema »  
  
-   Privilège « SELECT » sur MySQL (pour les fonctions définies par l’utilisateur)  
  
## <a name="establishing-a-connection-to-mysql"></a>Établissement d’une connexion à MySQL  
Lorsque vous vous connectez à une base de données, SSMA lit les métadonnées de la base de données, puis ajoute ces métadonnées au fichier projet. Ces métadonnées sont utilisées par SSMA lorsqu’il convertit des objets en SQL Server ou SQL Azure syntaxe, et lorsqu’il migre des données vers SQL Server ou SQL Azure. Vous pouvez parcourir ces métadonnées dans le volet Explorateur de métadonnées MySQL et consulter les propriétés des objets de base de données individuels.  
  
> [!IMPORTANT]  
> Avant d’essayer de vous connecter, assurez-vous que le serveur de base de données est en cours d’exécution et peut accepter des connexions.  
  
**Pour se connecter à MySQL**  
  
1.  Dans le menu **fichier** , sélectionnez **se connecter à MySQL** (cette option sera activée après la création du projet).  
  
    Si vous êtes connecté précédemment à MySQL, le nom de la commande sera **reconnecté à MySQL**.  
  
2.  Dans la zone **fournisseur** , sélectionnez pilote ODBC 5,1 MySQL (approuvé). Il s’agit du fournisseur par défaut en mode standard.  
  
3.  Dans la zone **mode** , sélectionnez **mode standard**. Il s'agit du mode par défaut.  
  
    Utilisez le mode standard pour spécifier le nom du serveur et le port.  
  
4.  En **mode standard**, indiquez les valeurs suivantes :  
  
    1.  Dans la zone **nom du serveur** , entrez le nom du serveur MySQL. Dans la zone **port du serveur** , entrez le numéro de port à 3306. Il s’agit du port par défaut.  
  
    2.  Dans la zone **nom d’utilisateur** , entrez un compte MySQL disposant des autorisations nécessaires.  
  
    3.  Dans la zone **mot de passe** , entrez le mot de passe du nom d’utilisateur spécifié.  
  
5.  **SSL :** Si vous souhaitez vous connecter en toute sécurité à MySQL, utilisez SSL (Secure Socket Layer) en activant la case à cocher **SSL** .  
  
6.  **Configurer :** Il fournit une option permettant de configurer la connexion à MySQL via SSL (Secure Socket Layer).  
  
    > [!NOTE]  
    > Pour activer la **configuration**, SSL doit avoir la valeur **true**.  
  
    Lorsque vous cliquez sur le bouton « configurer », une boîte de dialogue s’affiche. Pour utiliser le chiffrement lors de la connexion à la base de données MySQL, le chemin d’accès aux trois fichiers de certificat suivants présents dans la boîte de dialogue doit être défini [Privacy Enhanced Mail les certificats (PEM)] :  
  
    -   **Autorité de certification SSL :** Spécifie le chemin d’accès à un fichier avec une liste d’autorités de certification SSL de confiance.  
  
    -   **Certificat SSL :** Spécifie le nom du fichier de certificat SSL à utiliser pour établir une connexion sécurisée.  
  
    -   **clé SSL :** Spécifie le nom du fichier de clé SSL à utiliser pour établir une connexion sécurisée.  
  
    > [!NOTE]  
    > -   Le bouton **OK** est activé lorsque les informations requises ont été fournies. Si l’un des chemins d’accès aux fichiers n’est pas valide, le bouton « OK » reste désactivé.  
    > -   Le bouton **Annuler** ferme la boîte de dialogue et **désactive** l’option SSL dans le formulaire de connexion principal.  
  
7.  Pour plus d’informations, consultez [se connecter à MySQL &#40;MySQLToSQL&#41;](../../ssma/mysql/connect-to-mysql-mysqltosql.md)  
  
## <a name="reconnecting-to-mysql"></a>Reconnexion à MySQL  
Votre connexion au serveur de base de données reste active jusqu’à ce que vous fermiez le projet. Lorsque vous rouvrez le projet, vous devez vous reconnecter si vous souhaitez une connexion active à la base de données. Vous pouvez travailler hors connexion jusqu’à ce que vous souhaitiez mettre à jour des métadonnées, charger des objets de base de données dans SQL Server ou SQL Azure et migrer des données.  
  
## <a name="refreshing-mysql-metadata"></a>Actualisation des métadonnées MySQL  
Les métadonnées relatives à la base de données MySQL ne sont pas automatiquement actualisées. Dans l’Explorateur de métadonnées MySQL, les métadonnées sont un instantané des métadonnées lorsque vous vous êtes connecté pour la première fois ou lors de la dernière actualisation manuelle des métadonnées. Vous pouvez mettre à jour manuellement les métadonnées de tous les schémas, un seul schéma ou des objets de base de données individuels.  
  
**Pour actualiser les métadonnées**  
  
1.  Assurez-vous que vous êtes connecté à la base de données.  
  
2.  Dans l’Explorateur de métadonnées MySQL, activez la case à cocher en regard de chaque schéma ou objet de base de données que vous souhaitez mettre à jour.  
  
3.  Cliquez avec le bouton droit sur **schémas**, ou sur l’objet de schéma ou de base de données, puis sélectionnez **Actualiser à partir de la base de données**.  
  
    Si vous ne disposez pas d’une connexion active, SSMA affiche la boîte de dialogue **connexion à MySQL** pour vous permettre de vous connecter.  
  
4.  Dans la boîte de dialogue actualiser à partir de la base de données, spécifiez les objets à actualiser.  
  
    -   Pour actualiser un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’une flèche s’affiche.  
  
    -   Pour empêcher l’actualisation d’un objet, cliquez sur le champ **actif** adjacent à l’objet jusqu’à ce qu’un **X** apparaisse.  
  
    -   Pour actualiser ou refuser une catégorie d’objets, cliquez sur le champ **actif** en regard du dossier de catégorie.  
  
    -   Pour afficher les définitions du codage en couleurs, cliquez sur le bouton **légende** .  
  
5.  Cliquez sur **OK**.  
  
## <a name="next-step"></a>étape suivante  
L’étape suivante du processus de migration consiste [à se connecter à SQL Server &#40;MySQLToSQL&#41;](../../ssma/mysql/connecting-to-sql-server-mysqltosql.md)  
  
## <a name="see-also"></a>Voir aussi  
[Migration de bases de données MySQL vers SQL Server Azure SQL Database &#40;MySQLToSql&#41;](../../ssma/mysql/migrating-mysql-databases-to-sql-server-azure-sql-db-mysqltosql.md)  
  
