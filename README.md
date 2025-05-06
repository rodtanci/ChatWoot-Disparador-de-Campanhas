# 🚀 Disparador de Campanhas via WhatsApp [SAAS]🚀 

## 🌟 Introdução 🌟

O disparador de campanhas oferece uma forma eficiente e eficaz de gerenciar e enviar mensagens de campanha utilizando o ChatWoot. Esta solução é projetada para facilitar a comunicação com seus clientes, permitindo agendamento de campanhas, envio de mensagens personalizadas e acompanhamento de desempenho.

## Funcionalidades

### 📱 Campanhas dentro do ChatWoot

Integração completa com o ChatWoot para gerenciar suas campanhas diretamente dentro da plataforma.

### ⏰ Agendamento e Disparo Imediato

- Agende suas campanhas para uma data e hora específicas.
- Opção para disparo imediato, permitindo envios em tempo real.

### 🖼️ Envio de Imagem

- Possibilidade de incluir imagens nas mensagens da campanha.
- Suporte para múltiplos formatos de imagem (JPEG e PNG).
- Usar a variavel &img=linkdaimagem.jpg

### 📝 Envio de PDF

- Possibilidade de incluir PDF junto com uma mensagem.
- Usar a variavel &doc=linkdopdf.pdf

### 🎬 Envio de Video

- Possibilidade de incluir video nas mensagens da campanha.
- Usar a variavel &vid=linkdovideo.mp4

### 🔊 Envio de Audio

- Possibilidade de incluir audio nas mensagens da campanha.
- Usar a variavel &audio=linkdoaudio.mp3

### 📊 Limitador de Envios Diário por Empresa

- Defina um limite diário de envios para cada empresa.
- Evite sobrecarregar a utilização da VPS com o alto fluxo de mensagens.

### ⏳ Temporizador Randômico entre os Envios

- Adicione um temporizador randômico entre os envios para evitar detecção como spam e bloquear o número.
- (Lembrando que isso não é uma garantia de que você não possa perder seu número e sim uma solução para reduzir os riscos.)

### 🔄 Contador de Envios e Falhas

- Conta número de envios ✅.
- Conta número de falhas ❌.

### 🚫 Mensagem de Excesso de Limite Diário

- Envio automático de uma mensagem quando o limite diário de envios for excedido.
- Mantenha os administradores informados sobre o status das campanhas.

### 📋 Mensagem de Relatório da Campanha

- Receba uma mensagem avisando que o disparo da campanha foi iniciado.
- Receba um relatório detalhado ao final de cada campanha.
- Inclui número total de envios e falhas.
- Inclui número de disparos que restam no dia.
- Inclui os números que não enviaram com o nome do contato.

### 📝 Personalização com Nome e Email do Contato

- Use a variável (&nome) para personalizar as mensagens com o nome do contato.
- Use a variável (&email) para personalizar as mensagens com o email do contato.
- Aumente a personalização e eficácia das suas mensagens de campanha.

### 🏷️ Disparos através das Etiquetas dos Contatos

- Utilize as etiquetas dos contatos para segmentar e direcionar suas campanhas de maneira eficiente.
- Agrupe contatos com base em características específicas e envie mensagens direcionadas.

## 🎉 Benefícios 🎉

- **Automatização**: Reduza o trabalho manual com a automação de campanhas. 🤖
- **Personalização**: Melhore a experiência do cliente com mensagens personalizadas. 🎯
- **Eficiência**: Acompanhe o desempenho em tempo real e otimize suas campanhas. 📊
- **Integração**: Dispare campanhas diretamente pelo ChatWoot sem precisar abrir outro sistema, otimizando tempo e recursos. 🚀

---

## 📘 Tutorial de Automação de Disparo de Campanhas

Vamos começar o tutorial para fazer a automação do sistema de disparo de campanhas usando o n8n e a Evolution API junto ao ChatWoot. 

Antes de iniciar, certifique-se de que você já tem instalado:

- ChatWoot
- n8n
- Evolution API
- pgAdmin ou outro de sua preferência para acessar o banco de dados do Postgres

### Passo 1: Criar uma Caixa de Entrada de Canal SMS do Tipo Bandwidth

1. **Acesse o ChatWoot**: Faça login na sua conta do ChatWoot.
2. **Configurações**: Vá para a seção de configurações.
3. **Caixas de Entrada**: Selecione "Caixas de Entrada" no menu.
4. **Adicionar Nova Caixa de Entrada**: Clique no botão "Adicionar Nova Caixa de Entrada".
5. **Escolher Tipo de Canal**: Selecione "SMS" e escolha "Bandwidth" como o tipo de canal.
6. **Configurar Detalhes do Canal**:
   - Nome da Caixa de Entrada: Disparador (ou o nome que preferir).
   - Número de telefone: +741963 (NUMERAÇÃO ALEATÓRIA)
   - ID da Conta: 1 (ID DA CAIXA DE ENTRADA CADASTRADA NO CHATWOOT QUE VAI MANDAR AS MENSAGENS)
   - ID da aplicação: Instancia (NOME DA INSTACIA NA EVOLUTION)
   - Chave API: +5511934849643 (NUMERO DE WHATSAPP PARA RECEBER O RELATORIO)
   - Chave secreta API: email@exemplo.com (EMAIL PARA RECEBER O RELATORIO)
7. **Salvar Configurações**: Clique em "Criar canal Bandwidth" para criar a nova caixa de entrada.

### Passo 2: Adicionar Colunas no Banco de Dados do ChatWoot

1. **Acesse o Banco de Dados**: Use o pgAdmin ou outro software de sua preferência para acessar o banco de dados do ChatWoot.
2. **Adicionar Coluna na Tabela Accounts**:
   - Execute o seguinte comando SQL para adicionar a coluna `limite_disparo`:
     ```sql
     ALTER TABLE accounts
     ADD COLUMN limite_disparo INTEGER NOT NULL DEFAULT 500;
     ```
3. **Adicionar Colunas na Tabela campaigns**:
   - Execute os seguintes comandos SQL para adicionar as colunas status_envia, enviou e falhou na tabela campaigns:
     ```sql
     ALTER TABLE campaigns
     ADD COLUMN status_envia INTEGER NOT NULL DEFAULT 0;
     
     ALTER TABLE campaigns
     ADD COLUMN enviou INTEGER NOT NULL DEFAULT 0;
     
     ALTER TABLE campaigns
     ADD COLUMN falhou INTEGER NOT NULL DEFAULT 0;
     ```
4. **Adicionar Coluna na Tabela tags**:
   - Execute o seguinte comandos SQL para adicionar as coluna account_id na tabela tags:
     ```sql
     ALTER TABLE IF EXISTS public.tags
        ADD COLUMN account_id integer,
        ADD COLUMN labels_id bigint;
     ```
5. **Adicionar nova Tabela para guardar os envios que falharem**:
   - Execute o seguinte comando SQL para adicionar a tabela campaigns_failled:
     ```sql
      -- Cria a sequência
      CREATE SEQUENCE campaigns_failled_id_seq;
      
      -- Cria a tabela com a coluna `id` usando a sequência criada
      CREATE TABLE campaigns_failled (
        id BIGINT PRIMARY KEY NOT NULL DEFAULT nextval('campaigns_failled_id_seq'::regclass),
        nomecontato TEXT NOT NULL,
        telefone CHARACTER VARYING NOT NULL,
        id_campanha INTEGER NOT NULL
      );
     ```

## 🛠️ OBRIGATORIO ❗ - 🚨 CORREÇÃO NO BANCO DE DADOS DO CHATWOOT ⚠️
### Após aplicar esta correção é recomendavel recriar as etiquetas (marcadores).

- Foi notado que os ID da tabela "labels" não condizia com os id ta tabela "tags" sendo assim criei algumas funções e triggers que corrigem esse problema.

6. **Criação das Funções de Replicação, Exclusão e Atualização**

   ***Cria na raiz do banco de dados***
   
      **Função para replicar inserções:**
      
      ```sql
      CREATE OR REPLACE FUNCTION public.replicate_label_to_tags()
         RETURNS trigger
         LANGUAGE 'plpgsql'
         COST 100
         VOLATILE NOT LEAKPROOF
      AS $BODY$
      BEGIN
      -- Verifica se já existe um registro em tags para este label_id
         IF NOT EXISTS (
         SELECT 1 
         FROM public.tags
         WHERE labels_id = NEW.id
         ) THEN
         INSERT INTO public.tags (labels_id, name, account_id)
         VALUES (NEW.id, NEW.title, NEW.account_id);
         END IF;
         RETURN NEW;
      END;
      $BODY$;

      ALTER FUNCTION public.replicate_label_to_tags()
         OWNER TO postgres;
      ```
      
      **Função para replicar exclusões:**
      
      ```sql
      CREATE OR REPLACE FUNCTION public.delete_label_from_tags()
         RETURNS trigger
         LANGUAGE 'plpgsql'
         COST 100
         VOLATILE NOT LEAKPROOF
      AS $BODY$
      BEGIN
         DELETE FROM public.tags
         WHERE labels_id = OLD.id;

         RETURN OLD;
      END;
      $BODY$;

      ALTER FUNCTION public.delete_label_from_tags()
         OWNER TO postgres;
      ```
      
      **Função para replicar atualizações:**
      
      ```sql
      CREATE OR REPLACE FUNCTION public.update_label_to_tag()
          RETURNS trigger
          LANGUAGE 'plpgsql'
          COST 100
          VOLATILE NOT LEAKPROOF
      AS $BODY$
      BEGIN
          UPDATE public.tags
          SET name = NEW.title,
              account_id = NEW.account_id  -- opcional, se quiser atualizar também a conta
          WHERE labels_id = NEW.id;
          
          RETURN NEW;
      END;
      $BODY$;
      
      ALTER FUNCTION public.update_label_to_tag()
          OWNER TO postgres;
      ```

6. **Criação dos Indices e Triger**

   
      **Esse índice garante que não possa haver duas tags com o mesmo nome (name) dentro da mesma conta (account_id). Ou seja, é uma restrição de unicidade por conta, impedindo duplicações acidentais.**
      
      ```sql
      CREATE UNIQUE INDEX IF NOT EXISTS index_tags_on_name_account
          ON public.tags USING btree
          (name COLLATE pg_catalog."default" ASC NULLS LAST, account_id ASC NULLS LAST)
          TABLESPACE pg_default;
      ```
      
      **Sempre que uma nova etiqueta (label) for criada, deletada ou removida no Chatwoot, esse gatilho automaticamente chama uma função que copia ou sincroniza essa etiqueta com a tabela de tags, que provavelmente é usada de forma mais genérica no sistema (como busca, filtros, etc.).**
      
      ```sql
      CREATE OR REPLACE TRIGGER trg_delete_label_from_tags
          AFTER DELETE
          ON public.labels
          FOR EACH ROW
          EXECUTE FUNCTION public.delete_label_from_tags();

      CREATE OR REPLACE TRIGGER trigger_replicate_label_to_tags
          AFTER INSERT
          ON public.labels
          FOR EACH ROW
          EXECUTE FUNCTION public.replicate_label_to_tags();
      
      CREATE OR REPLACE TRIGGER trigger_update_label_to_tag
          AFTER UPDATE 
          ON public.labels
          FOR EACH ROW
          EXECUTE FUNCTION public.update_label_to_tag();
      ```

---

### Passo 3: Importar Workflows no n8n

1. **Acesse o n8n**: Faça login na sua instância do n8n.
2. **Adicionar Novo Workflow**:
   - Clique em "Add Workflow".
3. **Importar Workflow**:
   - Clique nos três pontinhos no canto superior direito.
   - Selecione "Import from File".
4. **Importar o Fluxo Disparador**:
   - Importe o arquivo de workflow disparador.json.
5. **Importar o Fluxo Reset-Limite-Campanhas**:
   - Repita os passos acima e importe o reset-limite-campanha.json.

### Passo 4: Editar o Workflow Disparador no n8n

1. **Acesse o Workflow Disparador**: No n8n, abra o workflow Disparador que você importou.
2. **Editar o primeiro nó do postgres Buscar campanhas**
   - Alterar na linha "select * from campaigns c  where campaign_type = 1  and status_envia = 0 and account_id = 1"
   - altere o valor e account_id para o ID da conta do chatwoot.
4. **Editar Nó Info_Base**:
   - Preencha os seguintes campos com suas informações:
     - **URL do ChatWoot**
     - **URL da Evolution API**
     - **URL do view Typebot**
     - **Token de acesso da conta do ChatWoot**
     - **Global API KEY da Evolution API**
     - **Email que vai enviar o relatório**
5. **Conectar Nós do Postgres ao Banco de Dados do ChatWoot**:
   - Conecte todos os nós do Postgres ao banco de dados do ChatWoot, garantindo que as informações fluam corretamente entre os sistemas.

### Passo 5: Editar o Workflow reset-limite-campanha no n8n

1. **Acesse o Workflow reset-limite-campanha**: No n8n, abra o workflow reset-limite-campanha que você importou.
2. **Conectar Nós do Postgres ao Banco de Dados do ChatWoot**:
   - Conecte todos os nós do Postgres ao banco de dados do ChatWoot, garantindo que as informações sejam atualizadas corretamente para resetar o limite de disparo diário.
   - Se desejar altere o limite de envio diario dentro do nó do postgres.
   - Recomendação para não sobrecarregar o worflow 500 disparos seguidos.

---

Agora que os workflows foram importados, configurados e editados, sua automação de disparo de campanhas está pronta para funcionar!

## 📢 Cadastro de uma Nova Campanha no ChatWoot

Para criar uma campanha, siga os passos abaixo:

1. **Clique em Campanhas**: Acesse a seção de Campanhas no ChatWoot.
2. **Clique em Única**: Selecione a opção "Única".
3. **Clique em Criar uma campanha única**: Adicione as informações da sua campanha.
   - **Título**: Insira o título da campanha.
   - **Mensagem**: Digite a mensagem que quer enviar na campanha.
     - Para adicionar o nome do contato à mensagem, digite `&nome`.
     - Para adicionar o email do contato à mensagem, digite `&email`.
     - Para adicionar uma imagem, digite `&img=urldaimagemaqui.jpg`.
     - Para adicionar um video, digite `&vid=urldovideo.mp4`.
     - Para adicionar um pdf, digite `&doc=urldopdf.pdf`.

### Exemplo de uso imagem:

"_Olá &nome, tudo bem?_
_Você já viu esta bela paisagem?_

_&img=https://img.freepik.com/fotos-gratis/paisagem-de-nevoeiro-matinal-e-montanhas-com-baloes-de-ar-quente-ao-nascer-do-sol_335224-794.jpg_"

### Exemplo de uso pdf:

"_Olá &nome, tudo bem?_
_Segue o nosso catalogo em PDF?_

_&doc=https://evolution-api.com/files/evolution-api.pdf_"

### Exemplo de uso video:

"_Olá &nome, tudo bem?_
Você  já viu esse vídeo??_

_&vid=https://evolution-api.com/files/video.mp4_"

4. **Selecionar Caixa de Entrada**: No campo "Selecionar caixa de entrada", selecione a caixa de SMS que você criou no início do tutorial.
5. **Público**: Selecione a etiqueta que está atribuída aos contatos que quer disparar a campanha.
6. **Horário Agendado**: Selecione o dia e a hora que quer disparar a campanha. Caso queira disparar a campanha na mesma hora, selecione a data e a hora atual.
7. **Clique em Criar**: Finalize a criação da campanha.

Agora tudo está pronto para enviar a sua campanha!

---

## 📅 Roadmap do Projeto

### Versão 1.1 🚀

**Mensagem de Início de Disparo**
- Implementar uma mensagem que indique o início do disparo da campanha para notificar os usuários sobre o processo inicial.

**Correção no limitador**
- Correção no limitador de disparo, foi encontrado um erro que ele rouba 1 limite da conta quando chega em 1.

### Versão 1.2 💡

**Relatório de Finalização por Email**
- Adicionar funcionalidade para enviar um relatório por email ao final da campanha, detalhando métricas e resultados.

**Inclusão do Limite de Disparos Diários no Relatório**
- Introduzir no relatório o número restante de disparos diários disponíveis, fornecendo transparência sobre os limites e uso.

### Versão 1.3 📊

**Adicionando variável**
- Vamos adicionar a variável &email para conseguir personalizar mais ainda a mensagem.

### Versão 1.4 🚨

**Relatório de Falhas de Envio**
- Desenvolver um relatório específico que liste os contatos que falharam no envio, incluindo nome e telefone, para análise e correção.

### Versão 1.5 📑

**Envio de PDF**
- Incluir a inserção de um arquivo pdf dentro da campanha.
  
### Versão 1.6 🎬

**Envio de VIDEO**
- Incluir a inserção de video dentro da campanha.

### Versão 1.7 🔊

**Envio de AUDIO**
- Incluir a inserção de audio dentro da campanha.

### Versão 1.8 🏷️🏷️

**Multiplas etiquetas**
- Disparar campanha para mais de uma etiqueta.

### Versão 1.9 📇💬 (Já disponível)

**Buscar etiquetas na conversa**
- Agora alem de buscar a etiqueta do contato ele busca nas conversas.
**Envia campanha para grupos**
- Agora suas campanhas tambem serão enviadas para grupos do whatsapp.

### Versão 2.0 [SAAS] 🌟

**Apenas um workflow por instancia**
- Este workflow foi desenvolvido para funcionar de forma centralizada com toda a instância do Chatwoot, eliminando a necessidade de criar um novo fluxo no n8n para cada caixa de entrada individual.

- Com esse modelo SaaS, é possível disparar campanhas por todos os números conectados à plataforma de forma unificada. Basta apenas criar as caixas de entrada (SMS) desejadas no Chatwoot, configurar os respectivos disparadores e informar corretamente o Application ID, que deve ser o nome da caixa de entrada cadastrada na Evolution API.

- Dessa forma, o envio de mensagens é direcionado automaticamente para o número vinculado à caixa de entrada correspondente, garantindo escalabilidade, agilidade e padronização no processo de campanhas.

### Considerações Finais 🛠️
- Este roadmap pode ser ajustado conforme novas ideias surjam ou prioridades se alterem durante o desenvolvimento. Cada etapa visa melhorar a funcionalidade e a eficiência do disparador de campanhas, proporcionando uma experiência mais completa e eficaz aos usuários.

---

## 📝 Apoio ao Projeto

Quem quiser apoiar o projeto com dicas de melhorias e reportar alguma falha pode me acionar pelo grupo do WhatsApp.

_https://chat.whatsapp.com/H2as2v9yHre8U2gjNaCWRc_

Canal do Youtube: https://www.youtube.com/@RodrigoTanci

Caso alguém queira contribuir de forma monetária, o chave PIX CNPJ: **36799434000140**
