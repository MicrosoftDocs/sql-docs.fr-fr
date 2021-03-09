---
title: Guide de résolution des problèmes SqlClient
description: Page fournissant des solutions aux problèmes couramment observés.
ms.date: 03/03/2021
dev_langs:
- csharp
- vb
ms.assetid: 6f5ff56a-a57e-49d7-8ae9-bbed697e42e3
ms.prod: sql
ms.prod_service: connectivity
ms.technology: connectivity
ms.topic: conceptual
author: David-Engel
ms.author: v-daenge
ms.reviewer: v-jizho2
ms.openlocfilehash: 725d1522f98e721f581a7d4b7614181fd2632962
ms.sourcegitcommit: 15c7cd187dcff9fc91f2daf0056b12ed3f0403f0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/08/2021
ms.locfileid: "102464802"
---
# <a name="sqlclient-troubleshooting-guide"></a>Guide de résolution des problèmes SqlClient

[!INCLUDE[Driver_ADONET_Download](../../includes/driver_adonet_download.md)]

## <a name="exceptions-when-connecting-to-sql-server"></a>Exceptions lors de la connexion à SQL Server

Plusieurs raisons peuvent expliquer l’échec de l’établissement de la connexion. Vous trouverez ci-dessous des conseils de résolution des problèmes qui peuvent être utilisés comme guide pour analyser et résoudre de nombreux problèmes.

### <a name="unable-to-load-native-sni-server-name-indication-library"></a>Impossible de charger la bibliothèque SNI native (Indication du nom du serveur)

#### <a name="issues-in-net-framework-applications"></a>Problèmes dans les applications .NET Framework

StackTrace observé :

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x64.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

```log
TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
DllNotFoundException: Unable to load DLL 'Microsoft.Data.SqlClient.SNI.x86.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)
```

SNI est la bibliothèque C++ native dont SqlClient dépend pour diverses opérations de réseau lors de l’exécution sur Windows. Dans des applications .NET Framework générées avec le kit de développement logiciel (SDK) de projet MSBuild, les DLL natives ne sont pas gérées avec les commandes de restauration. Par conséquent, un fichier « .targets » est inclus dans le package NuGet « Microsoft.Data.SqlClient.SNI » qui définit les opérations de « Copie » nécessaires.

Le fichier « .targets » inclus est automatiquement référencé lorsqu’une dépendance directe est faite à la bibliothèque « Microsoft.Data.SqlClient ». Dans les scénarios où une référence transitive (indirecte) est effectuée, ce fichier « .targets » doit être référencé manuellement pour garantir que les opérations de « Copie » peuvent s’exécuter si nécessaire.

**Solution recommandée :** Assurez-vous que le fichier « .targets » est référencé dans le fichier « .csproj » de l’application pour vous assurer que les opérations « Copier » sont exécutées.

Ces cibles couvrent uniquement les cibles connues et couramment utilisées par Microsoft. Si une application ou un outil externe définit des cibles personnalisées pour copier les fichiers binaires, les nouvelles cibles doivent être définies par des chargés de maintenance des outils pour s’assurer que les DLL SNI natives sont copiées sur les fichiers binaires de Microsoft.Data.SqlClient.dll et sont disponibles lors de l’exécution d’applications clientes.

#### <a name="issues-in-net-core-applications"></a>Problèmes dans les applications .Net Core

StackTrace observé :

```log
System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.TdsParser' threw an exception.
---> System.TypeInitializationException: The type initializer for 'Microsoft.Data.SqlClient.SNILoadHandle' threw an exception.
---> System.DllNotFoundException: Unable to load shared library 'Microsoft.Data.SqlClient.SNI.dll' or one of its dependencies.
```

> [!NOTE]
> Cette erreur peut se produire uniquement sur les applications Windows. Si elle se produit dans un environnement UNIX, vous devez vous assurer que votre application est générée pour cibler de manière appropriée un runtime UNIX et non pour Windows.

SNI est la bibliothèque C++ native dont SqlClient dépend pour diverses opérations de réseau lors de l’exécution sur Windows. Microsoft.Data.SqlClient ne gère pas le chargement/déchargement de cette bibliothèque dans .NET Core.

**Solution recommandée :** Vérifiez que les autorisations « Exécuter » sont accordées sur le système de fichiers dans lequel les bibliothèques de runtime natives sont chargées dans le processus .NET Core. Si cela ne résout pas le problème, vous pouvez envoyer un problème dans le référentiel [dotnet/runtime](https://github.com/dotnet/runtime) pour obtenir un support supplémentaire.

#### <a name="native-sni-pdb-not-found-errors"></a>Erreurs de SNI native (PDB introuvable)

StackTrace observé :

```log
An assembly specified in the application dependencies manifest (sql2csv.deps.json) was not found:
  package: 'Microsoft.Data.SqlClient.SNI.runtime', version: '2.0.0'
  path: 'runtimes/win-x64/native/Microsoft.Data.SqlClient.SNI.pdb'
```

**Solution recommandée :** Vérifiez que l’application cliente fait référence au minimum à la version [v2.1.0](https://www.nuget.org/packages/Microsoft.Data.SqlClient/2.1.0) version du package Microsoft.Data.SqlClient. Quand vous utilisez EF Core, ajoutez directement une référence à cette version de package de Microsoft.Data.SqlClient pour remplacer la dépendance.

### <a name="hostname-resolution-errors"></a>Erreurs de la résolution du nom d’hôte

StackTrace observé :

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 0 - No such host is known.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible.
Verify that the instance name is correct and that SQL Server is configured to allow remote connections.
(provider: TCP Provider, error: 35 - An internal exception was caught)
 ---> System.Net.Internals.SocketExceptionFactory+ExtendedSocketException (00000005, 0xFFFDFFFF): Name does not resolve
```

#### <a name="possible-reasons"></a>Causes possibles

- Le protocole TCP/canaux nommés n’est pas activé sur SQL Server

  **Solution recommandée :** Activez le protocole TCP/canaux nommés sur l’instance SQL à partir de la console Gestionnaire de configuration SQL Server.

- Nom d’hôte non connu

  **Solution recommandée :** Assurez-vous que le nom d’hôte correspond à l’adresse IP du serveur à partir du client où la connexion est lancée.


### <a name="login-phase-errors"></a>Erreurs de la phase de connexion

StackTraces observé :

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): A connection was successfully established with the server, but then an error occurred during the pre-login handshake.
(provider: SSL Provider, error: 31 - Encryption(ssl/tls) handshake failed)
System.IO.EndOfStreamException: End of stream reached
```

```log
A connection was successfully established with the server, but then an error occurred during the login process.
(provider: SSL Provider, error: 0 - The target principal name is incorrect.)
```

```log
Microsoft.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections.
The duration spent while attempting to connect to this server was - [Pre-Login] initialization=837; handshake=394; [Login] initialization=3; authentication=15; [Post-Login] complete=1027;
---> System.ComponentModel.Win32Exception (258): Unknown error 258
at Microsoft.Data.SqlClient.SqlInternalConnection.OnError(SqlException exception, Boolean breakConnection, Action1 wrapCloseInAction)
```

#### <a name="possible-reasons-and-solutions"></a>Causes et solutions possibles

- SQL Server ne prend pas en charge TLS 1.2

  Cette erreur se produit généralement dans les environnements clients tels que les conteneurs d’images de l’ancrage, les clients UNIX ou les clients Windows où TLS 1.2 est le protocole TLS minimal pris en charge.

  **Solution recommandée :** Installez les dernières mises à jour sur les versions prises en charge de SQL Server <sup>1</sup> et assurez-vous que le protocole TLS 1.2 est activé sur le serveur.

  _<sup>1</sup> afficher le [cycle de vie du support des pilotes SqlClient](sqlclient-driver-support-lifecycle.md) pour obtenir la liste des versions de SQL Server prises en charge avec les différentes versions de Microsoft.Data.SqlClient._

  **Solution non sécurisée :** Configurez les paramètres TLS/SSL dans l’environnement client/image de Docker pour la connexion avec TLS 1.0.

  ```docker
  MinProtocol = TLSv1
  CipherString = DEFAULT@SECLEVEL=1
  ```

  > [!NOTE]
  > Lors de la connexion avec Microsoft.Data.SqlClient v2.0+ à partir d’un environnement Windows/Linux avec TLS 1.0 ou TLS 1.1, un message d’avertissement de sécurité est levé si le SQL Server cible et le client ne peuvent pas négocier au minimum la version TLS 1.2 lors de l’établissement de la connexion : `Security Warning: The negotiated <TLS1.0 | TLS1.1> is an insecure protocol and is supported for backward compatibility only. The recommended protocol version is TLS 1.2 and later.`

- Chiffrement appliqué SQL Server

  Si le serveur cible est une instance Azure SQL ou SQL Server sur site avec la propriété « Forcer le chiffrement » activée, une connexion chiffrée est établie, pour laquelle le client doit établir une relation de confiance avec le serveur.

  **Solution recommandée :** Deux options sont disponibles pour résoudre ce problème :

    1. Installez le certificat TLS/SSL de SQL Server cible dans l’environnement client. Il sera validé si le chiffrement est nécessaire.
    2. Définissez la propriété « Faire confiance au certificat de serveur = true » dans la chaîne de connexion.

  **Solution non sécurisée :** Désactivez le paramètre « Forcer le chiffrement » sur SQL Server.

- Les certificats TLS/SSL ne sont pas signés avec SHA-256 ou version ultérieure.

  **Solution recommandée :** Générez un nouveau certificat TLS/SSL pour le serveur dont le hachage est signé avec au moins l’algorithme de hachage SHA-256.

- Suites de chiffrement très restreintes sur Linux avec .NET 5 et versions ultérieures

  .NET 5 a introduit un changement cassant pour les clients Linux, où une liste très restreinte de suites de chiffrement autorisées est utilisée par défaut. Si nécessaire, développez la liste des suites de chiffrement par défaut pour accepter les clients hérités (ou pour contacter les serveurs hérités) en spécifiant une valeur `CipherSuitePolicy` ou en modifiant le fichier de configuration _OpenSSL_.
  
  Consultez [Suites de chiffrement TLS par défaut pour .NET sur Linux](/dotnet/core/compatibility/cryptography/5.0/default-cipher-suites-for-tls-on-linux) pour en savoir plus sur l’action recommandée.

### <a name="connection-pool-exhaustion-errors"></a>Erreurs pool de connexions épuisé

StackTrace observé :

```log
System.InvalidOperationException: Timeout expired. The timeout period elapsed prior to obtaining a connection from the pool.
This may have occurred because all pooled connections were in use and max pool size was reached.
```

#### <a name="possible-reasons-and-solutions"></a>Causes et solutions possibles

L’application cliente ouvre plus de connexions que le pool de connexions peut conserver active à un moment donné.

**Solution recommandée :** Configurez la propriété de connexion « Taille maximale du pool » sur une valeur supérieure et fermez les connexions inutilisées en temps opportun.

## <a name="contact-support"></a>Contacter le support technique

Si ce guide ne résout pas vos problèmes de connectivité, vous pouvez consulter les problèmes existants dans le référentiel [dotnet/SqlClient](https://github.com/dotnet/SqlClient) et ouvrir un nouveau problème si nécessaire.
