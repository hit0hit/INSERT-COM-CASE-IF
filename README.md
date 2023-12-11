# INSERT-COM-CASE-IF
Insert com case if / para inserir um ou outo, 
Este bloco PL/SQL parece ser responsável por inserir dados em duas tabelas (`tarefa` e `tarefa_anexo`) e atualizar a tabela `TAREFA_EMPRESA`. Vamos analisar as principais partes do código:

1. **Declaração de Variáveis:**
   ```plsql
   DECLARE
       insert_id NUMBER;
       comp_exists NUMBER := 0;
   BEGIN
   ```
   - `insert_id`: Variável que armazenará o ID da tarefa inserida.
   - `comp_exists`: Variável inicializada com 0, mas não é usada no restante do código.

2. **Condição IF:**
   ```plsql
   IF :P15_FUNCIONARIO IS NOT NULL THEN
       -- Código executado se o item P15_FUNCIONARIO não for nulo
   ELSE
       -- Código executado se o item P15_FUNCIONARIO for nulo
   END IF;
   ```
   - O código verifica se o valor do item `P15_FUNCIONARIO` não é nulo.
   - Executa diferentes blocos de código dependendo da condição.

3. **Inserção na Tabela `tarefa`:**
   ```plsql
   INSERT INTO tarefa (
       -- colunas
   ) VALUES (
       -- valores
   ) RETURNING id_tarefa INTO insert_id;
   ```
   - Insere dados na tabela `tarefa` e recupera o ID da tarefa inserida na variável `insert_id`.

4. **Loop FOR para Inserção na Tabela `tarefa_anexo`:**
   ```plsql
   FOR aq IN (
       -- SELECT da coleção 'DROPZONE_UPLOAD'
   ) LOOP
       INSERT INTO tarefa_anexo (
           -- colunas
       ) VALUES (
           -- valores
       );
   END LOOP;
   ```
   - Itera sobre os registros da coleção 'DROPZONE_UPLOAD' e insere dados na tabela `tarefa_anexo` para cada registro.

5. **Procedimento `PROC_SYNC_COMPETENCIA`:**
   ```plsql
   PROC_SYNC_COMPETENCIA(v_id_tarefa => insert_id, v_force => 0);
   ```
   - Chama um procedimento chamado `PROC_SYNC_COMPETENCIA` para sincronizar a tarefa com a competência.

6. **Atualização na Tabela `TAREFA_EMPRESA`:**
   ```plsql
   UPDATE TAREFA_EMPRESA
   SET
       -- colunas
   WHERE
       ID_TAREFA = insert_id;
   ```
   - Atualiza a tabela `TAREFA_EMPRESA` com informações relacionadas à tarefa.

