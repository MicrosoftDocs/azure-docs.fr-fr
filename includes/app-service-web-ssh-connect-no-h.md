---
title: Fichier include
description: Fichier include
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 03/29/2019
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 458cd36a35ea37b2a317fe98fdeb5acc69a36ce8
ms.sourcegitcommit: 32e0fedb80b5a5ed0d2336cea18c3ec3b5015ca1
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/30/2021
ms.locfileid: "91822795"
---
Pour établir une session SSH directe avec votre conteneur, votre application doit être en cours d’exécution.

Collez l’URL suivante dans votre navigateur et remplacez `<app-name>` par le nom de votre application :

```
https://<app-name>.scm.azurewebsites.net/webssh/host
```

Si vous n’êtes pas encore authentifié, vous devez le faire avec votre abonnement Azure pour vous connecter. Une fois authentifié, vous voyez un interpréteur de commandes dans le navigateur, vous permettant d’exécuter des commandes à l’intérieur de votre conteneur.

![Connexion SSH](./media/app-service-web-ssh-connect-no-h/app-service-linux-ssh-connection.png)
