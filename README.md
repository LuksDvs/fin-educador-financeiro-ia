# Fin — Educador Financeiro Inteligente

Desafio Final Bootcamp DIO — Assistente Virtual com Inteligência Artificial.

Fin é um assistente conversacional que ajuda a acompanhar reserva de emergência, entender produtos de renda fixa/variável e avaliar se um gasto cabe no orçamento. Arquitetura é RAG de verdade: os fatos vêm sempre da base de dados (`data/`), e quem redige a resposta é um LLM (Google Gemini 1.5 Flash), instruído por um system prompt anti-alucinação a nunca extrapolar o contexto — com fallback determinístico por palavras-chave caso a IA não esteja disponível.

## Estrutura do repositório
```
assistente-virtual-ia/
  README.md
  data/
    perfil_investidor.json
    transacoes.csv
    produtos_financeiros.json
    historico_atendimento.csv
  docs/
    documentacao.md
    prompts.md
    metricas.md
    pitch.md
  src/
    app.py              → versão Streamlit / CLI
    fin_colab.py        → mesmo código em .py
  notebooks/
    fin_colab.ipynb     → notebook pronto para Google Colab (ARQUIVO PRINCIPAL)
```

## Como rodar - fin_colab.ipynb (recomendado)
1. Abra https://colab.research.google.com
2. Arquivo > Fazer upload de notebook > selecione `notebooks/fin_colab.ipynb`
3. Execute as células em ordem. Na célula 3, cole sua API key gratuita do Google AI Studio (https://aistudio.google.com/apikey) ou aperte Enter para usar só modo por regras.
4. Na última célula, o chat interativo inicia automaticamente com `chat()`.

### Por que usei API do Gemini?
O desafio pede Assistente Virtual COM IA, não só if/else. O modo por regras (`responder_regras`) era robótico e dava "Gastar 900 = 86%" pra tudo. Com Gemini + RAG, o modelo entende linguagem natural, usa o contexto recuperado de `transacoes.csv` e `perfil_investidor.json` e responde citando fonte, com proteção anti-golpe para rentabilidade garantida.

## Opção 2 - Local
```bash
pip install -r requirements.txt
python notebooks/fin_colab.py
# ou
streamlit run src/app.py
```

## Os 6 passos do desafio
1. Documentação → docs/documentacao.md
2. Base de Conhecimento → data/
3. Prompts → docs/prompts.md
4. Aplicação Funcional → notebooks/fin_colab.ipynb
5. Avaliação → docs/metricas.md
6. Pitch → docs/pitch.md
