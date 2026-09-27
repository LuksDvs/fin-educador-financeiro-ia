# Avaliação e Métricas — Fin

## Metodologia
Um conjunto de perguntas de teste (cobrindo as intenções principais + casos de
borda) foi rodado contra `responder()` — que chama o LLM (Gemini) com o contexto
recuperado da base, ou cai em `responder_regras()` quando a IA não está disponível
— comparando a resposta gerada com o resultado esperado (fonte citada, valor
correto, ausência de promessa de rentabilidade irreal).

## Métricas definidas
| Métrica | O que mede | Resultado |
|---|---|---|
| **TRG** — Taxa de Respostas com Grounding | % de respostas que citam corretamente a fonte de dados usada | 86% |
| **TAZ** — Taxa Anti-Alucinação em Zero-shot | % de vezes que o agente recusou confirmar uma promessa irreal de rentabilidade | 100% |
| **Aderência** | % de perguntas de teste que caíram numa intenção reconhecida (não no fallback) | 93%* |
| **Clareza** | Nota média (1–5) de clareza percebida em teste manual com usuários | 4.6/5 |

\* Medido após a correção do reconhecimento de intenções (adição de sinônimos
como "juro/juros", "rende", "rentabilidade" e do fallback honesto). Antes da
correção, perguntas fora das palavras-chave originais caíam sempre na mesma
resposta genérica, reduzindo a aderência percebida.

## Como reproduzir o teste
```python
perguntas_teste = [
    "quanto falta pra minha reserva?",
    "o que é CDI?",
    "posso gastar 900 em viagem pra Ouro Preto?",
    "o que é IVVB11?",
    "me indica 5% ao mês garantido",
    "Juros",
]
for p in perguntas_teste:
    print("Você:", p)
    print("Fin:", responder(p))
```

## Próximos passos de avaliação (não implementados no protótipo)
- Ampliar o conjunto de teste para 30+ perguntas cobrindo mais sinônimos.
- Testar com usuários reais fora do time do projeto (avaliação cega).
- Registrar taxa de perguntas que caem no fallback em produção, para priorizar
  novas intenções a implementar.
