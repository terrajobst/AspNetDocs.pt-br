---
title: Less, Sass e fonte Awesome no ASP.NET Core
author: ardalis
description: Saiba como usar Less, Sass e fonte Awesome em aplicativos ASP.NET Core."
ms.author: tdykstra
ms.date: 10/14/2016
uid: client-side/less-sass-fa
ms.openlocfilehash: 2229c4e3b0238ff17c15e78f657b9acb10495c72
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57065553"
---
# <a name="less-sass-and-font-awesome-in-aspnet-core"></a>Less, Sass e fonte Awesome no ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Os usuários de aplicativos da web têm expectativas cada vez mais altas quando se trata de estilo e a experiência geral. Aplicativos web modernos com frequência aproveitam ferramentas avançadas e estruturas para definir e gerenciar sua aparência de uma maneira consistente. Estruturas como [Bootstrap](http://getbootstrap.com/) pode percorrem um longo caminho em direção a definição de um conjunto comum de estilos e opções de layout para sites da web. No entanto, a maioria dos sites não trivial também se beneficiar sendo capaz de definir e manter arquivos de (CSS) de folha de estilos em cascata e estilos com eficiência, bem como para ter acesso fácil aos ícones não de imagem que ajudam a tornar a interface do site mais intuitiva. É aí que linguagens e ferramentas que dão suporte ao [menos](http://lesscss.org/) e [Sass](http://sass-lang.com/), e bibliotecas, como [Font Awesome](http://fontawesome.io/), entram.

## <a name="css-preprocessor-languages"></a>Idiomas de pré-processador do CSS

Linguagens que são compiladas em outras linguagens, para melhorar a experiência de trabalhar com a linguagem subjacente, são chamadas de pré-processadores. Há dois pré-processadores populares para CSS: Menor e Sass.  Esses pré-processadores adicionar recursos a CSS, como o suporte para variáveis e regras aninhadas, o que melhorar a facilidade de manutenção de folhas de estilos grandes e complexas. CSS como uma linguagem é muito básico, que não tem suporte a até mesmo algo tão simples como variáveis, e isso tende a tornar arquivos CSS repetitivas e inflado. Adicionar recursos de linguagem de programação real por meio de pré-processadores pode ajudar a reduzir a duplicação e fornecer uma melhor organização de regras de estilo. Visual Studio fornece suporte interno para ambas as Less e Sass, bem como extensões que podem melhorar ainda mais a experiência de desenvolvimento ao trabalhar com essas linguagens.

Como um exemplo de como os pré-processadores podem melhorar a legibilidade e a facilidade de manutenção de informações de estilo, considere este CSS:

```css
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    color: black;
    font-weight: bold;
    font-size: 14px;
    font-family: Helvetica, Arial, sans-serif;
}
```

Usando Less, isso pode ser reescrito para eliminar a duplicação, usando um *mixin* (chamada assim porque ele permite que você "misture" propriedades de uma classe ou conjunto de regras em outra):

```less
.header {
    color: black;
    font-weight: bold;
    font-size: 18px;
    font-family: Helvetica, Arial, sans-serif;
}

.small-header {
    .header;
    font-size: 14px;
}
```

## <a name="less"></a>Less

O pré-processador CSS less é executado usando o Node. js. Para instalar less, use o Gerenciador de pacotes de nó (npm) em um prompt de comando (-g significa "global"):

```console
npm install -g less
```

Se você estiver usando o Visual Studio, você pode começar com less adicionando um ou mais arquivos less ao seu projeto e, em seguida, configurando Gulp (ou Grunt) para processá-los em tempo de compilação. Adicione uma pasta *estilos* ao seu projeto e, em seguida, adicione um novo arquivo less chamado *main.less* nesta pasta.

![Adicionar arquivo less](less-sass-fa/_static/add-less-file.png)

Depois de adicionado, a estrutura de pastas deve ser algo parecido com isto:

![Estrutura de pastas](less-sass-fa/_static/folder-structure.png)

Agora você pode adicionar alguns estilos básicos para o arquivo, que será compilado em CSS e implantado na pasta wwwroot pelo Gulp.

Modifique *Main. Less* para incluir o conteúdo a seguir, que cria uma paleta de cores simples de uma única cor base.

```less
@base: #663333;
@background: spin(@base, 180);
@lighter: lighten(spin(@base, 5), 10%);
@lighter2: lighten(spin(@base, 10), 20%);
@darker: darken(spin(@base, -5), 10%);
@darker2: darken(spin(@base, -10), 20%);

body {
    background-color:@background;
}
.baseColor  {color:@base}
.bgLight    {color:@lighter}
.bgLight2   {color:@lighter2}
.bgDark     {color:@darker}
.bgDark2    {color:@darker2}
```

`@base` e o outro @-prefixed itens são variáveis. Cada um deles representa uma cor. Exceto para `@base`, eles são definidos usando funções de cor: Clarear, Escurecer e rotação. Clarear e escurecer fazer muito bem o que você esperaria; rotação ajusta o matiz da cor por um número de graus (cerca de roda de cores). O processador de menos é inteligente o suficiente para ignorar as variáveis que não são usadas, portanto, para demonstrar como funcionam essas variáveis, precisamos usá-los em algum lugar. As classes `.baseColor`, etc. demonstrará os valores calculados de cada uma das variáveis no arquivo CSS que é produzido.

### <a name="get-started"></a>Introdução

Criar uma **arquivo de configuração npm** (*Package. JSON*) na pasta do projeto e editá-lo para fazer referência `gulp` e `gulp-less`:

```json
{
  "version": "1.0.0",
  "name": "asp.net",
  "private": true,
  "devDependencies": {
    "gulp": "3.9.1",
    "gulp-less": "3.3.0"
  }
}
```

Instalar as dependências em um prompt de comando na pasta do projeto ou no Visual Studio **Gerenciador de soluções** (**dependências > npm > Restaurar pacotes**).

```console
npm install
```

![Restaurar os pacotes do VS](less-sass-fa/_static/restore-packages.png)

Na pasta do projeto, criar um **Gulp arquivo de configuração** (*gulpfile.js*) para definir o processo automatizado.  Adicione uma variável na parte superior do arquivo para representar less e uma tarefa para execução less:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less");

gulp.task("less", function () {
  return gulp.src('Styles/main.less')
    .pipe(less())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Abra o **Explorador do Executador de tarefas** (**exibição > outros Windows > Gerenciador do executor de tarefas**). Entre as tarefas, você deve ver uma nova tarefa chamada `less`. Você talvez precise atualizar a janela.

Execute o `less` tarefa e você verá uma saída semelhante ao que é mostrado aqui:

![less executor de tarefas](less-sass-fa/_static/less-task-runner.png)

O *wwwroot/css* pasta contém um novo arquivo, agora *Main*:

![css principal criado](less-sass-fa/_static/main-css-created.png)

Abra *Main* e você verá algo semelhante ao seguinte:

```css
body {
    background-color: #336666;
}
.baseColor {
    color: #663333;
}
.bgLight {
    color: #884a44;
}
.bgLight2 {
    color: #aa6355;
}
.bgDark {
    color: #442225;
}
.bgDark2 {
    color: #221114;
}
```

Adicionar uma página HTML simples para o *wwwroot* pasta e referência *Main* para ver a paleta de cores em ação.

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <link href="css/main.css" rel="stylesheet" />
    <title></title>
</head>
<body>
    <div>
        <div class="baseColor">BaseColor</div>
        <div class="bgLight">Light</div>
        <div class="bgLight2">Light2</div>
        <div class="bgDark">Dark</div>
        <div class="bgDark2">Dark2</div>
    </div>
</body>
</html>
```

Você pode ver que a 180 graus de rotação em `@base` usada para produzir `@background` resultou no disco de cores opostos cor do `@base`:

![exemplo de teste de less](less-sass-fa/_static/less-test-screenshot.png)

Less também oferece suporte para regras aninhadas, bem como as consultas de mídia aninhada. Por exemplo, definindo hierarquias aninhadas como menus podem resultar em regras de CSS detalhadas como estes:

```css
nav {
    height: 40px;
    width: 100%;
}
nav li {
    height: 38px;
    width: 100px;
}
nav li a:link {
    color: #000;
    text-decoration: none;
}
nav li a:visited {
    text-decoration: none;
    color: #CC3333;
}
nav li a:hover {
    text-decoration: underline;
    font-weight: bold;
}
nav li a:active {
    text-decoration: underline;
}
```

O ideal é que todas as regras de estilo relacionadas serão colocadas juntos dentro do arquivo de CSS, mas na prática não há nada impor essa regra, exceto a convenção e talvez os comentários do bloco.

Definir essas mesmas regras usando less tem esta aparência:

```less
nav {
    height: 40px;
    width: 100%;
    li {
        height: 38px;
        width: 100px;
        a {
            color: #000;
            &:link { text-decoration:none}
            &:visited { color: #CC3333; text-decoration:none}
            &:hover { text-decoration:underline; font-weight:bold}
            &:active {text-decoration:underline}
        }
    }
}
```

Observe que, nesse caso, todos os elementos subordinados de `nav` estão contidos dentro de seu escopo. Não há nenhuma repetição de elementos pai (`nav`, `li`, `a`), e a contagem total de linha também descartada (embora alguns dos que é um resultado de colocar valores nas linhas do mesmas no segundo exemplo). Ele pode ser muito útil, organização ver todas as regras para um determinado elemento de interface do usuário dentro de um escopo limitado explicitamente, nesse caso, configure off do restante do arquivo de chaves.

O `&` sintaxe é um recurso seletor less, com & que representa o pai de seletor atual. Portanto, dentro do {...} bloco, `&` representa um `a` marca e, portanto, `&:link` é equivalente a `a:link`.

Consultas de mídia, extremamente úteis na criação de designs de resposta também podem contribuir muito para repetição e a complexidade em CSS. Less permite que as consultas de mídia a ser aninhada dentro de classes, para que a definição de classe inteira não precisa ser repetido em diferentes nível superior `@media` elementos. Por exemplo, aqui está o CSS para um menu de resposta:

```css
.navigation {
    margin-top: 30%;
    width: 100%;
}
@media screen and (min-width: 40em) {
    .navigation {
        margin: 0;
    }
}
@media screen and (min-width: 62em) {
    .navigation {
        width: 960px;
        margin: 0;
    }
}
```

Isso pode ser melhor definido em less como:

```less
.navigation {
    margin-top: 30%;
    width: 100%;
    @media screen and (min-width: 40em) {
        margin: 0;
    }
    @media screen and (min-width: 62em) {
        width: 960px;
        margin: 0;
    }
}
```

Outro recurso de less que já vimos é seu suporte para operações matemáticas, permitindo que os atributos de estilo a ser construído de variáveis predefinidas. Isso facilita a atualização estilos relacionados muito mais fácil, já que a variável de base pode ser modificada e todos os valores dependentes alterar automaticamente.

Arquivos CSS, especialmente para grandes sites (e especialmente se as consultas de mídia estão sendo usadas), tendem a ter muito grandes ao longo do tempo, tornando a trabalhar com elas complicada. Arquivos less podem ser definidos separadamente, obtidas usando `@import` diretivas. Less também pode ser usado para importar arquivos CSS individuais, se desejado.

*Mixins* pode aceitar parâmetros e less oferece suporte à lógica condicional na forma de protege mesclado, que fornecem uma maneira declarativa para definir quando determinados mixins entra em vigor. Um uso comum para protege mesclado ajustar as cores com base em como a luz ou escuro a cor da fonte. Dado um mesclado que aceita um parâmetro para a cor, um protetor mesclado pode ser usado para modificar o mesclado com base na cor:

```less
.box (@color) when (lightness(@color) >= 50%) {
    background-color: #000;
}
.box (@color) when (lightness(@color) < 50%) {
    background-color: #FFF;
}
.box (@color) {
    color: @color;
}

.feature {
    .box (@base);
}
```

Considerando nossa atual `@base` valor `#663333`, esse script less produzirá o seguinte CSS:

```css
.feature {
    background-color: #FFF;
    color: #663333;
}
```

Less fornece uma série de recursos adicionais, mas isso deve dar uma ideia da energia desse idioma de pré-processamento.

## <a name="sass"></a>Sass

Sass é semelhante ao menos, fornecendo suporte para muitos dos mesmos recursos, mas com uma sintaxe ligeiramente diferente. Ele é criado usando o Ruby, em vez de JavaScript, e então tem requisitos de instalação diferentes. O idioma original do Sass não usa chaves ou ponto e vírgula, mas em vez disso, definida pelo escopo usando o espaço em branco e recuo. Na versão 3 do Sass, uma nova sintaxe foi introduzida, **SCSS** "Sassy CSS ("). SCSS é semelhante ao CSS que ignora os níveis de recuo e espaços em branco e, em vez disso, usa o ponto e vírgula e entre chaves.

Para instalar o Sass, normalmente você seria instalar primeiro o Ruby (pré-instalado no macOS) e, em seguida, execute:

```console
gem install sass
```

No entanto, se você estiver executando o Visual Studio, você pode começar com Sass em grande parte da mesma maneira como você faria com less. Abra *Package. JSON* e adicionar o pacote "gulp sass" `devDependencies`:

```json
"devDependencies": {
  "gulp": "3.9.1",
  "gulp-less": "3.3.0",
  "gulp-sass": "3.1.0"
}
```

Em seguida, modifique *gulpfile. js* para adicionar uma variável de sass e uma tarefa para compilar seu Sass arquivos e colocar os resultados na pasta wwwroot:

```javascript
var gulp = require("gulp"),
  fs = require("fs"),
  less = require("gulp-less"),
  sass = require("gulp-sass");

// other content removed

gulp.task("sass", function () {
  return gulp.src('Styles/main2.scss')
    .pipe(sass())
    .pipe(gulp.dest('wwwroot/css'));
});
```

Agora você pode adicionar o arquivo do Sass *main2.scss* para o *estilos* pasta na raiz do projeto:

![Adicionar arquivo scss](less-sass-fa/_static/add-scss-file.png)

Abra *main2.scss* e adicione o seguinte:

```sass
$base: #CC0000;
body {
    background-color: $base;
}
```

Salve todos os seus arquivos. Agora quando você atualiza **Task Runner Explorer**, você verá um `sass` tarefa. Execute-o e examine os */wwwroot/css* pasta. Agora há uma *main2.css* arquivo, com esses conteúdos:

```css
body {
    background-color: #CC0000;
}
```

Sass oferece suporte a aninhamento praticamente o mesmo foi que less não, fornecer benefícios semelhantes. Os arquivos podem ser divididos por função e incluídos usando a `@import` diretiva:

```sass
@import 'anotherfile';
```

Sass dá suporte a mesclas também, usando o `@mixin` palavra-chave para defini-los e `@include` incluí-los, como neste exemplo de [sass-lang.com](http://sass-lang.com):

```sass
@mixin border-radius($radius) {
    -webkit-border-radius: $radius;
    -moz-border-radius: $radius;
    -ms-border-radius: $radius;
    border-radius: $radius;
}

.box { @include border-radius(10px); }
```

Além de mesclas, o Sass também suporta o conceito de herança, permitindo que uma classe estender outra. Ele é conceitualmente semelhante a uma mescla, mas resulta em menos código CSS. Isso é feito usando o `@extend` palavra-chave. Para experimentar mesclas, adicione o seguinte ao seu *main2.scss* arquivo:

```sass
@mixin alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @include alert;
    border-color: green;
}

.error {
    @include alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Examine a saída na *main2.css* depois de executar o `sass` tarefa **Task Runner Explorer**:

```css
.success {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    border-color: green;
 }

.error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Observe que todas as propriedades comuns de mescla de alerta são repetidas em cada classe. A mescla fez um bom trabalho de ajudar a eliminar a duplicação em tempo de desenvolvimento, mas ele ainda está criando um CSS com muita duplicação, resultando em maior do que os arquivos necessários do CSS - um possível problema de desempenho.

Agora, substitua a mescla de alerta com um `.alert` classe e altere `@include` para `@extend` (Lembre-se de estender `.alert`, e não `alert`):

```sass
.alert {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    @extend .alert;
    border-color: green;
}

.error {
    @extend .alert;
    color: red;
    border-color: red;
    font-weight:bold;
}
```

Execute o Sass mais uma vez e examine o CSS resultante:

```css
.alert, .success, .error {
    border: 1px solid black;
    padding: 5px;
    color: #333333;
}

.success {
    border-color: green;
}

.error {
    color: red;
    border-color: red;
    font-weight: bold;
}
```

Agora as propriedades são definidas apenas como quantas vezes forem necessárias, e melhor CSS é gerado.

Sass também inclui funções e operações de lógica condicional, semelhantes ao less. Na verdade, os recursos de dois idiomas são muito semelhantes.

## <a name="less-or-sass"></a>Less ou Sass?

Ainda não há consenso se geralmente é melhor usar less ou Sass (ou até mesmo se preferir o Sass original ou a sintaxe SCSS mais recente em Sass). Provavelmente, a decisão mais importante é **usar uma dessas ferramentas**, em vez de apenas codificação manual em seus arquivos CSS. Depois que você fez essa decisão, ambos Less e Sass são boas opções.

## <a name="font-awesome"></a>Font Awesome

Além de pré-processadores do CSS, outro excelente recurso para aplicativos web modernos de estilo é Font Awesome. Font Awesome é um kit de ferramentas que fornece ícones vetoriais escaláveis mais de 500 que podem ser usados livremente em seus aplicativos web. Ele foi projetado originalmente para trabalhar com o Bootstrap, mas ele não tem dependências nessa estrutura ou em todas as bibliotecas de JavaScript.

A maneira mais fácil começar com o Font Awesome é adicionar uma referência a ele, usando seu local de rede (CDN) do fornecimento de conteúdo público:

```html
<link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.3.0/css/font-awesome.min.css">
```

Você também pode adicioná-lo ao seu projeto do Visual Studio ao adicioná-lo para "dependências" na *bower. JSON*:

```json
{
  "name": "ASP.NET",
  "private": true,
  "dependencies": {
    "bootstrap": "3.0.0",
    "jquery": "1.10.2",
    "jquery-validation": "1.11.1",
    "jquery-validation-unobtrusive": "3.2.2",
    "hammer.js": "2.0.4",
    "bootstrap-touch-carousel": "0.8.0",
    "Font-Awesome": "4.3.0"
  }
}
```

Quando você tiver uma referência ao Font Awesome em uma página, você pode adicionar ícones ao seu aplicativo por meio da aplicação classes Font Awesome, normalmente prefixadas com "fa-", aos seus elementos HTML embutido (como `<span>` ou `<i>`).  Por exemplo, você pode adicionar ícones de simples listas e menus usando código como este:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="utf-8" />
    <title></title>
    <link href="lib/font-awesome/css/font-awesome.css" rel="stylesheet" />
</head>
<body>
    <ul class="fa-ul">
        <li><i class="fa fa-li fa-home"></i> Home</li>
        <li><i class="fa fa-li fa-cog"></i> Settings</li>
    </ul>
</body>
</html>
```

Isso produz o seguinte no navegador, observe o ícone ao lado de cada item:

![ícones de lista](less-sass-fa/_static/list-icons-screenshot.png)

Você pode exibir uma lista completa dos demais ícones disponíveis aqui:

http://fontawesome.io/icons/

## <a name="summary"></a>Resumo

Aplicativos web modernos exigem designs cada vez mais responsivos e fluidos que são normais, intuitivos e fáceis de usar em uma variedade de dispositivos. Gerenciar a complexidade das folhas de estilo CSS necessárias para atingir essas metas é melhor usando um tipo de pré-processador less ou Sass. Além disso, kits de ferramentas como a Awesome fonte rapidamente fornecem ícones conhecidos a menus de navegação textual e experiência de botões, melhorando o usuário do seu aplicativo.
