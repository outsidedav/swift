---

copyright:
  years: 2018
lastupdated: "2018-11-12"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Desenvolvendo Localmente

Ao desenvolver localmente, é possível construir, executar e testar facilmente os apps Swift. Use o {{site.data.keyword.dev_cli_short}} para executar essas ações usando comandos padrão. 

É possível usar o {{site.data.keyword.dev_cli_short}} para gerenciar os seus aplicativos do lado do servidor com mais de uma dúzia de comandos. Saiba mais sobre os vários comandos em [Comandos `ibmcloud dev` da CLI do IBM Cloud Developer Tools](/docs/cli/idt/commands.html).

## Antes de começar

Para desenvolver localmente, deve-se instalar o {{site.data.keyword.dev_cli_notm}}. Execute o comando a seguir para executar o script de instalação:
```
curl -sL https://ibm.biz/idt-installer | bash
```
{: codeblock}

Consulte [Configurando a CLI do IBM Cloud Developer Tools](/docs/cli/idt/setting_up_idt.html) para saber mais sobre a configuração e o uso do {{site.data.keyword.dev_cli_notm}}.

## Recuperando as Credenciais de Serviço

Após clonar o aplicativo por meio do Git, deve-se recuperar as credenciais para os serviços que estão ligados ao aplicativo, pois elas não são armazenadas no repositório Git para o seu aplicativo. Recuperar as credenciais permite o uso de serviços ligados. É possível fazer download facilmente das credenciais executando o comando a seguir na raiz do diretório de aplicativo:
```
ibmcloud dev get-credentials
```
{: codeblock}

## Construindo, Executando e Implementando seu Aplicativo

1. **Construir** - Agora é possível construir seu aplicativo, que é um pré-requisito para executá-lo.
  Use o comando a seguir na raiz do diretório de aplicativo para construir o seu app:
  ```
  ibmcloud dev build
  ```
  {: codeblock}

2. **Executar** - Após uma construção bem-sucedida, é possível executar o aplicativo em um contêiner local com o comando a seguir:
  ```
  ibmcloud dev run
  ```
  {: codeblock}

  Um host e uma porta locais para visualizar a página de entrada do seu aplicativo serão exibidos se o comando for executado com êxito.

3. **Implementar** - Implemente o aplicativo no {{site.data.keyword.Bluemix_notm}} com o comando `deploy`:
  ```
  ibmcloud dev deploy
  ```
  {: codeblock}
