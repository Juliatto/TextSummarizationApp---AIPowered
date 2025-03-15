# **Projeto: Resumo Automático de Textos com IA**

Este projeto utiliza **LangChain** e **Claude-3 Opus** (Anthropic) para gerar resumos automáticos de textos. O código recebe um texto longo, divide-o em partes menores e gera um resumo utilizando um modelo de inteligência artificial.

## **Pré-requisitos**
Antes de iniciar, você precisará de:

- **Python 3.8+**
- **Git** (opcional, mas recomendado)
- **Conta na Anthropic AI** (para obter uma API Key)

---

## **1. Como instalar e executar o projeto**

### **1.1. Clonando o repositório**
Para copiar o repositório e acessar a pasta do projeto, utilize o terminal ou prompt de comando:

```bash
git clone https://github.com/seu-usuario/resumo-automatico-ia.git
cd resumo-automatico-ia
```

### **1.2. Criando e ativando um ambiente virtual**
#### **Windows (PowerShell)**
```powershell
python -m venv .venv
.venv\Scripts\activate
Set-ExecutionPolicy RemoteSigned -Scope Process
```

#### **macOS/Linux**
```bash
python -m venv .venv
source .venv/bin/activate
```

### **1.3. Instalando as dependências**
Após ativar o ambiente virtual, instale as bibliotecas necessárias:

```bash
pip install -r requirements.txt
```

Se o arquivo `requirements.txt` não existir, crie um e adicione:

```
langchain
langchain-community
langchain-anthropic
python-dotenv
anthropic
```

### **1.4. Configuração da API**
O projeto usa uma chave da **Anthropic AI**. Para configurar:

1. Crie um arquivo `.env` na raiz do projeto.
2. Adicione a seguinte linha, substituindo `"SUA_CHAVE_AQUI"` pela sua chave real:

```ini
ANTHROPIC_API_KEY=SUA_CHAVE_AQUI
```

---

## **2. Explicação do Código**

O código está estruturado em etapas, descritas a seguir.

### **2.1. Importação de Bibliotecas**
```python
from langchain_anthropic import ChatAnthropic
from langchain.docstore.document import Document
from langchain.text_splitter import CharacterTextSplitter
from langchain.chains.summarize import load_summarize_chain
from dotenv import load_dotenv, find_dotenv
import os
```
- `ChatAnthropic`: Interface para interagir com o modelo Claude-3.
- `Document`: Representa cada parte do texto como um objeto manipulável.
- `CharacterTextSplitter`: Divide o texto em pedaços menores.
- `load_summarize_chain`: Configura a pipeline de sumarização.
- `dotenv`: Permite carregar a chave da API do arquivo `.env`.
- `os`: Permite acessar variáveis de ambiente.

---

### **2.2. Carregar as Variáveis de Ambiente**
```python
load_dotenv(find_dotenv())
ANTHROPIC_API_KEY = os.environ["ANTHROPIC_API_KEY"]
```
- Lê o arquivo `.env` e carrega a chave `ANTHROPIC_API_KEY`.

---

### **2.3. Criar o Modelo de IA**
```python
llm = ChatAnthropic(
    model='claude-3-opus-20240229',
    temperature=1,
    anthropic_api_key=ANTHROPIC_API_KEY
)
```
- Define o modelo Claude-3 Opus para geração de resumos.
- `temperature=1` indica maior criatividade na geração do texto.

---

### **2.4. Definir o Texto para Resumo**
```python
text = (
    "O Brasil é um país localizado na América do Sul, sendo o quinto maior país do mundo em área territorial. "
    "É uma república federativa presidencialista, formada pela união de 26 estados federados e por um distrito federal. "
    "A capital do Brasil é Brasília e a cidade mais populosa é São Paulo. A língua oficial do Brasil é o português e a moeda é o real. "
    "O Brasil é um país com uma grande diversidade cultural e é conhecido por suas belezas naturais, como as praias, a floresta amazônica e as cachoeiras. "
    "A economia do Brasil é uma das maiores do mundo e é baseada na agricultura, na indústria e nos serviços. "
    "A hiperautomação transforma empresas ao otimizar os processos de negócios, eliminando tarefas repetitivas e automatizando as manuais. "
    "Isso traz vários benefícios importantes."
)
```
- Define um texto sobre o Brasil e hiperautomação que será resumido.

---

### **2.5. Dividir o Texto em Partes Menores**
```python
text_splitter = CharacterTextSplitter()
texts = text_splitter.split_text(text)
```
- O texto é dividido em partes menores para melhor processamento pelo modelo.

---

### **2.6. Criar Documentos para Sumarização**
```python
docs = [Document(page_content=chunk) for chunk in texts]
```
- Transforma cada pedaço de texto em um documento LangChain.

---

### **2.7. Carregar a Cadeia de Sumarização**
```python
chain = load_summarize_chain(llm=llm, chain_type="stuff")
```
- Configura a cadeia de sumarização usando o modelo Claude-3.

---

### **2.8. Gerar e Exibir o Resumo**
```python
summary = chain.invoke(docs)
print(summary['output_text'])
```
- O modelo gera o resumo e o resultado é exibido no terminal.

---

## **3. Como Executar**
Com todas as configurações feitas, execute o script:

```bash
python main.py
```

Isso imprimirá um resumo do texto definido no código.

---

## **4. Possíveis Erros e Soluções**

| Erro | Causa | Solução |
|------|-------|---------|
| `ModuleNotFoundError: No module named 'langchain_anthropic'` | Dependências ausentes | Rode `pip install -r requirements.txt` |
| `KeyError: 'ANTHROPIC_API_KEY'` | Chave da API ausente | Verifique se `.env` contém `ANTHROPIC_API_KEY=SUA_CHAVE_AQUI` |
| `AttributeError: 'Anthropic' object has no attribute 'count_tokens'` | Biblioteca desatualizada | Rode `pip install --upgrade langchain-community` |

---

## **5. Contribuição**
Sinta-se à vontade para abrir **issues** e **pull requests** para melhorias no projeto!
