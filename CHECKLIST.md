# Checklist de desenvolvimento — Portal Nova Casa Igreja Viva

Legenda: `[ ]` pendente · `[~]` em andamento · `[x]` concluído

> Contexto do projeto, arquitetura e convenções ficam em [CLAUDE.md](CLAUDE.md).
> Mudanças na planilha são sempre aplicadas pelo usuário no Google Drive — nunca daqui.
> Itens marcados com **(planilha)** dependem dessa ação.

---

## Bloco 1 — Dados e indicadores ✅
> Maior impacto, menor esforço. Muda o que a página comunica, não como ela parece.

- [x] Corrigir `NaN%` nas barras quando todos os valores são zero
- [x] Ocultar categorias zeradas e exibir aviso quando não há nenhum lançamento
- [x] Site passou a ler `doacoes`; rótulo agora é "Doações recebidas"
- [x] Remover "ticket médio" dos indicadores (linguagem de empresa)
- [x] Substituir "metros conquistados" por "meta total" no 4º indicador, evitando exibir zero
- [x] Gráfico mês a mês com rótulo em cada barra, e acumulado como número ao lado
- [x] Retirar a linguagem de compra: a conquista do metro passa a ser coletiva
- [x] Trocar metro quadrado por cotas: 10.000 cotas de R$ 100, derivadas de `meta_total ÷ valor_cota`
- [ ] **(planilha)** Renomear dois rótulos na coluna A das abas `parametros` **e** `pub_kpis`
  — `doadores` → `doacoes` e `ticket_referencia` → `valor_cota`.
  Não apagar a linha `preco_m2`: as fórmulas usam posição de célula.
  *(o site aceita os dois nomes durante a transição)*
- [ ] Medir o tempo de renderização do mosaico de 10.000 quadradinhos em celular simples

## Incidente — publicação da planilha perdida (2026-09-13)
> Registrado para não repetir.

Uma sessão do Claude via extensão web reestruturou as abas `pub_` da planilha (a pedido de um
prompt deste projeto, para tornar `pub_origem`/`pub_despesas`/`pub_serie` automáticas). Isso
apagou a publicação inteira do documento — os 5 links antigos pararam de funcionar e teve que
ser tudo republicado, com `gid` novos.

- [x] Republicar as 5 abas e atualizar `CONFIG.GIDS` no `index.html` com os novos `gid`
- [x] Validar as 5 novas URLs e rodar o site contra os dados reais antes de considerar resolvido
- **Decisão**: `pub_serie` agora só lista meses com algum lançamento, em vez de um calendário
  fixo do início ao fim da campanha. Ficou assim de propósito — ver "Decisões tomadas" abaixo.
- [x] Reduzir de 6 para 4 indicadores, sem repetir o mesmo número em formatos diferentes
- [x] Usar a meta da etapa atual como referência principal, e não o total de R$ 1.000.000
- [x] Mover "saldo em caixa" dos indicadores de topo para a prestação de contas
- [x] Anel de progresso da etapa atual dentro do card

## Bloco 2 — Estrutura e usabilidade ✅
> Ordem da página e facilidade de doar. Pensado para leitura em celular.

- [x] Mover "Quem confere" para logo depois da prestação de contas, deixando "Doar" como fecho
- [x] Botão "copiar chave PIX" com confirmação visual e seleção do texto como alternativa
- [x] Publicar os dados bancários reais (ver "Decisões" abaixo)
- [x] Avisar que o nome na confirmação do banco é a razão social da igreja
- [x] Barra fixa discreta de doação no celular
- [x] Tabela reduzida a 3 colunas, com os saldos atrás de um botão
- [x] Encurtar as aberturas de seção e os textos de "Quem confere"
- [x] Remover os dois links mortos de documentos; sem link, o documento aparece como "em breve"
- [ ] **(planilha)** Criar a aba `pub_documentos` com as colunas `nome`, `url`, `data`, publicar
  em CSV e enviar o `gid` — a lista de documentos passa a sair dela
- [ ] Mover a seção de vídeos para o topo *(depende da gravação)*

## Bloco 3 — Identidade visual e movimento ✅
> Alinhar com o projeto do site da igreja. Movimento discreto, sem custo de bateria.

- [x] Cards arredondados com borda e gradiente sutil nos indicadores, gráficos, regras e doação
- [x] Selo de ícone em quadrado laranja nos quatro cards de "Quem confere"
- [x] Destaque em laranja nas palavras-chave dentro dos textos
- [x] Brilho de brasa deslizando lentamente atrás do topo, só em CSS
- [x] Contagem animada dos números quando entram na tela
- [x] Respeitar `prefers-reduced-motion` em todas as animações
- [x] Manter o estilo editorial na tabela, que continua sem cantos arredondados
- [x] Ícone em contorno laranja em cada indicador e em cada card de "Quem confere"
- [x] Sparkline do acumulado dentro do card "Arrecadado"
- [x] Trilha de etapas no card "Meta total", mostrando em qual das quatro estamos
- [x] Anel da etapa maior, com duas casas decimais
- [x] Entrada escalonada e realce sutil ao passar o mouse nos indicadores
- [x] Planilha local fora do versionamento *(a fonte de verdade é o Drive)*

---

## Pendências externas
> Dependem da igreja, não do desenvolvimento.

- [ ] Gravar os três vídeos → então mover a seção para o topo
- [ ] Enviar ao Drive os PDFs da ata da assembleia e do extrato da conta
- [ ] **(planilha)** Corrigir o passo 5 da aba `LEIA-ME`: não existe mais `CONFIG.PLANILHA_ID`,
  agora são `CONFIG.PUB_BASE` e `CONFIG.GIDS`
- [ ] Logo oficial em alta resolução
- [ ] Confirmar cidade e endereço exibidos no rodapé
- [ ] Definir o WhatsApp da tesouraria para envio de comprovante

## Antes de divulgar publicamente
- [ ] Conferir dados bancários com a tesouraria
- [ ] Testar a página em celular simples e em conexão lenta
- [ ] Validar os textos com alguém de fora da equipe do projeto

---

## Decisões tomadas

**Dados para doação**
- Chave PIX: `casaigrejaviva@gmail.com` (e-mail)
- Titular: Igreja Comunidade Cristã Aviva Nações
- CNPJ: 67.189.329/0001-13 · Banco: Cora SCFI
- Não exibir campos de agência e conta
- Exibir a razão social exata, já que é o nome que aparece na confirmação do banco

**Documentos em PDF**
- Arquivos hospedados no Google Drive, com link de leitura pública
- Lista alimentada por uma nova aba `pub_documentos` (colunas `nome`, `url`, `data`)
- Assim a tesouraria publica documentos novos sem precisar de desenvolvimento

**Unidade de doação**
- A casa é dividida em **cotas de R$ 100** — 10.000 no total, derivadas de `meta_total ÷ valor_cota`
- Metro quadrado foi descartado: o prédio tem 400 m², o que daria R$ 2.500 por metro, longe do
  valor acessível que a campanha precisa comunicar
- Nada de "comprar": ninguém compra pedaço da casa, a comunidade conquista junto
- O mosaico tem um quadradinho por cota, o que mostra o tamanho real da tarefa

**Gráfico mensal**
- `pub_serie` lista só os meses com algum lançamento, não um calendário fixo do início ao fim
- Um mês sem nenhuma doação nem despesa simplesmente não aparece no gráfico — decisão
  consciente, para manter a fórmula simples

**Movimento e identidade visual**
- Sem rede de partículas reagindo ao mouse: pesa em celular simples e não existe em toque
- Linguagem de cards do site da igreja nas seções de entrada; estilo editorial mantido nas de dado duro
