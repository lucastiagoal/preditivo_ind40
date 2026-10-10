# Disclaimer:
Foi utilizado apoio de IA (ChatGPT) para explicações dos conceitos e revisão dos textos.

# Preditivo Ind40

Projeto manutenção preditiva industrial em Python. O objetivo é comparar KNN e Árvore de Decisão na classificação de falhas usando medições de temperatura, rotação, torque e desgaste.

## Estado do desenvolvimento

As sete fases estão implementadas, com resultados salvos no notebook. O vídeo de apresentação está disponível no link ao final deste README.

A modelagem foi executada no Google Colab com Python 3.13.16 porque o Smart App Control bloqueou um componente do scikit-learn no Windows. O arquivo de dependências registra as versões informadas pelo ambiente usado na execução.

## Dados

O arquivo fornecido pela disciplina fica em `data/manutencao_preditiva.csv`: 10.000 registros e 14 colunas. O alvo é `falha_maquina` (0 = normal; 1 = falha). As cinco colunas que descrevem causas de falhas não serão usadas como preditoras.

## Estrutura

- `preditivo_falha_ind40.ipynb`: notebook principal, organizado em sete fases.
- `data/manutencao_preditiva.csv`: base de dados.
- `requirements.txt`: versões das bibliotecas usadas no Colab.
- `planejamento.md`: estado das tarefas e pendências.
- `.gitignore`: exclui ambientes virtuais e arquivos locais do Git.

## Instalação e execução

### Google Colab — ambiente usado no projeto

1. Abra o Colab e envie o notebook pela opção de upload.
2. Conecte o ambiente. No painel de arquivos, crie a pasta `data` dentro de `/content` e envie o CSV para ela. O caminho deve ficar `/content/data/manutencao_preditiva.csv`.
3. Para instalar as versões registradas, envie também `requirements.txt` para `/content` e execute `%pip install -r /content/requirements.txt` em uma célula. Se o Colab solicitar reiniciar a sessão após a instalação, reinicie antes de continuar.
4. Execute as células de cima para baixo com “Executar tudo”.
5. Salve e baixe o notebook com os resultados para atualizar a cópia do repositório.

Os arquivos enviados e as variáveis são temporários no Colab. Após uma nova sessão, envie o CSV novamente e execute o notebook desde o início.

### VS Code — execução local

1. Abra esta pasta no VS Code com as extensões Python e Jupyter. Use Python 3.13 para acompanhar a versão principal e secundária do ambiente de execução do projeto.
2. Crie o ambiente: `python -m venv .venv`.
3. No terminal PowerShell desta pasta, instale as dependências:
   `./.venv/Scripts/python.exe -m pip install -r requirements.txt`.
4. Selecione o interpretador e o kernel da `.venv`.
5. Abra o notebook e execute as células de cima para baixo.

O antigo ambiente local usa Python 3.14.7. A instalação das dependências, sozinha, não resolveu o bloqueio identificado pelo Smart App Control.

## Técnicas e decisões

Pandas e NumPy para os dados e cálculos; Matplotlib e Seaborn para gráficos; scikit-learn para divisão, escalonamento, modelos e acurácia; imbalanced-learn para SMOTE.

Usei média para preencher temperaturas e torque e mediana para rotação. Os valores foram calculados somente no treino. Não foram encontradas duplicatas. Mantive os valores extremos porque não havia evidência suficiente de erro.

Calculei a potência mecânica em watts com torque × rotação × 2π/60. Reutilizei a divisão estratificada de 80% para treino e 20% para teste. Apliquei SMOTE somente no treino e StandardScaler para o KNN, ajustado somente no treino. A árvore usa os dados sem escalonamento.

Comparei K igual a 3, 5 e 7 e profundidades da árvore iguais a 3, 5 e sem limite. Os identificadores, as colunas de causas de falha e a categoria `tipo` ficaram fora das entradas; esta versão usa medições numéricas.

## Resultados e limitações

| Modelo | Configuração selecionada | Acurácia no teste |
| --- | --- | --- |
| KNN | K = 3 | 91,85% |
| Árvore de Decisão | Sem limite de profundidade | 93,90% |

A árvore foi selecionada por ter maior acurácia entre os dois modelos. No treino, ela apresentou 98,96%, uma diferença que merece atenção ao discutir sobreajuste. Treino e teste têm proporções de classes diferentes após o SMOTE, então essa diferença não é uma prova isolada de sobreajuste.

O teste tem 1.932 registros normais e 68 falhas. Prever sempre normal teria 96,60% de acurácia, mas deixaria passar todas as falhas. Portanto, a acurácia sozinha não demonstra boa detecção de falhas nem permite afirmar que o modelo está pronto para uso industrial.

Os parâmetros foram selecionados usando o próprio teste, conforme a comparação solicitada no projeto. Esse teste não é uma avaliação final independente da seleção.

## Git e GitHub

O desenvolvimento usa `develop`, com branches por etapa e integração por pull requests. Foram usadas `feat/exploracao-limpeza`, `feat/tratamento-atributos` e `feat/modelagem`. A avaliação final está em `feat/avaliacao-final`. A versão de entrega será integrada à `main` após a revisão final. As branches serão preservadas após os pull requests.

## Melhorias futuras

Avaliar a detecção de falhas além da acurácia e separar a seleção dos parâmetros de uma avaliação final independente. Essas melhorias não foram implementadas nesta entrega.

## Vídeo de demonstração

**Vídeo no Google Drive:** [Assistir à apresentação](https://drive.google.com/drive/folders/10tt8Ba7YOIqKvKzP1qbnpdwOZN0S8Rj-?usp=sharing)
