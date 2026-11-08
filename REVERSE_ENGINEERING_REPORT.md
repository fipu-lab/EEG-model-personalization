# REVERSE_ENGINEERING_REPORT — EEG-model-personalization

## A. Diagnóstico geral

### Arquitetura identificada
Projeto de aprendizado de máquina para personalização de modelos EEG (electroencephalogram). Arquitetura:

- `models.py`: definições de modelos CNN 1D (CNN_1D_model, CNN_1D_BN_model, KAN_CNN_1D_model, KAN_CNN_1D_BN_model)
- `data_loader.py`: funções `read_data()` e `load_data()` para carregar dados .npy e criar janelas deslizantes
- `data_recorder.py`: script para gravar dados EEG via LSL (Lab Streaming Layer)
- `Personalization.ipynb`: notebook de treinamento individual, fine-tuning e transfer learning
- `Results.ipynb`: notebook de análise de resultados (sumário, LaTeX, gráfico de Pareto)
- `.gitmodules`: referencia tfkan como submodule (https://github.com/ZPZhou-lab/tfkan/)
- `.gitignore`: padrão Python + Jupyter

### Tecnologias e dependências
- Python (versão não declarada)
- Dependências inferidas: tensorflow==2.5.0, numpy==1.19.5, scikit-learn, matplotlib, jupyter, tfkan
- Não há requirements.txt
- Não há Pipfile

### Estado de manutenção
Último commit: `f94ba6f Update .gitmodules`. Branch default: main. Repositório: https://github.com/fipu-lab/EEG-model-personalization.

### Mecanismos de testes existentes
Nenhum teste automatizado encontrado.

### Limitações da investigação
- TensorFlow não instalado; não é possível executar os notebooks
- Dados de EEG (.npy) não estão no repositório (estão em .gitignore via data/)
- tfkan submodule não está inicializado no clone
- REA 6.7.0 não operacional; análise estática apenas

## B. Problemas encontrados

| ID | Título | Severidade | Status | Localização |
|----|--------|------------|--------|-------------|
| REA-eeg-001 | Ausência de requirements.txt | P3 | CONFIRMADO | raiz do repo |
| REA-eeg-002 | Falta de parâmetro seed para reprodutibilidade | P3 | CONFIRMADO | `data_loader.py` |
| REA-eeg-003 | Notebook clona tfkan via git dentro da célula | P2 | PROVÁVEL | `Personalization.ipynb:7-8` |
| REA-eeg-004 | data_recorder.py usa input() interativo | P3 | CONFIRMADO | `data_recorder.py:7` |

### Detalhes dos problemas confirmados

**REA-eeg-001 (P3, CONFIRMADO)**: Não há `requirements.txt` no repositório. As dependências são inferidas apenas a partir dos imports e de um comentário no notebook (`# !pip install numpy==1.19.5 tensorflow==2.5.0 keras==2.5.0rc0 scikit-learn==1.1.2 matplotlib`).

**REA-eeg-002 (P3, CONFIRMADO)**: `data_loader.py` não tem parâmetro `seed` para controlar a aleatoriedade do `np.random.shuffle()` em `load_data()`, tornando os resultados não reproduzíveis entre execuções.

Evidência: `data_loader.py:36-38` usa `np.random.shuffle(ind)` sem seed.

**REA-eeg-004 (P3, CONFIRMADO)**: `data_recorder.py:7` usa `input("Upisi direktorij osoba/akcija: ")` para capturar o nome do objeto, tornando o script interativo e impossível de executar em modo automatizado.

### Detalhes do problema provável

**REA-eeg-003 (P2, PROVÁVEL)**: `Personalization.ipynb:7-8` clona o repositório tfkan via `git clone` dentro de uma célula do notebook. Isso quebra a reprodutibilidade se o repositório remoto estiver indisponível ou mudar.

## C. Problemas recorrentes
Nenhum problema recorrente público encontrado.

## D. Erros não documentados
Todos os itens da seção B são problemas não documentados anteriormente.

## E. Performance
O treinamento usa EarlyStopping com paciência de 4 épocas e batches de 64. Não há operações de performance críticas além do treinamento de ML em si.

## F. Melhorias técnicas
- Adicionar requirements.txt com versões fixadas (REA-eeg-001, implementado)
- Adicionar parâmetro seed para reprodutibilidade (REA-eeg-002, implementado)
- Substituir git clone no notebook por submodule ou dependência fixada (REA-eeg-003)
- Substituir input() por argumento de linha de comando ou variável de ambiente (REA-eeg-004)

## G. Plano de execução
Correções independentes:
1. REA-eeg-001, REA-eeg-002 — requirements.txt e seed (baixo risco, implementado)
2. REA-eeg-003 — tfkan dependency (médio esforço)
3. REA-eeg-004 — input() interativo (baixo esforço)

## H. Resultado esperado
Ambiente reproduzível, resultados determinísticos com seed, execução não interativa do gravador de dados.

**Ferramentas usadas:** git, gh, PowerShell, Python 3.12 (py_compile), inspeção estática de notebooks.  
**Limitação:** TensorFlow não instalado; notebooks não executados.  
**Ambiente:** Windows, PowerShell 5.1, `C:\Users\User\audit-repos\EEG-model-personalization` (clone do upstream, branch main).
