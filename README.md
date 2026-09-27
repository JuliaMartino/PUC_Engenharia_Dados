# PUC_Engenharia_Dados
Trabalho para a sprint Engenharia de Dados da pós-graduação em Ciência de Dados da PUC Rio.

# Música em Contexto: MusicBrainz + Spotify
Breve descrição do trabalho

## Contexto de Negócios e Perguntas
perguntas de negócio que foram formuladas, explicação do contexto dos dados brutos e resumo da estrutura desses dados brutos (colunas e tabelas). Explique sobre a licença dos dados.
Perguntas que me motivaram a fazer esse estudo:
- Será que existem gêneros de música que se relacionam com ...
- Será que as características sonoras das músicas etc
- Mais uma pergunta

## Carga dos Dados
Explicação da carga de dados, como foi feita e referência ao script no GitHub (se aplicável).

Para esse estudo utilizei três dataframes:


### MusicBrainz Canonical Data
https://musicbrainz.org/doc/Canonical_MusicBrainz_data

| Campo | Informação |
|---|---|
| Fonte | MusicBrainz Canonical Data |
| Autor | MusicBrainz / MetaBrainz Foundation |
| Licença | CC0 1.0 |
| Formato | CSV |
| Finalidade | Correspondência entre dados do Spotify e entidades do MusicBrainz |
| Campos utilizados | `artist_mbids`, `artist_credit_name`, `release_mbid`, `release_name`, `recording_mbid`, `recording_name`, `combined_lookup`, `score` |

### MusicBrainz Artist Data
https://musicbrainz.org/doc/Development/JSON_Data_Dumps

| Campo | Informação |
|---|---|
| Fonte | MusicBrainz Artist Data |
| Autor | MusicBrainz / MetaBrainz Foundation |
| Licença | CC0 1.0 |
| Formato | JSON |
| Finalidade | Associar uma localização a cada artista da base de dados. |
| Campos utilizados | `id`, `name`, `country` |

### Spotify Huge Audio Features
https://huggingface.co/datasets/GildasLeDrogoff/spotify-huge-track-analysis-dataset

| Campo | Informação |
|---|---|
| Fonte | Spotify Huge Track Analysis Dataset |
| Autor | Gildas Le Drogoff |
| Origem | Hugging Face |
| Licença | Creative Commons Attribution-NonCommercial 4.0 International (CC BY-NC 4.0) |
| Formato | Parquet |
| Finalidade | Relacionar músicas a suas características sonoras, gênero e popularidade atual |
| Dados utilizados no projeto | Dados de faixas, gênero, popularidade e características acústicas |

## Modelagem e Catálogo de Dados
Explicação da modelagem com a estrutura das tabelas (catálogo de dados transcrito e screenshots do sistema de catálogo).

Estrutura das tabelas que fazem parte do Pipeline:

| Camada | Tabela            | Função                                  |
| ------ | ----------------- | --------------------------------------- |
| Bronze | `spotify`         | Dados musicais originais                |
| Bronze | `musicbrainz`     | Dados de correspondência do MusicBrainz |
| Bronze | `mb_artist`       | Dados dos artistas utilizados           |
| Silver | `mb_spotify`      | Correspondência Spotify–MusicBrainz     |
| Silver | `contexto_silver` | Dados musicais + contexto dos artistas  |
| Gold   | `contexto_gold`   | Dataset final para análise              |

### Catálogo de Dados

Fazer uma tabela dessas para cada tabela:

**Bronze** — spotify

| Campo         | Tipo    | Descrição             |
| ------------- | ------- | --------------------- |
| `track_name`  | string  | Nome da faixa         |
| `artist_name` | string  | Nome do artista       |
| `track_genre` | string  | Gênero musical        |
| `popularity`  | integer | Popularidade da faixa |
| ...           | ...     | ...                   |

DEPOIS INCLUIR SCREENSHOTS DO CATÁLOGO DE CADA CAMADA!!!!!

## Pipeline de Dados
O processo foi dividido nos notebooks:

**Staging**, onde foram baixados os dados brutos e feita a primeira análise exploratória dos dados. Aqui também foram tomadas as primeiras decisões do projeto: quais datasets deveriam ser salvos em tabelas para serem usados no estudo e quais datasets não seriam úteis.

**Bronze**, os datasets úteis foram salvos em tabelas e comentados. Descobri que o JSON **artist** de MusicBrainz continha vários campos que precisariam de transformação para serem salvos. Decidi salvar a tabela **mb_artist** apenas com os campos que seriam usados no estudo, já mapeados na análise prévia em Staging.

INCLUIR OS SCREENSHOTS!!!!!!!! Adicione referência aos scripts disponibilizados no Github e screenshots que evidencie que essas tabelas foram salvas (persistidas) na plataforma de nuvem utilizada.

**Silver**, aqui foi feita a normalização dos nomes de artistas e músicas para criar uma correspondência entre as tabelas **musicbrainz** e **spotify**. A partir dessa nova coluna criada foi feita a join, criando uma nova tabela, **mb_spotify**. Nesse join foram selecionadas as instâncias para as quais foi possível a conrrespondência. Isso foi feito sem perda de dados, pois as tabelas originais, **musicbrainz** e **spotify** foram preservadas na camada **bronze**. Depois foi feito um novo join, com a tabela **artist** que contém as informações de países relacionados aos artistas. Esse join foi feito usando as chaves **id** e **artist_mbids**, que são correspondentes, criando assim uma nova tabela, a **contexto_silver**.

**Gold**, nesse notebook a tabela **contexto_silver** foi limpa de todas as colunas desnecessárias para o estudo crinado assim a tabela **contexto_gold**. Além disso, mais algumas outras análises foram feitas, comparando a tabela final com as tabelas origem.

**Análise**, aqui foi feito o estudo da tabela **contexto_gold** visando responder às perguntas iniciais.

## Qualidade de Dados
Quais problemas foram detectados e como resolveu cada um deles, que transformações foram feitas.

## Análise de Dados
Análise feita e respondendo as perguntas elaboradas na etapa 4.1.

## Autoavaliação
Ao finalizar o trabalho, é esperado que o aluno faça uma autoavaliação contendo uma discussão sobre se conseguiu atingir os objetivos delineados antes do início das outras etapas, suas dificuldades encontradas na execução do trabalho, bem como trabalhos futuros para enriquecer o problema e sua solução em seu portfólio.
