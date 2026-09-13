# Portal da Transparência — Campanha Nova Casa Igreja Viva

Portal público de prestação de contas da campanha de compra de um prédio para a Igreja Viva
Foursquare. Site estático, sem build, alimentado por uma planilha do Google.

- **Por que o projeto existe e as decisões de concepção:** [CONTEXTO.md](CONTEXTO.md)
- **O que já foi feito e o que vem a seguir:** [CHECKLIST.md](CHECKLIST.md) — manter sempre atualizado

## Arquitetura

- `index.html` — o site inteiro: HTML, CSS e JS num arquivo só, imagens embutidas em base64.
  Sem framework, sem build, sem backend.
- Os dados vêm de uma planilha do Google publicada em CSV, lida no navegador via `fetch`.
- `CONFIG` no topo do `<script>` concentra `PUB_BASE`, os `GIDS` de cada aba e a lista de documentos.
- Publicação: `git push` na branch `homolog` → o Netlify publica sozinho em
  https://novacasaigrejaviva.netlify.app

## Regra da planilha — importante

**Nunca edito a planilha, e não dá para editá-la a partir daqui.** Sempre que uma mudança de
dados for necessária, eu digo ao usuário exatamente o que alterar, em qual aba e em qual célula,
e ele aplica no Google Drive.

O arquivo `campanha-casa-igreja-viva.xlsx` na pasta é apenas uma cópia de referência: **o site
não lê esse arquivo**, lê a planilha do Drive.

### Nunca substituir a planilha do Drive, nem apagar/recriar uma aba `pub_`

As URLs publicadas dependem do documento **e** do `gid` de cada aba. Subir um `.xlsx` novo cria
outro documento, os cinco endereços do `CONFIG` morrem e o site cai para dados de demonstração.
Alterações devem ser feitas **na planilha existente, no lugar**.

**Já aconteceu de verdade** (2026-09-13): uma sessão de IA reestruturou as abas `pub_` para
torná-las automáticas, e isso derrubou a publicação do documento inteiro — os 5 links pararam
de funcionar e precisaram ser republicados com `gid` novos. A causa mais provável é apagar e
recriar uma aba em vez de só editar fórmulas dentro dela. Qualquer mudança estrutural na
planilha deve ser seguida de um teste imediato: buscar as 5 URLs publicadas e conferir se ainda
respondem com os dados esperados, antes de considerar a tarefa concluída.

Se a substituição for mesmo inevitável, é preciso republicar as cinco abas e atualizar os cinco
`gid` no `CONFIG` do `index.html`.

### Estrutura da planilha

- **Abas de entrada**, preenchidas à mão: `parametros`, `doacoes`, `despesas`, `fases`
- **Abas calculadas**, só fórmulas: `pub_kpis`, `pub_serie`, `pub_origem`, `pub_despesas`, `pub_fases`
- O site lê **apenas as cinco abas `pub_`**
- `pub_kpis` referencia `parametros` **por posição de célula** (`=parametros!B5`). Renomear rótulo
  na coluna A é seguro; **apagar linha quebra as fórmulas** — na dúvida, deixar a linha obsoleta lá.
- Só entra no site o que estiver com `conferido = SIM` na aba `doacoes`.
- A planilha nunca guarda nome, CPF, telefone ou e-mail de doador — apenas um `id`.

## Linguagem e público

O público inclui pessoas de toda escolaridade, inclusive quem lê pouco. Isso manda no texto:

- **Nada de "comprar"** quando o sujeito é o doador. Ninguém compra pedaço da casa; a comunidade
  conquista junto. "Comprar" só aparece quando quem compra é a igreja comprando o imóvel.
- **Nada de vocabulário corporativo** — "ticket médio" foi removido a pedido do pastor.
- A unidade de doação é a **cota de R$ 100**, e o total de cotas sai de `meta_total ÷ valor_cota`.
- O enquadramento principal é a **etapa atual**, não o R$ 1.000.000 — a meta total assusta e
  paralisa. O valor cheio continua visível, só não é o denominador.
- Número ou imagem antes do parágrafo, nunca o contrário.

## Como verificar antes de publicar

1. Servidor local: `python -m http.server 8765` e abrir `http://localhost:8765/index.html`.
   Abrir o arquivo direto do disco **não funciona** — o navegador bloqueia o `fetch` da planilha
   em `file://`.
2. Testar em largura de celular: boa parte do comportamento é específico de tela estreita.
3. Conferir os dois cenários de dados, porque campanha no começo tem muito zero: valores normais
   e valores zerados (divisão por zero já causou `NaN%` em produção uma vez).

## Cuidados

- Toda mudança de dinheiro exibida na tela precisa de conferência com a tesouraria antes de ir ao ar.
- Não publicar link morto, ainda mais na seção de documentos — sem link, o item aparece como "em breve".
- Movimento na tela deve ser barato: boa parte da congregação acessa por Android simples.
