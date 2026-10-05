# ⚡ Flash Sales API — Infraestrutura e Operações DevOps

Microsserviço de alta performance voltado para cenários de **Vendas Relâmpago (Flash Sales)**, processamento de checkout concorrente, gestão de pedidos e exportação assíncrona de relatórios. Projetado para suportar picos de tráfego com escalabilidade, baixa latência e conformidade rigorosa com práticas de DevSecOps.

---

## 🛠️ Tecnologias Utilizadas

### Core & Aplicação
* **Node.js & TypeScript**: Ambiente de execução assíncrono com checagem estática de tipos, evitando inconsistências de tipagem em fluxos críticos de checkout e transações.
* **Express.js**: Framework para construção de APIs RESTful e roteamento modular (`CheckoutController`, `ExportController`).
* **Jest**: Suíte de testes automatizados para validação de regras de negócio, cálculos transacionais e fluxo de pedidos.

### Persistência, Cache & Concorrência
* **PostgreSQL / TypeORM**: Banco de dados relacional para persistência transacional de pedidos (`Order`), com suporte a isolamento de transações e consistência contábil.
* **Redis**: Camada de armazenamento chave-valor em memória utilizada para:
  - Controle de concorrência e trava distribuída (*distributed locking*) em itens com estoque disputado.
  - Caching de dados quentes e fila/estado de exportações em segundo plano.

### Observabilidade & Confiabilidade
* **Prometheus Client (`prom-client`)**: Instrumentação de métricas expostas em `/metrics` via middleware customizado, permitindo medir latência de checkout (p95/p99), volume de pedidos por segundo e taxa de requisições com falha.

### Conteinerização, Orquestração & DevSecOps
* **Docker Multi-Stage Build**: Construção otimizada utilizando base Linux mínima (`Alpine`), separando dependências de build da imagem final de produção.
* **Execução Não-Root (`USER node`)**: Aderência ao Princípio do Menor Privilégio dentro do contêiner.
* **Docker Compose**: Orquestração local para ambiente integrado com isolamento de redes e healthchecks.
* **Gestão de Segredos via Variáveis de Ambiente**: Nenhuma credencial ou URL sensível hardcoded no código ou nos manifestos.

---

## 🏗️ Justificativa de Arquitetura

O padrão arquitetural foi desenhado para mitigar os gargalos clássicos de cenários de *flash sales*:

1. **Resiliência a Picos de Concorrência**:
   - O uso intensivo de **Redis** desacopla a verificação de alta frequência do banco de dados relacional, prevenindo *deadlocks* e contenção de conexões no PostgreSQL durante picos de compras.
2. **Processamento e Exportação Desacoplados**:
   - A separação do `CheckoutController` e `ExportController` isola operações críticas de vendas daquelas de processamento e extração de dados pesados, impedindo que requisições analíticas afetem o throughput de vendas.
3. **Observabilidade e Métricas Orientadas a SRE**:
   - Com o endpoint `/metrics` ativo, métricas de taxa de erros HTTP e tempo de resposta alimentam painéis do Grafana e alertas do Prometheus para autoescalonamento (HPA) e resposta rápida a incidentes.
4. **Segurança de Contêineres (DevSecOps - Unidade 10)**:
   - **Superfície de Ataque Reduzida**: Imagens base mínimas (`node:alpine`) minimizam a presença de pacotes vulneráveis comuns (como utilitários de shell extras).
   - **Princípio do Menor Privilégio (PoLP)**: O contêiner nunca roda como `root`, mitigando riscos de escape de contêiner.
   - **Isolamento de Segredos**: Credenciais de banco, portas e chaves de cache são injetadas estritamente via runtime environment ou secret managers.

---

## 📋 Pré-requisitos

* [Git](https://git-scm.com/)
* [Node.js 18+](https://nodejs.org/) (para execução local direta)
* [Docker](https://www.docker.com/) e [Docker Compose](https://docs.docker.com/compose/)

---

## ⚙️ Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto (nunca comitado no repositório) baseado no modelo:

```env
PORT=3000
NODE_ENV=development

# Banco de Dados PostgreSQL
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=flashsales_db

# Redis
REDIS_HOST=localhost
REDIS_PORT=6379

---

## Como executar o Projeto

1. Execução via Docker Compose (Recomendado)
Sobe toda a stack (API + PostgreSQL + Redis):

# Construir imagem e subir contêineres em segundo plano
docker compose up -d --build

# Acompanhar logs da API
docker compose logs -f api

A API estará acessível em: http://localhost:3000

2. Execução Local para Desenvolvimento (Sem Docker)

# Instalar dependências
npm install

# Iniciar em modo de desenvolvimento com hot-reload
npm run dev

## Testes Automatizados

O repositório conta com testes cobrindo as regras de negócio de checkout e processamento de pedidos:

# Rodar todos os testes
npm test

# Rodar testes com relatório de cobertura
npm test -- --coverage

## Endpoints Principais

Método	Rota	Descrição
GET	/health	Checagem de disponibilidade da aplicação
GET	/metrics	Métricas operacionais em formato Prometheus
POST	/checkout	Processamento de checkout e criação de pedido (Order)
GET	/checkout/:id	Consulta o status de um pedido
POST	/export	Dispara a rotina de exportação assíncrona de pedidos

## Políticas de Segurança e DevSecOps

- Usuário Não-Root: Configuração de segurança USER node no Dockerfile.
- Scan Contínuo de Vulnerabilidades: Compatível com scanners estáticos em CI/CD (Grype, Trivy, Snyk) para análise de dependências e camadas Docker.
- Zero Hardcoded Secrets: Segredos geridos fora do controle de versão via .gitignore.

---

### Como salvar e subir no repositório `api-flashsales`:

No terminal, dentro da pasta **`C:\Users\Aluno\api-flashsales`**:

1. Crie ou cole o conteúdo no arquivo `README.md`.
2. Execute os comandos para versionar e enviar para o GitHub:

```powershell
git add README.md
git commit -m "docs: add comprehensive DevOps README for Flash Sales API"
git push origin main```

## Atividade no treinamento DevOps | FAP 2026

Profesor: Bruno Álexys
Treinanda: Alexsandra tavares