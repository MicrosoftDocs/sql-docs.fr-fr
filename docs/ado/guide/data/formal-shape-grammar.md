---
description: Grammaire formelle de la commande SHAPE
title: Grammaire de forme formelle | Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO], shape grammar
- data shaping [ADO], shape grammar
ms.assetid: ea691475-0f03-4abe-a785-b77e77712d1d
author: rothja
ms.author: jroth
ms.openlocfilehash: 6a8d92abc3a1b0d7e6d39ac4149c186c5a2fc2eb
ms.sourcegitcommit: e700497f962e4c2274df16d9e651059b42ff1a10
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88453371"
---
# <a name="formal-shape-grammar"></a>Grammaire formelle de la commande SHAPE
Il s’agit de la grammaire formelle pour créer n’importe quelle commande de forme :  
  
-   Les termes de grammaire requis sont des chaînes de texte délimitées par des crochets pointus (« <> »).  
  
-   Les termes facultatifs sont délimités par des crochets ("[]").  
  
-   Les alternatives sont indiquées par un virgule (« &#124; »).  
  
-   Les alternatives répétées sont indiquées par des points de suspension (« ... »).  
  
-   *Alpha* indique une chaîne de lettres alphabétiques.  
  
-   *Digit* indique une chaîne de nombres.  
  
-   *Unicode-digit* indique une chaîne de chiffres Unicode.  
  
 Tous les autres termes sont des littéraux.  
  
|Terme|Définition|  
|----------|----------------|  
|\<shape-command>|SHAPE [ \<table-exp> [[] \<alias> ]] [ \<shape-action> ]|  
|\<table-exp>|{ \<provider-command-text> } &#124;<br /><br /> ( \<shape-command> ) &#124;<br /><br /> &#124; de TABLE \<quoted-name><br /><br /> \<quoted-name>|  
|\<shape-action>|Ajouter \<aliased-field-list> &#124;<br /><br /> Compute \<aliased-field-list> [by \<field-list> ]|  
|\<aliased-field-list>|\<aliased-field> [, \<aliased-field...>]|  
|\<aliased-field>|\<field-exp> [[AS] \<alias> ]|  
|\<field-exp>|( \<relation-exp> ) &#124;<br /><br /> \<calculated-exp> &#124;<br /><br /> \<aggregate-exp> &#124;<br /><br /> \<new-exp>|  
|<relation_exp>|\<table-exp> [[AS] \<alias> ]<br /><br /> SE rapportent \<relation-cond-list>|  
|\<relation-cond-list>|\<relation-cond> [, \<relation-cond>...]|  
|\<relation-cond>|\<field-name> À \<child-ref>|  
|\<child-ref>|\<field-name> &#124;<br /><br /> PARAMÈTRE \<param-ref>|  
|\<param-ref>|\<number>|  
|\<field-list>|\<field-name> [, \<field-name>]|  
|\<aggregate-exp>|SUM ( \<qualified-field-name> ) &#124;<br /><br /> MOYENNE ( \<qualified-field-name> ) &#124;<br /><br /> &#124; MIN ( \<qualified-field-name> )<br /><br /> MAX ( \<qualified-field-name> ) &#124;<br /><br /> &#124; de nombre ( \<qualified-alias> &#124; \<qualified-name> )<br /><br /> ECARTYPE ( \<qualified-field-name> ) &#124;<br /><br /> ANY ( \<qualified-field-name> )|  
|\<calculated-exp>|CALC ( \<expression> )|  
|\<qualified-field-name>|\<alias>.[\<alias>...]\<field-name>|  
|\<alias>|\<quoted-name>|  
|\<field-name>|\<quoted-name> [[AS] \<alias> ]|  
|\<quoted-name>|\<string>&#124;<br /><br /> « \<string> » &#124;<br /><br /> [ \<string> ] &#124;<br /><br /> \<name>|  
|\<qualified-name>|alias [. alias...]|  
|\<name>|alpha [chiffre &#124; alpha &#124; _ &#124; # &#124; : &#124;...]|  
|\<number>|chiffre [chiffre...]|  
|\<new-exp>|NOUVEAU \<field-type> [( \<number> [, \<number> ])]|  
|\<field-type>|Type de données OLE DB ou ADO.|  
|\<string>|Unicode-Char [Unicode-char...]|  
|\<expression>|Expression Visual Basic pour Applications dont les opérandes sont d’autres colonnes non CALCULées dans la même ligne.|  
  
## <a name="see-also"></a>Voir aussi  
 [Accès aux lignes d’un jeu d’enregistrements hiérarchique](../../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md)   
 [Présentation de la mise en forme des données](../../../ado/guide/data/data-shaping-overview.md)   
 [Fournisseurs requis pour la mise en forme des données](../../../ado/guide/data/required-providers-for-data-shaping.md)   
 [Clause APPEND de la forme](../../../ado/guide/data/shape-append-clause.md)   
 [Mettre en forme les commandes en général](../../../ado/guide/data/shape-commands-in-general.md)   
 [Shape Compute, clause](../../../ado/guide/data/shape-compute-clause.md)   
 [Fonctions Visual Basic pour Applications](../../../ado/guide/data/visual-basic-for-applications-functions.md)
