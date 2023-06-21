DECLARE
    insert_id NUMBER;
    comp_exists NUMBER := 0;
BEGIN
    IF :P15_FUNCIONARIO IS NOT NULL THEN
        -- Executar o INSERT e o UPDATE
        INSERT INTO tarefa (
            id_empresa,
            id_departamento,
            nome_tarefa,
            descricao,
            urls,
            criado_por,
            atualizado_por,
            id_tipo_competencia,
            id_status_competencia,
            data_execucao,
            data_finalizacao,
            id_prioridade
        ) VALUES (
            (CASE WHEN to_number(:P15_SELECT_CLIENTE) != 0 THEN to_number(:P15_SELECT_CLIENTE) ELSE NULL END),
            :P15_SELECT_DEPARTAMENTO,
            :P15_NAME,
            :P15_DESCRICAO,
            :P15_URLS,
            :APP_USER,
            :APP_USER,
            :P15_TIPO_COMPETENCIA,
            1,
            CASE WHEN :P15_DATA_EXECUCAO IS NULL THEN trunc(add_months(sysdate, 1), 'mm') ELSE to_date(:P15_DATA_EXECUCAO) END,
            :P15_DATA_FINALIZACAO,
            :P15_SELECT_PRIORIDADE
        ) RETURNING id_tarefa INTO insert_id;

        FOR aq IN (
            SELECT c001    AS filename,
                c002    AS mime_type,
                d001    AS date_created,
                n001    AS file_id,
                blob001 AS file_content
            FROM apex_collections
            WHERE collection_name = 'DROPZONE_UPLOAD'
        ) LOOP
            INSERT INTO tarefa_anexo (
                id_tarefa,
                file_name,
                mimetype,
                file_data,
                criado_em,
                criado_por,
                atualizado_por
            ) VALUES (
                insert_id,
                aq.filename,
                aq.mime_type,
                aq.file_content,
                aq.date_created,
                :APP_USER,
                :APP_USER
            );
        END LOOP;
        
        -- Final Sicroniza a Tarefa com a Competência
        PROC_SYNC_COMPETENCIA(v_id_tarefa => insert_id, v_force => 0);
        
        UPDATE TAREFA_EMPRESA
        SET ID_RESPONSAVEL = :P15_FUNCIONARIO,
            ATUALIZADO_EM = to_char(sysdate, 'DD/MM/YYYY HH:MI:SS'),
            ATUALIZADO_POR = :APP_USER
        WHERE ID_TAREFA = insert_id;

    ELSE
        INSERT INTO tarefa (
            id_empresa,
            id_departamento,
            nome_tarefa,
            descricao,
            urls,
            criado_por,
            atualizado_por,
            id_tipo_competencia,
            id_status_competencia,
            data_execucao,
            data_finalizacao,
            id_prioridade
        ) VALUES (
            (CASE WHEN to_number(:P15_SELECT_CLIENTE) != 0 THEN to_number(:P15_SELECT_CLIENTE) ELSE NULL END),
            :P15_SELECT_DEPARTAMENTO,
            :P15_NAME,
            :P15_DESCRICAO,
            :P15_URLS,
            :APP_USER,
            :APP_USER,
            :P15_TIPO_COMPETENCIA,
            1,
            CASE WHEN :P15_DATA_EXECUCAO IS NULL THEN trunc(add_months(sysdate, 1), 'mm') ELSE to_date(:P15_DATA_EXECUCAO) END,
            :P15_DATA_FINALIZACAO,
            :P15_SELECT_PRIORIDADE
        ) RETURNING id_tarefa INTO insert_id;

        FOR aq IN (
            SELECT c001    AS filename,
                c002    AS mime_type,
                d001    AS date_created,
                n001    AS file_id,
                blob001 AS file_content
            FROM apex_collections
            WHERE collection_name = 'DROPZONE_UPLOAD'
        ) LOOP
            INSERT INTO tarefa_anexo (
                id_tarefa,
                file_name,
                mimetype,
                file_data,
                criado_em,
                criado_por,
                atualizado_por
            ) VALUES (
                insert_id,
                aq.filename,
                aq.mime_type,
                aq.file_content,
                aq.date_created,
                :APP_USER,
                :APP_USER
            );
        END LOOP;
        
        -- Final Sicroniza a Tarefa com a Competência
        PROC_SYNC_COMPETENCIA(v_id_tarefa => insert_id, v_force => 0);
    END IF;
END;

