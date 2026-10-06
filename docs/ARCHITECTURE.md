# 🏗️ Arquitetura do Kiki.entrega

## Visão Geral

Kiki.entrega segue uma arquitetura em camadas com separação clara entre frontend, backend e infraestrutura.

## Camadas da Aplicação

### 1. **Presentation Layer (Frontend)**
- React.js com TypeScript
- Redux/Context API para state management
- Material-UI para componentes
- Mapas interativos com Google Maps

### 2. **API Layer (Backend)**
- Express.js com Node.js
- Controladores para roteamento
- Middleware de autenticação e validação
- Socket.io para updates em tempo real

### 3. **Business Logic Layer**
- Services para cada domínio
- Validações de negócio
- Orquestração de processos
- Integração com APIs externas

### 4. **Data Access Layer**
- ORM (Prisma ou TypeORM)
- Repositórios para abstrair banco de dados
- Migrations para versionamento do schema
- Query builders otimizados

### 5. **Persistence Layer**
- PostgreSQL para dados transacionais
- Redis para cache e filas
- Índices para performance

## Fluxo de uma Requisição

```
1. Cliente faz requisição HTTP
   ↓
2. Express.js recebe e passa por middleware
   ↓
3. Router encaminha para o Controlador
   ↓
4. Controlador valida entrada e chama Service
   ↓
5. Service executa lógica de negócio
   ↓
6. Repository acessa o banco de dados
   ↓
7. Dados retornam pela cadeia até o Cliente
```

## Estrutura de Diretórios

```
backend/
├── src/
│   ├── controllers/     # Manipuladores de requisições HTTP
│   ├── services/        # Lógica de negócio
│   ├── repositories/    # Acesso a dados
│   ├── models/          # Entidades e DTOs
│   ├── middleware/      # Auth, validação, logging
│   ├── routes/          # Definição de rotas
│   ├── config/          # Configurações
│   ├── utils/           # Funções utilitárias
│   └── index.ts         # Entry point
├── migrations/          # Versionamento do BD
├── tests/               # Testes unitários
└── package.json
```

## Padrões de Design

### Service Pattern
Cada domínio (pedidos, entregas, rotas) tem um Service que centraliza a lógica:

```typescript
class OrderService {
  async createOrder(data: CreateOrderDTO): Promise<Order> {
    // Validações
    // Lógica de negócio
    // Persistência
    // Notificações
  }
}
```

### Repository Pattern
Abstração do acesso a dados:

```typescript
class OrderRepository {
  async findById(id: string): Promise<Order | null>
  async save(order: Order): Promise<void>
  async findByStatus(status: string): Promise<Order[]>
}
```

### Middleware Pattern
Processamento de requisições:

```typescript
app.use(authMiddleware)
app.use(validationMiddleware)
app.use(errorHandlerMiddleware)
```

## Segurança

- **Autenticação:** JWT com refresh tokens
- **Autorização:** Role-based access control (RBAC)
- **Validação:** Joi/Zod para schemas
- **Rate Limiting:** Por IP e por usuário
- **CORS:** Restrito a domínios conhecidos
- **Secrets:** Gerenciados via variáveis de ambiente

## Performance

- **Índices de BD:** Em campos de busca frequente
- **Cache:** Redis para dados que mudam pouco
- **Queries Otimizadas:** N+1 query prevention
- **Paginação:** Em listagens
- **Compression:** Gzip em respostas
- **CDN:** Para assets estáticos

## Escalabilidade

- **Load Balancing:** Nginx em frente
- **Horizontal Scaling:** Múltiplas instâncias do backend
- **Filas:** Bull/BullMQ para jobs assíncronos
- **Message Queue:** Para eventos entre serviços
- **Database Replication:** Master-slave setup

## Monitoramento

- **Logging:** Winston/Pino para registros
- **Error Tracking:** Sentry para exceções
- **Metrics:** Prometheus para KPIs
- **Health Checks:** Endpoints de status

---

**Próximos passos:** Consulte [API.md](./API.md) para detalhes dos endpoints.
