---

copyright:
  years: 2018, 2019
lastupdated: "2019-02-28"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Aggiunta di servizi di back-end alla tua applicazione con un SDK generato
{: #sdk-cli}

Il plug-in {{site.data.keyword.IBM}} SDK Generator può essere installato nella [CLI {{site.data.keyword.cloud_notm}}](/docs/cli/index.html).

Con il plug-in {{site.data.keyword.IBM_notm}} SDK Generator, puoi integrare i tuoi servizi di backend nella tua applicazione utilizzando un SDK generato. Quando si verifica una modifica a un'API REST, puoi rigenerare l'SDK e sostituire quello vecchio per eseguire facilmente l'upgrade dell'SDK. Puoi anche aggiungere la CLI in una pipeline DevOps e assicurare che l'SDK sia sempre congruente con la specifica API ogni volta che l'applicazione viene creata.

La definizione dell'API REST deve essere valida e ospitata su un endpoint server attivo o su un file locale sul tuo sistema.

## Prima di cominciare
{: #prereqs-sdk-cli}

Assicurati di disporre dei seguenti prerequisiti.

* Un [account {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://cloud.ibm.com){: new_window}.
* Una definizione API valida che sia conforme alla specifica [ Open API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ](https://www.openapis.org/){: new_window}.

## Installazione del plug-in SDK
{: #install-sdk-cli}

1. [Installa la CLI {{site.data.keyword.cloud_notm}}](/docs/cli/index.html).

2. [Installa il plug-in SDK](/docs/cli/sdk/index.html).
  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Generazione dell'SDK
{: #commands-sdk-cli}

Genera un SDK immettendo:
```
ibmcloud sdk generate [arguments...] [command options]
```
{: codeblock}

### Argomenti
{: #gen-args-sdk-cli}

* **APP_NAME** - Il nome dell'applicazione Cloud Foundry nel tuo spazio corrente.
* **OPENAPI_DOC_LOCATION** - Un URL o percorso file relativo al JSON o yaml della definizione API REST non elaborato.
* **GENERATED_SDK_NAME** (facoltativo) - Il nome dell'SDK generato.

### Opzioni
{: #gen-options-sdk-cli}

**Platform** (obbligatorio):
  * `--ios` - Genera un SDK Swift iOS.
  * `--swift` - Genera un SDK server Swift.
  * `--js` - Genera un SDK JavaScript.

**Location** (obbligatorio):

Specifica il tipo per l'argomento `OPENAPI_DOC_LOCATION`.

  * `-r` - Un URL remoto.
  * `-f` - Un nome file.
  * `-a` - Applicazione eseguita su {{site.data.keyword.cloud_notm}}.
  * `-l` - L'URL host locale.

**Facoltativo**:
  * `--output "YOUR_RELATIVE_PATH"` - Inserisce l'SDK generato nella directory specificata da `YOUR_RELATIVE_PATH` (gli SDK esistenti vengono sovrascritti se presenti).
  * `--unzip` - Estrae l'SDK generato (sovrascrive i dati esistenti se sono presenti delle risorse SDK).

### Utilizzo
{: #gen-usage-sdk-cli}

Per generare un SDK da un'applicazione Cloud Foundry in esecuzione in {{site.data.keyword.cloud_notm}}, puoi utilizzare il nome dell'applicazione come parametro per la CLI. Il seguente comando utilizza il nome dell'applicazione come `SDK_Name`.

```
ibmcloud sdk generate [APP_NAME] [LOCATION] [PLATFORM]
```
{: codeblock}

Per generare un SDK da un URL in un file di definizione Open API o in un file JSON o yaml locale, utilizza il seguente comando.

```
ibmcloud sdk generate [OPENAPI_DOC_LOCATION] [SDK_Name] [LOCATION] [PLATFORM]
```
{: codeblock}


## Convalida delle definizioni Open API
{: #validating-sdk-cli}

Esegui questo comando: `ibmcloud sdk validate [argument]`

### Argomenti
{: #val-args-sdk-cli}

* `APP_NAME` - Il nome dell'applicazione Cloud Foundry nel tuo spazio corrente.
* `OPENAPI_DOC_LOCATION` - Un URL o percorso file relativo al JSON o yaml della definizione API REST non elaborato.

### Utilizzo
{: #val-usage-sdk-cli}

Per convalidare la specifica API di un'applicazione Cloud Foundry in esecuzione in {{site.data.keyword.cloud_notm}}, puoi utilizzare il nome dell'applicazione come parametro per la CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Per convalidare un SDK dall'URL a un documento di specifica API o un file JSON o yaml locale, utilizza il seguente comando:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}

