---
title: 'Interface CLI : Déployer à partir d’un dépôt Git local'
description: Découvrez comment utiliser Azure CLI pour automatiser le déploiement et la gestion de votre application App Service. Cet exemple montre comment déployer du code à partir d’un dépôt Git.
author: msangapu-msft
tags: azure-service-management
ms.assetid: 048f98aa-f708-44cb-9b9e-953f67dc6da8
ms.devlang: azurecli
ms.topic: sample
ms.date: 12/11/2017
ms.author: msangapu
ms.custom: mvc, seodec18, devx-track-azurecli
ms.openlocfilehash: d0be7b057a54c23b15fc4e898687d434c18a956e
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107787841"
---
# <a name="create-an-app-service-app-and-deploy-code-from-a-local-git-repository-using-azure-cli"></a>Créer une application App Service et déployer le code à partir d’un dépôt Git local à l’aide d’Azure CLI

Cet exemple de script crée une application dans App Service avec ses ressources associées, puis déploie votre code d’application dans un dépôt Git local.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

 - Ce tutoriel nécessite l’interface Azure CLI version 2.0 ou ultérieure. Si vous utilisez Azure Cloud Shell, la version la plus récente est déjà installée.

## <a name="sample-script"></a>Exemple de script

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/deploy-local-git/deploy-local-git.sh?highlight=3-5 "Create an app and deploy code from a local Git repository")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explication du script

Ce script utilise les commandes suivantes. Chaque commande du tableau renvoie à une documentation spécifique.

| Commande | Notes |
|---|---|
| [`az group create`](/cli/azure/group#az_group_create) | Crée un groupe de ressources dans lequel toutes les ressources sont stockées. |
| [`az appservice plan create`](/cli/azure/appservice/plan#az_appservice_plan_create) | Crée un plan App Service. |
| [`az webapp create`](/cli/azure/webapp#az_webapp_create) | Crée une application App Service. |
| [`az webapp deployment user set`](/cli/azure/webapp/deployment/user#az_webapp_deployment_user_set) | Définit les informations d’identification de déploiement au niveau des comptes pour App Service. |
| [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config_local_git) | Crée une configuration de contrôle source pour un référentiel Git local. |

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur l’interface Azure CLI, consultez la [documentation relative à l’interface Azure CLI](/cli/azure).

Vous trouverez des exemples supplémentaires de scripts CLI App Service dans la [documentation relative à Azure App Service](../samples-cli.md).
