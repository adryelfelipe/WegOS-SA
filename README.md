# WEG OS – Sistema de Gestão de Ordens de Serviço e Ocorrências 🏭

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=java&logoColor=white)
![MySQL](https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white)
![Architecture](https://img.shields.io/badge/Architecture-Clean_Arch_%2B_DDD-green?style=for-the-badge)

## 📌 Sobre o Projeto

Este projeto foi desenvolvido como parte da **Situação de Aprendizagem Integrada** (Projeto Integrador). O objetivo é solucionar a dor fictícia da empresa WEG, automatizando o controle de manutenções industriais que anteriormente era manual.

O sistema gerencia **Máquinas**, **Ordens de Serviço (OS)** e **Ocorrências**, com foco em integridade de dados, rastreabilidade e automação de regras de negócio complexas.

---

## 🏗️ Arquitetura do Projeto

Diferente da estrutura MVC tradicional, optamos por uma arquitetura baseada nos princípios de **Clean Architecture** e **DDD (Domain-Driven Design)**. Isso garante que as regras de negócio da WEG estejam desacopladas de detalhes técnicos como banco de dados ou interface.

### 📂 Estrutura de Pastas (Explicação)

A organização do código reflete a separação de responsabilidades:

* **`Dominio` (O Coração do Sistema):**
    * Contém as Entidades (`OrdemDeServico`, `Maquina`, `Funcionario`), Value Objects e **Interfaces de Repositório**.
    * *Regra de Ouro:* Esta camada desconhece o banco de dados.
* **`Aplicacao` (Casos de Uso):**
    * Orquestra as ações do sistema (ex: "Abrir uma nova OS", "Gerar Relatório"). Faz a ponte entre a View e o Domínio.
* **`Infraestrutura` (Detalhes Técnicos):**
    * **Persistencia:** Implementação dos Repositórios usando **JDBC Puro**.
    * **Configuracao:** `ConnectionFactory` para conexão centralizada com o banco (MySQL/Aiven).
    * Aqui aplicamos o padrão **Mapper** para converter `ResultSet` em Objetos de Domínio.
* **`Views` (Interface):**
    * Menus de console separados por contexto (FuncionarioView, MaquinaView, etc.) para interação com o usuário.
* **`Util`:**
    * Classes utilitárias auxiliares.

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
