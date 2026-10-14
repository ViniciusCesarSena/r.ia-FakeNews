# Deploy no Azure for Students

O `Dockerfile` da raiz gera o frontend React e executa a API Flask no mesmo
container. O navegador acessa `/api/v1` no mesmo domínio, sem CORS ou um segundo
serviço.

## Configuração recomendada

- Azure Container Apps, plano Consumption
- 1 vCPU e 2 GiB de memória
- mínimo de 0 e máximo de 1 réplica
- ingresso externo na porta exposta pelo container
- healthcheck: `/api/v1/health`

## Variáveis e segredos

Configure `GEMINI_API_KEY` como segredo. As demais variáveis recomendadas são:

```dotenv
NCBI_EMAIL=seu_email
LLM_MODEL=gemini-flash-lite-latest
WATCH_INTERVAL_HOURS=0
VECTOR_STORE_BACKEND=memory
HYBRID_RETRIEVAL_ENABLED=false
```

O armazenamento local do plano Consumption é temporário. O histórico resumido
do usuário continua no Firestore; análises em andamento podem ser perdidas se a
réplica for encerrada ou substituída.
