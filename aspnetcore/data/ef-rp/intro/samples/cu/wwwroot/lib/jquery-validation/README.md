---
ms.openlocfilehash: 83a958fe6a8d61becf9f9062e8ad0ff69108b435
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57064193"
---
<a name="jquery-validation-pluginhttpjqueryvalidationorg---form-validation-made-easy"></a><span data-ttu-id="2c7e0-101">[Plug-in do jQuery Validation](http://jqueryvalidation.org/) – validação de formulário mais fácil</span><span class="sxs-lookup"><span data-stu-id="2c7e0-101">[jQuery Validation Plugin](http://jqueryvalidation.org/) - Form validation made easy</span></span>
================================

<span data-ttu-id="2c7e0-102">[![Status do Build](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Status de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="2c7e0-102">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="2c7e0-103">O Plug-in de Validação do jQuery oferece uma validação para seus formulários existentes, ao mesmo tempo que faz todos os tipos de personalização para se adequar ao seu aplicativo com facilidade.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-103">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="2c7e0-104">Ajudar o projeto</span><span class="sxs-lookup"><span data-stu-id="2c7e0-104">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="2c7e0-105">[![Ajudar o projeto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="2c7e0-105">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="2c7e0-106">Este projeto está procurando ajuda!</span><span class="sxs-lookup"><span data-stu-id="2c7e0-106">This project is looking for help!</span></span> <span data-ttu-id="2c7e0-107">[Você pode fazer uma doação para a campanha em andamento da pledgie](http://pledgie.com/campaigns/18159) e ajudar na divulgação.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-107">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="2c7e0-108">Se você usou o plug-in, ou tem planos de usar, considere uma doação, qualquer valor ajudará.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-108">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="2c7e0-109">Encontre o plano de como gastar o dinheiro na [página da pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="2c7e0-109">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="getting-started"></a><span data-ttu-id="2c7e0-110">Guia de Introdução</span><span class="sxs-lookup"><span data-stu-id="2c7e0-110">Getting Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="2c7e0-111">Baixar os arquivos pré-criados</span><span class="sxs-lookup"><span data-stu-id="2c7e0-111">Downloading the prebuilt files</span></span>

<span data-ttu-id="2c7e0-112">Os arquivos predefinidos podem ser baixados de http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="2c7e0-112">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="2c7e0-113">Baixar as alterações mais recentes</span><span class="sxs-lookup"><span data-stu-id="2c7e0-113">Downloading the latest changes</span></span>

<span data-ttu-id="2c7e0-114">Os arquivos de implantação não lançados podem ser obtidos por:</span><span class="sxs-lookup"><span data-stu-id="2c7e0-114">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="2c7e0-115">[Baixar](https://github.com/jzaefferer/jquery-validation/archive/master.zip) ou criar bifurcação deste repositório</span><span class="sxs-lookup"><span data-stu-id="2c7e0-115">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="2c7e0-116">Configurar o build</span><span class="sxs-lookup"><span data-stu-id="2c7e0-116">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="2c7e0-117">Execute `grunt` para criar os arquivos criados no diretório "dist"</span><span class="sxs-lookup"><span data-stu-id="2c7e0-117">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="2c7e0-118">Incluí-lo na sua página</span><span class="sxs-lookup"><span data-stu-id="2c7e0-118">Including it on your page</span></span>

<span data-ttu-id="2c7e0-119">Inclua o jQuery e o plug-in em uma página.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-119">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="2c7e0-120">Em seguida, selecione um formulário para validar e chame o método `validate`.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-120">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="2c7e0-121">Como alternativa, inclua o jQuery e o plug-in via requirejs em seu módulo.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-121">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="2c7e0-122">Para obter mais informações sobre como configurar regras e personalizações, [confira a documentação](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="2c7e0-122">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="2c7e0-123">Relatar problemas e código de contribuição</span><span class="sxs-lookup"><span data-stu-id="2c7e0-123">Reporting issues and contributing code</span></span>

<span data-ttu-id="2c7e0-124">Confira as [Diretrizes de Contribuição](CONTRIBUTING.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-124">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="2c7e0-125">**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-125">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="2c7e0-126">A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="2c7e0-126">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="2c7e0-127">Vamos acompanhar e usar a mesma verificação.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-127">We will follow their lead and use the same check.</span></span> <span data-ttu-id="2c7e0-128">Se você acredita que as especificações estejam erradas, denuncie o erro.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-128">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="2c7e0-129">Se você tiver requisitos diferentes, considere [usar um método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="2c7e0-129">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="2c7e0-130">Licença</span><span class="sxs-lookup"><span data-stu-id="2c7e0-130">License</span></span>
<span data-ttu-id="2c7e0-131">Direitos autorais &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="2c7e0-131">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="2c7e0-132">Licenciado sob a licença MIT.</span><span class="sxs-lookup"><span data-stu-id="2c7e0-132">Licensed under the MIT license.</span></span>
