---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Implantação da Web do ASP.NET usando o Visual Studio: transformações de arquivo Web. config | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros, por usin...
ms.author: riande
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: a9d39547c94a63003442ba6fe1257693dde24b05
ms.sourcegitcommit: e7e91932a6e91a63e2e46417626f39d6b244a3ab
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2020
ms.locfileid: "78632828"
---
# <a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implantação da Web do ASP.NET usando o Visual Studio: transformações de arquivo Web. config

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta série de tutoriais mostra como implantar (publicar) um aplicativo Web ASP.NET em aplicativos Web do serviço Azure App ou em um provedor de Hospedagem de terceiros usando o Visual Studio 2012 ou o Visual Studio 2010. Para obter informações sobre a série, consulte [o primeiro tutorial da série](introduction.md).

## <a name="overview"></a>Visão geral

Este tutorial mostra como automatizar o processo de alteração do arquivo *Web. config* ao implantá-lo em ambientes de destino diferentes. A maioria dos aplicativos tem configurações no arquivo *Web. config* que devem ser diferentes quando o aplicativo é implantado. Automatizar o processo de fazer essas alterações evita que você precise fazê-las manualmente toda vez que implantar, o que seria entediante e propenso a erros.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config versus parâmetros de Implantação da Web

Há duas maneiras de automatizar o processo de alteração das configurações do arquivo *Web. config* : [transformações de Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx). Um arquivo de transformação *Web. config* contém marcação XML que especifica como alterar o arquivo *Web. config* quando ele é implantado. Você pode especificar diferentes alterações para configurações de compilação específicas e para perfis de publicação específicos. As configurações de compilação padrão são Debug e Release, e você pode criar configurações de compilação personalizadas. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre os perfis de publicação no tutorial [implantando no IIS como um ambiente de teste](deploying-to-iis.md) .)

Implantação da Web parâmetros podem ser usados para especificar vários tipos diferentes de configurações que devem ser configuradas durante a implantação, incluindo configurações que são encontradas em arquivos *Web. config* . Quando usado para especificar alterações no arquivo *Web. config* , implantação da Web parâmetros são mais complexos de configurar, mas eles são úteis quando você não sabe o valor a ser definido até implantar. Por exemplo, em um ambiente corporativo, você pode criar um *pacote de implantação* e fornecê-lo a uma pessoa no departamento de ti para instalar em produção, e essa pessoa precisa ser capaz de inserir cadeias de conexão ou senhas que você não conhece.

Para o cenário abordado por essa série de tutoriais, você sabe com antecedência tudo o que precisa ser feito no arquivo *Web. config* , portanto, não é necessário usar parâmetros de implantação da Web. Você configurará algumas transformações que diferem dependendo da configuração de compilação usada e algumas que diferem dependendo do perfil de publicação usado.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especificando as configurações do Web. config no Azure

Se as configurações do arquivo *Web. config* que você deseja alterar estiverem no elemento `<connectionStrings>` ou `<appSettings>` e se estiver implantando em aplicativos Web no serviço Azure app, você terá outra opção para automatizar as alterações durante a implantação. Você pode inserir as configurações que deseja entrar em vigor no Azure na guia **Configurar** da página do portal de gerenciamento para seu aplicativo Web (role para baixo até as seções **configurações do aplicativo** e **cadeias de conexão** ). Quando você implanta o projeto, o Azure aplica as alterações automaticamente. Para obter mais informações, consulte [sites do Windows Azure: como as cadeias de caracteres de aplicativo e de conexão funcionam](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Arquivos de transformação padrão

No **Gerenciador de soluções**, expanda *Web. config* para ver os arquivos de transformação *Web. Debug. config* e *Web. Release. config* que são criados por padrão para as duas configurações de compilação padrão.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizadas clicando com o botão direito do mouse no arquivo Web. config e escolhendo **Adicionar transformações de configuração** no menu de contexto. Para este tutorial, você não precisa fazer isso e a opção de menu está desabilitada, pois você não criou nenhuma configuração de compilação personalizada.

Posteriormente, você criará mais três arquivos de transformação, um para o teste, preparo e perfis de publicação de produção. Um exemplo típico de uma configuração que você trataria em um arquivo de transformação de perfil de publicação porque ele depende do ambiente de destino é um ponto de extremidade do WCF que é diferente para teste versus produção. Você criará arquivos de transformação de perfil de publicação em Tutoriais posteriores depois de criar os perfis de publicação com os quais eles vão.

## <a name="disable-debug-mode"></a>Desabilitar modo de depuração

Um exemplo de uma configuração que depende da configuração de Build em vez do ambiente de destino é o atributo `debug`. Para uma compilação de versão, normalmente você deseja que a depuração seja desabilitada, independentemente de qual ambiente você está implantando. Portanto, por padrão, os modelos de projeto do Visual Studio criam arquivos de transformação *Web. Release. config* com código que remove o atributo `debug` do elemento `compilation`. Aqui está o *Web. Release. config*padrão: além de algum código de transformação de exemplo que é comentado, ele inclui código no elemento `compilation` que remove o atributo `debug`:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

O atributo `xdt:Transform="RemoveAttributes(debug)"` especifica que você deseja que o atributo `debug` seja removido do elemento `system.web/compilation` no arquivo *Web. config* implantado. Isso será feito toda vez que você implantar uma compilação de versão.

## <a name="limit-error-log-access-to-administrators"></a>Limitar o acesso ao log de erros aos administradores

Se houver um erro enquanto o aplicativo for executado, o aplicativo exibirá uma página de erro genérica no lugar da página de erro gerada pelo sistema e usará o [pacote NuGet do ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para registro em log de erros e relatórios. O elemento `customErrors` no arquivo *Web. config* do aplicativo especifica a página de erro:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver a página de erro, altere temporariamente o atributo `mode` do elemento `customErrors` de "RemoteOnly" para "on" e execute o aplicativo no Visual Studio. Causa um erro solicitando uma URL inválida, como *Studentsxxx. aspx*. Em vez de uma página de erro "o recurso não pode ser encontrado" gerado pelo IIS, você verá a página *GenericErrorPage. aspx* .

![Página de erro](web-config-transformations/_static/image2.png)

Para ver o log de erros, substitua tudo na URL após o número da porta com o *ELMAH. axd* (por exemplo, `http://localhost:51130/elmah.axd`) e pressione ENTER:

![Página do ELMAH](web-config-transformations/_static/image3.png)

Não se esqueça de definir o elemento `customErrors` de volta para o modo "RemoteOnly" quando terminar.

No seu computador de desenvolvimento, é conveniente permitir acesso gratuito à página de log de erros, mas em produção que seria um risco de segurança. Para o site de produção, você deseja adicionar uma regra de autorização que restringe o acesso ao log de erros aos administradores e para certificar-se de que a restrição funcionará em teste e preparo também. Portanto, essa é outra alteração que você deseja implementar toda vez que implantar uma compilação de versão e, portanto, ela pertence ao arquivo *Web. Release. config* .

Abra *Web. Release. config* e adicione um novo elemento `location` imediatamente antes da marca `configuration` de fechamento, conforme mostrado aqui.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

O valor de atributo `Transform` de "Insert" faz com que esse elemento `location` seja adicionado como um irmão a qualquer elemento de `location` existente no arquivo *Web. config* . (Já existe um elemento `location` que especifica regras de autorização para a página de **créditos de atualização** .)

Agora você pode visualizar a transformação para certificar-se de que você o códigou corretamente.

Em **Gerenciador de soluções**, clique com o botão direito do mouse em *Web. Release. config* e clique em **Visualizar transformação**.

![Menu de transformação de visualização](web-config-transformations/_static/image4.png)

Será aberta uma página que mostra o arquivo *Web. config* de desenvolvimento à esquerda e a aparência do arquivo *Web. config* implantado à direita, com as alterações realçadas.

![Visualização da transformação de depuração](web-config-transformations/_static/image5.png)

![Visualização da transformação de local](web-config-transformations/_static/image6.png)

(Na visualização, você pode observar algumas alterações adicionais para as quais você não escreveu as transformações: elas normalmente envolvem a remoção de espaço em branco que não afeta a funcionalidade.)

Ao testar o site após a implantação, você também testará para verificar se a regra de autorização está em vigor.

> [!NOTE] 
> 
> **Observação de segurança** Nunca exiba detalhes do erro para o público em um aplicativo de produção ou armazene essas informações em um local público. Os invasores podem usar informações de erro para descobrir vulnerabilidades em um site. Se você usar o ELMAH em seu próprio aplicativo, configure o ELMAH para minimizar os riscos de segurança. O exemplo do ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta na qual o aplicativo deve ser capaz de criar arquivos. Para obter mais informações, consulte [Securing The ELMAH Endpoint](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).

## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Uma configuração que você tratará em publicar arquivos de transformação de perfil

Um cenário comum é ter configurações de arquivo *Web. config* que devem ser diferentes em cada ambiente implantado no. Por exemplo, um aplicativo que chama um serviço WCF pode precisar de um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University também inclui uma configuração desse tipo. Essa configuração controla um indicador visível nas páginas de um site que informa em qual ambiente você está, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(dev)" ou "(teste)" ao título principal na página mestra do *site.* Master:

![Indicador de ambiente](web-config-transformations/_static/image7.png)

O indicador de ambiente é omitido quando o aplicativo está sendo executado em preparo ou produção.

As páginas da Web da Contoso University lêem um valor definido em `appSettings` no arquivo *Web. config* para determinar em qual ambiente o aplicativo está sendo executado:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

O valor deve ser "Test" no ambiente de teste e "Prod" para preparo e produção.

O código a seguir em um arquivo de transformação implementará essa transformação:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

O valor do atributo `xdt:Transform` "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no arquivo *Web. config* . O valor do atributo de `xdt:Locator` "match (Key)" indica que o elemento a ser modificado é aquele cujo atributo `key` corresponde ao atributo `key` especificado aqui. O único atributo do elemento `add` é `value`, e isso é o que será alterado no arquivo *Web. config* implantado. O código mostrado aqui faz com que o atributo `value` do elemento `Environment` `appSettings` seja definido como "Test" no arquivo *Web. config* implantado.

Essa transformação pertence aos arquivos de transformação de perfil de publicação, que você ainda não criou. Você criará e atualizará os arquivos de transformação que implementam essa alteração ao criar os perfis de publicação para os ambientes de teste, de preparo e de produção. Você fará isso nos tutoriais [implantar no IIS](deploying-to-iis.md) e [implantar em produção](deploying-to-production.md) .

> [!NOTE]
> Como essa configuração está no elemento `<appSettings>`, você tem outra alternativa para especificar a transformação quando estiver implantando em aplicativos Web no serviço Azure App consulte [especificando as configurações do Web. config no Azure](#watransforms) , anteriormente neste tópico.

## <a name="setting-connection-strings"></a>Configurar cadeias de conexão

Embora o arquivo de transformação padrão contenha um exemplo que mostre como atualizar uma cadeia de conexão, na maioria dos casos, não é necessário configurar as transformações de cadeia de conexão, pois você pode especificar cadeias de conexão no perfil de publicação. Você fará isso nos tutoriais [implantar no IIS](deploying-to-iis.md) e [implantar em produção](deploying-to-production.md) .

## <a name="summary"></a>Resumo

Agora você tem feito o máximo possível com as transformações de *Web. config* antes de criar os perfis de publicação, e já viu uma visualização do que estará no arquivo Web. config implantado.

![Visualização da transformação de local](web-config-transformations/_static/image8.png)

No tutorial a seguir, você cuidará das tarefas de configuração de implantação que exigem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte [usando as transformações de Web. config para alterar as configurações no arquivo Web. config de destino ou no arquivo app. config durante a implantação](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) no mapa de conteúdo da implantação da Web para Visual Studio e ASP.net.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Próximo](project-properties.md)
