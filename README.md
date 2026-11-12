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
| < 35% | Caminho para a santa fogueira assegurado |

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

### Adicionar uma pergunta

```js
{axis:"trindade", nivel:2, tipo:"Afirmação", q:"Enunciado…",
 o:[
  {p:-1, t:"Opção do polo esquerdo", c:"Melhor argumento dessa posição"},
  {p: 0, t:"Opção ortodoxa",        c:"…"},
  {p: 1, t:"Opção do polo direito", c:"…"}
 ]}
```

### Ativar as versões média e longa

No HTML, os botões `.ver` dessas versões estão com `disabled`. Quando houver perguntas de nível 2 ou 3, troque `disabled` por `data-v="2"` ou `data-v="3"` e remova o selo "em preparação".

## Fontes

- Catecismo da Igreja Católica ([vatican.va](https://www.vatican.va/archive/cathechism_po/index_new/prima-pagina-cic_po.html))
- Decretos dos concílios (Niceia I e II, Constantinopla I, Éfeso, Calcedônia, Orange, Trento, Vaticano I)
- Obras dos Padres citados (Inácio de Antioquia, Irineu, Agostinho, João Damasceno, Tomás de Aquino)

> **Revisar antes de usar em aula:** as referências históricas e os números de parágrafos do Catecismo foram redigidos de memória e precisam de conferência, principalmente as datas dos sínodos menores (Quierzy 853, Frankfurt 794, Constantinopla 543).

## Publicação

A versão jogável está publicada como artefato no claude.ai. O compartilhamento é feito pelo botão "Compartilhar" da página do artefato; use a opção "versão mais recente" para que o link acompanhe as atualizações.

## Próximos passos

- Versão média: eixos de Igreja e autoridade, Escritura e Tradição, Moral (rigorismo ↔ laxismo) e Maria.
- Versão longa: todas as afirmações e dilemas.
- Conferir as referências com os textos do vatican.va.
