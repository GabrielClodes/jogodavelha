JOGO DA VELHA
=======================
O objetivo do jogo da velha em Python, são dois jogadores podem competir entre si.


ÍNDICE
=======================

- Como Jogar;
- Instalação;
- Configuração;
- Execução do Pipeline;


COMO JOGAR?
=======================

1. **Objetivo**: O objetivo do jogo é ser o primeiro a alinhar três marcas consecutivas, seja na horizontal, vertical ou diagonal.
   
2. **Estrutura do Jogo**:
   - O jogo é executado em um loop principal que continua até que um jogador vença ou ocorra um empate.
     
   - O tabuleiro do jogo é representado por **3 linhas e 3 colunas** de 3x3, onde cada célula do tabuleiro vazia pode ser marcada com "X" ou "O".
     
   - **Jogadores**: São representados por "X" e "O". O jogador "X" começa a partida.

     
3. **Regras**:
   - Os jogadores devem escolher uma posição vazia para colocar sua marca.
     
   - Após cada jogada, o tabuleiro é reexibido, e o jogo continua até que haja um vencedor ou empate.


INSTALAÇÃO
=======================

Instruções para Instalação do Jogo da Velha
Para instalar e executar o jogo da velha, siga os passos abaixo:

Baixe o Repositório: Faça o download da pasta disponível no link (https://github.com/GabrielClodes/jogodavelha/blob/3e3732671e7497d77ac434e6b059276493d540c2/jogodavelha.zip).

Upload no Ambiente Virtual: Após o download, faça o upload da pasta para o seu ambiente de desenvolvimento preferido, como VSCode, Jupyter Notebook, PyCharm ou Google Colab.

Arquivo de Código: Dentro da pasta, você encontrará o arquivo codigo.txt. Este arquivo contém o código completo para a execução do jogo dentro do ambiente virtual escolhido.

Ambientes Virtuais e Requisitos: Para rodar o código com sucesso, é necessário ter um dos seguintes ambientes virtuais instalados em sua máquina:

- VSCode
- Jupyter Notebook
- PyCharm
- Google Colab
- Requisitos Adicionais: Além do ambiente virtual, é necessário ter o Apache Spark instalado para o funcionamento do pipeline do projeto. O Apache Spark é utilizado para o processamento e gestão dos dados dentro do projeto.

Após a instalação e configuração do ambiente, basta executar o código presente no arquivo codigo.txt para iniciar o jogo da velha.


CONFIGURAÇÃO
=======================

Para rodar o Jogo da Velha no seu ambiente local ou no Google Colab, siga os passos abaixo:


# Importando as bibliotecas

      import ipywidgets as widgets  # Para criar os botões interativos para o Jogo da Velha
      import pyspark as spark  # Para manipulação e processamento de dados com PySpark
      import matplotlib.pyplot as plt  # Para visualizações gráficas (não utilizado diretamente aqui)
      import numpy as np  # Para operações numéricas (não utilizado diretamente aqui)
      import pandas as pd  # Para manipulação de dados (não utilizado diretamente aqui)
      import seaborn as sns  # Para visualizações estatísticas (não utilizado diretamente aqui)
      from IPython.display import display  # Para exibir widgets interativos no Jupyter Notebook
      from pyspark.sql import SparkSession  # Para criar uma SparkSession para usar o PySpark
      from google.colab import files


# Criando seção Spark

      spark = SparkSession.builder \
      .appName("jogodavelha") \
      .getOrCreate()

# Fazendo o upload do código e buscando o mesmo nos arquivos do computador:

        uploaded = files.upload()  # Você faz o upload do arquivo
        codigo = list(uploaded.keys())[0]  # Pega o nome do arquivo, por exemplo, 'jogo.txt'
        with open(codigo, 'r') as file:
        criar_tabuleiro = file.read()  # Lê o conteúdo de 'jogo.txt' e armazena em criar_tabuleiro


# Função para criar o tabuleiro do Jogo da Velha

    def criar_tabuleiro():
    """
    Cria e exibe o tabuleiro interativo do Jogo da Velha utilizando o ipywidgets.
    O tabuleiro é composto por 9 botões, um para cada célula do jogo.
    Cada vez que um botão é pressionado, o jogo alterna entre os jogadores 'X' e 'O'.
    Além disso, verifica se algum jogador venceu ou se houve empate.
    """
    #Criando os botões do tabuleiro
    botoes = [widgets.Button(description=' ') for _ in range(9)]
    for i in range(9):
        botoes[i].layout.width = '100px'
        botoes[i].layout.height = '100px'
        botoes[i].layout.border = '1px solid black'  # Adicionando borda aos botões
        


# Criando os botões do Tabuleiro
    def jogar(botao, index):
        """
        Função que alterna entre os jogadores 'X' e 'O' e realiza a jogada no botão pressionado.
        Se o botão estiver vazio, ele será preenchido com 'X' ou 'O', dependendo do jogador atual.
        Em seguida, chama a função verificar_vencedor().
        """
       

# Função para fazer a jogada
    def jogar(botao, index):
        """
        Função que alterna entre os jogadores 'X' e 'O' e realiza a jogada no botão pressionado.
        Se o botão estiver vazio, ele será preenchido com 'X' ou 'O', dependendo do jogador atual.
        Em seguida, chama a função verificar_vencedor().
        """
        if botao.description == ' ':
            if jogador_atual[0] == 'X':
                botao.description = 'X'
                botao.style.button_color = 'Purple'
                jogador_atual[0] = 'O'
            else:
                botao.description = 'O'
                botao.style.button_color = 'Green'
                jogador_atual[0] = 'X'
            verificar_vencedor(botoes)

# Função para verificar se algum jogador venceu
    def verificar_vencedor(botoes):
        """
        Verifica se algum jogador venceu o jogo verificando todas as combinações de vitória.
        Se houver um vencedor, imprime uma mensagem indicando o vencedor.
        Caso todos os botões sejam preenchidos e não haja vencedor, imprime "Empate!".
        """
        combinacoes_vitoria = [(0, 1, 2), (3, 4, 5), (6, 7, 8), (0, 3, 6), (1, 4, 7), (2, 5, 8), (0, 4, 8), (2, 4, 6)]
        for c in combinacoes_vitoria:
            if botoes[c[0]].description == botoes[c[1]].description == botoes[c[2]].description != ' ':
                vencedor = botoes[c[0]].description
                salvar_resultado(vencedor)
                for botao in botoes:
                    botao.disabled = True
                return
        if all(botao.description != ' ' for botao in botoes):
            salvar_resultado("Empate!")
            return

# Definindo as ações de clique nos botões
    for i in range(9):
        botoes[i].on_click(lambda x, index=i: jogar(x, index))


# Exibindo o tabuleiro no formato de grade
    grid = widgets.GridBox(
        children=botoes,
        layout=widgets.Layout(
            grid_template_columns="repeat(3, 100px)",
            grid_template_rows="repeat(3, 100px)",
            grid_gap='2px',  # Espaço entre os botões
            border='2px solid black'  # Borda ao redor do tabuleiro
        )
    )

    display(grid)

# Salvar o resultado e adicionar um arquivo no Google Colab com o resultado final

    # Criando um arquivo para mostrar o resultado
      def salvar_resultado(resultado):
    """
    Função para salvar o resultado do jogo em um arquivo de texto.
    """
    # Criando uma pasta chamada 'n_resultado_jogo' se não existir
    !mkdir -p /content/resultado_jogo

    # Caminho do arquivo onde o resultado será salvo
    caminho_arquivo = "/content/resultado_jogo/resultado_jogo.txt"

    # Salvar o resultado em um arquivo de texto
    with open(caminho_arquivo, 'w') as file:
        file.write(f"O jogador vencedor foi: {resultado}")

# Jogador atual

      jogador_atual = ['X']

# Criando e exibindo o tabuleiro

      criar_tabuleiro()

Ao ser identificado o vencedor podendo ser “X” ou “O” o jogo será congelado, caso esteja utilizando o Google Colab, 
será gerado uma pasta no mesmo informando o resultado do vencedor.


EXECUÇÃO DO PIPELINE
=======================

Após a instalação do Apache Spark, a configuração do ambiente e o upload dos arquivos necessários, será exibido o pipeline de execução do código relacionado ao Jogo da Velha. Esse pipeline representa a sequência de etapas que o código segue, desde a leitura e processamento das jogadas até a verificação do vencedor e o armazenamento do resultado. O fluxo de execução será gerenciado de forma eficiente, utilizando as capacidades do Apache Spark para processar e registrar as jogadas em tempo real.


**Desafios enfrentados**

A instalação do Docker foi realizada para criar o repositório do jogo, utilizando os programas Anaconda, PyCharm e Jupyter Notebook. No entanto, ao tentar configurar o Apache Airflow, surgiram conflitos de versão e dificuldades na importação do pacote, o que impediu a execução do pipeline conforme o esperado.

Como alternativa para demonstrar o desafio proposto, optei por executar o projeto no ambiente Google Colab, onde foi possível importar o Apache Spark para visualizar o pipeline de maneira eficiente. Além disso, para melhorar a interação com o jogo, criei uma interface interativa no Google Colab, permitindo que os usuários jogassem o Jogo da Velha diretamente na plataforma. O resultado da partida, incluindo o vencedor, foi automaticamente gravado em uma nova subpasta para registro.
