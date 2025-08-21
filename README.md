# LocaDB – Sistema de Gerenciamento de Locação de Roupas

**Projeto de Banco de Dados – 2025/2**

> Repositório acadêmico para modelagem, implementação e experimentação de conceitos de Banco de Dados usando **PostgreSQL** e **SQL** em um domínio real: uma **loja de locação de roupas** (trajes sociais, fantasias, vestidos de festa etc.).

---

## 📌 Descrição

Este projeto foi concebido a partir de práticas de sala de aula em cursos de **Engenharia de Sofware**. A proposta é conduzir do **básico ao avançado**, sempre com **implementação passo a passo** e **exercícios práticos**. Você aprenderá desde a criação de tabelas e consultas até o desenvolvimento completo de um **banco relacional aplicado a um cenário comercial**.

Durante as atividades utilizaremos **SQL (Structured Query Language)** e **PostgreSQL**, uma das SGBDs mais populares do mercado.

**Tópicos trabalhados:**

* Criar, inserir, alterar e excluir dados de tabelas.
* Implementar **chaves primárias** e **chaves estrangeiras**.
* Agregar dados (soma, média, mínimos, máximos, contagem).
* Relacionar tabelas utilizando **JOINs**.
* Criar campos **autoincremento** e **valores default**.
* Criar **funções**, **stored procedures**, **triggers** e **domínios**.
* Criar usuários com **diferentes permissões** de acesso.
* Executar **backup** e **restore** da base de dados.
* Implementar consultas com **Álgebra Relacional**.
* Projetar do zero usando **Modelo Entidade‑Relacionamento** → **Modelo Conceitual** → **Modelo Lógico**.
* Aplicar **Formas Normais** (1FN, 2FN, 3FN, BCNF, 4FN, 5FN).

---

## 🎯 Objetivos de Aprendizagem

* Compreender e aplicar **modelagem relacional** a um domínio real.
* Escrever **SQL idiomático e performático** para consultas analíticas e operacionais.
* Dominar **integridade**, **normalização** e **regras de negócio** no nível do banco.
* Automatizar rotinas com **triggers**, **funções** e **views**.
* Gerenciar **acessos**, **papéis (roles)** e **privilégios**.
* Realizar **backup/restore** com segurança e rastreabilidade.

---

## 🧩 Escopo Funcional do Domínio

A loja de locação de roupas atende **clientes** que alugam **itens** (roupas e acessórios) por **períodos** específicos. O sistema deve registrar:

* **Catálogo:** produtos, categorias, tamanhos, cores, estado de conservação.
* **Estoque:** quantidade por item/tamanho, disponibilidade por período.
* **Clientes:** cadastro, contatos, histórico, preferências.
* **Locações:** reservas, retirada, devolução, renovação, cancelamentos.
* **Financeiro:** valores, descontos, multas por atraso/danos, formas de pagamento.
* **Operação:** funcionários, horários, fluxo de aprovação, auditoria de alterações.

> **Regras de negócio (exemplos):**
>
> * Um item **não pode** ser alocado a duas locações **com períodos sobrepostos**.
> * Se a devolução ultrapassar a data/hora prevista, calcular **multa proporcional**.
> * Danos reportados geram **taxas adicionais** e **bloqueio** do item até manutenção.
> * Cancelamentos obedecem **política parametrizável** (janela, taxa, exceções).

---

## 🏗️ Modelagem (Visão Geral)

**Entidades principais:** `cliente`, `funcionario`, `categoria`, `produto`, `produto_variacao` (tamanho/cor), `estoque`, `locacao`, `locacao_item`, `pagamento`, `desconto`, `multa`, `auditoria`.

**Relacionamentos essenciais:**

* `cliente` 1—N `locacao`
* `locacao` 1—N `locacao_item` N—1 `produto_variacao`
* `produto` 1—N `produto_variacao` N—1 `categoria`
* `locacao` 1—N `pagamento`

> **Observação:** o diagrama ER completo está em `/docs/er/`.

---

## 🗃️ Estrutura do Repositório

```
LocaDB/
├─ docs/
│  ├─ er/
│  │  └─ locadb-er.vpd  (ou .png/.pdf)
│  └─ dicionario-dados.md
├─ sql/
│  ├─ 01_schema.sql
│  ├─ 02_constraints.sql
│  ├─ 03_seed.sql
│  ├─ 04_views.sql
│  ├─ 05_functions.sql
│  ├─ 06_triggers.sql
│  ├─ 07_roles.sql
│  └─ 99_teardown.sql
├─ scripts/
│  ├─ create_database.ps1
│  ├─ reset_database.ps1
│  └─ backup_restore_examples.sql
└─ README.md
```

---

## 🔧 Requisitos

* **PostgreSQL** ≥ 14
* **psql** (CLI) ou cliente gráfico (pgAdmin/DBeaver)
* Acesso local ou Docker (opcional)

---

## ▶️ Como Executar

1. **Criar a base:**

   ```bash
   psql -U postgres -c "CREATE DATABASE locadb ENCODING 'UTF8' TEMPLATE template1;"
   ```
2. **Rodar os scripts em ordem:**

   ```bash
   psql -U postgres -d locadb -f sql/01_schema.sql
   psql -U postgres -d locadb -f sql/02_constraints.sql
   psql -U postgres -d locadb -f sql/03_seed.sql
   psql -U postgres -d locadb -f sql/04_views.sql
   psql -U postgres -d locadb -f sql/05_functions.sql
   psql -U postgres -d locadb -f sql/06_triggers.sql
   psql -U postgres -d locadb -f sql/07_roles.sql
   ```
3. **(Opcional) Resetar tudo:**

   ```bash
   psql -U postgres -d locadb -f sql/99_teardown.sql
   ```

> **Dica:** Use uma transação por arquivo para garantir atomicidade e facilitar rollback em caso de erro.

---

## 🧱 Convenções de Modelagem

* **Nomes em snake\_case** (ex.: `produto_variacao`).
* **Chaves primárias** com `id` tipo `BIGSERIAL` ou `UUID`.
* **FKs** nomeadas como `fk_<tabela>__<referencia>`.
* **Índices** com `ix_<tabela>__<coluna>`; **índices únicos** com `ux_...`.
* **Restrições** (`CHECK`) para domínios como tamanhos (`PP,P,M,G,GG`), cores etc.
* **Timestamps**: `created_at`, `updated_at` (default `now()` / triggers).

---

## 🔐 Segurança e Acesso

* Criação de **roles**: `locadb_admin`, `locadb_app`, `locadb_readonly`.
* **Privilégios mínimos** por esquema/tabela/visão.
* **RLS (Row‑Level Security)** opcional para partição por filial/loja.

---

## 💾 Backup & Restore

Exemplos em `scripts/backup_restore_examples.sql`:

```sql
-- Backup (pg_dump)
-- pg_dump -U postgres -d locadb -F c -f locadb.backup

-- Restore (pg_restore)
-- pg_restore -U postgres -d locadb -c -1 locadb.backup
```

---

## 🔎 Consultas de Exemplo

```sql
-- Itens disponíveis por período
SELECT pv.id, p.nome, pv.tamanho, pv.cor
FROM produto_variacao pv
JOIN produto p ON p.id = pv.produto_id
WHERE NOT EXISTS (
  SELECT 1 FROM locacao_item li
  JOIN locacao l ON l.id = li.locacao_id
  WHERE li.produto_variacao_id = pv.id
    AND tstzrange(l.datahora_retirada_prevista, l.datahora_devolucao_prevista, '[)') &&
        tstzrange($1::timestamptz, $2::timestamptz, '[)')
);

-- Faturamento por mês
SELECT date_trunc('month', pagamento.data) AS mes,
       SUM(pagamento.valor) AS total
FROM pagamento
GROUP BY 1
ORDER BY 1;
```

---

## 🧪 Testes e Qualidade

* **Dados de teste** em `03_seed.sql` com cenários de reserva, atraso e dano.
* **Triggers** de auditoria em `06_triggers.sql` para rastrear updates críticos.
* **Validações** via `CHECK`, `NOT NULL`, `UNIQUE` e **FKs** em cascata controlada.

---

## 🛣️ Roadmap Sugerido

* [ ] RLS por filial
* [ ] Particionamento de tabelas de histórico
* [ ] Índices parciais para consultas de disponibilidade
* [ ] Relatórios materializados (REFRESH MATERIALIZED VIEW)
* [ ] Integração com app externo (API apenas leitura via `dblink`/FDW)

---

## 📄 Licença

Defina a licença do projeto (ex.: MIT). Inclua o arquivo `LICENSE` na raiz do repositório.

---
