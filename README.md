# WEG OS – Sistema de Gestão de Ordens de Serviço e Ocorrências 🏭

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Clean_Arch_%2B_DDD-green?style=for-the-badge)

## 📌 Visão Geral

Este projeto foi desenvolvido como parte da **Situação de Aprendizagem Integrada** (Projeto Integrador). O objetivo é solucionar a dor fictícia da empresa WEG, automatizando o controle de manutenções industriais que anteriormente era manual e propenso a falhas.

O sistema gerencia **Máquinas**, **Ordens de Serviço (OS)** e **Ocorrências**, com foco em integridade de dados, rastreabilidade total e automação de regras de negócio complexas.

---

## 🏗️ Arquitetura do Projeto

Para garantir escalabilidade, testabilidade e alta coesão, adotamos uma **Arquitetura Modular** baseada nos princípios do **DDD (Domain-Driven Design)** e **Clean Architecture**. O sistema separa claramente os componentes técnicos (Infraestrutura) das regras de negócio (Domínio) e da orquestração de tarefas (Aplicação).

### 📂 Estrutura de Pastas

A organização do código reflete a separação de responsabilidades:

```text
src/main/java/
├── Aplicacao/                # Camada de Orquestração (Use Cases)
│   ├── Funcionario/          # Regras de aplicação para usuários
│   ├── Maquina/              # Casos de uso de ativos
│   ├── Ocorrencia/           # Gestão de regras de ocorrências
│   └── OrdemDeServico/       # Fluxos de abertura/fechamento de OS
│
├── Dominio/                  # O "Coração" do Negócio (Core)
│   ├── Funcionario/          # Entidades e Interfaces de Repositório
│   ├── Maquina/              # Regras de validação de equipamentos
│   ├── Ocorrencia/           # Regras de negócio de falhas recorrentes
│   └── OrdemDeServico/       # Lógica complexa e Agregados
│
├── Infraestrutura/           # Componentes Técnicos (Support)
│   ├── Configuracao/         # ConnectionFactory (Singleton)
│   └── Persistencia/         # Implementação JDBC dos Repositórios
│
├── Util/                     # Ferramentas Auxiliares
│   ├── Ferramentas.java      # Utilitários gerais de formatação
│   └── Validador.java        # Validações transversais
│
└── Views/                    # Interface do Usuário (Console)
    ├── Sistema/              # Menus principais
    └── [Modulos]/            # Telas específicas por contexto
```

### 🧠 Decisões Arquiteturais

1.  **Domínio (Core):**
    * É a camada mais importante. Contém as Entidades (`OrdemDeServico`, `Maquina`) e as **Interfaces de Repositório**.
    * *Regra de Ouro:* Esta camada desconhece o banco de dados e a interface visual.

2.  **Aplicação (Application):**
    * Atua como a ponte entre a View e o Domínio. Ela coordena as tarefas.
    * *Fluxo:* `O usuário pediu para abrir uma OS` → `Verifica permissão` → `Chama Domínio` → `Salva`.

3.  **Infraestrutura:**
    * Responsável por "falar com o mundo externo". Implementa as interfaces do Domínio usando **JDBC Puro** e SQL.
    * Utiliza o padrão **Mapper** para transformar resultados do banco (`ResultSet`) em objetos ricos.

---

### 🚀 Principais Funcionalidades

#### 1. Gestão de Acessos (RBAC)
* **Técnico:** Executa manutenções, registra custos e encerra OS.
* **Supervisor:** Abre OS, gerencia seu setor e trata ocorrências.
* **Gerente:** Visão global, gestão de usuários e relatórios.

#### 2. Ciclo de Vida da Manutenção
* **Tipos de OS:** Preventiva, Corretiva e Preditiva.
* Custos, datas e status são rigorosamente auditados.
* Uso de **Enums** no banco de dados para garantir integridade referencial dos tipos e status.

#### 3. Automação de Ocorrências (Regra de Negócio Crítica) ⚠️
O sistema possui inteligência para detectar falhas recorrentes e sugerir ações:
> **A Regra:** Se uma máquina registrar **3 Ordens Corretivas** consecutivas sem uma manutenção Preditiva ativa, o sistema gera automaticamente uma **Ocorrência Pendente**.
>
> **Ação Necessária:** O Supervisor deve então abrir uma **OS Preditiva** para tratar a causa raiz e encerrar a ocorrência.

#### 4. Histórico e Logs (Auditoria)
Implementamos uma estratégia de **Espelhamento de Tabelas**:
* Mesmo se um funcionário for demitido ou uma máquina for excluída, os dados históricos (Logs) permanecem intactos para auditoria futura.

---

### 🛠️ Tecnologias e Padrões

* **Linguagem:** Java (JDK 17+)
* **Banco de Dados:** MySQL (Hospedado na Aiven Cloud)
* **Persistência:** JDBC Puro (Java Database Connectivity) - *Sem frameworks ORM.*
* **Padrões de Projeto (Design Patterns):**
    * **Repository Pattern:** Abstração do acesso a dados.
    * **Dependency Injection:** Inversão de controle entre camadas.
    * **Factory Method:** Usado na `ConnectionFactory`.
    * **Data Mapper:** Conversão de SQL para Objetos.
    * **Aggregate Pattern (DDD):** Tratamento de OS como entidade raiz.
