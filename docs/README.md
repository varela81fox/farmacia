# docs

## plano-site.html

Plano de produto do e-commerce da farmácia de manipulação — v2, 11/09/2026.

Premissas confirmadas pelo cliente: o laboratório já opera há anos e está regularizado
(AFE e RT ativos), e o escopo de venda inclui suplementos **e** medicamentos manipulados.

Cobre: quatro perfis de demanda, benchmark da Oficialfarma, restrições regulatórias
(RDC 67/2007, RDC 44/2009, RDC 20/2011, Portaria 344/98, RDC 96/2008, IN 28/2018,
LGPD art. 11, CDC art. 49), as três faixas sanitárias que governam o catálogo,
arquitetura de informação com as duas portas de entrada, modelo de dados de produto e de
solicitação de fórmula, especificação bloco a bloco das páginas, os dois fluxos de
receita como máquina de estados, conversão e retenção, stack recomendada, pagamento e
frete, SEO, direção visual, métricas e roadmap em 5 fases.

Decisões estruturantes da v2: duas naturezas de venda no mesmo site (catálogo pronto e
fórmula magistral sob prescrição); faixa sanitária como campo de cadastro que bloqueia
publicação; captura de pagamento condicionada à triagem farmacêutica; stack headless
desde o início, com Fase 0 de 3 semanas capturando receita antes de a loja existir;
portal do prescritor como canal de aquisição próprio de um laboratório estabelecido.

Versão publicada (navegável):
https://claude.ai/code/artifact/3dec07bf-1538-44b8-b9be-f4061741b3e0

### Formato

O arquivo é o *corpo* de uma página — sem as tags `<!doctype>`, `<html>`, `<head>` e
`<body>`, que são adicionadas na publicação. Para abrir localmente, envolva o conteúdo
nessas tags ou republique pelo mesmo caminho para atualizar a versão hospedada.

### Pendências antes de tratar o plano como decidido

- Validação de todo o capítulo regulatório pelo farmacêutico responsável e por advogado
  sanitarista. Nenhum item da seção 04 é parecer jurídico.
- Respostas às sete perguntas da seção 17. A primeira — se o ERP do laboratório tem API —
  é a maior incógnita de custo e prazo da Fase 1 e deve ser levantada antes de qualquer
  desenvolvimento.
