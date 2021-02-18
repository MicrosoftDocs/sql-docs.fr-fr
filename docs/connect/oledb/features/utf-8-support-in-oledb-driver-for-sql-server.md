---
title: Prise en charge d’UTF-8 dans le pilote OLE DB pour SQL Server | Microsoft Docs
description: En savoir plus sur la prise en charge d’OLE DB Driver pour SQL Server pour l’encodage du serveur UTF-8.
ms.custom: ''
ms.date: 05/25/2020
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
ms.reviewer: v-kaywon
ms.author: v-daenge
author: David-Engel
ms.openlocfilehash: 063021cfbd1b8668c18c3f0bddfe2c716a790467
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100352342"
---
# <a name="utf-8-support-in-ole-db-driver-for-sql-server"></a>Prise en charge d’UTF-8 dans OLE DB Driver pour SQL Server
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

Le pilote Microsoft OLE DB pour SQL Server (version 18.2.1) prend en charge l’encodage de serveur UTF-8. Pour plus d’informations sur la prise en charge UTF-8 SQL Server, voir :
- [Prise en charge d'Unicode et du classement](../../../relational-databases/collations/collation-and-unicode-support.md)
- [Prise en charge d’UTF-8](../../../relational-databases/collations/collation-and-unicode-support.md#utf8)

La version 18.4.0 du pilote ajoute la prise en charge de l’encodage du client UTF-8 (activé avec la case à cocher « Use Unicode UTF-8 for worldwide language support » dans les paramètres régionaux de Windows 10).

> [!NOTE]  
> Microsoft OLE DB Driver pour SQL Server utilise la fonction [GetACP](/windows/win32/api/winnls/nf-winnls-getacp) pour déterminer l’encodage de la mémoire tampon d’entrée DBTYPE_STR.
>
> Les scénarios dans lesquels GetACP retourne un encodage UTF-8 (activé avec la case à cocher « Use Unicode UTF-8 for worldwide language support » dans les paramètres régionaux de Windows 10) sont pris en charge à partir de la version 18.4. Dans les versions précédentes, si la mémoire tampon doit stocker des données Unicode, le type de données de la mémoire tampon doit être défini sur *DBTYPE_WSTR* (codé en UTF-16).

## <a name="data-insertion-into-a-utf-8-encoded-char-or-varchar-column"></a>Insertion de données dans une colonne CHAR ou VARCHAR encodée UTF-8
Si vous créez une mémoire tampon de paramètres d’entrée pour l’insertion, elle est décrite à l’aide d’un tableau de [structures DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Chaque structure DBBINDING associe un seul paramètre à la mémoire tampon du consommateur et contient différentes informations, comme la longueur et le type de la valeur de données. Pour une mémoire tampon de paramètres d’entrée de type CHAR, le *wType* de la structure DBBINDING doit être défini sur DBTYPE_STR. Pour une mémoire tampon de paramètres d’entrée de type WCHAR, le *wType* de la structure DBBINDING doit être défini sur DBTYPE_WSTR.

Lorsque vous exécutez une commande comportant des paramètres, le pilote construit les informations de type de données de ces derniers. Si le type de la mémoire tampon d’entrée correspond à celui des données des paramètres, aucune conversion n’est effectuée dans le pilote. Sinon, il convertit la mémoire tampon des paramètres d’entrée dans le type de données des paramètres. L’utilisateur peut définir explicitement le type de données des paramètres en appelant [ICommandWithParameters::SetParameterInfo](/previous-versions/windows/desktop/ms725393(v=vs.85)). Si cette information n’est pas fournie, le pilote la dérive en (a) récupérant les métadonnées de colonne auprès du serveur lorsque l’instruction est préparée ou (b) en tentant une conversion par défaut à partir du type de données des paramètres d’entrée.

La mémoire tampon des paramètres d’entrée peut être convertie dans le classement de colonne du serveur par le pilote ou par le serveur en fonction du type de données de la mémoire tampon d’entrée et des paramètres. Lors de la conversion, il y a un risque de perte de données si la page de codes du client ou celle du classement de base de données ne peut pas représenter tous les caractères de la mémoire tampon d’entrée. Le tableau suivant décrit le processus de conversion en cas d’insertion de données dans une colonne compatible UTF-8 :

|Type de données de la mémoire tampon|Type de données de paramètre|Conversion|Précaution de l’utilisateur|
|---             |---                |---       |---            |
|DBTYPE_STR|DBTYPE_STR|Conversion de serveur de la page de codes du client à celle du classement de base de données ; conversion de serveur de la page de codes du classement de base de données à celle du classement de colonne.|Vérifiez que la page de codes du client et celle du classement de base de données peuvent représenter tous les caractères des données d’entrée. Par exemple, pour insérer un caractère polonais, la page de codes du client peut être définie sur 1250 (ANSI Europe centrale) ; le classement de base de données peut utiliser le polonais comme indicateur de classement (par exemple, Polish_100_CI_AS_SC) ou être compatible UTF-8.|
|DBTYPE_STR|DBTYPE_WSTR|Conversion de pilote de la page de codes du client à l’encodage UTF-16 ; conversion de serveur de l’encodage UTF-16 à la page de codes du classement de colonne.|Vérifiez que la page de codes du client peut représenter tous les caractères des données d’entrée. Par exemple, pour insérer un caractère polonais, la page de codes du client peut être définie sur 1250 (ANSI Europe centrale).|
|DBTYPE_WSTR|DBTYPE_STR|Conversion de pilote de l’encodage UTF-16 à la page de codes du classement de base de données ; conversion de serveur de la page de codes du classement de base de données à celle du classement de colonne.|Vérifiez que la page du classement de base de données peut représenter tous les caractères des données d’entrée. Par exemple, pour insérer un caractère polonais, la page de codes du classement de base de données peut utiliser le polonais comme indicateur de classement (par exemple, Polish_100_CI_AS_SC) ou être compatible UTF-8.|
|DBTYPE_WSTR|DBTYPE_WSTR|Conversion de serveur de l’encodage UTF-16 à la page de codes du classement de colonne.|Aucun.|

## <a name="data-retrieval-from-a-utf-8-encoded-char-or-varchar-column"></a>Extraction de données à partir d’une colonne CHAR ou VARCHAR encodée UTF-8
Si vous créez une mémoire tampon pour les données récupérées, elle est décrite à l’aide d’un tableau de [structures DBBINDING](/previous-versions/windows/desktop/ms716845(v=vs.85)). Chaque structure DBBINDING associe une seule colonne dans la ligne récupérée. Pour récupérer les données de la colonne sous forme de CHAR, définissez le *wType* de la structure sur DBBINDING DBTYPE_STR. Pour récupérer les données de la colonne sous forme de WCHAR, définissez le *wType* de la structure sur DBBINDING DBTYPE_WSTR.

Pour l’indicateur de type DBTYPE_STR de la mémoire tampon, le pilote convertit les données encodées UTF-8 dans l’encodage du client. L’utilisateur doit vérifier que cet encodage peut représenter les données de la colonne UTF-8 ; sinon, il y a un risque de perte de données.

Pour l’indicateur de type DBTYPE_WSTR de la mémoire tampon, le pilote convertit les données encodées UTF-8 dans l’encodage UTF-16.

## <a name="communication-with-servers-that-dont-support-utf-8"></a>Communication avec des serveurs qui ne prennent pas en charge UTF-8
Microsoft OLE DB Driver pour SQL Server garantit que les données sont exposées au serveur d’une manière compréhensible par ce dernier. Lors de l’insertion de données à partir de clients compatibles UTF-8, le pilote convertit les chaînes UTF-8 en une page de codes du classement de la base de données avant de les envoyer au serveur.

> [!NOTE]  
> L’utilisation de l’interface [ISequentialStream](/previous-versions/windows/desktop/ms718035(v=vs.85)) pour l’insertion de données encodées en UTF-8 dans une colonne de texte héritée est limitée uniquement aux serveurs qui prennent en charge UTF-8. Pour plus d’informations, consultez [Objets BLOB et OLE](../ole-db-blobs/blobs-and-ole-objects.md).

## <a name="see-also"></a>Voir aussi  
[Fonctionnalités OLE DB Driver pour SQL Server](../../oledb/features/oledb-driver-for-sql-server-features.md) 

[Prise en charge d’UTF-16 dans OLE DB Driver pour SQL Server](../../oledb/features/utf-16-support-in-oledb-driver-for-sql-server.md)    
  
