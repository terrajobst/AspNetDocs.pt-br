---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: Usar o AJAX para fornecer atualizações dinâmicas | Microsoft Docs
author: microsoft
description: A etapa 10 implementa o suporte para usuários conectados a seu interesse em participar de um jantar, usando uma abordagem baseada em AJAX integrada nos detalhes do jantar...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 3edc02fec546609505b5e085440fa684abe7acd0
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78600845"
---
# <a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="b688a-103">Usar o AJAX para fornecer atualizações dinâmicas</span><span class="sxs-lookup"><span data-stu-id="b688a-103">Use AJAX to Deliver Dynamic Updates</span></span>

<span data-ttu-id="b688a-104">pela [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="b688a-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="b688a-105">Baixar PDF</span><span class="sxs-lookup"><span data-stu-id="b688a-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="b688a-106">Esta é a etapa 10 de um [tutorial de aplicativo "NerdDinner"](introducing-the-nerddinner-tutorial.md) gratuito que percorre como criar um aplicativo Web pequeno, mas completo usando o ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="b688a-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="b688a-107">A etapa 10 implementa o suporte para usuários conectados a seu interesse em participar de um jantar, usando uma abordagem baseada em AJAX integrada na página de detalhes do jantar.</span><span class="sxs-lookup"><span data-stu-id="b688a-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="b688a-108">Se você estiver usando o ASP.NET MVC 3, recomendamos seguir as [introdução com](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) os tutoriais da [loja de música](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) MVC 3 ou MVC.</span><span class="sxs-lookup"><span data-stu-id="b688a-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>

## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="b688a-109">Etapa 10 do NerdDinner: habilitação do AJAX para aceitar</span><span class="sxs-lookup"><span data-stu-id="b688a-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="b688a-110">Agora, vamos implementar o suporte para usuários conectados a seu interesse em participar de um jantar.</span><span class="sxs-lookup"><span data-stu-id="b688a-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="b688a-111">Habilitaremos isso usando uma abordagem baseada em AJAX integrada na página de detalhes do jantar.</span><span class="sxs-lookup"><span data-stu-id="b688a-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="b688a-112">Indicando se o usuário é o RSVP</span><span class="sxs-lookup"><span data-stu-id="b688a-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="b688a-113">Os usuários podem visitar a URL do */Dinners/Details/[ID*] para ver detalhes sobre um jantar específico:</span><span class="sxs-lookup"><span data-stu-id="b688a-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="b688a-114">O método de ação Details () é implementado da seguinte forma:</span><span class="sxs-lookup"><span data-stu-id="b688a-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="b688a-115">Nossa primeira etapa para implementar o suporte a RSVP será adicionar um método auxiliar "IsUserRegistered (username)" ao nosso objeto de jantar (dentro da classe parcial Dinner.cs que criamos anteriormente).</span><span class="sxs-lookup"><span data-stu-id="b688a-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="b688a-116">Esse método auxiliar retorna true ou false, dependendo se o usuário é atualmente RSVP para o jantar:</span><span class="sxs-lookup"><span data-stu-id="b688a-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="b688a-117">Em seguida, podemos adicionar o código a seguir ao nosso modelo de exibição details. aspx para exibir uma mensagem apropriada indicando se o usuário está registrado ou não para o evento:</span><span class="sxs-lookup"><span data-stu-id="b688a-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="b688a-118">E agora, quando um usuário visita um jantar, ele está registrado para receber esta mensagem:</span><span class="sxs-lookup"><span data-stu-id="b688a-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="b688a-119">E quando visitarem um jantar, eles não serão registrados para eles verão a mensagem abaixo:</span><span class="sxs-lookup"><span data-stu-id="b688a-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="b688a-120">Implementando o método de ação de registro</span><span class="sxs-lookup"><span data-stu-id="b688a-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="b688a-121">Agora, vamos adicionar a funcionalidade necessária para permitir que os usuários façam o RSVP para um jantar da página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="b688a-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="b688a-122">Para implementar isso, criaremos uma nova classe "RSVPController" clicando com o botão direito do mouse no diretório \Controllers e escolhendo o comando de menu do controlador Add-&gt;.</span><span class="sxs-lookup"><span data-stu-id="b688a-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="b688a-123">Vamos implementar um método de ação "Register" dentro da nova classe RSVPController que usa uma ID para um jantar como um argumento, recupera o objeto de jantar apropriado, verifica se o usuário conectado está atualmente na lista de usuários que se registraram para ele e se Não adiciona um objeto RSVP para eles:</span><span class="sxs-lookup"><span data-stu-id="b688a-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="b688a-124">Observe que, acima de como estamos retornando uma cadeia de caracteres simples como a saída do método de ação.</span><span class="sxs-lookup"><span data-stu-id="b688a-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="b688a-125">Poderíamos ter inserido essa mensagem em um modelo de exibição – mas, como é tão pequeno, usaremos o método auxiliar Content () na classe base Controller e retornaremos uma mensagem de cadeia de caracteres como acima.</span><span class="sxs-lookup"><span data-stu-id="b688a-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="b688a-126">Chamando o método de ação RSVPForEvent usando AJAX</span><span class="sxs-lookup"><span data-stu-id="b688a-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="b688a-127">Usaremos o AJAX para invocar o método de ação de registro de nossa exibição de detalhes.</span><span class="sxs-lookup"><span data-stu-id="b688a-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="b688a-128">Implementar isso é muito fácil.</span><span class="sxs-lookup"><span data-stu-id="b688a-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="b688a-129">Primeiro, adicionaremos duas referências de biblioteca de script:</span><span class="sxs-lookup"><span data-stu-id="b688a-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="b688a-130">A primeira biblioteca faz referência à biblioteca de scripts do lado do cliente do ASP.NET AJAX básico.</span><span class="sxs-lookup"><span data-stu-id="b688a-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="b688a-131">Esse arquivo tem aproximadamente 24K de tamanho (compactado) e contém a funcionalidade básica do AJAX do lado do cliente.</span><span class="sxs-lookup"><span data-stu-id="b688a-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="b688a-132">A segunda biblioteca contém funções utilitárias que se integram aos métodos auxiliares AJAX internos do ASP.NET MVC (que usaremos em breve).</span><span class="sxs-lookup"><span data-stu-id="b688a-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="b688a-133">Em seguida, podemos atualizar o código do modelo de exibição que adicionamos anteriormente para que, em vez de gerar uma mensagem "você não está registrado para esse evento", em vez disso, renderizamos um link que, quando enviado por push, executa uma chamada AJAX que invoca nosso método de ação RSVPForEvent em nosso controlador RSVP e RSVPs o usuário:</span><span class="sxs-lookup"><span data-stu-id="b688a-133">We can then update the view template code we added earlier so that instead of outputting a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="b688a-134">O método auxiliar Ajax. ActionLink () usado acima é integrado ao MVC ASP.NET e é semelhante ao método auxiliar HTML. ActionLink (), exceto que, em vez de executar uma navegação padrão, ele faz uma chamada AJAX para o método de ação quando o link é clicado.</span><span class="sxs-lookup"><span data-stu-id="b688a-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="b688a-135">Acima, estamos chamando o método de ação "Register" no controlador "RSVP" e passando o Jantarid como o parâmetro "ID" para ele.</span><span class="sxs-lookup"><span data-stu-id="b688a-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="b688a-136">O parâmetro final AjaxOptions que estamos passando indica que queremos pegar o conteúdo retornado do método Action e atualizar o elemento HTML &lt;div&gt; na página cuja ID é "rsvpmsg".</span><span class="sxs-lookup"><span data-stu-id="b688a-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="b688a-137">E agora, quando um usuário navega para um jantar para o qual não está registrado, ainda assim ele verá um link para o RSVP para ele:</span><span class="sxs-lookup"><span data-stu-id="b688a-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="b688a-138">Se eles clicarem no link "RSVP para este evento", eles farão uma chamada AJAX para o método de ação registrar no controlador RSVP e, quando ele for concluído, verão uma mensagem atualizada, como a seguir:</span><span class="sxs-lookup"><span data-stu-id="b688a-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="b688a-139">A largura de banda de rede e o tráfego envolvidos ao fazer essa chamada AJAX é realmente leve.</span><span class="sxs-lookup"><span data-stu-id="b688a-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="b688a-140">Quando o usuário clica no link "RSVP para este evento", uma pequena solicitação de rede HTTP POST é feita à URL */Dinners/Register/1* que se parece com a seguinte na conexão:</span><span class="sxs-lookup"><span data-stu-id="b688a-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="b688a-141">E a resposta do nosso método de ação de registro é simplesmente:</span><span class="sxs-lookup"><span data-stu-id="b688a-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="b688a-142">Essa chamada leve é rápida e funcionará mesmo em uma rede lenta.</span><span class="sxs-lookup"><span data-stu-id="b688a-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="b688a-143">Adicionando uma animação do jQuery</span><span class="sxs-lookup"><span data-stu-id="b688a-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="b688a-144">A funcionalidade do AJAX que implementamos funciona bem e rápida.</span><span class="sxs-lookup"><span data-stu-id="b688a-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="b688a-145">Às vezes, pode acontecer tão rápido que um usuário talvez não perceba que o link RSVP foi substituído por um novo texto.</span><span class="sxs-lookup"><span data-stu-id="b688a-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="b688a-146">Para tornar o resultado um pouco mais óbvio, podemos adicionar uma animação simples para chamar atenção para a mensagem de atualização.</span><span class="sxs-lookup"><span data-stu-id="b688a-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="b688a-147">O modelo de projeto ASP.NET MVC padrão inclui jQuery – uma biblioteca JavaScript de software livre excelente (e muito popular) que também é suportada pela Microsoft.</span><span class="sxs-lookup"><span data-stu-id="b688a-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="b688a-148">o jQuery fornece vários recursos, incluindo uma boa biblioteca de efeitos e seleção de DOM HTML.</span><span class="sxs-lookup"><span data-stu-id="b688a-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="b688a-149">Para usar o jQuery, primeiro adicionaremos uma referência de script a ele.</span><span class="sxs-lookup"><span data-stu-id="b688a-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="b688a-150">Como vamos usar o jQuery dentro de uma variedade de locais em nosso site, adicionaremos a referência de script em nosso site. Master Master Page file para que todas as páginas possam usá-lo.</span><span class="sxs-lookup"><span data-stu-id="b688a-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="b688a-151">*Dica: Verifique se você instalou o hotfix do JavaScript IntelliSense para VS 2008 SP1 que permite suporte mais avançado ao IntelliSense para arquivos JavaScript (incluindo jQuery). Você pode baixá-lo em: http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="b688a-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="b688a-152">O código escrito usando o JQuery geralmente usa um método JavaScript "$ ()" global que recupera um ou mais elementos HTML usando um seletor de CSS.</span><span class="sxs-lookup"><span data-stu-id="b688a-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="b688a-153">Por exemplo, *$ ("#rsvpmsg")* seleciona qualquer elemento HTML com a ID de rsvpmsg, enquanto *$ (". algo")* selecionaria todos os elementos com o nome de classe CSS "algo".</span><span class="sxs-lookup"><span data-stu-id="b688a-153">For example, *$("#rsvpmsg")* selects any HTML element with the id of rsvpmsg, while *$(".something")* would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="b688a-154">Você também pode escrever consultas mais avançadas como "retornar todos os botões de opção marcados" usando uma consulta de seletor como: *$ ("Input [@type= Radio] [@checked]")* .</span><span class="sxs-lookup"><span data-stu-id="b688a-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: *$("input[@type=radio][@checked]")*.</span></span>

<span data-ttu-id="b688a-155">Depois de selecionar elementos, você pode chamar métodos para executar ações, como ocultá-los: *$ ("#rsvpmsg"). Hide ();*</span><span class="sxs-lookup"><span data-stu-id="b688a-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="b688a-156">Para nosso cenário de RSVP, definiremos uma função JavaScript simples denominada "AnimateRSVPMessage", que seleciona o &lt;&gt; div "rsvpmsg" e anima o tamanho de seu conteúdo de texto.</span><span class="sxs-lookup"><span data-stu-id="b688a-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="b688a-157">O código abaixo inicia o texto pequeno e faz com que ele aumente em um período de 400 milissegundos:</span><span class="sxs-lookup"><span data-stu-id="b688a-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="b688a-158">Em seguida, podemos conectar essa função JavaScript a ser chamada depois que nossa chamada AJAX for concluída com êxito passando seu nome para nosso método auxiliar Ajax. ActionLink () (por meio da propriedade de evento AjaxOptions "OnSuccess"):</span><span class="sxs-lookup"><span data-stu-id="b688a-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="b688a-159">E agora, quando o link "RSVP para este evento" for clicado e nossa chamada AJAX for concluída com êxito, a mensagem de conteúdo enviada será animada e crescerá grande:</span><span class="sxs-lookup"><span data-stu-id="b688a-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="b688a-160">Além de fornecer um evento "OnSuccess", o objeto AjaxOptions expõe eventos OnBegin, OnFailure e OnComplete que você pode manipular (juntamente com uma variedade de outras propriedades e opções úteis).</span><span class="sxs-lookup"><span data-stu-id="b688a-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="b688a-161">Limpeza-refatorar uma exibição parcial de RSVP</span><span class="sxs-lookup"><span data-stu-id="b688a-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="b688a-162">Nosso modelo de exibição de detalhes está começando a ter um pouco de tempo, que a hora extra irá torná-lo um pouco mais difícil de entender.</span><span class="sxs-lookup"><span data-stu-id="b688a-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="b688a-163">Para ajudar a melhorar a legibilidade do código, vamos concluir criando uma exibição parcial – RSVPStatus. ascx – que encapsula todo o código de exibição de RSVP para nossa página de detalhes.</span><span class="sxs-lookup"><span data-stu-id="b688a-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="b688a-164">Podemos fazer isso clicando com o botão direito do mouse na pasta \Views\Dinners e escolhendo o comando de menu Add-&gt;View.</span><span class="sxs-lookup"><span data-stu-id="b688a-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="b688a-165">Vamos pegar um objeto de jantar como seu ViewModel fortemente tipado.</span><span class="sxs-lookup"><span data-stu-id="b688a-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="b688a-166">Em seguida, podemos copiar/colar o conteúdo RSVP de nossa exibição details. aspx nele.</span><span class="sxs-lookup"><span data-stu-id="b688a-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="b688a-167">Depois de fazer isso, vamos criar também outra exibição parcial – EditAndDeleteLinks. ascx, que encapsula nosso código de exibição de link de edição e exclusão.</span><span class="sxs-lookup"><span data-stu-id="b688a-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="b688a-168">Também vamos pegar um objeto de jantar como seu ViewModel fortemente tipado e copiar/colar a lógica de edição e exclusão de nossa exibição details. aspx para ela.</span><span class="sxs-lookup"><span data-stu-id="b688a-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="b688a-169">Nosso modelo de exibição de detalhes pode, então, incluir apenas duas chamadas de método html. RenderPartial () na parte inferior:</span><span class="sxs-lookup"><span data-stu-id="b688a-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="b688a-170">Isso torna o limpador de código ler e manter.</span><span class="sxs-lookup"><span data-stu-id="b688a-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="b688a-171">Próxima etapa</span><span class="sxs-lookup"><span data-stu-id="b688a-171">Next Step</span></span>

<span data-ttu-id="b688a-172">Agora, vamos examinar como podemos usar o AJAX ainda mais e adicionar suporte de mapeamento interativo ao nosso aplicativo.</span><span class="sxs-lookup"><span data-stu-id="b688a-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b688a-173">[Anterior](secure-applications-using-authentication-and-authorization.md)
> [Próximo](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="b688a-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
