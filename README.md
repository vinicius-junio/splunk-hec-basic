# HTTP Event Collector

## Índice

- [Introdução](#introdução)
- [Configuração do Index](#configuração-do-index)
- [Configuração Data Inputs](#configuração-data-inputs)
- [Testando Envio CURL](#testando-envio-curl)


  
## Introdução
<div align = "justify">
  O HTTP Event Collector (HEC) do Splunk é uma interface que permite enviar dados diretamente para o Splunk por meio de solicitações HTTP ou HTTPS. Ele foi projetado para facilitar a ingestão de dados de várias fontes em tempo real e é especialmente útil para enviar dados de aplicações, serviços web, dispositivos IoT e scripts personalizados.<br><br>
Neste tutorial vou utilizar um código simples em python que envia um arquivo JSON com alguns eventos.<br><br>
  Por ser um tutorial básico do fluxo, estamos utilizando HTTP.
</div>

## Configuração do Index

<div align = "justify">
  No Splunk, um índice (ou index) é um repositório onde os dados ingeridos são armazenados e organizados. Pense em um índice como um banco de dados que armazena logs e eventos coletados de várias fontes de dados. Cada índice é configurado para armazenar dados de maneira otimizada para pesquisa e análise rápida.<br><br>
</div>

  1. Na console do Splunk:  Clique em Settings -> Indexes.<br><br>
     ![Splunk Index](splunk_images/index.png)<br><br>
  2. Clique em New Index -> Atribua um nome ao index e realize as configurações necessárias, e ao final clique em Save.<br><br>
     ![Splunk Index](splunk_images/new_index.png)<br><br>

     Após esta etapa temos o index criado.
  
## Configuração Data Inputs

<div align = "justify">
  O Data Inputs no Splunk é uma funcionalidade que permite configurar as fontes de dados que o Splunk irá monitorar e indexar. Quando você configura um novo Data Input, está essencialmente dizendo ao Splunk onde e como coletar os dados.<br><br>
  Em nosso caso vamos coletar os dados via HEC (HTTP Event Collector).
</div>

  1. Clique em Settings -> Data Inputs.<br><br>
     ![Data Inputs](splunk_images/data-inputs.png)<br><br>
  2. Em HTTP Event Collector clique em + Add new -> Atribua um nome e clique em Next >.<br><br>
     ![Data Inputs](splunk_images/data-inputs-01.png)<br><br>
  3. Selecione o Source Type -> Selecione o Index e clique em Review >.<br><br>
     ![Data Inputs](splunk_images/data-inputs-02.png)<br><br>
  4. Após verificar as informações, clique em Submit >.<br><br>
     ![Data Inputs](splunk_images/data-inputs-03.png)<br><br>
  5. Agora nosso Token foi criado para utilizarmos em nosso código Python ou diretamente no CURL.<br><br>
     ![Data Inputs](splunk_images/data-inputs-04.png)<br><br>

## Testando Envio CURL

  Podemos realizar o teste de envio de eventos via CURL:<br>
  ```
    curl -k http://<splunk_host>:8088/services/collector/event -H "Authorization: Splunk <seu_token>" -d "{\"event\": {\"timestamp\": \"2024-07-14T19:01:04.104900Z\", \"event\": {\"type\": \"modify\", \"source\": \"web_application\", \"severity\": \"warning\"}, \"user\": {\"id\": \"user234\", \"name\": \"User 0\", \"role\": \"user\"}, \"action\": {\"type\": \"login\", \"status\": \"failure\", \"ip_address\": \"192.168.42.105\"}, \"system\": {\"hostname\": \"webserver10\", \"os\": \"Oracle Linux 8.3\", \"application\": \"MyWebApp\", \"version\": \"3.2.9\"}}, \"sourcetype\": \"_json\"}"
  ```

  Substituindo as informações do splunk_host e seu-token temos a resposta:<br>
  
  ```
    {"text":"Success","code":0}
  ```
  E no Splunk temos o log:<br>
   


