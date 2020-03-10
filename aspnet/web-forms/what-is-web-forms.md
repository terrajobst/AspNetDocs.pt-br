---
uid: web-forms/what-is-web-forms
title: O que é Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: riande
ms.date: 02/21/2014
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 19be419c499759713971a6c77674c924867d1bbc
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78636573"
---
# <a name="what-is-web-forms"></a>O que é Web Forms

ASP.NET Web Forms faz parte da estrutura de aplicativos Web do ASP.NET e está incluída no [Visual Studio](https://www.asp.net/downloads). É um dos quatro modelos de programação que você pode usar para criar aplicativos Web ASP.NET, os outros são ASP.NET MVC, Páginas da Web do ASP.NET e ASP.NET aplicativos de página única.

Web Forms são páginas que os usuários solicitam usando seu navegador. Essas páginas podem ser escritas usando uma combinação de HTML, script de cliente, controles de servidor e código de servidor. Quando os usuários solicitam uma página, eles são compilados e executados no servidor pela estrutura e, em seguida, o Framework gera a marcação HTML que o navegador pode renderizar. Uma página de Web Forms ASP.NET apresenta informações para o usuário em qualquer navegador ou dispositivo cliente.

Usando o Visual Studio, você pode criar ASP.NET Web Forms. O IDE (ambiente de desenvolvimento integrado) do Visual Studio permite arrastar e soltar controles de servidor para definir o layout de sua página de Web Forms. Você pode facilmente definir propriedades, métodos e eventos para controles na página ou para a própria página. Essas propriedades, métodos e eventos são usados para definir o comportamento, a aparência e a sensação da página da Web e assim por diante. Para gravar o código do servidor para lidar com a lógica da página, você pode usar uma linguagem .NET como C#Visual Basic ou.

> [!NOTE] 
> 
> A documentação do ASP.NET e do Visual Studio abrange várias versões. Tópicos que realçam recursos de versões anteriores podem ser úteis para suas tarefas atuais e cenários usando as versões mais recentes.

**Web Forms ASP.NET são:** 

- Com base na tecnologia Microsoft ASP.NET, em que o código executado no servidor gera dinamicamente a saída da página da Web para o dispositivo do navegador ou do cliente.
- Compatível com qualquer navegador ou dispositivo móvel. Uma página da Web do ASP.NET automaticamente renderiza o HTML correto em conformidade com o navegador para recursos como estilos, layout e assim por diante.
- Compatível com qualquer linguagem com suporte do Common Language Runtime .NET, como Microsoft Visual Basic e Microsoft Visual C#.
- Criado com base na estrutura de Microsoft .NET. Isso fornece todos os benefícios da estrutura, incluindo um ambiente gerenciado, segurança de tipo e herança.
- Flexível porque você pode adicionar controles de terceiros e criados pelo usuário a eles.

**Oferta de Web Forms ASP.NET:** 

- Separação de HTML e outro código de interface do usuário da lógica do aplicativo.
- Um pacote avançado de controles de servidor para tarefas comuns, incluindo acesso a dados.
- Ligação de dados avançada, com excelente suporte a ferramentas.
- Suporte para scripts do lado do cliente que é executado no navegador.
- Suporte para uma variedade de outros recursos, incluindo roteamento, segurança, desempenho, internacionalização, teste, depuração, tratamento de erros e gerenciamento de estado.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>ASP.NET Web Forms ajuda você a superar os desafios

A programação de aplicativos Web apresenta desafios que normalmente não surgem durante a programação de aplicativos tradicionais baseados em cliente. Entre os desafios estão:

- **Implementando uma interface do usuário Web rica** , pode ser difícil e entediante criar e implementar uma interface do usuário usando instalações básicas de HTML, especialmente se a página tiver um layout complexo, uma grande quantidade de conteúdo dinâmico e objetos totalmente interativos do usuário.
- **Separação de cliente e servidor** – em um aplicativo Web, o cliente (navegador) e o servidor são programas diferentes geralmente em execução em computadores diferentes (e até mesmo em sistemas operacionais diferentes). Consequentemente, as duas metades do aplicativo compartilham muito pouca informação; Eles podem se comunicar, mas normalmente trocam apenas pequenas partes de informações simples.
- **Execução sem estado** – quando um servidor Web recebe uma solicitação de uma página, ele localiza a página, processa-a, envia-a para o navegador e, em seguida, descarta todas as informações da página. Se o usuário solicitar a mesma página novamente, o servidor repetirá a sequência inteira, reprocessando a página do zero. Em outras palavras, um servidor não tem memória de páginas processadas — a página não tem estado. Portanto, se um aplicativo precisar manter informações sobre uma página, sua natureza sem estado poderá se tornar um problema.
- **Recursos de cliente desconhecidos** – em muitos casos, os aplicativos Web podem ser acessados por muitos usuários usando navegadores diferentes. Os navegadores têm recursos diferentes, dificultando a criação de um aplicativo que será executado igualmente bem em todos eles.
- **Complicações com acesso a dados** -a leitura e a gravação de uma fonte de dados em aplicativos Web tradicionais podem ser complicadas e intensivas de recursos.
- **Complicações com escalabilidade** -em muitos casos, os aplicativos da Web criados com métodos existentes falham ao atender às metas de escalabilidade devido à falta de compatibilidade entre os vários componentes do aplicativo. Isso geralmente é um ponto de falha comum para aplicativos sob um ciclo de crescimento pesado.

Atender a esses desafios para aplicativos Web pode exigir tempo e esforço substanciais. ASP.NET Web Forms e a estrutura ASP.NET resolvem esses desafios das seguintes maneiras:

- **Modelo de objeto consistente e intuitivo** – a estrutura da página ASP.net apresenta um modelo de objeto que permite que você considere seus formulários como uma unidade, não como partes de cliente e servidor separadas. Nesse modelo, você pode programar a página de forma mais intuitiva do que em aplicativos Web tradicionais, incluindo a capacidade de definir propriedades para elementos de página e responder a eventos. Além disso, os controles de servidor ASP.NET são uma abstração do conteúdo físico de uma página HTML e da interação direta entre o navegador e o servidor. Em geral, você pode usar controles de servidor da maneira como você pode trabalhar com controles em um aplicativo cliente e não precisa pensar em como criar o HTML para apresentar e processar os controles e seu conteúdo.
- **Modelo de programação orientada a eventos** -ASP.NET Web Forms trazer para aplicativos Web o modelo conhecido de escrever manipuladores de eventos para eventos que ocorrem no cliente ou no servidor. A estrutura da página ASP.NET abstrai esse modelo de forma que o mecanismo subjacente de capturar um evento no cliente, transmitindo-o ao servidor e chamando o método apropriado seja automático e invisível para você. O resultado é uma estrutura de código clara e facilmente escrita que dá suporte ao desenvolvimento controlado por eventos.
- **Gerenciamento de estado intuitivo** -a estrutura de página do ASP.net manipula automaticamente a tarefa de manter o estado de sua página e seus controles e fornece maneiras explícitas de manter o estado das informações específicas do aplicativo. Isso é feito sem uso intensivo de recursos do servidor e pode ser implementado com ou sem enviar cookies para o navegador.
- **Aplicativos independentes de navegador** – a estrutura de página do ASP.net permite que você crie toda a lógica do aplicativo no servidor, eliminando a necessidade de codificar explicitamente as diferenças em navegadores. No entanto, ele ainda permite que você aproveite os recursos específicos do navegador escrevendo código do lado do cliente para fornecer desempenho aprimorado e uma experiência de cliente mais rica.
- **Suporte de Common Language Runtime .NET Framework** -a estrutura de página ASP.net é criada no .NET Framework, portanto, toda a estrutura está disponível para qualquer aplicativo ASP.net. Seus aplicativos podem ser escritos em qualquer linguagem compatível com o tempo de execução. Além disso, o acesso a dados é simplificado usando a infraestrutura de acesso a dados fornecida pelo .NET Framework, incluindo ADO.NET.
- **.NET Framework desempenho escalonável do servidor** – a estrutura de página do ASP.net permite que você dimensione seu aplicativo Web de um computador com um único processador para um Web farm de vários computadores de forma limpa e sem alterações complicadas na lógica do aplicativo.

## <a name="features-of-aspnet-web-forms"></a>Recursos do ASP.NET Web Forms

- **Controles de servidor**-controles de servidor Web ASP.NET são objetos em páginas da web do ASP.NET que são executadas quando a página é solicitada e que processam a marcação no navegador. Muitos controles de servidor Web são semelhantes aos elementos HTML familiares, como botões e caixas de texto. Outros controles abrangem um comportamento complexo, como controles de calendário e controles que você pode usar para se conectar a fontes de dados e exibir dados.
- **Páginas mestras**– as páginas mestras ASP.NET permitem que você crie um layout consistente para as páginas em seu aplicativo. Uma única página mestra define a aparência e comportamento padrão que você deseja para todas as páginas (ou um grupo de páginas) em seu aplicativo. Então, você pode criar páginas de conteúdo individuais com o conteúdo que você deseja exibir. Quando os usuários solicitam as páginas de conteúdo, eles mesclam com a página mestra para produzir saída que combina o layout da página mestra com o conteúdo da página de conteúdo.
- **Trabalhar com data**-ASP.NET fornece muitas opções para armazenar, recuperar e exibir dados. Em um aplicativo ASP.NET Web Forms, você usa controles vinculados a dados para automatizar a apresentação ou a entrada de dados em elementos da interface do usuário da página da Web, como tabelas e caixas de texto e listas suspensas.
- A **Associação**-ASP.net Identity armazena as credenciais de seus usuários em um banco de dados criado pelo aplicativo. Quando os usuários fizerem logon, o aplicativo validará suas credenciais lendo o banco de dados. A pasta *conta* do seu projeto contém os arquivos que implementam as várias partes de associação: registro, logon, alteração de senha e autorização do acesso. Além disso, o ASP.NET Web Forms dá suporte ao OAuth e ao OpenID. Esses aprimoramentos de autenticação permitem que os usuários façam logon em seu site usando credenciais existentes, a partir de contas como Facebook, Twitter, Windows Live e Google. Por padrão, o modelo cria um banco de dados de associação usando um nome de banco de dados padrão em uma instância do SQL Server Express LocalDB, o servidor de banco de dados de desenvolvimento que vem com Visual Studio Express 2013 para Web.
- **Estruturas de script de cliente e cliente**-você pode aprimorar os recursos baseados em servidor do ASP.net, incluindo a funcionalidade de script de cliente em páginas de formulário da Web do ASP.net. Você pode usar o script de cliente para fornecer uma interface de usuário mais rica e responsiva aos usuários. Você também pode usar o script de cliente para fazer chamadas assíncronas para o servidor Web enquanto uma página está em execução no navegador.
- O roteamento de URL de **Roteamento**permite que você configure um aplicativo para aceitar URLs de solicitação que não são mapeadas para arquivos físicos. Uma URL de solicitação é simplesmente a URL que um usuário insere em seu navegador para localizar uma página no seu site. Você usa o roteamento para definir as URLs que são semanticamente significativas para os usuários e que podem ajudar com a SEO (otimização do mecanismo de pesquisa).
- **Gerenciamento de estado**-o ASP.NET Web Forms inclui várias opções que ajudam a preservar os dados por página e por uma base em todo o aplicativo.
- **Segurança**– uma parte importante do desenvolvimento de um aplicativo mais seguro é entender as ameaças a ele. A Microsoft desenvolveu uma maneira de categorizar ameaças: falsificação, violação, repúdio, divulgação de informações, negação de serviço, elevação de privilégio (STRIDE). No ASP.NET Web Forms, você pode adicionar pontos de extensibilidade e opções de configuração que permitem personalizar vários comportamentos de segurança no ASP.NET Web Forms.
- **Desempenho**-o desempenho pode ser um fator importante em um site ou projeto bem-sucedido. O ASP.NET Web Forms permite que você modifique o desempenho relacionado ao processamento de página e de controle de servidor, gerenciamento de estado, acesso a dados, configuração e carregamento de aplicativos e práticas de codificação eficientes.
- **Internacionalização**-ASP.NET Web Forms permite que você crie páginas da Web que podem obter conteúdo e outros dados com base na configuração de idioma preferencial para o navegador ou com base na escolha explícita do idioma do usuário. O conteúdo e outros dados são chamados de recursos e esses dados podem ser armazenados em arquivos de recursos ou outras fontes. Em uma página Web Forms do ASP.NET, você configura controles para obter seus valores de propriedade dos recursos. Em tempo de execução, as expressões de recurso são substituídas por recursos do arquivo de recurso localizado apropriado.
- **Depuração e tratamento de erro**-o ASP.NET inclui recursos para ajudá-lo a diagnosticar problemas que podem surgir no seu aplicativo Web Forms. A depuração e o tratamento de erros têm suporte no ASP.NET Web Forms para que seus aplicativos sejam compilados e executados com eficiência.
- **Implantação e hospedagem**– o Visual Studio, o ASP.net, o Azure e o IIS fornecem ferramentas que ajudam você com o processo de implantação e hospedagem do seu aplicativo Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidindo quando criar um aplicativo Web Forms

Você deve considerar atentamente se deve implementar um aplicativo Web usando o modelo de Web Forms ASP.NET ou outro modelo, como o ASP.NET MVC Framework. A estrutura MVC não substitui o modelo de Web Forms; você pode usar ambas as estruturas para aplicativos Web. Antes de decidir usar o modelo de Web Forms ou a estrutura MVC para um site específico, avalie as vantagens de cada abordagem.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Vantagens de um aplicativo Web baseado em Web Forms

A estrutura baseada em Web Forms oferece as seguintes vantagens:

- Ela suporta um modelo de evento que preserva o estado sobre HTTP, o que beneficia o desenvolvimento de aplicativos Web de linha de negócios. O aplicativo baseado em Web Forms fornece dezenas de eventos que são suportados em centenas de controles de servidor.
- Ela usa um padrão Page Controller que adiciona funcionalidade em páginas individuais. Para obter mais informações, consulte [controlador de página](https://go.microsoft.com/fwlink/?LinkId=106359 "Controlador de página") no site do MSDN.
- Ele usa o estado de exibição ou formulários baseados em servidor, o que pode facilitar o gerenciamento de informações de estado.
- Ela funciona bem com pequenas equipes de desenvolvedores e Web designers que desejem tirar proveito do grande número de componentes disponíveis para um rápido desenvolvimento do aplicativo.
- Em geral, é menos complexo para o desenvolvimento de aplicativos, pois os componentes (a classe de **página** , os controles e assim por diante) são totalmente integrados e geralmente exigem menos código do que o modelo MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Vantagens de um aplicativo Web baseado em MVC

A estrutura ASP.NET MVC oferece as seguintes vantagens:

- Ela torna mais fácil gerenciar a complexidade ao dividir o aplicativo em modelo, exibição e controlador.
- Ela não utiliza o estado de exibição nem formulários baseados no servidor. Isto torna a estrutura MVC ideal para desenvolvedores que desejam controle completo sobre o comportamento do aplicativo.
- Ela usa um padrão Front Controller que processa as solicitações do aplicativo Web através de um único controlador. Isto permite desenvolver um aplicativo que suporta uma poderosa infraestrutura de roteamento. Para obter mais informações, consulte [front Controller](https://go.microsoft.com/fwlink/?LinkId=106357 "Controlador frontal") no site do MSDN.
- Ela fornece um melhor suporte para desenvolvimento controlado por testes (TDD – test-driven development).
- Ele funciona bem para aplicativos Web que têm suporte em grandes equipes de desenvolvedores e Web designers que precisam de um alto grau de controle sobre o comportamento do aplicativo.
