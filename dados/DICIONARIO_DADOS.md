# Dicionário de dados

As aulas leem estes arquivos direto do GitHub, com uma linha:

```python
URL_DADOS = "https://raw.githubusercontent.com/MarlonRF/oficina-conexao-ccsro/main/dados/"
pd.read_csv(URL_DADOS + "reais/dados_santa_maria.csv")
```

## Origem

Os dados vêm da pesquisa de campo de João Paulo Assis Gobo numa praça de Santa Maria (RS): três campanhas (agosto de 2015, janeiro de 2016 e julho de 2016), nove dias, uma estação meteorológica e um questionário aplicado aos usuários da praça.

- Tese: GOBO, J. P. A. *Bioclimatologia subtropical e modelização do conforto humano*. Tese (Doutorado), FFLCH, Universidade de São Paulo, 2018. DOI: [10.11606/T.8.2018.tde-23022018-094537](https://doi.org/10.11606/T.8.2018.tde-23022018-094537).
- Base publicada: [Zenodo, registro 15412056](https://zenodo.org/records/15412056).

Ao usar os dados, cite a tese e o registro no Zenodo.

## `reais/dados_santa_maria.csv`

As 1.720 entrevistas de Santa Maria, extraídas da primeira aba (`dados_brutos`) de `dados_brutos_oficina.xlsx`, linhas físicas 2–1721, na ordem original. São 288 instantes entre 05/08/2015 e 08/07/2016. **Uma linha é uma entrevista.** As entrevistas de um mesmo instante compartilham as condições ambientais (283 dos 288 instantes têm seis entrevistas). A extração preserva os valores e vazios da planilha.

| Coluna | Significado | Unidade ou formato |
|---|---|---|
| `linha_origem` | Linha física da planilha. Serve para rastrear o registro. | Inteiro, sem unidade física |
| `instante` | Data e hora registradas (`data_cplt`). | `AAAA-MM-DD HH:MM:SS`, sem fuso atribuído |
| `idade` | Idade registrada. | Anos |
| `sexo` | Código registrado. Caixa original preservada (`f`, `F`, `m`). | Categoria, sem unidade física |
| `peso` | Massa registrada. O registro de 26 kg permanece na base e merece investigação contextual. | kg |
| `altura` | Altura registrada. | cm |
| `vestimenta` | Isolamento da vestimenta. Há três vazios. | clo |
| `temperatura` | Temperatura do ar (`tmedia`). | °C |
| `umidade` | Umidade relativa do ar (`ur`). | % (70 % se escreve `70`) |
| `radiacao` | Radiação solar média (`rsolarmed`). Há 216 vazios. | W/m² |
| `vento` | Velocidade do vento (`vel_vento`). | m/s |
| `tu` | Temperatura declarada de bulbo úmido, com procedência incerta. Há 431 vazios. | °C |
| `p1` | Sensação térmica. −3 muito frio, −2 frio, −1 um pouco de frio, 0 nem frio nem calor, +1 um pouco de calor, +2 calor, +3 muito calor. | Categoria ordenada, sem unidade física |
| `p5` | Preferência de temperatura. −1 mais baixa, 0 como está, +1 mais alta, `x` não sei. Os códigos observados `99` e `-3` estão fora das opções. | Código textual, sem unidade física |

A linha de origem não identifica uma pessoa. P1 = 0 é uma resposta de sensação, com significado próprio no questionário. Os valores de Tu têm procedência incerta.

## Arquivos didáticos

Gerados a partir da base acima por scripts em [`geracao/`](../geracao/). As colunas com o mesmo nome conservam significado e unidade.

| Arquivo | Gerado por | Uma linha é | Colunas | Usado em |
|---|---|---|---|---|
| `didaticos/instantes_santa_maria.csv` | [`criar_instantes.py`](../geracao/criar_instantes.py) | um **instante** de medição: 288 linhas, uma por instante, nas três campanhas | `instante`, `temperatura` (°C), `umidade` (%), `radiacao` (W/m², 36 instantes vazios, todos de julho de 2016), `vento` (m/s) | Intermediário, aulas 01–18 |
| `didaticos/reta_amostra.csv` | [`criar_instantes.py`](../geracao/criar_instantes.py) | um instante de 05/08/2015: seis linhas, a cada 1h30, das 09:10 às 16:40 | `instante`, `temperatura` (°C), `umidade` (%) | Intermediário, aulas 05, 07, 11, 14 e 15 |
| `didaticos/lacunas.csv` | [`criar_lacunas.py`](../geracao/criar_lacunas.py) | um instante de 05/08/2015: 48 linhas, a cada 10 min, das 09:10 às 17:00 | `instante`; `temperatura` (°C) com **12 valores escondidos** (vazios); `temperatura_real` (°C), a medição completa, que serve de gabarito | Intermediário, aula 13 |

**Sobre `lacunas.csv`.** Os 12 valores escondidos são medições reais, apagadas de propósito para medir o erro de cada método de preenchimento: um bloco de quatro instantes seguidos (12:40 a 13:10) e oito instantes isolados (10:30, 11:20, 12:20, 14:10, 14:50, 15:20, 16:10 e 16:30), sorteados com semente fixa (2015). A coluna `temperatura_real` é igual à `temperatura` de `instantes_santa_maria.csv` no mesmo instante. Numa pesquisa de verdade não existe gabarito; aqui ele existe para você poder conferir.

Use a base de **instantes** quando a pergunta for sobre o ambiente (temperatura, umidade, radiação): na base de entrevistas, cada medição aparece repetida uma vez por entrevistado. Use a base de **entrevistas** quando a pergunta envolver as respostas das pessoas (`p1`, `p5`).
