# WEG OS – Sistema de Gestão de Ordens de Serviço e Ocorrências 🏭

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Clean_Arch_%2B_DDD-green?style=for-the-badge)

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como parte da **Situação de Aprendizagem Integrada** (Projeto Integrador). O objetivo é solucionar a dor fictícia da empresa WEG, automatizando o controle de manutenções industriais que anteriormente era manual.

O sistema gerencia **Máquinas**, **Ordens de Serviço (OS)** e **Ocorrências**, com foco em integridade de dados, rastreabilidade e automação de regras de negócio complexas.

---

## 🏗️ Arquitetura do Projeto

Para garantir escalabilidade e manutenção simplificada, o sistema foi organizado seguindo princípios de **Clean Architecture** e **DDD (Domain-Driven Design)**. A estrutura de diretórios separa claramente as regras de negócio (Domínio) dos detalhes técnicos (Infraestrutura) e da interação com o usuário (Views).

Abaixo, a árvore de diretórios explicada:

src/main/java/
├── Aplicacao/                # Camada de Orquestração (Use Cases)
│   ├── Funcionario/          # Regras de aplicação para usuários
│   ├── Maquina/              # Casos de uso de ativos
│   └── OrdemDeServico/       # Fluxos de abertura/fechamento de OS
│
├── Dominio/                  # O "Coração" do Negócio (Core)
│   ├── Funcionario/          # Entidades e Interfaces de Repositório
│   ├── Maquina/              # Regras de validação de equipamentos
│   └── OrdemDeServico/       # Lógica complexa e Agregados
│
├── Infraestrutura/           # Componentes Técnicos (Support)
│   ├── Configuracao/         # ConnectionFactory (Singleton)
│   ├── Persistencia/         # Implementação JDBC dos Repositórios
│   └── Util/                 # Validadores e ferramentas auxiliares
│
└── Views/                    # Interface do Usuário (Console)
    ├── Sistema/              # Menus principais
    └── [Modulos]/            # Telas específicas por contexto

---

## 🚀 Principais Funcionalidades

### 1. Gestão de Acessos (RBAC)
* **Técnico:** Executa e encerra Ordens de Serviço.
* **Supervisor:** Abre OS, gerencia seu setor e trata ocorrências.
* **Gerente:** Acesso total e relatórios globais.

### 2. Ciclo de Vida da Manutenção
* Tipos de OS: **Preventiva**, **Corretiva** e **Preditiva**.
* Custos, datas e status são auditados.

### 3. Automação de Ocorrências (Regra de Negócio Crítica)
O sistema monitora a saúde das máquinas automaticamente:
> **Regra:** Se uma máquina registrar **3 Ordens Corretivas** consecutivas sem uma Preditiva ativa, o sistema gera automaticamente uma **Ocorrência Pendente**.
> O Supervisor deve então abrir uma **OS Preditiva** para tratar a causa raiz.

### 4. Histórico e Logs (Auditoria)
Implementamos uma estratégia de **Tabelas de Histórico**:
* Mesmo se um funcionário for demitido ou máquina excluída, os dados históricos (Logs) permanecem intactos para auditoria, garantindo a integridade referencial.

---

## 🛠️ Tecnologias e Padrões Utilizados

* **Linguagem:** Java (JDK 17+)
* **Banco de Dados:** MySQL (Hospedado na Nuvem - Aiven)
* **Conexão:** JDBC Puro (Sem frameworks como Hibernate/JPA para fins didáticos).
* **Padrões de Projeto:**
    * Repository Pattern
    * Dependency Injection (Inversão de Dependência)
    * Factory Pattern (`ConnectionFactory`)
    * Mapper Pattern
    * Aggregate Pattern (DDD)

---
