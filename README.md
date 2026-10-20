# Relatório Semanal de Faturamento — Entreposto Aduaneiro

Sistema de planilhas para acompanhar semanalmente a carteira de faturamento em regime de entreposto aduaneiro, com painel executivo para a diretoria e um gerador de apresentações comerciais por cliente.

A lógica de negócio e a estrutura foram especificadas por mim. A automação em Python foi implementada com apoio de ferramentas de IA generativa, enquanto aprendo a linguagem.

> Este repositório documenta a arquitetura e a lógica da solução. Os arquivos de trabalho (planilhas, scripts e dados) não estão incluídos porque pertencem à empresa e contêm informação comercial confidencial.

## Contexto

O acompanhamento da carteira (itens faturáveis, a faturar e pendentes) era manual e fragmentado, sem visão executiva consolidada e sem processo padronizado para gerar material de apresentação por cliente. A solução atende as duas pontas: um painel semanal para a diretoria e um gerador de apresentações para a equipe comercial.

## Componentes da solução

| Componente | O que faz |
|---|---|
| Relatório semanal | Planilha "mãe", alimentada toda semana com a base atualizada |
| Gerador de apresentações | Seleciona-se o cliente em uma lista e todas as abas se refazem |
| Scripts em Python (openpyxl) | Reconstroem os dois arquivos do zero quando o layout muda |

### Relatório semanal

Uma única fonte de dados (aba BASE) e várias visões derivadas por fórmula. A rotina semanal é colar a extração atualizada na BASE; o resto recalcula sozinho.

- **Diretoria**: panorama da carteira, comparativo semanal e os três estados da carteira (faturável, a faturar, pendente).
- **Painel**: os mesmos três estados, com filtros por cliente, regime e incoterm.
- **Versão para o cliente**: documento sem informações internas de custo, pronto para compartilhar.
- **Abas de apoio** a vendas, compras e planejamento.
- **Instruções**: passo a passo da atualização semanal.

### Gerador de apresentações

1. Colar a base semanal atualizada.
2. Selecionar o cliente na lista suspensa: todas as abas se refazem.
3. Revisar e remover as abas internas antes de enviar.

As abas geradas trazem o resumo por fornecedor, a posição item a item do cliente e a programação por onda de atendimento.

## Ferramentas

- Microsoft Excel (fórmulas avançadas, múltiplas abas interligadas)
- Python (openpyxl) para gerar os arquivos, implementado com apoio de ferramentas de IA generativa como parte do meu aprendizado da linguagem

## Sobre os dados neste repositório

Nenhum dado real de clientes, fornecedores, valores ou volumes está incluído.
