---
description: Installation du logiciel (ODBC)
title: Installation du logiciel (ODBC) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ODBC driver for Oracle [ODBC], installing
- installing ODBC driver for Oracle [ODBC]
ms.assetid: dfac8ade-eebe-4ebe-a199-feb740ed5bae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: fe161ef45ef91f67317d7a0b465e00d3d2045c40
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88449471"
---
# <a name="installing-the-software-odbc"></a>Installation du logiciel (ODBC)
> [!IMPORTANT]  
>  Cette fonctionnalité sera supprimée dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. Utilisez plutôt le pilote ODBC fourni par Oracle.  
  
 Le pilote ODBC pour Oracle est l’un des composants d’accès aux données. Il accompagne d’autres composants ODBC, tels que l’administrateur de la source de données ODBC, et doit déjà être installé. Le pilote se trouve également sous « pilotes et autres téléchargements » sur le site Web Microsoft Product Support Services (en anglais) sur [www.Microsoft.com](https://www.microsoft.com).  
  
 Le logiciel réseau doit être installé conformément à sa propre documentation. Le pilote ODBC pour Oracle ne nécessite pas de considérations particulières relatives à l’installation tant que le logiciel réseau est pris en charge.  
  
 Le logiciel Oracle doit être installé conformément à sa propre documentation. Le pilote ODBC pour Oracle ne requiert généralement pas de considérations particulières relatives à l’installation tant que le pilote prend en charge la version. Toutefois, pour maintenir la compatibilité des produits, installez le pilote ODBC pour Oracle en dernier pour vous assurer que vous disposez de la dernière version du pilote. Oracle gère un site FTP public sur lequel il publie, entre autres, des correctifs pour les produits serveur Oracle et le composant client fourni avec les produits serveur. Ces correctifs sont requis pour le bon fonctionnement de plusieurs produits et technologies Microsoft. Pour plus d’informations sur ce site, consultez [correctifs logiciels Oracle](../../odbc/microsoft/oracle-software-patches.md).  
  
> [!CAUTION]  
>  L’installation du logiciel Oracle sur MDAC/Windows DAC peut remplacer les versions actuelles de MDAC. Si des problèmes surviennent lors de l’utilisation de composants ODBC, réinstallez MDAC.
