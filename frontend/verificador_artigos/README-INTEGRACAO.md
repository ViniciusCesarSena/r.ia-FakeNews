# ArtFact — integração frontend/backend

A partir desta branch, o novo frontend React foi trazido para este repositório
sem remover a interface web anterior:

```text
 r.ia-FakeNews/
 ├── src/fatofake/              # API Flask e frontend web legado
 └── frontend/verificador_artigos/
     ├── src/                   # React + TypeScript, login Firebase e indicadores
     ├── tests/                 # testes frontend
     ├── vite.config.ts         # proxy /api -> backend local
     └── package.json
```

O frontend antigo permanece em `src/fatofake/web_ui.py` e
`src/fatofake/static/`. Ele deve continuar sendo usado como referência para
adequar os dados exibidos pelo novo frontend. O backend deste repositório é o
mesmo utilizado pelo novo app; não é necessário clonar outro repositório ou
extrair um ZIP `frontend`.

## Iniciar
No PowerShell, em um terminal:

```powershell
cd C:\artfact2\r.ia-FakeNews
# Use o ambiente Python 3.11 que já tiver instalado:
.\.venv\Scripts\python.exe run_acceptance_app.py
```

Em outro terminal, na pasta do novo frontend:

```powershell
cd C:\artfact2\r.ia-FakeNews\frontend\verificador_artigos
npm ci
npm run dev
```

Navegue até http://localhost:8443. Verifique antes se http://127.0.0.1:5000/api/v1/health responde.

Crie um `.env` local para o backend a partir de `.env.example` apenas se ainda não possuir um. Configure `GEMINI_API_KEY` e demais serviços necessários. **Nunca compartilhe ou publique este arquivo, senhas, tokens ou chaves de contas de serviço.**

## Contrato com a API Flask
| Ação | Rota |
| --- | --- |
| Iniciar análise de link, DOI ou arquivo | POST `/api/v1/article-analyses` |
| Consultar progresso | GET `/api/v1/analyses/{id}?view=status` |
| Obter alegações e resultados | GET `/api/v1/analyses/{id}` |
| Selecionar alegações | POST `/api/v1/article-analyses/{id}/claims` |
| Relatório Markdown | GET `/api/v1/analyses/{id}/report.md` |

O usuário seleciona até 10 alegações. O frontend não simula o resultado; utiliza os status enviados pelo backend: QUEUED, RUNNING, AWAITING_CLAIM_SELECTION, RESEARCHING, SUCCEEDED, FAILED.

## Indicadores científicos
Cobertura, quantidade de artigos, trechos de evidência e triagem metodológica são lidos de `user_summary` e `verification`. Não significam “X% verdadeiro”; indicadores sem dados suficientes permanecem `N/D`.

## Login, Firestore e segurança
Autenticação Google e Email/Senha (Firebase Authentication). Habilite os provedores e os domínios no console Firebase. O histórico dos artigos é resumido e salvo no Firestore por UID, exigindo que as regras de `firestore.rules` sejam aplicadas manualmente. Teste isolamento com duas contas. O Flask continua sem validação de ID token; não use seus endpoints como armazenamento privado até que a autorização no backend seja implementada.

## Testes
```powershell
npm test
npm run typecheck
npm run build
```

O GitHub Actions também executa estes passos. Não substitui testes de artigos reais com PubMed/Gemini nem a revisão científica humana.
