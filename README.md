# Analisando Dados com SQL

Neste projeto, realizei uma análise exploratória sobre os dados de óbito por covid com dados extraídos no formato excel do site [Our World in Data](https://ourworldindata.org/covid-deaths). Para isso, o arquivo excel foi carregado em um banco de dados SQLite usando a o pacote sqlite3 do python, e a análise foi realizada utilizando queries SQL. 

## Perguntas norteadoras da análise
### Quais os países com o maior número de mortes?
Para realizar esta query, criei uma nóva variável chamada TotalDeathCount, de modo que a consulta utilizada foi a seguinte:

```
Select Location, MAX(cast(Total_deaths as int)) as TotalDeathCount
From Mortes
Where continent is not null 
Group by Location
order by TotalDeathCount desc
```

O resultado da consulta informou que os 5 países com o maior numero de mortes são:

![image](https://user-images.githubusercontent.com/77032413/183773194-126e657d-5a5b-4195-bfef-365f9d6a7ab1.png)

Esse resultado tem uma informação ja conhecida, porém ainda assim impressionante: O Brasil tem um número de mortes por covid superior ao da India, sendo que a população brasileira é de cerca de 212 milhões de habitantes contra 1,38 bilhão de indianos. Tendo essa informação em vista, resolvi levantar dados do Brasil e da India para fins de comparação.

### Qual a taxa de letalidade da covid no Brasil? e no Mundo?

Esta pergunta também foi respondida através da criação de uma nova coluna no banco de dados, a coluna DeathPercentage. A consulta foi feita da seguinte forma:

```
Select Location, date, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
From Mortes
Where location like '%Brazil%'
and continent is not null 
order by 1,2
```

O resultado informa que no dia 07/08 (data final da amostra), a taxa de mortalidade da covid no brasil era de aproximadamente 1,99. Isso significa que quem era diagnosticado com covid nesse dia, tinha mais ou menos 2% de chance de vir a óbito. Este valor é levemente menor que a taxa de mortalidade na América do Sul, que é de cerca de 2,1%. Entretanto, ambos os valores são superiores à média global,de cerca de 1,09%. Por fim, a taxa de mortalidade indiana esteve mais próxima da média global, com 1,19%

### Qual o percentual de infectados no Brasil? e no Mundo?

Assim como nas questões anteriores, esta pergunta foi respondida através da criação de uma nova coluna, a PercentPopulationInfected, criada a partir da razão entre o número total de casos e a população do país. A query foi feita da seguinte forma:

```
Select Location, date, Population, total_cases,  (total_cases/population)*100 as PercentPopulationInfected
From Mortes
Where location like '%Brazil%'
order by 1,2
```

Os resultados da consulta mostram que no dia 07/08/2022, a taxa de infeccção para o Brasil é de cerca de 1,58% da população. No caso da Índia, a taxa é quase o dobro da brasileira, com cerca de 3,13%. No caso da América Latina, a taxa não coincidentemente esteve próxima do valor encontrado no Brasil, com cerca de 1,44%. Este valor ja era esperado tendo em vista que o Brasil tem a maior população do continente e tem mais peso no cálculo da média. Finalmente, a taxa de infeccção global é de cerca de 7,39%, mais que o dobro da indiana. Este é um forte indício que apesar da efetividade das vacinas, a pandemia ainda não acabou.

### Quais os países com as maiores taxas de infecção?

Inspirado nos resultados da consulta anterior, resolvi investigar os países com as maiores taxas de infecção no mundo. Os resultados foram os seguintes:

![image](https://user-images.githubusercontent.com/77032413/183775450-ecd01ad9-86bd-4048-b34a-4cae8c6f4b97.png)

Não é surpreendente constatar que micropaíses como Andorra e San Marino estão entre os países com as maiores taxas de infecção. Suas pequenas populações significam que até mesmo um número baixo de infectados representa uma proporção maior em relação à população do país.

### Analisando por Regiões - Continentes com o maior número de mortes por população

Para finalizar, debrucei-me nos dados de mortes por região, realizando uma query similar à realizada na primeira pergunta, mas dessa vez com foco nos continentes.
Não coincidentemente, América do Sul e América do Norte são os continentes com o maior número de mortes.  Os números de óbitos por covid em cada continente foram os seguintes:

![image](https://user-images.githubusercontent.com/77032413/183776021-f6ffef8f-9d46-4a46-b4ff-02b7e279e9c0.png)

## Conclusão

O objetivo deste projeto era implementar linguagem SQL para a realização de uma análise exploratória dos dados sobre covid no mundo. Através da análise realizada foi possível constatar que apesar dos números de óbitos e taxas de mortalidade estejam em queda em todo o mundo, em grande parte por causa da vacinação, a pandemia ainda não acabou. Também foi possível confirmar através dos dados o custo da negligência dos governos brasileiro e estadunidense na gestão da pandemia: Os dois países tem números de obitos superiores à India e China, dois países com mais de 1 bilhão de habiantes cada, mas que adotaram politicas muito mais restritivas. 

Por fim, vale salientar que esta pesquisa se limitou apenas a investigar relações numéricas entre as variáveis disponíveis. Para uma avaliação mais completa e ilustrativa dos dados, uma análise exploratória através da implementação de gráficos pode ser mais eficiente. O código completo da análise com comentários está no arquivo Jupyter Notebook.
