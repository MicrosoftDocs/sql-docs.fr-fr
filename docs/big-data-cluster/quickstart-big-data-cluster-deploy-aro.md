---
title: Script Python de déploiement sur Azure Red Hat OpenShift
titleSuffix: SQL Server Big Data Clusters
description: Découvrez comment utiliser un script de déploiement pour déployer des clusters Big Data SQL Server sur Azure Red Hat OpenShift (ARO).
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: c5889a9411c54312dd52ee1a5e9f2b7917da6838
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/17/2020
ms.locfileid: "97637434"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>Déploiement d’un cluster Big Data SQL Server sur Azure Red Hat OpenShift (ARO) avec un script Python

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

Dans ce tutoriel, vous allez utiliser un exemple de script de déploiement Python pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] sur [Azure Red Hat OpenShift (ARO)](/azure/virtual-machines/linux/openshift-get-started). Cette option de déploiement est prise en charge à compter de SQL Server 2019 CU5.

> [!TIP]
> ARO n’est qu’une option d’hébergement de Kubernetes parmi d’autres pour un cluster Big Data. Pour découvrir les autres options de déploiement et savoir comment les personnaliser, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md).


> [!WARNING]
> Les volumes persistants créés avec la classe de stockage intégrée *managed-premium* ont une stratégie de récupération *Delete (Supprimer)* . Lorsque vous supprimez le cluster Big Data SQL Server, les revendications de volumes persistants sont supprimées, puis les volumes persistants le sont. Vous devez créer des classes de stockage personnalisées en utilisant le provisionneur azure-disk avec une stratégie de récupération *Conserver*, comme décrit dans [Stockage de concepts](/azure/aks/concepts-storage/#storage-classes). Le script ci-dessous utilise la classe de stockage *managed-premium*. Pour plus d’informations, consultez la rubrique [Persistance des données](concept-data-persistence.md).

Le déploiement de cluster Big Data par défaut utilisé ici se compose d’une instance principale SQL, d’une instance de pool de calcul, de deux instances de pool de données et de deux instances de pool de stockage. Les données sont conservées avec des volumes persistants Kubernetes qui utilisent les classes de stockage par défaut d’ARO. La configuration par défaut utilisée dans ce tutoriel est adaptée aux environnements de développement et de test.

## <a name="prerequisites"></a>Prérequis

- Un abonnement Azure.
- [oc](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Python 3.0 minimum](https://www.python.org/downloads)
- [Interface CLI `az`](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>Se connecter à votre compte Azure

Le script utilise Azure CLI pour automatiser la création d’un cluster ARO. Avant d’exécuter le script, vous devez vous connecter à votre compte Azure avec Azure CLI au moins une fois. Exécutez la commande suivante à partir d’une invite de commandes.

```terminal
az login
```

## <a name="instructions"></a>Instructions

1. Téléchargez le script Python `deploy-sql-big-data-aro.py` et le fichier YAML `bdc-scc.yaml`.

   >Ces fichiers se trouvent dans cet article sous :
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. Exécutez le script à l’aide de la commande suivante :

```terminal
python deploy-sql-big-data-aro.py
```

À l’invite, fournissez en entrée votre ID d’abonnement Azure et le groupe de ressources Azure dans lequel les ressources seront créées. Vous pouvez également indiquer d’autres configurations ou utiliser les valeurs par défaut fournies. Par exemple :

- `azure_region`
- `vm_size` pour les nœuds Worker OpenShift. Pour une expérience optimale de validation des scénarios de base, nous vous recommandons d’utiliser au moins huit processeurs virtuels et 64 Go de mémoire sur tous les nœuds Worker du cluster. Le script utilise par défaut `Standard_D8s_v3` et trois nœuds Worker. Une configuration de taille des clusters Big Data par défaut exploite également environ 24 disques pour les revendications de volume persistant sur tous les composants.
- Configuration réseau du déploiement de cluster OpenShift. Pour plus d’informations sur chaque paramètre, consultez l’article [Déploiement d’ARO](\azure\openshift\tutorial-create-cluster).
- `cluster_name` : cette valeur est utilisée pour le cluster ARO et le cluster Big Data SQL Server qui s’appuie dessus. Notez que le nom du cluster Big Data SQL sera un espace de noms Kubernetes.
- `username ` : il s’agit du nom d’utilisateur associé aux comptes provisionnés pendant le déploiement du compte d’administrateur du contrôleur, du compte de l’instance maître SQL Server et de la passerelle. Notez que le compte SQL Server `sa` est désactivé automatiquement, conformément aux meilleures pratiques.
- `password` : la même valeur sera utilisée pour tous les comptes.

Le cluster Big Data SQL Server est maintenant déployé sur ARO. Vous pouvez maintenant utiliser Azure Data Studio pour vous connecter au cluster. Pour plus d’informations, consultez [Se connecter à un cluster Big Data SQL Server avec Azure Data Studio](connect-to-big-data-cluster.md).

## <a name="clean-up"></a>Nettoyer

Si vous testez [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] dans Azure, supprimez le cluster ARO une fois que vous avez fini pour éviter les charges inattendues. Ne supprimez pas le cluster si vous envisagez de continuer à l’utiliser.

> [!WARNING]
> Les étapes suivantes détruisent le cluster ARO, ce qui supprime également le cluster Big Data SQL Server. Si vous voulez conserver des bases de données ou des données HDFS, sauvegardez-les avant de supprimer le cluster.

Exécutez la commande Azure CLI suivante pour supprimer le cluster Big Data et le service ARO dans Azure (remplacez `<resource group name>` par le **groupe de ressources Azure** spécifié dans le script de déploiement) :

```terminal
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

Le script de cette section déploie le cluster Big Data SQL Server dans Azure Red Hat OpenShift. Copiez le script sur votre station de travail et enregistrez-le en tant que `deploy-sql-big-data-aro.py` avant de commencer le déploiement.

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

Le manifeste .yaml suivant définit une contrainte de contexte de sécurité personnalisée pour le déploiement Clusters Big Data. Copiez-le dans le même répertoire que `deploy-sql-big-data-aro.py`.

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>Étapes suivantes

Le script de déploiement a configuré Azure Kubernetes Service et a également déployé un cluster Big Data SQL Server 2019. Vous pouvez également choisir de personnaliser des déploiements futurs via des installations manuelles. Pour en savoir plus sur le déploiement des clusters Big Data et sur la personnalisation des déploiements, consultez [Guide pratique pour déployer [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] sur Kubernetes](deployment-guidance.md).

Maintenant que le cluster Big Data SQL Server est déployé, vous pouvez charger des exemples de données et explorer les tutoriels :

> [!div class="nextstepaction"]
> [Tutoriel : Charger un exemple de données dans un cluster Big Data SQL Server 2019](tutorial-load-sample-data.md)