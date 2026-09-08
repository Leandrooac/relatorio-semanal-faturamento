# Relatório Semanal de Faturamento

Sistema de planilhas para acompanhamento semanal da carteira de faturamento (entreposto aduaneiro), com um painel executivo para a diretoria e um gerador de apresentações comerciais por cliente.

## Arquivos principais

| Arquivo | O que é |
|---|---|
| `Relatorio_Semanal_Faturamento.xlsx` | O relatório semanal operacional — a planilha "mãe", alimentada toda semana colando a base atualizada. |
| `Gerador_Apresentacao_Clientes.xlsx` | Gerador de apresentações comerciais: escolhe-se o cliente numa lista e todas as abas se refazem. |
| `apresentacoes/` | Exemplos de apresentações já geradas, uma por cliente. |
| `build_gerador.py`, `gerar_apresentacoes.py` | Scripts Python (openpyxl) que constroem os arquivos acima a partir da base — usados para regenerar o gerador quando o layout muda. |

## `Relatorio_Semanal_Faturamento.xlsx`

Planilha operacional com uma única fonte de dados (aba **BASE**) e várias visões derivadas por fórmula.

**Rotina semanal:** cole a extração da planilha operacional na aba `BASE` (colunas A até AS) e todo o arquivo recalcula sozinho.

Abas principais:
- **DIRETORIA** — painel executivo: panorama da carteira com comparativo semanal (aba `SEMANA ANTERIOR`), os três estados da carteira (faturável / a faturar / pendente), risco de fornecedor e exposição por cliente.
- **PAINEL** — os mesmos três estados com filtros de cliente, regime e incoterm.
- **PARA O CLIENTE** — documento sem custo, FOB ou margem, pronto para compartilhar com o cliente; aceita seleção de múltiplos clientes.
- **ANÁLISE PEDIDO**, **FATURÁVEL AGORA**, **PENDENTE SKU**, **POR PEDIDO**, **POR CLIENTE**, **RISCO FORNECEDOR**, **REGIME E TF**, **SUGESTÃO COMPRA** — recortes operacionais para vendas, compras e planejamento.
- **INSTRUÇÕES** — passo a passo de atualização semanal.

Conceitos-chave:
- **Margem Bruta** = Receita (preço de venda) − Preço FOB de compra. Não inclui frete, impostos, armazenagem nem despesas operacionais.
- **Risco de Fornecedor** incide apenas sobre o saldo pendente — item já disponível ou em trânsito conta como "Normal" mesmo que o fornecedor esteja em situação de risco.

## `Gerador_Apresentacao_Clientes.xlsx`

Planilha para montar, por cliente, o material de apresentação do "novo modelo de atendimento" (ondas de atendimento: DA/DI, DUIMP imediato, embarques por data de documento, pendências).

**Como usar:**
1. Cole a base semanal na aba `BASE` (colunas A até AS).
2. Cole a extração de itens já faturados/em nacionalização (CEDRS) na aba `BASE NACIONALIZACAO`.
3. Cole o catálogo de itens (SKU, quantidade por caixa) na aba `Sheet1`.
4. Na aba `INICIO`, escolha o cliente na lista suspensa — todas as abas se refazem.
5. Revise `NOVO MODELO`, `PARA O CLIENTE` e as abas `PED n - <categoria>` (as sem itens para o cliente se identificam sozinhas no título).
6. Para enviar: salve uma cópia e apague as abas de dados (`BASE`, `BASE NACIONALIZACAO`, `Sheet1`, `CODIGOS CLIENTE`, `CONFIG`, `INICIO`) e as abas `PED` vazias.

Abas geradas:
- **NOVO MODELO** — resumo por fornecedor e grade de produtos, com uma coluna por "onda" de atendimento (quantidade, saldo, valor em US$), categoria do produto (regra por fornecedor: RUHOF → Saneantes, GOJO/APODAN → Cosméticos, demais → Produtos para Saúde), coluna "Em Nacionalização" (itens já faturados, fora da cascata) e colunas manuais de Observações e Programação Futura.
- **PARA O CLIENTE** — posição item a item do cliente selecionado, com as datas do processo e colunas manuais de Observações, Sugestão, Pedido Dezembro e Pedido Janeiro.
- **PED 1 - DA-DI**, **PED 2/3/4… - PS/COS/SNT** — uma aba por onda × categoria regulatória, prontas para virar pedido de compra.

Clientes configurados na aba `CONFIG` (HIAE e SC-HIAE tratados como um único grupo comercial; demais clientes separados).

## Como regenerar

Os scripts Python usam `openpyxl` e recriam os arquivos do zero a partir da base mais recente:

```bash
python3 build_gerador.py        # gera Gerador_Apresentacao_Clientes.xlsx
python3 gerar_apresentacoes.py  # gera os arquivos individuais em apresentacoes/
