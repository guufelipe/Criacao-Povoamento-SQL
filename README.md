# Projeto 1: Criação e Povoamento de Banco de Dados SQL

Este repositório contém o primeiro projeto desenvolvido para a disciplina de Banco de Dados, realizado em colaboração com outros colegas. O projeto foca na criação de um esquema de banco de dados, seu povoamento com dados de exemplo e a execução de consultas SQL.

O objetivo principal foi aplicar os conceitos de modelagem de dados e normalização, garantindo a integridade e eficiência da estrutura do banco.

## Estrutura do Repositório

A estrutura de pastas e arquivos está organizada da seguinte forma:

* `CriaçãoTabelas`: Arquivo SQL com os comandos DDL para a criação das tabelas do banco de dados.
* `Povoamento`: Arquivo SQL com os comandos DML para a inserção de dados nas tabelas.
* `consultas`: Arquivos SQL contendo diversas consultas para extrair informações do banco de dados.
* `README.md`: Este arquivo, com a descrição do projeto e o esquema do banco.

## Esquema do Banco de Dados Normalizado

A seguir, apresentamos o esquema do banco de dados, com a descrição das tabelas, chaves primárias (PK) e estrangeiras (FK), e a forma normal em que se encontram.

### Pessoa
- **Estrutura**: `Pessoa (cpf_pk, gênero, nascimento, nome, cep_fk, num_endereço_fk)`
- **Referências**: `cep`, `num_endereço` referencia `Endereço(cep, num_endereço)`
- **Forma Normal**: 3FN (Terceira Forma Normal). Todos os atributos dependem unicamente da chave primária `cpf`. A separação do `cep` para a tabela `Endereço` elimina dependências transitivas.

### Endereço
- **Estrutura**: `Endereço (cep_pk, num_endereço_pk, complemento, rua, bairro, cidade)`
- **Forma Normal**: 3FN. A chave primária é composta por `cep` e `num_endereço`, e todos os atributos dependem diretamente dela.

### Telefone
- **Estrutura**: `Telefone (cpf_p_fk_pk, telefone_pk)`
- **Referências**: `cpf_p` referencia `Pessoa(cpf)`
- **Forma Normal**: 1FN (Primeira Forma Normal). Elimina valores multivalorados (múltiplos telefones por pessoa), associando cada telefone a um `cpf_p`. Garante atomicidade e unicidade.

### Email
- **Estrutura**: `Email (cpf_e_fk_pk, email_pk)`
- **Referências**: `cpf_e` referencia `Pessoa(cpf)`
- **Forma Normal**: 1FN. Garante que não há grupos repetitivos e que os dados estão atômicos. Cada e-mail está associado a um CPF único.

### Cliente
- **Estrutura**: `Cliente (cpf_c_pk, cpf_indicador_fk)`
- **Referências**: `cpf_c` referencia `Pessoa(cpf)`. `cpf_indicador` referencia `Cliente(cpf_c)`
- **Forma Normal**: 3FN. A chave primária é `cpf_c`, e o único atributo adicional (`cpf_indicador`) depende diretamente dela. A estrutura evita redundâncias e garante integridade.

### Dependente
- **Estrutura**: `Dependente (cpf_responsavel_fk_pk, num_seq_pk, nome)`
- **Referências**: `cpf_responsavel` referencia `Cliente(cpf_c)`
- **Forma Normal**: 3FN. `cpf_responsavel` e `num_seq` formam a chave primária. `nome` depende totalmente dessa composição, sem transitividade.

### Funcionário
- **Estrutura**: `Funcionário (cpf_f_pk, titulo_cargo_fk, num_alugueis, turno, cpf_supervisionador_fk)`
- **Referências**: `cpf_f` referencia `Pessoa(cpf)`. `titulo_cargo` referencia `Cargo(título)`. `cpf_supervisionador` referencia `Funcionário(cpf_f)`.
- **Forma Normal**: 3FN. Atributos dependem da chave `cpf_f`. As referências a `Cargo` e a outro `Funcionário` (supervisor) evitam redundâncias e transitividades.

### Cargo
- **Estrutura**: `Cargo (título_pk, salário)`
- **Forma Normal**: 3FN. O atributo `salário` depende exclusivamente do `título`, que é a chave primária.

### Gerente
- **Estrutura**: `Gerente (cpf_g_fk_pk, data_promoção, num_subordinados)`
- **Referências**: `cpf_g` referencia `Funcionário(cpf_f)`
- **Forma Normal**: 3FN. `cpf_g` é a chave primária e os demais dados dependem totalmente dela.

### Produto
- **Estrutura**: `Produto (id_pk, titulo, tamanho, lançamento, estoque, qnt_alugada)`
- **Forma Normal**: 3FN. Todos os atributos dependem diretamente da chave `id`. Não há dependências transitivas.

### Gênero_produto
- **Estrutura**: `Gênero_produto (id_produto_fk_pk, genero_pk)`
- **Referências**: `id_produto` referencia `Produto(id)`
- **Forma Normal**: 1FN. Relacionamento N:N resolvido por tabela auxiliar. Uma entrada por linha, sem repetições.

### Produtora_produto
- **Estrutura**: `Produtora_produto (id_produto_fk_pk, produtora_pk)`
- **Referências**: `id_produto` referencia `Produto(id)`
- **Forma Normal**: 1FN. Cada linha representa um vínculo entre produto e produtora.

### Criadores_produto
- **Estrutura**: `Criadores_produto (id_produto_fk_pk, criadores_pk)`
- **Referências**: `id_produto` referencia `Produto(id)`
- **Forma Normal**: 1FN. Cada criador por linha evita repetições.

### Conta
- **Estrutura**: `Conta (num_pk, criação, credito, qnt_alugados, cpf_c_fk)`
- **Referências**: `cpf_c` referencia `Cliente(cpf_c)`
- **Forma Normal**: 3FN. Cada conta tem `num` como chave primária, com atributos diretamente dependentes.

### Bônus
- **Estrutura**: `Bônus (id_pk, valor)`
- **Forma Normal**: 3FN. A chave primária é `id`, e o atributo `valor` depende diretamente dele.

### Ganha
- **Estrutura**: `Ganha (cpf_c_fk_pk, num_fk_pk, id_fk_pk)`
- **Referências**: `cpf_c` e `num` referenciam `Cliente_conta(cpf_c, num)`. `id` referencia `Bônus(id)`.
- **Forma Normal**: 3FN. `cpf_c`, `num`, e `id` formam a chave. Não há dependências parciais.

### Avalia
- **Estrutura**: `Avalia (cpf_c_fk_pk, id_fk_pk, valor)`
- **Referências**: `cpf_c` referencia `Cliente(cpf_c)`. `id` referencia `Produto(id)`.
- **Forma Normal**: 3FN. `valor` depende da combinação de `cpf_c` e `id`.

### Avalia2
- **Estrutura**: `Avalia2 (cpf_responsavel_fk_pk, num_seq_fk_pk, id_fk_pk, valor)`
- **Referências**: `cpf_responsável` e `num_seq` referenciam `Dependente(cpf_resposavel, num_seq)`. `id` referencia `Produto(id)`.
- **Forma Normal**: 3FN. `valor` depende da chave composta.

### Aluga
- **Estrutura**: `Aluga (cpf_f_fk_pk, num_fk_pk, id_fk_pk, data_inicio, multa, preço, forma_pgmto, prazo_devolução)`
- **Referências**: `cpf_f` referencia `Funcionário(cpf_f)`. `num` referencia `Conta(num)`. `id` referencia `Produto(id)`.
- **Forma Normal**: 3FN. Chave composta entre `cpf_f`, `num` e `id`. Os demais atributos dependem totalmente dessa chave.

---

## Repositório Oficial e Colaboradores

Este projeto foi desenvolvido em equipe. Você pode encontrar o repositório original (utilizado durante a disciplina) e informações sobre os demais colaboradores no link abaixo.

* **Link do Repositório Oficial**: https://github.com/marcelaraposoo/Banco-de-Dados/blob/main/README.md

---