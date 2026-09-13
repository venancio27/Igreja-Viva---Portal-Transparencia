# Projeto: Portal da Transparência — Campanha Casa Igreja Viva

## Contexto e motivação

A comunidade fazia parte da Igreja Quadrangular nacional. Após escândalos da liderança
nacional, exigências de transferir a titularidade de imóveis para o presidente, retenção de
repasses a missionários e aumento unilateral do percentual de dízimos/ofertas repassado
(de 12% para 20%, sem contrapartida), a igreja decidiu em assembleia se desassociar da
sede nacional e responder diretamente à Foursquare Internacional.

O prédio atual foi comprado (~2012) e construído/reformado ao longo dos anos inteiramente
com recursos da própria comunidade (dízimos, ofertas, tempo voluntário). A ex-liderança
nacional entrou com ação judicial para tomar o imóvel; uma decisão liminar já proíbe a
comunidade de permanecer no local enquanto o processo corre, criando risco de despejo a
qualquer momento.

Diante disso, a igreja lançou uma campanha de arrecadação popular (venda de balas em
sinaleiro, lavagem de carro, rifas, camisas, doações diretas, conversas com empresários
etc.) para comprar um novo imóvel na região. **Prioridade explícita do pastor e da
liderança: transparência total** sobre o dinheiro arrecadado — daí a decisão de construir
um portal/dashboard público de prestação de contas.

> Esses fatos são a motivação do projeto e não mudam com frequência — mantidos aqui como
> pano de fundo. Valores da meta, saldo atual, número de doadores, vídeos publicados etc.
> são dados **dinâmicos** e vivem na planilha do Google Sheets / no HTML, não neste arquivo.

## Objetivo do site

Portal público, bonito e profissional (não é um site institucional comum — é uma
prestação de contas), mostrando:
- KPIs da campanha (arrecadado, % da meta, doadores, faltantes)
- Timeline/fases da campanha
- Visualização "metro quadrado" (R$ 500/m² — doador entende exatamente o que comprou)
- Gráfico de evolução da arrecadação + breakdown por origem/despesas
- Vídeos explicando o propósito (pastor, tesouraria, visita ao imóvel)
- Chave PIX e outras formas de doar
- Tabela de entradas/saídas ligada à planilha oficial

## Fonte de dados

Uma planilha Google Sheets (preenchida manualmente pela tesouraria) é a fonte única de
verdade, com 5 abas: `pub_kpis`, `pub_serie`, `pub_origem`, `pub_despesas`, `pub_fases`.
Cada aba é publicada individualmente via **Arquivo → Compartilhar → Publicar na
web → CSV**, o que gera uma URL fixa por aba (`.../pub?gid=<id da aba>&output=csv`), todas
sob o mesmo `CONFIG.PUB_BASE` em `portal-casa-igreja-viva.html` — só o `gid` muda por aba
(mapeado em `CONFIG.GIDS`). Sem backend, sem build step. Enquanto `PUB_BASE`/`GIDS` não
estiverem configurados, o site roda em modo demonstração com dados fake e um aviso visível
disso.

Os valores nas células vêm formatados como moeda BR (`R$ 1.234,56`) e as abas trazem
linhas de instrução para quem preenche (ex.: "Digite os meses no formato aaaa-mm") — o
parser CSV do site (`parseCSV`) já remove o prefixo `R$`, converte separadores pt-BR e
descarta essas linhas de instrução automaticamente.

## Decisões de design

- **Sem meta "R$ 1 milhão" exposta de forma crua** — pode assustar; preferir enquadrar em
  unidades menores e tangíveis (ex.: "faltam X pessoas doando R$100", "faltam Y metros
  quadrados").
- **Modelo "R$ 500 por m²"**: doação vinculada a uma unidade física do imóvel — modelo
  clássico em igrejas, gera clareza sobre o que a doação "comprou". Representado
  visualmente como um mosaico/imagem do prédio que vai "preenchendo" (colorido) conforme
  a arrecadação avança, e o restante em tons de cinza (a conquistar).
- **Paleta de cores da própria igreja** (fogo/dourado sobre fundo escuro — carvão, tijolo,
  brasa, ouro), já implementada em `:root` no CSS do HTML.
- **Referência de estilo/interação** (não copiar, só inspiração): portfólio com efeito de
  "holofote" seguindo o mouse e elementos que reagem ao cursor
  (https://manoelja.vercel.app/) — aplicado de forma sutil, ex. no efeito "prédio que
  acende" ao passar o mouse na imagem hero.
- Site estático (HTML/CSS/JS puro, sem framework/build), pensado para publicação simples
  (GitHub Pages ou similar) e fácil manutenção por quem não é desenvolvedor de profissão
  (o responsável técnico se descreve como cientista de dados fazendo "vibecoding").

## Stack

- HTML/CSS/JS vanilla em arquivo único (`portal-casa-igreja-viva.html`)
- Fontes: Google Fonts (Archivo / Archivo Black)
- Dados: Google Sheets publicado como CSV (sem backend, sem build step)
- Hospedagem prevista: GitHub (repositório público ou GitHub Pages)

## Arquivos do projeto

- `portal-casa-igreja-viva.html` — o site (primeira versão)
- `campanha-casa-igreja-viva.xlsx` — planilha de referência/rascunho local (a fonte oficial
  em produção é o Google Sheets publicado, não este arquivo)

## Próximos passos / em aberto

- Definir e plugar o `PLANILHA_ID` real do Google Sheets
- Substituir logo/imagens de simulação por mídia oficial da igreja
- Gravar e linkar os vídeos (propósito, visita ao imóvel, como o dinheiro é conferido)
- Confirmar chave PIX definitiva
- Decidir formato final de exibição da meta (evitar número "assustador")
- Iterar sobre o mosaico de metros quadrados (arte final da fachada)
