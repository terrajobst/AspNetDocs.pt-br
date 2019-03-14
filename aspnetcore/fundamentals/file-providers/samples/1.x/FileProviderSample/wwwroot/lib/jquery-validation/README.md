---
ms.openlocfilehash: 24c36493284988aa45b15c78e2577c0ef96aba56
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57029983"
---
<a name="jquery-validation-pluginhttpsjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="cd83d-101">[Plug-in do jQuery Validation](https://jqueryvalidation.org/) – validação de formulário mais fácil</span><span class="sxs-lookup"><span data-stu-id="cd83d-101">[jQuery Validation Plugin](https://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="cd83d-102">[![Status do Build](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![Status de devDependency](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="cd83d-102">[![Build Status](https://secure.travis-ci.org/jquery-validation/jquery-validation.svg)](https://travis-ci.org/jquery-validation/jquery-validation)
[![devDependency Status](https://david-dm.org/jquery-validation/jquery-validation/dev-status.svg?theme=shields.io)](https://david-dm.org/jquery-validation/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="cd83d-103">O Plug-in de Validação do jQuery oferece uma validação para seus formulários existentes, ao mesmo tempo que faz todos os tipos de personalização para se adequar ao seu aplicativo com facilidade.</span><span class="sxs-lookup"><span data-stu-id="cd83d-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="getting-started"></a><span data-ttu-id="cd83d-104">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="cd83d-104">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="cd83d-105">Baixar os arquivos pré-criados</span><span class="sxs-lookup"><span data-stu-id="cd83d-105">Downloading the prebuilt files</span></span>

<span data-ttu-id="cd83d-106">Os arquivos predefinidos podem ser baixados de https://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="cd83d-106">Prebuilt files can be downloaded from https://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="cd83d-107">Baixar as alterações mais recentes</span><span class="sxs-lookup"><span data-stu-id="cd83d-107">Downloading the latest changes</span></span>

<span data-ttu-id="cd83d-108">Os arquivos de implantação não lançados podem ser obtidos por:</span><span class="sxs-lookup"><span data-stu-id="cd83d-108">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="cd83d-109">[Baixar](https://github.com/jquery-validation/jquery-validation/archive/master.zip) ou criar bifurcação deste repositório</span><span class="sxs-lookup"><span data-stu-id="cd83d-109">[Downloading](https://github.com/jquery-validation/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="cd83d-110">Configurar o build</span><span class="sxs-lookup"><span data-stu-id="cd83d-110">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="cd83d-111">Execute `grunt` para criar os arquivos criados no diretório "dist"</span><span class="sxs-lookup"><span data-stu-id="cd83d-111">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="cd83d-112">Incluí-lo na sua página</span><span class="sxs-lookup"><span data-stu-id="cd83d-112">Including it on your page</span></span>

<span data-ttu-id="cd83d-113">Inclua o jQuery e o plug-in em uma página.</span><span class="sxs-lookup"><span data-stu-id="cd83d-113">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="cd83d-114">Em seguida, selecione um formulário para validar e chame o método `validate`.</span><span class="sxs-lookup"><span data-stu-id="cd83d-114">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="cd83d-115">Como alternativa, inclua o jQuery e o plug-in via requirejs em seu módulo.</span><span class="sxs-lookup"><span data-stu-id="cd83d-115">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="cd83d-116">Para obter mais informações sobre como configurar regras e personalizações, [confira a documentação](https://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="cd83d-116">For more information on how to setup a rules and customizations, [check the documentation](https://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="cd83d-117">Relatar problemas e código de contribuição</span><span class="sxs-lookup"><span data-stu-id="cd83d-117">Reporting issues and contributing code</span></span>

<span data-ttu-id="cd83d-118">Confira as [Diretrizes de Contribuição](CONTRIBUTING.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="cd83d-118">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="cd83d-119">**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**.</span><span class="sxs-lookup"><span data-stu-id="cd83d-119">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="cd83d-120">A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="cd83d-120">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="cd83d-121">Vamos acompanhar e usar a mesma verificação.</span><span class="sxs-lookup"><span data-stu-id="cd83d-121">We will follow their lead and use the same check.</span></span> <span data-ttu-id="cd83d-122">Se você acredita que as especificações estejam erradas, denuncie o erro.</span><span class="sxs-lookup"><span data-stu-id="cd83d-122">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="cd83d-123">Se você tiver requisitos diferentes, considere [usar um método personalizado](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="cd83d-123">If you have different requirements, consider [using a custom method](https://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>
<span data-ttu-id="cd83d-124">Caso você precise ajustar os padrões de expressão regular da validação interna, [siga a documentação](https://jqueryvalidation.org/jQuery.validator.methods/).</span><span class="sxs-lookup"><span data-stu-id="cd83d-124">In case you need to adjust the built-in validation regular expression patterns, please [follow the documentation](https://jqueryvalidation.org/jQuery.validator.methods/).</span></span>

<span data-ttu-id="cd83d-125">**NOTA IMPORTANTE SOBRE O MÉTODO REQUERIDO**.</span><span class="sxs-lookup"><span data-stu-id="cd83d-125">**IMPORTANT NOTE ABOUT REQUIRED METHOD**.</span></span> <span data-ttu-id="cd83d-126">A partir da versão 1.14.0, esse plug-in interrompe a remoção de espaços em branco do valor do elemento anexado.</span><span class="sxs-lookup"><span data-stu-id="cd83d-126">As of version 1.14.0 this plugin stops trimming white spaces from the value of the attached element.</span></span> <span data-ttu-id="cd83d-127">Se você deseja obter o mesmo resultado, pode usar o [`normalizer`](https://jqueryvalidation.org/normalizer/) que pode ser usado para transformar o valor de um elemento antes da validação.</span><span class="sxs-lookup"><span data-stu-id="cd83d-127">If you want to achieve the same result, you can use the [`normalizer`](https://jqueryvalidation.org/normalizer/) that can be used to transform the value of an element before validation.</span></span> <span data-ttu-id="cd83d-128">Este recurso estava disponível desde `v1.15.0`.</span><span class="sxs-lookup"><span data-stu-id="cd83d-128">This feature was available since `v1.15.0`.</span></span> <span data-ttu-id="cd83d-129">Em outras palavras, você pode fazer algo parecido com isto:</span><span class="sxs-lookup"><span data-stu-id="cd83d-129">In other words, you can do something like this:</span></span>
``` js
$("#myForm").validate({
    rules: {
        username: {
            required: true,
            // Using the normalizer to trim the value of the element
            // before validating it.
            //
            // The value of `this` inside the `normalizer` is the corresponding
            // DOMElement. In this example, `this` references the `username` element.
            normalizer: function(value) {
                return $.trim(value);
            }
        }
    }
});
```

## <a name="license"></a><span data-ttu-id="cd83d-130">Licença</span><span class="sxs-lookup"><span data-stu-id="cd83d-130">License</span></span>
<span data-ttu-id="cd83d-131">Direitos autorais &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="cd83d-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="cd83d-132">Licenciado sob a licença MIT.</span><span class="sxs-lookup"><span data-stu-id="cd83d-132">Licensed under the MIT license.</span></span>
