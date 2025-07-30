Projeto: Pipeline de Transcrição e Resumo de Áudio com IA (N8N)
Este repositório contém o fluxo de trabalho (workflow) do N8N para um pipeline automatizado de transcrição e resumo inteligente de arquivos de áudio, utilizando Inteligência Artificial.

🚀 Visão Geral do Projeto
Este projeto demonstra a construção de um pipeline de dados que monitora uma pasta específica no Google Drive, processa arquivos de áudio MP3, transcreve seu conteúdo usando uma API de IA (OpenAI Whisper) e, futuramente, gerará um resumo inteligente e organizará os resultados.

📈 Status Atual do Projeto
O pipeline está funcional para a etapa de transcrição de áudio. As etapas de monitoramento, validação, download, normalização binária e integração com a API de transcrição estão operacionais. As próximas etapas incluem a implementação do resumo inteligente e a organização dos resultados.

✨ Como Funciona
O pipeline opera em etapas automatizadas:

Monitoramento: Um gatilho no N8N monitora uma pasta designada no Google Drive (Audios) em busca de novos arquivos MP3.

Validação: Ao detectar um novo arquivo, ele é validado para garantir que é um MP3 e está dentro de um limite de tamanho aceitável (evitando processamento de arquivos inválidos ou excessivamente grandes).

Download: O arquivo MP3 validado é baixado do Google Drive para processamento local no N8N.

Normalização Binária: Um nó de código (Preparar Ficheiro Binário) prepara o arquivo binário, extraindo-o da estrutura de saída do nó de download e atribuindo-o a uma propriedade binária de nome consistente (myAudioFile). Isso garante compatibilidade de formato para o envio à API de IA.

Transcrição com IA: O áudio é enviado para a API da OpenAI (modelo Whisper) para ser transcrito em texto. Esta etapa está totalmente implementada e funcional.

(Próximas Etapas): O texto transcrito será então enviado para um Large Language Model (LLM) para gerar um resumo inteligente e, por fim, os resultados (transcrição e resumo) serão salvos e organizados em outra pasta no Google Drive.

🛠️ Tecnologias Utilizadas
N8N: Ferramenta de automação de fluxo de trabalho (workflow automation) low-code/no-code.

Google Drive API: Para monitoramento, download e upload de arquivos.

OpenAI API:

Whisper Model: Para transcrição de áudio para texto.

GPT Models (futuro): Para geração de resumos inteligentes.

JavaScript: Utilizado em nós Code para validação e normalização de dados.

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

No nó Solicitação HTTP (Transcrição IA) e Resumo IA (futuro), clique em "Authentication" e selecione "Create new credential".

Escolha "Header Auth" como "Credential Type".

Header Name: Authorization

Header Value: Bearer SEU_API_KEY_OPENAI (substitua pela sua chave de API da OpenAI. Nunca exponha sua chave diretamente em arquivos de código ou repositórios públicos!).

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

💡 Solução de Problemas Comuns
Caso encontre problemas durante a configuração ou execução:

Erro "Acesso bloqueado" (Google Drive): Verifique se o seu aplicativo OAuth2 no Google Cloud Console está "Em Produção" e se o seu e-mail está adicionado como usuário de teste.

Erro "O servidor DNS retornou um erro": Verifique sua conexão com a internet, reinicie o roteador ou considere mudar os servidores DNS do seu sistema.

Erro "The item has no binary field": Verifique a aba "SAÍDA" (Output) > "Binário" (Binary) do nó Download file para identificar o nome exato do campo binário. Se o problema persistir, certifique-se de que o nó Preparar Ficheiro Binário está corretamente configurado para normalizar o nome do ficheiro binário para myAudioFile.

Erro 401 (Unauthorized) / "Authorization failed": Verifique sua chave de API da OpenAI. Crie uma nova chave, certifique-se de que ela está ativa e que foi inserida corretamente na credencial Header Auth (formato Bearer SEU_API_KEY).

Erro 429 (Too Many Requests) / Excedeu a cota: Verifique o uso e o faturamento da sua conta OpenAI em https://platform.openai.com/account/usage. Você pode ter atingido os limites do plano gratuito ou da sua cota.

🗺️ Próximas Etapas e Melhorias Futuras
Este pipeline é uma base sólida e pode ser expandido com as seguintes funcionalidades:

Resumo Inteligente: Implementar o nó HTTP Request para chamar um LLM (como GPT-4o) e gerar um resumo estruturado da transcrição.

Organização de Resultados: Criar nós Google Drive para salvar a transcrição (.txt) e o resumo (.md) em pastas específicas (/Resumos/[YYYY-MM-DD]_[nome_original]_transcricao.txt e /Resumos/[YYYY-MM-DD]_[nome_original]_resumo.md).

Notificações: Adicionar nós de Email ou Webhook para notificar a conclusão do processo (sucesso ou falha).

Suporte a Vídeos (Plus): Adicionar lógica para aceitar arquivos de vídeo, extrair o áudio (via FFmpeg, se disponível no ambiente) e processá-los.

Processamento de Links do YouTube (Super Plus): Adicionar um fluxo para baixar áudio de vídeos do YouTube (via yt-dlp), processá-los e organizar em pastas específicas.

Detecção de Idioma: Implementar detecção automática de idioma na transcrição.

Identificação de Múltiplos Speakers (Diarização): Utilizar APIs que suportam a identificação de diferentes vozes.

Geração de Tags/Categorias: Extrair tags relevantes do conteúdo.

Diferentes Formatos de Resumo: Oferecer opções de resumo executivo ou detalhado.

🤝 Contribuição
Sinta-se à vontade para explorar, testar e aprimorar este pipeline. Sugestões e melhorias são bem-vindas!

📄 Licença
Este projeto é distribuído sob a licença [Escolha uma licença, ex: MIT License]. Veja o arquivo LICENSE para mais detalhes.
