# Roteiro do vídeo — artfact

**Duração estimada:** 4min30s a 5min
**Formato:** gravação de tela da aplicação + falas dos integrantes
**Divisão sugerida:** 6 integrantes, com aproximadamente 35 a 50 segundos para cada um.

> Substituam “Pessoa 1”, “Pessoa 2” etc. pelos nomes dos integrantes. Todos devem aparecer ou participar com voz identificada.

### Divisão dos participantes

1. **Pessoa 1:** contexto e problema.
2. **Pessoa 2:** produto, solução e demonstração da interface.
3. **Pessoa 3:** pipeline de IA — entrada, preparação da busca e fontes.
4. **Pessoa 4:** pipeline de IA — recuperação, Gemini e explicabilidade.
5. **Pessoa 5:** tecnologias, execução com Docker e principais desafios.
6. **Pessoa 6:** conclusão e resultados.

---

## 0:00–0:40 — Contexto e problema

### Pessoa 1 — Contexto

**Fala:**

> “Todos os dias, informações sobre saúde circulam nas redes sociais, sites e aplicativos. O problema é que muitas dessas informações aparecem sem contexto, sem fonte ou baseadas em interpretações incompletas de artigos científicos.
>
> Além disso, mesmo quando uma pessoa encontra um artigo confiável, pode ser difícil entender o objetivo do estudo, seus resultados, suas limitações e se os dados realmente têm relação com determinada alegação.
>
> Foi pensando nesse problema que desenvolvemos o artfact, uma aplicação que ajuda o usuário a consultar a literatura científica e comparar alegações com evidências rastreáveis.”

### Tela

Mostrar:

- Página inicial;
- Campo de pesquisa;
- Frase da aplicação: “Evidências para conferir”;
- Fontes PubMed e PMC.

---

## 0:40–1:20 — Produto e solução

### Pessoa 2 — Apresentação da aplicação

**Fala:**

> “O artfact é uma aplicação web de apoio à leitura e verificação de alegações científicas.
>
> O usuário pode pesquisar um tema, informar um PMID, DOI ou link do PubMed, ou enviar um PDF ou imagem de um artigo.
>
> Na pesquisa temática, existem filtros para escolher o tipo de artigo, a disponibilidade do texto completo, a indexação no MEDLINE, o idioma e a quantidade de resultados.
>
> Depois da recuperação dos artigos, o usuário pode selecionar um estudo, visualizar o texto disponível e revisar as alegações extraídas antes de iniciar a investigação.
>
> Também é possível adicionar uma alegação própria, selecionar apenas as alegações relevantes e investigar cada uma separadamente.”

### Tela

Demonstrar:

1. Digitar um tema, por exemplo:
   - “exercício físico e diabetes tipo 2”.
2. Mostrar os filtros.
3. Exibir os resultados do PubMed.
4. Selecionar um artigo.
5. Mostrar a etapa “Alegações”.
6. Desmarcar algumas alegações e manter apenas uma selecionada.

---

# 1:20–3:35 — Pipeline de IA em formato de receita

## Pessoa 3 — Receita do pipeline: entrada e busca

**Fala:**

> “O pipeline de inteligência artificial pode ser explicado como uma receita em etapas.
>
> **Primeiro ingrediente: a entrada.** O sistema recebe um tema, uma alegação ou um artigo. A aplicação valida o texto, remove inconsistências e verifica se a referência informada é um PMID, DOI, link do PubMed ou arquivo aceito.
>
> **Segundo ingrediente: preparação da busca.** A alegação é transformada em consultas científicas. O sistema identifica termos importantes, conceitos e palavras relacionadas. Quando necessário, utiliza tradução português-inglês para melhorar a recuperação no PubMed. Se a tradução não estiver disponível, a consulta original continua sendo usada.
>
> **Terceiro ingrediente: busca bibliográfica.** A aplicação consulta o PubMed por meio das APIs E-utilities da NCBI. Os resultados trazem informações como título, autores, resumo, PMID, DOI, data e tipo de publicação.
>
> **Quarto ingrediente: confirmação da fonte.** O sistema confere a identidade do artigo e evita misturar registros duplicados ou inconsistentes. Quando o texto completo está disponível, a aplicação tenta recuperá-lo no PubMed Central, o PMC. Caso não exista texto completo, utiliza o resumo do PubMed e informa essa limitação ao usuário.
>
> **Quinto ingrediente: organização do texto.** O artigo é separado em seções e trechos menores, chamados de chunks. Cada trecho mantém sua origem, como artigo, seção, página, PMID ou PMCID. Assim, o resultado continua rastreável.”

### Tela

Mostrar:

- Requisição ou resultado do PubMed;
- Metadados de um artigo;
- Texto do artigo no leitor;
- Seções e trechos destacados;
- Indicação de “texto completo” ou “somente resumo”.

---

## Pessoa 4 — Receita do pipeline: recuperação, IA e explicação

**Fala:**

> “Depois de dividir o artigo, começa a recuperação das evidências.
>
> A primeira busca é lexical, usando correspondência de palavras e termos, com BM25. Ela encontra trechos que usam vocabulário semelhante ao da alegação.
>
> Quando o modo vetorial está habilitado, o sistema também transforma os trechos em embeddings, que são representações numéricas do significado do texto. Esses vetores podem ser usados em um índice em memória ou no Qdrant, um banco especializado em busca vetorial.
>
> A aplicação combina os resultados lexical e semântico com um ranking híbrido baseado em RRF. Dessa forma, um trecho pode ser encontrado tanto porque contém as mesmas palavras quanto porque possui significado semelhante.
>
> Os trechos recuperados são enviados ao Gemini como dados da fonte, e não como instruções. O Gemini classifica a relação entre a alegação e a evidência, por exemplo, como suporte, contradição, resultado inconclusivo ou conflito. Ele também identifica a citação, o desenho informado do estudo e uma justificativa.
>
> Depois disso, o sistema valida se a citação realmente existe no trecho recuperado. Se não houver evidência literal ou se a origem não puder ser confirmada, a resposta é tratada com incerteza.
>
> Por fim, os resultados são organizados por alegação, artigo e trecho, gerando uma resposta explicável, com fontes, limitações e indicação do que precisa ser conferido manualmente.”

### Tela

Mostrar:

- Resultado de uma alegação;
- Trechos de evidência;
- Relação entre alegação e estudo;
- Citações;
- Indicadores de cobertura;
- Seção de limitações;
- Resultado por alegação.

---

# 3:35–4:35 — Tecnologias e principais desafios

## Pessoa 5 — Tecnologias utilizadas

**Fala:**

> “A aplicação foi desenvolvida principalmente em Python, com uma API web baseada em Flask. A interface utiliza HTML, CSS e JavaScript.
>
> O PubMed é usado para descoberta dos artigos e metadados. O PubMed Central é usado para recuperar o texto completo quando ele está disponível.
>
> A NCBI também fornece consultas auxiliares ao Gene e ao ClinVar para casos biomédicos específicos. Esses registros são apresentados como informação estruturada auxiliar e não geram sozinhos um veredito.
>
> O Gemini é usado na organização de consultas, extração de alegações, classificação textual das evidências e geração de resumos. A aplicação não permite que o modelo invente uma fonte: os resultados são relacionados a trechos recuperados.
>
> Para recuperação de informação, são utilizados BM25, embeddings, ranking híbrido e, opcionalmente, Qdrant para armazenamento vetorial.
>
> Os dados locais, análises, artigos e anotações são armazenados em SQLite. A execução pode ser reproduzida com Docker, usando o Dockerfile e o Docker Compose do projeto.”

---

### Pessoa 5 — Principais desafios

**Fala:**

> “Os principais desafios foram garantir que a inteligência artificial não apresentasse uma conclusão sem fonte, lidar com artigos que possuem apenas resumo, evitar duplicidade de estudos e preservar a origem de cada trecho.
>
> Outro desafio foi permitir que o usuário tivesse controle sobre o processo. Por isso, o artfact não investiga automaticamente todas as alegações sem revisão: o usuário pode editar, adicionar, selecionar ou remover alegações antes da busca.
>
> Também foi necessário tratar falhas de rede, indisponibilidade do Gemini, limites das APIs e diferenças entre artigos em português e inglês.”

---

# 4:35–5:00 — Conclusão e resultados

## Pessoa 6 — Encerramento

**Fala:**

> “Como resultado, o artfact transforma uma alegação ou artigo em um fluxo rastreável de pesquisa científica.
>
> A aplicação não promete dizer automaticamente se algo é verdadeiro ou falso. Ela mostra quais estudos foram encontrados, quais trechos foram usados, qual relação textual foi identificada e quais são as limitações da análise.
>
> Dessa forma, o sistema combina inteligência artificial, recuperação de informação e revisão humana para tornar a leitura científica mais clara, transparente e verificável.
>
> Esse é o artfact: evidências para conferir.”

---

# Receita técnica resumida para aparecer no vídeo

Podem colocar este slide durante a explicação da IA:

```text
1. Receber tema, alegação ou artigo
2. Validar e normalizar a entrada
3. Preparar termos e consultas
4. Consultar PubMed/NCBI
5. Confirmar metadados e identidade
6. Buscar texto completo no PMC
7. Usar resumo do PubMed como fallback
8. Dividir o texto em chunks
9. Recuperar por BM25
10. Gerar embeddings, quando habilitado
11. Buscar semanticamente no índice/Qdrant
12. Combinar rankings com RRF
13. Selecionar trechos rastreáveis
14. Enviar evidências ao Gemini
15. Classificar suporte, contradição ou incerteza
16. Validar citações e proveniência
17. Sintetizar por alegação
18. Mostrar resultado, fontes e limitações
```

---

# Demonstração recomendada na gravação

Para o vídeo ficar objetivo, usem este exemplo:

1. Pesquisar: **“exercício físico e diabetes tipo 2”**.
2. Mostrar os filtros e explicar rapidamente cada um.
3. Exibir os artigos recuperados.
4. Selecionar um artigo.
5. Mostrar o texto e as alegações extraídas.
6. Desmarcar todas as alegações.
7. Selecionar apenas uma.
8. Clicar em **“Investigar selecionadas”**.
9. Mostrar:
   - alegação analisada;
   - artigos recuperados;
   - trechos de evidência;
   - relação textual;
   - limitações;
   - fontes para conferência manual.

---

# Observação importante para não falar algo incorreto

No vídeo, apresentem como funcionalidades do **MVP atual**:

- PubMed como fonte principal de descoberta;
- PMC como fonte de texto completo;
- Gemini para classificação textual e síntese;
- BM25 e embeddings para recuperação;
- Qdrant como opção de armazenamento vetorial;
- SQLite para persistência local;
- Docker para execução reproduzível.

Evitem dizer que o sistema atual usa OpenAlex, SciELO, Crossref, Europe PMC ou AMSTAR 2 em tempo de execução, porque essas integrações aparecem no histórico do projeto, mas não fazem parte do runtime principal desta versão PubMed/PMC.
