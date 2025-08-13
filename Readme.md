# 🏋️ Fitness Platform - Microservices Architecture

[![Java](https://img.shields.io/badge/Java-17-orange.svg)](https://openjdk.java.net/projects/jdk/17/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.4-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Docker](https://img.shields.io/badge/Docker-Compose-blue.svg)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A modern fitness platform built with Spring Boot microservices architecture. This project demonstrates enterprise-level
patterns including service discovery, API gateway, event-driven architecture, and containerization.

## 🌟 Features

- **User Management** - Registration, authentication, and profile management
- **Workout Programs** - Create and track workout routines and exercises
- **Gym Directory** - Find gyms, read reviews, and ratings
- **Subscriptions** - Payment processing and billing management
- **Notifications** - Email, SMS, and push notifications
- **Microservices Architecture** - Scalable and maintainable design

## 🏗️ Architecture

```
┌─────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ Web Browser │───▶│   API Gateway   │───▶│Service Discovery│
└─────────────┘    └─────────────────┘    └─────────────────┘
                            │
            ┌───────────────┼───────────────┐
            ▼               ▼               ▼
    ┌─────────────┐ ┌─────────────┐ ┌─────────────┐
    │   User Svc  │ │ Workout Svc │ │   Gym Svc   │
    └─────────────┘ └─────────────┘ └─────────────┘
            │               │               │
            └───────────────┼───────────────┘
                            ▼
                ┌─────────────────────────┐
                │  Subscription & Notify  │
                └─────────────────────────┘
```

## 📋 Prerequisites

**Before you start, make sure you have these installed on your computer:**

### For Windows Users:

1. **Java 17** - [Download here](https://adoptium.net/temurin/releases/?version=17)
2. **Docker Desktop** - [Download here](https://www.docker.com/products/docker-desktop/)
3. **Git** - [Download here](https://git-scm.com/download/win)

### For Mac Users:

```bash
# Install using Homebrew (recommended)
brew install openjdk@17
brew install docker
brew install git
```

### For Linux Users:

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-17-jdk docker.io docker-compose git

# CentOS/RHEL
sudo yum install java-17-openjdk docker git
```

### ✅ Verify Installation

Open your terminal/command prompt and run these commands:

```bash
java -version          # Should show Java 17
docker --version       # Should show Docker version
git --version          # Should show Git version
```

## 🚀 Quick Start Guide

### Step 1: Get the Code

```bash
# Clone the repository
git clone https://github.com/your-username/fitness-platform.git

# Navigate to the project directory
cd fitness-platform
```

### Step 2: Start the Application

```bash
# Start all services with one command
docker-compose up --build
```

**⏰ Wait 2-3 minutes** for all services to start. You'll see logs from multiple services.

### Step 3: Verify Everything is Running

Open your web browser and visit:

- **Main Application**: http://localhost:8080
- **Service Dashboard**: http://localhost:8761
- **Database Admin**: http://localhost:8080/h2-console

### Step 4: Test the API

```bash
# Test user registration
curl -X POST http://localhost:8080/api/users/register \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123","name":"John Doe"}'

# Test user login
curl -X POST http://localhost:8080/api/users/login \
  -H "Content-Type: application/json" \
  -d '{"email":"john@example.com","password":"password123"}'
```

## 📁 Project Structure

```
fitness-platform/
├── README.md                           # You are here!
├── docker-compose.yml                  # All services configuration
├── pom.xml                            # Main project configuration
│
├── config-server/                     # Configuration management
├── eureka-server/                     # Service discovery
├── api-gateway/                       # API routing and security
│
├── user-service/                      # User management
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/com/fitness/user/
│
├── workout-service/                   # Workout programs & exercises
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/com/fitness/workout/
│
├── gym-service/                       # Gym directory & reviews
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/com/fitness/gym/
│
├── subscription-service/              # Payments & billing
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/com/fitness/subscription/
│
└── notification-service/              # Email, SMS, push notifications
    ├── Dockerfile
    ├── pom.xml
    └── src/main/java/com/fitness/notification/
```

## 🐳 Docker Configuration

### Infrastructure Services

```yaml
# docker-compose.yml - Key services
services:
  # Database
  postgres:
    image: postgres:15
    ports: [ "5432:5432" ]
    environment:
      POSTGRES_DB: fitness_db
      POSTGRES_USER: fitness_user
      POSTGRES_PASSWORD: fitness_pass

  # Cache
  redis:
    image: redis:7-alpine
    ports: [ "6379:6379" ]

  # Message Queue
  kafka:
    image: confluentinc/cp-kafka:latest
    ports: [ "9092:9092" ]
    environment:
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092

  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    ports: [ "2181:2181" ]
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
```

## 🔧 Development Guide

### Running Services Individually

If you want to develop a specific service:

```bash
# Start only infrastructure
docker-compose up postgres redis kafka zookeeper

# Run a specific service locally
cd user-service
mvn spring-boot:run
```

### Building from Source

```bash
# Build all services
mvn clean install

# Build specific service
cd user-service
mvn clean install

# Build Docker image for specific service
docker build -t fitness-user-service .
```

### Service Ports

| Service              | Port | URL                   |
|----------------------|------|-----------------------|
| API Gateway          | 8080 | http://localhost:8080 |
| Config Server        | 8888 | http://localhost:8888 |
| Eureka Server        | 8761 | http://localhost:8761 |
| User Service         | 8081 | http://localhost:8081 |
| Workout Service      | 8082 | http://localhost:8082 |
| Gym Service          | 8083 | http://localhost:8083 |
| Subscription Service | 8084 | http://localhost:8084 |
| Notification Service | 8085 | http://localhost:8085 |

## 📊 Monitoring and Health Checks

### Service Health

```bash
# Check all services status
curl http://localhost:8761

# Check specific service health
curl http://localhost:8081/actuator/health
curl http://localhost:8082/actuator/health
```

### Database Access

- **PostgreSQL**: `localhost:5432`
    - Database: `fitness_db`
    - Username: `fitness_user`
    - Password: `fitness_pass`

### Message Queue

- **Kafka**: `localhost:9092`
- **Zookeeper**: `localhost:2181`

## 🧪 API Examples

### User Management

```bash
# Register new user
curl -X POST http://localhost:8080/api/users/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane@example.com",
    "password": "securepass123",
    "firstName": "Jane",
    "lastName": "Smith",
    "dateOfBirth": "1990-05-15"
  }'

# User login
curl -X POST http://localhost:8080/api/users/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "jane@example.com",
    "password": "securepass123"
  }'
```

### Workout Management

```bash
# Create workout program
curl -X POST http://localhost:8080/api/workouts/programs \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "name": "Beginner Full Body",
    "description": "A complete workout for beginners",
    "difficulty": "BEGINNER",
    "estimatedDuration": "45 minutes"
  }'
```

### Gym Directory

```bash
# Find nearby gyms
curl "http://localhost:8080/api/gyms/nearby?latitude=51.5074&longitude=-0.1278&radius=5"

# Submit gym review
curl -X POST http://localhost:8080/api/gyms/1/reviews \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_JWT_TOKEN" \
  -d '{
    "rating": 5,
    "comment": "Great facilities and friendly staff!"
  }'
```

## 🚨 Troubleshooting

### Common Issues

**Problem**: Services won't start

```bash
# Solution: Check if ports are available
netstat -an | findstr :8080    # Windows
lsof -i :8080                  # Mac/Linux

# Stop any conflicting services
docker-compose down
```

**Problem**: Database connection errors

```bash
# Solution: Reset database
docker-compose down -v
docker-compose up postgres
```

**Problem**: Out of memory errors

```bash
# Solution: Increase Docker memory limit
# Docker Desktop → Settings → Resources → Memory → Increase to 4GB+
```

### Logs and Debugging

```bash
# View logs for all services
docker-compose logs

# View logs for specific service
docker-compose logs user-service

# Follow live logs
docker-compose logs -f user-service

# View last 100 log lines
docker-compose logs --tail=100 user-service
```

## 🔐 Security Features

- **JWT Authentication** - Stateless authentication tokens
- **Password Hashing** - BCrypt encryption for user passwords
- **API Gateway Security** - Centralized authentication and authorization
- **HTTPS Support** - SSL/TLS encryption (production ready)
- **Input Validation** - Comprehensive request validation

## 📈 Performance Features

- **Caching** - Redis for session management and frequent data
- **Connection Pooling** - Optimized database connections
- **Lazy Loading** - JPA optimizations for database queries
- **Async Processing** - Kafka for non-blocking operations
- **Health Checks** - Built-in service monitoring

## 🌐 Production Deployment

### Environment Variables

```bash
# Required for production
export DATABASE_URL=jdbc:postgresql://prod-db:5432/fitness_db
export REDIS_URL=redis://prod-redis:6379
export KAFKA_BROKERS=prod-kafka:9092
export JWT_SECRET=your-super-secret-jwt-key
export STRIPE_SECRET_KEY=sk_live_your_stripe_key
```

### Docker Production

```bash
# Build for production
docker-compose -f docker-compose.prod.yml build

# Deploy to production
docker-compose -f docker-compose.prod.yml up -d
```

## 🤝 Contributing

We welcome contributions! Here's how to get started:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Coding Standards

- Use **Java 17** features
- Follow **Spring Boot** best practices
- Write **tests** for new features
- Document **API endpoints**
- Use **conventional commits**

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🏢 Enterprise Support

This project demonstrates enterprise-ready patterns suitable for production environments. For enterprise support,
consulting, or custom development:

- **Architecture Reviews**
- **Performance Optimization**
- **Security Audits**
- **Team Training**
- **Custom Feature Development**

## 📞 Support

- **Documentation**: Check this README and code comments
- **Issues**: [GitHub Issues](https://github.com/Amir-Golmoradi/Fitness-Platform/issues)
- **Discussions**: [GitHub Discussions](https://github.com/your-username/Fitness-Platform/discussions)
- **Email**: support@fitness-platform.com

## 🙏 Acknowledgments

- **Spring Boot Team** - For the excellent framework
- **Netflix OSS** - For Eureka service discovery
- **Apache Kafka** - For reliable messaging
- **Docker** - For containerization
- **PostgreSQL** - For robust data storage

---

**Made with ❤️ by the Fitness Platform Team**

*Ready to revolutionize fitness technology? Star ⭐ this repo and join our community!*