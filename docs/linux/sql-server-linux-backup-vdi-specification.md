---
title: Spécification de la sauvegarde VDI - SQL Server sur Linux
description: Découvrez les interfaces fournies par le kit de développement logiciel (SDK) de client de l’interface VDI pour SQL Server sur Linux.
author: VanMSFT
ms.author: vanto
ms.reviewer: vanto
ms.date: 03/17/2017
ms.topic: conceptual
ms.prod: sql
ms.technology: linux
ms.assetid: 0250ba2b-8cdd-450e-9109-bf74f70e1247
ms.openlocfilehash: dcdbd7ba3a00048b63836e52092835fe6f7a618f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/10/2021
ms.locfileid: "100343180"
---
# <a name="sql-server-on-linux-vdi-client-sdk-specification"></a>Spécification du kit de développement logiciel (SDK) de client VDI pour SQL Server sur Linux

[!INCLUDE [SQL Server - Linux](../includes/applies-to-version/sql-linux.md)]

Ce document traite les interfaces fournies par le kit de développement logiciel (SDK) de client de l’interface VDI pour SQL Server sur Linux. Les éditeurs de logiciels indépendants peuvent utiliser l’interface de programmation d’applications (API) d’appareil de virtuel de sauvegarde pour intégrer SQL Server à leurs produits. En général, l’infrastructure VDI sur Linux se comporte de la même façon que sur Windows, avec les modifications suivantes :

- La mémoire partagée Windows devient la mémoire partagée POSIX.
- Les sémaphores Windows deviennent des sémaphores POSIX.
- Les types Windows comme HRESULT et DWORD sont remplacés par des équivalents entiers.
- Les interfaces COM sont supprimées et remplacées par une ++ paire de classes C++.
- SQL Server sur Linux ne prend pas en charge les instances nommées, aussi les références au nom d’instance ont été supprimées. 
- La bibliothèque partagée est implémentée dans libsqlvdi.so, installé dans /opt/mssql/lib/libsqlvdi.so

Ce document est un addendum à **vbackup.chm**, qui détaille les spécifications VDI MS SQL Server pour Windows. Téléchargez les [spécifications VDI SQL pour Windows](https://www.microsoft.com/download/details.aspx?id=17282).

Passez également en revue l’exemple de solution de sauvegarde VDI dans le [référentiel GitHub d’exemples pour SQL Server](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/sqlvdi-linux).

## <a name="user-permissions-setup"></a>Configuration des autorisations d’utilisateur

Sur Linux, les primitives POSIX sont détenues par l’utilisateur qui les crée et leur groupe par défaut. Pour les objets créés par SQL Server, elles sont détenues par l’utilisateur mssql et le groupe mssql, par défaut. Pour autoriser le partage entre SQL Server et le client VDI, il est recommandé d’utiliser une des deux méthodes suivantes :

1. Exécuter le client VDI en tant qu’utilisateur mssql
   
   Exécutez la commande suivante pour basculer vers l’utilisateur mssql :
   
   ```bash
   sudo su mssql
   ```

2. Ajoutez l’utilisateur mssql au groupe de vdiuser et le vdiuser au groupe mssql.
   
   Exécutez les commandes suivantes :

   ```bash
   sudo useradd vdiuser
   sudo usermod -a -G mssql vdiuser
   sudo usermod -a -G vdiuser mssql
   ```

   Redémarrez le serveur pour récupérer les nouveaux groupes pour SQL Server et vdiuser

## <a name="client-functions"></a>Fonctions clientes

Ce chapitre contient les descriptions de chacune des fonctions clientes. Les descriptions incluent les informations suivantes :

- Objectif de la fonction
- Syntaxe de la fonction
- Liste de paramètres
- Valeurs retournées
- Notes

## <a name="clientvirtualdevicesetcreate"></a>ClientVirtualDeviceSet::Create

**Objectif** : cette fonction crée le jeu d’appareils virtuels.

**Syntaxe**
   ```
   int ClientVirtualDeviceSet::Create (
   char *   name,       // name for the set
   VDConfig   * cfg     // configuration for the set
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| | **name** | Identifie l’ensemble des appareils virtuels. Les règles pour les noms utilisés par CreateFileMapping() doivent être suivies. N’importe quel caractère à l’exception de la barre oblique inverse (\) peut être utilisé. Il s’agit d’une chaîne de caractères. Il est recommandé de préfixer la chaîne avec le nom de la société ou du produit de l’utilisateur et le nom de la base de données. |
| |**cfg** | Il s’agit de la configuration du jeu d’appareils virtuels. Pour plus d’informations, consultez la section « Configuration » plus loin dans ce document.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** | La fonction a réussi. |
| |**VD_E_NOTSUPPORTED** |Un ou plusieurs champs de la configuration ne sont pas valides ou ne sont pas pris en charge. |
| |**VD_E_PROTOCOL** | L’ensemble d’appareils virtuels existe déjà.

**Remarques** La méthode Create ne doit être appelée qu’une seule fois par opération de sauvegarde ou de restauration. Après l’appel de la méthode Close, le client peut réutiliser l’interface pour créer un autre ensemble d’appareils virtuels.

## <a name="clientvirtualdevicesetgetconfiguration"></a>ClientVirtualDeviceSet::GetConfiguration

**Objectif** : cette fonction est utilisée pour attendre que le serveur configure l’ensemble d’appareils virtuels.
**Syntaxe**
   ```
   int ClientVirtualDeviceSet::GetConfiguration (
   time_t       timeout,    // in milliseconds
   VDConfig *       cfg // selected configuration
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| | **timeout** | Il s’agit du délai d’attente en millisecondes. Utilisez Infinite ou un entier négatif pour éviter tout délai d’attente.
| | **cfg** | Une fois l’exécution réussie, contient la configuration sélectionnée par le serveur. Pour plus d’informations, consultez la section « Configuration » plus loin dans ce document.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** | La configuration a été retournée.
| |**VD_E_ABORT** |SignalAbort a été appelé.
| |**VD_E_TIMEOUT** |Expiration du délai d’attente de la fonction.

**Remarques** Cette fonction se bloque dans un état d’attente d’alerte. Une fois l’appel réussi, les appareils de l’ensemble d’appareils virtuels peuvent être ouverts.


## <a name="clientvirtualdevicesetopendevice"></a>ClientVirtualDeviceSet::OpenDevice
**Objectif** : cette fonction ouvre un des appareils dans l’ensemble d’appareils virtuels.
**Syntaxe**
   ```
   int ClientVirtualDeviceSet::OpenDevice (
   char *           name,       // name for the set
   ClientVirtualDevice **       ppVirtualDevice // returns interface to device
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| | **name** |Identifie l’ensemble des appareils virtuels.
| | **ppVirtualDevice** |Lorsque la fonction réussit, un pointeur vers l’appareil virtuel est retourné. Cet appareil est utilisé pour GetCommand et CompleteCommand.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |La fonction a réussi.
| |**VD_E_ABORT** | L’annulation a été demandée.
| |**VD_E_OPEN** |  Tous les appareils sont ouverts.
| |**VD_E_PROTOCOL** |  L’ensemble n’est pas dans l’état d’initialisation ou cet appareil particulier est déjà ouvert.
| |**VD_E_INVALID** |Le nom de l’appareil n’est pas valide. Il ne s’agit pas d’un des noms connus pour inclure l’ensemble.

**Remarques** VD_E_OPEN peut être retourné sans problème. Le client peut appeler OpenDevice au moyen d’une boucle jusqu’à ce que ce code soit retourné.
Si plusieurs appareils sont configurés, par exemple *n* appareils, le jeu d’appareils virtuels retourne *n* interfaces d’appareil uniques.

La fonction `GetConfiguration` peut être utilisée pour attendre que les appareils puissent être ouverts.
Si cette fonction échoue, une valeur null est retournée par le biais de ppVirtualDevice.
 
## <a name="clientvirtualdevicegetcommand"></a>ClientVirtualDevice::GetCommand

**Objectif** : cette fonction est utilisée pour obtenir la commande suivante mise en file d’attente sur un appareil. Quand elle est demandée, cette fonction attend la commande suivante.

**Syntaxe**
   ```
   int ClientVirtualDevice::GetCommand (
   time_t       timeout,    // time-out in milliseconds
   VDC_Command**    ppCmd   // returns the next command
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |**timeout** |Il s’agit du délai d’attente en millisecondes. Utilisez INFINITE pour attendre indéfiniment. Utilisez 0 pour interroger une commande. VD_E_TIMEOUT est retourné si aucune commande n’est actuellement disponible. Si le délai d’expiration s’écoule, le client décide de l’action suivante.
| |**Délai d'expiration** |Il s’agit du délai d’attente en millisecondes. Utilisez INFINITE ou une valeur négative pour attendre indéfiniment. Utilisez 0 pour interroger une commande. VD_E_TIMEOUT est retourné si aucune commande n’est disponible avant l’expiration du délai d’attente. Si le délai d’expiration s’écoule, le client décide de l’action suivante.
| |**ppCmd** |Quand une commande est correctement retournée, le paramètre retourne l’adresse d’une commande à exécuter. La mémoire retournée est en lecture seule. Lorsque la commande est terminée, ce pointeur est passé à la routine CompleteCommand. Pour plus d’informations sur chaque commande, consultez la section « Commandes » plus loin dans ce document.
        
| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |Une commande a été extraite.
| |**VD_E_CLOSE** |L’appareil a été fermé par le serveur.
| |**VD_E_TIMEOUT** |Aucune commande n’était disponible et le délai d’attente a expiré.
| |**VD_E_ABORT** |Le client ou le serveur a utilisé le SignalAbort pour forcer un arrêt.

**Remarques** Quand VD_E_CLOSE est retourné, SQL Server ferme l’appareil. Cela fait partie de l’arrêt normal. Une fois que tous les appareils ont été fermés, le client appelle ClientVirtualDeviceSet::Close pour fermer l’ensemble d’appareils virtuels.
Quand cette routine doit se bloquer pour attendre une commande, le thread reste dans une condition d’attente d’alerte.

## <a name="clientvirtualdevicecompletecommand"></a>ClientVirtualDevice::CompleteCommand

**Objectif** : cette fonction est utilisée pour notifier SQL Server qu’une commande est terminée. Les informations d’achèvement appropriées pour la commande doivent être retournées. Pour plus d’informations, consultez la section « Commandes » plus loin dans ce document.

**Syntaxe** 

   ```
   int ClientVirtualDevice::CompleteCommand (
   VDC_Command pCmd,        // the command
   int  completionCode,     // completion code
   unsigned long    bytesTransferred,   // bytes transferred
   int64_t  position        // current position
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |**pCmd** |Il s’agit de l’adresse d’une commande précédemment retournée à partir de ClientVirtualDevice::GetCommand.
| |**completionCode** |Il s’agit d’un code d’état qui indique l’état d’achèvement. Ce paramètre doit être retourné pour toutes les commandes. Le code retourné doit être approprié pour la commande en cours d’exécution. ERROR_SUCCESS est utilisé dans tous les cas pour indiquer une commande exécutée avec succès. Pour obtenir la liste complète des codes possibles, consultez le fichier vdierror.h. Une liste de codes d’état par défaut typiques pour chaque commande apparaît dans la section « Commandes » plus loin dans ce document.
| |**bytesTransferred** |Il s’agit du nombre d’octets transférés avec succès. Il est retourné uniquement pour les commandes de transfert de données en lecture et en écriture.
| |**position** |Il s’agit d’une réponse à la commande GetPosition uniquement.
        
| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |La saisie semi-automatique a été correctement notée.
| |**VD_E_INVALID** |pCmd n’était pas une commande active.
| |**VD_E_ABORT** |Abort a été signalé.
| |**VD_E_PROTOCOL** |L'appareil n'est pas ouvert.

**Remarques** Aucune

## <a name="clientvirtualdevicesetsignalabort"></a>ClientVirtualDeviceSet::SignalAbort

**Objectif** : cette fonction est utilisée pour signaler qu’un arrêt anormal doit se produire.

**Syntaxe** 

   ```
   int ClientVirtualDeviceSet::SignalAbort ();
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |None | Non applicable
        
| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR**|La notification d’abandon a été correctement publiée.

**Remarques** À tout moment, le client peut choisir d’abandonner l’opération de sauvegarde ou de restauration. Cette routine signale que toutes les opérations doivent cesser. L’ensemble d’appareils virtuels entier entre dans un état d’arrêt anormal. Aucune autre commande n’est retournée sur les appareils. Toutes les commandes incomplètes sont automatiquement terminées, en retournant ERROR_OPERATION_ABORTED comme code d’achèvement. Le client doit appeler ClientVirtualDeviceSet::Close après avoir terminé de façon sûre toute utilisation en attente des mémoires tampons fournies au client. Pour plus d’informations, consultez la section « Arrêt anormal » plus haut dans ce document.

## <a name="clientvirtualdevicesetclose"></a>ClientVirtualDeviceSet::Close

**Objectif** : cette fonction ferme l’ensemble d’appareils virtuels créé par ClientVirtualDeviceSet::Create. Cela entraîne la libération de toutes les ressources associées à l’ensemble d’appareils virtuels.

**Syntaxe** 

   ```
   int ClientVirtualDeviceSet::Close ();
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |None |Non applicable
        
| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |Cette valeur est retournée lorsque l’ensemble d’appareils virtuels a été correctement fermé.
| |**VD_E_PROTOCOL** |Aucune action n’a été effectuée, car l’ensemble d’appareils virtuels n’était pas ouvert.
| |**VD_E_OPEN** |Les appareils étaient encore ouverts.

**Remarques** L’appel de Close est une déclaration client indiquant que toutes les ressources utilisées par l’ensemble d’appareils virtuels doivent être libérées. Le client doit s’assurer que toutes les activités impliquant des tampons de données et des appareils virtuels sont terminées avant d’appeler Close. Toutes les interfaces d’appareil virtuel retournées par OpenDevice sont invalidées par Close.
Le client est autorisé à émettre un appel Create sur l’interface de l’ensemble d’appareils virtuels après le retour de l’appel Close. Un tel appel créerait un nouvel ensemble d’appareils virtuels pour une opération de sauvegarde ou de restauration ultérieure.
Si Close est appelé lorsqu’un ou plusieurs appareils virtuels sont toujours ouverts, VD_E_OPEN est retourné. Dans ce cas, SignalAbort est déclenché en interne, afin de garantir un arrêt correct, si possible. Les ressources VDI sont publiées. Le client doit attendre une indication VD_E_CLOSE sur chaque appareil avant d’appeler ClientVirtualDeviceSet::Close. Si le client sait que l’ensemble d’appareils virtuels est déjà dans un état d’arrêt anormal, il ne doit pas attendre une indication VD_E_CLOSE de GetCommand et peut appeler ClientVirtualDeviceSet::Close dès que l’activité sur les mémoires tampons partagées est terminée.
Pour plus d’informations, consultez la section « Arrêt anormal » plus haut dans ce document.

## <a name="clientvirtualdevicesetopeninsecondary"></a>ClientVirtualDeviceSet::OpenInSecondary

**Objectif** : cette fonction ouvre l’ensemble d’appareils virtuels dans un client secondaire. Le client principal doit déjà avoir utilisé Create et GetConfiguration pour configurer l’ensemble d’appareils virtuels.

**Syntaxe** 
   
   ```
   int ClientVirtualDeviceSet::OpenInSecondary (
   char *   setName         // name of the set
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |**setName** |Cela identifie l’ensemble. Ce nom respecte la casse et doit correspondre au nom utilisé par le client principal lors de son appel à ClientVirtualDeviceSet::Create.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |La fonction a réussi.
| |**VD_E_PROTOCOL** |L’ensemble d’appareils virtuels n’a pas été créé, est déjà ouvert sur ce client, ou l’ensemble d’appareils virtuels n’est pas prêt à accepter les demandes d’ouverture des clients secondaires.
| |**VD_E_ABORT** |L’opération est en cours d’abandon.

**Remarque** Lorsque vous utilisez un modèle à plusieurs processus, le client principal est responsable de la détection de l’arrêt normal ou anormal des clients secondaires.

## <a name="clientvirtualdevicesetgetbufferhandle"></a>ClientVirtualDeviceSet::GetBufferHandle

**Objectif :** Certaines applications peuvent nécessiter plusieurs processus pour fonctionner sur les mémoires tampons retournées par ClientVirtualDevice::GetCommand. Dans ce cas, le processus qui reçoit la commande peut utiliser GetBufferHandle pour obtenir un descripteur indépendant du processus qui identifie la mémoire tampon. Ce descripteur peut ensuite être communiqué à tout autre processus qui a également le même ensemble d’appareils virtuels ouvert. Ce processus utilise ensuite ClientVirtualDeviceSet::MapBufferHandle pour obtenir l’adresse de la mémoire tampon. L’adresse sera probablement différente de celle de son partenaire, car chaque processus peut mapper des mémoires tampons à des adresses différentes.

**Syntaxe** 

   ```
   int ClientVirtualDeviceSet::GetBufferHandle (
   uint8_t*     pBuffer,        // in: buffer address
   unsigned int*        pBufferHandle   // out: buffer handle
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |**pBuffer** |Il s’agit de l’adresse d’une mémoire tampon obtenue à partir d’une commande de lecture ou d’écriture.
| |**BufferHandle** |Un identificateur unique pour la mémoire tampon est retourné.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |La fonction a réussi.
| |**VD_E_PROTOCOL** |L’ensemble d’appareils virtuels n’est pas actuellement ouvert.
| |**VD_E_INVALID** |Le pBuffer n’est pas une adresse valide.

Remarques Le processus qui appelle la fonction GetBufferHandle est chargé d’appeler ClientVirtualDevice::CompleteCommand lorsque le transfert de données est terminé.

## <a name="clientvirtualdevicesetmapbufferhandle"></a>ClientVirtualDeviceSet::MapBufferHandle

**Objectif** : cette fonction est utilisée pour obtenir une adresse de mémoire tampon valide à partir d’un descripteur de mémoire tampon obtenu à partir d’un autre processus. 

**Syntaxe** 

   ```
   int ClientVirtualDeviceSet::MapBufferHandle (
   i        nt  dwBuffer,   // in: buffer handle
   uint8_t**    ppBuffer        // out: buffer address
   );
   ```

| Paramètres | Argument | Explication
| ----- | ----- | ------ |
| |**dwBuffer** |Il s’agit du descripteur retourné par ClientVirtualDeviceSet::GetBufferHandle.
| |**ppBuffer** |Il s’agit de l’adresse de la mémoire tampon qui est valide dans le processus actuel.

| Valeurs de retour | Argument | Explication
| ----- | ----- | ------ |
| |**NOERROR** |La fonction a réussi.
| |**VD_E_PROTOCOL** |L’ensemble d’appareils virtuels n’est pas actuellement ouvert.
| |**VD_E_INVALID** |Le ppBuffer est un descripteur non valide.

**Remarques** Veillez à bien communiquer les descripteurs. Les descripteurs sont locaux à un seul ensemble d’appareils virtuels. Les processus partenaires qui partagent un descripteur doivent s’assurer que les descripteurs de la mémoire tampon sont utilisés uniquement dans l’étendue de l’ensemble d’appareils virtuels à partir duquel la mémoire tampon a été obtenue à la base.


