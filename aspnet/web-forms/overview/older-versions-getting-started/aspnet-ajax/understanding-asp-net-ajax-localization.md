---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Entendendo a localização do ASP.NET AJAX | Microsoft Docs
author: scottcate
description: A localização é o processo de design e integração de suporte para um idioma e cultura específicos em um aplicativo ou um componente de aplicativo. O MIC...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 003e7939accd7a68dab97441b3d999bca835b85a
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78566216"
---
# <a name="understanding-aspnet-ajax-localization"></a>Noções básicas sobre a localização do AJAX ASP.NET

por [Scott Cate](https://github.com/scottcate)

[Baixar PDF](https://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> A localização é o processo de design e integração de suporte para um idioma e cultura específicos em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para a localização de aplicativos ASP.NET padrão integrando o modelo de localização do .NET padrão; a estrutura do Microsoft AJAX utiliza o modelo integrado para dar suporte aos diversos cenários em que a localização pode ser executada.

## <a name="introduction"></a>Introdução

A tecnologia ASP.NET da Microsoft traz um modelo de programação orientado a objeto e orientado a eventos e o une com os benefícios do código compilado. No entanto, seu modelo de processamento do lado do servidor tem várias desvantagens inerentes à tecnologia, muitas das quais podem ser abordadas pelos novos recursos incluídos no namespace System. Web. Extensions, que encapsula os serviços do Microsoft AJAX na .NET Framework 3,5. Essas extensões habilitam muitos recursos avançados do cliente, anteriormente disponíveis como parte das extensões AJAX do ASP.NET 2,0, mas agora fazem parte da biblioteca de classes base do Framework. Os controles e recursos nesse namespace incluem o processamento parcial de páginas sem a necessidade de uma atualização de página completa, a capacidade de acessar serviços Web por meio de script de cliente (incluindo a API de criação de perfil do ASP.NET) e uma API extensiva do lado do cliente projetada para espelhar muitos de os esquemas de controle vistos no conjunto de controle do lado do servidor ASP.NET.

Este White Paper examina os recursos de localização presentes na Microsoft AJAX Framework e na biblioteca de scripts do Microsoft AJAX, no contexto da necessidade comercial de suporte à localização e na revisão do suporte já integrado para localização na Web aplicativos fornecidos pelo .NET Framework. A biblioteca de scripts do Microsoft AJAX utiliza o formato de arquivo. resx já usado por aplicativos .NET, que fornece suporte integrado a IDE e um tipo de recurso compartilhável.

Este White Paper se baseia na versão beta 2 do Microsoft Visual Studio 2008. Este White Paper também pressupõe que você trabalhará com o Visual Studio 2008, não com o Visual Web Developer Express e fornecerá orientações de acordo com a interface do usuário do Visual Studio. Alguns exemplos de código usarão modelos de projeto que podem estar indisponíveis no Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*A necessidade de localização*

Especialmente para desenvolvedores de aplicativos empresariais e desenvolvedores de componentes, a capacidade de criar ferramentas que podem estar cientes das diferenças entre culturas e linguagens tornou-se cada vez mais necessária. A criação de componentes com a capacidade de adaptar-se à localidade do cliente aumenta a produtividade do desenvolvedor e reduz a quantidade de trabalho necessária para a adaptação de um componente para funcionar globalmente.

A localização é o processo de design e integração de suporte para um idioma e cultura específicos em um aplicativo ou um componente de aplicativo. A plataforma Microsoft ASP.NET fornece amplo suporte para a localização de aplicativos ASP.NET padrão integrando o modelo de localização do .NET padrão; a estrutura do Microsoft AJAX utiliza o modelo integrado para dar suporte aos diversos cenários em que a localização pode ser executada. Com o Microsoft AJAX Framework, os scripts podem ser localizados ao serem implantados em assemblies satélite ou utilizando uma estrutura de sistema de arquivos estática.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Inserindo scripts com assemblies satélite*

Consistente com a estratégia de localização de .NET Framework padrão, os recursos podem ser incluídos em assemblies satélites. Os assemblies satélite fornecem várias vantagens sobre a inclusão de recursos tradicionais em binários. qualquer localização específica pode ser atualizada sem Atualizar a imagem maior, localizações adicionais podem ser implantadas simplesmente instalando assemblies satélite em a pasta do projeto e os assemblies satélite podem ser implantados sem causar uma recarga do assembly do projeto principal. Especialmente em projetos ASP.NETs, isso é benéfico porque pode reduzir significativamente a quantidade de recursos do sistema usados por atualizações incrementais e interrompe minimamente o uso do site de produção.

Os scripts são inseridos em assemblies, incluindo-os em arquivos. resx (ou compiled. Resources) gerenciados, que são incluídos no assembly em tempo de compilação. Seus recursos são disponibilizados para o aplicativo de script por meio do código gerado pelo tempo de execução AJAX, por meio de atributos de nível de assembly

*Convenções de nomenclatura para arquivos de script inseridos*

O gerenciamento de scripts do Microsoft AJAX Framework dá suporte a várias opções para uso na implantação e no teste de scripts, e as diretrizes são fornecidas para facilitar essas opções.

*Para facilitar a depuração:*

Os scripts de versão (produção) não devem incluir o qualificador de `.debug` no nome do arquivo. Os scripts criados para depuração devem incluir `.debug` no nome do arquivo.

*Para facilitar a localização:*

Os scripts de cultura neutra não devem incluir nenhum identificador de cultura no nome do arquivo. Para scripts que contêm recursos localizados, o código de idioma ISO deve ser especificado no nome do arquivo. Por exemplo, `es-CO` significa espanhol, Colúmbia.

A tabela a seguir resume as convenções de nomenclatura de arquivo com exemplos:

| Nome de arquivo | Significado |
| --- | --- |
| Script.js | Um script de cultura neutra de versão release. |
| Script.debug.js | Um script de cultura neutra de versão de depuração. |
| Script.en-US.js | Uma versão de lançamento em inglês, Estados Unidos script. |
| Script.debug.es-CO.js | Um script de Colômbia de versão de depuração espanhol. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Walkthrough: criar um script localizado e inserido

*Observação: este passo a passos requer o uso do Visual Studio 2008, pois o Visual Web Developer Express não inclui um modelo de projeto para projetos de biblioteca de classes.*

1. Crie um novo projeto de site com extensões AJAX ASP.NET integradas. Crie outro projeto, um projeto de biblioteca de classes, dentro da solução chamada LocalizingResources.
2. Adicione um arquivo JScript chamado VerifyDeletion. js ao projeto LocalizingResources, bem como os arquivos de recursos. resx chamados DeletionResources. resx e DeletionResources. es. resx. O primeiro conterá recursos neutros à cultura; o último conterá recursos de idioma espanhol.
3. Adicione o seguinte código a VerifyDeletion. js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para aqueles que não conhecem a sintaxe de Regex de JavaScript, o texto em barras simples (no exemplo anterior,/FILENAME/é um exemplo) denota um objeto RegExp. A biblioteca MSDN contém uma referência ampla de JavaScript, e os recursos em objetos nativos JavaScript podem ser encontrados online.

1. Adicione as seguintes cadeias de caracteres de recurso a DeletionResources. resx: 

    **VerifyDelete**: tem certeza de que deseja excluir o nome de arquivo?

    **Excluído**: o nome de arquivo foi excluído.

1. Adicione as seguintes cadeias de caracteres de recurso a DeletionResources. es. resx: 

    **VerifyDelete**: est seguro que DESEE quitar filename?

    **Excluído**: filename se ha quitado.
2. Adicione as seguintes linhas de código ao arquivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Adicione referências a System. Web e System. Web. Extensions ao projeto LocalizingResources.
2. Adicione uma referência ao projeto LocalizingResources do projeto de site.
3. Em Default. aspx, no projeto do site, atualize o controle do ScriptManager com a seguinte marcação adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. Em Default. aspx, em qualquer lugar da página, inclua esta marcação:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Pressione F5. Se solicitado, habilite a depuração. Quando a página for carregada, pressione o botão excluir. Observe que você será solicitado em inglês (a menos que o computador esteja configurado para preferir recursos de idioma espanhol por padrão) para confirmação.
2. Feche a janela do navegador e retorne para default. aspx. Na diretiva de cabeçalho @Page, substitua auto para Culture e UICulture por es-ES. Pressione F5 novamente para iniciar o aplicativo Web no navegador novamente. Desta vez, observe que você será solicitado a excluir o arquivo em espanhol:

[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image3.png))

[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Clique para exibir a imagem em tamanho normal](understanding-asp-net-ajax-localization/_static/image6.png))

Observe que há várias variações para este passo a passos. Por exemplo, os scripts podem ser registrados com o controle ScriptManager programaticamente durante o carregamento da página.

## <a name="including-a-static-script-file-structure"></a>*Incluindo uma estrutura de arquivo de script estático*

Ao usar arquivos de script estático para implantação, você perde alguns dos benefícios de usar o esquema de localização inerente do .NET. Visível principalmente é que você perde o tipo automático gerado de incluir arquivos de recurso de script; na explicação passo acima, por exemplo, os recursos foram expostos por um tipo gerado automaticamente chamado Message do controle ScriptManager.

No entanto, há alguns benefícios em usar uma estrutura de arquivo de script estático. As atualizações podem ser executadas sem recompilar e reimplantar assemblies satélites, e o uso de uma estrutura de arquivo estático também pode ser feito para substituir o script inserido, para integrar uma pequena parte da funcionalidade que pode não ter sido enviada com um componente.

A Microsoft recomenda evitar um problema de controle de versão gerando automaticamente os recursos de script durante a compilação do projeto. Ao manter uma base de código de script extensiva, pode ser cada vez mais difícil garantir que as alterações de código sejam refletidas em cada script localizado. Como alternativa, você pode simplesmente manter um script lógico e vários scripts de localização, mesclando os arquivos durante a compilação do projeto.

Como não há recursos para incluir declarativamente, os arquivos de script estáticos devem ser referenciados adicionando elementos `<asp:ScriptElement>` como um filho da marca `<Scripts>` do controle ScriptManager ou adicionando programaticamente `ScriptReference` objetos à propriedade `Scripts` do controle `ScriptManager` na página em tempo de execução.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*O ScriptManager e sua função na localização*

O ScriptManager permite vários comportamentos automáticos para aplicativos localizados:

- Ele localiza automaticamente os arquivos de script com base nas configurações e nas convenções de nomenclatura; por exemplo, ele carrega scripts habilitados para depuração quando estiver no modo de depuração e carrega scripts localizados com base na seleção de interface do usuário do navegador.
- Ele habilita a definição de culturas, incluindo culturas personalizadas.
- Ele habilita a compactação de arquivos de script via HTTP.
- Ele armazena em cache os scripts para gerenciar muitas solicitações com eficiência.
- Ele adiciona uma camada de indireção a scripts, canalizando-os por meio de uma URL criptografada.

As referências de script podem ser adicionadas ao controle do ScriptManager de forma programática ou por meio de marcação declarativa. A marcação declarativa é particularmente útil ao trabalhar com scripts inseridos em assemblies que não sejam o próprio projeto de site, pois o nome do script provavelmente não será alterado à medida que as revisões são enviadas por push.

## <a name="summary"></a>Resumo

À medida que os aplicativos da Web crescem para alcançar um público maior, a necessidade de alcançar culturas e comunidades mais amplas se torna fundamental para um modelo de negócios; aplicativos Web de comércio eletrônico precisam ser capazes de lidar com moedas estrangeiras, os sistemas de gerenciamento de conteúdo precisam ser capazes de não apenas apresentar seu conteúdo, mas também suas dicas de navegação e campos de formulário em outras linguagens, e as empresas precisam saber que essa necessidade é está.

O .NET Framework é intrinsecamente compatível com uma estrutura de localização avançada, utilizando assemblies satélite e arquivos de recurso XML (. resx) para apresentar uma maneira uniforme de Pesquisar cadeias de caracteres de recursos e imagens. As extensões do ASP.NET AJAX, incluindo o Microsoft AJAX Framework e a biblioteca de scripts do Microsoft AJAX, fornecem suporte para esse modelo de programação no código do lado do cliente, permitindo pesquisas de cadeia de caracteres de recurso simples. Os assemblies satélite dão suporte à inclusão automática de recursos de script (arquivos. js reais) por meio de ScriptResource. axd, contanto que os nomes de arquivo sigam um esquema de nomenclatura específico. Com esse suporte, as extensões AJAX do ASP.NET simplificam a localização de scripts e a globalização de aplicativos.

## <a name="bio"></a>*Biografia*

Scott Cate tem trabalhado com tecnologias Web da Microsoft desde 1997 e é presidente da myKB.com ([www.myKB.com](http://www.myKB.com)), onde é especialista em escrever aplicativos baseados em ASP.net voltados para as soluções de software da base de dados de conhecimento. Scott pode ser contatado por email em [scott.cate@myKB.com](mailto:scott.cate@myKB.com) ou em seu blog em [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Próximo](understanding-asp-net-ajax-web-services.md)
