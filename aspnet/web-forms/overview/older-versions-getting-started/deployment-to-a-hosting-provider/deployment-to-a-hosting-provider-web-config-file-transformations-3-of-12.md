---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: transformações de arquivo Web. config-3 de 12 | Microsoft Docs'
author: tdykstra
description: Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: fe71e6cfb0f4c5f1d99b326e9d90edb6c8c5feee
ms.sourcegitcommit: 22fbd8863672c4ad6693b8388ad5c8e753fb41a2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74600517"
---
# <a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Implantando um aplicativo Web ASP.NET com SQL Server Compact usando o Visual Studio ou o Visual Web Developer: transformações de arquivo Web. config-3 de 12

por [Tom Dykstra](https://github.com/tdykstra)

[Baixar o projeto inicial](https://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta série de tutoriais mostra como implantar (publicar) um projeto de aplicativo Web ASP.NET que inclui um banco de dados SQL Server Compact usando o Visual Studio 2012 RC ou o Visual Studio Express 2012 RC para Web. Você também pode usar o Visual Studio 2010 se instalar a atualização de publicação na Web. Para obter uma introdução à série, consulte [o primeiro tutorial da série](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver um tutorial que mostra os recursos de implantação introduzidos após a versão RC do Visual Studio 2012, mostra como implantar SQL Server edições diferentes de SQL Server Compact e mostra como implantar o Azure App aplicativos Web do serviço, consulte [implantação da Web do ASP.NET usando o Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).

## <a name="overview"></a>{1&gt;Visão Geral&lt;1}

Este tutorial mostra como automatizar o processo de alteração do arquivo *Web. config* ao implantá-lo em ambientes de destino diferentes. A maioria dos aplicativos tem configurações no arquivo *Web. config* que devem ser diferentes quando o aplicativo é implantado. Automatizar o processo de fazer essas alterações evita que você precise fazê-las manualmente toda vez que implantar, o que seria entediante e propenso a erros.

Lembrete: se você receber uma mensagem de erro ou algo não funcionar enquanto percorre o tutorial, certifique-se de verificar a [página de solução de problemas](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformações de Web. config versus parâmetros de Implantação da Web

Há duas maneiras de automatizar o processo de alteração das configurações do arquivo *Web. config* : [transformações de Web. config](https://msdn.microsoft.com/library/dd465326.aspx) e [parâmetros de implantação da Web](https://msdn.microsoft.com/library/ff398068.aspx). Um arquivo de transformação *Web. config* contém marcação XML que especifica como alterar o arquivo *Web. config* quando ele é implantado. Você pode especificar diferentes alterações para configurações de compilação específicas e para perfis de publicação específicos. As configurações de compilação padrão são Debug e Release, e você pode criar configurações de compilação personalizadas. Um perfil de publicação geralmente corresponde a um ambiente de destino. (Você aprenderá mais sobre os perfis de publicação no tutorial [implantando no IIS como um ambiente de teste](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) .)

Implantação da Web parâmetros podem ser usados para especificar vários tipos diferentes de configurações que devem ser configuradas durante a implantação, incluindo configurações que são encontradas em arquivos *Web. config* . Quando usado para especificar alterações no arquivo *Web. config* , implantação da Web parâmetros são mais complexos de configurar, mas eles são úteis quando você não sabe o valor a ser definido até implantar. Por exemplo, em um ambiente corporativo, você pode criar um *pacote de implantação* e fornecê-lo a uma pessoa no departamento de ti para instalar em produção, e essa pessoa precisa ser capaz de inserir cadeias de conexão ou senhas que você não conhece.

Para o cenário que este tutorial aborda, você sabe tudo o que precisa ser feito no arquivo *Web. config* , portanto, não é necessário usar parâmetros de implantação da Web. Você configurará algumas transformações que diferem dependendo da configuração de compilação usada e algumas que diferem dependendo do perfil de publicação usado.

## <a name="creating-transformation-files-for-publish-profiles"></a>Criando arquivos de transformação para perfis de publicação

No **Gerenciador de soluções**, expanda *Web. config* para ver os arquivos de transformação *Web. Debug. config* e *Web. Release. config* que são criados por padrão para as duas configurações de compilação padrão.

![Web. config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Você pode criar arquivos de transformação para configurações de compilação personalizadas clicando com o botão direito do mouse no arquivo Web. config e escolhendo **Adicionar transformações de configuração** no menu de contexto, mas para este tutorial, você não precisa fazer isso.

Você precisa de mais dois arquivos de transformação para configurar as alterações relacionadas ao destino da implantação em vez de à configuração da compilação. Um exemplo típico desse tipo de configuração é um ponto de extremidade do WCF que é diferente para teste versus produção. Em Tutoriais posteriores, você criará perfis de publicação chamados teste e produção, portanto, você precisa de um arquivo *Web. Test. config* e um arquivo *Web. Production. config* .

Os arquivos de transformação que estão vinculados a perfis de publicação devem ser criados manualmente. Em **Gerenciador de soluções**, clique com o botão direito do mouse no projeto ContosoUniversity e selecione **abrir pasta no Windows Explorer**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

No **Windows Explorer**, selecione o arquivo *Web. Release. config* , copie o arquivo e, em seguida, Cole duas cópias dele. Renomeie essas cópias *Web. Production. config* e *Web. Test. config*e, em seguida, feche o **Windows Explorer**.

Em **Gerenciador de soluções**, clique em **Atualizar** para ver os novos arquivos.

Selecione os novos arquivos, clique com o botão direito do mouse e, em seguida, clique em **incluir no projeto** no menu de contexto.

![Incluindo arquivos de configuração de teste e de produção no projeto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Para impedir que esses arquivos sejam implantados, selecione-os em **Gerenciador de soluções**e, na janela **Propriedades** , altere a propriedade **ação de compilação** de **conteúdo** para **nenhum**. (Os arquivos de transformação baseados em configurações de compilação são impedidos automaticamente de serem implantados.)

Agora você está pronto para inserir as transformações de *Web. config* nos arquivos de transformação *Web. config* .

## <a name="limiting-error-log-access-to-administrators"></a>Limitando o acesso ao log de erros aos administradores

Se houver um erro enquanto o aplicativo for executado, o aplicativo exibirá uma página de erro genérica no lugar da página de erro gerada pelo sistema e usará o [pacote NuGet do ELMAH](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) para registro em log de erros e relatórios. O elemento `customErrors` no arquivo *Web. config* especifica a página de erro:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Para ver a página de erro, altere temporariamente o atributo `mode` do elemento `customErrors` de "RemoteOnly" para "on" e execute o aplicativo no Visual Studio. Causa um erro solicitando uma URL inválida, como *Studentsxxx. aspx*. Em vez de uma página de erro "página não encontrada" gerada pelo IIS, você verá a página *GenericErrorPage. aspx* .

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Para ver o log de erros, substitua tudo na URL após o número da porta com o *ELMAH. axd* (para obter o exemplo na captura de tela, `http://localhost:51130/elmah.axd`) e pressione ENTER:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

Não se esqueça de definir o elemento `customErrors` de volta para o modo "RemoteOnly" quando terminar.

No seu computador de desenvolvimento, é conveniente permitir acesso gratuito à página de log de erros, mas em produção que seria um risco de segurança. Para o site de produção, você pode adicionar uma regra de autorização que restringe o acesso ao log de erros apenas aos administradores Configurando uma transformação no arquivo *Web. Production. config* .

Abra *Web. Production. config* e adicione um novo elemento `location` imediatamente após a marca de `configuration` de abertura, conforme mostrado aqui. (Certifique-se de adicionar apenas o elemento `location` e não a marcação ao redor, que é mostrada aqui apenas para fornecer algum contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

O valor de atributo `Transform` de "Insert" faz com que esse elemento `location` seja adicionado como um irmão a qualquer elemento de `location` existente no arquivo *Web. config* . (Já existe um elemento `location` que especifica regras de autorização para a página de **créditos de atualização** .) Ao testar o site de produção após a implantação, você testará para verificar se essa regra de autorização está em vigor.

Você não precisa restringir o acesso ao log de erros no ambiente de teste, de modo que você não precisa adicionar esse código ao arquivo *Web. Test. config* .

> [!NOTE] 
> 
> **Observação de segurança** Nunca exiba detalhes do erro para o público em um aplicativo de produção ou armazene essas informações em um local público. Os invasores podem usar informações de erro para descobrir vulnerabilidades em um site. Se você usar o ELMAH em seu próprio aplicativo, certifique-se de investigar maneiras em que o ELMAH pode ser configurado para minimizar os riscos de segurança. O exemplo do ELMAH neste tutorial não deve ser considerado uma configuração recomendada. É um exemplo que foi escolhido para ilustrar como lidar com uma pasta na qual o aplicativo deve ser capaz de criar arquivos.

## <a name="setting-an-environment-indicator"></a>Configurando um indicador de ambiente

Um cenário comum é ter configurações de arquivo *Web. config* que devem ser diferentes em cada ambiente implantado no. Por exemplo, um aplicativo que chama um serviço WCF pode precisar de um ponto de extremidade diferente em ambientes de teste e produção. O aplicativo Contoso University também inclui uma configuração desse tipo. Essa configuração controla um indicador visível nas páginas de um site que informa em qual ambiente você está, como desenvolvimento, teste ou produção. O valor da configuração determina se o aplicativo acrescentará "(dev)" ou "(teste)" ao título principal na página mestra do *site.* Master:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

O indicador de ambiente é omitido quando o aplicativo está sendo executado em produção.

As páginas da Web da Contoso University lêem um valor definido em `appSettings` no arquivo *Web. config* para determinar em qual ambiente o aplicativo está sendo executado:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

O valor deve ser "teste" no ambiente de teste e "Prod" no ambiente de produção.

Abra *Web. Production. config* e adicione um elemento `appSettings` imediatamente antes da marca de abertura do elemento `location` que você adicionou anteriormente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

O valor do atributo `xdt:Transform` "SetAttributes" indica que a finalidade dessa transformação é alterar os valores de atributo de um elemento existente no arquivo *Web. config* . O valor do atributo de `xdt:Locator` "match (Key)" indica que o elemento a ser modificado é aquele cujo atributo `key` corresponde ao atributo `key` especificado aqui. O único atributo do elemento `add` é `value`, e isso é o que será alterado no arquivo *Web. config* implantado. Esse código faz com que o atributo `value` do elemento `Environment` `appSettings` seja definido como "Prod" no arquivo *Web. config* que é implantado para produção.

Em seguida, aplique a mesma alteração ao arquivo *Web. Test. config* , exceto defina o `value` como "Test" em vez de "Prod". Quando terminar, a seção `appSettings` em *Web. Test. config* será parecida com o exemplo a seguir:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Desabilitando o modo de depuração

Para uma compilação de versão, você não deseja que a depuração seja habilitada, independentemente de qual ambiente você está implantando. Por padrão, o arquivo de transformação *Web. Release. config* é criado automaticamente com o código que remove o atributo `debug` do elemento `compilation`:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

O atributo `Transform` faz com que o atributo `debug` seja omitido do arquivo *Web. config* implantado sempre que você implantar uma compilação de versão.

Essa mesma transformação está em arquivos de transformação de teste e de produção, pois você os criou copiando o arquivo de transformação de liberação. Você não precisa dele duplicado, portanto, abra cada um desses arquivos, remova o elemento **Compilation** e salve e feche cada arquivo.

## <a name="setting-connection-strings"></a>Definindo cadeias de conexão

Na maioria dos casos, não é necessário configurar as transformações de cadeia de conexão, pois você pode especificar cadeias de conexão no perfil de publicação. Mas há uma exceção quando você está implantando um banco de dados SQL Server Compact e está usando Migrações do Entity Framework Code First para atualizar o banco de dados no servidor de destino. Para este cenário, você precisa especificar uma cadeia de conexão adicional que será usada no servidor para atualizar o esquema de banco de dados. Para configurar essa transformação, adicione um **&lt;connectionstrings&gt;** elemento imediatamente após a abertura&lt;marca de **configuração&gt;** nos arquivos de transformação *Web. Test. config* e *Web. Production. config* :

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

O atributo `Transform` especifica que essa cadeia de conexão será adicionada ao elemento *connectionStrings* no arquivo *Web. config* implantado. (O processo de publicação cria essa cadeia de conexão adicional automaticamente para você se ela não existir, mas por padrão o atributo **ProviderName** é definido como `System.Data.SqlClient`, o que não funciona para SQL Server Compact. Ao adicionar a cadeia de conexão manualmente, você mantém o processo de implantação criando um elemento de cadeia de conexão com o nome do provedor errado.)

Agora você especificou todas as transformações de *Web. config* necessárias para implantar o aplicativo da Contoso University para teste e produção. No tutorial a seguir, você cuidará das tarefas de configuração de implantação que exigem a definição de propriedades do projeto.

## <a name="more-information"></a>Mais Informações

Para obter mais informações sobre os tópicos abordados por este tutorial, consulte o cenário de transformação Web. config no [mapa de conteúdo de implantação do ASP.net](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Próximo](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
