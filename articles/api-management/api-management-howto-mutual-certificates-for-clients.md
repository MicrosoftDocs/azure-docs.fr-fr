---
title: Sécuriser les API à l’aide d’une authentification par certificat client dans Gestion des API
titleSuffix: Azure API Management
description: Apprenez à sécuriser l’accès aux API à l’aide de certificats clients. Vous pouvez utiliser des expressions de stratégie pour valider des certificats entrants.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/13/2020
ms.author: apimpm
ms.openlocfilehash: 553b4527796db3e5d0f430afd6c5e614626187e5
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "99988890"
---
# <a name="how-to-secure-apis-using-client-certificate-authentication-in-api-management"></a>Comment sécuriser les API à l'aide d'une authentification par certificat client dans la Gestion des API

La Gestion des API permet de sécuriser l'accès aux API (par ex. client à gestion des API) en utilisant des certificats client. Vous pouvez valider le certificat entrant et en vérifier les propriétés en les comparant aux valeurs souhaitées à l’aide d’expressions de stratégie.

Pour savoir comment sécuriser l’accès au service back-end d’une API à l’aide de certificats clients (par exemple, de la Gestion des API vers le back-end), consultez [Comment sécuriser les services principaux à l’aide d’une authentification par certificat client dans la Gestion des API Azure](./api-management-howto-mutual-certificates.md).

> [!IMPORTANT]
> Pour recevoir et vérifier des certificats clients via HTTP/2 dans les niveaux Developer, Basic, Standard ou Premium, vous devez activer le paramètre « Négocier le certificat client » dans le panneau « Domaines personnalisés », comme indiqué ci-dessous.

![Négocier le certificat client](./media/api-management-howto-mutual-certificates-for-clients/negotiate-client-certificate.png)

> [!IMPORTANT]
> Pour recevoir et vérifier des certificats clients dans le niveau Consommation, vous devez activer le paramètre « Demander un certificat client » dans le panneau « Domaines personnalisés », comme indiqué ci-dessous.

![Demander un certificat client](./media/api-management-howto-mutual-certificates-for-clients/request-client-certificate.png)

## <a name="checking-the-issuer-and-subject"></a>Vérification de l’émetteur et du sujet

Les stratégies suivantes peuvent être configurées pour vérifier l’émetteur et le sujet d’un certificat client :

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Issuer != "trusted-issuer" || context.Request.Certificate.SubjectName.Name != "expected-subject-name")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Pour désactiver la vérification de la liste de révocation de certificats, utilisez `context.Request.Certificate.VerifyNoRevocation()` et non `context.Request.Certificate.Verify()`.
> Si le certificat client est auto-signé, le ou les certificats de l’autorité de certification racine (ou intermédiaire) doivent être [chargés](api-management-howto-ca-certificates.md) dans la Gestion des API pour `context.Request.Certificate.Verify()` et `context.Request.Certificate.VerifyNoRevocation()` pour fonctionner.

## <a name="checking-the-thumbprint"></a>Vérification de l’empreinte numérique

Les stratégies suivantes peuvent être configurées pour vérifier l’empreinte d’un certificat client :

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify() || context.Request.Certificate.Thumbprint != "DESIRED-THUMBPRINT-IN-UPPER-CASE")" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>
```

> [!NOTE]
> Pour désactiver la vérification de la liste de révocation de certificats, utilisez `context.Request.Certificate.VerifyNoRevocation()` et non `context.Request.Certificate.Verify()`.
> Si le certificat client est auto-signé, le ou les certificats de l’autorité de certification racine (ou intermédiaire) doivent être [chargés](api-management-howto-ca-certificates.md) dans la Gestion des API pour `context.Request.Certificate.Verify()` et `context.Request.Certificate.VerifyNoRevocation()` pour fonctionner.

## <a name="checking-a-thumbprint-against-certificates-uploaded-to-api-management"></a>Vérification d’une empreinte par rapport aux certificats téléchargés dans la gestion des API

L’exemple suivant montre comment vérifier l’empreinte d’un certificat client par rapport aux certificats téléchargés dans la gestion des API :

```xml
<choose>
    <when condition="@(context.Request.Certificate == null || !context.Request.Certificate.Verify()  || !context.Deployment.Certificates.Any(c => c.Value.Thumbprint == context.Request.Certificate.Thumbprint))" >
        <return-response>
            <set-status code="403" reason="Invalid client certificate" />
        </return-response>
    </when>
</choose>

```

> [!NOTE]
> Pour désactiver la vérification de la liste de révocation de certificats, utilisez `context.Request.Certificate.VerifyNoRevocation()` et non `context.Request.Certificate.Verify()`.
> Si le certificat client est auto-signé, le ou les certificats de l’autorité de certification racine (ou intermédiaire) doivent être [chargés](api-management-howto-ca-certificates.md) dans la Gestion des API pour `context.Request.Certificate.Verify()` et `context.Request.Certificate.VerifyNoRevocation()` pour fonctionner.

> [!TIP]
> Le problème de blocage de certificat client décrit dans cet [article](https://techcommunity.microsoft.com/t5/Networking-Blog/HTTPS-Client-Certificate-Request-freezes-when-the-Server-is/ba-p/339672) peut se manifester de différentes manières, notamment par des demandes qui se figent, des demandes qui génèrent un code d’état `403 Forbidden` après une expiration du délai, `context.Request.Certificate` qui a la valeur `null`. Ce problème affecte généralement les demandes `POST` et `PUT` avec une longueur de contenu d’environ 60 Ko ou plus.
> Pour éviter que ce problème ne se reproduise, activez le paramètre « Négocier le certificat client » pour les noms d’hôte souhaités dans le panneau « Domaines personnalisés », comme indiqué dans la première image de ce document. Cette fonctionnalité n’est pas disponible dans le niveau Consommation.

## <a name="certificate-validation-in-self-hosted-gateway"></a>Validation de certificat dans une passerelle auto-hébergée

L’image de [passerelle auto-hébergée](self-hosted-gateway-overview.md) de Gestion des API par défaut ne prend pas en charge la validation des certificats serveur et client à l’aide de [certificats racine d’autorité de certification](api-management-howto-ca-certificates.md) chargés sur une instance Gestion des API. Les clients présentant un certificat personnalisé à la passerelle auto-hébergée peuvent être confrontés à des réponses lentes, car la validation de la liste de révocation des certificats (CRL) peut prendre un certain temps à expirer sur la passerelle. 

Pour contourner le problème lors de l’exécution de la passerelle, vous pouvez configurer l’adresse IP PKI de façon à ce qu’elle pointe vers l’adresse localhost (127.0.0.1) au lieu de l’instance Gestion des API. Cela cause l’échec rapide de la validation CRL lorsque la passerelle tente de valider le certificat client. Pour configurer la passerelle, ajoutez une entrée DNS pour l’instance Gestion des API afin de la résoudre sur localhost dans le fichier `/etc/hosts` du conteneur. Vous pouvez ajouter cette entrée pendant le déploiement de la passerelle :
 
* Pour le déploiement Docker, ajoutez le paramètre `--add-host <hostname>:127.0.0.1` à la commande `docker run`. Pour plus d’informations, consultez [Ajouter des entrées à un fichier d’hôtes de conteneur](https://docs.docker.com/engine/reference/commandline/run/#add-entries-to-container-hosts-file---add-host)
 
* Pour le déploiement Kubernetes, ajoutez une spécification `hostAliases` au fichier de configuration `myGateway.yaml`. Pour plus d’informations, consultez [Ajout d’entrées à Pod /etc/hosts avec des alias d’hôte](https://kubernetes.io/docs/concepts/services-networking/add-entries-to-pod-etc-hosts-with-host-aliases/).




## <a name="next-steps"></a>Étapes suivantes

-   [Comment sécuriser les services principaux à l'aide d'une authentification par certificat client](./api-management-howto-mutual-certificates.md)
-   [Comment télécharger des certificats](./api-management-howto-mutual-certificates.md)
