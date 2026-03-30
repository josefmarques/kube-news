# kube-news

Aplicação de exemplo para demonstrar o uso de containers com Node.js, PostgreSQL e Kubernetes.

## Stack

| Tecnologia | Descrição |
|------------|-----------|
| Node.js | Runtime JavaScript |
| Express.js | Framework web |
| Sequelize | ORM para PostgreSQL |
| EJS | Template engine |
| PostgreSQL | Banco de dados |
| Prometheus | Métricas |
| Docker/Kubernetes | Containerização |

## Variáveis de Ambiente

| Variável | Descrição | Padrão |
|----------|-----------|--------|
| `NODE_ENV` | Ambiente (production/development) | production |
| `PORT` | Porta da aplicação | 8080 |
| `DB_HOST` | Host do PostgreSQL | localhost |
| `DB_DATABASE` | Nome do banco | kubedevnews |
| `DB_USERNAME` | Usuário do banco | kubedevnews |
| `DB_PASSWORD` | Senha do banco | Pg#123 |

## Docker Compose (Desenvolvimento)

```bash
# Subir ambiente completo
docker-compose up --build

# Parar serviços
docker-compose down

# Parar e remover volumes
docker-compose down -v
```

**Serviços disponíveis:**
- App: http://localhost:3000
- PostgreSQL: localhost:5432

## Kubernetes

### Pré-requisitos

- Kubernetes 1.25+
- kubectl configurado

### Deploy

```bash
# Criar namespace e recursos
kubectl apply -f k8s/namespace.yaml
kubectl apply -f k8s/configmap.yaml
kubectl apply -f k8s/secret.yaml
kubectl apply -f k8s/rbac.yaml
kubectl apply -f k8s/deployment.yaml

# Verificar status
kubectl get pods -n kube-news
kubectl get svc -n kube-news
```

### Estrutura dos manifests

```
k8s/
├── namespace.yaml   # Namespace do projeto
├── configmap.yaml   # Configurações não-sensíveis
├── secret.yaml      # Credenciais (base64)
├── rbac.yaml        # Permissões ServiceAccount
└── deployment.yaml  # Deployment, Service e PDB
```

## Endpoints

| Endpoint | Método | Descrição |
|----------|--------|-----------|
| `/` | GET | Página inicial com lista de posts |
| `/post` | GET | Formulário de criação |
| `/post` | POST | Criar novo post |
| `/post/:id` | GET | Visualizar post |
| `/api/post` | POST | API para批量 criar posts |
| `/health` | GET | Liveness probe |
| `/ready` | GET | Readiness probe |
| `/metrics` | GET | Métricas Prometheus |

## Estrutura do Projeto

```
kube-news/
├── src/
│   ├── server.js          # Entry point
│   ├── middleware.js      # Middlewares Express
│   ├── system-life.js     # Health/Readiness
│   ├── models/
│   │   └── post.js        # Modelo Sequelize
│   ├── views/             # Templates EJS
│   │   ├── index.ejs
│   │   ├── edit-news.ejs
│   │   ├── view-news.ejs
│   │   └── partial/
│   ├── static/            # Assets estáticos
│   │   ├── styles/
│   │   └── img/
│   ├── Dockerfile         # Build produção
│   ├── Dockerfile.dev     # Build desenvolvimento
│   └── package.json
├── k8s/                   # Manifests Kubernetes
├── docker-compose.yaml    # Ambiente local
└── README.md
```
