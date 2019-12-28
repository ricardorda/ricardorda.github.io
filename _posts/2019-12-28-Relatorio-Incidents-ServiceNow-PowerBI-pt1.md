---
layout: posts
title: "Relatório de Incidentes do ServiceNow com PowerBI - Parte 1"
date: 2019-12-28
tags: [Power BI]
author_profile: false
excerpt: "Acessando API do ServiceNow pelo PowerBI"
---

Nesse série de posts iremos criar um relatório de acompanhamento dos incidentes do ServiceNow no PowerBI.

## Acessando a API do ServiceNow
O ServiceNow possui uma API Rest onde podemos ler as informações desejadas. Abaixo link com os detalhes da API:

[https://developer.servicenow.com/app.do#!/rest_api_doc?v=madrid&id=c_TableAPI](https://developer.servicenow.com/app.do#!/rest_api_doc?v=madrid&id=c_TableAPI)


No meu caso desejo acompanhanhar os tickets de apenas um Assignment Group, então vou utilizar os filtros abaixo:
- assignment_group=Nome do Grupo desejado
- sysparm_display_value=true (*Exibe o nome dos recursos ao invés do ID*)
- sysparm_exclude_reference_link=true (*Remove o link dos detalhes do conteúdo*)


Desse modo a URL final será:

[https://sua-url-servicenow/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&assignment_group=Nome-do-grupo](https://sua-url-servicenow/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&assignment_group=Nome-do-grupo)

## Conectando na API usando o Power BI

No Power BI desktop clique em **_Get Data_** e escolha a opção Web.
![Importando](/assets/images/posts/PowerBIServiceNow1.png)

Informe a URL e clique em OK, quando necessário informe as credenciais de acesso.
![Importando](/assets/images/posts/PowerBIServiceNow2.png)

Após carregamento, clique em _**List**_ e depois no botão _**To Table**_
![Importando](/assets/images/posts/PowerBIServiceNow3.png)
![Importando](/assets/images/posts/PowerBIServiceNow4.png)

Clique em OK
![Importando](/assets/images/posts/PowerBIServiceNow5.png)

Clique na opção para expandir as colunas e desmarque a opção *Use Original column name as prefix*
![Importando](/assets/images/posts/PowerBIServiceNow6.png)

Converta as colunas _**Opened_At**_ e _**Closed_At**_ para DateTime e mude o nome da tabela para _**Incidentes**_
![Importando](/assets/images/posts/PowerBIServiceNow7.png)

De volta para a visão de relatórios crie uma Matrix para visualizar os dados.
![Importando](/assets/images/posts/PowerBIServiceNow8.png)


Na próxima parte iremos trabalhar na construção dos relatórios.


