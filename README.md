# CONSULTA DE CEPs

Para esse desafio consulta de CEPs vamos adaptar a arquitetura e os princípios da Clean Architecture (por Robert C. Martin / Uncle Bob), alinhando com Spring Boot, SOLID e injeção de dependência.

o objectivo é manter as regras de negócio desacopladas de frameworks, bancos de dados e APIs externas, garantindo testabilidade, escalabilidade e baixo acoplamento.

# Arquitetura baseada em Clean Architecture

Aqui está o mapeamento Clean Architecture aplicado:

Entities (Domain): Regras de negócio (ex: entidades como Cep, lógica de validação de formato)

Use Cases: Casos de uso da aplicação (ex: ConsultarCepUseCase)

Interface Adapters: Convertem dados entre os use cases e o mundo externo (API REST, DB, WireMock)

Frameworks/Drivers: Implementações técnicas (Spring, JPA, API client, banco de dados, WireMock)

# Clean Architecture - Camadas

# Presentation Layer (Camada de Apresentação)
Responsabilidade: Entrada e saída de dados (REST APIs).

Tecnologias: Spring Web (@RestController)
Componentes:
CepController
Validação de entrada
Conversão de DTOs

# Application Layer (Camada de Aplicação)
Responsabilidade: Orquestração de casos de uso da aplicação, lógica de negócios sem dependência de frameworks.

Componentes:
CepService
Casos de uso (ConsultarCepUseCase)
Interfaces (CepGateway, CepRepository)

# Domain Model (Modelo de Domínio)
Responsabilidade: Entidades de negócio e regras do domínio puras.

Componentes:
Cep
Validações internas
Objetos de valor (ex: Endereco)

# Infrastructure Layer (Infraestrutura)
Responsabilidade: Integrações externas e persistência.

# Tecnologias:
Spring Data JPA
WireMock
Docker
PostgreSQL
AWS (RDS, ECS)

# Componentes:
CepRepositoryImpl (implementação JPA)
ExternalCepClient (chamada à API externa via RestTemplate ou WebClient)
LogRepository para armazenar logs no banco

# Test Layer (TDD e QA)
Responsabilidade: Garantir cobertura e qualidade do código.
Tecnologias: JUnit 5, Mockito, Testcontainers, WireMock

# Testes:
Unitários (serviços, casos de uso)
Integração (chamada à API externa mockada)
Contratos (testes com dados simulados)

# Fluxo de chamada
CepController recebe uma requisição REST.
Chama ConsultarCepUseCase na camada de aplicação.
O use case delega para ExternalCepClient (mockada com WireMock) e salva log com CepRepository.
O resultado é retornado para o controller e enviado ao usuário.


# Vantagens em aplicar Clean Architecture aqui

Testabilidade: Fácil mockar gateways e testar use cases isoladamente
Evolutividade: Pode trocar API externa, banco ou framework sem mexer no core
Clareza: Separação explícita entre regra de negócio e detalhes de implementação
Reutilização: Regras de negócio podem ser reaproveitadas em outros canais (CLI, API, fila)
