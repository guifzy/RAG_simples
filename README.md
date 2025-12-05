# proj4 — Busca Semântica em Textos de Filosofia

Este repositório implementa um fluxo de Recuperação de Informação com Embeddings (RAG/Vector Search) sobre textos clássicos de filosofia em português, a partir de PDFs. O notebook `proj4.ipynb` constrói um índice semântico com FAISS a partir de chunks textuais extraídos e anotados (NER), permitindo consultas por similaridade e exploração guiada do conteúdo.

## Objetivos
- Explorar e compreender clusters em buscas semanticas e atravez de embbedings.
- Permitir busca semântica (não apenas por palavra exata) e navegação contextual.
- Enriquecer os trechos com anotações de Entidades Nomeadas (NER) para análise e referências.

## Principais Stacks e Bibliotecas
- **Python**: 3.10+ recomendado
- **Jupyter Notebook**: execução do fluxo em `proj4.ipynb`
- **Leitura e preparação de dados**: `pypdf`, `pandas`, `numpy`
- **Splitters e orquestração**: `langchain`, `langchain-text-splitters`
- **Embeddings**: `sentence-transformers`, `transformers`, `torch`, `accelerate`, `safetensors`
- **Índice vetorial**: `faiss-cpu`
- **NLP/NER**: `spacy` (modelo pt a ser instalado, ver instruções)

As dependências principais estão em `requirements.txt`.

## Estrutura do Repositório
- `proj4.ipynb`: notebook com todas as etapas do pipeline (carregamento, chunking, NER, embeddings, FAISS e consultas).
- `textos/`: PDFs de filosofia usados como fonte (por exemplo: Apologia de Sócrates, Carta a Meneceu, Mito da Caverna).
- `chunks_filosofia.json`: chunks já processados com metadados e NER (ex.: título, autor, página, `chunk_id`, `texto`, `ner`).
- `requirements.txt`: lista de dependências Python.

## Como o Projeto Foi Construído (Resumo do Pipeline)
1. **Coleta**: leitura dos PDFs em `textos/` com `pypdf`.
2. **Pré‑processamento**: limpeza e normalização do texto.
3. **Segmentação**: divisão em chunks (via `langchain-text-splitters`) com sobreposição para preservar contexto.
4. **Anotação (NER)**: identificação de entidades (pessoas, locais, obras) com `spacy`.
5. **Embeddings**: vetorização dos chunks via `sentence-transformers`/`transformers` + `torch`.
6. **Indexação**: construção de um índice vetorial com `faiss-cpu` para busca por similaridade.
7. **Consulta**: consulta por texto livre, recuperação dos top‑k trechos e exibição com metadados.

Observação: o arquivo `chunks_filosofia.json` evidencia a etapa de NER e a estrutura dos metadados gerados no pipeline.

## Execução em Ambiente Local (Windows/PowerShell)
Pré‑requisitos:
- Python 3.10 ou superior
- `git` (opcional, caso já tenha o diretório local não é necessário)

1) Criar e ativar um ambiente virtual:
```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -U pip
```

2) Instalar dependências:
```powershell
pip install -r requirements.txt
```

3) Instalar o modelo de português para spaCy (necessário para NER):
```powershell
python -m spacy download pt_core_news_sm
```

4) Abrir o Jupyter e executar o notebook:
```powershell
python -m pip install jupyter
jupyter notebook proj4.ipynb
```
No Jupyter, execute as células na ordem. As células constroem/atualizam o índice e permitem fazer consultas semânticas.


## Exemplos de Uso (no Notebook)
- Executar uma consulta do tipo: “Quais são os principais argumentos na Apologia de Sócrates?” para recuperar trechos relevantes com páginas e título da obra.
- Explorar entidades nomeadas detectadas (ex.: `Sócrates`, `Zeus`) para identificar passagens e contextos.

## Considerações Finais
Este projeto foca na aplicação prática de RAG (Retrieval‑Augmented Generation) e buscas semânticas sobre textos filosóficos em português, enfatizando como representações em espaços latentes capturam significado além de termos exatos. Ao combinar chunking contextual, embeddings (via `sentence-transformers`/`transformers`) e indexação vetorial (`faiss-cpu`), a recuperação torna‑se mais coesa e fiel ao conteúdo original.

Um ponto central é a integração de NER (Reconhecimento de Entidades Nomeadas) no pipeline: ao evidenciar entidades como autores, personagens e conceitos (por exemplo, `Sócrates`, `Platão`, `Zeus`), os vetores dos chunks tendem a se organizar em clusters mais estáveis no espaço latente. Essa sinalização semântica auxilia o modelo a diferenciar contextos e a aproximar trechos relacionados, melhorando a densidade dos agrupamentos e, consequentemente, a precisão e a cobertura da recuperação.

Do ponto de vista acadêmico, esse comportamento de coesão em espaço vetorial permite:
- Análise de tópicos e conceitos com maior granularidade.
- Navegação guiada por entidades, reforçando citações e referências.
- Expansão de consultas com termos semanticamente próximos sem perder fidelidade.

O notebook pode ser estendido para: (i) incorporar reescrita de respostas com LLMs baseada nos trechos recuperados (RAG completo), (ii) ajustar estratégias de chunking/overlap para balancear contexto e custo computacional, (iii) experimentar diferentes famílias de embeddings e métricas (cosine/inner product), e (iv) integrar novos corpora e validar qualidade com métricas de IR (recall@k, MRR). Esses caminhos ampliam tanto a robustez quanto o valor acadêmico do sistema de recuperação.
