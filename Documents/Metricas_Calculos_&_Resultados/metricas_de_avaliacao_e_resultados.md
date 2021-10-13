# Métricas de Avaliação

## MTTF, MTTR, MTBF e Disponibilidade

------

```python
# Carregando Pandas para uso nos calculos das planilhas
import pandas as pd
from datetime import datetime
# Carregando arquivo.csv em memória (para manipulaçaõ)
# Em meu caso, eu estou modificando o "df = pandas.read_csv('../Database/dataset.csv')"
# Eu estou utilizando uma função lambda para usar um parse no campo datas
dateparse = lambda x: datetime.strptime(x, '%H:%M:%S')
df = pd.read_csv('../Database/dataset.csv', parse_dates=True, date_parser=dateparse)
df.head(31)
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
      <th>tempo_inicio_recebimento_mercadoria</th>
      <th>tempo_fim_recebimento_mercadoria</th>
      <th>tempo_inicio_pre_triagem</th>
      <th>tempo_fim_pre_triagem</th>
      <th>tempo_inicio_raio_x</th>
      <th>tempo_fim_raio_x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>05:16:21</td>
      <td>05:30:21</td>
      <td>18:53:43</td>
      <td>18:57:43</td>
      <td>16:43:04</td>
      <td>16:44:04</td>
    </tr>
    <tr>
      <th>1</th>
      <td>07:51:07</td>
      <td>08:05:07</td>
      <td>01:12:47</td>
      <td>01:19:47</td>
      <td>17:55:17</td>
      <td>17:59:17</td>
    </tr>
    <tr>
      <th>2</th>
      <td>03:28:10</td>
      <td>03:37:10</td>
      <td>16:28:54</td>
      <td>16:34:54</td>
      <td>05:45:50</td>
      <td>05:45:50</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14:54:25</td>
      <td>14:56:25</td>
      <td>17:58:10</td>
      <td>18:07:10</td>
      <td>14:33:26</td>
      <td>14:48:26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14:49:53</td>
      <td>14:57:53</td>
      <td>06:19:11</td>
      <td>06:23:11</td>
      <td>00:40:29</td>
      <td>00:43:29</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04:24:12</td>
      <td>04:27:12</td>
      <td>21:25:22</td>
      <td>21:35:22</td>
      <td>06:37:33</td>
      <td>06:51:33</td>
    </tr>
    <tr>
      <th>6</th>
      <td>04:33:30</td>
      <td>04:47:30</td>
      <td>02:30:53</td>
      <td>02:40:53</td>
      <td>08:01:21</td>
      <td>08:05:21</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22:50:12</td>
      <td>22:53:12</td>
      <td>13:24:58</td>
      <td>13:35:58</td>
      <td>10:29:12</td>
      <td>10:36:12</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13:44:59</td>
      <td>13:48:59</td>
      <td>09:16:28</td>
      <td>09:29:28</td>
      <td>09:26:00</td>
      <td>09:36:00</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14:50:50</td>
      <td>14:52:50</td>
      <td>13:37:44</td>
      <td>13:40:44</td>
      <td>10:48:19</td>
      <td>10:52:19</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12:32:16</td>
      <td>12:45:16</td>
      <td>15:15:16</td>
      <td>15:30:16</td>
      <td>05:41:14</td>
      <td>05:52:14</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10:54:34</td>
      <td>10:54:34</td>
      <td>16:26:16</td>
      <td>16:38:16</td>
      <td>20:19:28</td>
      <td>20:30:28</td>
    </tr>
    <tr>
      <th>12</th>
      <td>03:16:24</td>
      <td>03:28:24</td>
      <td>13:05:14</td>
      <td>13:17:14</td>
      <td>22:33:44</td>
      <td>22:43:44</td>
    </tr>
    <tr>
      <th>13</th>
      <td>17:09:17</td>
      <td>17:19:17</td>
      <td>13:54:10</td>
      <td>13:59:10</td>
      <td>20:21:11</td>
      <td>20:36:11</td>
    </tr>
    <tr>
      <th>14</th>
      <td>07:32:06</td>
      <td>07:45:06</td>
      <td>12:40:02</td>
      <td>12:47:02</td>
      <td>11:38:40</td>
      <td>11:52:40</td>
    </tr>
    <tr>
      <th>15</th>
      <td>01:07:34</td>
      <td>01:11:34</td>
      <td>23:58:09</td>
      <td>00:03:09</td>
      <td>05:11:24</td>
      <td>05:14:24</td>
    </tr>
    <tr>
      <th>16</th>
      <td>22:23:57</td>
      <td>22:30:57</td>
      <td>09:43:03</td>
      <td>09:50:03</td>
      <td>09:33:34</td>
      <td>09:38:34</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12:28:29</td>
      <td>12:39:29</td>
      <td>14:01:29</td>
      <td>14:06:29</td>
      <td>12:05:57</td>
      <td>12:11:57</td>
    </tr>
    <tr>
      <th>18</th>
      <td>21:55:29</td>
      <td>21:58:29</td>
      <td>07:52:06</td>
      <td>07:59:06</td>
      <td>05:16:42</td>
      <td>05:26:42</td>
    </tr>
    <tr>
      <th>19</th>
      <td>09:38:08</td>
      <td>09:46:08</td>
      <td>20:29:04</td>
      <td>20:39:04</td>
      <td>05:29:26</td>
      <td>05:40:26</td>
    </tr>
    <tr>
      <th>20</th>
      <td>03:53:04</td>
      <td>04:06:04</td>
      <td>16:15:00</td>
      <td>16:23:00</td>
      <td>13:11:08</td>
      <td>13:23:08</td>
    </tr>
    <tr>
      <th>21</th>
      <td>17:33:54</td>
      <td>17:48:54</td>
      <td>15:00:34</td>
      <td>15:06:34</td>
      <td>22:46:14</td>
      <td>22:51:14</td>
    </tr>
    <tr>
      <th>22</th>
      <td>18:11:42</td>
      <td>18:18:42</td>
      <td>16:30:01</td>
      <td>16:40:01</td>
      <td>13:57:02</td>
      <td>14:01:02</td>
    </tr>
    <tr>
      <th>23</th>
      <td>18:26:26</td>
      <td>18:37:26</td>
      <td>01:54:48</td>
      <td>02:09:48</td>
      <td>04:18:59</td>
      <td>04:31:59</td>
    </tr>
    <tr>
      <th>24</th>
      <td>04:42:27</td>
      <td>04:47:27</td>
      <td>15:06:39</td>
      <td>15:20:39</td>
      <td>08:54:14</td>
      <td>09:06:14</td>
    </tr>
    <tr>
      <th>25</th>
      <td>04:48:58</td>
      <td>04:52:58</td>
      <td>04:44:45</td>
      <td>04:47:45</td>
      <td>23:08:56</td>
      <td>23:18:56</td>
    </tr>
    <tr>
      <th>26</th>
      <td>16:57:28</td>
      <td>17:02:28</td>
      <td>08:40:10</td>
      <td>08:54:10</td>
      <td>05:27:54</td>
      <td>05:35:54</td>
    </tr>
    <tr>
      <th>27</th>
      <td>16:41:53</td>
      <td>16:55:53</td>
      <td>16:24:08</td>
      <td>16:34:08</td>
      <td>03:03:40</td>
      <td>03:05:40</td>
    </tr>
    <tr>
      <th>28</th>
      <td>10:04:45</td>
      <td>10:13:45</td>
      <td>18:51:15</td>
      <td>19:05:15</td>
      <td>13:04:02</td>
      <td>13:19:02</td>
    </tr>
    <tr>
      <th>29</th>
      <td>19:26:42</td>
      <td>19:37:42</td>
      <td>20:57:19</td>
      <td>21:09:19</td>
      <td>23:06:30</td>
      <td>23:13:30</td>
    </tr>
  </tbody>
</table>
</div>

```python
#
# Etapas para tratar os dados e realizar os cálculos - MTTF, MTTR, MTBF e Disponibilidade - Tempo de Recebimento de Mercadoria
#
```

```python
# Primeiramente, iremos Ordenar o valores de recebimento, para criar uma sequência de dados crescente (tempo_inicio_recebimento_mercadoria)
```

```python
df.sort_values(by=['tempo_inicio_recebimento_mercadoria'])
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
      <th>tempo_inicio_recebimento_mercadoria</th>
      <th>tempo_fim_recebimento_mercadoria</th>
      <th>tempo_inicio_pre_triagem</th>
      <th>tempo_fim_pre_triagem</th>
      <th>tempo_inicio_raio_x</th>
      <th>tempo_fim_raio_x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>15</th>
      <td>01:07:34</td>
      <td>01:11:34</td>
      <td>23:58:09</td>
      <td>00:03:09</td>
      <td>05:11:24</td>
      <td>05:14:24</td>
    </tr>
    <tr>
      <th>12</th>
      <td>03:16:24</td>
      <td>03:28:24</td>
      <td>13:05:14</td>
      <td>13:17:14</td>
      <td>22:33:44</td>
      <td>22:43:44</td>
    </tr>
    <tr>
      <th>2</th>
      <td>03:28:10</td>
      <td>03:37:10</td>
      <td>16:28:54</td>
      <td>16:34:54</td>
      <td>05:45:50</td>
      <td>05:45:50</td>
    </tr>
    <tr>
      <th>20</th>
      <td>03:53:04</td>
      <td>04:06:04</td>
      <td>16:15:00</td>
      <td>16:23:00</td>
      <td>13:11:08</td>
      <td>13:23:08</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04:24:12</td>
      <td>04:27:12</td>
      <td>21:25:22</td>
      <td>21:35:22</td>
      <td>06:37:33</td>
      <td>06:51:33</td>
    </tr>
    <tr>
      <th>6</th>
      <td>04:33:30</td>
      <td>04:47:30</td>
      <td>02:30:53</td>
      <td>02:40:53</td>
      <td>08:01:21</td>
      <td>08:05:21</td>
    </tr>
    <tr>
      <th>24</th>
      <td>04:42:27</td>
      <td>04:47:27</td>
      <td>15:06:39</td>
      <td>15:20:39</td>
      <td>08:54:14</td>
      <td>09:06:14</td>
    </tr>
    <tr>
      <th>25</th>
      <td>04:48:58</td>
      <td>04:52:58</td>
      <td>04:44:45</td>
      <td>04:47:45</td>
      <td>23:08:56</td>
      <td>23:18:56</td>
    </tr>
    <tr>
      <th>0</th>
      <td>05:16:21</td>
      <td>05:30:21</td>
      <td>18:53:43</td>
      <td>18:57:43</td>
      <td>16:43:04</td>
      <td>16:44:04</td>
    </tr>
    <tr>
      <th>14</th>
      <td>07:32:06</td>
      <td>07:45:06</td>
      <td>12:40:02</td>
      <td>12:47:02</td>
      <td>11:38:40</td>
      <td>11:52:40</td>
    </tr>
    <tr>
      <th>1</th>
      <td>07:51:07</td>
      <td>08:05:07</td>
      <td>01:12:47</td>
      <td>01:19:47</td>
      <td>17:55:17</td>
      <td>17:59:17</td>
    </tr>
    <tr>
      <th>19</th>
      <td>09:38:08</td>
      <td>09:46:08</td>
      <td>20:29:04</td>
      <td>20:39:04</td>
      <td>05:29:26</td>
      <td>05:40:26</td>
    </tr>
    <tr>
      <th>28</th>
      <td>10:04:45</td>
      <td>10:13:45</td>
      <td>18:51:15</td>
      <td>19:05:15</td>
      <td>13:04:02</td>
      <td>13:19:02</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10:54:34</td>
      <td>10:54:34</td>
      <td>16:26:16</td>
      <td>16:38:16</td>
      <td>20:19:28</td>
      <td>20:30:28</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12:28:29</td>
      <td>12:39:29</td>
      <td>14:01:29</td>
      <td>14:06:29</td>
      <td>12:05:57</td>
      <td>12:11:57</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12:32:16</td>
      <td>12:45:16</td>
      <td>15:15:16</td>
      <td>15:30:16</td>
      <td>05:41:14</td>
      <td>05:52:14</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13:44:59</td>
      <td>13:48:59</td>
      <td>09:16:28</td>
      <td>09:29:28</td>
      <td>09:26:00</td>
      <td>09:36:00</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14:49:53</td>
      <td>14:57:53</td>
      <td>06:19:11</td>
      <td>06:23:11</td>
      <td>00:40:29</td>
      <td>00:43:29</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14:50:50</td>
      <td>14:52:50</td>
      <td>13:37:44</td>
      <td>13:40:44</td>
      <td>10:48:19</td>
      <td>10:52:19</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14:54:25</td>
      <td>14:56:25</td>
      <td>17:58:10</td>
      <td>18:07:10</td>
      <td>14:33:26</td>
      <td>14:48:26</td>
    </tr>
    <tr>
      <th>27</th>
      <td>16:41:53</td>
      <td>16:55:53</td>
      <td>16:24:08</td>
      <td>16:34:08</td>
      <td>03:03:40</td>
      <td>03:05:40</td>
    </tr>
    <tr>
      <th>26</th>
      <td>16:57:28</td>
      <td>17:02:28</td>
      <td>08:40:10</td>
      <td>08:54:10</td>
      <td>05:27:54</td>
      <td>05:35:54</td>
    </tr>
    <tr>
      <th>13</th>
      <td>17:09:17</td>
      <td>17:19:17</td>
      <td>13:54:10</td>
      <td>13:59:10</td>
      <td>20:21:11</td>
      <td>20:36:11</td>
    </tr>
    <tr>
      <th>21</th>
      <td>17:33:54</td>
      <td>17:48:54</td>
      <td>15:00:34</td>
      <td>15:06:34</td>
      <td>22:46:14</td>
      <td>22:51:14</td>
    </tr>
    <tr>
      <th>22</th>
      <td>18:11:42</td>
      <td>18:18:42</td>
      <td>16:30:01</td>
      <td>16:40:01</td>
      <td>13:57:02</td>
      <td>14:01:02</td>
    </tr>
    <tr>
      <th>23</th>
      <td>18:26:26</td>
      <td>18:37:26</td>
      <td>01:54:48</td>
      <td>02:09:48</td>
      <td>04:18:59</td>
      <td>04:31:59</td>
    </tr>
    <tr>
      <th>29</th>
      <td>19:26:42</td>
      <td>19:37:42</td>
      <td>20:57:19</td>
      <td>21:09:19</td>
      <td>23:06:30</td>
      <td>23:13:30</td>
    </tr>
    <tr>
      <th>18</th>
      <td>21:55:29</td>
      <td>21:58:29</td>
      <td>07:52:06</td>
      <td>07:59:06</td>
      <td>05:16:42</td>
      <td>05:26:42</td>
    </tr>
    <tr>
      <th>16</th>
      <td>22:23:57</td>
      <td>22:30:57</td>
      <td>09:43:03</td>
      <td>09:50:03</td>
      <td>09:33:34</td>
      <td>09:38:34</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22:50:12</td>
      <td>22:53:12</td>
      <td>13:24:58</td>
      <td>13:35:58</td>
      <td>10:29:12</td>
      <td>10:36:12</td>
    </tr>
  </tbody>
</table>
</div>

```python
tempo_inicio_recebimento_mercadoria = df['tempo_inicio_recebimento_mercadoria']
```

```python
tempo_fim_recebimento_mercadoria = df['tempo_fim_recebimento_mercadoria']
```

```python
tempo_inicio_recebimento_mercadoria = pd.to_datetime(tempo_inicio_recebimento_mercadoria)
```

```python
tempo_fim_recebimento_mercadoria = pd.to_datetime(tempo_fim_recebimento_mercadoria)
```

```python
# Usando uma variável para armazenar o valores de Tempo Total Disponível (1ª tempo inicial - último tempo final)
# Com a subtração acima, é possível ter um valor de tempo disponível
```

```python
tempo_total_disponivel_recebimento_mercadoria = tempo_fim_recebimento_mercadoria[7] - tempo_inicio_recebimento_mercadoria[15]
```

```python
tempo_total_disponivel_recebimento_mercadoria
```

    Timedelta('0 days 21:45:38')

```python
tempo_duracao_recebimento_mercadoria = tempo_fim_recebimento_mercadoria - tempo_inicio_recebimento_mercadoria
```

```python
tempo_duracao_recebimento_mercadoria
```

    0    0 days 00:14:00
    1    0 days 00:14:00
    2    0 days 00:09:00
    3    0 days 00:02:00
    4    0 days 00:08:00
    5    0 days 00:03:00
    6    0 days 00:14:00
    7    0 days 00:03:00
    8    0 days 00:04:00
    9    0 days 00:02:00
    10   0 days 00:13:00
    11   0 days 00:00:00
    12   0 days 00:12:00
    13   0 days 00:10:00
    14   0 days 00:13:00
    15   0 days 00:04:00
    16   0 days 00:07:00
    17   0 days 00:11:00
    18   0 days 00:03:00
    19   0 days 00:08:00
    20   0 days 00:13:00
    21   0 days 00:15:00
    22   0 days 00:07:00
    23   0 days 00:11:00
    24   0 days 00:05:00
    25   0 days 00:04:00
    26   0 days 00:05:00
    27   0 days 00:14:00
    28   0 days 00:09:00
    29   0 days 00:11:00
    dtype: timedelta64[ns]

```python
# Usando uma variável para armazenar Tempo Total Utilizado (somando todos os valores da sequência de tempo de duração)
# Com esta operação, é possível ter o valor final de tempo de atividade (produção)
```

```python
tempo_total_utilizado_recebimento_mercadoria = tempo_duracao_recebimento_mercadoria.sum()
```

```python
tempo_total_utilizado_recebimento_mercadoria
```

    Timedelta('0 days 04:08:00')

```python
# Usando uma variável para armazernar o Tempo Total de Paradas (subtraindo do Tempo Total Disponível o Tempo Total Utilizado)
```

```python
tempo_total_parada_recebimento_mercadoria = tempo_total_disponivel_recebimento_mercadoria - tempo_total_utilizado_recebimento_mercadoria
```

```python
tempo_total_parada_recebimento_mercadoria
```

    Timedelta('0 days 17:37:38')

```python
# Usando uma variável para armazenar as ocorrências de paradas (quantidade de registro -1, nos mostra todas as paradas da série)
```

```python
qtde_paradas_recebimento_mercadorias = tempo_duracao_recebimento_mercadoria.count() - 1
```

```python
qtde_paradas_recebimento_mercadorias
```

    29

```python
# Calculando o MTTF das Colunas de Recebimento
```

```python
mttf_recebimento_mercadoria = tempo_duracao_recebimento_mercadoria.mean()
```

```python
mttf_recebimento_mercadoria
```

    Timedelta('0 days 00:08:16')

```python
# Calculando o MTTR das Colunas de Recebimento
```

```python
mttr_recebimento_mercadoria = tempo_total_parada_recebimento_mercadoria / qtde_paradas_recebimento_mercadorias
```

```python
mttr_recebimento_mercadoria
```

    Timedelta('0 days 00:36:28.206896551')

```python
# Calculando o MTBF das Colunas de Recebimento
```

```python
mtbf_recebimento_mercadoria = (tempo_total_disponivel_recebimento_mercadoria - tempo_total_parada_recebimento_mercadoria) / qtde_paradas_recebimento_mercadorias
```

```python
mtbf_recebimento_mercadoria
```

    Timedelta('0 days 00:08:33.103448275')

```python
# Calculando a Disponibilidade das Colunas de Recebimento
```

```python
disponibilidade_recebimento_mercadoria = mtbf_recebimento_mercadoria / (mttr_recebimento_mercadoria + mtbf_recebimento_mercadoria)
```

```python
disponibilidade_recebimento_mercadoria
```

    0.18994613086859172

```python
#
# Etapas para tratar os dados e realizar os cálculos - MTTF, MTTR, MTBF e Disponibilidade - Tempo de Pré Triagem
#
```

```python
# Primeiramente, iremos Ordenar o valores de recebimento, para criar uma sequência de dados crescente (tempo_inicio_pre_triagem)
```

```python
df.sort_values(by=['tempo_inicio_pre_triagem'])
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
      <th>tempo_inicio_recebimento_mercadoria</th>
      <th>tempo_fim_recebimento_mercadoria</th>
      <th>tempo_inicio_pre_triagem</th>
      <th>tempo_fim_pre_triagem</th>
      <th>tempo_inicio_raio_x</th>
      <th>tempo_fim_raio_x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>07:51:07</td>
      <td>08:05:07</td>
      <td>01:12:47</td>
      <td>01:19:47</td>
      <td>17:55:17</td>
      <td>17:59:17</td>
    </tr>
    <tr>
      <th>23</th>
      <td>18:26:26</td>
      <td>18:37:26</td>
      <td>01:54:48</td>
      <td>02:09:48</td>
      <td>04:18:59</td>
      <td>04:31:59</td>
    </tr>
    <tr>
      <th>6</th>
      <td>04:33:30</td>
      <td>04:47:30</td>
      <td>02:30:53</td>
      <td>02:40:53</td>
      <td>08:01:21</td>
      <td>08:05:21</td>
    </tr>
    <tr>
      <th>25</th>
      <td>04:48:58</td>
      <td>04:52:58</td>
      <td>04:44:45</td>
      <td>04:47:45</td>
      <td>23:08:56</td>
      <td>23:18:56</td>
    </tr>
    <tr>
      <th>4</th>
      <td>14:49:53</td>
      <td>14:57:53</td>
      <td>06:19:11</td>
      <td>06:23:11</td>
      <td>00:40:29</td>
      <td>00:43:29</td>
    </tr>
    <tr>
      <th>18</th>
      <td>21:55:29</td>
      <td>21:58:29</td>
      <td>07:52:06</td>
      <td>07:59:06</td>
      <td>05:16:42</td>
      <td>05:26:42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>16:57:28</td>
      <td>17:02:28</td>
      <td>08:40:10</td>
      <td>08:54:10</td>
      <td>05:27:54</td>
      <td>05:35:54</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13:44:59</td>
      <td>13:48:59</td>
      <td>09:16:28</td>
      <td>09:29:28</td>
      <td>09:26:00</td>
      <td>09:36:00</td>
    </tr>
    <tr>
      <th>16</th>
      <td>22:23:57</td>
      <td>22:30:57</td>
      <td>09:43:03</td>
      <td>09:50:03</td>
      <td>09:33:34</td>
      <td>09:38:34</td>
    </tr>
    <tr>
      <th>14</th>
      <td>07:32:06</td>
      <td>07:45:06</td>
      <td>12:40:02</td>
      <td>12:47:02</td>
      <td>11:38:40</td>
      <td>11:52:40</td>
    </tr>
    <tr>
      <th>12</th>
      <td>03:16:24</td>
      <td>03:28:24</td>
      <td>13:05:14</td>
      <td>13:17:14</td>
      <td>22:33:44</td>
      <td>22:43:44</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22:50:12</td>
      <td>22:53:12</td>
      <td>13:24:58</td>
      <td>13:35:58</td>
      <td>10:29:12</td>
      <td>10:36:12</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14:50:50</td>
      <td>14:52:50</td>
      <td>13:37:44</td>
      <td>13:40:44</td>
      <td>10:48:19</td>
      <td>10:52:19</td>
    </tr>
    <tr>
      <th>13</th>
      <td>17:09:17</td>
      <td>17:19:17</td>
      <td>13:54:10</td>
      <td>13:59:10</td>
      <td>20:21:11</td>
      <td>20:36:11</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12:28:29</td>
      <td>12:39:29</td>
      <td>14:01:29</td>
      <td>14:06:29</td>
      <td>12:05:57</td>
      <td>12:11:57</td>
    </tr>
    <tr>
      <th>21</th>
      <td>17:33:54</td>
      <td>17:48:54</td>
      <td>15:00:34</td>
      <td>15:06:34</td>
      <td>22:46:14</td>
      <td>22:51:14</td>
    </tr>
    <tr>
      <th>24</th>
      <td>04:42:27</td>
      <td>04:47:27</td>
      <td>15:06:39</td>
      <td>15:20:39</td>
      <td>08:54:14</td>
      <td>09:06:14</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12:32:16</td>
      <td>12:45:16</td>
      <td>15:15:16</td>
      <td>15:30:16</td>
      <td>05:41:14</td>
      <td>05:52:14</td>
    </tr>
    <tr>
      <th>20</th>
      <td>03:53:04</td>
      <td>04:06:04</td>
      <td>16:15:00</td>
      <td>16:23:00</td>
      <td>13:11:08</td>
      <td>13:23:08</td>
    </tr>
    <tr>
      <th>27</th>
      <td>16:41:53</td>
      <td>16:55:53</td>
      <td>16:24:08</td>
      <td>16:34:08</td>
      <td>03:03:40</td>
      <td>03:05:40</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10:54:34</td>
      <td>10:54:34</td>
      <td>16:26:16</td>
      <td>16:38:16</td>
      <td>20:19:28</td>
      <td>20:30:28</td>
    </tr>
    <tr>
      <th>2</th>
      <td>03:28:10</td>
      <td>03:37:10</td>
      <td>16:28:54</td>
      <td>16:34:54</td>
      <td>05:45:50</td>
      <td>05:45:50</td>
    </tr>
    <tr>
      <th>22</th>
      <td>18:11:42</td>
      <td>18:18:42</td>
      <td>16:30:01</td>
      <td>16:40:01</td>
      <td>13:57:02</td>
      <td>14:01:02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14:54:25</td>
      <td>14:56:25</td>
      <td>17:58:10</td>
      <td>18:07:10</td>
      <td>14:33:26</td>
      <td>14:48:26</td>
    </tr>
    <tr>
      <th>28</th>
      <td>10:04:45</td>
      <td>10:13:45</td>
      <td>18:51:15</td>
      <td>19:05:15</td>
      <td>13:04:02</td>
      <td>13:19:02</td>
    </tr>
    <tr>
      <th>0</th>
      <td>05:16:21</td>
      <td>05:30:21</td>
      <td>18:53:43</td>
      <td>18:57:43</td>
      <td>16:43:04</td>
      <td>16:44:04</td>
    </tr>
    <tr>
      <th>19</th>
      <td>09:38:08</td>
      <td>09:46:08</td>
      <td>20:29:04</td>
      <td>20:39:04</td>
      <td>05:29:26</td>
      <td>05:40:26</td>
    </tr>
    <tr>
      <th>29</th>
      <td>19:26:42</td>
      <td>19:37:42</td>
      <td>20:57:19</td>
      <td>21:09:19</td>
      <td>23:06:30</td>
      <td>23:13:30</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04:24:12</td>
      <td>04:27:12</td>
      <td>21:25:22</td>
      <td>21:35:22</td>
      <td>06:37:33</td>
      <td>06:51:33</td>
    </tr>
    <tr>
      <th>15</th>
      <td>01:07:34</td>
      <td>01:11:34</td>
      <td>23:58:09</td>
      <td>00:03:09</td>
      <td>05:11:24</td>
      <td>05:14:24</td>
    </tr>
  </tbody>
</table>
</div>

```python
tempo_inicio_pre_triagem = df['tempo_inicio_pre_triagem']
```

```python
tempo_fim_pre_triagem = df['tempo_fim_pre_triagem']
```

```python
tempo_inicio_pre_triagem = pd.to_datetime(tempo_inicio_pre_triagem)
```

```python
tempo_fim_pre_triagem = pd.to_datetime(tempo_fim_pre_triagem)
```

```python
# Usando uma variável para armazenar o valores de Tempo Total Disponível (1ª tempo inicial - último tempo final)
# Com a subtração acima, é possível ter um valor de tempo disponível
# O uso da operação "+ pd.Timedelta("1 days")" é para corrigir erro do valor negativo.
```

```python
tempo_total_disponivel_pre_triagem = (tempo_fim_pre_triagem[15] - tempo_inicio_pre_triagem[1]) + pd.Timedelta("1 days")
```

```python
tempo_total_disponivel_pre_triagem
```

    Timedelta('0 days 22:50:22')

```python
tempo_duracao_pre_triagem = tempo_fim_pre_triagem - tempo_inicio_pre_triagem
```

```python
tempo_duracao_pre_triagem
```

    0      0 days 00:04:00
    1      0 days 00:07:00
    2      0 days 00:06:00
    3      0 days 00:09:00
    4      0 days 00:04:00
    5      0 days 00:10:00
    6      0 days 00:10:00
    7      0 days 00:11:00
    8      0 days 00:13:00
    9      0 days 00:03:00
    10     0 days 00:15:00
    11     0 days 00:12:00
    12     0 days 00:12:00
    13     0 days 00:05:00
    14     0 days 00:07:00
    15   -1 days +00:05:00
    16     0 days 00:07:00
    17     0 days 00:05:00
    18     0 days 00:07:00
    19     0 days 00:10:00
    20     0 days 00:08:00
    21     0 days 00:06:00
    22     0 days 00:10:00
    23     0 days 00:15:00
    24     0 days 00:14:00
    25     0 days 00:03:00
    26     0 days 00:14:00
    27     0 days 00:10:00
    28     0 days 00:14:00
    29     0 days 00:12:00
    dtype: timedelta64[ns]

```python
# Usando uma variável para armazenar Tempo Total Utilizado (somando todos os valores da sequência de tempo de duração)
# Com esta operação, é possível ter o valor final de tempo de atividade (produção)
```

```python
tempo_total_utilizado_pre_triagem = tempo_duracao_pre_triagem.sum() + pd.Timedelta("1 days")
```

```python
tempo_total_utilizado_pre_triagem
```

    Timedelta('0 days 04:28:00')

```python
# Usando uma variável para armazernar o Tempo Total de Paradas (subtraindo do Tempo Total Disponível o Tempo Total Utilizado)
```

```python
tempo_total_parada_pre_triagem = tempo_total_disponivel_pre_triagem - tempo_total_utilizado_pre_triagem
```

```python
tempo_total_parada_pre_triagem
```

    Timedelta('0 days 18:22:22')

```python
# Usando uma variável para armazenar as ocorrências de paradas (quantidade de registro -1, nos mostra todas as paradas da série)
```

```python
qtde_paradas_pre_triagem = tempo_duracao_pre_triagem.count() - 1
```

```python
qtde_paradas_pre_triagem
```

    29

```python
# Calculando o MTTF das Colunas de Pré Triagem
```

```python
# Correção do item 15 da nossa lista (-1 days +00:05:00).
# A correção é necessária para desprezar o valor de "-1 days" do resultado. Correto é "00:05:00"
# Observação-1: a multiplicação por -1 (*-1) não resolvia o problema, por que estamos trabalhando com tipo data/hora
# Observação-2: esta solução deve ser executada uma única vez. Caso necessário uma segunda ou mais execuções
#               será necessário "limpar todas as saídas" e rexecutar todos os calculos novamente.
```

```python
x_tempo_duracao_pre_triagem = tempo_duracao_pre_triagem
```

```python
x_tempo_duracao_pre_triagem[15] = x_tempo_duracao_pre_triagem[15] + pd.Timedelta("1 days")
```

```python
mttf_pre_triagem = x_tempo_duracao_pre_triagem.mean()
```

```python
mttf_pre_triagem
```

    Timedelta('0 days 00:08:56')

```python
# Calculando o MTTR das Colunas de Pré Triagem
```

```python
mttr_pre_triagem = tempo_total_parada_pre_triagem / qtde_paradas_pre_triagem
```

```python
mttr_pre_triagem
```

    Timedelta('0 days 00:38:00.758620689')

```python
# Calculando o MTBF das Colunas de Pré Triagem
```

```python
mtbf_pre_triagem = (tempo_total_disponivel_pre_triagem - tempo_total_parada_pre_triagem) / qtde_paradas_pre_triagem
```

```python
mtbf_pre_triagem
```

    Timedelta('0 days 00:09:14.482758620')

```python
# Calculando a Disponibilidade das Colunas de Pré Triagem
```

```python
disponibilidade_pre_triagem = mtbf_pre_triagem / (mttr_pre_triagem + mtbf_pre_triagem)
```

```python
disponibilidade_pre_triagem
```

    0.19556809612983905

```python
#
# Etapas para tratar os dados e realizar os cálculos - MTTF, MTTR, MTBF e Disponibilidade - Tempo de Raio X
#
```

```python
# Primeiramente, iremos Ordenar o valores de recebimento, para criar uma sequência de dados crescente (tempo_inicio_raio_x)
```

```python
df.sort_values(by=['tempo_inicio_raio_x'])
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
      <th>tempo_inicio_recebimento_mercadoria</th>
      <th>tempo_fim_recebimento_mercadoria</th>
      <th>tempo_inicio_pre_triagem</th>
      <th>tempo_fim_pre_triagem</th>
      <th>tempo_inicio_raio_x</th>
      <th>tempo_fim_raio_x</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>14:49:53</td>
      <td>14:57:53</td>
      <td>06:19:11</td>
      <td>06:23:11</td>
      <td>00:40:29</td>
      <td>00:43:29</td>
    </tr>
    <tr>
      <th>27</th>
      <td>16:41:53</td>
      <td>16:55:53</td>
      <td>16:24:08</td>
      <td>16:34:08</td>
      <td>03:03:40</td>
      <td>03:05:40</td>
    </tr>
    <tr>
      <th>23</th>
      <td>18:26:26</td>
      <td>18:37:26</td>
      <td>01:54:48</td>
      <td>02:09:48</td>
      <td>04:18:59</td>
      <td>04:31:59</td>
    </tr>
    <tr>
      <th>15</th>
      <td>01:07:34</td>
      <td>01:11:34</td>
      <td>23:58:09</td>
      <td>00:03:09</td>
      <td>05:11:24</td>
      <td>05:14:24</td>
    </tr>
    <tr>
      <th>18</th>
      <td>21:55:29</td>
      <td>21:58:29</td>
      <td>07:52:06</td>
      <td>07:59:06</td>
      <td>05:16:42</td>
      <td>05:26:42</td>
    </tr>
    <tr>
      <th>26</th>
      <td>16:57:28</td>
      <td>17:02:28</td>
      <td>08:40:10</td>
      <td>08:54:10</td>
      <td>05:27:54</td>
      <td>05:35:54</td>
    </tr>
    <tr>
      <th>19</th>
      <td>09:38:08</td>
      <td>09:46:08</td>
      <td>20:29:04</td>
      <td>20:39:04</td>
      <td>05:29:26</td>
      <td>05:40:26</td>
    </tr>
    <tr>
      <th>10</th>
      <td>12:32:16</td>
      <td>12:45:16</td>
      <td>15:15:16</td>
      <td>15:30:16</td>
      <td>05:41:14</td>
      <td>05:52:14</td>
    </tr>
    <tr>
      <th>2</th>
      <td>03:28:10</td>
      <td>03:37:10</td>
      <td>16:28:54</td>
      <td>16:34:54</td>
      <td>05:45:50</td>
      <td>05:45:50</td>
    </tr>
    <tr>
      <th>5</th>
      <td>04:24:12</td>
      <td>04:27:12</td>
      <td>21:25:22</td>
      <td>21:35:22</td>
      <td>06:37:33</td>
      <td>06:51:33</td>
    </tr>
    <tr>
      <th>6</th>
      <td>04:33:30</td>
      <td>04:47:30</td>
      <td>02:30:53</td>
      <td>02:40:53</td>
      <td>08:01:21</td>
      <td>08:05:21</td>
    </tr>
    <tr>
      <th>24</th>
      <td>04:42:27</td>
      <td>04:47:27</td>
      <td>15:06:39</td>
      <td>15:20:39</td>
      <td>08:54:14</td>
      <td>09:06:14</td>
    </tr>
    <tr>
      <th>8</th>
      <td>13:44:59</td>
      <td>13:48:59</td>
      <td>09:16:28</td>
      <td>09:29:28</td>
      <td>09:26:00</td>
      <td>09:36:00</td>
    </tr>
    <tr>
      <th>16</th>
      <td>22:23:57</td>
      <td>22:30:57</td>
      <td>09:43:03</td>
      <td>09:50:03</td>
      <td>09:33:34</td>
      <td>09:38:34</td>
    </tr>
    <tr>
      <th>7</th>
      <td>22:50:12</td>
      <td>22:53:12</td>
      <td>13:24:58</td>
      <td>13:35:58</td>
      <td>10:29:12</td>
      <td>10:36:12</td>
    </tr>
    <tr>
      <th>9</th>
      <td>14:50:50</td>
      <td>14:52:50</td>
      <td>13:37:44</td>
      <td>13:40:44</td>
      <td>10:48:19</td>
      <td>10:52:19</td>
    </tr>
    <tr>
      <th>14</th>
      <td>07:32:06</td>
      <td>07:45:06</td>
      <td>12:40:02</td>
      <td>12:47:02</td>
      <td>11:38:40</td>
      <td>11:52:40</td>
    </tr>
    <tr>
      <th>17</th>
      <td>12:28:29</td>
      <td>12:39:29</td>
      <td>14:01:29</td>
      <td>14:06:29</td>
      <td>12:05:57</td>
      <td>12:11:57</td>
    </tr>
    <tr>
      <th>28</th>
      <td>10:04:45</td>
      <td>10:13:45</td>
      <td>18:51:15</td>
      <td>19:05:15</td>
      <td>13:04:02</td>
      <td>13:19:02</td>
    </tr>
    <tr>
      <th>20</th>
      <td>03:53:04</td>
      <td>04:06:04</td>
      <td>16:15:00</td>
      <td>16:23:00</td>
      <td>13:11:08</td>
      <td>13:23:08</td>
    </tr>
    <tr>
      <th>22</th>
      <td>18:11:42</td>
      <td>18:18:42</td>
      <td>16:30:01</td>
      <td>16:40:01</td>
      <td>13:57:02</td>
      <td>14:01:02</td>
    </tr>
    <tr>
      <th>3</th>
      <td>14:54:25</td>
      <td>14:56:25</td>
      <td>17:58:10</td>
      <td>18:07:10</td>
      <td>14:33:26</td>
      <td>14:48:26</td>
    </tr>
    <tr>
      <th>0</th>
      <td>05:16:21</td>
      <td>05:30:21</td>
      <td>18:53:43</td>
      <td>18:57:43</td>
      <td>16:43:04</td>
      <td>16:44:04</td>
    </tr>
    <tr>
      <th>1</th>
      <td>07:51:07</td>
      <td>08:05:07</td>
      <td>01:12:47</td>
      <td>01:19:47</td>
      <td>17:55:17</td>
      <td>17:59:17</td>
    </tr>
    <tr>
      <th>11</th>
      <td>10:54:34</td>
      <td>10:54:34</td>
      <td>16:26:16</td>
      <td>16:38:16</td>
      <td>20:19:28</td>
      <td>20:30:28</td>
    </tr>
    <tr>
      <th>13</th>
      <td>17:09:17</td>
      <td>17:19:17</td>
      <td>13:54:10</td>
      <td>13:59:10</td>
      <td>20:21:11</td>
      <td>20:36:11</td>
    </tr>
    <tr>
      <th>12</th>
      <td>03:16:24</td>
      <td>03:28:24</td>
      <td>13:05:14</td>
      <td>13:17:14</td>
      <td>22:33:44</td>
      <td>22:43:44</td>
    </tr>
    <tr>
      <th>21</th>
      <td>17:33:54</td>
      <td>17:48:54</td>
      <td>15:00:34</td>
      <td>15:06:34</td>
      <td>22:46:14</td>
      <td>22:51:14</td>
    </tr>
    <tr>
      <th>29</th>
      <td>19:26:42</td>
      <td>19:37:42</td>
      <td>20:57:19</td>
      <td>21:09:19</td>
      <td>23:06:30</td>
      <td>23:13:30</td>
    </tr>
    <tr>
      <th>25</th>
      <td>04:48:58</td>
      <td>04:52:58</td>
      <td>04:44:45</td>
      <td>04:47:45</td>
      <td>23:08:56</td>
      <td>23:18:56</td>
    </tr>
  </tbody>
</table>
</div>

```python
tempo_inicio_raio_x = df['tempo_inicio_raio_x']
```

```python
tempo_fim_raio_x = df['tempo_fim_raio_x']
```

```python
tempo_inicio_raio_x = pd.to_datetime(tempo_inicio_raio_x)
```

```python
tempo_fim_raio_x = pd.to_datetime(tempo_fim_raio_x)
```

```python
# Usando uma variável para armazenar o valores de Tempo Total Disponível (1ª tempo inicial - último tempo final)
# Com a subtração acima, é possível ter um valor de tempo disponível
```

```python
tempo_total_disponivel_raio_x = (tempo_fim_raio_x[4] - tempo_inicio_raio_x[27]) + pd.Timedelta("1 days")
```

```python
tempo_total_disponivel_raio_x
```

    Timedelta('0 days 21:39:49')

```python
tempo_duracao_raio_x = tempo_fim_raio_x - tempo_inicio_raio_x
```

```python
# Usando uma variável para armazenar Tempo Total Utilizado (somando todos os valores da sequência de tempo de duração)
# Com esta operação, é possível ter o valor final de tempo de atividade (produção)
```

```python
tempo_total_utilizado_raio_x = tempo_duracao_raio_x.sum()
```

```python
tempo_total_utilizado_raio_x
```

    Timedelta('0 days 04:06:00')

```python
# Usando uma variável para armazernar o Tempo Total de Paradas (subtraindo do Tempo Total Disponível o Tempo Total Utilizado)
```

```python
tempo_total_parada_raio_x = tempo_total_disponivel_raio_x - tempo_total_utilizado_raio_x
```

```python
tempo_total_parada_raio_x
```

    Timedelta('0 days 17:33:49')

```python
# Usando uma variável para armazenar as ocorrências de paradas (quantidade de registro -1, nos mostra todas as paradas da série)
```

```python
qtde_paradas_raio_x = tempo_duracao_raio_x.count() - 1
```

```python
qtde_paradas_raio_x
```

    29

```python
# Calculando o MTTF das Colunas de Raio X
```

```python
mttf_raio_x = tempo_duracao_raio_x.mean()
```

```python
mttf_raio_x
```

    Timedelta('0 days 00:08:12')

```python
# Calculando o MTTR das Colunas de Raio X
```

```python
mttr_raio_x = tempo_total_parada_raio_x / qtde_paradas_raio_x
```

```python
mttr_raio_x
```

    Timedelta('0 days 00:36:20.310344827')

```python
# Calculando o MTBF das Colunas de Raio X
```

```python
mtbf_raio_x = (tempo_total_disponivel_raio_x - tempo_total_parada_raio_x) / qtde_paradas_raio_x
```

```python
mtbf_raio_x
```

    Timedelta('0 days 00:08:28.965517241')

```python
# Calculando a Disponibilidade das Colunas de Raio X
```

```python
disponibilidade_raio_x = mtbf_raio_x / (mttr_raio_x + mtbf_raio_x)
```

```python
disponibilidade_raio_x
```

    0.18925745938522484

```python

```
