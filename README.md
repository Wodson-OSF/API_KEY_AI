# Instala silenciosamente a biblioteca google-generativeai atualizada
!pip install -q -U google-generativeai

# Importando o SDK Python do Google Generative AI
import google.generativeai as genai

# Importando o userdata do Colab para armazenamento seguro da chave de API
from google.colab import userdata

# Substitua 'CHAVE API DO GOOGLE-COLE AQUI' pela sua chave de API real
GOOGLE_API_KEY = userdata.get('GOOGLE_API_KEY')

# Configura o SDK com a chave de API (substitua por sua chave real)
genai.configure(api_key=GOOGLE_API_KEY)

# Procura por modelos disponíveis que suportam o método 'generateContent'
for modelo in genai.list_models():
  if 'generateContent' in modelo.supported_generation_methods:
    # Imprime o nome do modelo compatível
    print(f"Modelo compatível encontrado: {modelo.name}")

    # Define a configuração de geração
    configuracao_geracao = {
        "candidate_count": 1,  # Número de candidatos de texto a gerar (1 neste caso)
        "temperature": 0.5,  # Parâmetro de controle da criatividade (0.5 para balanceamento)
    }

    # Define as configurações de segurança
    configuracoes_seguranca = {
        "HARASSMENT": "BLOCK_NONE",  # Bloqueia nenhum tipo de assédio
        "HATE": "BLOCK_NONE",       # Bloqueia nenhum tipo de discurso de ódio
        "SEXUAL": "BLOCK_NONE",      # Bloqueia nenhum tipo de conteúdo sexual
        "DANGEROUS": "BLOCK_NONE",   # Bloqueia nenhum tipo de conteúdo perigoso
    }

    # Cria um modelo generativo com o nome especificado, configurações de geração e segurança
    modelo = genai.GenerativeModel(model_name="gemini-1.0-pro",
                                  generation_config=configuracao_geracao,
                                  safety_settings=configuracoes_seguranca)

    # Define a entrada de texto para geração de conteúdo
    entrada_texto = "Vamos apreder conteúdo de IA. Me dê sugestões."

    # Gera conteúdo utilizando o modelo e a entrada de texto
    resposta = modelo.generate_content(entrada_texto)

    # Imprime o texto gerado pelo modelo
    print(f"Texto gerado: {resposta.text}")
