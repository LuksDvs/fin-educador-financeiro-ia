# Documentação do Agente — Fin

## O que é
Fin é um assistente virtual de educação financeira, criado como desafio final do
Bootcamp DIO — Assistente Virtual com IA. Ele conversa com a pessoa usuária sobre
sua reserva de emergência, produtos de renda fixa/variável e se um gasto pontual
cabe no orçamento, sempre com base em dados estruturados (nunca "inventando" números).

## Para quem
Perfil de referência usado no protótipo: pessoa com renda mensal fixa, perfil de
risco moderado, que já iniciou uma reserva de emergência mas ainda não bateu a meta
— situação comum entre jovens profissionais que querem organizar as finanças sem
depender de um consultor humano para dúvidas simples do dia a dia.

## Como deve se comportar
- Responder apenas com base na base de conhecimento (`data/`).
- Nunca simular certeza sobre algo que não está nos dados.
- Alertar ativamente sobre promessas de rentabilidade fora da realidade (ex.: "5% ao mês garantido").
- Sempre citar a fonte da informação usada (nome do arquivo).
- Ser direto e didático — sem jargão financeiro sem explicação.
- Reconhecer quando não tem informação suficiente e dizer isso claramente, em vez
  de repetir uma resposta genérica de forma robótica.

## Arquitetura (RAG: recuperação + geração)
```
pergunta do usuário
      │
      ▼
montar_contexto()  ──► calcular_resumo() + perfil_investidor.json
                        + transacoes.csv + produtos_financeiros.json
      │
      ▼
LLM (Gemini) recebe: system prompt (8 regras anti-alucinação) + CONTEXTO + pergunta
      │
      ├── sucesso ──► resposta gerada pelo modelo, sempre citando a fonte
      │
      └── falha (sem chave/internet/cota) ──► responder_regras() [fallback por palavras-chave]
```

A parte de **recuperação** (grounding) é a mesma desde o início: nenhuma resposta
usa dado que não esteja em `perfil_investidor.json`, `transacoes.csv` ou
`produtos_financeiros.json`. A diferença é que agora quem **redige** a resposta em
linguagem natural é um modelo de linguagem (Gemini), instruído via system prompt a
nunca extrapolar o que está no contexto — e não mais um conjunto fixo de templates
de f-string. O motor por palavras-chave (`responder_regras`) continua existindo,
mas como rede de segurança quando a IA não está disponível, não como a lógica principal.

## Limitações conhecidas
- Base de conhecimento é um mock pequeno (fins de protótipo/portfólio).
- O reconhecimento de intenção é por palavras-chave, não por um modelo de NLP —
  perguntas muito fora do padrão caem no fallback honesto ("não tenho essa informação").
- Não substitui aconselhamento financeiro profissional.
