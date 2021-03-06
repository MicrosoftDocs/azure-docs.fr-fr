---
title: Tutoriel expliquant comment filtrer et analyser des données sur Azure Stack Edge Pro FPGA pour un déploiement avancé avec un rôle de calcul
description: Apprenez à configurer un rôle de calcul sur Azure Stack Edge Pro FPGA et à l'utiliser pour transformer des données à destination d'un flux de déploiement avancé avant de les envoyer à Azure.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 01/06/2021
ms.author: alkohli
ms.openlocfilehash: 8f6fff328e90c37804e86e4b258cbcd0cb2255d7
ms.sourcegitcommit: 80d311abffb2d9a457333bcca898dfae830ea1b4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/26/2021
ms.locfileid: "110461461"
---
# <a name="tutorial-transform-data-with-azure-stack-edge-pro-fpga-for-advanced-deployment-flow"></a>Tutoriel : Transformer des données avec Azure Stack Edge Pro FPGA pour un flux de déploiement avancé

Ce tutoriel explique comment configurer un rôle de calcul pour un flux de déploiement avancé sur votre appareil Azure Stack Edge Pro FPGA. Une fois le rôle de calcul configuré, Azure Stack Edge Pro FPGA peut transformer des données avant de les envoyer à Azure.

Le calcul peut être configuré pour le flux d’un déploiement simple ou avancé sur votre appareil.

| Critères | Déploiement simple                                | Déploiement avancé                   |
|------------------|--------------------------------------------------|---------------------------------------|
| Usage prévu     | Administrateurs informatiques                                | Développeurs                            |
| Type             | Utiliser le service Azure Stack Edge pour déployer des modules      | Utiliser le service IoT Hub pour déployer des modules |
| Modules déployés | Unique                                           | Modules chaînés ou multiples           |


Cette procédure peut prendre environ 20 à 30 minutes.

Dans ce tutoriel, vous allez apprendre à :

> [!div class="checklist"]
> * Configurer le calcul
> * Ajouter des partages
> * Ajouter un déclencheur
> * Ajouter un module de calcul
> * Vérifier la transformation des données et transférer

 
## <a name="prerequisites"></a>Prérequis

Avant de configurer un rôle de calcul sur votre appareil Azure Stack Edge Pro FPGA, vérifiez que :

- Vous avez activé votre appareil Azure Stack Edge Pro FPGA, comme décrit dans [Connecter, configurer et activer Azure Stack Edge Pro FPGA](azure-stack-edge-deploy-connect-setup-activate.md).


## <a name="configure-compute"></a>Configurer le calcul

Pour configurer le calcul sur votre appareil Azure Stack Edge Pro FPGA, vous allez créer une ressource IoT Hub.

1. Dans le portail Azure de votre ressource Azure Stack Edge, accédez à **Vue d’ensemble**. Dans le volet droit, sélectionnez la vignette **IoT Edge**.

    ![Bien démarrer avec le calcul](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-1.png)

2. Sur la vignette **Activer le service IoT Edge**, sélectionnez **Ajouter**. Cette action active le service IoT Edge qui vous permet de déployer des modules IoT Edge localement sur votre appareil.

    ![Bien démarrer avec le calcul 2](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-2.png)

3. Dans **Create IoT Edge service** (Créer le service IoT Edge), entrez les informations suivantes :

   
    |Champ  |Valeur  |
    |---------|---------|
    |Abonnement     |Sélectionnez un abonnement pour votre ressource IoT Hub. Vous pouvez choisir le même abonnement que celui utilisé par la ressource Azure Stack Edge.        |
    |Resource group     |Entrez un nom pour le groupe de ressources de votre ressource IoT Hub. Vous pouvez choisir le même groupe de ressources que celui utilisé par la ressource Azure Stack Edge.         |
    |IoT Hub     | Choisissez **Nouveau** ou **Existant**. <br> Par défaut, un niveau Standard (S1) est utilisé pour créer une ressource IoT. Pour utiliser une ressource IoT de niveau gratuit, créez-en une, puis sélectionnez-la.      |
    |Name     |Acceptez le nom par défaut ou entrez un autre nom pour votre ressource IoT Hub.         |

    ![Bien démarrer avec le calcul 3](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-3.png)

4. Sélectionnez **Vérifier + créer**. La création de ressources IoT Hub prend quelques minutes. Une fois la ressource IoT Hub créée, la page **Vue d’ensemble**  est mise à jour pour avertir que le service IoT Edge est en cours d’exécution. 

    Quand le service IoT Edge est configuré sur l’appareil Edge, il crée deux appareils : un appareil IoT et un appareil IoT Edge. Ces deux appareils peuvent être visualisés dans la ressource IoT Hub. Un runtime IoT Edge est également exécuté sur cet appareil IoT Edge. À ce stade, seule la plateforme Linux est disponible pour votre appareil IoT Edge.

    Pour vérifier que le rôle de calcul Edge a été configuré, sélectionnez **Service IoT Edge > Propriétés**, et examinez l’appareil IoT et l’appareil IoT Edge. 

    ![Bien démarrer avec le calcul 4](./media/azure-stack-edge-deploy-configure-compute-advanced/configure-compute-4.png)
    

## <a name="add-shares"></a>Ajouter des partages

Pour le déploiement avancé dans ce tutoriel, vous aurez besoin de deux partages : un partage Edge et un autre partage local Edge.

1. Ajouter un partage Edge sur l’appareil en effectuant les étapes suivantes :

    1. Dans votre ressource Azure Stack Edge, accédez à **IoT Edge > Partages**.
    2. Dans la page **Partages**, dans la barre de commandes, sélectionnez **+ Ajouter un partage**.
    3. Sur le panneau **Ajouter un partage**, fournissez le nom du partage et sélectionnez le type de partage.
    4. Pour monter le partage Edge, cochez la case **Utiliser le partage avec le computing en périphérie**.
    5. Sélectionnez le **Compte de stockage**, le **Service de stockage**, un utilisateur existant, puis sélectionnez **Créer**.

        ![Ajouter un partage Edge](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-1.png)

    Une fois le partage Edge créé, vous recevrez une notification de création réussie. La liste de partage est mise à jour pour refléter le nouveau partage.

2. Ajoutez un partage local Edge sur l’appareil Edge en répétant les étapes décrites à l’étape précédente et en cochant la case **Configurer en tant que partage local Edge**. Les données dans le partage local restent sur l’appareil.

    ![Ajouter un partage local Edge](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-2.png)

3. Dans le panneau **Partages**, vous voyez la liste des partages mise à jour.

    ![Liste des partages mise à jour](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-3.png)

4. Pour afficher les propriétés du nouveau partage local, sélectionnez-le dans la liste. Dans la zone **Point de montage local pour les modules de computing en périphérie**, copiez la valeur correspondant à ce partage.

    Vous utiliserez ce point de montage local quand vous déploierez le module.

    ![Zone « Point de montage local pour les modules de computing en périphérie »](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-4.png)
 
5. Pour afficher les propriétés du partage Edge que vous avez créé, sélectionnez-le dans la liste. Dans la zone **Point de montage local pour les modules de computing en périphérie**, copiez la valeur correspondant à ce partage.

    Vous utiliserez ce point de montage local quand vous déploierez le module.

    ![Ajouter un module personnalisé](./media/azure-stack-edge-deploy-configure-compute-advanced/add-edge-share-5.png)


## <a name="add-a-trigger"></a>Ajouter un déclencheur

1. Accédez à votre ressource Azure Stack Edge, puis accédez à **IoT Edge > Déclencheurs**. Sélectionnez **+ Ajouter un déclencheur**.

    ![Ajouter un déclencheur](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-1.png)

2. Dans le panneau **Ajouter un déclencheur**, entrez les valeurs suivantes.

    |Champ  |Valeur  |
    |---------|---------|
    |Nom du déclencheur     | Nom unique de votre déclencheur.         |
    |Type de déclencheur     | Sélectionnez le déclencheur **Fichier**. Un déclencheur de fichier est activé chaque fois qu’un événement de fichier se produit, tel que l’écriture d’un fichier sur le partage d’entrée. Un déclencheur planifié, quant à lui, se déclenche en fonction d’une planification définie par vos soins. Pour cet exemple, nous avons besoin d’un déclencheur de fichier.    |
    |Partage d’entrée     | Sélectionnez un partage d’entrée. Dans ce cas, le partage local Edge est le partage d’entrée. Le module utilisé ici déplace les fichiers depuis le partage local Edge vers un partage Edge où ils sont chargés sur le cloud.        |

    ![Ajouter un déclencheur 2](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-2.png)

3. Un message vous informe que le déclencheur a bien été créé. La liste des déclencheurs est mise à jour pour afficher le nouveau déclencheur. Sélectionnez le déclencheur que vous venez de créer.

    ![Ajouter un déclencheur 3](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-3.png)

4. Copiez et enregistrez l’exemple de route. Vous le modifierez et l’utiliserez ultérieurement dans le hub IoT.

    `"sampleroute&quot;: &quot;FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/modulename/inputs/input1\")"`

    ![Ajouter un déclencheur 4](./media/azure-stack-edge-deploy-configure-compute-advanced/add-trigger-4.png)

## <a name="add-a-module"></a>Ajouter un module

Il n’existe aucun module personnalisé sur cet appareil Edge. Vous pouvez ajouter un module prédéfini ou personnalisé. Pour apprendre à créer un module personnalisé, accédez à [Développer un module C# pour votre appareil Azure Stack Edge Pro FPGA](azure-stack-edge-create-iot-edge-module.md).

Dans cette section, vous allez ajouter un module personnalisé à l'appareil IoT Edge que vous avez créé dans [Développer un module C# pour votre appareil Azure Stack Edge Pro FPGA](azure-stack-edge-create-iot-edge-module.md). Ce module personnalisé place des fichiers d’un partage local Edge sur l’appareil de périphérie, puis les déplace vers un partage Edge (cloud) sur l’appareil. Le partage cloud envoie ensuite les fichiers vers le compte de stockage Azure associé au partage cloud.

1. Accédez à votre ressource Azure Stack Edge, puis à **IoT Edge > Vue d’ensemble**. Sur la vignette **Modules**, sélectionnez **Accéder à Azure IoT Hub**.

    ![Sélectionner le déploiement avancé](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-1.png)

<!--2. In the **Configure and add module** blade, input the following values:  

    |Input share     | Select an input share. The Edge local share is the input share in this case. The module used here moves files from the Edge local share to an Edge share where they are uploaded into the cloud.        |
    |Output share     | Select an output share. The Edge share is the output share in this case.        |
-->

2. Dans votre ressource IoT Hub, accédez à **Appareil IoT Edge**, puis sélectionnez votre appareil IoT Edge.

    ![Accéder à l’appareil IoT Edge dans IoT Hub](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-2.png)

3. Dans **Détails sur l’appareil**, sélectionnez **Définir des modules**.

    ![Lien Définir des modules](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-3.png)

4. Sous **Ajouter des modules**, effectuez les étapes suivantes :

    1. Entrez le nom, l’adresse, le nom d’utilisateur et le mot de passe dans les paramètres du registre de conteneurs du module personnalisé.
    Le nom, l’adresse et les informations d’identification indiquées sont utilisées pour récupérer des modules avec une URL correspondante. Pour déployer ce module, sous **Deployment modules** (Modules de déploiement), sélectionnez **IoT Edge module** (Module IoT Edge). Ce module IoT Edge est un conteneur Docker que vous pouvez déployer sur l'appareil IoT Edge associé à votre appareil Azure Stack Edge Pro FPGA.

        ![Page Définir des modules](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-4.png) 
 
    2. Spécifiez les paramètres du module personnalisé IoT Edge. Entrez les valeurs suivantes.
     
        |Champ  |Valeur  |
        |---------|---------|
        |Nom     | Nom unique pour le module. Ce module est un conteneur Docker que vous pouvez déployer sur l'appareil IoT Edge associé à votre Azure Stack Edge Pro FPGA.        |
        |URI d’image     | URI d’image de l’image conteneur associée pour le module.        |
        |Informations d’identification obligatoires     | Si cette case est cochée, le nom d’utilisateur et le mot de passe sont utilisés pour récupérer les modules avec une URL correspondante.        |
    
        Dans la zone **Options de création de conteneur**, entrez les points de montage locaux des modules de périphérie que vous avez copiés lors des étapes précédentes pour le partage Edge et le partage local Edge.

        > [!IMPORTANT]
        > Les chemins utilisés ici sont montés dans votre conteneur ; ils doivent donc correspondre aux attentes de la fonctionnalité dans votre conteneur. Si vous utilisez [Créer un module personnalisé](azure-stack-edge-create-iot-edge-module.md#update-the-module-with-custom-code), le code spécifié dans ce module attend les chemins copiés. Ne modifiez pas ces chemins.
    
        Dans la zone **Options de création de conteneur**, vous pouvez coller l’exemple suivant :
    
        ```
        {
          "HostConfig": 
          {
           "Binds": 
            [
             "/home/hcsshares/mydbesmbedgelocalshare1:/home/input",
             "/home/hcsshares/mydbesmbedgeshare1:/home/output"
            ]
           }
        }
        ```

        Indiquez les variables d’environnement utilisées pour votre module. Les variables d’environnement fournissent des informations facultatives qui aident à définir l’environnement dans lequel votre module s’exécute.

        ![Zone Options de création de conteneur](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-5.png) 
 
    4. Si nécessaire, configurez les paramètres avancés du runtime Edge, puis cliquez sur **Suivant**.

        ![Ajouter un module personnalisé 2](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-6.png)
 
5. Sous **Spécifier des routes**, définissez des routes entre les modules.  
   
   ![Spécifier des routes](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-7.png)

    Vous pouvez remplacer *route* par la chaîne de route suivante copiée précédemment. Dans cet exemple, entrez le nom du partage local qui enverra des données vers le partage cloud. Remplacez `modulename` par le nom du module. Sélectionnez **Suivant**.
        
    ```
    "route&quot;: &quot;FROM /* WHERE topic = 'mydbesmbedgelocalshare1' INTO BrokeredEndpoint(\"/modules/filemove/inputs/input1\")"
    ```

    ![Section Spécifier des routes](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-8.png)

6. Sous **Passer en revue le déploiement**, passez en revue tous les paramètres et, si vous êtes satisfait, sélectionnez **Soumettre** pour soumettre le module pour déploiement.

   ![Page Définir des modules 2](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-9.png)
 
    Cette action démarre le déploiement de module. Une fois le déploiement terminé, l’**État du runtime** du module est **En cours d’exécution**.

    ![Ajouter un module personnalisé 3](./media/azure-stack-edge-deploy-configure-compute-advanced/add-module-10.png)

## <a name="verify-data-transform-transfer"></a>Vérifier la transformation des données et transférer

L’étape finale consiste à vérifier que le module est connecté et qu’il s’exécute comme prévu. L’état du runtime du module doit être en cours d’exécution pour votre appareil IoT Edge dans la ressource IoT Hub.

Effectuez les étapes suivantes pour vérifier la transformation des données et le transfert vers Azure.
 
1. Dans l’Explorateur de fichiers, connectez-vous aux partages local Edge et Edge que vous avez créés.

   ![Vérifier la transformation des données](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-2.png)
 
1. Ajoutez des données au partage local.

   ![Vérifier la transformation des données 2](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-3.png)
 
    Les données sont déplacées vers le partage cloud.

    ![Vérifier la transformation des données 3](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-4.png)  

    Les données sont ensuite envoyées du partage cloud vers le compte de stockage. Pour voir les données, accédez à votre compte de stockage, puis sélectionnez **Explorateur de stockage**. Vous pouvez voir les données chargées dans votre compte de stockage.

    ![Vérifier la transformation des données 4](./media/azure-stack-edge-deploy-configure-compute-advanced/verify-data-5.png)
 
Vous avez terminé le processus de validation.

## <a name="next-steps"></a>Étapes suivantes

Dans ce didacticiel, vous avez appris à :

> [!div class="checklist"]
> * Configurer le calcul
> * Ajouter des partages
> * Ajouter un déclencheur
> * Ajouter un module de calcul
> * Vérifier la transformation des données et transférer

Pour apprendre à gérer votre appareil Azure Stack Edge Pro FPGA, consultez :

> [!div class="nextstepaction"]
> [Administrer un appareil Azure Stack Edge Pro FPGA avec l'interface utilisateur web locale](azure-stack-edge-manage-access-power-connectivity-mode.md)
