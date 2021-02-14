---
title: API d’extensibilité
description: Découvrez les API d’extensibilité Azure Data Studio, qui permettent aux extensions d’interagir avec d’autres parties d’Azure Data Studio (telles que l’Explorateur d’objets).
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.topic: conceptual
author: markingmyname
ms.author: maghan
ms.reviewer: alayu, maghan, sstein
ms.custom: seodec18
ms.date: 09/24/2018
ms.openlocfilehash: d795d96d47fa7f07744c492d8639cdecdc2e23fb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100052155"
---
# <a name="azure-data-studio-extensibility-apis"></a>API d’extensibilité d’Azure Data Studio

Azure Data Studio fournit une API que les extensions peuvent utiliser pour interagir avec d’autres parties d’Azure Data Studio, telles que l’Explorateur d’objets. Ces API sont disponibles à partir du fichier [`src/sql/azdata.d.ts`](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.d.ts) et sont décrites ci-dessous.

## <a name="connection-management"></a>Gestion des connexions
`azdata.connection`

### <a name="top-level-functions"></a>Fonctions de niveau supérieur

- `getCurrentConnection(): Thenable<azdata.connection.Connection>` Obtient la connexion actuelle en fonction de l’éditeur actif ou de la sélection de l’Explorateur d’objets.

- `getActiveConnections(): Thenable<azdata.connection.Connection[]>` Obtient une liste de toutes les connexions de l’utilisateur qui sont actives. Retourne une liste vide s’il n’existe aucune connexion de ce type.

- `getCredentials(connectionId: string): Thenable<{ [name: string]: string }>` Obtient un dictionnaire contenant les informations d’identification associées à une connexion. Elles seraient sinon renvoyées dans le dictionnaire d’options sous un objet `azdata.connection.Connection`, mais supprimées de cet objet. 

### `Connection`
- `options: { [name: string]: string }` Dictionnaire des options de connexion
- `providerName: string` Nom du fournisseur de connexion (par exemple « mssql »)
- `connectionId: string` Identificateur unique pour la connexion

### <a name="example-code"></a>Exemple de code
```
> let connection = azdata.connection.getCurrentConnection();
connection: {
    providerName: 'MSSQL',
    connectionId: 'd97bb63a-466e-4ef0-ab6f-00cd44721dcc',
    options: {
        server: 'mairvine-sql-server',
        user: 'sa',
        authenticationType: 'sqlLogin',
        ...
    },
    ...
}
> let credentials = azdata.connection.getCredentials(connection.connectionId);
credentials: {
    password: 'abc123'
}

```

## <a name="object-explorer"></a>Explorateur d’objets

`azdata.objectexplorer`


### <a name="top-level-functions"></a>Fonctions de niveau supérieur
- `getNode(connectionId: string, nodePath?: string): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtient un nœud de l’Explorateur d’objets correspondant à la connexion et au chemin d’accès donnés. Si aucun chemin d’accès n’est fourni, retourne le nœud de niveau supérieur pour la connexion donnée. S’il n’y a aucun nœud dans le chemin d’accès donné, il retourne `undefined`. Remarque : Le `nodePath` pour un objet est généré par le backend du service Outils SQL et est difficile à construire manuellement. Les futures améliorations de l’API vous permettront d’obtenir des nœuds basés sur les métadonnées que vous fournissez sur le nœud, telles que le nom, le type et le schéma.

- `getActiveConnectionNodes(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtient tous les nœuds de connexion actifs de l’Explorateur d’objets.

- `findNodes(connectionId: string, type: string, schema: string, name: string, database: string, parentObjectNames: string[]): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Recherche tous les nœuds de l’Explorateur d’objets qui correspondent aux métadonnées fournies. Les arguments `schema`, `database` et `parentObjectNames` doivent être `undefined` lorsqu’ils ne sont pas applicables. `parentObjectNames` est une liste d’objets parents qui ne sont pas des bases de données, du plus haut au plus bas niveau de l’Explorateur d’objets, sous lesquels se trouve l’objet souhaité. Par exemple, lors de la recherche d’une colonne « Column1 » qui appartient à une table « schema1.table1» et à la base de données « Database1 » avec l’ID de connexion `connectionId`, appelez `findNodes(connectionId, 'Column', undefined, 'column1', 'database1', ['schema1.table1'])`. Consultez également la [liste des types qu’Azure Data Studio prend en charge par défaut](https://github.com/Microsoft/azuredatastudio/wiki/Object-Explorer-types-supported-by-FindNodes-API) pour cet appel d’API.

### <a name="objectexplorernode"></a>ObjectExplorerNode
- `connectionId: string` ID de la connexion dans laquelle le nœud existe

- `nodePath: string` Chemin d’accès du nœud, tel qu’il est utilisé pour un appel à la fonction `getNode`.

- `nodeType: string` Chaîne représentant le type du nœud

- `nodeSubType: string` Chaîne représentant le sous-type du nœud

- `nodeStatus: string` Chaîne représentant l’état du nœud

- `label: string` L’étiquette du nœud tel qu’elle apparaît dans l’Explorateur d’objets

- `isLeaf: boolean` Si le nœud est un nœud terminal et n’a donc pas d’enfants

- `metadata: azdata.ObjectMetadata` Métadonnées décrivant l’objet représenté par ce nœud

- `errorMessage: string` Message affiché si le nœud est dans un état d’erreur

- `isExpanded(): Thenable<boolean>` Si le nœud est actuellement développé dans l’Explorateur d’objets

- `setExpandedState(expandedState: vscode.TreeItemCollapsibleState): Thenable<void>` Définit si le nœud est développé ou réduit. Si l’état est défini sur None, le nœud ne sera pas modifié.

- `setSelected(selected: boolean, clearOtherSelections?: boolean): Thenable<void>` Définit si le nœud est sélectionné. Si `clearOtherSelections` a la valeur true, effacez toutes les autres sélections lors de la création de la nouvelle sélection. Si la valeur est false, laissez les sélections existantes. `clearOtherSelections` prend la valeur true par défaut quand `selected` est true, et false lorsque `selected` a la valeur false.

- `getChildren(): Thenable<azdata.objectexplorer.ObjectExplorerNode[]>` Obtient tous les nœuds enfants de ce nœud. Retourne une liste vide s’il n’y a pas d’enfants.

- `getParent(): Thenable<azdata.objectexplorer.ObjectExplorerNode>` Obtient le nœud parent de ce nœud. Retourne undefined s’il n’y a aucun parent.

### <a name="example-code"></a>Exemple de code

```cs
private async interactWithOENode(selectedNode: azdata.objectexplorer.ObjectExplorerNode): Promise<void> {
    let choices = ['Expand', 'Collapse', 'Select', 'Select (multi)', 'Deselect', 'Deselect (multi)'];
    if (selectedNode.isLeaf) {
        choices[0] += ' (is leaf)';
        choices[1] += ' (is leaf)';
    } else {
        let expanded = await selectedNode.isExpanded();
        if (expanded) {
            choices[0] += ' (is expanded)';
        } else {
            choices[1] += ' (is collapsed)';
        }
    }
    let parent = await selectedNode.getParent();
    if (parent) {
        choices.push('Get Parent');
    }
    let children = await selectedNode.getChildren();
    children.forEach(child => choices.push(child.label));
    let choice = await vscode.window.showQuickPick(choices);
    let nextNode: azdata.objectexplorer.ObjectExplorerNode = undefined;
    if (choice === choices[0]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Expanded);
    } else if (choice === choices[1]) {
        selectedNode.setExpandedState(vscode.TreeItemCollapsibleState.Collapsed);
    } else if (choice === choices[2]) {
        selectedNode.setSelected(true);
    } else if (choice === choices[3]) {
        selectedNode.setSelected(true, false);
    } else if (choice === choices[4]) {
        selectedNode.setSelected(false);
    } else if (choice === choices[5]) {
        selectedNode.setSelected(false, true);
    } else if (choice === 'Get Parent') {
        nextNode = parent;
    } else {
        let childNode = children.find(child => child.label === choice);
        nextNode = childNode;
    }
    if (nextNode) {
        let updatedNode = await azdata.objectexplorer.getNode(nextNode.connectionId, nextNode.nodePath);
        this.interactWithOENode(updatedNode);
    }
}

vscode.commands.registerCommand('mssql.objectexplorer.interact', () => {
    azdata.objectexplorer.getActiveConnectionNodes().then(activeConnections => {
        vscode.window.showQuickPick(activeConnections.map(connection => connection.label + ' ' + connection.connectionId)).then(selection => {
            let selectedNode = activeConnections.find(connection => connection.label + ' ' + connection.connectionId === selection);
            this.interactWithOENode(selectedNode);
        });
    });
});
```

## <a name="proposed-apis"></a>API proposées

Nous avons ajouté des API proposées pour permettre aux extensions d’afficher une interface utilisateur personnalisée dans des boîtes de dialogue, des assistants et des onglets de document, entre autres fonctionnalités. Pour plus d’informations, consultez le [fichier des types d’API proposées](https://github.com/Microsoft/azuredatastudio/blob/main/src/sql/azdata.proposed.d.ts), mais gardez à l’esprit que ces API sont susceptibles d’être modifiées à tout moment. Vous trouverez des exemples d’utilisation de certaines de ces API dans [l’exemple d’extension « sqlservices »](https://github.com/Microsoft/azuredatastudio/tree/main/samples/sqlservices).


