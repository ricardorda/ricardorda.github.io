---
layout: post
title: "Criando Um Relatório de Incidentes do ServiceNow Com Power BI"
date: 2019-12-28
tags: [Power BI]
categories: [Power BI]

---

Nesse post iremos criar um relatório de acompanhamento dos incidentes do ServiceNow no Power BI.

## Acessando a API do ServiceNow
O ServiceNow possui uma API Rest onde podemos ler as informações desejadas. Abaixo link com os detalhes da API:

[https://developer.servicenow.com/app.do#!/rest_api_doc?v=madrid&id=c_TableAPI](https://developer.servicenow.com/app.do#!/rest_api_doc?v=madrid&id=c_TableAPI)


No meu caso desejo acompanhanhar os tickets de apenas um Assignment Group, então vou utilizar os filtros abaixo:
- assignment_group=Nome do Grupo desejado
- sysparm_display_value=true (*Exibe o nome dos recursos ao invés do ID*)
- sysparm_exclude_reference_link=true (*Remove o link dos detalhes do conteúdo*)


Desse modo a URL final será:<br/>
[https://sua-url-servicenow/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&assignment_group=Nome-do-grupo](https://sua-url-servicenow/api/now/table/incident?sysparm_display_value=true&sysparm_exclude_reference_link=true&assignment_group=Nome-do-grupo)

## Conectando na API usando o Power BI

No Power BI desktop clique em **_Get Data_** e escolha a opção Web.
![Get Data](/assets/images/posts/PowerBIServiceNow1.png)

Informe a URL e clique em OK, quando necessário informe as credenciais de acesso.
![Informe URL](/assets/images/posts/PowerBIServiceNow2.png)

Após carregamento, clique em _**List**_ e depois no botão _**To Table**_
![Convertendo para Lista](/assets/images/posts/PowerBIServiceNow3.png)<br/>

![Convertendo para TAbela](/assets/images/posts/PowerBIServiceNow4.png)

Clique em OK
![Confirmando](/assets/images/posts/PowerBIServiceNow5.png)

Clique na opção para expandir as colunas e desmarque a opção *Use Original column name as prefix*
![Expandindo as colunas](/assets/images/posts/PowerBIServiceNow6.png)

Converta as colunas _**Opened_At**_ e _**Closed_At**_ para DateTime e mude o nome da tabela para _**Incidentes**_
![Convertendo as colunas para datetime](/assets/images/posts/PowerBIServiceNow7.png)

De volta para a visão de relatórios crie uma Matrix para visualizar os dados.
![Criando relatório para visualizar os dados](/assets/images/posts/PowerBIServiceNow8.png)



## Criando Uma Dimensão de Calendário

Precisamos criar uma nova dimensão Calendário para que possamos realizar as diversas comparações de data.

![Criando tabela Calendario](/assets/images/posts/PowerBIServiceNow9.png)

Abaixo código DAX para criar a tabela:
```
Calendario = 
ADDCOLUMNS (
CALENDAR (DATE(YEAR(MIN(Incidentes[opened_at]));1;1); DATE(YEAR(MAX(Incidentes[closed_at]));12;31));
"Year"; YEAR ( [Date] );
"MonthNameShort"; FORMAT ( [Date]; "mmm" );
"MonthNameLong"; FORMAT ( [Date]; "mmmm" );
"MonthYear"; FORMAT ( [Date]; "mmm" ) & "/" & FORMAT ( [Date]; "YY" );
"WeekNumber"; "W" & FORMAT( WEEKNUM( [Date] );"00");
"DayOfWeekNumber"; WEEKDAY ( [Date] );
"DayOfWeek"; FORMAT ( [Date]; "dddd" );
"DayOfWeekShort"; FORMAT ( [Date]; "ddd" );
"Quarter"; "Q" & FORMAT ( [Date]; "Q" );
"YearQuarter"; FORMAT ( [Date]; "YYYY" ) & "/Q" & FORMAT ( [Date]; "Q" );
"YearMonth"; FORMAT ( [Date]; "YYYY" )*100 & FORMAT ( [Date]; "MM" )
)
```


## Criando o Relacionamento

Abra novamente o Query Editor e crie as colunas Opened_Date e Closed_Date como apenas data.
![Criando colunas Opened_Date e Closed_Date](/assets/images/posts/PowerBIServiceNow10.png)

Na visão model relacione as colunas Opened_Date e Closed_Date com a Date da tabela calendário.
![Relacionamento entre Opened_Date, Closed_Date e Calendario](/assets/images/posts/PowerBIServiceNow11.png)

Crie as medidas abaixo para calcularmos os números de tickets abertos, fechados e o backlog.
![Criando medidas customizadas](/assets/images/posts/PowerBIServiceNow12.png)
```
Opened Tickets = CALCULATE(COUNTA(Incidentes[Number]); USERELATIONSHIP(Calendario[Date]; Incidentes[Opened_Date]))
Closed Tickets = CALCULATE(COUNTA(Incidentes[Closed_Date]); USERELATIONSHIP(Calendario[Date]; Incidentes[Closed_Date]))
Balance = [Opened Tickets]-[Closed Tickets]
Backlog = CALCULATE([Balance]; FILTER(ALL(Calendario);Calendario[Date] <= MAX(Calendario[Date])))
Backlog LY = CALCULATE([Backlog]; SAMEPERIODLASTYEAR(Calendario[Date])) 
Opened Tickets LY = CALCULATE([Opened Tickets]; SAMEPERIODLASTYEAR(Calendario[Date]))
Closed Tickets LY = CALCULATE([Closed Tickets];SAMEPERIODLASTYEAR(Calendario[Date]))
```


## Criando o Relatório


Crie uma Matrix e coloque a coluna _**MonthYear**_ da tabela Calendario no campo _**Columns**_ e as medidas _**Opened Tickets, Closed Tickets e Backlog**_ no campo _**Values**_
![Criando Matrix com números mensais](/assets/images/posts/PowerBIServiceNow13.png)

Para deixar as medidas como linhas ao invés de colunas marque a opção _**Show on rows**_ dentro da tab _**Format**_, categoria _**Values**_
![Criando Matrix com números mensais](/assets/images/posts/PowerBIServiceNow14.png)

Como a coluna _**MonthYear**_ é um texto, por padrão ela será ordenada alfabeticamente, para ordenar pela data vá até a visão das tabelas, clique na coluna _**MonthYear**_ e depois 
no menu _**Modeling > Sort By Column**_ escolha a coluna _**YearMonth**_
![Criando Matrix com números mensais](/assets/images/posts/PowerBIServiceNow15.png)


Essa é a estrutura básica, novos relatórios podem ser feitos de acordo com a necessidade.










