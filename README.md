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
- **Perfil por eixo**: um marcador no espectro, calculado pela média dos valores (de -1 a +1), e a seção "Aprofundar", que tem duas partes:
  - **Suas respostas**: cada pergunta do eixo com todas as opções classificadas (✠ em verde para a ortodoxa, em vermelho com o nome da heresia para as demais) e a resposta do jogador destacada.
  - **As posições na história**: quem defendeu cada posição, os concílios e os parágrafos do Catecismo.

  O título do "Aprofundar" mostra quantas respostas do eixo foram ortodoxas (ex.: "2 de 2 ortodoxas").

## Conteúdo atual (v0.3.0)

| Ramo | Eixo | Polo esquerdo | Centro | Polo direito | Versão |
|---|---|---|---|---|---|
| I · Deus | A Trindade | Modalismo | Santo Atanásio | Arianismo | curta |
| I · Deus | Criação e matéria | Gnosticismo/maniqueísmo | Santo Irineu | Panteísmo | curta |
| II · Cristo | Verdadeiro Deus e verdadeiro homem | Docetismo | Santo Inácio de Antioquia | Adocionismo | curta |
| II · Cristo | Uma Pessoa, duas naturezas | Nestorianismo | São Leão Magno | Monofisismo | curta |
| III · Graça e salvação | Graça e liberdade | Pelagianismo | Santo Agostinho | Jansenismo/calvinismo | curta |
| III · Graça e salvação | O destino final | Apocatástase | São Francisco de Sales | Dupla predestinação | curta |
| IV · Sacramentos | A Eucaristia | Simbolismo | Santo Tomás de Aquino | Cafarnaísmo | curta |
| IV · Sacramentos | As imagens sagradas | Iconoclasmo | São João Damasceno | Superstição | curta |
| V · Igreja | Escritura e Tradição | Sola Scriptura | São Vicente de Lérins | Tradição secreta (gnose) | média |
| V · Igreja | Autoridade na Igreja | Conciliarismo | São Roberto Belarmino | Papolatria (ultramontanismo extremo) | média |
| VI · Vida cristã | Lei, consciência e misericórdia | Rigorismo | Santo Afonso de Ligório | Laxismo | média |
| VI · Vida cristã | Maria | Negação dos privilégios marianos | São Bernardo de Claraval | Coliridianismo (mariolatria) | média |

| Versão | Perguntas | Eixos | Composição |
|---|---|---|---|
| Curta (`nivel:1`) | 16 | 8 | 2 por eixo dos ramos I–IV: uma **afirmação** e um **dilema** |
| Média (`nivel:2`) | 32 | 12 | as 16 da curta + 1 pergunta a mais em cada eixo dos ramos I–IV + 2 por eixo dos ramos V–VI |
| Longa (`nivel:3`) | — | — | em preparação |

Nem todos os polos são heresias formalmente condenadas: "Superstição" e "Papolatria" são exageros que a Igreja rejeita, sem condenação conciliar própria. O texto do "Aprofundar" deixa isso claro.

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
- A ordem das perguntas durante o teste segue a ordem dos eixos em `AXES`, e não a posição em `QUESTIONS`. Dentro de um mesmo eixo, vale a ordem em `QUESTIONS`. Por isso as perguntas de cada versão podem ficar em blocos separados no código (há um bloco comentado "Perguntas da versão média").

## Guia de edição

Todas as mudanças são feitas em `index.html`. Depois de editar, siga o passo a passo de [Publicar uma atualização](#publicar-uma-atualização).

### Adicionar uma pergunta

Acrescente um objeto à lista `QUESTIONS`, no bloco da versão a que a pergunta pertence. Não importa onde ela fica dentro da lista: durante o teste, as perguntas são agrupadas pelo eixo.

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
- Um eixo que só tem perguntas de nível 2 aparece só a partir da versão média (é o caso dos ramos V e VI). Na versão curta, ele é simplesmente omitido do resultado.

### Adicionar um ramo

Os ramos não têm uma lista própria: um ramo é só o texto do campo `ramo` de cada eixo. Para criar um ramo novo:

1. Escolha o nome com numeração romana, por exemplo `"V · Moral"`.
2. Use exatamente o mesmo texto em todos os eixos desse ramo. Qualquer diferença, até num espaço, cria um ramo separado.
3. Coloque os eixos do ramo juntos na lista `AXES`. Como as perguntas seguem a ordem dos eixos, o ramo aparecerá em bloco durante o teste.
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

- **Contadores** (`.stats`): hoje mostram "12 eixos", "6 ramos" e "24 erros". Conte dois erros por eixo. Eles descrevem a versão mais completa disponível.
- **Cartões das versões** (`.ver`): número de perguntas e tempo estimado, em torno de 20 a 25 segundos por pergunta.
- **Como funciona** (`.how`): só precisa mudar se a mecânica do teste mudar.

### Ativar a versão longa

A versão média já está ativa. No HTML, o botão `.ver` da longa ainda está com `disabled`. Quando houver perguntas de nível 3:

1. Troque `disabled` por `data-v="3"`.
2. Remova o `<span class="soon">em preparação</span>`.
3. Atualize o número de perguntas, o tempo e o número de eixos no cartão.
4. O selo "Recomendada" está na média (classe `rec`). Para mudá-lo de lugar, mova a classe para outro botão.

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
- Decretos dos concílios (Niceia I e II, Constantinopla I e III, Éfeso, Calcedônia, Orange, Constança, Trento, Vaticano I e II: *Dei Verbum*, *Lumen Gentium*)
- Documentos papais citados (*Execrabilis*, de Pio II, 1460; condenações do laxismo, de Inocêncio XI, 1679, e do tuciorismo, de Alexandre VIII, 1690)
- Obras dos Padres citados (Inácio de Antioquia, Irineu, Agostinho, João Damasceno, Tomás de Aquino)

> **Revisar antes de usar em aula:** as referências históricas e os números de parágrafos do Catecismo foram redigidos de memória e precisam de conferência, principalmente as datas dos sínodos menores (Quierzy 853, Frankfurt 794, Constantinopla 543) e as referências da versão média (Execrabilis 1460, condenações de 1679 e 1690, Panarion 79 de Santo Epifânio, a frase atribuída a W. G. Ward).

## Publicação

A versão jogável está publicada [aqui](https://daisonpaz.github.io/santo-ou-herege/), pelo GitHub Pages. O passo a passo de atualização está em [Publicar uma atualização](#publicar-uma-atualização).

## Próximos passos

- Versão longa: todas as afirmações e dilemas, com novos eixos (por exemplo: sacramentos e ministros, com donatismo; Igreja e Estado; fé e razão, com fideísmo e racionalismo; justificação).
- Conferir as referências com os textos do vatican.va.
