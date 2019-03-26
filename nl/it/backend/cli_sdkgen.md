---

copyright:
  years: 2017, 2019
lastupdated: "2019-01-15"

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Integrazione di servizi di back-end alla tua applicazione con un SDK generato
{: #sdkgen-cli}

Il plug-in {{site.data.keyword.IBM}} SDK Generator può essere installato nella [CLI {{site.data.keyword.cloud_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html){: new_window}.

Questo plug-in {{site.data.keyword.IBM_notm}} SDK Generator integra i tuoi servizi di back-end nella tua applicazione con un SDK generato. Quando si verifica una modifica a un'API REST, puoi rigenerare l'SDK e sostituire quello vecchio per eseguire facilmente l'upgrade dell'SDK. Puoi anche integrare la CLI in una pipeline DevOps e assicurare che l'SDK sia sempre congruente con la specifica API ogni volta che l'applicazione viene creata.

La definizione dell'API REST deve essere valida e ospitata su un endpoint server attivo o su un file locale sul tuo sistema.

## Prima di cominciare
{: #prereqs-sdkgen}

Assicurati di avere:

* Un [account {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](http://cloud.ibm.com){: new_window}
* Una definizione API valida che sia conforme alla specifica [ Open API ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno") ](https://www.openapis.org/){: new_window}

## Installazione del plug-in SDK
{: #install-sdkgen}

1. [Installa la CLI {{site.data.keyword.Bluemix}}](/docs/cli/reference/bluemix_cli/get_started.html).

2. [Installa il plug-in ![Icona link esterno](../../icons/launch-glyph.svg "Icona link esterno")](/docs/cli/index.html#install_plug-in){: new_window}.

  ```
  ibmcloud plugin install sdk-gen
  ```
  {: codeblock}

## Generazione dell'SDK
{: #commands-sdkgen}

Genera un SDK immettendo: `ibmcloud sdk generate [arguments...] [command options]`

### Argomenti
{: #gen-args-sdkgen}

* `APP_NAME` - il nome dell'applicazione Cloud Foundry nel tuo spazio corrente
* `OPENAPI_DOC_LOCATION` - un URL o percorso file relativo al JSON o yaml della definizione API REST non elaborato
* `GENERATED_SDK_NAME` (facoltativo) - il nome dell'SDK generato

### Opzioni
{: #gen-options-sdkgen}

* `PLATFORM` (obbligatorio)
   * `--ios` - genera un SDK Swift iOS
   * `--swift` - genera un SDK server Swift
   * `--js` - genera un SDK JavaScript
* `LOCATION` (obbligatorio) - specifica il tipo per `OPENAPI_DOC_LOCATION`
   * `-r` - URL remoto
   * `-f` - file
   * `-a` - applicazione eseguita su {{site.data.keyword.Bluemix_notm}}
   * `-l` - URL host locale
* `--output "YOUR_RELATIVE_PATH"` (facoltativo) - inserisce l'SDK generato nella directory specificata da `YOUR_RELATIVE_PATH` (gli SDK esistenti vengono sovrascritti).
* `--unzip` (facoltativo) - estrae l'SDK generato (gli SDK esistenti vengono sovrascritti).

### Utilizzo
{: #gen-usage-sdkgen}

Per generare un SDK da un'applicazione Cloud Foundry in esecuzione in {{site.data.keyword.Bluemix_notm}}, puoi utilizzare il nome dell'applicazione come parametro per la CLI. Il seguente comando utilizza il nome dell'applicazione come `SDK_Name`.

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
{: #validating-sdkgen-sdkgen}

Esegui il seguente comando:
```
ibmcloud sdk validate [argument]
```
{: codeblock}

### Argomenti
{: #val-args-sdkgen}

* `APP_NAME` - il nome dell'applicazione Cloud Foundry nel tuo spazio corrente
* `OPENAPI_DOC_LOCATION` - un URL o percorso file relativo al JSON o yaml della definizione API REST non elaborato

### Utilizzo
{: #val-usage-sdkgen}

Per convalidare la specifica API di un'applicazione Cloud Foundry in esecuzione in {{site.data.keyword.Bluemix_notm}}, puoi utilizzare il nome dell'applicazione come parametro per la CLI.
```
ibmcloud sdk validate [APP_NAME] [LOCATION]
```
{: codeblock}

Per convalidare un SDK dall'URL a un documento di specifica API o un file JSON o yaml locale, utilizza il seguente comando:
```
ibmcloud sdk validate [OPENAPI_DOC_LOCATION] [LOCATION]
```
{: codeblock}
