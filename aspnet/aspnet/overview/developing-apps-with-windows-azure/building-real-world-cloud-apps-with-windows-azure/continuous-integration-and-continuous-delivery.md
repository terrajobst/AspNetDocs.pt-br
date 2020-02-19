---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integração contínua e entrega contínua (criando aplicativos de nuvem do mundo real com o Azure) | Microsoft Docs
author: MikeWasson
description: A criação de aplicativos de nuvem do mundo real com o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: cf3c65ef95528173eed3fb08984035b2512861c4
ms.sourcegitcommit: 7709c0a091b8d55b7b33bad8849f7b66b23c3d72
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77457031"
---
# <a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integração contínua e entrega contínua (criando aplicativos de nuvem do mundo real com o Azure)

por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://twitter.com/RickAndMSFT), [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto de ti](https://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) ou [baixar o livro eletrônico](https://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> A **criação de aplicativos de nuvem do mundo real com** o livro eletrônico do Azure é baseada em uma apresentação desenvolvida por Scott Guthrie. Ele explica 13 padrões e práticas que podem ajudá-lo a desenvolver com sucesso aplicativos Web para a nuvem. Para obter informações sobre o livro eletrônico, consulte [o primeiro capítulo](introduction.md).

Os dois primeiros padrões de processo de desenvolvimento recomendados foram [automatizar tudo](automate-everything.md) e o [controle do código-fonte](source-control.md), e o terceiro padrão de processo os combina. A CI (integração contínua) significa que sempre que um desenvolvedor fizer check-in no código para o repositório de origem, uma compilação será disparada automaticamente. A entrega contínua (CD) leva isso um pouco mais: depois que um Build e testes de unidade automatizados forem bem-sucedidos, você implantará automaticamente o aplicativo em um ambiente no qual poderá fazer testes mais detalhados.

A nuvem permite minimizar o custo de manutenção de um ambiente de teste, pois você paga apenas pelos recursos do ambiente, desde que esteja usando-os. O processo de CD pode configurar o ambiente de teste quando necessário, e você pode desligar o ambiente quando terminar o teste.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Fluxo de trabalho de integração contínua e entrega contínua

Em geral, recomendamos que você faça entrega contínua para seus ambientes de desenvolvimento e preparo. A maioria das equipes, mesmo na Microsoft, requer um processo manual de revisão e aprovação para a implantação de produção. Para uma implantação de produção, talvez você queira ter certeza de que ela acontece quando as principais pessoas da equipe de desenvolvimento estão disponíveis para suporte ou durante períodos de baixo tráfego. Mas não há nada para impedir que você automatize completamente seus ambientes de desenvolvimento e teste para que tudo o que um desenvolvedor precise fazer seja fazer o check-in de uma alteração e um ambiente esteja configurado para teste de aceitação.

O diagrama a seguir de [um livro eletrônico de padrões e práticas da Microsoft sobre a entrega contínua](https://aka.ms/ReleasePipeline) ilustra um fluxo de trabalho típico. Clique na imagem para vê-la em tamanho total em seu contexto original.

[fluxo de trabalho de entrega contínua ![](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Como a nuvem permite CI e CD econômicos

Automatizar esses processos no Azure é fácil. Como você está executando tudo na nuvem, não precisa comprar ou gerenciar servidores para suas compilações ou seus ambientes de teste. E você não precisa esperar que um servidor esteja disponível para fazer seu teste no. Com cada compilação que você faz, você poderia criar um ambiente de teste no Azure usando seu script de automação, executar testes de aceitação ou mais testes detalhados em relação a ele e, em seguida, quando terminar, bastará subdividi-lo. E se você só executar esse servidor por 2 horas ou 8 horas ou por dia, a quantidade de dinheiro que você precisa pagar por ele é mínima, pois você está pagando apenas pelo tempo em que um computador está realmente em execução. Por exemplo, o ambiente necessário para o aplicativo corrigir ti basicamente custa cerca de 1 cento por hora se você passar uma camada para cima do nível gratuito. Ao longo de um mês, se você só executou o ambiente uma hora por vez, seu ambiente de teste provavelmente custaria menos do que um expresso que você comprar em Starbucks.

## <a name="azure-devops-services"></a>Azure DevOps Services 

O Azure DevOps Services fornece vários recursos para ajudá-lo com o desenvolvimento de aplicativos do planejamento à implantação.

- Ele dá suporte ao controle de origem git (distribuído) e TFVC (centralizado).
- Ele oferece um serviço de Build elástico, o que significa que ele cria servidores de compilação dinamicamente quando eles são necessários e os retira quando eles são concluídos. Você pode disparar automaticamente uma compilação quando alguém verificar as alterações no código-fonte e não precisar alocar e pagar por seus próprios servidores de compilação que estejam ociosos na maior parte do tempo. O serviço de compilação é gratuito contanto que você não exceda um determinado número de compilações. Se você pretende fazer um grande volume de compilações, você pode pagar um pouco mais para servidores de Build reservados.
- Ele dá suporte à entrega contínua para o Azure.
- Ele dá suporte ao teste de carga automatizado. O teste de carga é essencial para um aplicativo de nuvem, mas geralmente fica inativo até que seja tarde demais. O teste de carga simula o uso intensivo de um aplicativo por milhares de usuários, permitindo que você encontre gargalos e melhore a taxa de transferência — antes de liberar o aplicativo para produção.
- Ele dá suporte à colaboração da sala de equipe, o que facilita a comunicação e a colaboração em tempo real para pequenas equipes Agile.
- Ele dá suporte ao gerenciamento de projeto ágil.

Para obter mais informações sobre os recursos de integração e entrega contínua do Azure DevOps Services, consulte [a documentação do Azure DevOps](/azure/devops/index).

Se você estiver procurando uma solução de gerenciamento de projeto, de colaboração de equipe e de controle de código-chave, confira Azure DevOps Services. Inscreva-se em [Azure DevOps Services](https://dev.azure.com/).

## <a name="summary"></a>Resumo

Os três primeiros padrões de desenvolvimento em nuvem foram sobre como implementar um processo de desenvolvimento reproduzível, confiável e previsível, com tempo de ciclo baixo. No [próximo capítulo](web-development-best-practices.md) , começamos a examinar os padrões de arquitetura e de codificação.

## <a name="resources"></a>Recursos

Para obter mais informações, consulte [implantar um aplicativo Web no serviço Azure app](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consulte também os seguintes recursos:

- [Criando um pipeline de versão com o Team Foundation Server 2012](https://aka.ms/ReleasePipeline). O livro eletrônico, laboratórios práticos e exemplos de código por meio de padrões e práticas da Microsoft, fornece uma introdução aprofundada à entrega contínua. Aborda o uso do Lab Management do Visual Studio e do Visual Studio Release Management.
- [Ferramentas e diretrizes do ALM Rangers DevOps](https://aka.ms/vsarsolutions/). O ALM Rangers introduziu a solução complementar de exemplo do DevOps Workbench e a orientação prática em colaboração com os padrões &amp; livro de práticas *criando um pipeline de lançamento com o tfs 2012*, como uma ótima maneira de começar a aprender os conceitos do DevOps &amp; Release Management para o TFS 2012 e para iniciar os pneus. A orientação mostra como compilar uma vez e implantar em vários ambientes.
- [Teste para entrega contínua com o Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). O livro eletrônico de práticas e padrões da Microsoft explica como integrar testes automatizados com entrega contínua.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código-fonte para uma ferramenta projetada para capturar uma compilação do TFS (com base em um rótulo), compilá-la, empacotá-la, permitir que alguém na função DevOps configure aspectos específicos dela e envie-a por push para o Azure. A ferramenta rastreia o processo de implantação a fim de habilitar operações para "reverter" para uma versão implantada anteriormente. A ferramenta não tem dependências externas e pode funcionar de forma autônoma usando APIs do TFS e o SDK do Azure.
- [Entrega contínua: versões de software confiáveis por meio da automação de compilação, teste e implantação](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Livro por Jez modesta.
- [Versão! projete e implante o software pronto para produção](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Livro por Michael T. Nygard.

> [!div class="step-by-step"]
> [Anterior](source-control.md)
> [Próximo](web-development-best-practices.md)
