---
title: Utiliser Azure Policy pour sécuriser votre cluster
description: Utilisez Azure Policy pour sécuriser un cluster Azure Kubernetes Service (AKS).
ms.service: container-service
ms.topic: how-to
ms.date: 02/17/2021
ms.custom: template-how-to
ms.openlocfilehash: 6462c2987155925b7df5241d8fb6aa13c1e37b89
ms.sourcegitcommit: 4b0e424f5aa8a11daf0eec32456854542a2f5df0
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/20/2021
ms.locfileid: "107777722"
---
# <a name="secure-your-cluster-with-azure-policy"></a>Sécuriser votre cluster avec Azure Policy

Pour améliorer la sécurité de votre cluster Azure Kubernetes Service (AKS), vous pouvez appliquer et renforcer des stratégies de sécurité intégrées sur votre cluster à l’aide d’Azure Policy. [Azure Policy][azure-policy] aide à appliquer les normes organisationnelles et à évaluer la conformité à grande échelle. Après l’installation du [module complémentaire Azure Policy pour AKS][kubernetes-policy-reference], vous pouvez appliquer des définitions de stratégie individuelles ou des groupes de définitions de stratégie appelés initiatives (parfois appelées ensembles de stratégies) à votre cluster. Pour obtenir la liste complète des définitions d’initiative et de stratégie AKS, consultez [Définitions intégrées d’Azure Policy pour Azure Kubernetes Service][aks-policies].

Cet article explique comment appliquer des définitions de stratégie à votre cluster et vérifier que ces attributions sont appliquées.

## <a name="prerequisites"></a>Prérequis

- Cluster AKS existant. Si vous avez besoin d’un cluster AKS, consultez le guide de démarrage rapide d’AKS [avec Azure CLI][aks-quickstart-cli]ou avec le [Portail Azure][aks-quickstart-portal].
- Module complémentaire Azure Policy pour AKS installé sur un cluster AKS. Pour installer le module complémentaire Azure Policy, [procédez comme suit][azure-policy-addon].

## <a name="assign-a-built-in-policy-definition-or-initiative"></a>Affecter une définition de stratégie ou une initiative intégrée

Pour appliquer une définition de stratégie ou une initiative, utilisez le Portail Azure.

1. Dans le Portail Azure, accédez au service Azure Policy.
1. Dans le volet gauche de la page Azure Policy, sélectionnez **Définitions**.
1. Sous **Catégories**, sélectionnez `Kubernetes`.
1. Choisissez la définition de stratégie ou l’initiative que vous souhaitez appliquer. Pour cet exemple, sélectionnez l’initiative `Kubernetes cluster pod security baseline standards for Linux-based workloads`.
1. Sélectionnez **Attribuer**.
1. Définissez **l’étendue** du groupe de ressources du cluster AKS avec le module complémentaire Azure Policy activé.
1. Sélectionnez la page **Paramètres** et mettez à jour **l’Effet** de `audit` à `deny` pour bloquer les nouveaux déploiements enfreignant l’initiative de base. Vous pouvez également ajouter des espaces de noms supplémentaires à exclure de l’évaluation. Pour cet exemple, conservez les valeurs par défaut.
1. Cliquez sur **Vérifier + Créer** puis **Créer** pour soumettre l’attribution des stratégies.

## <a name="validate-a-azure-policy-is-running"></a>Valider l’exécution d’une stratégie Azure Policy

Vérifiez que les attributions de stratégies sont appliquées à votre cluster en exécutant la commande suivante :

```azurecli-interactive
kubectl get constrainttemplates
```

> [!NOTE]
> Les attributions de stratégies peuvent prendre [jusqu’à 20 minutes pour se synchroniser][azure-policy-assign-policy] dans chaque cluster.

La sortie doit ressembler à ce qui suit :

```console
$ kubectl get constrainttemplate
NAME                                     AGE
k8sazureallowedcapabilities              23m
k8sazureallowedusersgroups               23m
k8sazureblockhostnamespace               23m
k8sazurecontainerallowedimages           23m
k8sazurecontainerallowedports            23m
k8sazurecontainerlimits                  23m
k8sazurecontainernoprivilege             23m
k8sazurecontainernoprivilegeescalation   23m
k8sazureenforceapparmor                  23m
k8sazurehostfilesystem                   23m
k8sazurehostnetworkingports              23m
k8sazurereadonlyrootfilesystem           23m
k8sazureserviceallowedports              23m
```

### <a name="validate-rejection-of-a-privileged-pod"></a>Valider le rejet d’un pod privilégié

Nous allons commencer par tester ce qui se passe lorsque vous planifiez un pod avec le contexte de sécurité `privileged: true`. Ce contexte de sécurité fait remonter les privilèges du pod. L’initiative n’autorise pas les pods privilégiés, de sorte que la requête est refusée, entraînant le rejet du déploiement.

Créez un fichier nommé `nginx-privileged.yaml` et collez le manifeste YAML suivant :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-privileged
spec:
  containers:
    - name: nginx-privileged
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
      securityContext:
        privileged: true
```

Créez le pod à l’aide de la commande [kubectl apply][kubectl-apply] et spécifiez le nom de votre manifeste YAML :

```console
kubectl apply -f nginx-privileged.yaml
```

LComme prévu, la planification de la commande échoue, comme indiqué dans l’exemple de sortie suivant :

```console
$ kubectl apply -f privileged.yaml

Error from server ([denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}): error when creating "privileged.yaml": admission webhook "validation.gatekeeper.sh" denied the request: [denied by azurepolicy-container-no-privilege-00edd87bf80f443fa51d10910255adbc4013d590bec3d290b4f48725d4dfbdf9] Privileged container is not allowed: nginx-privileged, securityContext: {"privileged": true}
```

Le pod n'atteint pas la phase de planification ; il n'existe aucune ressource à supprimer avant de poursuivre.

### <a name="test-creation-of-an-unprivileged-pod"></a>Tester la création d’un pod non privilégié

Dans l’exemple précédent, l’image de conteneur tentait automatiquement d’utiliser la racine pour lier NGINX au port 80. Cette requête a été refusée par l’initiative de la stratégie, de sorte que le pod ne démarre pas. Essayons maintenant d’exécuter ce même pod NGINX sans la requête d’accès privilégié.

Créez un fichier nommé `nginx-unprivileged.yaml` et collez le manifeste YAML suivant :

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-unprivileged
spec:
  containers:
    - name: nginx-unprivileged
      image: mcr.microsoft.com/oss/nginx/nginx:1.15.5-alpine
```

Créez le pod à l’aide de la commande [kubectl apply][kubectl-apply] et spécifiez le nom de votre manifeste YAML :

```console
kubectl apply -f nginx-unprivileged.yaml
```

Le pod est planifié avec succès. Lorsque vous vérifiez l’état du pod à l’aide de la commande [kubectl get pods][kubectl-get], le pod est *en cours d’exécution* :

```console
$ kubectl get pods

NAME                 READY   STATUS    RESTARTS   AGE
nginx-unprivileged   1/1     Running   0          18s
```

Cet exemple montre l’initiative de base qui affecte uniquement les déploiements violant les stratégies de la collection. Les déploiements autorisés continuent à fonctionner.

Supprimez le pod non privilégié NGINX à l’aide de la commande [kubectl delete][kubectl-delete] et spécifiez le nom de votre manifeste YAML :

```console
kubectl delete -f nginx-unprivileged.yaml
```

## <a name="disable-a-policy-or-initiative"></a>Désactiver une stratégie ou une initiative

Pour supprimer l’initiative de la ligne de base :

1. Accédez au volet de Stratégie sur le Portail Azure.
1. Sélectionnez **Attributions** dans le volet gauche.
1. Cliquez sur le bouton **...** en regard de l’initiative `Kubernetes cluster pod security baseline standards for Linux-based workloads`.
1. Sélectionnez **Supprimer l’attribution**.

## <a name="next-steps"></a>Étapes suivantes

Pour plus d’informations sur le fonctionnement d’Azure Policy :

- [Vue d’ensemble d’Azure Policy][azure-policy]
- [Initiatives et stratégies Azure Policy pour AKS][aks-policies]
- Supprimez le [module complémentaire Azure Policy][azure-policy-addon-remove].

<!-- LINKS - external -->
[kubectl-apply]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply
[kubectl-delete]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete
[kubectl-get]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get
[kubectl-create]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#create
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe
[kubectl-logs]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#logs

<!-- LINKS - internal -->
[aks-policies]: policy-reference.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[azure-policy]: ../governance/policy/overview.md
[azure-policy-addon]: ../governance/policy/concepts/policy-for-kubernetes.md#install-azure-policy-add-on-for-aks
[azure-policy-addon-remove]: ../governance/policy/concepts/policy-for-kubernetes.md#remove-the-add-on-from-aks
[azure-policy-assign-policy]: ../governance/policy/concepts/policy-for-kubernetes.md#assign-a-built-in-policy-definition
[az-aks-get-credentials]: /cli/azure/aks#az_aks_get_credentials
[kubernetes-policy-reference]: ../governance/policy/concepts/policy-for-kubernetes.md
