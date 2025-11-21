# LocaDB - Sistema de Gerenciamento de Locação de Roupas

<div align="center">
  
  ![LocaDB](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow?style=for-the-badge)
  ![Database](https://img.shields.io/badge/Database-PostgreSQL-336791?style=for-the-badge&logo=postgresql)
  ![License](https://img.shields.io/badge/License-MIT-green?style=for-the-badge)

</div>

## 📋 Sobre o Projeto

LocaDB é um sistema de gerenciamento de locação de roupas desenvolvido como projeto da disciplina de **Projeto de Banco de Dados**. O sistema gerencia clientes, produtos (roupas), pedidos de locação, vendedores, transportadoras e fornecedores de forma integrada e eficiente.

### 🎬 Demonstração

#### Sistema em Ação
![Demo do Sistema](assets/demo.gif)

#### Estrutura do Banco de Dados
![Banco de Dados](assets/database.gif)

#### Consultas SQL
![Consultas](assets/queries.gif)

## 🎯 Funcionalidades Principais

- ✅ Cadastro e gerenciamento de clientes
- ✅ Controle de inventário de roupas
- ✅ Gestão de pedidos de locação
- ✅ Administração de vendedores
- ✅ Controle de transportadoras
- ✅ Gerenciamento de fornecedores
- ✅ Relatórios e consultas avançadas

## 🗄️ Estrutura do Banco de Dados

### Tabelas Principais

| Tabela | Descrição |
|--------|-----------|
| `cliente` | Dados dos clientes (nome, CPF, RG, endereço, profissão) |
| `produto` | Roupas disponíveis para locação |
| `pedido` | Registros de locações realizadas |
| `pedido_produto` | Itens de cada pedido |
| `vendedor` | Vendedores do sistema |
| `transportadora` | Empresas de transporte |
| `fornecedor` | Fornecedores de roupas |
| `profissao` | Profissões dos clientes |
| `nacionalidade` | Nacionalidades |
| `municipio` | Municípios |
| `uf` | Estados |
| `bairro` | Bairros |
| `complemento` | Tipos de complemento de endereço |

## 🚀 Como Usar

### Pré-requisitos

- PostgreSQL 12+
- Git

### Instalação

1. Clone o repositório:
```bash
git clone https://github.com/FELIPE-NOBREGA06/LocaDB-Sistema-de-Gerenciamento-de-Loca-o-de-Roupas.git
cd LocaDB-Sistema-de-Gerenciamento-de-Loca-o-de-Roupas
```

2. Crie um banco de dados:
```bash
createdb locadb
```

3. Execute o script SQL:
```bash
psql -U seu_usuario -d locadb -f "Script curso.sql"
```

## 📊 Exemplos de Consultas

### Listar todos os clientes
```sql
SELECT * FROM cliente;
```

### Pedidos entre datas específicas
```sql
SELECT * FROM pedido 
WHERE data_pedido BETWEEN '2008-04-10' AND '2008-04-25' 
ORDER BY valor;
```

### Clientes por município
```sql
SELECT c.nome, m.nome as municipio 
FROM cliente c
JOIN municipio m ON c.idmunicipio = m.idmunicipio;
```

### Total de vendas por vendedor
```sql
SELECT v.nome, SUM(p.valor) as total_vendas
FROM vendedor v
JOIN pedido p ON v.idvendedor = p.idvendedor
GROUP BY v.nome
ORDER BY total_vendas DESC;
```

## 📁 Estrutura do Projeto

```
LocaDB/
├── Script curso.sql          # Script principal do banco de dados
├── README.md                 # Este arquivo
└── docs/                     # Documentação adicional (opcional)
```

## 🎓 Disciplina

- **Curso:** Projeto de Banco de Dados
- **Instituição:** UNIVAG 
- **Período:** 2025/2

## 👨‍💻 Autor

**Cleverson Felipe Nobrega Dos santos**
- GitHub: [@FELIPE-NOBREGA06](https://github.com/FELIPE-NOBREGA06)

## 📝 Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.

## 🤝 Contribuições

Contribuições são bem-vindas! Sinta-se à vontade para abrir issues e pull requests.

---

<div align="center">
  
  Desenvolvido com ❤️ para a disciplina de Projeto de Banco de Dados
  
</div>
