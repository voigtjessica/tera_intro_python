
# Introdução ao Pandas

Agora vamos colocar a mão na massa!

Ao final dessa aula vamos conseguir realizar as três primeiras etapas da análise de dados com DataFrames e teremos instrumentos suficientes para responder diversas perguntas que podemos fazer aos nossos dados (4a. etapa)

## Algumas orientações:

1. A partir de agora, as anotações deverão ser apenas no seu script, utilizando os # para fazer comentários:


```python
#Isso é um comentário
```

2. Copiem o código das células (espaços em cinza) no seu notebook
   Pelo menos nesta aula, não se preocupem em escrever o código e procurem entender o que cada função faz. Isso é essencial    para entender a lógica da linguagem e não perder tempo com typos; <br>

3. Não se preocupem muito com a sintaxe;<br>

4. Usem o seu notebook para análises futuras.

## 0. Lembrando: objetos e listas

Podemos atribuir a um objeto o resultado de nossas operações matemáticas, strings, listas ou, como veremos mais pra frente, o nosso dataframe. O objeto ficará armazenado na memória do jupyter e poderá ser acessado e modificado em operações futuras.

Para atribuir uma operação a um objeto basta utilizar o sinal de =


```python
# Exemplo:

obj = 2*4

lista = [0, 1, 1, 2, 3, 5, 8, 13]

lista_str = ["Jessica", "Alex"]

lista_de_objs = ["Jessica", 15]
```

*É possível traduzir esta operação com "obj RECEBE 2*4*, *lista recebe...* e assim em diante


```python
#Consultando:

obj
```

E realizar operações com ele


```python
obj * 10
```

E realizar operações com elementos da lista


```python
#Realizando operações com elementos da lista:

lista2 = []

for i in lista:
    lista2.append(i*10)

lista2
```

## Bibliotecas:

Bibliotecas são conjuntos de verbos que auxiliam a linguagem que estamos utilizando. São como "pacotes de expansão", permitindo que mais coisas sejam feitas dentro de uma determinada linguagem.

Uma biblioteca muito utilizada na ciência de dados em python é a **Pandas** .

Quando Python é instalado utilizando Anaconda, as bibliotecas *Pandas* e *NumPy* são instaladas automaticamente. Caso contrário, é possível instalar *Pandas* via pip no CMD com o comando `pip install pandas`.

Depois de instalado, a biblioteca deve ser carregada no início de cada script:




```python
#importando a biblioteca pandas e dando um "apelido" para ela.

import pandas as pd

#pd é a nomenclatura padrão.
```

Pronto, só isso =)

As funções (verbos) costumam ser precedidos da biblioteca ou do objeto que ele irá trabalhar. Veremos exemplos a seguir.

## 01. Checklist dos B.O.s


Como falamos nos slides, existem alguns B.O.s que são figurinhas marcadas na análise de dados:

* Separador
* Encoding
* Duplicata
* Missings

Resolver esses problemas é uma etapa importante para a normalização do seu Dataframe e impede erros de interpretação futuros. Boa parte dos problemas acontecem no momento da importação!

### Importando

Vamos começar importando um banco de dados com todos os países do mundo e algumas informações básicas. Trata-se de um arquivo CSV, separado por ponto e vírgula e com encoding 'utf-8' 



```python
data_countries = pd.read_csv("data_countries.csv", encoding = 'utf-8', sep=";")
```

Em python é assim: se não aconteceu nada, é porque deu certo!

As informações de encoding e separador normalmente estão descritas na fonte do seu banco de dados (no codebook, no local de download, etc). A função `pd.read_csv()` possui como padrão o separador "," e encoding = none e por isso temos que avisar à função que o encoding e o separador são diferentes do esperado. Para saber todos os parâmetros padrões da função, use o comando `help(pd.read_csv)` 

Vamos primeiro olhar algumas características do banco, como número de linhas e colunas com o `shape`:

Agora vamos olhar se a nossa importação deu certo com o comando `head()`, que mostra as 5 primeiras linhas do nosso DataFrame:


```python
#Verificando as cinco primeiras colunas
data_countries.head()
```

A nossa importação deu certo: as linhas e colunas parecem estar "no lugar". Mas esse banco tem um problema: **vocês conseguiram identificar?**

<br><br> Pois é, vamos dar uma olhadinha na quantidade de linhas e colunas que o nosso arquivo importado possui:


```python
#Olhando qtde de linhas e colunas do meu df
data_countries.shape
```

Agora, vamos resolver o problema das duplicatas, atribuindo ao nosso DataFrame original o resultado da operação `drop_duplicates()`:


```python
#Removendo duplicatas:
data_countries = data_countries.drop_duplicates()
```

E vamos olhar novamente o número de linhas e colunas para ver se algo mudou no nosso arquivo:


```python
#Olhando qtde de linhas e colunas do meu df
data_countries.shape
```

Já cairam dois, viram?
E, por fim, vamos descobrir quantos *missings* temos em cada coluna somando todos com duas operações seguidas: `is.null()`e `sum()`


```python
# Contando a quantidade de missing values por coluna:
data_countries.isnull().sum()
```

Que bom, nosso banco não tem *missings*.

Por fim, vamos ver os tipos de colunas existentes no nosso banco:


```python
#Verificando os tipos de colunas:
data_countries.dtypes
```

Não foi possível identificar os tipos de coluna. *Object* costuma indicar que existem "coisas" diferentes em uma mesma variável, como um número e uma string . Neste caso, eu sei que tudo é texto. Como veremos, podemos sobrescrever as variáveis de um dataframe e transforma-las em um outro tipo.
<br><br>
Agora vamos importar um segundo arquivo da FIFA com todos os jogadores convocados para a copa de 2018. 
É um arquivo do Excel (.XLSX):


```python
fifa = pd.read_excel("fifa2018.xlsx")
```

Vamos executar as mesmas etapas que fizemos com o banco anterior. Primeiro vamos ver se importou certinho:


```python
#Agora, o head
fifa.head()
```


```python
#Agora, o shape:
fifa.shape
```

Não sabemos se esse arquivo tem duplicatas e não vamos olhar as 736 linhas. Neste caso, vamos dar um `drop_duplicates()` preventivo:


```python
#Removendo duplicatas:
fifa = fifa.drop_duplicates()
fifa.shape
```




    (736, 12)




```python
Não tinham duplicatas =)
```


```python
#Verificando os tipos de colunas:
fifa.dtypes
```

*Float* e *Interger* são duas categorias numéricas em programação e variam quanto a sua precisão. Como vamos trabalhar com bancos simples, não existe diferença significativa na precisão dos números. Essas categorias são mais importantes para trabalhos científicos ou análises mais complexas.

Saber os tipos de colunas é importante para evitar erros na importação de cadastros numéricos, como CPFs.


```python
#Verificando soma dos missings dentro de nosso dataframe:
fifa.isnull().sum()
```




    number                 0
    fifa_display_name      0
    last_name              0
    first_name             0
    shirt_name             0
    day_of_birth           0
    position               0
    club                   0
    height                 0
    caps                   4
    goals                290
    team                   0
    dtype: int64



### Substituindo os valores nulos de uma variável:

Em alguns casos, nós queremos substituir os *missings* do nosso banco porque eles significam alguma coisa. No caso, a ausência de registros em goals e caps significa que os jogadores não jogaram nenhum jogo ou não fizeram nenhum gol com a camisa da seleção. <br><br> Para os cálculos estatísticos na sessão seguinte, é importante que esses registros sejam contatos. 

Agora vamos substituir aqueles *missings* encontrados no banco fifa por 0 com o `fillna()`:


```python
#Sobrescrevendo NAs de uma coluna:
fifa.goals = fifa.goals.fillna(0)
fifa.caps = fifa.caps.fillna(0)

#Consultando soma dos NAs
fifa.isnull().sum()
```




    number               0
    fifa_display_name    0
    last_name            0
    first_name           0
    shirt_name           0
    day_of_birth         0
    position             0
    club                 0
    height               0
    caps                 0
    goals                0
    team                 0
    dtype: int64



**Pergunta:** E se os *missings* estivessem em uma variável tipo *string*? Como eu poderia substitui-los?

## 02. Análises descritivas do banco

Nós já fizemos algumas descrições iniciais com o `head` e o `dtypes`, agora vamos nos aprofundar mais na pergunta *O que que tem nesse Dataframe?*

Sabemos que o dataframe Fifa contém informações sobre os jogadores convocados para copa do mundo de 2018. Vamos olhar uma descrição mais detalhada dos nossos dados com o `describe`:


```python
#Breve descrição estatística:

fifa.describe()
```

Se eu quisesse apenas somar os valores de uma coluna, digamos, para saber quantos gols ao todo foram feitos por todos os jogadores até o momento da copa?


```python
#somando itens de uma coluna:
fifa.goals.sum()
```

E a média de gols por jogador?


```python
#média dos valores de uma coluna
fifa.goals.median()
```

E qual foi o maior número de jogos jogados pela seleção (caps) de um jogador?


```python
#media dos valores de uma coluna
fifa.caps.max()
```

**Importante** O python 


Agora eu gostaria de saber todas as seleções classificadas para a copa do mundo de 2018 (em outras palavras: todos os valores únicos da coluna "Team"):


```python
#Acessando os valores únicos de uma variável:
pd.unique(fifa.team)
```

E podemos atribuir esses valores a um objeto:


```python
#Atribuindo os valores únicos de uma coluna a um objeto
paises = pd.unique(fifa.team)

paises
```

E se eu quisesse que esse array virasse parte de um dataframe chamado `paises`, dentro de uma coluna chamada `pais` ?


```python
#Criando um dataframe com uma coluna chamada 'pais'
paises = pd.DataFrame({'pais': pd.unique(fifa.team)})

paises.head()
```

Por fim, eu gostaria de fazer uma *query* e descobrir quais foram os jogadores da seleção brasileira na copa de 2018. Para isso, selecionarei as linhas cujo team == 'Brazil' <br><br> As querys funcionam bem quando você sabe o valor que quer buscar


```python
# Realizando uma query: coluna igual a um determinado valor   
fifa.query('team == "Brazil"')
```

Agora, vocês lembram do jogador que jogou 158 jogos pela sua seleção? Vamos descobrir quem é esse cara. Uma forma é realizando uma outra query:


```python
fifa.query('caps == 158')
```

Mas se nós não soubessemos ainda o valor máximo? Podemos mandar "imprimir" uma linha dizendo *"Imprima o banco fifa onde fifa.caps seja exatamente igual ao fifa.caps.max()"*


```python
# Localize no banco Fifa onde fifa capps é exatamente igual ao máximo valor de fifa caps

fifa.loc[fifa.caps == fifa.caps.max()]
```

## 03. Processamento de dados

Nós também já fizemos algum processamento no banco fifa quando substituimos os missings por zero e quando nós removemos as duplicatas do banco data_countries, mas agora o processamento é pra valer :)

As vezes, precisamos não apenas consultar mas também modificar nossos dados para análise ou construção de um novo dataframe.

### Criando novas variáveis

Agora eu gostaria de criar uma nova variável no meu banco fifa, para afirmar que o esporte praticado por todos aqueles atletas é "futebol"



```python
#Criando uma nova variável chamada esporte

fifa['esporte'] = "Futebol"
```

Ao atribuir o valor "Futebol" para o objeto `fifa['esporte']`, já estamos automaticamente criando a nova coluna no nosso dataframe.

*OBS: A outra sintaxe `fifa.esporte` não funciona para criação de novas colunas


```python
fifa.head()
```

Agora, digamos que vocês estão elaborando as estatísticas dos jogadores antes da copa. Com essas informações da Fifa, seria interessante saber qual é a média de gols por jogador com a camisa da sua seleção. Para isso vamos criar uma nova coluna que divide o número de gols (goals) pelo número de jogos pela seleção (caps).


```python
#Criando uma variável
fifa['media_gols'] = fifa.goals/fifa.caps
fifa.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>number</th>
      <th>fifa_display_name</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>caps</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>10.02.1986</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>18.03.1987</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.142857</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>31.08.1992</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>20.09.1986</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.200000</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>30.01.1986</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.017241</td>
    </tr>
  </tbody>
</table>
</div>



E se quisessemos diminuir o número de casas decimais na nossa nova variável?


```python
#Criando uma variável
fifa['media_gols'] = round(fifa.goals/fifa.caps,2)
fifa.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>number</th>
      <th>fifa_display_name</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>caps</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>10.02.1986</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>18.03.1987</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>31.08.1992</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>20.09.1986</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>30.01.1986</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
</div>



### Modificando datas

Vocês devem ter reparado quando usamos a função `fifa.dtypes` que a coluna *day_of_birt* está como *object*, neste caso texto. Vamos sobrescrever essa coluna com os mesmos valores, mas como Data. Para isso usamos a função do pandas `pd.to_datetime`. 


```python
#Sobrescrevendo variável categórica como data:

fifa['day_of_birth'] = pd.to_datetime(fifa['day_of_birth']) 

#Consultando:
fifa.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>number</th>
      <th>fifa_display_name</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>caps</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
</div>



### Renomeando variáveis

Caps é um nome muito ruim. Vamos colocar um nome mais intuitívo, como *jogos_selecao*


```python
#Renomeando e atribuindo ao nosso dataframe:
fifa = fifa.rename(columns={"caps": "jogos_selecao"})

fifa.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>number</th>
      <th>fifa_display_name</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
</div>



E se eu quisesse agora renomear várias colunas ao mesmo tempo?


```python
#Renomeando
fifa = fifa.rename(columns={"number": "num_camisa",
                            "fifa_display_name": "nome_fifa"})
fifa.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_camisa</th>
      <th>nome_fifa</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
    </tr>
  </tbody>
</table>
</div>



### Juntando dois dataframes

Uma das belezas de trabalhar com dados em Python é que podemos juntar dois dataframes diferentes por meio de uma *chave* comum ou "colar" dois dataframes iguais um sobre o outro.

#### Merge

Com a função `merge` é possível juntar dois dataframes horizontalmente baseados em uma coluna (key) comum. Ele opera de maneira similar ao SQL::JOIN e ao DPLYR::left_join


**Pergunta:** Qual é a chave entre os bancos fifa e data_countries?
<br><br>
Vamos criar um terceiro dataframe chamado fifa_countries, que irá conter a junção dos bancos fifa e data_countries:


```python
#Joining

fifa_countries = pd.merge(fifa, data_countries, left_on=['team'], right_on=['country'])

fifa_countries.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_camisa</th>
      <th>nome_fifa</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>team</th>
      <th>media_gols</th>
      <th>country</th>
      <th>continent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>Argentina</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
      <td>Argentina</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>Argentina</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
      <td>Argentina</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
      <td>Argentina</td>
      <td>South America</td>
    </tr>
  </tbody>
</table>
</div>



Tivemos que usar os parâmetros `left_on` e `right_on` porque as colunas-chave possuem nomes diferentes nos dois bancos. A outra opção é renomear a coluna de um dos dois bancos para precisarmos apenas especificar o parâmetro `on`, como faremos a seguir:


```python
#Renomeando:
fifa = fifa.rename(columns={'team': 'country'})

#Juntando:
fifa_countries = pd.merge(fifa, data_countries, on='country')

fifa_countries.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_camisa</th>
      <th>nome_fifa</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>country</th>
      <th>media_gols</th>
      <th>continent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
      <td>South America</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
      <td>South America</td>
    </tr>
  </tbody>
</table>
</div>



### Concat

Para "empilhar" dois dataframes, vamos usar a função `concat`.
Vamos criar um dataframe com apenas uma observação (linha), que seria um novo jogador inventado para a seleção da Argentina. Esse novo dataframe terá as mesmas colunas do dataframe fifa:


```python
extra = pd.DataFrame({'num_camisa': [100],
                      'nome_fifa': ['DALÍ'],
                      'last_name': ['Dalí'],
                      'first_name': ['Salvador'],
                      'shirt_name': ['DALÍ'],
                      'day_of_birth': ['2013-10-10'],
                      'position': ['GK'],
                      'club': ['Barcelona(ESP)'],
                      'height': [100],
                      'jogos_selecao': [0],
                      'goals': [0],
                      'country': ['Argentina'],
                      'esporte': ['Futebol'],
                      'media_gols': [0],
                      'continent': ['South America']})

extra
```


```python
#Concatenando dois dataframes:

fifa_extra = pd.concat([fifa , extra], axis = 0, sort=False)

```

O parâmetro `axis = 0` indica que os dataframes serão agrupados verticalmente, e o parâmetro `sort = FALSE` estabelece que não haverá alteração na disposição das colunas


```python
#Procurando os jogadores do time da Argentina:

fifa_extra.query('country == "Argentina"')
```

Nosso novo jogador está aí ;)

**Pergunta:** Que outra consulta eu poderia ter feito para saber se o meu jogador estava ou não aí?

### Realizando operações com agrupamentos

Agora que nós já aprendemos analisar, processar e fundir diferentes bancos de dados restam apenas as operações seguidas de  agrupamentos.

**Quando nós usamos agrupamentos:** Normalmente quando queremos informações sobre *grupos* de dentro do meu dataframe. 

Vamos supor que eu esteja muito interessada em comparar estatísticas entre jogadores altos e baixos e vamos estabelecer que um jogador alto é aquele que tem mais de 1,80m. Vamos realizar essas análises com a determinação dos grupos "alto" e "baixo. Para isso, vamos primeiro criar uma nova coluna *boolean* (true e false), que me dirá se determinado jogador tem mais de 1,80m:


```python
#Criando nossa coluna boolean:
fifa_countries['alto'] = fifa_countries.height > 180

#Verificando:
fifa_countries.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_camisa</th>
      <th>nome_fifa</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>country</th>
      <th>media_gols</th>
      <th>continent</th>
      <th>alto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>Nahuel GUZMAN</td>
      <td>GUZMÁN</td>
      <td>Nahuel Ignacio</td>
      <td>GUZMÁN</td>
      <td>1986-10-02</td>
      <td>GK</td>
      <td>Tigres (MEX)</td>
      <td>192</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>South America</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>Gabriel MERCADO</td>
      <td>MERCADO</td>
      <td>Gabriel Ivan</td>
      <td>MERCADO</td>
      <td>1987-03-18</td>
      <td>DF</td>
      <td>Sevilla FC (ESP)</td>
      <td>181</td>
      <td>21.0</td>
      <td>3.0</td>
      <td>Argentina</td>
      <td>0.14</td>
      <td>South America</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>Nicolas TAGLIAFICO</td>
      <td>TAGLIAFICO</td>
      <td>Nicolás Alejandro</td>
      <td>TAGLIAFICO</td>
      <td>1992-08-31</td>
      <td>DF</td>
      <td>Ajax (NED)</td>
      <td>169</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Argentina</td>
      <td>0.00</td>
      <td>South America</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>Cristian ANSALDI</td>
      <td>ANSALDI</td>
      <td>Cristian Daniel</td>
      <td>ANSALDI</td>
      <td>1986-09-20</td>
      <td>DF</td>
      <td>Torino (ITA)</td>
      <td>181</td>
      <td>5.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.20</td>
      <td>South America</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>Lucas BIGLIA</td>
      <td>BIGLIA</td>
      <td>Lucas Rodrigo</td>
      <td>BIGLIA</td>
      <td>1986-01-30</td>
      <td>MF</td>
      <td>AC Milan (ITA)</td>
      <td>175</td>
      <td>58.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.02</td>
      <td>South America</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
</div>



Vamos contar quandos jogadores nós temos acima e abaixo de 1,80m:


```python
#Agrupe o fifa_countries de acordo com a variável 'alto' e me retorne a contagem de gols para cada grupo
fifa_countries.groupby('alto')['alto'].count()
```

Agora vamos verificar qual é a média de gols para jogadores maiores e menores que 180. Para isso, vamos usar o `groupby`e depois `sum`:


```python
#Agrupe o fifa_countries de acordo com a variável 'alto' e me retorne a média de gols para cada grupo
fifa_countries.groupby('alto')['goals'].median()
```

Agora eu vou agrupar por *continent* e *alto*:


```python
#Agrupe o fifa_countries de acordo com as combinações das variáveis 'continent' e 'alto' e me retorne 
#a média de gols para cada grupo
fifa_countries.groupby(['continent','alto'])['goals'].median()
```

Agora além de agrupar por *continent* e *alto* eu vou verificar a média de gols e o total de gols feitos. Para isso vamos usar o `agg`:


```python
#Agrupe o fifa_countries de acordo com as combinações das varaveis 'continent' e 'alto' e me 
#retorne a média e a soma da variável 'goals' para cada grupo
fifa_countries.groupby(['continent','alto']).agg({'goals': ['median', 'sum']})
```

Agora em um nível mais hard (nível 5 na escala CRÉU) eu quero saber, além da média e do total de gols, quantos jogadores acima e abaixo de 1,80m eu tenho em cada continente:


```python
#Agrupe o fifa_countries de acordo com as combinações das varaveis 'continent' e 'alto' e me 
#retorne a contagem da variável 'alto' para cada grupo e a média e a soma da variável 'goals' para cada grupo
fifa_countries.groupby(['continent','alto']).agg({'alto':['count'],
                                                  'goals': ['median', 'sum']})
```

Agora, vamos salvar essa última operação em um novo dataframe e exportar esse dataframe como um .csv e um .excel:


```python
#Armazenando o objeto:
final = fifa_countries.groupby(['continent','alto']).agg({'alto':['count'],
                                                  'goals': ['median', 'sum']})

#Consultando:
final.head()
```

Especificando o diretório onde vamos salvar nosso arquivo (sem essa etapa, ele vai salvar na mesma pasta onde está este notebook).

Lembrando: barra para direita: "/" ou duas barras para esquerda: "\\"


```python
#Diretório:
import os
os.chdir("C:/Users/coliv/Documents/Python Scripts/tera_intro_python/docs")

#Conferindo:
os.getcwd()
```


```python
#Exportando em CSV
final.to_csv(r'final.csv')

#Exportando em Excel
final.to_excel(r'final.xlsx')
```

OBS: As vezes ocorre um erro na exportação em excel, dizendo que um determinado módulo não foi encontrado . Eu resolvi o meu problema executando `pip install xlsxwriter` no prompt
<br><br>

### Extra groupby + loc

A maioria das nossas operações de agrupamentos retornaram apenas os valores das operações. E se quisessemos que usar uma operação de agrupamento seguida de uma filtragem?

Quem são os jogadores mais altos de cada time?


```python
# Atribuia ao objeto mais_altos // localize no banco fifa_countries , depois de agrupart por 'country' , 
# os index máximos de cada 'height'

mais_altos = fifa_countries.loc[fifa_countries.groupby('country')['height'].idxmax()]

mais_altos.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>num_camisa</th>
      <th>nome_fifa</th>
      <th>last_name</th>
      <th>first_name</th>
      <th>shirt_name</th>
      <th>day_of_birth</th>
      <th>position</th>
      <th>club</th>
      <th>height</th>
      <th>jogos_selecao</th>
      <th>goals</th>
      <th>country</th>
      <th>media_gols</th>
      <th>continent</th>
      <th>alto</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>Federico FAZIO</td>
      <td>FAZIO</td>
      <td>Federico Julián</td>
      <td>FAZIO</td>
      <td>1987-03-17</td>
      <td>DF</td>
      <td>AS Roma (ITA)</td>
      <td>199</td>
      <td>9.0</td>
      <td>1.0</td>
      <td>Argentina</td>
      <td>0.11</td>
      <td>South America</td>
      <td>True</td>
    </tr>
    <tr>
      <th>34</th>
      <td>12</td>
      <td>Brad JONES</td>
      <td>JONES</td>
      <td>Bradley Scott</td>
      <td>JONES</td>
      <td>1982-03-19</td>
      <td>GK</td>
      <td>Feyenoord (NED)</td>
      <td>193</td>
      <td>6.0</td>
      <td>0.0</td>
      <td>Australia</td>
      <td>0.00</td>
      <td>Oceania</td>
      <td>True</td>
    </tr>
    <tr>
      <th>46</th>
      <td>1</td>
      <td>Thibaut COURTOIS</td>
      <td>COURTOIS</td>
      <td>Thibaut Nicolas</td>
      <td>COURTOIS</td>
      <td>1992-11-05</td>
      <td>GK</td>
      <td>Chelsea (ENG)</td>
      <td>199</td>
      <td>59.0</td>
      <td>0.0</td>
      <td>Belgium</td>
      <td>0.00</td>
      <td>Europe</td>
      <td>True</td>
    </tr>
    <tr>
      <th>84</th>
      <td>16</td>
      <td>CASSIO</td>
      <td>RAMOS</td>
      <td>Cassio</td>
      <td>CASSIO</td>
      <td>1987-06-06</td>
      <td>GK</td>
      <td>SC Corinthians (BRA)</td>
      <td>195</td>
      <td>1.0</td>
      <td>0.0</td>
      <td>Brazil</td>
      <td>0.00</td>
      <td>South America</td>
      <td>True</td>
    </tr>
    <tr>
      <th>104</th>
      <td>13</td>
      <td>Yerry MINA</td>
      <td>MINA GONZALEZ</td>
      <td>Yerry Fernando</td>
      <td>Y. MINA</td>
      <td>1994-09-23</td>
      <td>DF</td>
      <td>FC Barcelona (ESP)</td>
      <td>194</td>
      <td>12.0</td>
      <td>3.0</td>
      <td>Colombia</td>
      <td>0.25</td>
      <td>South America</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>



Nós utilizamos o parâmetro `idxmax()` no fim da operação porque queremos que o `loc` encontre o *index* associado a cada valor máximo de 'height'.

## Exercício:

Agora que nóspassamos por todas essas etapas vamos fazer um exercício para fixar os conteúdos?

<br> Abra um novo notebook para os exercícios e realize as seguintes operações:

1. Importe o arquivo fifa2018 e o arquivo data_countries como fizemos em aula;
2. Verifique se há missings, duplicatas e substitua os missings por 0, quando for necessário;
3. Junte os dois dataframes fifa e data_countries baseados em uma chave comum
    - lembre-se que a chave tem que ser a mesma dos dois lados!
    
4. Responda (com prints de operações ou de dataframes) as seguintes perguntas:
   - a) Qual é média de altura dos jogadores?
   - b) Quantos jogadores têm mais de 30 anos?
   - c) Quem são os goleiros (GK) de cada time?
   - d) Quais são os jogadores com o maior número de gols de cada time?
   
As respostas estão no [gabarito](https://voigtjessica.github.io/tera_intro_python/gabarito)

