# Configuration Repository

This directory contains externalized configuration for all microservices in the LeetCode Clone application.

## Configuration Profiles

### Development Profile (`-dev.properties`)
Used for local development with:
- Local PostgreSQL databases
- Local RabbitMQ
- Debug logging enabled
- Actuator endpoints exposed with details

### Kubernetes Profile (`-k8s.properties`)
Used for Kubernetes deployments with:
- Service discovery for databases and message queues
- Environment variable injection for secrets
- Production logging levels
- Minimal actuator details

## Services Configuration

### Infrastructure Services

#### API Gateway (Port: 8080)
- **Files**: `api-gateway-dev.properties`, `api-gateway-k8s.properties`
- **Purpose**: Entry point for all client requests, routes to microservices
- **Features**: Load balancing, circuit breakers, rate limiting

#### Eureka Server (Port: 8761)
- **Purpose**: Service registry and discovery
- **Note**: Configured in service's own application.properties

#### Config Server (Port: 8888)
- **Purpose**: Centralized configuration management
- **Note**: Configured in service's own application.properties

### Core Services

#### Auth Service (Port: 8081)
- **Files**: `auth-service-dev.properties`, `auth-service-k8s.properties`
- **Database**: `leetcode_auth`
- **Purpose**: Authentication, authorization, JWT token management
- **Features**: 
  - JWT token generation and validation
  - User authentication
  - Password management
  - Token refresh

#### User Service (Port: 8082)
- **Files**: `user-service-dev.properties`, `user-service-k8s.properties`
- **Database**: `leetcode_users`
- **Purpose**: User profile and data management
- **Features**:
  - User CRUD operations
  - Profile management
  - User preferences

#### Problem Service (Port: 8086)
- **Files**: `problem-service-dev.properties`, `problem-service-k8s.properties`
- **Database**: `leetcode_problems`
- **Purpose**: Problem management and retrieval
- **Features**:
  - Problem CRUD operations
  - Test cases management
  - Problem categories and difficulty levels

#### Judge Service (Port: 8083)
- **Files**: `judge-service-dev.properties`, `judge-service-k8s.properties`
- **Database**: `leetcode_judge`
- **Message Queue**: RabbitMQ
- **Purpose**: Code execution and evaluation
- **Features**:
  - Multi-language code execution
  - Test case validation
  - Resource limiting (timeout, memory)
  - Sandbox execution
- **Settings**:
  - Timeout: 10 seconds
  - Max Memory: 512MB

#### AI Assist Service (Port: 8084)
- **Files**: `ai-assist-service-dev.properties`, `ai-assist-service-k8s.properties`
- **Database**: `leetcode_ai` (for caching)
- **External API**: OpenAI GPT-4
- **Purpose**: AI-powered hints and explanations
- **Features**:
  - Code explanation
  - Hint generation
  - Solution suggestions
  - Response caching
- **Settings**:
  - Model: GPT-4
  - Max Tokens: 2000
  - Temperature: 0.7
  - Timeout: 30 seconds

#### Metrics Service (Port: 8085)
- **Files**: `metrics-service-dev.properties`, `metrics-service-k8s.properties`
- **Database**: `leetcode_metrics`
- **Purpose**: Application metrics and monitoring
- **Features**:
  - User statistics
  - Problem solving metrics
  - System performance metrics
  - Prometheus integration
- **Settings**:
  - Collection interval: 60 seconds
  - Data retention: 90 days

## Port Allocation

| Service | Port | Database |
|---------|------|----------|
| API Gateway | 8080 | - |
| Auth Service | 8081 | leetcode_auth |
| User Service | 8082 | leetcode_users |
| Judge Service | 8083 | leetcode_judge |
| AI Assist Service | 8084 | leetcode_ai |
| Metrics Service | 8085 | leetcode_metrics |
| Problem Service | 8086 | leetcode_problems |
| Config Server | 8888 | - |
| Eureka Server | 8761 | - |

## Environment Variables

### Development
- `JWT_SECRET`: JWT signing secret (default provided in dev properties)
- `OPENAI_API_KEY`: OpenAI API key for AI assist service

### Kubernetes
Required environment variables:
- `DB_USERNAME`: PostgreSQL username
- `DB_PASSWORD`: PostgreSQL password
- `JWT_SECRET`: JWT signing secret
- `OPENAI_API_KEY`: OpenAI API key
- `RABBITMQ_USERNAME`: RabbitMQ username
- `RABBITMQ_PASSWORD`: RabbitMQ password

## Database Schema

Each service has its own database for data isolation:
- **leetcode_auth**: User credentials, roles, permissions
- **leetcode_users**: User profiles, preferences, settings
- **leetcode_problems**: Problems, test cases, solutions
- **leetcode_judge**: Submission history, execution results
- **leetcode_ai**: Cached AI responses, usage statistics
- **leetcode_metrics**: Performance metrics, user statistics

## Usage

### Local Development
Services automatically use `-dev` profile when `spring.profiles.active=dev` is set.

### Kubernetes Deployment
Services use `-k8s` profile when `spring.profiles.active=k8s` is set.
Inject environment variables via ConfigMaps and Secrets.

## Actuator Endpoints

### Development
- Full access to all actuator endpoints
- Health details visible
- Metrics exposed

### Production (Kubernetes)
- Limited actuator endpoints
- Health details hidden
- Prometheus metrics enabled

## Logging

### Development
- Package level: DEBUG
- Spring Cloud: INFO
- SQL queries visible

### Production
- Package level: INFO
- Spring Cloud: WARN
- SQL queries hidden

