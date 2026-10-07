# API Flash Sales - Infraestrutura DevOps

Projeto desenvolvido para executar uma API de Flash Sales em ambiente containerizado, aplicando práticas de infraestrutura, automação e operação DevOps.

## Arquitetura

A solução utiliza:

- Node.js, TypeScript e Express para a API
- PostgreSQL e TypeORM para persistência
- Redis para controle de disponibilidade de ingressos
- Docker e Docker Compose para containerização e gerenciamento dos serviços
- Jest para testes automatizados
- GitHub Actions para Integração Contínua (CI)
- prom-client para exposição de métricas

```text
Cliente
   |
API Node.js
   |
   +-- PostgreSQL
   +-- Redis
   +-- /metrics
```

## Como executar

Clone o repositório:

```bash
git clone https://github.com/alexsabrasil/api-flashsales.git
cd api-flashsales
```

Crie o arquivo de ambiente:

```bash
cp .env.example .env
```

Defina uma senha em `POSTGRES_PASSWORD` no arquivo `.env`.

Suba a infraestrutura:

```bash
docker compose up -d --build
```

Verifique os serviços:

```bash
docker compose ps
```

A API estará disponível em `http://localhost:3000`.

## Como testar

Métricas da aplicação:

```bash
curl http://localhost:3000/metrics
```

Testes automatizados e build:

```bash
npm ci
npm test
npm run build
```

## Automação

O workflow `.github/workflows/ci.yml` é executado em pushes e pull requests para a branch `main`.

A pipeline realiza:

```text
npm ci -> npm test -> npm run build -> Docker build
```

## Justificativa da Arquitetura

**Escalabilidade:** API, PostgreSQL e Redis são serviços separados, permitindo a evolução independente dos componentes. O Redis auxilia no controle de disponibilidade durante o processamento de checkout.

**Segurança:** as credenciais são fornecidas por variáveis de ambiente. O arquivo `.env` não é versionado e a aplicação executa no contêiner com usuário não-root.

**Confiabilidade:** PostgreSQL e Redis utilizam volumes persistentes, o PostgreSQL possui healthcheck e os serviços possuem política de reinicialização. Testes automatizados e CI validam a aplicação e a construção da imagem Docker.

## Encerrar o ambiente

```bash
docker compose down
```

---

## Atividade Prática do curso DevOps | FAP 2026

**Professor:** Bruno Álexys  
**Aluna/Treinanda:** Alexsandra Tavares