---
ms.openlocfilehash: 3be420696add54094f24424285a905e7e7963bad
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032883"
---
# <a name="bootstraphttpgetbootstrapcom"></a>[Bootstrap](http://getbootstrap.com)

[![Slack](https://bootstrap-slack.herokuapp.com/badge.svg)](https://bootstrap-slack.herokuapp.com)
![Versão Bower](https://img.shields.io/bower/v/bootstrap.svg)
[![versão npm](https://img.shields.io/npm/v/bootstrap.svg)](https://www.npmjs.com/package/bootstrap)
[![Status do Build](https://img.shields.io/travis/twbs/bootstrap/master.svg)](https://travis-ci.org/twbs/bootstrap)
[![Status do devDependency](https://img.shields.io/david/dev/twbs/bootstrap.svg)](https://david-dm.org/twbs/bootstrap#info=devDependencies)
[![NuGet](https://img.shields.io/nuget/v/bootstrap.svg)](https://www.nuget.org/packages/Bootstrap)
[![Status de Teste do Selenium](https://saucelabs.com/browser-matrix/bootstrap.svg)](https://saucelabs.com/u/bootstrap)

O Bootstrap é um framework de front-end elegante, intuitiva e eficiente para facilitar e agilizar o desenvolvimento na Web, foi criado por [Mark Otto](https://twitter.com/mdo) e [Jacob Thornton](https://twitter.com/fat) e é mantido pela [equipe principal](https://github.com/orgs/twbs/people) com o suporte massivo e envolvente da comunidade.

Para começar, confira <http://getbootstrap.com>!


## <a name="table-of-contents"></a>Sumário

* [Início rápido](#quick-start)
* [Bugs e solicitações de recursos](#bugs-and-feature-requests)
* [Documentação](#documentation)
* [Contribuição](#contributing)
* [Comunidade](#community)
* [Controle de versão](#versioning)
* [Criadores](#creators)
* [Direitos autorais e licença](#copyright-and-license)


## <a name="quick-start"></a>Início rápido

Há várias opções de início rápido disponíveis:

* [Baixar a versão mais recente](https://github.com/twbs/bootstrap/archive/v3.3.7.zip).
* Clonar o repositório: `git clone https://github.com/twbs/bootstrap.git`.
* Instalar com [Bower](http://bower.io): `bower install bootstrap`.
* Instalar com [npm](https://www.npmjs.com): `npm install bootstrap@3`.
* Instalar com [Meteor](https://www.meteor.com): `meteor add twbs:bootstrap`.
* Instalar com [Composer](https://getcomposer.org): `composer require twbs/bootstrap`.

Leia a [Página de Introdução](http://getbootstrap.com/getting-started/) para obter informações sobre os exemplos, modelos e conteúdos de framework, e muito mais.

### <a name="whats-included"></a>O que está incluso

No download, você encontrará os seguintes diretórios e arquivos, logicamente agrupando ativos comuns e fornecendo variações compiladas e minimizadas. Você verá algo assim:

```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```

Nós oferecemos CSS e JS (`bootstrap.*`) compilados, bem como CSS e JS (`bootstrap.min.*`) compilados e minimizados. [Mapas de origem](https://developer.chrome.com/devtools/docs/css-preprocessors) (`bootstrap.*.map`) de CSS estão disponíveis para uso com ferramentas de desenvolvedor de determinados navegadores. Fontes de Glyphicons são incluídas, uma vez que são o tema opcional do Bootstrap.


## <a name="bugs-and-feature-requests"></a>Bugs e solicitações de recursos

Tem um bug ou uma solicitação de recurso? Leia as [diretrizes do problema](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md#using-the-issue-tracker) e pesquise os problemas existentes e resolvidos. Se o seu problema ou ideia ainda não tiver sido abordado, [abra um novo problema](https://github.com/twbs/bootstrap/issues/new).

Observe que **as solicitações de recursos devem ser direcionadas ao [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev),** porque o Bootstrap v3 está no modo de manutenção e está fechado para novos recursos. Assim podemos concentrar nossos esforços no Bootstrap v4.


## <a name="documentation"></a>Documentação

A documentação do Bootstrap, incluída com este repositório no diretório raiz, é criada com o [Jekyll](http://jekyllrb.com) e hospedada publicamente nas Páginas do GitHub em <http://getbootstrap.com>. Os documentos também podem ser executados localmente.

### <a name="running-documentation-locally"></a>Executar a documentação localmente

1. Se necessário, [instale o Jekyll](http://jekyllrb.com/docs/installation) e outras dependências do Ruby com `bundle install`.
   **Observação para usuários do Windows:** Leia [este guia não oficial](http://jekyll-windows.juthilo.com/) para colocar o Jekyll em funcionamento sem problemas.
2. Do diretório raiz do `/bootstrap`, execute `bundle exec jekyll serve` na linha de comando.
4. Abra `http://localhost:9001` no seu navegador e pronto.

Saiba mais sobre como usar o Jekyll lendo a [documentação](http://jekyllrb.com/docs/home/).

### <a name="documentation-for-previous-releases"></a>Documentação de versões anteriores

A documentação da v2.3.2 foi disponibilizada temporariamente em <http://getbootstrap.com/2.3.2/>, enquanto as pessoas realizam a transição para o Bootstrap 3.

[Versões anteriores](https://github.com/twbs/bootstrap/releases) e sua documentação também estão disponíveis para download.


## <a name="contributing"></a>Contribuição

Leia nossas [diretrizes de contribuição](https://github.com/twbs/bootstrap/blob/master/CONTRIBUTING.md). Estão inclusas instruções de como abrir problemas, codificar padrões e notas sobre desenvolvimento.

Além disso, se sua solicitação pull contiver recursos ou correções do JavaScript, inclua [testes de unidades relevantes](https://github.com/twbs/bootstrap/tree/master/js/tests). Todos os HTML e CSS devem estar de acordo com o [Guia de Códigos](https://github.com/mdo/code-guide), mantido por [Mark Otto](https://github.com/mdo).

**O Bootstrap v3 está fechado para novos recursos.** Ele entrou em modo de manutenção para que possamos concentrar nossos esforços no [Bootstrap v4](https://github.com/twbs/bootstrap/tree/v4-dev), o futuro do framework. As solicitações pull que adicionam novos recursos (em vez de corrigir bugs) devem ser destinadas ao [Bootstrap v4 (a ramificação git do `v4-dev`)](https://github.com/twbs/bootstrap/tree/v4-dev).

As preferências do Editor estão disponíveis nas [configurações do editor](https://github.com/twbs/bootstrap/blob/master/.editorconfig) para facilitar a utilização em editores de texto comuns. Leia mais e baixe os plug-ins em <http://editorconfig.org>.


## <a name="community"></a>Comunidade

Obtenha atualizações no chat e desenvolvimento do Bootstrap com os mantenedores do projeto e membros da comunidade.

* Siga [@getbootstrap no Twitter](https://twitter.com/getbootstrap).
* Leia e assine o [Blog Oficial do Bootstrap](http://blog.getbootstrap.com).
* Participe da [sala oficial do Slack](https://bootstrap-slack.herokuapp.com).
* Converse por chat com colegas do Bootstrap no IRC. No servidor do `irc.freenode.net`, no canal `##bootstrap`.
* A ajuda de implementação apode ser encontrada no Stack Overflow (marcado como [`twitter-bootstrap-3`](https://stackoverflow.com/questions/tagged/twitter-bootstrap-3)).
* Os desenvolvedores devem usar a palavra-chave `bootstrap` em pacotes que modificam ou adicionam à funcionalidade do Bootstrap ao distribuir por meio de [npm](https://www.npmjs.com/browse/keyword/bootstrap) ou mecanismos de entrega semelhantes para a descoberta máxima.


## <a name="versioning"></a>Controle de versão

Para garantir a transparência em nosso ciclo de lançamento e ao tentar manter a compatibilidade reversa, o Bootstrap é mantido nas [diretrizes de Versões Semânticas](http://semver.org/). Às vezes falhamos, mas seguimos essas regras sempre que possível.

Confira [a seção Versões do nosso projeto do GitHub](https://github.com/twbs/bootstrap/releases) para obter os logs de alterações para cada versão do Bootstrap. As publicações de anúncios de lançamentos no [blog oficial do Bootstrap](http://blog.getbootstrap.com) contêm resumos das principais alterações feitas em cada versão.


## <a name="creators"></a>Criadores

**Mark Otto**

* <https://twitter.com/mdo>
* <https://github.com/mdo>

**Jacob Thornton**

* <https://twitter.com/fat>
* <https://github.com/fat>


## <a name="copyright-and-license"></a>Direitos autorais e licença

Direitos autorais de código e documentação 2011-2016 Twitter, Inc. Código lançado sob a [licença MIT](https://github.com/twbs/bootstrap/blob/master/LICENSE). Documentos lançados sob licença [Creative Commons](https://github.com/twbs/bootstrap/blob/master/docs/LICENSE).
