# DATATHON ESPM 2026 
**Meridional · Inteligência com evidências**
**Converse com os dados. Compare cenários. Entenda o que sustenta cada resultado.**

Projeto da equipe **Coffeelanders** para o Datathon ESPM 2026, voltado à análise dos efeitos da reforma tributária no desafio da Atacadista Meridional.
Equipe formada por: **Ana Proença, Gustavo Gomes, Julia Egute, Pedro Perroni e Ricardo Matteo.**

A solução reúne classificações fiscais recebidas, simulações e uma aplicação conversacional em Python. Permite explorar produtos, fornecedores e operações em português, com gráficos, contexto entre perguntas e acesso às evidências.

> **Contexto:** os dados são sintéticos e as regras seguem as premissas congeladas do desafio. Os resultados são simulações acadêmicas, não economia financeira realizada ou orientação tributária.

## O que você encontra aqui

| Arquivo | Conteúdo |
| ---     | ---       |
| [Aplicação Python e notebook](meridional_colab.zip) | Código completo, dependências, dados recebidos, testes, documentação e notebook para Colab. Baixe pelo botão de download do arquivo. |
| [Portfólio do agente](Portfolio_Agente_Meridional.docx) | Explicação da solução, arquitetura, decisões técnicas, testes e limitações. |
| [Apresentação da equipe](Coffeelanders_Pitch_Final.pptx) | Narrativa do desafio, abordagem de classificação e análise dos cenários. |
| [Dashboard Tableau](Dashboard.twbx) | Workbook empacotado, separado do dashboard interativo da aplicação Python. Requer software compatível com Tableau. |
| [Simulação](simulacao.json) | Resultados por item e resumo dos cenários hoje, 2027 e 2033. |
| [Amostra de classificações](itens_classificados.xlsx) | Quatro registros ilustrativos. Não representa a base completa nem substitui o CSV final preservado no pacote. |

O código está atualmente dentro do ZIP. Após extrair, consulte `README.md`, `meridional/`, `tests/` e `docs/` para conhecer a implementação.

## O que a aplicação faz

- Compara os cenários de hoje, 2027 e 2033.
- Apresenta rankings de contribuições positivas e negativas por produto, fornecedor, destinatário e finalidade.
- Separa o efeito dos débitos das vendas do efeito dos créditos das compras.
- Mantém filtros e cenário na conversa, inclusive em continuações como “E em 2027?”.
- Mostra fontes, auditoria e exportação de todos os registros do recorte.
- Simula alterações explícitas de preços em 2033, mantendo quantidades e mix constantes por hipótese.
- Informa lacunas, solicita esclarecimentos e recusa perguntas fora do escopo.

O agente utiliza as classificações da referência v5; não treina um novo classificador fiscal nem certifica a precisão das classificações recebidas.

## Como funciona

```text
Pergunta em português + contexto da conversa
                    ↓
API OpenAI: interpretação e plano estruturado
                    ↓
Python: validação dos parâmetros e ferramentas permitidas
                    ↓
Cálculos locais ou recuperação de fontes
                    ↓
Resposta com evidências + dashboard + exportação
```

A IA interpreta; Python calcula. O modelo não executa código arbitrário nem altera arquivos originais. A resposta final é composta localmente com números calculados e fontes disponíveis.

O motor utiliza `Decimal` e `ROUND_HALF_UP`, arredondando CBS e IBS separadamente por item. Os registros são relacionados por `chave + nItem`, preservando códigos textuais e zeros. Os rankings completos reconciliam com os totais do mesmo recorte.

**Tecnologias:** Python 3.12, Gradio, Plotly, Pydantic, pypdf, python-docx, API OpenAI, pytest e Playwright/Chromium. Modelo padrão configurável: `gpt-4.1-mini`.

## Como experimentar

### No Google Colab

1. Baixe `meridional_colab.zip` e extraia uma cópia para localizar `Meridional_IA.ipynb`. Guarde também o ZIP original.
2. Abra o Google Colab e use **Arquivo → Fazer upload de notebook** para abrir esse notebook.
3. Execute a primeira célula, envie o ZIP da aplicação e execute a instalação.
4. Para experimentar sem API, escolha explicitamente **Experimentar modo local · sem IA**. Para interpretação com IA, configure uma chave própria por segredo do Colab ou entrada protegida e selecione o modo OpenAI.
5. Execute a interface, faça uma pergunta e consulte **Fontes e registros**. O notebook também oferece conversa direta alternativa.

O modo local é limitado e não interpreta livremente qualquer pergunta. A API requer acesso e créditos próprios; a assinatura do ChatGPT não substitui esse acesso. Não coloque chaves no repositório. A API recebe a pergunta e contexto resumido, sem envio da base integral.

A interface usa `share=False`, sem criar um túnel Gradio público. O funcionamento completo no Colab ainda precisa de validação nativa; os testes registrados foram executados no ambiente local.

### Em ambiente Python

Extraia o ZIP, abra um terminal na pasta extraída e execute:

```bash
python -m venv .venv
# Ative o ambiente virtual conforme seu sistema operacional.
python -m pip install -r requirements.txt
python app.py
```

Abra o endereço local informado no terminal. As instruções detalhadas estão no README incluído no pacote.

## Perguntas para explorar

- Quais produtos têm o maior ganho tributário em 2033?
- Quais fornecedores concentram o aumento dos créditos?
- E em 2027?
- O resultado vem das vendas ou dos créditos das compras?
- Qual é o efeito das compras para uso e consumo?
- Por que esse produto recebeu essa classificação?
- Quais documentos sustentam essa conclusão?
- Saldo negativo significa prejuízo?

## Escala e resultados verificados

A referência contém **15.000 itens**, **1.387 chaves de nota**, **95 códigos de produto** e **19 fornecedores**. CSV final, JSON da v5 e base de itens apresentam os mesmos pares `chave + nItem`, sem duplicatas ou registros sem correspondência nessa junção.
