---

copyright:
  years: 2018, 2019
lastupdated: "2019-06-07"

keywords: object storage swift, static storage swift, file services swift, swift storage class, cos swift, swift data encryption, static swift

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Utilisation d'Object Storage pour le contenu statique
{: #object-storage}

Object Storage est un composant fondamental du Cloud Computing qui offre de puissantes fonctions aux développeurs Apple et à leurs applications. A la différence du stockage d'informations dans une hiérarchie de fichiers (telle que le Stockage par blocs ou le Stockage de fichiers), un conteneur d'objets se compose uniquement de fichiers et de leurs métadonnées, stockées dans des collections appelées compartiments. Par définition, ces objets sont non modifiables, ce qui les rend parfaits pour les données comme les images, les vidéos et d'autres documents statiques. Pour les données qui changent souvent ou les données relationnelles, vous pouvez utiliser les services de base de données [Cloudant](/docs/swift/data?topic=swift-cloudant#cloudant) et [SQL](/docs/swift/data?topic=swift-sql_data#sql_data).

{{site.data.keyword.cos_full_notm}} (COS) est système de stockage qui peut être utilisé pour le stockage de données non structurées à la fois flexibles, abordables et évolutives. Les données sont accessible via des logiciels SDK ou via l'interface utilisateur IBM. Vous pouvez utiliser {{site.data.keyword.cos_full_notm}} pour accéder aux données non structurées via un portail en libre-service adossé à des API RESTful et des logiciels SDK. 

Selon vos besoins, vous pouvez utiliser {{site.data.keyword.cos_short}} pour les services suivants :

* Référentiel de contenu
* Sauvegarde et restauration
* Archivage de données
* Services de fichier
* Transfert de données

Lorsque vous créez un compartiment, vous devez sélectionner un niveau de résilience (inter-régional ou régional). En fonction de votre sélection, vos données sont dispersés et stockées dans plusieurs emplacements géographiques.

## API
{: #api-cos}

L'API {{site.data.keyword.cos_full}} est un API REST pour la lecture et l'écriture d'objets. Elle prend en charge un sous-ensemble d'API S3 pour une migration facile des applications vers {{site.data.keyword.cloud_notm}}. N'importe quel logiciel SDK S3 peut utiliser {{site.data.keyword.cos_full}}. Pour plus d'informations, voir la documentation détaillée [{{site.data.keyword.cos_short}} API Reference](/docs/services/cloud-object-storage?topic=cloud-object-storage-compatibility-api).

## Sécurisation du stockage d'objets
{: #security-cos}

{{site.data.keyword.cos_full_notm}} est hautement sécurisé. Initialement, seuls les propriétaires du compartiment et de l'objet ont accès au service {{site.data.keyword.cos_full_notm}} qu'ils créent. Il prend également en charge l'authentification d'utilisateur pour l'accès aux données. Utilisez des mécanismes de contrôle d'accès comme mes stratégies de compartiment pour accorder de manière sélective des droits aux utilisateurs et aux applications. Vous pouvez transférer de manière sécurisée le transfert de vos données à {{site.data.keyword.cos_short}} via des noeuds finaux SSL à l'aide du protocole HTTPS. Si vous avez besoin d'une sécurité supplémentaire, vous pouvez utiliser l'option SSE (Server-Side Encryption) ou SSE-C (Server-Side Encryption with Customer-Provided Keys) pour chiffrer les données stockées au repos. {{site.data.keyword.cos_full_notm}} fournit la technologie de chiffrement pour SSE et SSE-C. Vous pouvez aussi utiliser vos propres bibliothèques de chiffrement pour chiffrer les données avant de les stocker dans Cloud Object Storage.

Vous pouvez utiliser les mécanismes de contrôle d'accès suivants pour sécuriser vos données dans {{site.data.keyword.cos_full_notm}}.

### Règles IAM (Identity and Access Management)
{: #iam-cos}

IAM permet aux organisations comptant de nombreux employés de créer et de gérer plusieurs utilisateurs sous un seul compte. Grâce aux stratégies IAM, les entreprises peuvent accorder aux utilisateurs IAM le contrôle d'accès à leurs compartiments {{site.data.keyword.cos_short}}.

### Listes de contrôle d'accès (ACL)
{: #acls-cos}

Les listes de contrôle d'accès vous permettent d'accorder des droits spécifiques (par exemple des droits d'écriture ou de lecture) à des utilisateurs spécifiques pour un compartiment individuel.

Les données au repos sont chiffrées côté fournisseur à l'aide d'un chiffrement AES (Advanced Encryption Standard) automatique de 256 bits et d'un hachage SHA-256 (Secure Hash Algorithm). Les données dynamiques sont sécurisées à l'aide d'un chiffrement de qualité opérateur intégré TLS (Transport Layer Security), SSL (Secure Sockets Layer) ou SNMPv3 avec AES.

### Chiffrement
{: #encryption-cos}

Les données au repos sont chiffrées côté fournisseur à l'aide d'un chiffrement AES (Advanced Encryption Standard) automatique de 256 bits et d'un hachage SHA-256 (Secure Hash Algorithm). Les données dynamiques sont sécurisées à l'aide d'un chiffrement de qualité opérateur intégré TLS/ SSL (Transport Layer Security/Secure Sockets Layer) ou SNMPv3 avec AES.

Le chiffrement côté serveur est toujours actif pour les données clients. Comparé au hachage nécessaire à l'authentification et au codage par effacement, le chiffrement ne représente pas une part significative du coût de traitement de Cloud Object Storage.

Vous pouvez fournir votre propre clé pour le chiffrement car SSE-C est pris en charge dans {{site.data.keyword.cos_full_notm}}. Toutefois, il vous incombe de gérer et de fournir la clé pour stocker et extraire les données.

## Résilience
{: #resiliency-cos}

Lorsque vous créez un compartiment, vous devez sélectionner un niveau de résilience (inter-régional ou régional). En fonction de votre sélection, vos données sont dispersés et stockées dans plusieurs emplacements géographiques.

La résilience régionale est pour la faible latence et vos données sont réparties dans trois centres au sein d'une seule et même région. Utilisez la résilience interrégionale pour une disponibilité optimale, car vos données sont stockées dans 3 régions différentes ou plus. La résilience interrégionale offre une résilience géographique et est disponible sur plus d'un point final. Tenez compte des besoins de votre application pour décider de choisir l'une des deux.

### Géographies
{: #geographies-cos}

Vous pouvez utiliser {{site.data.keyword.cos_full_notm}} depuis n'importe où dans le monde. Vous pouvez choisir où vous souhaitez stocker vos données au moment de la création du compartiment. Les informations stockées dans IBM COS sont cryptées et dispersées sur plusieurs emplacements géographiques grâce aux technologies de stockage distribué fournies par IBM Object Storage System. 

Tenez compte des facteurs suivants pour choisir l'emplacement géographique de votre magasin d'objets lorsque vous choisissez entre les options régionales et interrégionales.

Considérations relatives à l'emplacement géographique
* Un emplacement qui est distant de vos opérations pour la redondance.
* Un emplacement pour répondre aux exigences juridiques et réglementaires.
* Réduction de la latence d'accès aux données.
* Pris en compte des modèles de tarification.

## Cas d'utilisation et classes de stockage
{: #usecases-cos}

En fonction de votre cas d'utilisation, vous pouvez réduire les coûts en sélectionnant un plan de service qui répond à vos besoins. Pour les opérations d'archivage qui impliquent un accès minimal au conteneur d'objets, la vitesse et la durabilité d'un objet fréquemment utilisé ne sont pas nécessaires, et cette distinction est reflétée dans la classe de support de stockage et le plan de tarification de vos applications. Les classes de stockage sont définies au niveau du compartiment, de sorte que vous pouvez utiliser une combinaison de plans qui répond à vos besoins. Créez un compartiment défini sur la classe de stockage que vous voulez utiliser.

D'autres informations sur la tarification sont disponibles dans la documentation relative à la [classe de stockage {{site.data.keyword.cos_short}}](/docs/services/cloud-object-storage/help?topic=cloud-object-storage-billing#ibm-cos-pricing).

### Exemple de classe de stockage
{: #samples-cos}

- Standard
  
  Ce service est pour les données non structurées qui requièrent un accès fréquent, comme DevOps, des référentiels de contenu d'action et de collaboration.

- Coffre
  
  Ce service est pour les charges de travail avec des données peu fréquemment utilisées, comme les sauvegardes, les archives et les charges de travail de conformité.

- Coffre froid
  
  Cette option de déploiement est idéale pour les exigences d'accès minimum, la conformité des enregistrements historiques et les sauvegardes à long terme.

- Flex

  Déploiement pour les exigences d'accès aux données variables et la protection de votre budget face aux fluctuations de coût imprévues. Les classes de stockage sont définies au niveau du compartiment. Créez un compartiment défini sur la classe de stockage que vous voulez utiliser.


