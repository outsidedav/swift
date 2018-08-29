---

copyright:
  years: 2018
lastupdated: "2018-06-05"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note:.deprecated}

# Cas d'utilisation de la logique iOS et Cloud
{: #sample_cloud}

Un exemple d'application iOS utilisant un composant BFF (Backend For Frontend) est présenté dans le [modèle d'application de partage de photo et d'image BluePic](https://github.com/IBM/BluePic). L'appli BluePic utilise les technologies suivantes :

* Object Storage et Cloudant pour le stockage des données d'image.
* Watson Visual Recognition et le service IBM Weather Company pour l'ajout d'informations supplémentaires aux images.
* Kitura et {{site.data.keyword.openwhisk}} pour fournir la logique de back end et contrôler l'authentification et les notifications push, le tout écrit en Swift.

![BluePic](images/cloudlogic.png "Flux BluePic")

Dans l'exemple BluePic, lorsqu'une image est publiée, ses données sont enregistrées dans Cloudant, et les données binaires d'image sont stockées dans Object Storage. Ensuite, une séquence {{site.data.keyword.openwhisk_short}} est appelée et entraîne le calcul de données météorologiques, telles que la température et les conditions actuelles (ensoleillées, nuageuses, par exemple) en fonction de l'emplacement à partir duquel une image a été importée. Watson Visual Recognition est également utilisé dans la séquence {{site.data.keyword.openwhisk_short}} pour analyse de l'image et extraction de balises de texte basées sur le contenu de l'image. Enfin, une notification push est envoyée à l'utilisateur, pour l'informer que l'image a été traitée et qu'elle inclut désormais des données météorologiques et des balises.
