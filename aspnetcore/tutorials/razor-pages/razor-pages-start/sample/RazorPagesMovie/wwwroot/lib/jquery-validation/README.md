---
ms.openlocfilehash: 1b563a8c0674d51f893415221ca8fab574b78265
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57032943"
---
<a name="--"></a>--
================================

<span data-ttu-id="05caf-101">[![Status do Build](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![Status de devDependency](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span><span class="sxs-lookup"><span data-stu-id="05caf-101">[![Build Status](https://secure.travis-ci.org/jzaefferer/jquery-validation.png)](http://travis-ci.org/jzaefferer/jquery-validation)
[![devDependency Status](https://david-dm.org/jzaefferer/jquery-validation/dev-status.png?theme=shields.io)](https://david-dm.org/jzaefferer/jquery-validation#info=devDependencies)</span></span>

<span data-ttu-id="05caf-102">O Plug-in de Validação do jQuery oferece uma validação para seus formulários existentes, ao mesmo tempo que faz todos os tipos de personalização para se adequar ao seu aplicativo com facilidade.</span><span class="sxs-lookup"><span data-stu-id="05caf-102">The jQuery Validation Plugin provides drop-in validation for your existing forms, while making all kinds of customizations to fit your application really easy.</span></span>

## <a name="help-the-projecthttppledgiecomcampaigns18159"></a>[<span data-ttu-id="05caf-103">Ajudar o projeto</span><span class="sxs-lookup"><span data-stu-id="05caf-103">Help the project</span></span>](http://pledgie.com/campaigns/18159)

<span data-ttu-id="05caf-104">[![Ajudar o projeto](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span><span class="sxs-lookup"><span data-stu-id="05caf-104">[![Help the project](http://www.pledgie.com/campaigns/18159.png?skin_name=chrome)](http://pledgie.com/campaigns/18159)</span></span>

<span data-ttu-id="05caf-105">Este projeto está procurando ajuda!</span><span class="sxs-lookup"><span data-stu-id="05caf-105">This project is looking for help!</span></span> <span data-ttu-id="05caf-106">[Você pode fazer uma doação para a campanha em andamento da pledgie](http://pledgie.com/campaigns/18159) e ajudar na divulgação.</span><span class="sxs-lookup"><span data-stu-id="05caf-106">[You can donate to the ongoing pledgie campaign](http://pledgie.com/campaigns/18159) and help spread the word.</span></span> <span data-ttu-id="05caf-107">Se você usou o plug-in, ou tem planos de usar, considere uma doação, qualquer valor ajudará.</span><span class="sxs-lookup"><span data-stu-id="05caf-107">If you've used the plugin, or plan to use, consider a donation - any amount will help.</span></span>

<span data-ttu-id="05caf-108">Encontre o plano de como gastar o dinheiro na [página da pledgie](http://pledgie.com/campaigns/18159).</span><span class="sxs-lookup"><span data-stu-id="05caf-108">You can find the plan for how to spend the money on the [pledgie page](http://pledgie.com/campaigns/18159).</span></span>

## <a name="get-started"></a><span data-ttu-id="05caf-109">Introdução</span><span class="sxs-lookup"><span data-stu-id="05caf-109">Get Started</span></span>

### <a name="downloading-the-prebuilt-files"></a><span data-ttu-id="05caf-110">Baixar os arquivos pré-criados</span><span class="sxs-lookup"><span data-stu-id="05caf-110">Downloading the prebuilt files</span></span>

<span data-ttu-id="05caf-111">Os arquivos predefinidos podem ser baixados de http://jqueryvalidation.org/</span><span class="sxs-lookup"><span data-stu-id="05caf-111">Prebuilt files can be downloaded from http://jqueryvalidation.org/</span></span>

### <a name="downloading-the-latest-changes"></a><span data-ttu-id="05caf-112">Baixar as alterações mais recentes</span><span class="sxs-lookup"><span data-stu-id="05caf-112">Downloading the latest changes</span></span>

<span data-ttu-id="05caf-113">Os arquivos de implantação não lançados podem ser obtidos por:</span><span class="sxs-lookup"><span data-stu-id="05caf-113">The unreleased development files can be obtained by:</span></span>

 1. <span data-ttu-id="05caf-114">[Baixar](https://github.com/jzaefferer/jquery-validation/archive/master.zip) ou criar bifurcação deste repositório</span><span class="sxs-lookup"><span data-stu-id="05caf-114">[Downloading](https://github.com/jzaefferer/jquery-validation/archive/master.zip) or Forking this repository</span></span>
 2. [<span data-ttu-id="05caf-115">Configurar o build</span><span class="sxs-lookup"><span data-stu-id="05caf-115">Setup the build</span></span>](CONTRIBUTING.md#build-setup)
 3. <span data-ttu-id="05caf-116">Execute `grunt` para criar os arquivos criados no diretório "dist"</span><span class="sxs-lookup"><span data-stu-id="05caf-116">Run `grunt` to create the built files in the "dist" directory</span></span>

### <a name="including-it-on-your-page"></a><span data-ttu-id="05caf-117">Incluí-lo na sua página</span><span class="sxs-lookup"><span data-stu-id="05caf-117">Including it on your page</span></span>

<span data-ttu-id="05caf-118">Inclua o jQuery e o plug-in em uma página.</span><span class="sxs-lookup"><span data-stu-id="05caf-118">Include jQuery and the plugin on a page.</span></span> <span data-ttu-id="05caf-119">Em seguida, selecione um formulário para validar e chame o método `validate`.</span><span class="sxs-lookup"><span data-stu-id="05caf-119">Then select a form to validate and call the `validate` method.</span></span>

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

<span data-ttu-id="05caf-120">Como alternativa, inclua o jQuery e o plug-in via requirejs em seu módulo.</span><span class="sxs-lookup"><span data-stu-id="05caf-120">Alternatively include jQuery and the plugin via requirejs in your module.</span></span>

```js
define(["jquery", "jquery.validate"], function( $ ) {
    $("form").validate();
});
```

<span data-ttu-id="05caf-121">Para obter mais informações sobre como configurar regras e personalizações, [confira a documentação](http://jqueryvalidation.org/documentation/).</span><span class="sxs-lookup"><span data-stu-id="05caf-121">For more information on how to setup a rules and customizations, [check the documentation](http://jqueryvalidation.org/documentation/).</span></span>

## <a name="reporting-issues-and-contributing-code"></a><span data-ttu-id="05caf-122">Relatar problemas e código de contribuição</span><span class="sxs-lookup"><span data-stu-id="05caf-122">Reporting issues and contributing code</span></span>

<span data-ttu-id="05caf-123">Confira as [Diretrizes de Contribuição](CONTRIBUTING.md) para obter detalhes.</span><span class="sxs-lookup"><span data-stu-id="05caf-123">See the [Contributing Guidelines](CONTRIBUTING.md) for details.</span></span>

<span data-ttu-id="05caf-124">**OBSERVAÇÃO IMPORTANTE SOBRE A VALIDAÇÃO DE EMAIL**.</span><span class="sxs-lookup"><span data-stu-id="05caf-124">**IMPORTANT NOTE ABOUT EMAIL VALIDATION**.</span></span> <span data-ttu-id="05caf-125">A partir da versão 1.12.0 este plug-in está usando a mesma expressão regular que a [especificação HTML5 sugere que os navegadores usem](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span><span class="sxs-lookup"><span data-stu-id="05caf-125">As of version 1.12.0 this plugin is using the same regular expression that the [HTML5 specification suggests for browsers to use](https://html.spec.whatwg.org/multipage/forms.html#valid-e-mail-address).</span></span> <span data-ttu-id="05caf-126">Vamos acompanhar e usar a mesma verificação.</span><span class="sxs-lookup"><span data-stu-id="05caf-126">We will follow their lead and use the same check.</span></span> <span data-ttu-id="05caf-127">Se você acredita que as especificações estejam erradas, denuncie o erro.</span><span class="sxs-lookup"><span data-stu-id="05caf-127">If you think the specification is wrong, please report the issue to them.</span></span> <span data-ttu-id="05caf-128">Se você tiver requisitos diferentes, considere [usar um método personalizado](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span><span class="sxs-lookup"><span data-stu-id="05caf-128">If you have different requirements, consider [using a custom method](http://jqueryvalidation.org/jQuery.validator.addMethod/).</span></span>

## <a name="license"></a><span data-ttu-id="05caf-129">Licença</span><span class="sxs-lookup"><span data-stu-id="05caf-129">License</span></span>
<span data-ttu-id="05caf-130">Direitos autorais &copy; Jörn Zaefferer</span><span class="sxs-lookup"><span data-stu-id="05caf-130">Copyright &copy; Jörn Zaefferer</span></span><br>
<span data-ttu-id="05caf-131">Licenciado sob a licença MIT.</span><span class="sxs-lookup"><span data-stu-id="05caf-131">Licensed under the MIT license.</span></span>
