# Sistema de Despesas Mensais - Backend API
![image](https://github.com/user-attachments/assets/e5d8cf35-bce5-4dd3-ab8f-b0baf7a44959)

## Antes de tudo

**📁 [Repositório com PyInstaller configurado](https://github.com/sampconrad/sistema-despesas)** - este link lhe levará para o monorepo com build automatizado, onde a aplicação é compilada em um único arquivo .exe para facilitar a utilização do sistema.

## Descrição

API REST desenvolvida em Python com Flask para gerenciamento de despesas mensais. Esta aplicação permite cadastrar, visualizar, atualizar e excluir despesas com diferentes tipos de pagamento (CRÉDITO FIXO, CRÉDITO PARCELADO, PIX, BOLETO).

A API oferece documentação completa via Swagger/OpenAPI, permitindo testes interativos e compreensão clara de todos os endpoints disponíveis.

## Funcionalidades

- **Operações CRUD completas**: Create, Read, Update, Delete
- **Documentação robusta**: Swagger/OpenAPI com exemplos e códigos de status
- **Validações**: Regras de negócio e validação de dados
- **CORS configurado**: Suporte completo para frontend
- **Logs detalhados**: Sistema de logging para monitoramento

## Tecnologias Utilizadas

- **Python 3.8+**
- **Flask**
- **Flask-OpenAPI3**
- **SQLAlchemy**
- **SQLite**
- **Flask-CORS**
- **Pydantic**

## Estrutura do Projeto

```
sistema-despesas-api/
├── app.py                # Aplicação principal Flask
├── requirements.txt      # Dependências Python
├── README.md             # Este arquivo
├── database/             # Diretório do banco de dados
│   └── db.sqlite3        # Banco SQLite
├── log/                  # Logs da aplicação
│   └── gunicorn.detailed.log
├── logger.py             # Configuração de logs
├── model/                # Modelos de dados
│   ├── __init__.py
│   ├── base.py           # Configuração base do SQLAlchemy
│   └── despesa.py        # Modelo de despesas
└── schemas/              # Schemas Pydantic
    ├── __init__.py
    ├── despesa.py        # Schemas de despesas
    └── error.py          # Schema de erros
```
## API Endpoints Disponíveis

| Método | Endpoint             | Descrição                   |
|--------|----------------------|-----------------------------|
| POST   | `/despesa`           | Criar nova despesa          |
| GET    | `/despesas`          | Listar todas as despesas    |
| GET    | `/despesa?id={id}`   | Buscar despesa por ID       |
| PUT    | `/despesa`           | Atualizar despesa existente |
| DELETE | `/despesa?id={id}`   | Excluir despesa             |

## Instalação

### Pré-requisitos

- Python 3.8 ou superior
- pip (gerenciador de pacotes Python)
- O front-end pode ser encontrado em https://github.com/sampconrad/sistema-despesas-client

### Passos para Instalação

1. **Clone o repositório** (se aplicável):
   ```bash
   git clone https://github.com/sampconrad/sistema-despesas-api.git
   ```

2. **Crie um ambiente virtual** (recomendado):
   
   É fortemente indicado o uso de ambientes virtuais do tipo [virtualenv](https://virtualenv.pypa.io/en/latest/installation.html).

   ```bash
   python -m venv .venv
   ```

3. **Ative o ambiente virtual**:
   
   **Windows:**
   ```bash
   .venv\Scripts\activate
   ```
   
   **Linux/Mac:**
   ```bash
   source .venv/bin/activate
   ```

4. **Instale as dependências**:
   ```bash
   pip install -r requirements.txt
   ```
5. **Levante a API**:
   **Opção 1: Sem reload automático**
   ```bash
   flask run --host 0.0.0.0 --port 5000
   ```

   **Opção 2: Com reload automático (recomendado para desenvolvimento)**
   ```bash
   flask run --host 0.0.0.0 --port 5000 --reload
   ```
6. **Acesse a API**:
   - A API estará disponível em: `http://localhost:5000`

## Documentação da API

### Acessando a Documentação

Após iniciar a aplicação, acesse:
- **Swagger UI**: `http://localhost:5000/openapi`
- **ReDoc**: `http://localhost:5000/openapi/redoc`
- **RapiDoc**: `http://localhost:5000/openapi/rapidoc`

### Banco de Dados

O banco SQLite será criado automaticamente na primeira execução no diretório `database/`.

**Nota:** Se houver mudanças no modelo de dados, pode ser necessário deletar o arquivo `database/db.sqlite3` para recriar o banco.

### Estrutura de Dados

#### Despesa (Criação)

```json
{
  "tipo": "CRÉDITO FIXO|CRÉDITO PARCELADO|PIX|BOLETO",
  "titulo": "string",
  "valor": 0.0,
  "dia_vencimento": 15,
  "parcelas": 12,
  "paga": false
}
```

#### Despesa (Resposta)

```json
{
  "id": 1,
  "tipo": "CRÉDITO FIXO",
  "titulo": "Cartão de Crédito Nubank",
  "valor": 1500.75,
  "parcelas": null,
  "dia_vencimento": 15,
  "paga": false,
  "data_insercao": "05/07/2025 19:17"
}
```

## Códigos de Status HTTP

- **200**: Sucesso - Operação realizada com sucesso
- **400**: Bad Request - Dados inválidos ou malformados
- **404**: Not Found - Recurso não encontrado
- **409**: Conflict - Conflito de dados (ex: violação de integridade)
- **500**: Internal Server Error - Erro interno do servidor

## Validações e Regras de Negócio

### Tipos de Despesa
- **CRÉDITO FIXO**: Despesas recorrentes com valor fixo
- **CRÉDITO PARCELADO**: Despesas parceladas com controle de parcelas restantes
- **PIX**: Pagamentos via PIX
- **BOLETO**: Pagamentos via boleto bancário

### Validações
- **tipo**: Deve ser um dos valores válidos
- **valor**: Deve ser maior que zero
- **dia_vencimento**: Deve estar entre 1 e 31
- **parcelas**: Deve ser positivo (apenas para CRÉDITO PARCELADO)

### Regras de Negócio
- Se o tipo for alterado para algo diferente de CRÉDITO PARCELADO, parcelas será zerado
- Pelo menos um campo opcional deve ser fornecido para atualização

## Troubleshooting

### Problemas Comuns

1. **Erro 422 Unprocessable Entity**: Verifique se está enviando FormData (não JSON)
2. **Erro de CORS**: Certifique-se de que o CORS está configurado corretamente
3. **Erro de banco de dados**: Delete o arquivo `database/db.sqlite3` para recriar

### Comandos Úteis

```bash
# Verificar dependências instaladas
pip list

# Atualizar dependências
pip install -r requirements.txt --upgrade

# Verificar logs
tail -f log/gunicorn.detailed.log
```

## Licença

Este projeto está sob a licença MIT. Veja o arquivo LICENSE para mais detalhes.
