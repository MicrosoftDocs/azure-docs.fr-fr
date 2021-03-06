---
author: alkohli
ms.service: databox
ms.topic: include
ms.date: 10/13/2020
ms.author: alkohli
ms.openlocfilehash: d12a5042d0c2d79989d82e86e7265d030912f5ee
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98900909"
---
Cette section décrit les limites du service Stockage Azure et les conventions d’affectation de noms requises pour Azure Files, les objets blob de blocs Azure et les objets blob de pages Azure, telles qu’applicables au service Azure Stack Edge. Examinez soigneusement les limites de stockage et suivez toutes les recommandations.

Pour les informations les plus récentes sur les limites du service de stockage Azure et les bonnes pratiques pour nommer les partages, les conteneurs et les fichiers, accédez à :

- [Affectation de noms et de références aux conteneurs](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata)
- [Affectation de noms et de références aux partages](/rest/api/storageservices/naming-and-referencing-shares--directories--files--and-metadata)
- [Conventions d’objets blob de blocs et d’objets blob de pages](/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs)

> [!IMPORTANT]
> S’il existe des fichiers ou répertoires qui dépassent les limites du service Stockage Azure, ou qui ne sont pas conformes aux conventions d’affectation de noms des fichiers/objets blob Azure, ces fichiers ou répertoires ne sont pas ingérés dans Stockage Azure par le biais du service Azure Stack Edge.