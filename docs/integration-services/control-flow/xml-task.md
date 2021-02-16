---
description: Tâche XML
title: XML, tâche | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.dts.designer.xmltask.f1
- sql13.dts.designer.xmltask.general.f1
helpviewer_keywords:
- XML [Integration Services]
- XML task [Integration Services]
ms.assetid: 9f761846-390e-46d5-9db7-858943d40849
author: chugugrace
ms.author: chugu
ms.openlocfilehash: f3b2b309d07e14347aa2d89038c897b3db804c9b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100354404"
---
# <a name="xml-task"></a>Tâche XML

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  La tâche XML est utilisée pour travailler avec des données XML. À l'aide de cette tâche, un package peut extraire des documents XML, appliquer des opérations aux documents en utilisant des feuilles de style XSLT (Extensible Stylesheet Language Transformations) et des expressions XPath, fusionner plusieurs documents, ou bien valider, comparer et enregistrer les documents mis à jour dans des fichiers et des variables.  
  
 Cette tâche permet à un package [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] de modifier de manière dynamique des documents XML au moment de l'exécution. Vous pouvez utiliser la tâche XML pour :  
  
-   remettre en forme un document XML. Cette tâche peut ainsi accéder à un rapport se trouvant dans un fichier XML et appliquer de manière dynamique une feuille de style XSLT afin de personnaliser la présentation du document ;  
  
-   sélectionner des sections d'un document XML. Cette tâche peut ainsi accéder à un rapport se trouvant dans un fichier XML et appliquer de manière dynamique une expression XPath afin de sélectionner une section du document. Cette opération peut également permettre d'obtenir et de traiter des valeurs dans un document ;  
  
-   fusionner des documents provenant de différentes sources. Cette tâche peut par exemple télécharger des rapports à partir de sources multiples et les fusionner de manière dynamique dans un document XML global unique.  
  
-   valider un document XML et éventuellement obtenir une sortie d’erreur détaillée. Pour plus d’informations, consultez [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
 Vous pouvez inclure des données XML dans un flux de données en utilisant une source XML pour extraire des valeurs d'un document XML. Pour plus d'informations, consultez [XML Source](../../integration-services/data-flow/xml-source.md).  
  
## <a name="xml-operations"></a>Opérations XML  
 La première action de la tâche XML consiste à extraire un document XML spécifique. Cette action est intégrée à la tâche XML et est effectuée automatiquement. Le document XML récupéré est utilisé comme source de données de l'opération réalisée par la tâche XML.  
  
 Les opérations XML Diff, Merge et Patch requièrent deux opérandes. Le premier opérande spécifie le document XML source. Le second opérande spécifie également un document XML, dont le contenu dépend de ce dont l'opération a besoin. Par exemple, l'opération Diff compare deux documents. Le second opérande spécifie donc un autre document XML similaire auquel le document XML source est comparé.  
  
 La tâche XML peut utiliser une variable ou un gestionnaire de connexions de fichiers comme source ou inclure les données XML dans une propriété de tâche.  
  
 Si la source est une variable, la variable spécifiée contient le chemin d'accès du document XML.  
  
 Si la source est un gestionnaire de connexions de fichiers, ce gestionnaire fournit les informations sources. Le gestionnaire de connexions de fichiers est configuré séparément de la tâche XML et est référencé dans la tâche XML. La chaîne de connexion du gestionnaire de connexions de fichiers spécifie le chemin d'accès du fichier XML. Pour plus d’informations, consultez [File Connection Manager](../../integration-services/connection-manager/file-connection-manager.md).  
  
 La tâche XML peut être configurée pour enregistrer les résultats de l'opération dans une variable ou un fichier. En cas d'enregistrement dans un fichier, la tâche XML utilise un gestionnaire de connexions de fichiers pour accéder au fichier. Vous pouvez également enregistrer les résultats du Diffgram généré par l'opération Diff dans des fichiers et des variables.  
  
## <a name="predefined-xml-operations"></a>Opérations XML prédéfinies  
 La tâche XML comprend un ensemble prédéfini d'opérations permettant de travailler avec des documents XML. Le tableau suivant décrit ces opérations.  
  
|Opération|Description|  
|---------------|-----------------|  
|Diff|Permet de comparer deux documents XML. Si le document XML source est utilisé comme document de base, l'opération Diff la compare à un second document XML, détecte leurs différences et écrit ces différences dans un document XML Diffgram. Cette opération inclut des propriétés permettant de personnaliser la comparaison.|  
|Fusionner|Permet de fusionner deux documents XML. Si le document XML source est utilisé comme document de base, l'opération Merge ajoute le contenu d'un second document dans le document de base. Cette opération peut spécifier un emplacement de fusion dans le document de base.|  
|Correctif|Applique la sortie de l'opération Diff, appelée document Diffgram, à un document XML afin de créer un document parent qui inclut le contenu du document Diffgram.|  
|Valider|Permet de valider le document XML en utilisant un DTD (Document Type Definition) ou un schéma XSD (XML Schema definition).|  
|XPath|Lance les requêtes et les évaluations XPath.|  
|XSLT|Effectue les transformations XSL sur les documents XML.|  
  
### <a name="diff-operation"></a>Opération Diff  
 L'opération Diff peut être configurée pour utiliser un algorithme de comparaison différent selon que la comparaison doive être rapide ou précise. Elle peut également être configurée pour sélectionner automatiquement une comparaison rapide ou précise en fonction de la taille des documents comparés.  
  
 L'opération Diff comprend un ensemble d'options permettant de personnaliser la comparaison XML. Le tableau ci-dessous décrit les options disponibles.  
  
|Option|Description|  
|------------|-----------------|  
|**IgnoreComments**|Valeur indiquant si les nœuds de commentaires sont comparés.|  
|**IgnoreNamespaces**|Valeur indiquant si les URI (Uniform Resource Identifier) d'espace de noms des noms d'un élément et de son attribut sont comparés. Si cette option a pour valeur **true**, deux éléments ayant le même nom local mais un espace de noms différent sont considérés comme identiques.|  
|**IgnorePrefixes**|Valeur indiquant si les préfixes des noms d'élément et d'attribut sont comparés. Si cette option a pour valeur **true,** deux éléments ayant le même nom local mais des URI d'espaces de noms et des préfixes différents sont considérés comme identiques.|  
|**IgnoreXMLDeclaration**|Valeur indiquant si les déclarations XML sont comparées.|  
|**IgnoreOrderOfChildElements**|Valeur indiquant si l'ordre des éléments enfants est comparé. Si cette option a pour valeur **true**, les éléments enfants qui diffèrent uniquement par leur position dans la liste des frères sont considérés comme identiques.|  
|**IgnoreWhiteSpaces**|Valeur indiquant si les espaces blancs sont comparés.|  
|**IgnoreProcessingInstructions**|Valeur indiquant si les instructions de traitement sont comparées.|  
|**IgnoreDTD**|Valeur indiquant si le schéma DTD est ignoré.|  
  
### <a name="merge-operation"></a>Opération de fusion  
 Lorsque vous utilisez une instruction XPath pour identifier l'emplacement de fusion dans le document source, cette instruction est supposée retourner un nœud unique. Si elle retourne plusieurs nœuds, seul le premier est utilisé. Le contenu du deuxième document est fusionné sous le premier nœud que la requête XPath retourne.  
  
### <a name="xpath-operation"></a>Opération XPath  
 L'opération XPath peut être configurée pour utiliser différents types de fonctionnalités XPath.  
  
-   Sélectionnez l’option **Évaluation** pour implémenter des fonctions XPath telles que sum().  
  
-   Sélectionnez l'option **Liste de nœuds** pour renvoyer les nœuds sélectionnés sous forme de fragment XML.  
  
-   Sélectionnez l'option **Valeurs** pour renvoyer les valeurs de texte de tous les nœuds sélectionnés, concaténées en une chaîne.  
  
### <a name="validation-operation"></a>Opération de validation  
 L'opération de validation peut être configurée pour utiliser un schéma DTD (Document Type Definition) ou XSD (XML Schema definition).  
  
 Activez **ValidationDetails** pour obtenir une sortie d’erreur détaillée. Pour plus d’informations, consultez [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
## <a name="xml-document-encoding"></a>Encodage de document XML  
 La tâche XML prend en charge la fusion de documents Unicode uniquement. Cela signifie que la tâche peut appliquer l'opération de fusion uniquement aux documents ayant un encodage Unicode. L'utilisation d'autres encodages provoquera l'échec de la tâche XML.  
  
> [!NOTE]  
>  Les opérations Diff et Patch comprennent une option qui ignore la déclaration XML dans les données XML du deuxième opérande, ce qui permet d'utiliser des documents avec d'autres encodages dans ces opérations.  
  
 Pour vérifier que le document XML peut être utilisé, vérifiez la déclaration XML. Celle-ci doit explicitement indiquer UTF-8, qui représente l'encodage unicode sur 8 bits.  
  
 La balise suivante montre l'encodage Unicode sur 8 bits.  
  
 `<?xml version="1.0" encoding="UTF-8"?>`  
  
## <a name="custom-logging-messages-available-on-the-xml-task"></a>Messages de journalisation personnalisés disponibles dans la tâche XML  
 Le tableau suivant décrit l'entrée de journal personnalisée de la tâche XML. Pour plus d’informations, consultez [Journalisation Integration Services &#40;SSIS&#41;](../../integration-services/performance/integration-services-ssis-logging.md).  
  
|Entrée du journal|Description|  
|---------------|-----------------|  
|**XMLOperation**|Fournit des informations sur l'opération que la tâche effectue.|  
  
## <a name="configuration-of-the-xml-task"></a>Configuration de la tâche XML  
 Vous pouvez définir les propriétés par le biais du concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] ou par programmation.  
  
 Pour plus d'informations sur les propriétés définissables dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur l'une des rubriques suivantes :  
  
-   [Effectuer une validation XML avec la tâche XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md)  
  
-   [Page Expressions](../../integration-services/expressions/expressions-page.md)  
  
 Pour plus d'informations sur la définition des propriétés dans le concepteur [!INCLUDE[ssIS](../../includes/ssis-md.md)] , cliquez sur la rubrique suivante :  
  
-   [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="programmatic-configuration-of-the-xml-task"></a>Configuration par programme de la tâche XML  
 Pour plus d'informations sur la définition par programme de ces propriétés, cliquez sur la rubrique suivante :  
  
-   <xref:Microsoft.SqlServer.Dts.Tasks.XMLTask.XMLTask>  
  
## <a name="related-tasks"></a>Tâches associées  
 [Définir les propriétés d'une tâche ou d'un conteneur](./add-or-delete-a-task-or-a-container-in-a-control-flow.md)  
  
## <a name="xml-task-editor-general-page"></a>XML Task Editor (General Page)
  Utilisez le nœud **Général** de la boîte de dialogue **Éditeur de tâche XML** pour préciser le type d'opération à effectuer, puis la configurer.  
  
 Pour en savoir plus sur cette tâche, consultez [Effectuer une validation XML avec la tâche XML](../../integration-services/control-flow/validate-xml-with-the-xml-task.md). Pour plus d'informations sur l'utilisation de documents et de données XML, consultez «[Employing XML in the .NET Framework](/previous-versions/aa720019(v=vs.71))» (en anglais) dans MSDN Library.  
  
### <a name="static-options"></a>Options statiques  
 **OperationType**  
 Permet de sélectionner le type d'opération dans la liste. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Procéder à la validation**|Permet de valider le document XML en utilisant un DTD (Document Type Definition) ou un schéma XSD (XML Schema definition). Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **Valider** s'affichent alors.|  
|**XSLT**|Effectue les transformations XSL sur les documents XML. Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **XSLT** s'affichent alors.|  
|**XPATH**|Lance les requêtes et les évaluations XPath. Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **XPATH** s'affichent alors.|  
|**Fusionner**|Permet de fusionner deux documents XML. Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **Fusionner** s'affichent alors.|  
|**Diff**|Permet de comparer deux documents XML. Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **Diff** s'affichent alors.|  
|**Patch**|Permet d'appliquer la sortie de l'opération Diff afin de créer un document. Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **Patch** s'affichent alors.|  
  
 **SourceType**  
 Permet de sélectionner le type de source correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **Source**  
 Si **Source** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source de document**.  
  
 Si **Source** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **Source** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur **\<New variable...>** pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
### <a name="operationtype-dynamic-options"></a>Options dynamiques OperationType  
  
#### <a name="operationtype--validate"></a>OperationType = Validate  
 Permet d'indiquer les options de l'opération Validate.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie produite par l'opération Validate.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **ValidationType**  
 Permet de sélectionner le type de validation. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**DTD**|Permet d'utiliser un DTD (Document Type Definition).|  
|**XSD**|Permet d'utiliser un schéma XSD (XML Schema Definition Language). Si cette valeur est sélectionnée, les options dynamiques incluses dans la section **ValidationType** s'affichent.|  
  
 **FailOnValidationFail**  
 Permet d'indiquer si le document doit ne pas être validé si l'opération échoue.  
  
 **ValidationDetails**  
 Fournit une sortie d’erreur détaillée quand la valeur de cette propriété est true. Pour plus d’informations, consultez [Validate XML with the XML Task](../../integration-services/control-flow/validate-xml-with-the-xml-task.md).  
  
### <a name="validationtype-dynamic-options"></a>Options dynamiques de ValidationType  
  
#### <a name="validationtype--xsd"></a>ValidationType = XSD  
 **SecondOperandType**  
 Permet de sélectionner le type de source correspondant au deuxième document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source**.  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xslt"></a>OperationType = XSLT  
 Permet d'indiquer les options relatives à l'opération de transformation XSLT.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie produite par l'opération XSLT.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Si **DestinationType** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperandType**  
 Permet de sélectionner le type de source correspondant au deuxième document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source**.  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
#### <a name="operationtype--xpath"></a>OperationType = XPATH  
 Permet de spécifier les options relatives à l'opération XPath.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie de l'opération XPath.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Si **DestinationType** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperandType**  
 Permet de sélectionner le type de source correspondant au deuxième document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source**.  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **PutResultInOneNode**  
 Permet de spécifier si le résultat doit être écrit dans un seul nœud.  
  
 **XPathOperation**  
 Permet d'indiquer le type de résultat XPath. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Evaluation**|Permet de retourner les résultats d'une fonction XPath.|  
|**Liste de nœuds**|Permet de retourner les nœuds sélectionnés en tant que fragment XML.|  
|**Valeurs**|Permet de retourner la valeur interne du texte correspondant à tous les nœuds sélectionnés, sous forme de chaîne concaténée.|  
  
#### <a name="operationtype--merge"></a>OperationType = Fusionner  
 Permet d'indiquer les différentes options de l'opération Fusionner.  
  
 **XPathStringSourceType**  
 Permet de sélectionner le type de source correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **XPathStringSource**  
 Si **XPathStringSourceType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source de document**.  
  
 Si **XPathStringSourceType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **XPathStringSourceType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
 Lorsque vous utilisez une instruction XPath pour identifier l'emplacement de fusion dans le document source, cette instruction est supposée retourner un nœud unique. Si elle retourne plusieurs nœuds, seul le premier est utilisé. Le contenu du deuxième document est fusionné sous le premier nœud que la requête XPath retourne.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie produite par l'opération Fusionner.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Si **DestinationType** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperandType**  
 Permet de sélectionner le type de destination correspondant au deuxième document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source de document** .  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** a la valeur **Variable**, sélectionnez une variable existante, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--diff"></a>OperationType = Diff  
 Permet d'indiquer les différentes options de l'opération Diff.  
  
 **DiffAlgorithm**  
 Permet de sélectionner l'algorithme Diff à utiliser pour la comparaison de documents. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Automatique**|Laisse la tâche XML déterminer si l'algorithme à utiliser est l'algorithme rapide ou l'algorithme précis.|  
|**Rapide**|L'algorithme de comparaison utilisé est le plus rapide des deux, mais aussi le moins précis.|  
|**Précise**|L'algorithme de comparaison utilisé est le plus précis des deux.|  
  
 **Options Diff**  
 Permet de définir les options de comparaison s'appliquant à l'opération Diff. Ces fonctions sont répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**IgnoreXMLDeclaration**|Permet d'indiquer si la déclaration XML doit être comparée.|  
|**IgnoreDTD**|Permet d'ignorer le DTD (Document Type Definition).|  
|**IgnoreWhite Spaces**|Spécifie s'il faut ignorer les différences dans le nombre d'espaces vides lorsque des documents sont comparés.|  
|**IgnoreNamespaces**|Permet d'indiquer si la comparaison doit aussi porter sur l'URI (Uniform Resource Identifier) d'espace de nom relatif à un élément ainsi que sur les noms d'attribut de ce même élément.<br /><br /> Remarque : Si cette option a la valeur **True**, deux éléments possédant le même nom local, mais des espaces de nom distincts sont considérés comme identiques.|  
|**IgnoreProcessingInstructions**|Permet d'indiquer si les instructions de traitement doivent être comparées.|  
|**IgnoreOrderOf ChildElements**|Permet d'indiquer si l'ordre des éléments enfants doit être comparé.<br /><br /> Remarque : Si cette option a la valeur **True**, les éléments enfants qui diffèrent uniquement par leur position dans une liste de frères sont considérés comme identiques.|  
|**IgnoreComments**|Permet d'indiquer si les nœuds de commentaires doivent être comparés.|  
|**IgnorePrefixes**|Permet d'indiquer si les préfixes d'un élément et ses noms d'attribut doivent être comparés.<br /><br /> Remarque : Si vous définissez cette option sur **True**, deux éléments possédant le même nom local, mais des URI et des préfixes d’espace de nom distincts sont considérés comme identiques.|  
  
 **FailOnDifference**  
 Permet d'indiquer si la tâche échoue si l'opération Diff échoue.  
  
 **SaveDiffGram**  
 Permet d'enregistrer ou non le résultat de comparaison sous forme de document DiffGram.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie produite par l'opération Diff.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Si **DestinationType** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperandType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source de document** .  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** a la valeur **Variable**, sélectionnez une variable existante, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
#### <a name="operationtype--patch"></a>OperationType = Patch  
 Permet d'indiquer les différentes options de l'opération Patch.  
  
 **SaveOperationResult**  
 Indique si la tâche XML enregistre ou non la sortie produite par l'opération Patch.  
  
 **OverwriteDestination**  
 Permet de spécifier si le fichier ou la variable de destination doit être remplacé.  
  
 **Destination**  
 Si **DestinationType** est défini sur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **DestinationType** a la valeur **Variable**, sélectionnez une variable existante ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md).  
  
 **DestinationType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperandType**  
 Permet de sélectionner le type de destination correspondant au document XML. Cette propriété dispose des options répertoriées dans le tableau suivant.  
  
|Valeur|Description|  
|-----------|-----------------|  
|**Entrée directe**|Permet de définir la source sur un document XML.|  
|**Connexion de fichiers**|Permet de sélectionner un fichier contenant le document XML.|  
|**Variable**|Permet de définir la source sur une variable contenant le document XML.|  
  
 **SecondOperand**  
 Si **SecondOperandType** a la valeur **Entrée directe**, indiquez le code XML ou cliquez sur le bouton de sélection **(...)** pour fournir le code XML nécessaire dans la boîte de dialogue **Éditeur de source de document** .  
  
 Si **SecondOperandType** a la valeur **Connexion de fichiers**, sélectionnez un gestionnaire de connexions de fichiers existant ou cliquez sur \<**New connection...**> pour en créer un.  
  
 **Rubriques connexes :** [Gestionnaire de connexions de fichiers](../../integration-services/connection-manager/file-connection-manager.md), [Éditeur du gestionnaire de connexions de fichiers](../connection-manager/file-connection-manager.md)  
  
 Si **SecondOperandType** a la valeur **Variable**, sélectionnez une variable existante, ou cliquez sur \<**New variable...**> pour en créer une.  
  
 **Rubriques connexes :** [Variables Integration Services &#40;SSIS&#41;](../../integration-services/integration-services-ssis-variables.md), [Ajouter une variable](../integration-services-ssis-variables.md)  
  
## <a name="related-content"></a>Contenu associé  

-   Exemple CodePlex, [Process XML Data Package Sample](https://msftisprodsamples.codeplex.com/wikipage?title=SS2008!Process%20XML%20Data%20Package%20Sample&version=10&ProjectName=msftisprodsamples), sur www.codeplex.com  
  
