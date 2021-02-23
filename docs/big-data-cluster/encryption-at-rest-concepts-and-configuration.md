---
title: Chiffrement au repos
titleSuffix: SQL Server Big Data Clusters
description: En savoir plus sur le chiffrement au repos sur un cluster Big Data SQL Server 2019.
author: dacoelho
ms.author: dacoelho
ms.reviewer: mikeray
ms.metadata: seo-lt-2019
ms.date: 10/19/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 69749c0da2a5f7ef672a4673b5bc857898e9964f
ms.sourcegitcommit: e8c0c04eb7009a50cbd3e649c9e1b4365e8994eb
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100489183"
---
# <a name="encryption-at-rest-concepts-and-configuration-guide"></a>Guide de configuration et de concepts du chiffrement au repos

À partir des Clusters Big Data SQL Server CU8, un ensemble complet de fonctionnalités de chiffrement au repos est disponible pour fournir le chiffrement au niveau de l’application à toutes les données stockées dans la plateforme. Ce guide décrit les concepts, l’architecture et la configuration de la fonctionnalité de chiffrement au repos définie pour les Clusters Big Data SQL Server.

Les Clusters Big Data SQL Server stockent les données dans les deux emplacements suivants :

* Instance maître __SQL Server__
* __HDFS__ utilisé par le pool de stockage et Spark.

Pour pouvoir chiffrer de manière transparente les données dans des Clusters Big Data SQL Server, il existe deux approches possibles :

* __Chiffrement de volume__. Cette approche est prise en charge par la plateforme Kubernetes et est attendue comme meilleure pratique pour les déploiements de Clusters Big Data. Ce guide ne couvre pas le chiffrement de volume. Consultez la documentation de votre plateforme ou de votre appliance Kubernetes pour savoir comment chiffrer correctement des volumes qui seront utilisés pour les Clusters Big Data SQL Server.
* __Chiffrement au niveau de l’application__. Cette architecture fait référence au chiffrement des données par l’application qui gère les données avant qu’elles ne soient écrites sur le disque. Si les volumes sont exposés, un attaquant ne pourra pas restaurer les artefacts de données ailleurs, sauf si le système de destination a également été configuré avec les mêmes clés de chiffrement.

L’__ensemble de fonctionnalités de chiffrement au repos des Clusters Big Data SQL Server__ prend en charge le scénario de base du __chiffrement au niveau de l’application__ pour les composants __SQL Server__ et __HDFS__.

Les fonctionnalités suivantes sont inclues :

* __Chiffrement au repos managé par le système__. Cette possibilité est disponible dans CU8.
* __Le chiffrement au repos managé par l’utilisateur (BYOK)__ , avec les intégrations managées par le système et de fournisseurs de clés externes. Actuellement, seules les clés créées par l’utilisateur managées par le service sont prises en charge.

## <a name="key-definitions"></a>Définitions clés

### <a name="sql-server-big-data-clusters-key-management-service-kms"></a>Service de gestion de clés des Clusters Big Data SQL Server (KMS)

Service hébergé par un contrôleur chargé de gérer les clés et les certificats pour la fonctionnalité de chiffrement au repos définie pour le cluster BDC SQL Server. Il s’agit d’un service qui prend en charge les fonctionnalités suivantes :

* Gestion et stockage sécurisés des clés et certificats utilisés pour le chiffrement au repos.
* Compatibilité KMS Hadoop. Il agit en tant que service de gestion de clés pour le composant HDFS sur BDC.
* Gestion des certificats TDE SQL Server.

Nous allons faire référence à ce service comme __KMS BDC__ dans le reste de ce document. Le terme __BDC__ est également utilisé pour faire référence à la plateforme de calcul des __Clusters Big Data SQL Server__.

### <a name="system-managed-keys-and-certificates"></a>Clés et certificats managés par le système

Le service KMS BDC gère l’ensemble des clés et certificats pour SQL Server et HDFS.

### <a name="user-provided-certificates"></a>Certificats fournis par l’utilisateur

Les clés et certificats fournis par l’utilisateur sont managés par le KMS BDC, communément appelé BYOK (Bring Your Own Key).

### <a name="external-providers"></a>Fournisseurs externes

Solutions de clé externe compatibles avec KMS BDC pour la délégation externe. Cette fonctionnalité est pas prise en charge pour l’instant.

## <a name="encryption-at-rest-on-sql-server-big-data-clusters"></a>Chiffrement au repos sur des clusters Big Data SQL Server

Lisez attentivement ce document pour évaluer complètement votre scénario.

L’ensemble de fonctionnalités présente le __service du contrôleur KMS BDC__ pour fournir des clés et des certificats managés par le système pour le chiffrement des données au repos sur SQL Server et HDFS. Ces clés et certificats sont managés par le service et cette documentation fournit des conseils opérationnels sur la façon d’interagir avec le service.

* Les instances __SQL__ tirent parti de la fonctionnalité [Transparent Data Encryption (TDE)](../relational-databases/security/encryption/transparent-data-encryption.md) établie.
* __HDFS__ utilise le service KMS Hadoop natif dans chaque pod pour interagir avec le KMS BDC sur le contrôleur. Cela active les zones de chiffrement HDFS, qui fournissent des chemins d’accès sécurisés sur HDFS.

### <a name="sql-server-instances"></a>Instances SQL

* Un certificat généré par le système est installé sur des pods SQL Server à utiliser avec les commandes TDE. La norme de dénomination des certificats managés par le système est `TDECertificate` + `timestamp`. Par exemple : `TDECertificate2020_09_15_22_46_27`.
* Les bases de données et bases de données utilisateur fournies par le BDC de l’instance maître ne sont pas chiffrées automatiquement. Les administrateurs de bases de données peuvent utiliser le certificat installé pour chiffrer n’importe quelle base de données.
* Le pool de calcul et le pool de stockage sera automatiquement chiffré à l’aide du certificat managé par le système.
* Le chiffrement du pool de données, quoique techniquement possible à l’aide de commandes `EXECUTE AT` T-SQL, est déconseillé et n’est pas pris en charge pour l’instant. L’utilisation de cette technique pour chiffrer les bases de données du pool de données peut ne pas être efficace et le chiffrement peut ne pas se produire à l’état souhaité. Elle crée également un chemin de mise à niveau incompatible vers les prochaines mises en production.
* La rotation des clés SQL Server est obtenue à l’aide des commandes d’administration T-SQL standard. Lisez le [Guide d’utilisation du TDE (Transparent Data Encryption) au repos des clusters Big Data SQL Server](encryption-at-rest-sql-server-tde.md) pour obtenir des instructions complètes.
* L’analyse du chiffrement s’effectue par le biais des DMV SQL Server standard existantes pour TDE.
* Elle est prise en charge pour la sauvegarde et la restauration d’une base de données TDE activée dans le cluster.
* La haute disponibilité est prise en charge. Si une base de données sur l’instance principale de SQL Server est chiffrée, le réplica secondaire de la base de données est également chiffré.

### <a name="hdfs-encryption-zones"></a>Zones de chiffrement HDFS

* [L’intégration Active Directory](active-directory-prerequisites.md) est nécessaire pour activer la fonctionnalité de zones de chiffrement pour HDFS.
* Une clé managée par le système sera approvisionnée dans Hadoop KMS. Le nom de la clé est `securelakekey`. Sur CU8, la clé par défaut est 256 bits et le chiffrement AES 256 bits est pris en charge.
* Une zone de chiffrement par défaut sera approvisionnée à l’aide de la clé générée par le système ci-dessus sur un chemin d’accès nommé `/securelake`.
* Les utilisateurs peuvent créer des clés et des zones de chiffrement supplémentaires à l’aide des instructions spécifiques fournies dans ce guide. Les utilisateurs peuvent choisir la taille de clé 128, 192 ou 256 pendant la création de la clé.
* La rotation des clés des zones de chiffrement HDFS est obtenue avec azdata. Lisez le [Guide d’utilisation des zones de chiffrement HDFS des clusters Big Data SQL Server](encryption-at-rest-hdfs-encryption-zones.md) pour obtenir des instructions complètes.
* Cela n’est pas pris en charge pour effectuer un montage hiérarchisé HDFS au-dessus d’une zone de chiffrement.

## <a name="encryption-at-rest-administration"></a>Administration du chiffrement au repos

La liste suivante contient les fonctionnalités d’administration du chiffrement au repos.

* La gestion du [TDE (Transparent Data Encryption) SQL Server](encryption-at-rest-sql-server-tde.md) est effectuée à l’aide des commandes T-SQL standard.
* La gestion des clés HDFS et des [zones de chiffrement HDFS](encryption-at-rest-hdfs-encryption-zones.md) est effectuée à l’aide des commandes azdata.
* Les fonctionnalités d’administration suivantes s’effectuent avec des [notebooks opérationnels](cluster-manage-notebooks.md) :
    - Sauvegarde et récupération des clés HDFS
    - Suppression des clés HDFS

## <a name="configuration-guide"></a>Guide de configuration

Pendant de __nouveaux déploiements de Clusters Big Data SQL Server__, CU8 et versions ultérieures, __le chiffrement au repos est activé et configuré par défaut__. Cela signifie que :

* Le composant KMS BDC sera déployé dans le contrôleur et générera un ensemble de clés et de certificats par défaut .
* SQL Server est déployé avec TDE activé et les certificats sont installés par le contrôleur.
* Hadoop KMS (pour HDFS) sera configuré pour interagir avec le KMS BDC pour les opérations de chiffrement. Les zones de chiffrement HDFS sont configurées et prêtes à l’emploi.

Les spécifications et les comportements par défaut décrits dans la section précédente s’appliquent.

__Si vous effectuez un nouveau déploiement de clusters Big Data SQL Server CU8 ou une mise à niveau directe vers CU9, aucune étape supplémentaire n’est nécessaire__.

### <a name="upgrade-scenarios"></a>Scénarios de mise à niveau

Sur les clusters existants, le processus de mise à niveau n’appliquera pas de nouveau chiffrement ni de rechiffrement sur les données utilisateur qui n’étaient pas déjà chiffrées. Ce comportement est dû à la conception et les éléments suivants doivent être pris en compte pour chaque composant :

* __SQL Server__

    1. __Instance maître SQL Server__. Le processus de mise à niveau n’affecte pas les bases de données d’instance maître ni les certificats TDE installés, mais il est vivement recommandé de sauvegarder vos bases de données et vos certificats TDE installés manuellement avant le processus de mise à niveau. Il est également recommandé de stocker ces artefacts en dehors du cluster BDC SQL Server.
    1. __Pool de calcul et de stockage__. Ces bases de données sont managées par le système, volatiles et seront recréées et automatiquement chiffrées lors de la mise à niveau du cluster.
    1. __Pool de données__. La mise à niveau n’a pas d’impact sur les bases de données de la partie Instances SQL du pool de données.

* __HDFS__

    1. __HDFS__. Le processus de mise à niveau ne touche pas les fichiers et dossiers HDFS en dehors des zones de chiffrement.

### <a name="upgrading-to-cu9-from-cu8-or-earlier"></a>Mise à niveau vers CU9 à partir de CU8 ou d’une version antérieure

Aucune étape supplémentaire n’est nécessaire.

### <a name="upgrading-to-cu8-from-cu6-or-earlier"></a>Mise à niveau vers CU8 à partir de CU6 ou d’une version antérieure

   > [!CAUTION]
   > Avant de procéder à la mise à niveau vers les Clusters Big Data SQL Server CU8, effectuez une sauvegarde complète de vos données.


__Les zones de chiffrement ne seront pas configurées__. Le composant KMS Hadoop ne sera pas configuré pour utiliser le service KMS BDC. Pour configurer et activer la fonctionnalité des zones de chiffrement HDFS après la mise à niveau, suivez les instructions de la section suivante.

#### <a name="enable-hdfs-encryption-zones-after-upgrade-to-cu8"></a>Activer les zones de chiffrement HDFS après la mise à niveau vers CU8

Si vous avez mis à niveau votre cluster vers CU8 (`azdata upgrade`) et souhaitez activer des zones de chiffrement HDFS, deux possibilités s’offrent à vous :

* Exécution du [notebook opérationnel](cluster-manage-notebooks.md) Azure Data Studio nommé __SOP0128 - Activer des zones de chiffrement HDFS dans des clusters Big Data__ pour effectuer la configuration.
* Exécution du script comme décrit ci-dessous.

Conditions requises :

- Cluster [Active Directory](active-directory-prerequisites.md) intégré.

- [!INCLUDE[azdata](../includes/azure-data-cli-azdata.md)] configuré et connecté au cluster en mode AD.

Suivez la procédure ci-dessous pour reconfigurer le cluster avec supporte des zones de chiffrement.

1. Après avoir effectué la mise à niveau avec `azdata`, enregistrez les scripts suivants.

    Exigences d’exécution des scripts :
        
    * Les deux scripts doivent se trouver dans le même répertoire. 
    * `kubectl` sur CHEMIN
    * ```config``` fichier pour Kubernetes dans le dossier ```$HOME/.kube```
    
    Ce script doit être nommé __```run-key-provider-patch.sh```__ :

    ```console
    #!/bin/bash
    
    if [[ -z "${BDC_DOMAIN}" ]]; then
      echo "BDC_DOMAIN environment variable with the domain name of the cluster is not defined."
      exit 1
    fi
    
    if [[ -z "${BDC_CLUSTER_NS}" ]]; then
      echo "BDC_CLUSTER_NS environment variable with the cluster namespace is not defined."
      exit 1
    fi
    
    kubectl get configmaps -n test
    
    diff <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    diff <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py) <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | python -m json.tool)
    
    # Replace the config maps.
    #
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-storage-0-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    kubectl replace -n $BDC_CLUSTER_NS -o json -f <(kubectl get configmaps mssql-hadoop-sparkhead-configmap -n $BDC_CLUSTER_NS -o json | ./updatekeyprovider.py)
    
    # Restart the pods which need to have the necessary changes with the core-site.xml
    kubectl delete pods -n $BDC_CLUSTER_NS nmnode-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-0
    kubectl delete pods -n $BDC_CLUSTER_NS storage-0-1
    
    # Wait for sometime for pods to restart
    #
    sleep 300
    
    # Check for the KMS process status.
    #
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop nmnode-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-0 -- bash -c 'ps aux | grep kms'
    kubectl exec -n $BDC_CLUSTER_NS -c hadoop storage-0-1 -- bash -c 'ps aux | grep kms'
    ```

    Ce script doit être nommé __```updatekeyprovider.py```__ :

    ```python
    #!/usr/bin/env python3
    
    import json
    import re
    import sys
    import xml.etree.ElementTree as ET
    import os
    
    class CommentedTreeBuilder(ET.TreeBuilder):
        def comment(self, data):
            self.start(ET.Comment, {})
            self.data(data)
            self.end(ET.Comment)
    
    domain_name = os.environ['BDC_DOMAIN']
    
    parser = ET.XMLParser(target=CommentedTreeBuilder())
    
    core_site = 'core-site.xml'
    j = json.load(sys.stdin)
    cs = j['data'][core_site]
    csxml = ET.fromstring(cs, parser=parser)
    props = [prop.find('value').text for prop in csxml.findall(
        "./property/name/..[name='hadoop.security.key.provider.path']")]
    
    kms_provider_path=''
    
    for x in range(5):
        if len(kms_provider_path) != 0:
            kms_provider_path = kms_provider_path + ';'
        kms_provider_path = kms_provider_path + 'nmnode-0-0.' + domain_name
    
    if len(props) == 0:
        prop = ET.SubElement(csxml, 'property')
        name = ET.SubElement(prop, 'name')
        name.text = 'hadoop.security.key.provider.path'
        value = ET.SubElement(prop, 'value')
        value.text = 'kms://https@' + kms_provider_path + ':9600/kms'
        cs = ET.tostring(csxml, encoding='utf-8').decode('utf-8')
    
    j['data'][core_site] = cs
    
    kms_site = 'kms-site.xml.tmpl'
    ks = j['data'][kms_site]
    
    kp_uri_regex = re.compile('(<name>hadoop.kms.key.provider.uri</name>\s*<value>\s*)(.*)(\s*</value>)', re.MULTILINE)
    
    def replace_uri(match_obj):
        key_provider_uri = 'bdc://https@hdfsvault-svc.' + domain_name
        if match_obj.group(2) == 'jceks://file@/var/run/secrets/keystores/kms/kms.jceks' or match_obj.group(2) == key_provider_uri:
            return match_obj.group(1) + key_provider_uri + match_obj.group(3)
        return match_obj.group(0)
    
    ks = kp_uri_regex.sub(replace_uri, ks)
    
    j['data'][kms_site] = ks
    print(json.dumps(j, indent=4, sort_keys=True))
    ```

    Exécutez __```run-key-provider-patch.sh```__ avec les paramètres appropriés. 

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur l’utilisation efficace des Clusters Big Data SQL Server du chiffrement au repos, consultez les articles suivants :

- [Chiffrement au repos - TDE SQL Server](encryption-at-rest-sql-server-tde.md)
- [Chiffrement au repos - zones de chiffrement HDFS](encryption-at-rest-hdfs-encryption-zones.md)

Pour plus d’informations sur les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)], consultez la vue d’ensemble suivante :

- [Que sont les [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] ?](big-data-cluster-overview.md)
