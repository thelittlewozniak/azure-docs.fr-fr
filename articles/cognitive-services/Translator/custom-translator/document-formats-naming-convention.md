---
title: Formats de documents et convention d’affectation de noms - Custom Translator
titleSuffix: Azure Cognitive Services
description: Ce guide présente les formats de documents et les convention d’affectation de noms en vigueur dans Custom Translator. Ce concept permet de mieux gérer les noms de documents et d’éviter les conflits de noms.
author: rajdeep-in
manager: christw
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: v-pawal
ms.topic: conceptual
ms.openlocfilehash: 29b74aeaaae0bcfd5f6ae4a3c38cc00176292899
ms.sourcegitcommit: 3d4121badd265e99d1177a7c78edfa55ed7a9626
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/30/2019
ms.locfileid: "66386913"
---
# <a name="document-formats-and-naming-convention-guidance"></a>Guide sur les formats de documents et les conventions d’affectation de noms

N’importe quel fichier utilisé pour un modèle de traduction personnalisée doit être d’au moins **quatre** caractères.

Cette table comprend tous les formats de fichier pris en charge que vous pouvez utiliser pour générer votre système de traduction :

| Format            | Extensions   | Description                                                                                                                                                                                                                                                                    |
|-------------------|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| XLIFF             | .XLF, .XLIFF | Un format de document parallèles, pour les systèmes d’exportation de mémoires de traductions. Les langues utilisées sont définies dans le fichier.                                                                                                                                                              |
| TMX               | .TMX         | Un format de document parallèles, pour les systèmes d’exportation de mémoires de traductions. Les langues utilisées sont définies dans le fichier.                                                                                                                                                              |
| ZIP               | .ZIP         | ZIP est un format de fichier d’archive.                                                                                                                                                                                                        |
| LocStudio         | .LCL         | Un format de Microsoft pour les documents parallèles                                                                                                                                                                                                                                      |
| Microsoft Word    | .DOCX        | Document Microsoft Word                                                                                                                                                                                                                                                        |
| Adobe Acrobat     | .PDF         | Document de la version portable d’Adobe Acrobat                                                                                                                                                                                                                                                |
| HTML              | .HTML, .HTM  | Document HTML                                                                                                                                                                                                                                                                  |
| Fichier texte         | .TXT         | Fichiers texte codés à UTF-16 ou UTF-8. Le nom de fichier ne doit pas contenir de caractères japonais.                                                                                                                                                                                        |
| Fichier d’alignement de textes | .ALIGN       | L’extension `.ALIGN` est une extension spéciale que vous pouvez utiliser si vous savez que des phrases de votre paire de documents se correspondent parfaitement. Si vous fournissez un fichier `.ALIGN`, Custom Translator n’aligne pas les phrases pour vous. |
| Fichier Excel        | .XLSX        | Fichier Excel (2013 ou version ultérieure). La première ligne de la feuille de calcul doit être le code de langue.                                                                                                                                                                                                                                                      |

## <a name="dictionary-formats"></a>Formats de dictionnaires

Custom Translator prend en charge tous les formats de fichier pris en charge pour le jeu d’apprentissage. Si vous utilisez le dictionnaire d’Excel, vérifiez que la première ligne de la feuille de calcul correspond aux codes de langue.

## <a name="zip-file-formats"></a>Formats de fichiers compressés

Les documents peuvent être regroupées dans un seul fichier compressé et téléchargés. Custom Translator prend en charge les formats de fichiers compressés .ZIP, .GZ et .TGZ.

Chaque document du fichier zip avec l’extension TXT, HTML, HTM, PDF, DOCX, ALIGN doit suivre cette convention de nommage :

{nom du document} \_{code de langue} où {nom du document} correspond au nom de votre document, {code de langue} correspond au LanguageID ISO (deux caractères), indiquant que le document contient des phrases dans cette langue. Il doit y avoir le code de langue doit être précédé d’un trait de soulignement (_).

Par exemple, pour télécharger deux documents parallèles au sein d’un fichier compressé pour un système traduisant de l’anglais en espagnol, les fichiers doivent être nommés « data_en » et « data_es ».

Cette convention de nommage utilisée pour indiquer une langue spécifique ne s’applique pas aux fichiers de mémoire de traduction (TMX, XLF, XLIFF, LCL, XLSX) .  

## <a name="next-steps"></a>Étapes suivantes

- Consultez les informations disponibles sur ce [projet](workspace-and-project.md#what-is-a-custom-translator-project) pour apprendre à les créer et les gérer.
