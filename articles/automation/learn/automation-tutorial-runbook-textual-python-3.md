---
title: Créer un runbook Python 3 (préversion) dans Azure Automation
description: Cet article vous apprend à créer, tester et publier un runbook Python 3 simple (préversion) dans votre compte Azure Automation.
services: automation
ms.subservice: process-automation
ms.date: 04/28/2021
ms.topic: tutorial
ms.custom: has-adal-ref, devx-track-python
ms.openlocfilehash: 4badf99bcae4fa6d32492960f3e5322ee3508f25
ms.sourcegitcommit: a5dd9799fa93c175b4644c9fe1509e9f97506cc6
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/28/2021
ms.locfileid: "108205050"
---
# <a name="tutorial-create-a-python-3-runbook-preview"></a>Tutoriel : Créer un runbook Python 3 (préversion)

Ce tutoriel vous explique comment créer un [runbook Python 3](../automation-runbook-types.md#python-runbooks) (préversion) dans Azure Automation. Les runbooks Python sont compilés sous Python 2 et 3. Vous pouvez modifier directement le code du runbook à l’aide de l’éditeur de texte du portail Azure.

> [!div class="checklist"]
> * Créer un runbook Python simple
> * Tester et publier le runbook
> * Exécuter le travail du runbook et en suivre l’état
> * Mettre à jour le runbook pour démarrer une machine virtuelle Azure avec des paramètres de runbook

## <a name="prerequisites"></a>Prérequis

Pour réaliser ce didacticiel, vous avez besoin des éléments suivants :

- Abonnement Azure. Si vous n’avez pas encore d’abonnement, vous pouvez [activer vos avantages abonnés MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) ou créer [un compte gratuit](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

- [compte Automation](../automation-security-overview.md) pour le stockage du Runbook et l’authentification auprès des ressources Azure. Ce compte doit avoir l’autorisation de démarrer et d’arrêter la machine virtuelle. Le [compte d’identification](../automation-security-overview.md#run-as-accounts) est nécessaire pour ce tutoriel. 

- Une machine virtuelle Azure. Dans ce tutoriel, vous allez démarrer et arrêter cette machine. Il ne doit donc pas s’agir d’une machine virtuelle de production.

## <a name="create-a-new-runbook"></a>Créer un Runbook

Commençons par créer un runbook simple qui retourne le texte *Hello World*.

1. Dans le portail Azure, ouvrez votre compte Automation.

    La page du compte Automation vous offre un aperçu rapide des ressources de ce compte. Vous devriez déjà posséder certains éléments. La plupart de ces éléments représentent les modules automatiquement inclus dans un nouveau compte Automation. Vous devez aussi disposer des informations d’identification du compte d’identification mentionnées dans les [prérequis](#prerequisites).

2. Sélectionnez **Runbooks** sous **Automatisation de processus** pour ouvrir la liste des runbooks.

3. Sélectionnez **Ajouter un runbook** pour créer un runbook.

4. Nommez le runbook **MyFirstRunbook-Python**.

5. Sélectionnez **Python 3** pour le **Type de runbook**.

6. Sélectionnez **Créer** pour créer le runbook et ouvrez l’éditeur de texte.

## <a name="add-code-to-the-runbook"></a>Ajouter du code au Runbook

Ajoutez maintenant une commande simple pour imprimer le texte `Hello World`.

```python
print("Hello World!")
```

Sélectionnez **Enregistrer** pour enregistrer le runbook.

## <a name="test-the-runbook"></a>Tester le Runbook

Avant de publier le runbook pour le rendre disponible en production, vous voulez le tester pour vous assurer qu’il fonctionne correctement. Lorsque vous testez un runbook, vous exécutez son brouillon et affichez sa sortie de manière interactive.

1. Sélectionnez l’option **Volet de test** pour ouvrir le volet **Test**.

2. Sélectionnez **Démarrer** pour démarrer le test. Ce doit être la seule option activée.

3. Une [tâche de Runbook](../automation-runbook-execution.md) est créée et son état apparaît.
   L’état initial de la tâche est **Mis en file d’attente** pour indiquer que la tâche attend qu’un Runbook Worker du cloud devienne disponible. Il passe à **En cours de démarrage** lorsqu’un Worker sélectionne la tâche, puis à **En cours d’exécution** lorsque le runbook se lance.

4. Lorsque la tâche du Runbook est terminée, sa sortie s'affiche. Dans ce cas, `Hello World` devrait apparaître.

5. Fermez le volet **Test** pour revenir au canevas.

## <a name="publish-and-start-the-runbook"></a>Publier et démarrer le Runbook

Le runbook que vous avez créé est toujours en mode brouillon. Il faut le publier pour pouvoir l’exécuter en production. Lorsque vous publiez un Runbook, vous écrasez la version publiée existante par la version brouillon. Dans ce cas, vous n’avez pas encore de version publiée car vous venez de créer le runbook.

1. Sélectionnez l’option **Publier** pour publier le runbook, puis cliquez sur **Oui** quand vous y êtes invité.

2. Si vous faites défiler la page vers la gauche pour visualiser la page **Runbooks**, vous devriez voir l’**État de création** **Publié**.

3. Faites défiler la page vers la droite pour visualiser le volet **MyFirstRunbook-Python3**.

   Les options de la partie supérieure vous permettent de démarrer le runbook, de l’afficher et de planifier son démarrage.

4. Sélectionnez **Démarrer** puis **OK** à l’ouverture du volet **Démarrer le runbook**.

5. Le volet **Tâche** s’ouvre pour la tâche du runbook qui vient d’être créée. Vous pouvez le fermer, mais laissez-le ouvert pour pouvoir suivre la progression de la tâche.

6. L’état de la tâche, indiqué dans le champ **Résumé de la tâche**, correspond aux états constatés lors du test du runbook.

7. Lorsque l’état du runbook est **Terminé**, sélectionnez **Sortie**. Le volet **Sortie** s’ouvre et vous pouvez y voir `Hello World`.

8. Fermez le volet **Sortie**.

9. Sélectionnez **Tous les journaux** pour ouvrir le volet **Flux** de la tâche du runbook. Vous ne devriez voir que `Hello World` dans le flux de sortie. Cependant, ce volet peut afficher d’autres flux pour une tâche de runbook, comme des **détails** ou des **erreurs**, si le runbook écrit des données dans ceux-ci.

10. Fermez les volets **Flux** et **Tâche** pour revenir au volet **MyFirstRunbook-Python3**.

11. Sélectionnez **Tâches** pour ouvrir la page **Tâches** de ce runbook. Cette page répertorie toutes les tâches créées par ce runbook. Vous devez voir un seul travail, car vous n’avez exécuté le travail qu’une seule fois.

12. Vous pouvez sélectionner ce travail pour ouvrir le même volet **Tâche** que celui vous avez consulté au démarrage du runbook. Ce volet vous permet de revenir en arrière et d’afficher les détails de toute tâche créée pour un runbook donné.

## <a name="add-authentication-to-manage-azure-resources"></a>Ajouter une authentification pour gérer les ressources Azure

Vous avez testé et publié votre runbook, mais jusqu’à présent, il ne fait rien d’utile. Vous souhaitez qu’il gère les ressources Azure.
Pour ce faire, le script doit s’authentifier à l’aide des informations d’identification du compte d’identification de votre compte Automation.

> [!NOTE]
> Le compte Automation doit avoir été créé avec le compte d’identification pour qu’un certificat d’identification puisse être exécuté.
> Si vous n’avez pas créé votre compte Automation avec le compte d’identification, vous pouvez vous authentifier comme décrit dans [S’authentifier avec les bibliothèques de gestion Azure pour Python](/azure/python/python-sdk-azure-authenticate) ou [Créer un compte d’identification](../create-run-as-account.md).

1. Ouvrez l’éditeur de texte en sélectionnant **Modifier** dans le volet **MyFirstRunbook-Python3**.

2. Ajoutez le code suivant pour vous authentifier dans Azure :

    ```python
    import os
    from azure.mgmt.compute import ComputeManagementClient
    import azure.mgmt.resource
    import automationassets
    
    def get_automation_runas_credential(runas_connection):
        from OpenSSL import crypto
        import binascii
        from msrestazure import azure_active_directory
        import adal
    
        # Get the Azure Automation RunAs service principal certificate
        cert = automationassets.get_automation_certificate("AzureRunAsCertificate")
        pks12_cert = crypto.load_pkcs12(cert)
        pem_pkey = crypto.dump_privatekey(crypto.FILETYPE_PEM,pks12_cert.get_privatekey())
    
        # Get run as connection information for the Azure Automation service principal
        application_id = runas_connection["ApplicationId"]
        thumbprint = runas_connection["CertificateThumbprint"]
        tenant_id = runas_connection["TenantId"]
    
        # Authenticate with service principal certificate
        resource ="https://management.core.windows.net/"
        authority_url = ("https://login.microsoftonline.com/"+tenant_id)
        context = adal.AuthenticationContext(authority_url)
        return azure_active_directory.AdalAuthentication(
        lambda: context.acquire_token_with_client_certificate(
                resource,
                application_id,
                pem_pkey,
                thumbprint)
        )
    
    # Authenticate to Azure using the Azure Automation RunAs service principal
    runas_connection = automationassets.get_automation_connection("AzureRunAsConnection")
    azure_credential = get_automation_runas_credential(runas_connection)
    ```

## <a name="add-code-to-create-python-compute-client-and-start-the-vm"></a>Ajouter du code pour créer le client Compute Python et démarrer la machine virtuelle

Pour utiliser des machines virtuelles Azure, créez une instance du [client Compute Azure pour Python](/python/api/azure-mgmt-compute/azure.mgmt.compute.computemanagementclient).

Utilisez le client Compute pour démarrer la machine virtuelle. Ajoutez le code suivant au Runbook :

```python
# Initialize the compute management client with the Run As credential and specify the subscription to work against.
compute_client = ComputeManagementClient(
    azure_credential,
    str(runas_connection["SubscriptionId"])
)


print('\nStart VM')
async_vm_start = compute_client.virtual_machines.start(
    "MyResourceGroup", "TestVM")
async_vm_start.wait()
```

`MyResourceGroup` correspondant au nom du groupe de ressource qui contient la machine virtuelle, et `TestVM` au nom de la machine virtuelle que vous souhaitez démarrer.

Testez et exécutez une nouvelle fois le Runbook pour vérifier qu’il démarre la machine virtuelle.

## <a name="use-input-parameters"></a>Utilisez les paramètres d’entrée

Actuellement, le runbook utilise des valeurs codées en dur pour le nom du groupe de ressources et celui de la machine virtuelle. Nous ajoutons maintenant du code pour obtenir ces valeurs des paramètres d’entrée.

Utilisez la variable `sys.argv` pour obtenir les valeurs du paramètre. Ajoutez le code suivant au Runbook à la suite des autres instructions `import` :

```python
import sys

resource_group_name = str(sys.argv[1])
vm_name = str(sys.argv[2])
```

Cette action importe le module `sys` et crée deux variables pour stocker le nom du groupe de ressources et celui de la machine virtuelle. Remarque : l’élément `sys.argv[0]` dans la liste des arguments correspond au nom du script et n’est pas entré par l’utilisateur.

Vous pouvez maintenant modifier les deux dernières lignes du runbook de façon à utiliser les valeurs du paramètre d’entrée à la place des valeurs codées en dur :

```python
async_vm_start = compute_client.virtual_machines.start(
    resource_group_name, vm_name)
async_vm_start.wait()
```

Lorsque vous démarrez un runbook Python (dans le volet **Test** ou en tant que runbook publié), vous pouvez entrer les valeurs des paramètres dans la page **Démarrer le runbook** sous **Paramètres**.

Après avoir entré une valeur dans la première boîte, une deuxième apparaît, et ainsi de suite. Vous pouvez entrer autant de valeurs de paramètres que nécessaire.

Les valeurs sont disponibles pour le script dans le tableau `sys.argv`, comme dans le code qui vient d’être ajouté.

Entrez le nom du groupe de ressources comme valeur du premier paramètre, et le nom de la machine virtuelle à démarrer comme valeur du deuxième paramètre.

![Entrer des valeurs de paramètres](../media/automation-tutorial-runbook-textual-python/runbook-python-params.png)

Sélectionnez **OK** pour démarrer le runbook. Le Runbook s’exécute et démarre la machine virtuelle que vous avez spécifiée.

## <a name="error-handling-in-python&quot;></a>Gestion des erreurs dans Python

Vous pouvez également utiliser les conventions suivantes pour récupérer différents flux à partir de vos runbooks Python, notamment les flux AVERTISSEMENT, ERREUR et DÉBOGAGE.

```python
print(&quot;Hello World output")
print("ERROR: - Hello world error")
print("WARNING: - Hello world warning")
print("DEBUG: - Hello world debug")
print("VERBOSE: - Hello world verbose")
```

L’exemple suivant illustre cette Convention utilisée dans un bloc `try...except`.

```python
try:
    raise Exception('one', 'two')
except Exception as detail:
    print ('ERROR: Handling run-time error:', detail)
```

## <a name="next-steps"></a>Étapes suivantes

- Pour en savoir plus sur les types de runbooks, leurs avantages et leurs limites, consultez [Types de runbooks Azure Automation](../automation-runbook-types.md).

- Pour en savoir plus sur le développement pour Azure avec Python, consultez [Azure pour les développeurs Python](/azure/python/).

- Pour voir des exemples de runbooks Python 3, consultez le dépôt [GitHub Azure Automation](https://github.com/azureautomation/runbooks/tree/master/Utility/Python).
