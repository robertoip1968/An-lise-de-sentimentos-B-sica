# Análise de sentimentos Básica
## Sem o uso de Modelos prontos de redes neurais

### Importação de bibliotecas
    import pandas as pd
    import matplotlib.pyplot as plt

### Leitura do arquivo xlxs
    file_path = '/Tweets.xlsx'  # Exemplo: Planilha local

### Código
    # Função para analisar sentimento em um texto (simplificada)
    def analisar_sentimento(texto):
      """
      Analisa o sentimento de um texto com base em palavras-chave.
      Retorna 1 para sentimento positivo, -1 para sentimento negativo e 0 para neutro.
      """
      texto = texto.lower()
      positivo = ['bom', 'ótimo', 'excelente', 'positivo', 'apoio', 'vitória']
      negativo = ['ruim', 'péssimo', 'negativo', 'crítica', 'derrota', 'fraco']

      for palavra in positivo:
          if palavra in texto:
              return 1
      for palavra in negativo:
          if palavra in texto:
              return -1
      return 0

    try:
      # Lê a planilha do Excel
      df = pd.read_excel(file_path)

      # Verifica se a coluna 'TweetText' existe
      if 'TweetText' not in df.columns:
          print("Erro: A coluna 'TweetText' não foi encontrada na planilha.")
      else:
          # Aplica a análise de sentimento aos textos dos tweets
          df['Sentimento'] = df['TweetText'].apply(analisar_sentimento)

          # Contar tweets com sentimento positivo, negativo e neutro
          total_positivo = df[df['Sentimento'] == 1].shape[0]
          total_negativo = df[df['Sentimento'] == -1].shape[0]
          total_neutro = df[df['Sentimento'] == 0].shape[0]

          # Imprimir os resultados
          print(f"Total de Tweets com Sentimento Positivo: {total_positivo}")
          print(f"Total de Tweets com Sentimento Negativo: {total_negativo}")
          print(f"Total de Tweets com Sentimento Neutro: {total_neutro}")

          # Criar gráfico de barras
          sentimento_counts = df['Sentimento'].value_counts()
          sentimento_labels = {1: "Positivo", -1: "Negativo", 0: "Neutro"}
          sentimento_counts.index = sentimento_counts.index.map(sentimento_labels)

          plt.figure(figsize=(8, 6))
          plt.bar(sentimento_counts.index, sentimento_counts.values, color=['green', 'red', 'blue'])
          plt.xlabel("Sentimento")
          plt.ylabel("Número de Tweets")
          plt.title("Análise de Sentimentos dos Tweets")
          plt.show()

    except FileNotFoundError:
      print(f"Erro: O arquivo '{file_path}' não foi encontrado.")
    except Exception as e:
      print(f"Ocorreu um erro: {e}")
