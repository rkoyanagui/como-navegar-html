# Como navegar por um documento HTML ou XML

Como usar _XPath_ e _CSS Selector_ para navegar um documento HTML.

## Entendendo o formato XML/HTML

Um documento XML/HTML contém elementos organizados hierarquicamente.

Um elemento [quase] sempre se abre e se fecha:

```xml

<foo></foo>
```

No exemplo acima, não são dois elementos **foo**, mas sim um único elemento, com a etiqueta, ou em inglês _tag_, de
abertura `<foo>`, e a tag de fechamento `</foo>`.

Um elemento pode ser vazio:

```xml

<foo></foo>
```

Um elemento vazio pode-se abrir e fechar em uma só _tag_, na forma `<foo/>`. Ou seja, `<foo></foo>` é igual a `<foo/>`.

Um elemento pode conter um texto:

```xml

<foo>Meu texto</foo>
```

Um elemento pode ter atributos/características/propriedades. É por meio de atributos que acrescentamos links às páginas
HTML, por exemplo. Um atributo sempre começa pelo nome do atributo, seguido do valor do atributo cercado por aspas
duplas:

```xml

<foo meu-atributo="um valor qualquer"></foo>
```

Um elemento pode conter outros elementos:

```xml

<foo>
    <bar></bar>
</foo>
```

No exemplo acima, usamos indentação (espaços ou tabs no início de uma linha), para ajudar na leitura no documento. A
indentação não afeta o significado de documento. É puro embelezamento e conveniência.

Para adicionar um comentário, isto é, um texto a ser ignorado pelo computador, que serve apenas para ajudar leitores
humanos, cercamos o texto assim `<!-- texto -->`:

```xml

<!-- Um comentário qualquer -->
<foo>
    <!-- Outro comentário -->
    <bar></bar>
</foo><!-- Último comentário -->
```

(Neste artigo, usaremos comentários para ajudar a enxergar como a seleção de elementos funciona.)

Se um elemento **foo** contém outro elemento **bar**, então dizemos que foo é o pai de
**bar**, ou alternativamente, que **bar** é filho de **foo**. Assim, forma-se a hierarquia ou árvore de elementos, em
semelhança a uma árvore genealógica.

Tanto XML como HTML têm o mesmo tipo de estrutura. A diferença maior é de propósito e de uso. As outras diferenças são
menores e irrelevantes para você conseguir localizar um elemento.

Temos dois jeitos de navegar por um documento HTML/XML: XPath e CSS Selector.

## XPath

### Barras e composição

Um localizador XPath diz qual é o caminho para chegar até um elemento qualquer. Um localizador sempre começa, ou por
uma barra, ou por duas barras.

`/foo` Uma barra: a partir da raiz do documento, pegue os elementos do tipo **foo**.

`//foo` Duas barras: em qualquer lugar do documento, pegue os elementos do tipo **foo**.

No documento abaixo, há três elementos **foo**.

```xml

<foo>
    <bar>
        <foo></foo>
    </bar>
    <foo></foo>
</foo>
```

Se usarmos `/foo`, localizaremos somente o elemento **foo** na raiz do documento.

```xml

<foo><!-- selecionado -->
    <bar>
        <foo></foo>
    </bar>
    <foo></foo>
</foo><!-- selecionado -->
```

Se usarmos `//foo`, localizaremos todos os elementos **foo** do documento inteiro, que são três:

```xml

<foo><!-- selecionado primeiro -->
    <bar>
        <foo></foo><!-- selecionado segundo -->
    </bar>
    <foo></foo><!-- selecionado terceiro -->
</foo><!-- selecionado primeiro -->
```

Até agora, só falamos de localizadores simples, mas podemos compô-los para formar localizadores maiores e mais
certeiros.

Se usarmos `/foo/bar`, localizaremos os **foo** na raiz, então localizaremos os **bar** filhos desses **foo**, isto é,
descendentes diretos, ou imediatamente dentro, desses **foo**. Neste caso, só há um:

```xml

<foo>
    <bar><!-- selecionado -->
        <foo></foo>
    </bar><!-- selecionado -->
    <foo></foo>
</foo>
```

Se usarmos `//bar/foo`, localizaremos elementos **bar** em todo o documento, então localizaremos elementos **foo**
filhos, isto é, descendentes diretos, ou imediatamente dentro, de cada **bar**. Novamente, encontramos só um:

```xml

<foo>
    <bar>
        <foo></foo><!-- selecionado -->
    </bar>
    <foo></foo>
</foo>
```

Se usarmos `//bar//foo` localizaremos todos os **foo** que forem descendentes, diretos ou indiretos (filhos, netos,
bisnetos, tataranetos, etc), de todos os **bar**. No exemplo abaixo, encontramos três:

```xml

<foo>
    <bar>
        <foo></foo><!-- selecionado primeiro -->
        <xpto>
            <foo><!-- selecionado segundo -->
                <abc></abc>
            </foo><!-- selecionado segundo -->
        </xpto>
    </bar>
    <foo>
        <foo>
            <foo>
                <bar>
                    <foo></foo><!-- selecionado terceiro -->
                </bar>
            </foo>
        </foo>
    </foo>
</foo>
```

### Coringa

Se não nos interessar qual é o tipo do elemento, podemos usar o asterisco.

Por exemplo, se usarmos `//bar/*`, localizaremos os **bar** em todo o documento, então selecionaremos os filhos (de
qualquer tipo) desses **bar**.

```xml

<foo>
    <bar>
        <foo></foo><!-- selecionado -->
    </bar>
    <foo></foo>
</foo>
```

### Elementos e predicados

Podemos acrescentar predicados, isto é, condições, a nossos localizadores. Isso restringe o localizador a encontrar
somente os elementos que cumpram o predicado. A sintaxe básica é:

`tipo-de-elemento[predicado]`

Há predicados de índice, isto é, selecionar o primeiro, ou o segundo, etc. Há também predicados de texto e de atributo.

No documento abaixo:

```xml

<bookstore>
    <book>
        <title lang="en">Harry Potter</title>
        <author>J. K. Rowling</author>
        <year>2005</year>
        <price>29.99</price>
    </book>
    <book>
        <title lang="pt">O Iluminado</title>
        <author>Stephen King</author>
        <year>1980</year>
        <price>34.99</price>
    </book>
</bookstore>
```

`//book[1]` seleciona o primeiro _book_ (o de título _Harry Potter_).

`//book[2]` seleciona o segundo _book_ (o de título _O Iluminado_).

`//author[text()='J. K. Rowling']` seleciona o _author_ cujo texto seja igual a _J. K. Rowling_.

`//author[contains(text(), 'Rowling')]` seleciona o _author_ cujo texto contenha _Rowling_.

`//title[@lang='pt']` seleciona o _title_ cujo atributo _lang_ seja igual a _pt_.

`//title[@lang!='pt']` seleciona o _title_ cujo atributo _lang_ seja diferente de _pt_.

Há muitos outros operadores aplicáveis a predicado de XPath. Confira os guias disponíveis no fim deste artigo.

## CSS Selector

TODO

## Links externos

* [W3Schools - XPath](https://www.w3schools.com/xml/xpath_intro.asp)
* [MDN Web Docs - XPath](https://developer.mozilla.org/en-US/docs/Web/XPath)
* [CSS Selector](https://www.w3schools.com/cssref/css_selectors.php)
