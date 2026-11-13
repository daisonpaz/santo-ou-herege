# Santo ou Herege?

Teste interativo de doutrina católica. O jogador responde a afirmações e dilemas históricos e descobre de quais santos, Doutores da Igreja e hereges históricos está mais próximo.

Inspirado na estrutura do teste [knowsoda.com/pt/filosofia](https://knowsoda.com/pt/filosofia).

## Princípios de design

1. **Três versões**: curta, média e longa. A versão maior não muda o resultado, só o torna mais preciso, acrescentando perguntas e eixos.
2. **Perguntas por temas**: ramos da doutrina divididos em eixos.
3. **Contexto em cada escolha**: cada opção traz o melhor argumento da posição, do jeito que seus defensores a viam. Os nomes das heresias só aparecem no resultado.
4. **Estética agradável**: manuscrito iluminado e vitral (vinho, ouro, pergaminho; fontes Cinzel e EB Garamond).

## Como funciona

Cada **eixo** é uma controvérsia real. As pontas são dois erros opostos e o centro é a fé definida pela Igreja: a ortodoxia como *via média*.

| Valor | Polo | Exemplo (eixo Trindade) |
|---|---|---|
| `-1` | esquerdo (`L`) | Modalismo, com Sabélio |
| `0` | centro (`C`) | Um Deus em três Pessoas, com Santo Atanásio |
| `+1` | direito (`R`) | Arianismo, com Ário |

### Resultado

- **Ortodoxia (%)**: respostas no centro ÷ total de perguntas.
- **Sentença**: um título irônico conforme a faixa de ortodoxia (`VERDICTS`).

| Faixa | Título |
|---|---|
| 100% | Novo venerável Doutor da Santa Igreja |
| ≥ 80% | Catequista de confiança |
| ≥ 60% | Seu nome foi anotado pelo Santo Ofício |
| ≥ 35% | Inaugure sua igreja e aproveite a renúncia fiscal |
| < 35% | Caminho assegurado para a santa fogueira inquisitória |

- **Afinidades**: para cada figura, a fração das respostas do eixo dela que caíram no seu polo. Aparecem as 8 maiores.
- **Perfil por eixo**: um marcador no espectro, calculado pela média dos valores (de -1 a +1), e a seção "Aprofundar", com a história, os concílios e os parágrafos do Catecismo de cada posição.

## Conteúdo atual (v0.1.0, versão curta)

| Ramo | Eixo | Polo esquerdo | Centro | Polo direito |
|---|---|---|---|---|
| I · Deus | A Trindade | Modalismo | Santo Atanásio | Arianismo |
| I · Deus | Criação e matéria | Gnosticismo/maniqueísmo | Santo Irineu | Panteísmo |
| II · Cristo | Verdadeiro Deus e verdadeiro homem | Docetismo | Santo Inácio de Antioquia | Adocionismo |
| II · Cristo | Uma Pessoa, duas naturezas | Nestorianismo | São Leão Magno | Monofisismo |
| III · Graça e salvação | Graça e liberdade | Pelagianismo | Santo Agostinho | Jansenismo/calvinismo |
| III · Graça e salvação | O destino final | Apocatástase | São Francisco de Sales | Dupla predestinação |
| IV · Sacramentos | A Eucaristia | Simbolismo | Santo Tomás de Aquino | Cafarnaísmo |
| IV · Sacramentos | As imagens sagradas | Iconoclasmo | São João Damasceno | Superstição |

São 16 perguntas, 2 por eixo: uma **afirmação** e um **dilema** histórico ou cotidiano.

## Estrutura do código

Tudo fica em um único arquivo, `index.html`, sem dependências além das Google Fonts.

- `AXES`: lista de eixos. Cada um tem `id`, `ramo`, `nome` e os polos `L`, `C` e `R`, cada polo com `tag` (nome da posição), `fig` (figura histórica), `ini` (inicial do medalhão) e `hist` (texto do "Aprofundar").
- `QUESTIONS`: lista de perguntas, cada uma com:
  - `axis`: o `id` do eixo
  - `nivel`: `1` = curta, `2` = média, `3` = longa (uma versão inclui todas as perguntas de nível menor ou igual ao seu)
  - `tipo`: rótulo exibido (ex.: `"Dilema · Niceia, 325"`)
  - `q`: enunciado
  - `o`: opções `{p, t, c}`, em que `p` é o valor (-1, 0 ou +1), `t` o texto e `c` o contexto
- As opções são embaralhadas a cada partida.

## Guia de edição

Todas as mudanças são feitas em `index.html`. Depois de editar, siga o passo a passo de [Publicar uma atualização](#publicar-uma-atualização).

### Adicionar uma pergunta

Acrescente um objeto à lista `QUESTIONS`, de preferência junto às outras perguntas do mesmo eixo:

```js
{axis:"trindade", nivel:2, tipo:"Afirmação", q:"Enunciado…",
 o:[
  {p:-1, t:"Opção do polo esquerdo", c:"Melhor argumento dessa posição"},
  {p: 0, t:"Opção ortodoxa",        c:"…"},
  {p: 1, t:"Opção do polo direito", c:"…"}
 ]}
```

- `axis` precisa ser o `id` de um eixo que já existe em `AXES`.
- `tipo` aceita qualquer texto. O padrão é `"Afirmação"` ou `"Dilema · lugar, data"`.
- No contexto (`c`), não diga o nome da heresia. Ele só deve aparecer no resultado.
- A ordem das opções não importa, porque elas são embaralhadas a cada partida.

### Adicionar um eixo

Acrescente um objeto à lista `AXES`. A posição dele na lista define a ordem em que aparece no resultado.

```js
{id:"moral", ramo:"V · Moral", nome:"Lei e consciência",
 L:{tag:"Rigorismo",  fig:"Nome da figura", ini:"R", hist:"Quem defendeu, quando, onde foi condenado."},
 C:{tag:"Posição ortodoxa", fig:"Santo Afonso de Ligório", ini:"A", hist:"Fundamento e parágrafos do Catecismo (§§ …)."},
 R:{tag:"Laxismo",    fig:"Nome da figura", ini:"L", hist:"…"}}
```

- `id`: identificador curto, sem espaços nem acentos. É ele que as perguntas usam em `axis`.
- `ramo`: o ramo a que o eixo pertence (veja a seção seguinte).
- `L` e `R` são os dois erros opostos e `C` é a posição ortodoxa. Mantenha a lógica de *via média*: as pontas devem errar em direções contrárias.
- `ini`: uma letra só, que aparece no medalhão. O medalhão de `C` fica azul (santo) e os de `L`/`R` ficam vermelhos (hereges).
- Um eixo sem perguntas não aparece no resultado. Crie pelo menos duas perguntas para ele, uma afirmação e um dilema.

### Adicionar um ramo

Os ramos não têm uma lista própria: um ramo é só o texto do campo `ramo` de cada eixo. Para criar um ramo novo:

1. Escolha o nome com numeração romana, por exemplo `"V · Moral"`.
2. Use exatamente o mesmo texto em todos os eixos desse ramo. Qualquer diferença, até num espaço, cria um ramo separado.
3. Coloque os eixos do ramo juntos na lista `AXES` e as perguntas deles juntas em `QUESTIONS`, para que o ramo apareça em bloco durante o teste.
4. Atualize a tela inicial (veja [Editar a tela inicial](#editar-a-tela-inicial)).

### Editar as sentenças do resultado

As sentenças ficam na lista `VERDICTS`, em ordem decrescente de exigência:

```js
{min:.8, t:"Catequista de confiança", d:"Texto explicativo da sentença."}
```

- `min`: a fração mínima de ortodoxia para receber a sentença (`1` = 100%, `.8` = 80%).
- `t`: o título; `d`: a descrição.
- A primeira sentença cuja exigência o jogador atinge é a escolhida. Por isso a lista precisa ir da maior para a menor, e a última deve ter `min:0`, para que sempre haja uma sentença.
- Para criar uma faixa nova, insira a linha na posição certa da ordem.
- Atualize a tabela de faixas na seção [Resultado](#resultado) deste README.

### Editar as afinidades

As figuras e os textos vêm dos polos de cada eixo (`fig`, `tag`, `ini`, `hist`). Para trocar uma figura, edite o polo correspondente em `AXES`.

Na função `results()`:

- `figs.slice(0,8)` define quantas afinidades aparecem. Troque o `8` pelo número desejado.
- Em caso de empate na porcentagem, os hereges aparecem antes dos santos. Essa regra está na linha do `figs.sort(...)`.
- `top.slice(0,4)` define quantas afinidades entram no texto do botão "Copiar resultado".

### Editar a tela inicial

A tela inicial fica no HTML, dentro de `<section id="start">`, e não é gerada automaticamente. Sempre que acrescentar eixos, ramos ou perguntas, confira:

- **Contadores** (`.stats`): "8 eixos", "4 ramos", "16 heresias". Conte duas heresias por eixo.
- **Cartões das versões** (`.ver`): número de perguntas e tempo estimado, em torno de 20 a 25 segundos por pergunta.
- **Como funciona** (`.how`): só precisa mudar se a mecânica do teste mudar.

### Ativar as versões média e longa

No HTML, os botões `.ver` dessas versões estão com `disabled`. Quando houver perguntas de nível 2 ou 3:

1. Troque `disabled` por `data-v="2"` (média) ou `data-v="3"` (longa).
2. Remova o `<span class="soon">em preparação</span>`.
3. Atualize o número de perguntas e o tempo no cartão.
4. Se quiser, passe o selo "Recomendada" para a versão média: mova a classe `rec` do botão da curta para o da média.

### Alterar a estética

As cores ficam em variáveis no início do `<style>`, dentro de `:root`:

| Variável | Uso |
|---|---|
| `--bg`, `--bg2` | fundo da página |
| `--wine`, `--wine2` | vinho: títulos dos cartões, botões de contexto, medalhões dos hereges |
| `--gold`, `--gold2` | ouro: botões, ornamentos, títulos |
| `--parch`, `--parch2` | pergaminho: fundo dos cartões |
| `--ink`, `--muted` | texto principal e secundário dos cartões |
| `--blue` | medalhões dos santos |

- Trocar o valor de uma variável muda a cor em todo o teste.
- As fontes são carregadas pela linha `<link href="https://fonts.googleapis.com/...">` e aplicadas em `body` (EB Garamond) e em `h1, h2, h3, .cinzel` (Cinzel). Para trocar uma fonte, mude as duas coisas.
- A rosácea do topo é um desenho SVG dentro de `.hero`.
- O favicon (o ícone da aba do navegador) é uma versão simplificada da rosácea, embutida no `<link rel="icon">` do `<head>`. É um SVG codificado como URL: no código, `%23` corresponde a `#` (então `%235a1a24` é a cor `#5a1a24`), `%3C` a `<` e `%3E` a `>`. Para trocar as cores, substitua os códigos depois de `%23`. Para trocar o desenho, escreva o SVG normalmente e converta-o com um codificador de "SVG para data URI".
- O espectro de cada eixo no resultado usa o gradiente da classe `.spec`: vermelho nas pontas, verde no centro.

### Publicar uma atualização

1. Teste o `index.html` no navegador: jogue a versão alterada até o fim e abra alguns "Aprofundar".
2. Registre as mudanças no `CHANGELOG.md`, numa seção nova no topo. Use a versão `0.x.0` para conteúdo novo (eixos, perguntas, versões) e `0.1.x` para correções.
3. Atualize as tabelas deste README, se o conteúdo mudou.
4. Faça o commit e envie para o GitHub:
   ```
   git add index.html README.md CHANGELOG.md
   git commit -m "v0.2.0: descrição da mudança"
   git push
   ```
5. O GitHub Pages republica o site sozinho a cada push em `main`, e em um ou dois minutos o [link público](https://daisonpaz.github.io/santo-ou-herege/) já mostra a nova versão. Se fizer edições direto no site do GitHub, rode `git pull` antes de editar no computador, para não gerar conflito.

## Fontes

- Catecismo da Igreja Católica ([vatican.va](https://www.vatican.va/archive/cathechism_po/index_new/prima-pagina-cic_po.html))
- Decretos dos concílios (Niceia I e II, Constantinopla I, Éfeso, Calcedônia, Orange, Trento, Vaticano I)
- Obras dos Padres citados (Inácio de Antioquia, Irineu, Agostinho, João Damasceno, Tomás de Aquino)

> **Revisar antes de usar em aula:** as referências históricas e os números de parágrafos do Catecismo foram redigidos de memória e precisam de conferência, principalmente as datas dos sínodos menores (Quierzy 853, Frankfurt 794, Constantinopla 543).

## Publicação

A versão jogável está publicada [aqui](https://daisonpaz.github.io/santo-ou-herege/), pelo GitHub Pages. O passo a passo de atualização está em [Publicar uma atualização](#publicar-uma-atualização).

## Próximos passos

- Versão média: eixos de Igreja e autoridade, Escritura e Tradição, Moral (rigorismo ↔ laxismo) e Maria.
- Versão longa: todas as afirmações e dilemas.
- Conferir as referências com os textos do vatican.va.
