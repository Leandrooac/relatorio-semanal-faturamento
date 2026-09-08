Relatório Semanal de Faturamento — Entreposto Aduaneiro

Sistema de planilhas para acompanhamento semanal da carteira de faturamento em regime de Entreposto Aduaneiro, com painel executivo para diretoria e um gerador de apresentações comerciais por cliente. Desenvolvido individualmente, incluindo automação em Python para geração dos arquivos de apresentação.

Contexto

O acompanhamento da carteira de faturamento (itens faturáveis, a faturar e pendentes) era feito de forma manual e fragmentada, sem uma visão executiva consolidada nem um processo padronizado para gerar material de apresentação comercial por cliente. Criei este sistema para resolver as duas pontas: um painel semanal para a diretoria, e um gerador automatizado de apresentações para a equipe comercial.

Arquivos principais
Arquivo	O que é
Relatorio_Semanal_Faturamento.xlsx	Relatório semanal operacional — planilha "mãe", alimentada toda semana com a base atualizada.
Gerador_Apresentacao_Clientes.xlsx	Gerador de apresentações comerciais: seleciona-se o cliente em uma lista e todas as abas se refazem automaticamente.
apresentacoes/	Exemplos de apresentações já geradas (uma por cliente).
build_gerador.py, gerar_apresentacoes.py	Scripts em Python (openpyxl) que constroem os arquivos acima a partir da base — usados para regenerar o gerador quando o layout muda.
Relatorio_Semanal_Faturamento.xlsx

Planilha operacional com uma única fonte de dados (aba BASE) e múltiplas visões derivadas por fórmula.

Rotina semanal: cola-se a extração atualizada na aba BASE, e o arquivo recalcula todas as demais visões automaticamente.

Abas principais:

DIRETORIA — painel executivo com panorama da carteira, comparativo semanal, os três estados da carteira (faturável / a faturar / pendente), risco de fornecedor e exposição por cliente.
PAINEL — os mesmos três estados, com filtros por cliente, regime e incoterm.
PARA O CLIENTE — documento sem custo, FOB ou margem, pronto para compartilhamento externo.
Abas operacionais de apoio a vendas, compras e planejamento (análise por pedido, itens faturáveis, pendências por SKU, risco de fornecedor, sugestão de compra, entre outras).
INSTRUÇÕES — passo a passo de atualização semanal.

Conceitos-chave:

Margem Bruta = Receita (preço de venda) − Preço FOB de compra. Não inclui frete, impostos, armazenagem ou despesas operacionais.
Risco de Fornecedor incide apenas sobre o saldo pendente — item já disponível ou em trânsito é tratado como "Normal", mesmo que o fornecedor esteja em situação de risco.
Gerador_Apresentacao_Clientes.xlsx

Planilha para montar, por cliente, o material de apresentação comercial, organizado por ondas de atendimento (com base no regime de importação e datas de documentação).

Como usar:

Colar a base semanal atualizada.
Colar a extração de itens já nacionalizados/faturados por outra entidade do grupo.
Colar o catálogo de itens (SKU, quantidade por unidade de embalagem).
Selecionar o cliente em uma lista suspensa — todas as abas se refazem.
Revisar as abas geradas antes do envio.
Para enviar: salvar uma cópia e remover as abas de dados internos antes de compartilhar externamente.

Abas geradas:

Resumo por fornecedor e grade de produtos, com uma coluna por onda de atendimento (quantidade, saldo, valor), categorização de produto por regra de negócio interna, e colunas manuais de observações e programação futura.
Posição item a item do cliente selecionado, com datas do processo.
Uma aba por onda × categoria regulatória, prontas para virar pedido de compra.
Como regenerar

Os scripts em Python usam a biblioteca openpyxl e recriam os arquivos do zero a partir da base mais recente:

bash
python3 build_gerador.py        # gera Gerador_Apresentacao_Clientes.xlsx
python3 gerar_apresentacoes.py  # gera os arquivos individuais em apresentacoes/
Ferramentas
Microsoft Excel (fórmulas avançadas, estrutura de múltiplas abas interligadas)
Python (openpyxl) para automação da geração de arquivos
Sobre os dados neste repositório

Nenhum dado real de clientes, fornecedores, valores ou volumes está incluído. Nomes de clientes e fornecedores citados na operação original foram omitidos deste repositório por serem informação comercial confidencial. Os arquivos de trabalho reais (.xlsx e dados de entrada) não estão incluídos neste repositório — este README documenta a arquitetura e a lógica da solução, não o sistema operacional original, que é propriedade da empresa em que foi desenvolvido.
