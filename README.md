# projeto5_rotabi
Projeto desenvolvido no Bootcamp de Análise de Dados do Laboratório

Ficha Técnica Projeto de Análise de Dados - Rota BI

Este projeto concentra-se na exploração e análise de dados relacionados à disponibilidade de quartos no Airbnb, utilizando ferramentas e conceitos de Business Intelligence (BI). Em um contexto onde a economia compartilhada e a hospitalidade convergem, maximizar a eficiência e rentabilidade é crucial para os anfitriões e a própria plataforma. A diversidade de dados gerados pela interação entre anfitriões e hóspedes no Airbnb oferece uma oportunidade única para aplicar técnicas de BI, buscando descobrir insights profundos que otimizem a disponibilidade de quartos, maximizem receitas e melhorem a experiência do usuário. Através de estratégias de integração de dados, relações entre tabelas e técnicas avançadas de BI, serão visualizadas tendências, identificados padrões e compreendidos os fatores que influenciam a ocupação de quartos, com o objetivo de fornecer uma base sólida para a tomada de decisões informada e ações estratégicas no dinâmico ecossistema do Airbnb.

## **Caso**

Em um mundo onde a economia compartilhada e a hospitalidade convergem, plataformas como Airbnb transformaram a forma como as pessoas procuram e oferecem acomodações. Nesse contexto, maximizar a eficiência e a rentabilidade tornou-se essencial tanto para os anfitriões quanto para a própria plataforma. O presente projeto concentra-se na exploração e análise de dados relacionados à disponibilidade de quartos no Airbnb, utilizando ferramentas e conceitos de Business Intelligence (BI) para desvendar padrões, identificar oportunidades e melhorar a tomada de decisões estratégicas.

A diversidade de informações geradas pela interação de anfitriões e hóspedes na plataforma Airbnb cria um vasto conjunto de dados. Desde detalhes sobre as propriedades, preços e localizações, até o feedback dos hóspedes, a riqueza desses dados proporciona uma oportunidade única para aplicar técnicas de BI. Ao empregar estratégias de integração de dados e relações entre tabelas, buscamos descobrir insights profundos que permitam aos anfitriões e ao próprio Airbnb otimizar a disponibilidade de quartos, maximizar as receitas e melhorar a experiência do usuário.

Esta análise exploratória utilizará técnicas avançadas de BI para visualizar tendências, identificar padrões e compreender os fatores que influenciam a ocupação das acomodações. Desde a alta temporada até as preferências regionais, diversos aspectos serão examinados para desvendar informações valiosas. O objetivo final é fornecer uma base sólida para a tomada de decisões informada, permitindo que os interessados tomem medidas estratégicas para melhorar a eficiência operacional e a rentabilidade no dinâmico ecossistema do Airbnb.

## **Marco 1**

Neste projeto vamos explorar alguns conceitos de business intelligence e colocar em prática relação de tabelas e colunas calculadas.

## **Variáveis**

**Tabela rooms (Dimensão):**

- **id:** um identificador exclusivo para cada quarto.
- **nome:** o nome do anúncio do Airbnb.
- **bairro:** sigla para o bairro em que o anúncio do Airbnb está localizado.
- **neighbourhood_group:** bairro onde o anúncio do Airbnb está localizado.
- **latitude:** a coordenada de latitude do anúncio do Airbnb.
- **longitude:** a coordenada de longitude do anúncio do Airbnb.
- **room_type:** o tipo de quarto oferecido no anúncio do Airbnb.
- **minimum_nights:** o número mínimo de noites aceito na reserva.

**Tabela hosts (Dimensão):**

- **host_id:** um identificador exclusivo para cada host.
- **nome*do*host:** o nome do anfitrião do anúncio do Airbnb.

**Tabela reviews (fatos):**

- **id:** um identificador exclusivo para cada quarto.
- **host_id:** um identificador exclusivo para cada host.
- **preço:** o preço por noite no anúncio do Airbnb.
- **number*of*reviews:** o número total de comentários que recebeu o anúncio do Airbnb.
- **last_review:** a data da última review que o anúncio do Airbnb recebeu.
- **reviews*per*month:** número médio de avaliações que o anúncio do Airbnb recebe por mês. -**calculated*host*listings_count:** o número total de listagens que o host tem.
- **availability_365:** o número de dias que o anúncio do Airbnb estará disponível para reserva em um ano

## **1. Introdução**

Graças à incessante coleta de dados que realizamos a todo momento, a tomada de decisões informada tornou-se um pilar fundamental para o sucesso empresarial. A Inteligência de Negócios (BI) emerge como uma ferramenta essencial, permitindo que organizações convertam grandes volumes de dados em informações cruciais para a tomada de decisões e ações. BI transcende a mera coleta de dados; envolve uma análise aprofundada e interpretação de padrões, oferecendo uma visão clara das operações comerciais e permitindo a identificação de oportunidades estratégicas, a melhoria da eficiência operacional e a manutenção da agilidade em um ambiente de negócios dinâmico e competitivo.

**O que você aprenderá:**

Neste projeto você aprenderá como relacionar tabelas no Power BI, a trabalhar com colunas calculadas e fórmulas DAX, além de criar um painel usando visualizações avançadas, como mapas geográficos. Essas habilidades te ajudaram em:

- Análise de dados complexos: relacionar tabelas no Power BI permite juntar conjuntos de dados de várias fontes. Isto é essencial quando você trabalha com dados complexos e precisa extrair informações valiosas de múltiplas fontes.
- Precisão de dados aprimorada: usando colunas calculadas e fórmulas DAX, você pode realizar cálculos personalizados e manipular os dados de acordo com as necessidades específicas de sua análise. Esse processo contribui para a precisão e relevância das informações que você apresenta.
- Tomada de decisão informada: a capacidade de criar painéis com visualizações avançadas, como mapas geográficos, fornecem uma maneira eficaz de representar dados visualmente. Isto facilita a identificação de padrões, tendências e relacionamentos, que por sua vez melhora a tomada de decisão informada.

## **2. Marco obrigatório - Mãos à obra!**

Business Intelligence (BI), traduzido para o portugués como inteligência de negócios ou inteligência empresarial, refere-se ao conjunto de processos, aplicações e tecnologias que permitem às empresas coletar, armazenar e analisar dados para tomar decisões informadas. O principal objetivo do business intelligence é converter grandes quantidades de dados em informações significativas e úteis, fornecendo às empresas uma visão mais clara de suas operações, clientes, fornecedores e outros aspectos relevantes.

As soluções de Business Intelligence geralmente incluem ferramentas e sistemas que facilitam a extração, transformação e carregamento de dados (ETL), armazenamento de dados, relatórios e painéis de controle (dashboards), bem como análise de dados para descobrir padrões, tendências e oportunidades. Essas ferramentas permitem aos usuários, como executivos, analistas e gerentes, acessarem informações de forma rápida e compreensível, facilitando a tomada de decisões estratégicas.

Neste projeto, você mergulhará na criação de uma estrutura de relacionamentos de tabelas que permitirão a construção de um completo painel de controle (dashboard) com recursos visuais integrados. Além disso, você terá a oportunidade de aplicar na prática todo o conhecimento adquirido sobre narrativa de dados, com o objetivo final de obter um painel fácil de usar.

**🟦 2.1 Processar e preparar a base de dados**

Conectar/importar Dados A Ferramentas

Identificar E Gerenciar Valores Nulos

Identificar E Gerenciar Valores Duplicados

Identificar E Manejar Dados Fora Do Alcance Da Análise

Identificar E Gerir Dados Discrepantes Em Variáveis Categóricas

Identificar E Gerenciar Dados Discrepantes Em Variáveis Numéricas

Verificar E Alterar Tipo De Dado

Criar Novas Variáveis

Receita Potencial Total = reviews[price] * [availability_365]

Unir Tabelas
Construir Tabelas Auxiliares (DAX)

Hospedes_Ano = 
VAR Avail = reviews[availability_365]
VAR MinNights = RELATED(rooms[minimum_nights])
RETURN
    IF(
        ISBLANK(Avail) || ISBLANK(MinNights) || MinNights = 0,
        0,
        INT(Avail / MinNights)
    )

🟪 2.2 Fazer uma análise exploratória

Agrupar dados de acordo com variáveis categóricas **✅**

Entender o em branco da variavel grupo bairro(erro relacionamento)

Visualizar as variáveis categóricas**✅**

Aplicar medidas de tendência central(MEDIANA, MEDIA, MODA)**✅**

Visualizar distribuição**✅**

Aplicar medidas de dispersão✅

Ver o comportamento dos dados ao longo do tempo✅

🟧 2.3 Resumir informações em um dashboard ou reporte

Representar Dados Através De Tabela Resumo Ou Scorecards**✅**

Representar Dados Através De Gráficos Simples**✅**

Representar Dados Através De Gráficos Ou Visuais Avançados**✅**

Aplicar Opções De Filtros Para Manejo E Interação**✅**

Criar um dashboard com Data Storytelling

🟩 2.4 Apresentar resultados

Crie uma apresentação

Apresente resultados com Data Storytelling

🛠 Ferramentas e Tecnologias:

Google BigQuery (SQL, views)

Apresentações Google (para apresentação do projeto)

Loom (gravação da apresentação em vídeo)

PowerBI

Links:

Apresentação: https://docs.google.com/presentation/d/1Im37VtAyaCR5cpYPx2YU2SO1s4-WPv26_tgXLasFOWQ/edit?usp=sharing

Vídeo Loom: https://www.loom.com/share/858986b1ff9f4ca893fcfd57771e0029?sid=a994f79f-f776-41e0-b7af-41beb3b71032
