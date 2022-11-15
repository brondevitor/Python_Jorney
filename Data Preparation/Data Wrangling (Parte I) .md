O objetivo dessa aula é introduzir conceitos sobre formatação de dados e formato ```tidy```.

**Tópicos**:
 - **O que é Data Wrangling:**
 - **Definição de Tidy Format** 
 - **Os 5 problemas mais comuns**
     - As Colunas são Valores e Não nomes de Variáveis
     - Múltiplas Unidades Observacionais armazenadas na mesma tabela

 

# O que é Data Wrangling

**Data Wrangling** é o processo pelo qual os dados coletados passam por uma transformação para que se tornem mais acionáveis, apropriados e valiosos para uma variedade de finalidades.

Muitas vezes o formato do dado recebido/coletado facilita a visualização e apresentação, mas dificulta a modelagem e exploração. Dessa forma, as operações envolvidas no processo de Data Wrangling tentam justamente resolver esse problema.

# Definição de Tidy Format

Wickham em seu <a href="http://vita.had.co.nz/papers/tidy-data.pdf"> artigo </a> 
definiu o conceito de tidy data de acordo com algumas propriedades que esse tipo de formato possuiria:

* Cada variável forma uma coluna e contém valores
* Cada observação forma uma linha
* Cada tipo de unidade observacional forma uma tabela



* Algumas Outras definições: 

    - Variável: Um atributo (característica). Altura, Peso, Sexo, etc.
    - Valor: Medição de alguma variável (atributo). 152 cm, 80 kg, Feminino etc.
    - Observação: Todas as medidads coletadas de um único evento, usuário, entre outros.


O que é uma unidade observacional?

A unidade observacional é a estrutura mais fundamental da observação. É nossa referência! Veja a tabela abaixo:

Temos medições de temperatura em duas cidades distintas e em semanas distintas.

| Week| Temperatura_Cidade_A | Temperatura_Cidade_B 
| :- | -: | :-: |
| 1 | 35 | 30| 30
| 2 | 40 | 33

Que também poderia ser representada assim:

| Week| 1 | 2 
| :- | -: | :-: |
| Temperatura_Cidade_A | 35 | 30| 30
| Temepratura_Cidade_B | 40 | 33

Contudo ambas são messy. O formato tidy para essa tabela seria esse:

| Week| Cidade | Temperatura 
| :- | -: | :-: |
| 1 | A | 35| 
| 1 | B | 30|
| 2 | A | 40| 
| 2 | B | 33|

Note que nossa referência (unidade observacional) é ```cidade-semana```.

Agora, imagine se estivéssemos coletando também a altitude da cidade. Note que a altitude.

Teríamos uma unidade observacional 1 (cidade-semana) e outra unidade observacional (cidade). Na mesma tabela teríamos informações sobre cidade-semana (temperaturas) e somente sobre as cidades. Do ponto de vista tidy, o ideal é que essas informações fiquem em tabelas diferentes.

Exemplo de um Messy Data (dado com formato não adequado.)

| Nome | Tratamento A | Tratamento B |
| :- | -: | :-: |
| Bruno | 25 | 2.3
| Maurício | 10 | 18.3
| Laura | 4.9 | 40.2



Exemplo de um Tidy Data (dado com formato adequado)

| Nome | Tratamento | Resultado |
| :- | -: | :-: |
| Bruno | A | 25
| Bruno | B | 2.3
| Mauricio | A | 10
| Mauricio | B | 18.3
| Laura | A | 4.9
| Laura | B | 40.2

**OBS**: Um Messy Data é todo aquele conjunto que não é Tidy :))

## Os 5 problemas mais comuns 

Em seu artigo, Wichkam traz alguns dos problemas mais frequentes em conjuntos de dados


* As colunas são valores e não nomes de variáveis
* Múltiplas variáveis armazenadas na mesma coluna
* Variáveis armazenadas nas linhas e nas colunas simultaneamente
* Múltiplos tipos de unidades observacionais armazenadas na mesma tabela
* Uma única unidade observacional representada em mais de uma tabela

Vamos estudar um pouco caso a caso e praticar um poouco a arte de transfomar os dados.

### As Colunas são Valores e Não nomes de Variáveis

Vamos ler o arquivo ```pew-raw.csv```


```python
import pandas as pd
import numpy as np
```


```python
df_pew_raw = pd.read_csv('pew_raw.csv')
```


```python
df_pew_raw
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
      <th>religion</th>
      <th>&lt;$10k</th>
      <th>$10-20k</th>
      <th>$20-30k</th>
      <th>$30-40k</th>
      <th>$40-50k</th>
      <th>$50-75k</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agnostic</td>
      <td>27</td>
      <td>34</td>
      <td>60</td>
      <td>81</td>
      <td>76</td>
      <td>137</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Atheist</td>
      <td>12</td>
      <td>27</td>
      <td>37</td>
      <td>52</td>
      <td>35</td>
      <td>70</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Buddhist</td>
      <td>27</td>
      <td>21</td>
      <td>30</td>
      <td>34</td>
      <td>33</td>
      <td>58</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Catholic</td>
      <td>418</td>
      <td>617</td>
      <td>732</td>
      <td>670</td>
      <td>638</td>
      <td>1116</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Dont know/refused</td>
      <td>15</td>
      <td>14</td>
      <td>15</td>
      <td>11</td>
      <td>10</td>
      <td>35</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Evangelical Prot</td>
      <td>575</td>
      <td>869</td>
      <td>1064</td>
      <td>982</td>
      <td>881</td>
      <td>1486</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hindu</td>
      <td>1</td>
      <td>9</td>
      <td>7</td>
      <td>9</td>
      <td>11</td>
      <td>34</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Historically Black Prot</td>
      <td>228</td>
      <td>244</td>
      <td>236</td>
      <td>238</td>
      <td>197</td>
      <td>223</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Jehovahs Witness</td>
      <td>20</td>
      <td>27</td>
      <td>24</td>
      <td>24</td>
      <td>21</td>
      <td>30</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Jewish</td>
      <td>19</td>
      <td>19</td>
      <td>25</td>
      <td>25</td>
      <td>30</td>
      <td>95</td>
    </tr>
  </tbody>
</table>
</div>



A coluna religion é um idenitifcador do registro e as demais colunas representam variáveis (faixas de valores) associadas a receita. Contudo, note que essas colunas poderiam ser condensadas em uma única coluna de receita.
Para fazer isso vamos utilizar a função ```pd.melt()``` do pandas.

```pd.melt(frame, id_vars=None, value_vars=None, var_name=None, value_name='value')```

* ```frame```: Objeto de dados (dataframe)
* ```id_vars```: Coluna identificadora (não será usada na transformação)
* ```value_vars```: Coluna(s) que você queira transformar
* ```var_name```: Nome da variável que será criada na transformação
* ```value_name```: Nome do valor associado a variável criada    


```python
pd.melt(frame = df_pew_raw , id_vars = 'religion' , var_name = 'faixa_receita' , value_name = 'frequencia')

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
      <th>religion</th>
      <th>faixa_receita</th>
      <th>frequencia</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Agnostic</td>
      <td>&lt;$10k</td>
      <td>27</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Atheist</td>
      <td>&lt;$10k</td>
      <td>12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Buddhist</td>
      <td>&lt;$10k</td>
      <td>27</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Catholic</td>
      <td>&lt;$10k</td>
      <td>418</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Dont know/refused</td>
      <td>&lt;$10k</td>
      <td>15</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Evangelical Prot</td>
      <td>&lt;$10k</td>
      <td>575</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Hindu</td>
      <td>&lt;$10k</td>
      <td>1</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Historically Black Prot</td>
      <td>&lt;$10k</td>
      <td>228</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Jehovahs Witness</td>
      <td>&lt;$10k</td>
      <td>20</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Jewish</td>
      <td>&lt;$10k</td>
      <td>19</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Agnostic</td>
      <td>$10-20k</td>
      <td>34</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Atheist</td>
      <td>$10-20k</td>
      <td>27</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Buddhist</td>
      <td>$10-20k</td>
      <td>21</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Catholic</td>
      <td>$10-20k</td>
      <td>617</td>
    </tr>
    <tr>
      <th>14</th>
      <td>Dont know/refused</td>
      <td>$10-20k</td>
      <td>14</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Evangelical Prot</td>
      <td>$10-20k</td>
      <td>869</td>
    </tr>
    <tr>
      <th>16</th>
      <td>Hindu</td>
      <td>$10-20k</td>
      <td>9</td>
    </tr>
    <tr>
      <th>17</th>
      <td>Historically Black Prot</td>
      <td>$10-20k</td>
      <td>244</td>
    </tr>
    <tr>
      <th>18</th>
      <td>Jehovahs Witness</td>
      <td>$10-20k</td>
      <td>27</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Jewish</td>
      <td>$10-20k</td>
      <td>19</td>
    </tr>
    <tr>
      <th>20</th>
      <td>Agnostic</td>
      <td>$20-30k</td>
      <td>60</td>
    </tr>
    <tr>
      <th>21</th>
      <td>Atheist</td>
      <td>$20-30k</td>
      <td>37</td>
    </tr>
    <tr>
      <th>22</th>
      <td>Buddhist</td>
      <td>$20-30k</td>
      <td>30</td>
    </tr>
    <tr>
      <th>23</th>
      <td>Catholic</td>
      <td>$20-30k</td>
      <td>732</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Dont know/refused</td>
      <td>$20-30k</td>
      <td>15</td>
    </tr>
    <tr>
      <th>25</th>
      <td>Evangelical Prot</td>
      <td>$20-30k</td>
      <td>1064</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Hindu</td>
      <td>$20-30k</td>
      <td>7</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Historically Black Prot</td>
      <td>$20-30k</td>
      <td>236</td>
    </tr>
    <tr>
      <th>28</th>
      <td>Jehovahs Witness</td>
      <td>$20-30k</td>
      <td>24</td>
    </tr>
    <tr>
      <th>29</th>
      <td>Jewish</td>
      <td>$20-30k</td>
      <td>25</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Agnostic</td>
      <td>$30-40k</td>
      <td>81</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Atheist</td>
      <td>$30-40k</td>
      <td>52</td>
    </tr>
    <tr>
      <th>32</th>
      <td>Buddhist</td>
      <td>$30-40k</td>
      <td>34</td>
    </tr>
    <tr>
      <th>33</th>
      <td>Catholic</td>
      <td>$30-40k</td>
      <td>670</td>
    </tr>
    <tr>
      <th>34</th>
      <td>Dont know/refused</td>
      <td>$30-40k</td>
      <td>11</td>
    </tr>
    <tr>
      <th>35</th>
      <td>Evangelical Prot</td>
      <td>$30-40k</td>
      <td>982</td>
    </tr>
    <tr>
      <th>36</th>
      <td>Hindu</td>
      <td>$30-40k</td>
      <td>9</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Historically Black Prot</td>
      <td>$30-40k</td>
      <td>238</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Jehovahs Witness</td>
      <td>$30-40k</td>
      <td>24</td>
    </tr>
    <tr>
      <th>39</th>
      <td>Jewish</td>
      <td>$30-40k</td>
      <td>25</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Agnostic</td>
      <td>$40-50k</td>
      <td>76</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Atheist</td>
      <td>$40-50k</td>
      <td>35</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Buddhist</td>
      <td>$40-50k</td>
      <td>33</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Catholic</td>
      <td>$40-50k</td>
      <td>638</td>
    </tr>
    <tr>
      <th>44</th>
      <td>Dont know/refused</td>
      <td>$40-50k</td>
      <td>10</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Evangelical Prot</td>
      <td>$40-50k</td>
      <td>881</td>
    </tr>
    <tr>
      <th>46</th>
      <td>Hindu</td>
      <td>$40-50k</td>
      <td>11</td>
    </tr>
    <tr>
      <th>47</th>
      <td>Historically Black Prot</td>
      <td>$40-50k</td>
      <td>197</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Jehovahs Witness</td>
      <td>$40-50k</td>
      <td>21</td>
    </tr>
    <tr>
      <th>49</th>
      <td>Jewish</td>
      <td>$40-50k</td>
      <td>30</td>
    </tr>
    <tr>
      <th>50</th>
      <td>Agnostic</td>
      <td>$50-75k</td>
      <td>137</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Atheist</td>
      <td>$50-75k</td>
      <td>70</td>
    </tr>
    <tr>
      <th>52</th>
      <td>Buddhist</td>
      <td>$50-75k</td>
      <td>58</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Catholic</td>
      <td>$50-75k</td>
      <td>1116</td>
    </tr>
    <tr>
      <th>54</th>
      <td>Dont know/refused</td>
      <td>$50-75k</td>
      <td>35</td>
    </tr>
    <tr>
      <th>55</th>
      <td>Evangelical Prot</td>
      <td>$50-75k</td>
      <td>1486</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Hindu</td>
      <td>$50-75k</td>
      <td>34</td>
    </tr>
    <tr>
      <th>57</th>
      <td>Historically Black Prot</td>
      <td>$50-75k</td>
      <td>223</td>
    </tr>
    <tr>
      <th>58</th>
      <td>Jehovahs Witness</td>
      <td>$50-75k</td>
      <td>30</td>
    </tr>
    <tr>
      <th>59</th>
      <td>Jewish</td>
      <td>$50-75k</td>
      <td>95</td>
    </tr>
  </tbody>
</table>
</div>



Perceba o que ocorreu:
+ Cada coluna (faixa) foi transformada em uma linha (uma nova linha para cada religião). 
+ O valor contido nas celulas do primeiro data frame foram transferidos para a coluna de frequência, onde há o match exato entre religião e faixa.

Billboard é um ranking musical lá dos EUA que mostra o ranking semanal das músicas ao longo de 75 semanas, a partir do momento em que a música entra no TOP 100.
* Problemas:
    - As colunas possuem valores (x1st.week, x2nd.week e etc.)
* O que podemos fazer:
    - Criar uma única coluna com as datas (cada semana irá tornar-se uma linha para cada música do ranking) e usar seu valor para criar a colunad e ranking. No caso de valores missing, deletaremos as linhas referentes às músicas que estiveram fora do ranking por menos de 75 semanas.


```python
df_billboard = pd.read_csv('billboard.csv')
```


```python
df_billboard.head()
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
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>date.entered</th>
      <th>date.peaked</th>
      <th>x1st.week</th>
      <th>x2nd.week</th>
      <th>x3rd.week</th>
      <th>...</th>
      <th>x67th.week</th>
      <th>x68th.week</th>
      <th>x69th.week</th>
      <th>x70th.week</th>
      <th>x71st.week</th>
      <th>x72nd.week</th>
      <th>x73rd.week</th>
      <th>x74th.week</th>
      <th>x75th.week</th>
      <th>x76th.week</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Independent Women Part I</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-09-23</td>
      <td>2000-11-18</td>
      <td>78</td>
      <td>63.0</td>
      <td>49.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000</td>
      <td>Santana</td>
      <td>Maria, Maria</td>
      <td>4:18</td>
      <td>Rock</td>
      <td>2000-02-12</td>
      <td>2000-04-08</td>
      <td>15</td>
      <td>8.0</td>
      <td>6.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000</td>
      <td>Savage Garden</td>
      <td>I Knew I Loved You</td>
      <td>4:07</td>
      <td>Rock</td>
      <td>1999-10-23</td>
      <td>2000-01-29</td>
      <td>71</td>
      <td>48.0</td>
      <td>43.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000</td>
      <td>Madonna</td>
      <td>Music</td>
      <td>3:45</td>
      <td>Rock</td>
      <td>2000-08-12</td>
      <td>2000-09-16</td>
      <td>41</td>
      <td>23.0</td>
      <td>18.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000</td>
      <td>Aguilera, Christina</td>
      <td>Come On Over Baby (All I Want Is You)</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-08-05</td>
      <td>2000-10-14</td>
      <td>57</td>
      <td>47.0</td>
      <td>45.0</td>
      <td>...</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 83 columns</p>
</div>




```python
id_vars = ['year' ,'artist.inverted','track' , 'time' , 'genre' , 'date.entered' , 'date.peaked']

df_tidy_billboard = pd.melt(df_billboard , id_vars = id_vars , var_name = 'semanas' , value_name = 'posicao')
```


```python
df_tidy_billboard.dropna(inplace = True)
```


```python
df_tidy_billboard.head()
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
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>date.entered</th>
      <th>date.peaked</th>
      <th>semanas</th>
      <th>posicao</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Independent Women Part I</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-09-23</td>
      <td>2000-11-18</td>
      <td>x1st.week</td>
      <td>78.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000</td>
      <td>Santana</td>
      <td>Maria, Maria</td>
      <td>4:18</td>
      <td>Rock</td>
      <td>2000-02-12</td>
      <td>2000-04-08</td>
      <td>x1st.week</td>
      <td>15.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000</td>
      <td>Savage Garden</td>
      <td>I Knew I Loved You</td>
      <td>4:07</td>
      <td>Rock</td>
      <td>1999-10-23</td>
      <td>2000-01-29</td>
      <td>x1st.week</td>
      <td>71.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000</td>
      <td>Madonna</td>
      <td>Music</td>
      <td>3:45</td>
      <td>Rock</td>
      <td>2000-08-12</td>
      <td>2000-09-16</td>
      <td>x1st.week</td>
      <td>41.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000</td>
      <td>Aguilera, Christina</td>
      <td>Come On Over Baby (All I Want Is You)</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-08-05</td>
      <td>2000-10-14</td>
      <td>x1st.week</td>
      <td>57.0</td>
    </tr>
  </tbody>
</table>
</div>



* Exercício:
    - Extraia o Número da Semana da coluna semana e armazene em uma nova coluna (tipo inteiro).
    - Extraia a Data referente a semana específica para cada artista e música e armazene em uma nova coluna.

Para o primeiro exercício precisamos usar uma expressão regular (regex) para extrair padrões em cadeias de caracteres (strings)


```python
df_tidy_billboard['semana_numero'] = df_tidy_billboard['semanas'].str.extract('(\d+)', expand=False).astype(int)
```


```python
df_tidy_billboard.head()
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
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>date.entered</th>
      <th>date.peaked</th>
      <th>semanas</th>
      <th>posicao</th>
      <th>semana_numero</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Independent Women Part I</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-09-23</td>
      <td>2000-11-18</td>
      <td>x1st.week</td>
      <td>78.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000</td>
      <td>Santana</td>
      <td>Maria, Maria</td>
      <td>4:18</td>
      <td>Rock</td>
      <td>2000-02-12</td>
      <td>2000-04-08</td>
      <td>x1st.week</td>
      <td>15.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000</td>
      <td>Savage Garden</td>
      <td>I Knew I Loved You</td>
      <td>4:07</td>
      <td>Rock</td>
      <td>1999-10-23</td>
      <td>2000-01-29</td>
      <td>x1st.week</td>
      <td>71.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000</td>
      <td>Madonna</td>
      <td>Music</td>
      <td>3:45</td>
      <td>Rock</td>
      <td>2000-08-12</td>
      <td>2000-09-16</td>
      <td>x1st.week</td>
      <td>41.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000</td>
      <td>Aguilera, Christina</td>
      <td>Come On Over Baby (All I Want Is You)</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-08-05</td>
      <td>2000-10-14</td>
      <td>x1st.week</td>
      <td>57.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



 Temos a data de entrada (que seria a primeira semana).
Então basta contar quantas semanas a frente da primeira semana estamos e adicionar a data de entrada para obter a data atual


```python
pd.to_datetime(df_tidy_billboard['date.entered'], format = '%Y-%m-%d')
```




    0       2000-09-23
    1       2000-02-12
    2       1999-10-23
    3       2000-08-12
    4       2000-08-05
               ...    
    19663   1999-06-05
    19700   1999-09-11
    19980   1999-06-05
    20017   1999-09-11
    20334   1999-09-11
    Name: date.entered, Length: 5307, dtype: datetime64[ns]




```python
pd.to_timedelta(df_tidy_billboard['semana_numero'], unit = 'W')
```




    0         7 days
    1         7 days
    2         7 days
    3         7 days
    4         7 days
              ...   
    19663   441 days
    19700   441 days
    19980   448 days
    20017   448 days
    20334   455 days
    Name: semana_numero, Length: 5307, dtype: timedelta64[ns]



A primeira semana se refere a data de entrada. A data segunda semana, se refere a data da primeira semana + 1 semana inteira. E assim por diante.

Data da semana = Data Entrada + Semanas Passadas 

Semanas passadas = Semana atual - 1


```python
df_tidy_billboard['semanas_passadas'] = df_tidy_billboard['semana_numero'] - 1
```


```python
df_tidy_billboard['semana_data'] = pd.to_datetime(df_tidy_billboard['date.entered']) + pd.to_timedelta(df_tidy_billboard['semanas_passadas'], unit='w')
```


```python
df_tidy_billboard.head()
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
      <th>year</th>
      <th>artist.inverted</th>
      <th>track</th>
      <th>time</th>
      <th>genre</th>
      <th>date.entered</th>
      <th>date.peaked</th>
      <th>semanas</th>
      <th>posicao</th>
      <th>semana_numero</th>
      <th>semanas_passadas</th>
      <th>semana_data</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2000</td>
      <td>Destiny's Child</td>
      <td>Independent Women Part I</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-09-23</td>
      <td>2000-11-18</td>
      <td>x1st.week</td>
      <td>78.0</td>
      <td>1</td>
      <td>0</td>
      <td>2000-09-23</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2000</td>
      <td>Santana</td>
      <td>Maria, Maria</td>
      <td>4:18</td>
      <td>Rock</td>
      <td>2000-02-12</td>
      <td>2000-04-08</td>
      <td>x1st.week</td>
      <td>15.0</td>
      <td>1</td>
      <td>0</td>
      <td>2000-02-12</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2000</td>
      <td>Savage Garden</td>
      <td>I Knew I Loved You</td>
      <td>4:07</td>
      <td>Rock</td>
      <td>1999-10-23</td>
      <td>2000-01-29</td>
      <td>x1st.week</td>
      <td>71.0</td>
      <td>1</td>
      <td>0</td>
      <td>1999-10-23</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2000</td>
      <td>Madonna</td>
      <td>Music</td>
      <td>3:45</td>
      <td>Rock</td>
      <td>2000-08-12</td>
      <td>2000-09-16</td>
      <td>x1st.week</td>
      <td>41.0</td>
      <td>1</td>
      <td>0</td>
      <td>2000-08-12</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2000</td>
      <td>Aguilera, Christina</td>
      <td>Come On Over Baby (All I Want Is You)</td>
      <td>3:38</td>
      <td>Rock</td>
      <td>2000-08-05</td>
      <td>2000-10-14</td>
      <td>x1st.week</td>
      <td>57.0</td>
      <td>1</td>
      <td>0</td>
      <td>2000-08-05</td>
    </tr>
  </tbody>
</table>
</div>



### Múltiplas Unidades Observacionais armazenadas na mesma tabela

Mesmo já melhorando a estrutura mais bruta do dataset, note que existe muita informação repetida (ano, título da música, artista, tempo de execução e gênero)

Isso ocorre, pois nosso dataset ainda não está de acordo com os princípios tidy em sua totalidade. Há um em especial que está sendo violado.

Veja que temos mais de uma unidade observacional nos nossos dados: (Ranking+Música+Data) e Música, que contém informações descritivas como artista, tempo e gênero. Pelo princípio do tidy data, precisamos separar esas informações em tabelas separadas.

Inicialmente vamos separar as músicas e suas respectivas informações.


```python
# Criando uma tabela separada
info_colunas_musicas = ["year" , "artist.inverted" , "track" , "time" , "genre"]
songs = df_tidy_billboard.loc[: , info_colunas_musicas]
```


```python
# Vamos resetar o índice
songs.reset_index(drop=True) # sem o drop ele cria uma coluna nova com o índice antigo
# vamos criar uma nova coluna de identificador de música
songs['identificador_musica'] = np.arange(1, len(songs)+1)
```


```python
info_colunas_ranking = ["posicao","semana_data"]
ranking = df_tidy_billboard.loc[:, info_colunas_ranking]
ranking['identificador_musica'] = songs['identificador_musica']
```


```python
ranking.head()
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
      <th>posicao</th>
      <th>semana_data</th>
      <th>identificador_musica</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>78.0</td>
      <td>2000-09-23</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>15.0</td>
      <td>2000-02-12</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>71.0</td>
      <td>1999-10-23</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>41.0</td>
      <td>2000-08-12</td>
      <td>4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>57.0</td>
      <td>2000-08-05</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



Agora as duas tabelas podem se comunicar por meio da coluna ```identificador_musica```.
