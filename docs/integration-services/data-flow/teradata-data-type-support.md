---
description: Prise en charge du type de données Teradata
title: Prise en charge des types de données Teradata | Microsoft Docs
ms.custom: ''
ms.date: 11/22/2019
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
author: chugugrace
ms.author: chugu
ms.openlocfilehash: cb58e1d27e955009cd420a089f8d8f938f26c10f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100346520"
---
# <a name="data-type-support"></a>Prise en charge des types de données

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]

Les composants SSIS utilisent l’API Teradata Parallel Transporter (API TPT) pour charger et transférer des données vers et à partir de la base de données Teradata. Ainsi, seul un type de données pris en charge par l’API TPT peut être utilisé dans SSIS.

> [!NOTE]
>
> Les types de données TIME, TIMESTAMP et INTERVAL dans Teradata sont gérés par l’API TPT comme chaînes de caractères de taille fixe. Ils sont gérés comme chaîne par les composants SSIS pour Teradata.

## <a name="data-type-mapping"></a>Mappage de types de données

Le tableau suivant présente les types de données de base de données Teradata et leur mappage par défaut aux types de données SSIS. Il montre également les types de données Teradata non pris en charge.

|Type de données SQL|Type de données SSIS|
|:-|:-|
|DECIMAL/NUMERIC|DT_NUMERIC|
|BYTEINT|DT_I1|
|SMALLINT|DT_I2|
|INTEGER|DT_I4|
|FLOAT/REAL/DOUBLE PRECISION|DT_R8|
|DATE|DT_DBDATE|
|TEMPS<br>TIME(n)|DT_STR|
|timestamp<br>TIMESTAMP (n)|DT_STR|
|TIME WITH TIMEZONE<br>TIME(n) WITH TIMEZONE|DT_STR|
|TIMESTAMP WITH TIMEZONE<br>TIMESTAMP(n) WITH TIMEZONE|DT_STR|
|INTERVAL YEAR<br>INTERVAL YEAR (n)|DT_STR|
|INTERVAL YEAR TO MONTH<br>INTERVAL YEAR (n) TO MONTH|DT_STR|
|INTERVAL MONTH<br>INTERVAL MONTH (n)|DT_STR|
|INTERVAL DAY<br>INTERVAL DAY (n)|DT_STR|
|INTERVAL DAY TO HOUR<br>INTERVAL DAY (n) TO HOUR|DT_STR|
|INTERVAL DAY TO MINUTE<br>INTERVAL DAY (n) TO MINUTE|DT_STR|
|INTERVAL DAY TO SECOND<br>INTERVAL DAY (n) TO SECOND<br>INTERVAL DAY TO SECOND (m)<br>INTERVAL DAY (n) TO SECOND (m)|DT_STR|
|INTERVAL HOUR<br>INTERVAL HOUR (n)|DT_STR|
|INTERVAL HOUR TO MINUTE<br>INTERVAL HOUR (n) TO MINUTE|DT_STR
|INTERVAL HOUR TO SECOND<br>INTERVAL HOUR (n) TO SECOND<br>INTERVAL HOUR TO SECOND (m)<br>INTERVAL HOUR (n) TO SECOND (m)|DT_STR|
|INTERVAL MINUTE<br>INTERVAL MINUTE (n)|DT_STR|
|INTERVAL MINUTE TO SECOND<br>INTERVAL MINUTE (n) TO SECOND<br>INTERVAL MINUTE TO SECOND (m)<br>INTERVAL MINUTE (n) TO SECOND (m)|DT_STR|
|INTERVAL SECOND<br>INTERVAL SECOND (n)<br>INTERVAL SECOND (n,m)|DT_STR|
|PERIOD(DATE)|DT_STR|
|PERIOD(TIME)|DT_STR|
|NUMBER|DT_STR|
|CHARACTER|DT_STR|
|VARCHAR|DT_STR (DT_WSTR pour le jeu de caractères Unicode)<br>**Remarques**:<br> La longueur maximale prise en charge de VARCHAR est 32 000. <br> La longueur maximale autorisée de DT_STR est de 8 000 caractères (4 000 caractères pour DT_WSTR). Les données sont tronquées si la longueur maximale est dépassée.|
|LONG VARCHAR|Non pris en charge|
|CLOB|Non pris en charge|
|BYTE|DT_BYTES<br>**Remarque** : La longueur maximale autorisée est de 8 000 octets. Les données sont tronquées si la longueur maximale est dépassée.|
|VARBYTE|DT_BYTES<br>**Remarque** : La longueur maximale autorisée est de 8 000 octets. Les données sont tronquées si la longueur maximale est dépassée.|
|BLOB|Non pris en charge|

## <a name="next-steps"></a>Étapes suivantes

- Configurer le [gestionnaire de connexions Teradata](teradata-connection-manager.md)
- Configurer la [source Teradata](teradata-source.md)
- Configurer la [destination Teradata](teradata-destination.md)
- Si vous avez des questions, rendez-vous sur [Tech Community](https://aka.ms/AA6iwdw).
