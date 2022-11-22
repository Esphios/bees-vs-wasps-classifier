# Bees Vs Wasps Classifier

## Para ver o Caderno de Projetos

Como implementei três arquiteturas no mesmo notebook, o tamanho do notebook ipynb é maior do que o normal e, portanto, pode não abrir em GitHub. Você pode ver o Caderno do Projeto [aqui](https://github.com/Esphios/bees-vs-wasps-classifier/blob/master/BeesVsWasps.ipynb) .

## Visão geral do Projeto

Este projeto foi inspirado no Kaggle Challange realizado em 2003, no qual milhares de imagens de abelhas e vespas foram dadas e um modelo deveria ser construído para classificar essas imagens em abelhas e vespas. A melhor precisão alcançada nessa competição foi de 98%!

Utilizei um subconjunto desses dados e construí meu modelo, no conjunto de dados original havia cerca de 25000 imagens para treinamento, mas só estou utilizando 2000 imagens...
Usei três arquiteturas diferentes para treinar este conjunto de dados e aumentei a precisão de validação de cerca de 73% para 96%!!!

As etapas básicas dobradas em cada arquitetura foram:

* Redimensionar as imagens para o tamanho de entrada desejado e aplicar o pré-processamento necessário como cisalhamento, rotação, deslocamento, etc.
* Projetar, treinar e validar o modelo utilizando o conjunto de dados.
* Baixar uma imagem da Internet e testá-la no modelo treinado.

## Arquitetura do modelo

### Transferir aprendizagem usando VGG16

|         Layer          |          Description          |
| :--------------------: | :---------------------------: |
|         Input          |      150x150x3 RGB image      |
|         VGG16          |          Functional           |
|        Flatten         |                               |
| Fully connected(Dense) |          Outputs=256          |
|          RELU          |                               |
|         Output         | Outputs=1, activation=sigmoid |

~~Você pode encontrar o arquivo do modelo treinado [aqui](https://github.com/Esphios/bees-vs-wasps-classifier/blob/master/models/vgg16_cnn.h5).~~ (removido por tamanho)

Para este modelo, os resultados obtidos foram:

* **Precisão de validação: 92.4%**
* **Precisão de treinamento: 98.3%**

## Model Design and Implementation

## Projeto e implementação do modelo

Nosso problema foi classificar as imagens coloridas em duas categorias: Abelhas e Vespas, para extrair as características de uma imagem CNNs são a melhor solução, embora muitas vezes exijam cálculos pesados. Assim, utilizei o Transfer learning usando a popular arquitetura [VGG16](https://arxiv.org/pdf/1409.1556.pdf). Este modelo tinha muitos parâmetros, pois mantive a camada VGG16 como funcional e não a congelei durante o treinamento! Treinei o modelo durante 30 épocas. Como esta era uma arquitetura complexa, eu criei pontos de verificação no final de cada época para que eu pudesse reverter se meu treinamento parasse abruptamente, também esta história de épocas pode ser usada para ajustar ainda mais o modelo. Para todos os modelos que usei RMSprop como otimizador, neste modelo usei uma taxa de aprendizagem particularmente baixa de 1e-7 para obter melhores resultados. Após o treinamento, obtive uma precisão de validação de 96%, o que, de acordo comigo, é um ótimo resultado para apenas 2000 imagens de treinamento.

## Teste, Validação e Análise

Os testes feitos nas imagens baixadas da rede também deram uma grande visão sobre o desempenho do modelo. Uma das imagens baixadas da rede consistia de muitos insetos.

Estatisticamente essa imagem tem mais abelhas do que vespas, quando a imagem foi dada como entrada para os modelos foi prevista como um cão com alta confiança nos três modelos, o que não é surpreendente, pois os abelhas ocupam uma parte maior da imagem em comparação com os vespas e, portanto, quando as camadas convolutivas extraem as características da imagem, as características dos abelhas são em sua maioria.

Para observar corretamente o trabalho das camadas convolutivas e das camadas maxpool, observei até mesmo a saída das camadas Conv e MaxPool individuais para o primeiro modelo, o que me deu uma grande visão do processo de filtragem e agrupamento.

## Adicionando novo conjunto de dados de treino

Se você quiser adicionar uma nova imagem de treinamento aos conjuntos de dados disponíveis, você pode adicionar as imagens de abelhas e vespas a **/data/train/bee*** e **/data/train/wasp** respectivamente. O tamanho da imagem não importa, pois ela é redimensionada antes do treinamento no código.

## Implementar este projeto

### I. Se você tiver uma GPU dedicada, siga estes passos para executar este modelo usando [miniconda](https://conda.io/en/latest/)

**Instalar** [miniconda](http://conda.pydata.org/miniconda.html) em sua máquina. Instruções detalhadas:

* **Linux:** <http://conda.pydata.org/docs/install/quick.html#linux-miniconda-install>
* **Mac:** <http://conda.pydata.org/docs/install/quick.html#os-x-miniconda-install>
* **Windows:** <http://conda.pydata.org/docs/install/quick.html#windows-miniconda-install>

**Clone** o repositório.

(Você precisará do Git para isso)

```sh
git clone https://github.com/Esphios/bees-vs-wasps-classifier.git
cd bees-vs-wasps-classifier
```

No Windows: Depois de clonar o repositório na mesma pasta, abra o Prompt de Comando ou então o Prompt Anaconda, para que o Promt de Comando seja executado, ative seu ambiente base.

**Crie** env_meu_projeto.  Executando este comando, será criado um novo ambiente `conda` chamado env_meu_projeto.

```
conda create --name env_meu_projeto python=3.7
```

**Baixe** as dependências.  Executando este comando, as dependências e bibliotecas serão baixadas no ambiente criado.

```
pip install -r requirements.txt
````

**Verifique** se o ambiente foi criado nos seus ambientes:

```sh
conda info --envs
```

**Caso você tenha certeza de que seu ambiente está instalado corretamente,**

**Ativar** o ambiente.  Executando este comando, será ativado o ambiente criado onde você poderá instalar as dependências.

`````
conda activate env_meu_projeto
`````

**Abrir** o notebook do jupyter.  A execução deste comando abrirá o notebook do jupyter.

```
jupyter notebook
````


**Desinstalar***
Para desinstalar o ambiente:

```sh
conda remove --name env_meu_projeto --all
```

**Abra os BeesVsWasps.ipynb** e certifique-se de escolher o ambiente certo onde você instalou suas bibliotecas e então comece a executar as células!

Para sair do ambiente quando tiver concluído sua sessão de trabalho, basta fechar a janela do terminal.
