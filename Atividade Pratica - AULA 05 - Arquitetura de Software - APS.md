# Atividade Prática, Aula 05, Arquitetura de Software

## APS, Tomada de Decisão e Estruturação Arquitetural de Software

**Aluna:** Thayná Batista da Silva  
**Curso:** Tecnólogo em Análise e Desenvolvimento de Sistemas  
**Instituição:** Faculdade Senac Recife, PE, Brasil  
**Unidade Curricular:** Análise e Projeto de Sistemas  
**Turma:** TADS25.109/3N  
**Professor:** Guibson Barros de Almeida Santana  
**Período:** 2026.2

## 1. Objetivo da atividade

A atividade tem como objetivo analisar o cenário da FastEventos e escolher uma arquitetura de software adequada para o desenvolvimento de um MVP.

A escolha deve considerar principalmente:

1. Equipe pequena, com 3 desenvolvedores.
2. Prazo curto, de 6 semanas.
3. Necessidade de entregar um MVP funcional.
4. Simplicidade e velocidade de desenvolvimento.
5. Possibilidade de evolução futura do sistema.

A atividade também solicita a definição das responsabilidades de cada parte do sistema e a organização das pastas do projeto, principalmente para o fluxo de compra de ingressos.

# 2. Estudo de caso, FastEventos

A **FastEventos** é uma startup que deseja digitalizar a venda e a gestão de ingressos para eventos locais, como shows, palestras e festas.

Atualmente, os produtores dependem de vendas físicas ou de plataformas que cobram taxas consideradas altas.

A plataforma deverá oferecer:

1. Criação e gestão de eventos.
2. Cadastro de diferentes lotes de ingressos e preços.
3. Checkout para compra de ingressos.
4. Processamento de pagamentos.
5. Geração automática de ingressos com QR Code.
6. Aplicativo ou interface de portaria para validar QR Codes em tempo real.
7. Dashboards com informações de vendas e lotação.

O principal desafio é desenvolver um MVP em apenas 6 semanas com uma equipe de 3 desenvolvedores. Por isso, a arquitetura precisa ser simples de entender, rápida de desenvolver e organizada para permitir futuras melhorias.

# 3. Escolha da arquitetura

## Arquitetura escolhida

Para a FastEventos, foi escolhida a arquitetura **Cliente Servidor com API REST e Backend em Camadas**.

A aplicação terá uma interface cliente responsável pela interação com o usuário e uma API REST responsável pelo processamento das operações do sistema.

Dentro do backend, as responsabilidades serão separadas em camadas:

1. Controller, responsável por receber as requisições.
2. Service, responsável pelas regras de negócio.
3. Repository, responsável pelo acesso aos dados.
4. Entity, responsável pela representação das entidades do domínio.
5. DTO, responsável pela entrada e saída de dados da API.

### Visão simplificada

```text
Usuário
   │
   ▼
Interface Web / App
   │
   │ HTTP
   ▼
API REST
   │
   ▼
Controller
   │
   ▼
Service
   │
   ▼
Repository
   │
   ▼
Banco de Dados
```

Essa organização segue a ideia apresentada em aula de separar interface, lógica de negócio e dados. A arquitetura em camadas ajuda na organização, manutenção e separação de responsabilidades. fileciteturn0file0L278-L295

# 4. Justificativa da escolha

## 4.1. Tamanho da equipe

A equipe possui somente 3 desenvolvedores.

Uma arquitetura com vários microsserviços aumentaria a quantidade de componentes que precisam ser desenvolvidos, configurados, monitorados e integrados.

Para uma equipe pequena, uma API centralizada com responsabilidades separadas em camadas permite que os desenvolvedores trabalhem de forma organizada sem criar uma infraestrutura desnecessariamente complexa.

O material da aula destaca que microsserviços oferecem independência e escalabilidade, mas também possuem maior complexidade e exigem comunicação entre serviços. fileciteturn0file0L343-L362

## 4.2. Prazo de 6 semanas

O prazo para desenvolver o MVP é curto.

A arquitetura escolhida permite começar rapidamente, pois existe uma API central e as responsabilidades são organizadas dentro do mesmo projeto.

Isso reduz a quantidade de configurações necessárias para colocar o MVP em funcionamento.

## 4.3. Complexidade inicial do domínio

A FastEventos possui funcionalidades importantes, como eventos, lotes, vendas, pagamentos, ingressos e validação por QR Code.

Apesar disso, o cenário inicial ainda está limitado a um MVP. Não existe, no enunciado, uma necessidade imediata de separar cada funcionalidade em um serviço independente.

Por esse motivo, a separação em camadas atende ao objetivo inicial sem adicionar complexidade desnecessária.

## 4.4. Evolução futura

A arquitetura escolhida não impede uma evolução futura.

Se a plataforma crescer e alguma funcionalidade precisar ser independente, ela poderá ser separada posteriormente.

Por exemplo, o módulo de pagamentos ou o módulo de validação de ingressos poderá ser transformado em um serviço independente caso exista uma necessidade real de escala ou isolamento.

Essa possibilidade permite começar com uma solução simples e evoluir conforme o sistema crescer.

# 5. Comparação das opções arquiteturais

<table>
<thead><tr><th>Arquitetura</th><th>Adequação ao cenário</th></tr></thead>
<tbody>
<tr><td>Camadas N Tier</td><td>Boa organização, mas o foco principal do enunciado está na API e na separação das responsabilidades</td></tr>
<tr><td>MVC</td><td>Pode organizar aplicações web, mas não representa sozinho toda a estrutura da API e do domínio</td></tr>
<tr><td>Cliente Servidor com API REST</td><td>Atende bem à comunicação entre interface e backend e permite separar as responsabilidades internas</td></tr>
<tr><td>Monólito Modular</td><td>Também seria uma opção possível, principalmente pela simplicidade e divisão por módulos</td></tr>
<tr><td>Microsserviços</td><td>Permite alta escalabilidade, mas adiciona complexidade para uma equipe de 3 pessoas e um MVP de 6 semanas</td></tr>
</tbody>
</table>

A escolha final é **Cliente Servidor com API REST e Backend em Camadas**, porque atende diretamente ao cenário proposto e permite separar interface, regras de negócio e persistência.

O material da aula apresenta Cliente Servidor como uma divisão entre cliente e servidor e mostra a arquitetura em camadas como uma forma de separar apresentação, lógica de negócio e dados. fileciteturn0file0L278-L302

# 6. Matriz de responsabilidades

<table>
<thead><tr><th>Camada</th><th>Responsabilidade</th><th>Exemplos</th></tr></thead>
<tbody>
<tr><td>Interface</td><td>Interagir com o usuário e apresentar informações</td><td>Tela de eventos, tela de checkout, confirmação da compra</td></tr>
<tr><td>Controller</td><td>Receber requisições HTTP, validar a entrada e chamar o Service</td><td><code>POST /pedidos</code>, <code>GET /eventos/{id}</code></td></tr>
<tr><td>Service</td><td>Executar as regras de negócio</td><td>Verificar disponibilidade, calcular valor, confirmar compra</td></tr>
<tr><td>Repository</td><td>Acessar o banco de dados</td><td>Buscar evento, salvar pedido, consultar ingresso</td></tr>
<tr><td>Entity</td><td>Representar as entidades do domínio</td><td>Evento, Lote, Pedido, Ingresso, Pagamento</td></tr>
<tr><td>DTO</td><td>Transportar os dados entre cliente e API</td><td>Dados da compra, resposta do pedido</td></tr>
<tr><td>Banco de Dados</td><td>Armazenar os dados do sistema</td><td>Eventos, lotes, pedidos, ingressos e pagamentos</td></tr>
</tbody>
</table>

A separação entre regra de negócio e acesso aos dados é importante porque permite alterar a forma de persistência sem colocar comandos de banco dentro das regras da aplicação.

No modelo apresentado pelo professor, o Controller recebe a requisição, o Service executa as regras de negócio e o Repository realiza o acesso aos dados. fileciteturn0file0L87-L123

# 7. Regras de negócio do checkout

Para o fluxo de compra de ingressos, as principais regras consideradas são:

1. O evento precisa existir.
2. O evento precisa estar disponível para venda.
3. O lote selecionado precisa existir.
4. O lote precisa estar disponível.
5. A quantidade solicitada não pode ser maior que a quantidade disponível.
6. O valor da compra deve ser calculado de acordo com o lote selecionado.
7. O pagamento precisa ser processado antes da confirmação da compra.
8. Após o pagamento confirmado, o ingresso deve ser gerado.
9. Cada ingresso deve possuir um QR Code para validação na portaria.
10. Um ingresso já utilizado não deve ser aceito novamente.

Essas regras pertencem principalmente à camada **Service**, pois representam regras de negócio e não operações diretamente relacionadas ao banco de dados.

# 8. Árvore de diretórios

A estrutura abaixo representa uma organização possível para o backend da FastEventos.

```text
fasteventos_backend/
│
├── src/
│   │
│   ├── controllers/
│   │   ├── EventoController.ts
│   │   ├── PedidoController.ts
│   │   └── IngressoController.ts
│   │
│   ├── services/
│   │   ├── EventoService.ts
│   │   ├── PedidoService.ts
│   │   ├── PagamentoService.ts
│   │   └── IngressoService.ts
│   │
│   ├── repositories/
│   │   ├── EventoRepository.ts
│   │   ├── PedidoRepository.ts
│   │   └── IngressoRepository.ts
│   │
│   ├── entities/
│   │   ├── Evento.ts
│   │   ├── Lote.ts
│   │   ├── Pedido.ts
│   │   ├── Ingresso.ts
│   │   └── Pagamento.ts
│   │
│   ├── dtos/
│   │   ├── CriarPedidoDTO.ts
│   │   └── CriarIngressoDTO.ts
│   │
│   ├── routes/
│   │   └── index.ts
│   │
│   ├── config/
│   │   └── database.ts
│   │
│   └── server.ts
│
├── package.json
└── tsconfig.json
```

A organização segue o modelo apresentado no exemplo da atividade, que separa Controllers, Services, Repositories, Entities, DTOs, configuração do banco e inicialização do servidor. fileciteturn0file0L124-L141

# 9. Fluxo de Compra de Ingresso

O fluxo de checkout pode ser representado da seguinte forma:

```text
Cliente
   │
   │ 1. Escolhe evento e lote
   ▼
Interface
   │
   │ 2. POST /pedidos
   ▼
PedidoController
   │
   │ 3. Valida os dados recebidos
   ▼
PedidoService
   │
   ├── Verifica evento
   ├── Verifica lote
   ├── Verifica disponibilidade
   ├── Calcula valor
   └── Processa pagamento
   │
   ▼
PagamentoService
   │
   │ 4. Pagamento aprovado
   ▼
PedidoService
   │
   │ 5. Solicita criação do ingresso
   ▼
IngressoService
   │
   │ 6. Gera QR Code
   ▼
IngressoRepository
   │
   │ 7. Salva ingresso
   ▼
Banco de Dados
   │
   │ 8. Retorna confirmação
   ▼
PedidoController
   │
   │ 9. HTTP 201 Created
   ▼
Cliente
```

# 10. Explicação do fluxo

### 1. Seleção do ingresso

O usuário escolhe um evento e um lote de ingressos pela interface.

### 2. Envio da compra

A interface envia uma requisição HTTP para a API:

```text
POST /pedidos
```

A requisição contém os dados necessários para realizar a compra.

### 3. Recebimento pelo Controller

O `PedidoController` recebe a requisição e verifica se os dados básicos estão corretos.

Depois disso, encaminha a operação para o `PedidoService`.

### 4. Aplicação das regras de negócio

O `PedidoService` verifica:

1. Se o evento existe.
2. Se o evento está disponível.
3. Se o lote existe.
4. Se existem ingressos disponíveis.
5. Qual é o valor da compra.

Depois, solicita o processamento do pagamento.

### 5. Processamento do pagamento

O `PagamentoService` realiza a operação de pagamento.

Se o pagamento for aprovado, a compra pode continuar.

Se o pagamento for recusado, o pedido não deve ser confirmado.

### 6. Geração do ingresso

Com o pagamento aprovado, o sistema cria o ingresso e gera o QR Code correspondente.

### 7. Persistência

O `IngressoRepository` salva os dados do ingresso no banco de dados.

### 8. Resposta

Após a conclusão da operação, o Controller retorna a confirmação para o cliente.

Exemplo:

```text
HTTP 201 Created
```

Esse fluxo segue a mesma lógica do exemplo resolvido da atividade, no qual a requisição chega ao Controller, passa pelo Service, utiliza o Repository para persistir os dados e depois retorna uma resposta HTTP ao cliente. fileciteturn0file0L142-L151

# 11. Exemplo de endpoints

<table>
<thead><tr><th>Método</th><th>Endpoint</th><th>Responsabilidade</th></tr></thead>
<tbody>
<tr><td>GET</td><td><code>/eventos</code></td><td>Listar eventos</td></tr>
<tr><td>GET</td><td><code>/eventos/{id}</code></td><td>Consultar um evento</td></tr>
<tr><td>POST</td><td><code>/eventos</code></td><td>Criar evento</td></tr>
<tr><td>POST</td><td><code>/pedidos</code></td><td>Criar uma compra</td></tr>
<tr><td>GET</td><td><code>/pedidos/{id}</code></td><td>Consultar uma compra</td></tr>
<tr><td>POST</td><td><code>/pagamentos</code></td><td>Processar pagamento</td></tr>
<tr><td>GET</td><td><code>/ingressos/{id}</code></td><td>Consultar ingresso</td></tr>
<tr><td>POST</td><td><code>/ingressos/{id}/validar</code></td><td>Validar ingresso na portaria</td></tr>
</tbody>
</table>

Esses endpoints são uma representação da estrutura proposta para o sistema. O enunciado não exige a implementação da API, portanto os endpoints servem apenas para demonstrar como a arquitetura poderia funcionar.

# 12. Responsabilidade de cada camada no checkout

```text
INTERFACE
Responsável por:
Escolher evento
Escolher lote
Informar quantidade
Informar dados da compra
Exibir resultado

        │
        ▼

CONTROLLER
Responsável por:
Receber HTTP
Validar entrada
Chamar o Service
Retornar resposta HTTP

        │
        ▼

SERVICE
Responsável por:
Validar regras de negócio
Verificar disponibilidade
Calcular valores
Processar a compra
Solicitar geração do ingresso

        │
        ▼

REPOSITORY
Responsável por:
Buscar dados
Inserir dados
Atualizar dados
Consultar disponibilidade

        │
        ▼

BANCO DE DADOS
Responsável por:
Armazenar eventos
Armazenar lotes
Armazenar pedidos
Armazenar ingressos
Armazenar pagamentos
```

Essa divisão mantém as responsabilidades separadas, um dos princípios destacados no material da aula. O conteúdo também relaciona uma boa arquitetura à clareza, alta coesão, baixo acoplamento, reutilização e facilidade de manutenção. fileciteturn0file0L222-L227

# 13. Por que não escolher microsserviços neste momento?

Microsserviços são uma alternativa válida para sistemas que precisam de alta escalabilidade e independência entre serviços.

Porém, no cenário apresentado, existem apenas 3 desenvolvedores e o MVP precisa ficar pronto em 6 semanas.

Adotar vários serviços independentes poderia aumentar o trabalho com:

1. Comunicação entre serviços.
2. Configuração dos serviços.
3. Monitoramento.
4. Deploy.
5. Tratamento de falhas.
6. Gerenciamento de diferentes componentes.

O próprio material da aula apresenta a alta complexidade e a necessidade de comunicação entre serviços como desvantagens dos microsserviços. fileciteturn0file0L354-L362

Por isso, para o MVP, a solução escolhida prioriza simplicidade, organização e velocidade de desenvolvimento.

# 14. Possibilidade de evolução

A arquitetura proposta permite que o sistema evolua.

No início, todas as funcionalidades podem funcionar dentro do mesmo backend organizado em camadas.

Com o crescimento da FastEventos, algumas funcionalidades poderão ser separadas caso exista uma necessidade real.

Um possível cenário futuro seria:

```text
FastEventos
│
├── Serviço de Eventos
├── Serviço de Pedidos
├── Serviço de Pagamentos
├── Serviço de Ingressos
└── Serviço de Validação
```

Essa mudança não precisa acontecer durante o MVP. A prioridade inicial é entregar uma solução funcional dentro do prazo estabelecido.

# 15. Conclusão

Para a FastEventos, foi escolhida a arquitetura **Cliente Servidor com API REST e Backend em Camadas**.

A escolha considera as principais restrições apresentadas no problema:

1. Equipe pequena.
2. Prazo de 6 semanas.
3. Necessidade de um MVP funcional.
4. Domínio ainda em fase inicial.
5. Necessidade de manter o projeto organizado.
6. Possibilidade de evolução futura.

A arquitetura separa a interface, os Controllers, as regras de negócio, os Repositories e os dados. Com isso, cada parte do sistema possui uma responsabilidade definida.

A proposta também evita adicionar complexidade desnecessária ao MVP. Caso a plataforma cresça, a arquitetura poderá evoluir para uma estrutura mais distribuída quando houver uma necessidade concreta.

# 16. Conferência da atividade

A atividade foi conferida com base nas instruções fornecidas pelo professor.

<table>
<thead><tr><th>Critério solicitado</th><th>Situação</th><th>Onde foi atendido</th></tr></thead>
<tbody>
<tr><td>Escolher uma arquitetura</td><td>Atendido</td><td>Seção 3</td></tr>
<tr><td>Justificar pelo tamanho da equipe</td><td>Atendido</td><td>Seção 4.1</td></tr>
<tr><td>Justificar pelo prazo de 6 semanas</td><td>Atendido</td><td>Seção 4.2</td></tr>
<tr><td>Considerar a complexidade inicial</td><td>Atendido</td><td>Seção 4.3</td></tr>
<tr><td>Apresentar matriz de responsabilidades</td><td>Atendido</td><td>Seção 6</td></tr>
<tr><td>Separar UI, negócio e persistência</td><td>Atendido</td><td>Seção 6 e Seção 12</td></tr>
<tr><td>Mostrar entrada das requisições</td><td>Atendido</td><td>Controllers</td></tr>
<tr><td>Mostrar lógica de validação e negócio</td><td>Atendido</td><td>Services</td></tr>
<tr><td>Mostrar acesso aos dados</td><td>Atendido</td><td>Repositories</td></tr>
<tr><td>Mostrar entidades do domínio</td><td>Atendido</td><td>Entities</td></tr>
<tr><td>Criar árvore de diretórios</td><td>Atendido</td><td>Seção 8</td></tr>
<tr><td>Focar no checkout</td><td>Atendido</td><td>Seções 9 e 10</td></tr>
<tr><td>Explicar o fluxo</td><td>Atendido</td><td>Seção 10</td></tr>
<tr><td>Usar termos técnicos corretamente</td><td>Atendido</td><td>Documento completo</td></tr>
<tr><td>Manter linguagem simples</td><td>Atendido</td><td>Documento completo</td></tr>
</tbody>
</table>

A rubrica da atividade atribui 2,5 pontos para coerência arquitetural, 2,5 pontos para separação de papéis, 3,0 pontos para estrutura de pastas e 2,0 pontos para qualidade técnica. fileciteturn0file0L152-L177

## Resultado da conferência

**Todos os requisitos explícitos encontrados no enunciado foram contemplados neste README.**

A principal decisão arquitetural está alinhada com o cenário do exercício: equipe pequena, prazo curto e prioridade para simplicidade e velocidade de entrega. O material da aula também destaca que a escolha da estratégia deve considerar a complexidade do sistema, a experiência da equipe e o tempo e os recursos disponíveis. fileciteturn0file0L216-L221

# Autora

<div align="center">

## Thayná Batista da Silva

[LinkedIn](https://br.linkedin.com/in/thaynabds)

[Instagram](https://www.instagram.com/thaynabdstec/)

[Email](mailto:thaynabdstec@gmail.com)

Estudante de Análise e Desenvolvimento de Sistemas  
Faculdade Senac Recife, PE  
Previsão de formatura: 2027

</div>

<div align="center">

Desenvolvido por **Thayná Batista da Silva** para a Unidade Curricular **ANÁLISE E PROJETO DE SISTEMAS, TADS25.109/3N, 2026.2**, da Faculdade Senac Recife, sob orientação do Professor **Guibson Barros de Almeida Santana**.

Copyright © 2026, ThaynaBDSTec.

</div>
