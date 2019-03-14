---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57034983"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a>[Plug-in do jQuery Validation](http://jqueryvalidation.org/) – validação de formulário mais fácil
================================

[![Status do Build](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Status de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)

O Plug-in de Validação do jQuery oferece uma validação para seus formulários existentes, ao mesmo tempo que faz todos os tipos de personalização para se adequar ao seu aplicativo com facilidade.

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[Ajudar o projeto](http://pledgie.com/campaigns/18159)

[![Ajudar o projeto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)

Este projeto está procurando ajuda! [Você pode fazer uma doação para a campanha em andamento da pledgie](http://pledgie.com/campaigns/18159) e ajudar na divulgação. Se você usou o plug-in, ou tem planos de usar, considere uma doação, qualquer valor ajudará.

Encontre o plano de como gastar o dinheiro na [página da pledgie](http://pledgie.com/campaigns/18159).

## <a name="getting-started"></a>Guia de Introdução

### <a name="downloading-the-prebuilt-files"></a>Baixar os arquivos pré-criados

Os arquivos predefinidos podem ser baixados de http://jqueryvalidation.org/

### <a name="downloading-the-latest-changes"></a>Baixar as alterações mais recentes

Os arquivos de implantação não lançados podem ser obtidos por:

 1. [Baixar](https://github.com/jzaefferer/jquery-validation/archive/master.zip) ou criar bifurcação deste repositório
 2. [Configurar o build](CONTRIBUTING.md#build-setup)
 3. Execute `grunt` para criar os arquivos criados no diretório "dist"

### <a name="including-it-on-your-page"></a>Incluí-lo na sua página

Inclua o jQuery e o plug-in em uma página. Em seguida, selecione um formulário para validar e chame o método `validate`.

```html
<form>
    <input required>
</form>
<script src="jquery.js"></script>
<script src="jquery.validate.js"></script>
<script>
$("form").validate();
</script>
```

Como alternativa, inclua o jQuery e o plug-in via requirejs em seu módulo.

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

Para obter mais informações sobre como configurar regras e personalizações, [confira a documentação](http://jqueryvalidation.org/documentation/).

## <a name="reporting-issues-and-contributing-code"></a>Relatar problemas e código de contribuição

Confira as [Diretrizes de Contribuição](CONTRIBUTING.md) para obter detalhes.

**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**. A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address). Vamos acompanhar e usar a mesma verificação. Se você acredita que as especificações estejam erradas, denuncie o erro. Se você tiver requisitos diferentes, considere [usar um método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).

## <a name="license"></a>Licença
Direitos autorais &copy; Jörn Zaefferer<br>
Licenciado sob a licença MIT.
