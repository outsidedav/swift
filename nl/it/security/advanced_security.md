---

copyright:
  years: 2018, 2019
lastupdated: "2019-03-28"

keywords: swift security, add security swift, crypto swift, hyper protect swift, ios hyper protect, dbaas swift, swift key management, swift advanced security

subcollection: swift

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:note: .deprecated}
{:tip: .tip} 

# Creazione con la sicurezza avanzata
{: #security}

Gli sviluppatori Swift possono facilmente utilizzare i servizi di sicurezza avanzati offerti da {{site.data.keyword.cloud}} per proteggere le loro chiavi e i loro dati inattivi, in uso o in transito al livello di sicurezza più elevato del settore.
{: shortdesc}

Come facile approccio, puoi utilizzare direttamente {{site.data.keyword.cloud_notm}} HyperSecure Platform Services per tutti i servizi di sicurezza avanzati in {{site.data.keyword.cloud}}. Per ulteriori informazioni, vedi [Introduzione a {{site.data.keyword.cloud_notm}} HyperSecure Platform Services](/docs/services/hypersecure-platform/index.html#getting-started-with-ibm-cloud-hyper-protect-developer-starter-kits).

## Utilizzo di {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}
{: #security-swift}

{{site.data.keyword.hscrypto}} è un servizio sperimentale che offre la crittografia sulle tue chiavi e i tuoi dati con il livello di sicurezza più elevato del settore. Porta la sicurezza e l'integrità di IBM Z al cloud. La stessa tecnologia crittografica a cui si affidano oggi le banche e i servizi finanziari è ora offerta agli utenti cloud tramite {{site.data.keyword.cloud_notm}}.

{{site.data.keyword.hscrypto}} offre degli HSM (hardware security module) indirizzabili di rete per fornire una crittografia sicura e protetta tramite le API (application programming interface) PKCS#11. Puoi accedere al massimo livello raggiungibile di sicurezza per l'hardware crittografico, ossia FIPS 140-2 Level 4. {{site.data.keyword.hscrypto}} utilizza anche la soluzione IBM ACSP (Advanced Crypto Service Provider) che abilita l'accesso remoto ai coprocessori crittografici di IBM.

{{site.data.keyword.hscrypto}} è anche il keystore per il servizio {{site.data.keyword.keymanagementserviceshort}}.

Per ulteriori informazioni su {{site.data.keyword.hscrypto}}, vedi [Introduzione a {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscrypto}}](/docs/services/hs-crypto?topic=hs-crypto-get-started#get-started).

## Utilizzo di {{site.data.keyword.cloud_notm}} {{site.data.keyword.keymanagementserviceshort}}
{: #swift-key-management}

{{site.data.keyword.keymanagementserviceshort}} ti aiuta a creare le chiavi crittografate per le applicazioni tra i servizi {{site.data.keyword.cloud_notm}}. Quando gestisci il ciclo di vita delle tue chiavi, puoi trarre vantaggio dal sapere che le tue chiavi sono protette da HSM (hardware security module) basati sul cloud che offrono protezione contro il furto di informazioni. Insieme a {{site.data.keyword.hscrypto}}, le tue chiavi sono protette al livello di sicurezza più elevato del certificato FIPS 140-2 Level 4.

Per ulteriori informazioni su {{site.data.keyword.keymanagementserviceshort}}, vedi [Introduzione a {{site.data.keyword.keymanagementserviceshort}}](/docs/services/key-protect?topic=key-protect-getting-started-tutorial#getting-started-tutorial).

## Utilizzo di {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #hypersecure-dbaas}

{{site.data.keyword.cloud_notm}} Hyper Protect DBaaS è un servizio {{site.data.keyword.cloud_notm}} sperimentale che crea database protetti su richiesta. Offre una piattaforma estensibile per eseguire il provisioning e gestire in modo rapido e facile il tuo database MongoDB.

Con questo servizio, puoi creare dei cluster di database in {{site.data.keyword.cloud_notm}}. Ogni cluster di database che crei è formato da un'istanza del database principale e due istanze del database secondarie che fungono da repliche a quella principale. La logica {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS crea e accede ai database MongoDB nei tuoi cluster di database.

### Protezione dei dati e della riservatezza
{: #swift-securing-data}

{{site.data.keyword.IBM_notm}} ospita i database in un ambiente altamente disponibile e sicuro:
 * I dati sono crittografati sia quando sono inattivi che quando sono in transito.
 * L'hardware del sistema e la configurazione del sistema e del database garantiscono un'elevata disponibilità.
 * Le tecnologie sottostanti impediscono {{site.data.keyword.IBM_notm}} o a una terza parte di poter accedere ai tuoi dati.

### Aggiunta della logica {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS alla tua applicazione
{: #swift-hyperprotect}

Per utilizzare un database MongoDB nella tua applicazione, vedi il documento di [introduzione all'utilizzo di un database](/docs/swift/hypersecure_dbaas?topic=swift-create-database-cluster#creating-a-highly-available-and-secure-database).  

### Ulteriori informazioni su {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS
{: #swift-learnmore}

Per ulteriori informazioni su {{site.data.keyword.cloud_notm}} Hyper Protect DBaaS, vedi il documento di [Introduzione a IBM Cloud Hyper Protect DBaaS](/docs/services/hyper-protect-dbaas?topic=hyper-protect-dbaas-gettingstarted#gettingstarted).

## Utilizzo di {{site.data.keyword.cloud_notm}} {{site.data.keyword.hscontainers}}
{: #swift-hscontainers}

{{site.data.keyword.hscontainers}} fornisce dei potenti strumenti combinando le tecnologie Docker e Kubernetes, un'esperienza utente intuitiva e la sicurezza e l'isolamento integrati per automatizzare la distribuzione, l'operatività, il ridimensionamento e il monitoraggio delle applicazioni inserite nel contenitore in un cluster di host di calcolo.

{{site.data.keyword.hscontainers}} sono disponibili solo per gli utenti sponsorizzati. Se prevedi un supporto della sicurezza dedicato, registrati come un utente sponsorizzato con l'[IBM Z Client Feedback Program](https://www-01.ibm.com/marketing/iwm/iwmdocs/web/cc/earlyprograms/zcustomer.shtml){: new_window} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") per distribuire la tua applicazione al cluster {{site.data.keyword.hscontainers}}.
{: tip}

Prima di diventare un utente sponsorizzato, puoi utilizzare {{site.data.keyword.containershort_notm}} per proteggere la tua applicazione. Per ulteriori informazioni, vedi [Introduzione a {{site.data.keyword.containershort_notm}}](/docs/containers?topic=containers-container_index#container_index).
