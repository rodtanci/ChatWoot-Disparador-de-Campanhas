# 🚀 Solução de Disparador de Campanhas via WhatsApp 🚀

## 🌟 Introdução 🌟

O disparador de campanhas oferece uma forma eficiente e eficaz de gerenciar e enviar mensagens de campanha utilizando o ChatWoot. Esta solução é projetada para facilitar a comunicação com seus clientes, permitindo agendamento de campanhas, envio de mensagens personalizadas e acompanhamento de desempenho.

## Funcionalidades

### 📱 Campanhas dentro do ChatWoot

Integração completa com o ChatWoot para gerenciar suas campanhas diretamente dentro da plataforma.

### ⏰ Agendamento e Disparo Imediato

- Agende suas campanhas para uma data e hora específicas.
- Opção para disparo imediato, permitindo envios em tempo real.

### 🖼️ Envio de Imagens

- Possibilidade de incluir imagens nas mensagens da campanha.
- Suporte para múltiplos formatos de imagem (JPEG e PNG).

### 📊 Limitador de Envios Diário por Empresa

- Defina um limite diário de envios para cada empresa.
- Evite sobrecarregar os a utilização da VPS com o alto fluxo de mensagens.

### ⏳ Temporizador Randômico entre os Envios

- Adicione um temporizador randômico entre os envios para evitar detecção como spam e bloquear o numero.
- (Lembrando que isso não é uma garantia que você não possa perder seu número e sim uma solução para reduzir os riscos.)

### 🔄 Contador de Envios e Falha

- Acompanhe o número total de mensagens enviadas e falhas.

### 🚫 Mensagem de Excesso de Limite Diário

- Envio automático de uma mensagem quando o limite diário de envios for excedido.
- Mantenha os administradores informados sobre o status das campanhas.

### 📋 Mensagem de Relatório Final da Campanha

- Receba um relatório detalhado ao final de cada campanha.
- Inclui número total de envios, falhas.

### 📝 Personalização com Nome do Contato

- Use a variável (&nome) para personalizar as mensagens com o nome do contato.
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
   - Nome da Caixa de Entrada: Disparador (ou o nome que preferir)
   - Número de telefone: +741963
   - ID da Conta: 741963
   - ID da aplicação: 741963
   - Chave API: id da conta do chatwoot
   - chave secreta API: 741963
7. **Salvar Configurações**: Clique em "Criar canal Bandwidth" para criar a nova caixa de entrada.

Agora que a caixa de entrada do canal SMS do tipo Bandwidth está configurada, podemos prosseguir para os próximos passos da automação!

