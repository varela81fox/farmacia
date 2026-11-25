---
name: anuncios-video-farmacia
description: Time de Marketing pt.2 (Fase 5 AutonomIA) para farmácia: roteiro em planilha, vídeo com avatar de IA e voz gravada, subida em lote no Meta Ads e diagnóstico de tráfego, com checagem sanitária.
---

# Anúncios em vídeo com avatar de IA — farmácia

Playbook da Fase 5 do curso Operação AutonomIA (Time de Marketing pt.2), adaptado para uma farmácia.
Cobre três times: **edição de vídeo com avatar**, **subida de criativos no Meta Ads** e **diagnóstico de tráfego** (Meta Ads / Google Ads).

Responda sempre em português do Brasil.

## Escopo — o que esta skill faz e não faz

Faz:
- Organiza o roteiro do criativo em planilha (cena a cena).
- Higieniza o áudio gravado, gera o avatar em lote (HeyGen via MCP/API), monta inserts/B-roll, lettering, logo, trilha e legenda palavra a palavra.
- Revisa cada vídeo com um "fiscal" antes da entrega.
- Gera legenda/descrição dos anúncios para aprovação e sobe os criativos em campanha e conjunto **já existentes**, sempre **pausados**.
- Faz diagnóstico de contas de anúncio (Meta e Google).

Não faz:
- Não inventa a voz nem o roteiro final sem aprovação humana.
- Não cria campanha, público ou orçamento, nem ativa anúncio.
- Não publica nada sem a **checagem sanitária** abaixo aprovada por um responsável da farmácia.

## Checagem sanitária (obrigatória em farmácia)

Antes de gerar ou subir qualquer criativo, revise o roteiro e a legenda contra estas regras e liste cada ponto como OK / RISCO:

1. **Medicamento de venda sob prescrição (tarja vermelha/preta)** não é anunciado ao público geral; fale de serviços, conveniência, atendimento farmacêutico, entrega, higiene/beleza, dermocosméticos, suplementos regularizados.
2. **Medicamento isento de prescrição (MIP)**: sem promessa de cura, sem "sem efeitos colaterais", sem incentivar uso indiscriminado ou automedicação, sem induzir autodiagnóstico; incluir as advertências exigidas (ex.: "se persistirem os sintomas, o médico deverá ser consultado").
3. Sem celebridade ou avatar "leigo" recomendando medicamento como se fosse profissional de saúde; avatar de farmacêutico só se for o farmacêutico real, com consentimento e CRF.
4. Sem antes/depois enganoso, sem depoimento inventado, sem preço ou promoção que não exista na loja.
5. Promoções e preços com validade e condições claras.
6. Conferir a política de anúncios da Meta/Google para farmácias e produtos de saúde antes de subir (pode exigir certificação ou restringir segmentação).

Base legal para conferir com o responsável/jurídico: Lei 6.360/1976, Lei 9.294/1996 (propaganda de medicamentos), RDC Anvisa 96/2008 (validade questionada no STJ em 2024 — tratar como referência de boa prática), Código de Ética Farmacêutica (CFF) e Anexo I do Código do Conar. **Esta checagem não substitui avaliação jurídica.** Na dúvida, marque RISCO e pare.

## Pré-requisitos

- Conta HeyGen no plano com acesso a API/MCP e avatar clone criado com **consentimento** da pessoa (mín. 15 s contínuos, ideal 1–2 min, peito para cima, vertical, sem mão na frente do rosto).
- Voz: **áudio gravado de verdade** (celular serve) é o padrão — é o que tira o "tom de IA". Alternativa: clone profissional na ElevenLabs (30+ min de áudio limpo).
- Python + FFmpeg instalados para tratamento de áudio e montagem.
- Banco de inserts/B-roll da farmácia (fachada, balcão, entrega, produtos permitidos, gravações de tela do app/site).
- Para subir anúncios: conta Business da Meta, app da Meta com token, campanha e conjunto já montados.
- Segredos (tokens HeyGen, ElevenLabs, Meta, Z-API) em `.env` — nunca no git.

## Fluxo 1 — Roteiro em planilha

Crie/leia a planilha de roteiro (`roteiros/<anuncio>.csv` ou Google Sheets) com as colunas:

| coluna | conteúdo |
|---|---|
| ordem | número da cena |
| fala | texto exato falado pelo expert |
| fase | hook (3–5 s), body ou CTA |
| tipo_cena | avatar sozinho · inserção de vídeo · lettering · logo · avatar + inserção |
| palavra_destaque | palavra para destacar na legenda |
| link_broll | YouTube, Instagram, Drive ou banco de inserts |
| zoom | sim/não |
| avatar_cantinho | sim/não |

O Claude pode sugerir o preenchimento, mas o usuário aprova. Rodar a checagem sanitária sobre a coluna `fala` antes de seguir.

## Fluxo 2 — Time de edição de vídeo (pipeline)

Execute em ordem; cada etapa grava sua saída em `videos/<anuncio>/<etapa>/`:

1. **Produtor** — lê a planilha/documento de copy e monta a lista de cenas.
2. **Engenheiro de áudio** — limpa o áudio em Python (ruído, volume, cortes). A boca do avatar vem do áudio: áudio ruim = vídeo ruim.
3. **Geração do avatar** — envia o áudio por cena ao HeyGen (MCP/API) em lote.
4. **Diretor de arte** — posição do lettering, logo e zonas seguras do Reels (não cobrir rosto nem a interface do Instagram).
5. **Editor de imagem** — inserts/B-roll, zoom e avatar no cantinho conforme a planilha.
6. **Editor de texto** — legenda palavra a palavra com o preset escolhido (ofereça ~10 estilos na primeira vez e salve a escolha).
7. **Sound designer** — trilha e efeitos de transição.
8. **Fiscal (gate)** — confere enquadramento, colisão de texto com rosto, sincronia labial e fidelidade ao roteiro; devolve o vídeo para a etapa que errou.
9. **Auditoria opcional** — nota de 0 a 10 por cena.
10. **Entregador** — sobe para o Drive e avisa (ex.: WhatsApp via Z-API) com o link, apenas após aprovação.

Tempo de referência: ~10 min por vídeo.

## Fluxo 3 — Meta Ads Uploader

1. Listar criativos aprovados (imagem e vídeo) e a checagem sanitária de cada um.
2. Gerar legenda/descrição de cada anúncio → mostrar ao usuário para aprovação.
3. Subir para a campanha e o conjunto informados, **sempre com status PAUSADO**.
4. Devolver relatório: anúncio, criativo, conjunto, status, link para revisão no Gerenciador.

## Fluxo 4 — Diagnóstico de tráfego

**Meta Ads** (via MCP da Meta): onboarding da conta → hierarquia campanha/conjunto/anúncio → gasto anormal → auditoria de pixel e API de Conversões (CAPI) → reconciliar vendas registradas com as vendas reais da farmácia (sistema/ERP/e-commerce) → recomendações de estratégia e ciclo de criativos.

**Google Ads**: apenas diagnóstico (o MCP oficial exige conta MCC e token de desenvolvedor).

Toda recomendação traz o número e a fonte. Nenhuma alteração na conta sem aprovação explícita.

## Variações de criativo a partir de uma gravação

- **Troca de locação** (fadiga de cenário): gerar o novo fundo e trocar mantendo rosto, voz, expressão e iluminação compatível — revisar sombra e luz antes de aprovar.
- **Multi-take**: gerar vários ângulos (perfil, close-up, plongée) a partir de um take estático.
- Antes de gravar, pedir ao Claude para **dirigir as cenas** (enquadramento, intenção, ângulos).

## Organização no repositório

```
roteiros/            planilhas/CSV de roteiro por anúncio
videos/<anuncio>/    saídas por etapa do pipeline
inserts/             índice do banco de B-roll (não versionar vídeos pesados; usar Drive/R2)
anuncios/            relatórios de subida e diagnósticos
docs/decisoes.md     documentação viva: presets escolhidos, ajustes de estilo, aprendizados
.env                 tokens (no .gitignore)
```

Adicione ao `.gitignore`: `.env`, `*.mp4`, `*.mov`, `*.wav` (mídia pesada vai para armazenamento externo).

## Princípios

- Voz gravada de verdade > voz sintética.
- Mapeie manual primeiro, automatize depois.
- Ferramenta é commodity; calibragem (roteiro, estilo, checagem) é o diferencial — registre em `docs/decisoes.md`.
- Sem checagem sanitária aprovada, nada sai do repositório.

## Formato de entrega

Ao final de cada fluxo, responda curto: o que foi produzido, resultado da checagem sanitária (OK/RISCO por item), onde estão os arquivos, o que precisa de aprovação e o próximo passo.
