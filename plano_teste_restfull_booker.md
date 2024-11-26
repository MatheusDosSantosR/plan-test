
# Plano de Teste para API Restful-Booker

---

## 1. Introdução

O objetivo deste plano de teste é validar os principais endpoints da API Restful-Booker, um sistema de reservas de hotel, garantindo sua conformidade funcional e comportamento esperado antes da integração com o front-end.

---

## 2. Escopo

Os testes abrangem:

- Autenticação
- Gestão de reservas (CRUD)
- Filtros e buscas

Não serão testados aspectos como desempenho, segurança avançada ou integração com outros sistemas nesta etapa.

---

## 3. Ferramentas

- **Postman**: Para criação e execução de testes automatizados e organização de requests.
- **Variáveis de ambiente**: Configuração para endpoints, tokens e IDs dinâmicos.
- **Documentação da API**: Base para criar os cenários de teste.

---

## 4. Estratégia de Teste

### 4.1 Tipos de Testes

1. **Funcional**: Garantir que os endpoints executem suas funções conforme especificado.
2. **Negativo**: Verificar tratamento de erros com dados inválidos ou incompletos.
3. **Validação de resposta**: Confirmar códigos HTTP, mensagens de erro e estruturas de dados.

### 4.2 Testes Automatizados

- Um teste será implementado para cada cenário descrito.
- Respostas serão validadas com assertions no Postman.

---

## 5. Cenários de Teste

### 5.1 Autenticação

| ID  | Cenário                                                | Dados de Entrada                            | Resultado Esperado         | Resultado Real              | Status  |
| --- | ------------------------------------------------------ | ------------------------------------------- | -------------------------- | --------------------------- | ------- |
| 1.1 | Gerar token de autenticação                            | Credenciais válidas                         | Código 200, token gerado   | Conforme esperado           | Sucesso |
| 1.2 | Gerar token com credenciais inválidas                  | Credenciais inválidas                       | Código 401, erro retornado | E retornado status code 200 | Falha   |
| 1.2 | Gerar token com credenciais contendo muitos caracteres | Credenciais contendo mais de 200 caracteres | Código 401, erro retornado | E retornado status code 200 | Falha   |

### 5.2 Gestão de Reservas

#### Listagem de reservas

| ID  | Cenário                                 | Dados de Entrada | Resultado Esperado                             | Resultado Real                        | Status  |
| --- | --------------------------------------- | ---------------- | ---------------------------------------------- | ------------------------------------- | ------- |
| 2.1 | Listar todas as reservas                | -                | Código 200, lista retornada                    | Conforme esperado                     | Sucesso |
| 2.2 | Listar reserva espeficia                | -                | Código 200, dados da reserva                   | Conforme esperado                     | Sucesso |
| 2.3 | Listar reserva espeficia que não existe | -                | Código 404, mensagem de reserva não encontrada | Não retorna mensagem com formato Json | Falha   |

#### Cadastro de reserva

| ID  | Cenário                                  | Dados de Entrada       | Resultado Esperado                                          | Resultado Real                               | Status |
| --- | ---------------------------------------- | ---------------------- | ----------------------------------------------------------- | -------------------------------------------- | ------ |
| 2.4 | Criar uma nova reserva                   | Payload válido         | Código 201, dados da reserva retornados com o ID            | É retornado status code 200 O ideal e 201    | Falha  |
| 2.5 | Criar reserva com dados inválidos        | ID e payload invalidos | Código 400, com mensagem de `Campo x com formato incorreto` | A reserva e criada e retornado status 200    | Falha  |
| 2.6 | Criar reserva faltando dados obrigatorio | Faltando username      | Código 400, com mensagem `Campo x é obrigatorio`            | É retornado status code 500 erro do servidor | Falha  |

#### Atualização de reserva

| ID   | Cenário                                          | Dados de Entrada       | Resultado Esperado                                 | Resultado Real                                                              | Status  |
| ---- | ------------------------------------------------ | ---------------------- | -------------------------------------------------- | --------------------------------------------------------------------------- | ------- |
| 2.7  | Atualizar uma reserva                            | Payload válido         | Código 200, ID retornado                           |                                                                             | Sucesso |
| 2.8  | Atualizar uma reserva com dados inválidos        | ID e payload inválidos | Código 400, mensagem de `Campo x invalido`         | E atualizado e o campo data retorna `0NaN-aN-aN`, demais campos retorna 500 | Falha   |
| 2.9  | Atualizar uma reserva faltando campo obrigatorio | ID e payload válidos   | Código 400, mensagem de `O campo x é obrigatorio.` | Não retorna mensagem com formato Json                                       | Falha   |
| 2.10 | Atualizar uma reserva com dados ID inválido      | ID e payload válidos   | Código 404, mensagem de `ID não encontrado`        | Retorna status code 400, e não retorna mensagem com formato Json            | Sucesso |

#### Exclusão de reserva

| ID   | Cenário                            | Dados de Entrada | Resultado Esperado                                | Resultado Real                                      | Status |
| ---- | ---------------------------------- | ---------------- | ------------------------------------------------- | --------------------------------------------------- | ------ |
| 2.11 | Deletar uma reserva                | ID válido        | Código 200, reserva deletada                      | É retornado status code 201, é não retorna um Json  | Falha  |
| 2.12 | Deletar uma reserva que não existe | ID inválido      | Código 404, mensagem de `Reserva não encontrada.` | É retornado status 405, sem conter mensagem em Json | Falha  |

### 5.3 Filtros e Buscas

| ID  | Cenário                                                            | Dados de Entrada            | Resultado Esperado         | Resultado Real                                | Status  |
| --- | ------------------------------------------------------------------ | --------------------------- | -------------------------- | --------------------------------------------- | ------- |
| 3.1 | Buscar reservas por nome                                           | Nome do hóspede             | Código 200, lista filtrada | Conforme esperado                             | Sucesso |
| 3.2 | Buscar reservas por nome e sobrenome                               | Nome e sobrenome do hóspede | Código 200, lista filtrada | Conforme esperado                             | Sucesso |
| 3.3 | Filtrar reservas por data de check-in                              | Data válida                 | Código 200, lista filtrada | É retornado um ID que não faz parte do filtro | Falha   |
| 3.4 | Filtrar reservas por data de check-out                             | Data válida                 | Código 200, lista filtrada | É retornado todas as reservas sem filtros     | Falha   |
| 3.5 | Buscar reservas com nome, sobrenome e filtros check-in e check-out | Todos os dados válidos      | Código 200, lista filtrada | Não e encontrada a reserva                    | Falha   |

---

## 6. Itens de Teste

Os endpoints testados incluirão:

1. `/auth` para autenticação
2. `/booking` para gestão de reservas
3. Filtros adicionais utilizando parâmetros de query

---

## 7. Critérios de Aceitação

1. Todos os endpoints devem retornar os resultados esperados conforme os cenários descritos.
2. Os códigos HTTP e mensagens de erro devem estar em conformidade com a documentação.
3. O tratamento de erros deve ser consistente e significativo.

---

## 8. Riscos e Dependências

### Riscos

- Falhas na conexão com o ambiente de teste.
- Alterações inesperadas na API durante os testes.

### Dependências

- Documentação precisa e ambiente de teste funcional.
- Credenciais válidas para autenticação.

---

## 9. Entregáveis

1. **Coleção do Postman** contendo:
   - Requests organizados. - OK
   - Testes automatizados para cada cenário. - OK
   - Configuração de variáveis de ambiente. - OK
2. **Relatório Markdown**:
   - Resultados obtidos. - OK
   - Lista de bugs encontrados (se houver) - Concluindo

---

## 10. Lista de Bugs

### Bug 001: Rota de exclusão retorna 201

- **Descrição**: Rota de exclusão deveria retornar status code 200 ou 204 e não 201, pois não esta sendo criado nada no banco de dados e sim removido.
- **Severidade**: Crítico
- **Prioridade**: Alta
- **Passos**:
  1. Crie uma reserva.
  2. Envie requisição do tipo DELETE para rota `/booking/:id`.
- **Resultado Esperado**: Mensagem clara informando "Excluido com sucesso." com status code 200.
- **Resultado Real**: É retornado status code 201, é não retorna um JSON valido.
- **Evidência**:
  ![Erro no Login](img/carrinho-vazio.png)

  ### Bug 002: Gerar token com credenciais inválidas retorna 200

- **Descrição**: Rota de autenticação com credenciais invalida retorna status code incorreto
- **Severidade**: Normal
- **Prioridade**: Normal
- **Passos**:
  1. Enviar requisição com credenciais `username` ou `password` inválidas.
- **Resultado Esperado**: Mensagem clara informando "Usuario ou senha incorreto." com status code 401.
- **Resultado Real**: É retornado status code 200, é não retorna um JSON valido.
- **Evidência**:
  ![Erro no Login](img/carrinho-vazio.png)

---

## Observações

- Na criação de reserva `POST /booking`deveria retornar status code 201, pois esta adicionado informações ao banco de dados.
- As rotas também não precisam de autenticação.

## Postman

  [Collection Postman](postman/Restfull%20booker.postman_collection.json)
  [Variaveis de ambiente Postman](postman/Env%20restFull%20booker.postman_environment.json)
