Projeto: Pipeline de Transcrição e Resumo de Áudio com IA (N8N)
Este repositório contém o fluxo de trabalho (workflow) do N8N para um pipeline automatizado de transcrição e resumo inteligente de arquivos de áudio, utilizando Inteligência Artificial.

🚀 Visão Geral do Projeto
Este projeto demonstra a construção de um pipeline de dados que monitora uma pasta específica no Google Drive, processa arquivos de áudio MP3, transcreve seu conteúdo usando uma API de IA (OpenAI Whisper) e, futuramente, gerará um resumo inteligente e organizará os resultados.

✨ Como Funciona
O pipeline opera em etapas automatizadas:

Monitoramento: Um gatilho no N8N monitora uma pasta designada no Google Drive (Audios) em busca de novos arquivos.

Validação: Ao detectar um novo arquivo, ele é validado para garantir que é um MP3 e está dentro de um limite de tamanho aceitável.

Download: O arquivo MP3 validado é baixado do Google Drive para processamento.

Normalização Binária: Um nó de código prepara o arquivo binário para ser enviado à API de IA, garantindo compatibilidade de formato.

Transcrição com IA: O áudio é enviado para a API da OpenAI (modelo Whisper) para ser transcrito em texto.

(Próximas Etapas): O texto transcrito será então enviado para um Large Language Model (LLM) para gerar um resumo inteligente e, por fim, os resultados (transcrição e resumo) serão salvos e organizados em outra pasta no Google Drive.

📋 Requisitos do Sistema e Ferramentas
Para configurar e executar este pipeline, você precisará de:

N8N: Uma instância do N8N (local ou em nuvem) em execução.

Conta Google Drive: Uma conta Google com acesso ao Google Drive para as pastas de entrada (Audios) e saída (Resumos).

Conta OpenAI: Uma conta ativa na plataforma OpenAI com uma chave de API válida e créditos suficientes para usar os modelos Whisper (transcrição) e GPT (resumo).

⚙️ Configuração e Instalação
Siga os passos abaixo para configurar o workflow em sua instância do N8N:

Exporte o Workflow:

Baixe o arquivo pipeline_transcricao_resumo.json deste repositório.

Importe o Workflow no N8N:

Abra sua instância do N8N no navegador.

No painel esquerdo, clique em "Workflows".

Clique em "Import from File" (Importar de Arquivo) e selecione o arquivo pipeline_transcricao_resumo.json.

Configure as Credenciais:

Google Drive:

No nó Gatilho do Google Drive, clique em "Credentials" e selecione "Create new credential" (se ainda não tiver).

Siga as instruções para configurar uma credencial OAuth2.0 para o Google Drive no Google Cloud Console (habilitar Google Drive API, criar ID e Segredo do Cliente, configurar URI de redirecionamento http://localhost:5678/rest/oauth2-credential/callback, e publicar o app para "Em Produção").

Certifique-se de que a mesma credencial esteja selecionada no nó Download file.

OpenAI API Key:

No nó Solicitação HTTP (Transcrição IA) e Resumo IA, clique em "Authentication" e selecione "Create new credential".

Escolha "Header Auth" como "Credential Type".

Header Name: Authorization

Header Value: Bearer SEU_API_KEY_OPENAI (substitua pela sua chave de API da OpenAI).

Ajuste os IDs das Pastas no Google Drive:

No seu Google Drive, crie uma pasta chamada Audios (para entrada) e outra chamada Resumos (para saída).

Abra a pasta Audios no navegador e copie o Folder ID da URL (a string longa após /folders/).

No nó Gatilho do Google Drive, cole este Folder ID no campo "Folder ID".

(Futuramente, você precisará dos IDs das pastas de saída nos nós de Upload).

Ative o Workflow:

No canto superior direito do N8N, clique no botão "Ativo" para ligar o workflow.

🧪 Testando o Pipeline
Para testar o pipeline:

Faça o upload de um arquivo MP3 pequeno (ex: 1-2 MB) para a pasta Audios no seu Google Drive.

Aguarde o tempo de polling (configurado no gatilho, ex: a cada minuto).

Observe o fluxo de trabalho no N8N. Todos os nós devem ficar verdes.

Inspecione a saída do nó Solicitação HTTP (Transcrição IA) na aba "SAÍDA" (Output) para ver o texto transcrito.

🤝 Contribuição
Sinta-se à vontade para explorar, testar e aprimorar este pipeline. Sugestões e melhorias são bem-vindas!

Espero que este README.md seja útil para o seu repositório! Ele está formatado em Markdown, pronto para ser copiado e colado em um arquivo README.md na raiz do seu projeto.
