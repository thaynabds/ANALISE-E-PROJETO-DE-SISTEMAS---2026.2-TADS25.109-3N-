# ATIVIDADE PRÁTICA, AULA 04

## Análise e Projeto de Sistemas, 2026.2

**Data da atividade:** 04/09/2026
**Data de conclusão:** 11/09/2026
**Alunas:** Thayná Batista da Silva e Poliana Fontes

| Informação         | Dados                                        |
| ------------------ | -------------------------------------------- |
| Curso              | Análise e Desenvolvimento de Sistemas        |
| Unidade Curricular | TADS25.109/3N, Análise e Projeto de Sistemas |
| Instituição        | Faculdade Senac Pernambuco                   |
| Alunas             | Thayná Batista da Silva e Poliana Fontes     |
| Local              | Recife, Pernambuco, Brasil                   |

---

## 1. Desafio, Escolhendo Arquitetura

### Cenário proposto

O desafio consiste em analisar a arquitetura mais adequada para um **sistema de delivery**, semelhante a plataformas como o iFood.

O sistema deverá disponibilizar, inicialmente, as seguintes funcionalidades principais:

* Cadastro de usuários.
* Gerenciamento de pedidos.
* Processamento de pagamentos.
* Gerenciamento de entregas.

A escolha da arquitetura deve considerar aspectos como organização, manutenção, escalabilidade, segurança, disponibilidade e possibilidade de evolução do sistema.

---

# 2. Arquitetura Escolhida

## Microsserviços

Para o sistema de delivery proposto, a arquitetura escolhida é a **Arquitetura de Microsserviços**.

Essa arquitetura consiste na divisão do sistema em serviços independentes, nos quais cada microsserviço possui uma responsabilidade específica e pode ser desenvolvido, implantado, escalado e atualizado de maneira independente.

A escolha é adequada ao cenário porque um sistema de delivery possui diferentes domínios de negócio, como usuários, pedidos, pagamentos e entregas. Esses domínios possuem comportamentos e necessidades diferentes e podem apresentar cargas distintas durante a utilização da plataforma.

---

# 3. Justificativa da Escolha

A arquitetura de microsserviços é a opção mais adequada para o sistema de delivery devido aos seguintes fatores.

### 3.1 Escalabilidade

Cada serviço pode ser escalado individualmente conforme sua demanda.

Por exemplo, durante horários de pico, o serviço responsável pelos pedidos pode receber uma quantidade muito maior de requisições do que o serviço de cadastro de usuários.

Com microsserviços, é possível aumentar a capacidade apenas do serviço que necessita de maior processamento.

### 3.2 Independência dos serviços

Cada microsserviço possui uma responsabilidade bem definida.

Uma alteração no serviço de pagamentos, por exemplo, não precisa exigir alterações nos serviços de usuários ou entregas.

Isso reduz o impacto das mudanças e facilita a evolução do sistema.

### 3.3 Manutenção

A separação por responsabilidades torna o código mais organizado e facilita a identificação de problemas.

Cada equipe ou desenvolvedor pode trabalhar em um serviço específico sem precisar compreender todo o sistema para realizar uma alteração.

### 3.4 Resiliência

Problemas em um serviço não precisam necessariamente interromper todo o sistema.

Por exemplo, caso o serviço de pagamentos apresente uma indisponibilidade temporária, o restante da plataforma pode continuar funcionando e tratar a operação de pagamento de maneira controlada.

Mecanismos como **Circuit Breaker, timeout, retry e filas de mensagens** podem ser utilizados para aumentar a resiliência.

### 3.5 Evolução tecnológica

Cada serviço pode evoluir de maneira independente, desde que mantenha seus contratos de comunicação.

Isso permite substituir ou atualizar componentes específicos sem a necessidade de modificar toda a aplicação.

---

# 4. Organização do Sistema

O sistema pode ser dividido nos seguintes microsserviços principais:

```text
                         SISTEMA DE DELIVERY
                                  |
                         API Gateway
                                  |
          +-----------------------+-----------------------+
          |                       |                       |
          v                       v                       v
   Usuários Service        Pedidos Service        Pagamentos Service
          |                       |                       |
          v                       v                       v
    Banco de Dados           Banco de Dados          Banco de Dados
                                  |
                                  v
                         Entregas Service
                                  |
                                  v
                           Banco de Dados
```

Cada serviço possui sua própria responsabilidade e banco de dados.

---

# 5. Microsserviço de Usuários

## Responsabilidade

Responsável pelo gerenciamento das informações dos usuários da plataforma.

### Principais funcionalidades

* Cadastro de usuários.
* Atualização de dados.
* Consulta de usuários.
* Autenticação.
* Gerenciamento de credenciais.
* Controle de acesso.

### Estrutura

```text
Usuario Service
│
├── Controller
├── Service
├── Repository
├── Model
└── Security
```

O serviço deve utilizar mecanismos seguros de autenticação e autorização, como **JWT ou OAuth 2.0**, conforme a necessidade do projeto.

---

# 6. Microsserviço de Pedidos

## Responsabilidade

Responsável pelo gerenciamento dos pedidos realizados pelos usuários.

### Principais funcionalidades

* Criar pedido.
* Consultar pedido.
* Atualizar status do pedido.
* Cancelar pedido.
* Consultar histórico.
* Integrar com pagamento.
* Integrar com entrega.

### Exemplos de status

```text
CRIADO
   |
   v
AGUARDANDO_PAGAMENTO
   |
   v
PAGAMENTO_APROVADO
   |
   v
EM_PREPARACAO
   |
   v
PRONTO_PARA_ENTREGA
   |
   v
EM_ENTREGA
   |
   v
ENTREGUE
```

O serviço de pedidos deve ser responsável apenas pelas regras relacionadas aos pedidos, evitando concentrar responsabilidades pertencentes aos demais microsserviços.

---

# 7. Microsserviço de Pagamentos

## Responsabilidade

Responsável pelo processamento e controle das transações financeiras relacionadas aos pedidos.

### Principais funcionalidades

* Solicitar pagamento.
* Validar pagamento.
* Registrar transação.
* Consultar status.
* Processar aprovação ou recusa.
* Realizar tratamento de falhas.

### Segurança

O serviço deve possuir requisitos de segurança mais rigorosos devido ao tratamento de informações financeiras.

Dados sensíveis não devem ser armazenados de maneira insegura.

A comunicação deve utilizar **HTTPS/TLS**, e credenciais e chaves de integração devem ser armazenadas por meio de variáveis de ambiente ou mecanismos seguros de gerenciamento de segredos.

---

# 8. Microsserviço de Entregas

## Responsabilidade

Responsável pelo gerenciamento da entrega dos pedidos.

### Principais funcionalidades

* Criar entrega.
* Associar entrega a um pedido.
* Atualizar status.
* Acompanhar entrega.
* Registrar conclusão.
* Informar ocorrências.

### Exemplos de status

```text
AGUARDANDO_ENTREGADOR
        |
        v
ENTREGADOR_ACEITOU
        |
        v
COLETANDO_PEDIDO
        |
        v
EM_TRANSITO
        |
        v
ENTREGA_CONCLUIDA
```

---

# 9. API Gateway

Para centralizar o acesso aos microsserviços, pode ser utilizado um **API Gateway**.

O cliente não precisa acessar diretamente cada serviço.

```text
Cliente
   |
   v
API Gateway
   |
   +------> Usuários Service
   |
   +------> Pedidos Service
   |
   +------> Pagamentos Service
   |
   +------> Entregas Service
```

O API Gateway pode ser responsável por:

* Roteamento.
* Autenticação.
* Autorização.
* Controle de acesso.
* Rate limiting.
* Monitoramento.
* Tratamento inicial de requisições.

---

# 10. Comunicação entre os Serviços

Os microsserviços podem utilizar dois modelos principais de comunicação.

## Comunicação síncrona

Pode ser realizada utilizando protocolos HTTP e APIs REST.

Exemplo:

```text
Pedidos Service
      |
      | HTTP
      v
Pagamentos Service
      |
      v
Resposta do pagamento
```

## Comunicação assíncrona

Para eventos que não precisam de resposta imediata, podem ser utilizadas filas ou sistemas de mensageria.

Exemplo:

```text
Pedidos Service
      |
      | Evento: Pedido criado
      v
Message Broker
      |
      +------> Pagamentos Service
      |
      +------> Entregas Service
```

Essa abordagem reduz o acoplamento entre os serviços e pode melhorar a resiliência da aplicação.

---

# 11. Banco de Dados

Na arquitetura de microsserviços, cada serviço deve possuir autonomia sobre seus dados.

Uma organização possível seria:

```text
Usuários Service
        |
        v
Banco de Dados de Usuários


Pedidos Service
        |
        v
Banco de Dados de Pedidos


Pagamentos Service
        |
        v
Banco de Dados de Pagamentos


Entregas Service
        |
        v
Banco de Dados de Entregas
```

Essa separação evita que todos os serviços dependam diretamente de um único banco de dados compartilhado.

---

# 12. Organização em Camadas

Cada microsserviço pode seguir uma organização em camadas:

```text
Controller
     |
     v
Service
     |
     v
Repository
     |
     v
Database
```

### Controller

Responsável por receber as requisições externas e encaminhá-las para a camada de serviço.

### Service

Responsável pelas regras de negócio da aplicação.

### Repository

Responsável pelo acesso aos dados persistidos.

### Database

Responsável pelo armazenamento das informações pertencentes ao microsserviço.

Essa organização contribui para a separação de responsabilidades e facilita manutenção e testes.

---

# 13. Segurança

O sistema deverá considerar requisitos de segurança desde sua arquitetura.

Entre as principais medidas estão:

* Autenticação baseada em JWT ou OAuth 2.0.
* Autorização baseada em permissões.
* Comunicação utilizando HTTPS/TLS.
* Senhas armazenadas utilizando algoritmos apropriados de hash.
* Validação das entradas recebidas pela API.
* Proteção contra acesso não autorizado.
* Gerenciamento seguro de segredos.
* Não armazenamento de credenciais diretamente no código.
* Uso de variáveis de ambiente.
* Arquivo `.gitignore` configurado para evitar o versionamento de informações sensíveis.

Exemplo:

```text
.env
application-secrets.yml
credentials.json
```

Esses arquivos não devem ser enviados ao repositório Git.

---

# 14. Resiliência

Como o sistema depende de vários serviços independentes, mecanismos de resiliência devem ser utilizados.

### Circuit Breaker

Impede chamadas repetidas para um serviço que está apresentando falhas.

### Timeout

Define um limite de tempo para uma requisição aguardar uma resposta.

### Retry

Permite realizar novas tentativas em determinadas falhas temporárias.

### Mensageria

Filas podem ser utilizadas para desacoplar processos e evitar que uma indisponibilidade temporária interrompa toda a operação.

---

# 15. Containerização

Cada microsserviço pode ser executado em um container independente utilizando Docker.

Exemplo:

```text
Docker
│
├── API Gateway
├── Usuários Service
├── Pedidos Service
├── Pagamentos Service
├── Entregas Service
└── Message Broker
```

Essa abordagem facilita a padronização dos ambientes de desenvolvimento, testes e produção.

---

# 16. Testes

Cada microsserviço deve possuir testes próprios.

A estratégia pode incluir:

* Testes unitários.
* Testes de integração.
* Testes de API.
* Testes de contrato.
* Testes de segurança.

Como meta de qualidade, recomenda-se manter **cobertura de testes superior a 80%**, principalmente nas regras de negócio críticas.

---

# 17. Vantagens e Desvantagens

| Vantagens                                 | Desvantagens                               |
| ----------------------------------------- | ------------------------------------------ |
| Escalabilidade independente               | Maior complexidade operacional             |
| Serviços desacoplados                     | Comunicação distribuída                    |
| Facilidade de evolução                    | Monitoramento mais complexo                |
| Manutenção por domínio                    | Maior quantidade de componentes            |
| Maior isolamento de falhas                | Necessidade de infraestrutura adequada     |
| Possibilidade de implantação independente | Consistência distribuída pode ser complexa |

---

# 18. Comparação com Arquitetura Monolítica

Uma arquitetura monolítica poderia atender a uma primeira versão muito simples do sistema.

Entretanto, considerando a evolução de uma plataforma de delivery, a arquitetura de microsserviços apresenta vantagens importantes.

| Critério                       | Monolítica        | Microsserviços  |
| ------------------------------ | ----------------- | --------------- |
| Implementação inicial          | Mais simples      | Mais complexa   |
| Escalabilidade                 | Aplicação inteira | Por serviço     |
| Implantação                    | Centralizada      | Independente    |
| Isolamento de falhas           | Menor             | Maior           |
| Manutenção em sistemas grandes | Mais difícil      | Mais organizada |
| Complexidade operacional       | Menor             | Maior           |
| Adequação ao cenário proposto  | Média             | Alta            |

Portanto, considerando que o cenário representa um sistema de delivery com potencial de crescimento, **microsserviços é a escolha mais adequada**.

---

# 19. Arquitetura Final Proposta

A solução pode ser representada da seguinte maneira:

```text
                         CLIENTE
                            |
                            v
                      API GATEWAY
                            |
          +-----------------+-----------------+
          |                 |                 |
          v                 v                 v
    USUÁRIOS SERVICE   PEDIDOS SERVICE   PAGAMENTOS SERVICE
          |                 |                 |
          v                 v                 v
      DB USERS          DB PEDIDOS       DB PAGAMENTOS
                            |
                            v
                     ENTREGAS SERVICE
                            |
                            v
                       DB ENTREGAS


                 MESSAGE BROKER
                       ^
                       |
          +------------+------------+
          |                         |
          |                         |
   PEDIDOS SERVICE           PAGAMENTOS SERVICE
```

---

# 20. Conclusão

Para o sistema de delivery proposto, a **Arquitetura de Microsserviços** é a alternativa escolhida por oferecer maior capacidade de escalabilidade, separação de responsabilidades, independência entre componentes e facilidade de evolução.

O sistema será dividido principalmente nos serviços de **Usuários, Pedidos, Pagamentos e Entregas**, com um **API Gateway** responsável pelo acesso aos serviços e possibilidade de utilização de um **Message Broker** para comunicação assíncrona.

Cada microsserviço deverá possuir suas próprias camadas de **Controller, Service e Repository**, mantendo suas responsabilidades bem definidas e evitando alto acoplamento.

Apesar de apresentar maior complexidade operacional quando comparada à arquitetura monolítica, essa abordagem é adequada para um sistema de delivery que pode crescer em quantidade de usuários, pedidos e transações.

Dessa forma, a arquitetura proposta busca atender não apenas ao funcionamento inicial do sistema, mas também aos requisitos de **escalabilidade, segurança, manutenção, disponibilidade e evolução futura**.

---

## 21. Identificação das Alunas

**Alunas:** Thayná Batista da Silva e Poliana Fontes
**Curso:** Análise e Desenvolvimento de Sistemas
**Instituição:** Faculdade Senac Pernambuco
**Unidade Curricular:** Análise e Projeto de Sistemas
**Turma:** TADS25.109/3N
**Atividade:** Atividade Prática, Aula 04
**Data:** 04/09/2026

---

> **“Antes de programar, é preciso estruturar o sistema.”**

A arquitetura é um dos elementos fundamentais para transformar uma solução improvisada em um sistema estruturado, sustentável e preparado para evolução.
