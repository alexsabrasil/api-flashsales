# API Flash Sales - Infraestrutura DevOps

Projeto desenvolvido para executar uma API de Flash Sales em ambiente containerizado, aplicando práticas de infraestrutura, automação e operação DevOps.

## Arquitetura

A solução utiliza:

- Node.js, TypeScript e Express para a API
- PostgreSQL e TypeORM para persistência
- Redis como armazenamento em memória
- Docker e Docker Compose para containerização e orquestração
- Jest para testes automatizados
- prom-client para métricas

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

## Testes e Build

Para validar a aplicação:

```bash
npm ci
npm test
npm run build
```

Os testes automatizados validam regras do serviço de checkout.

## Justificativa da Arquitetura

**Escalabilidade:** API, PostgreSQL e Redis são executados como serviços separados, permitindo que os componentes evoluam de forma independente.

**Segurança:** as credenciais são fornecidas por variáveis de ambiente e o arquivo `.env` não é versionado. A aplicação também executa no contêiner com usuário não-root.

**Confiabilidade:** PostgreSQL e Redis utilizam volumes persistentes. O PostgreSQL possui healthcheck e os serviços possuem política de reinicialização. Os testes automatizados validam regras da aplicação antes da execução.

## Encerrar o ambiente

```bash
docker compose down
```

---

## Atividade Prática do curso DevOps | FAP 2026

**Professor:** Bruno Álexys  
**Aluna/Treinanda:** Alexsandra Tavares# API Flash Sales - Infraestrutura DevOps

Projeto desenvolvido para executar uma API de Flash Sales em ambiente containerizado, aplicando práticas de infraestrutura, automação e operação DevOps.

## Arquitetura

A solução utiliza:

- Node.js, TypeScript e Express para a API
- PostgreSQL e TypeORM para persistência
- Redis como armazenamento em memória
- Docker e Docker Compose para containerização e orquestração
- Jest para testes automatizados
- prom-client para métricas

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

## Testes e Build

Para validar a aplicação:

```bash
npm ci
npm test
npm run build
```

Os testes automatizados validam regras do serviço de checkout.

## Justificativa da Arquitetura

**Escalabilidade:** API, PostgreSQL e Redis são executados como serviços separados, permitindo que os componentes evoluam de forma independente.

**Segurança:** as credenciais são fornecidas por variáveis de ambiente e o arquivo `.env` não é versionado. A aplicação também executa no contêiner com usuário não-root.

**Confiabilidade:** PostgreSQL e Redis utilizam volumes persistentes. O PostgreSQL possui healthcheck e os serviços possuem política de reinicialização. Os testes automatizados validam regras da aplicação antes da execução.

## Encerrar o ambiente

```bash
docker compose down
```

---

## Atividade Prática do curso DevOps | FAP 2026

**Professor:** Bruno Álexys  
**Aluna/Treinanda:** Alexsandra Tavares
