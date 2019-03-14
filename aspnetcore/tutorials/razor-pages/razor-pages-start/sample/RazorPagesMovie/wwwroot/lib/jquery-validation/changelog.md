---
ms.openlocfilehash: 170d5ec71b0789bc1efb43fbdd4e24eab36a8cc4
ms.sourcegitcommit: 24b1f6decbb17bb22a45166e5fdb0845c65af498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "57037363"
---
<a name="--"></a>--
==================

## <a name="core"></a>Núcleo
  * Remover método removeAttrs não utilizado
  * Substituir a expressão regular para o método de url
  * Remover parâmetro de url inválido em $.ajax, substituído por $.extend
  * Lidar adequadamente com o botão aninhado de cancelar envio
  * Corrigir recuo
  * Refatorar attributeRules e dataRules para compartilhar normalizador
  * Método dataRules para converter o valor em número para entradas de número
  * Atualizar método de url para permitir URLs relativas ao protocolo
  * Remover o espaço reservado $.format preterido
  * Usar jQuery 1.7 + ativado/desativado, adicionar método destroy
  * Compatibilidade do IE8 mudou .indexOf para $.inArray
  * Converter atributos de valor NaN para indefinido para Opera Mini
  * Parar valor de corte dentro do método exigido
  * Uso: seletor desabilitado para corresponder a elementos desabilitados
  * Excluir algumas teclas do teclado para evitar revalidar o campo
  * Não pesquisar no DOM inteiro por elementos de rádio/caixa de seleção
  * Gerar erros melhores para métodos de regra inválida
  * Erro de validação de número fixo
  * Fixar referência à especificação whatwg
  * Foca em elemento inválido ao validar um conjunto personalizado de entradas
  * Redefinir estilos de elemento ao usar os métodos de realce personalizados
  * Sinal de cifrão para escape na id do erro
  * Reverter "Ignorar somente leitura, bem como campos desabilitados."
  * Atualizar o link em comentário para o algoritmo de Luhn

## <a name="additionals"></a>Adicionais
  * Atualizar dateITA para resolver problema de fuso horário
  * Fixar método de extensão somente para o período de método
  * Fixar método de aceitação para corresponder apenas ao período
  * Atualizar método de tempo para permitir hora de dígito único
  * Remover teste inválido para método notEqualTo
  * Adicionar método notEqualTo
  * Usar referência de jQuery correta por meio de `$`
  * Remover método inútil de iban de check-in de expressão regular
  * Número de CPF do Brasil

## <a name="localization"></a>Localização
  * Atualizar messages_tr.js
  * Atualizar messages_sr_lat.js
  * Adicionando Espanhol do Peru (ES PE)
  * Adicionando Georgiano (ქართული, ge)
  * Correção de erro de digitação na tradução do catalão
  * Aprimoramento da tradução de finlandês (fi)
  * Adição da localidade armênio (hy_AM)
  * Extensão da tradução de italiano (it) com o método atual
  * Adicionar localidade bn_BD
  * Atualizar localidade zh
  * Remover o ponto no final de mensagens em italiano

<a name="1131--2014-10-14"></a>1.13.1 / 2014-10-14
==================

## <a name="core"></a>Núcleo
  * Permitir 0 como valor para autoCreateRanges
  * Aplicar ignorar configuração a todos os elementos validationTargetFor
  * Não cortar valor nos métodos mín/máx/rangelength
  * Escapar id/nome antes de usá-lo como um seletor em errorsFor
  * Padrão explícito para a opção focusCleanup
  * Corrigir regexp incorreta para correspondência describedby
  * Ignorar somente leitura, bem como campos desabilitados
  * Melhorar escape de ID, armazenar ID com escape em describedby
  * Usar o valor de retorno de submitHandler para permitir ou impedir o envio de formulário

## <a name="additionals"></a>Adicionais
  * Adicionar o método postalcodeBR
  * Corrigir o método padrão quando o parâmetro for uma cadeia de caracteres


<a name="1130--2014-07-01"></a>1.13.0 / 2014-07-01
==================

## <a name="all"></a>Todos
* Adicionar wrapper UMD de plug-in

## <a name="core"></a>Núcleo
* Respeitar non-error aria-describedby e erros ocultos vazios
* Aprimorar dateISO RegExp
* Opção/caixa de seleção adicionada para delegar o evento de clique
* Usar aria-describedby para elementos que não são rótulo
* Registrar focusin, focusout e keyup também na opção/caixa de seleção
* Corrigir normalização para o valor do atributo rangelength
* Atualizar método elementValue para lidar com campos type="number"
* Usar charAt em vez de notação de matriz em cadeias de caracteres, para dar suporte ao IE8(?)

## <a name="localization"></a>Localização
* Corrigir a tradução de sk do método rangelength
* Adicionar métodos em finlandês
* Correção de mensagem de validação de número GL
* Correção de mensagem de validação de método de número ES
* Adição de galego (GL)
* Correção de mensagens de francês para métodos mín. e máx.

## <a name="additionals"></a>Adicionais
* Adição do método statesUS
* Correção do método dateITA para lidar com o bug de horário de verão
* Adição do método de data persa
* Adição do método postalCodeCA
* Adição do método postalcodeIT

<a name="1120--2014-04-01"></a>1.12.0 / 2014-04-01
==================

* Adição do teste ARIA ([3d5658e](https://github.com/jzaefferer/jquery-validation/commit/3d5658e9e4825fab27198c256beed86f0bd12577))
* Adição de mensagens de localização em es-AR. ([7b30beb](https://github.com/jzaefferer/jquery-validation/commit/7b30beb8ebd218c38a55d26a63d529e16035c7a2))
* Adição de pontos ausentes para mensagens em 'es' e 'es_AR'. ([a2a653c](https://github.com/jzaefferer/jquery-validation/commit/a2a653cb68926ca034b4b09d742d275db934d040))
* Localização de indonésio (ID) adicionada ([1d348bd](https://github.com/jzaefferer/jquery-validation/commit/1d348bdcb65807c71da8d0bfc13a97663631cd3a))
* Validação de números de documentos NIF, NIE e CIF em espanhol ([#830](https://github.com/jzaefferer/jquery-validation/issues/830), [317c20f](https://github.com/jzaefferer/jquery-validation/commit/317c20fa9bb772770bb9b70d46c7081d7cfc6545))
* Formulário atual para o contexto da solicitação ajax remota adicionado ([0a18ae6](https://github.com/jzaefferer/jquery-validation/commit/0a18ae65b9b6d877e3d15650a5c2617a9d2b11d5))
* Adicionais: Atualizar método IBAN, à direita de espaços em branco ([#970](https://github.com/jzaefferer/jquery-validation/issues/970), [347b04a](https://github.com/jzaefferer/jquery-validation/commit/347b04a7d4e798227405246a5de3fc57451d52e1))
* Método BIC: Aprimoramento do RegEx, {1} sempre é redundante. Fecha gh-744 ([5cad6b4](https://github.com/jzaefferer/jquery-validation/commit/5cad6b493575e8a9a82470d17e0900c881130873))
* Bower: Adicionar o bower. JSON para o registro do pacote ([e86ccb0](https://github.com/jzaefferer/jquery-validation/commit/e86ccb06e301613172d472cf15dd4011ff71b398))
* Altera as referências de dólar para 'jQuery', para compatibilidade com jQuery.noConflict. Fecha gh-754 ([2049afe](https://github.com/jzaefferer/jquery-validation/commit/2049afe46c1be7b3b89b1d9f0690f5bebf4fbf68))
* Núcleo: Adicione o campo "method" para entrada de lista de erros ([89a15c7](https://github.com/jzaefferer/jquery-validation/commit/89a15c7a4b17fa2caaf4ff817f09b04c094c3884))
* Núcleo: Adicionado suporte para mensagens genéricas por meio do atributo data-msg ([5bebaa5](https://github.com/jzaefferer/jquery-validation/commit/5bebaa5c55c73f457c0e0181ec4e3b0c409e2a9d))
* Núcleo: Permitir atributos para ter um valor de zero (por exemplo, min = '0') ([#854](https://github.com/jzaefferer/jquery-validation/issues/854), [9dc0d1d](https://github.com/jzaefferer/jquery-validation/commit/9dc0d1dd946b2c6178991fb16df0223c76162579))
* Núcleo: Desabilitar preterido $. Format ([#755](https://github.com/jzaefferer/jquery-validation/issues/755), [bf3b350](https://github.com/jzaefferer/jquery-validation/commit/bf3b3509140ea8ab5d83d3ec58fd9f1d7822efc5))
* Núcleo: Correção de suporte para várias classes de erro ([c1f0baf](https://github.com/jzaefferer/jquery-validation/commit/c1f0baf36c21ca175bbc05fb9345e5b44b094821))
* Núcleo: Ignorar eventos em elementos ignorados ([#700](https://github.com/jzaefferer/jquery-validation/issues/700), [a864211](https://github.com/jzaefferer/jquery-validation/commit/a86421131ea69786ee9e0d23a68a54a7658ccdbf))
* Núcleo: Melhorar método elementValue ([6c041ed](https://github.com/jzaefferer/jquery-validation/commit/6c041edd21af1425d12d06cdd1e6e32a78263e82))
* Núcleo: Verifique Element () lidar com elementos ignorado corretamente. ([3f464a8](https://github.com/jzaefferer/jquery-validation/commit/3f464a8da49dbb0e4881ada04165668e4a63cecb))
* Núcleo: Alternar dataRules para análise ao estilo de especificação W3C HTML5 ([460fd22](https://github.com/jzaefferer/jquery-validation/commit/460fd22b6c84a74c825ce1fa860c0a9da20b56bb))
* Núcleo: Disparar sucesso em opcional, mas ter outros validadores bem-sucedidos ([#851](https://github.com/jzaefferer/jquery-validation/issues/851), [f93e1de](https://github.com/jzaefferer/jquery-validation/commit/f93e1deb48ec8b3a8a54e946a37db2de42d3aa2a))
* Núcleo: Usar elemento simples em vez de cancelar o encapsulamento do elemento novamente ([03cd4c9](https://github.com/jzaefferer/jquery-validation/commit/03cd4c93069674db5415a0bf174a5870da47e5d2))
* Núcleo: certificar-se de que o remoto seja executado por último ([#711](https://github.com/jzaefferer/jquery-validation/issues/711), [ad91b6f](https://github.com/jzaefferer/jquery-validation/commit/ad91b6f388b7fdfb03b74e78554cbab9fd8fca6f))
* Demo: Use a opção correta na demonstração de várias partes. ([#1025](https://github.com/jzaefferer/jquery-validation/issues/1025), [070edc7](https://github.com/jzaefferer/jquery-validation/commit/070edc7be4de564cb74cfa9ee4e3f40b6b70b76f))
* Correção do uso de $/jQuery em métodos adicionais. Corrige #839 ([#839](https://github.com/jzaefferer/jquery-validation/issues/839), [59bc899](https://github.com/jzaefferer/jquery-validation/commit/59bc899e4586255a4251903712e813c21d25b3e1))
* Melhorar traduções do chinês ([1a0bfe3](https://github.com/jzaefferer/jquery-validation/commit/1a0bfe32b16f8912ddb57388882aa880fab04ffe))
* Implementação inicial de ARIA-Required ([bf3cfb2](https://github.com/jzaefferer/jquery-validation/commit/bf3cfb234ede2891d3f7e19df02894797dd7ba5e))
* Localização: alterar valores aceitos para extensão. Corrige #771, fecha gh-793. ([#771](https://github.com/jzaefferer/jquery-validation/issues/771), [12edec6](https://github.com/jzaefferer/jquery-validation/commit/12edec66eb30dc7e86756222d455d49b34016f65))
* Mensagens: Adicionar localização do Islandês ([dc88575](https://github.com/jzaefferer/jquery-validation/commit/dc885753c8872044b0eaa1713cecd94c19d4c73d))
* Mensagens: Adicione pontos ausentes a mensagens de "bg", "fr" e "sr". ([adbc636](https://github.com/jzaefferer/jquery-validation/commit/adbc6361c377bf6b74c35df9782479b1115fbad7))
* Mensagens: Criar messages_sr_lat. js ([f2f9007](https://github.com/jzaefferer/jquery-validation/commit/f2f90076518014d98495c2a9afb9a35d45d184e6))
* Mensagens: Criar messages_tj. js ([de830b3](https://github.com/jzaefferer/jquery-validation/commit/de830b3fd8689a7384656c17565ee92c2878d8a5))
* Mensagens: Corrigir tradução de sr_lat, adicionar espaço ausente ([880ba1c](https://github.com/jzaefferer/jquery-validation/commit/880ba1ca545903a41d8c5332fc4038a7e9a580bd))
* Mensagens: Atualizar messages_sr. js, corrigir espaço ausente ([10313f4](https://github.com/jzaefferer/jquery-validation/commit/10313f418c18ea75f385248468c2d3600f136cfb))
* Métodos: Adicionar outro método para a moeda ([1a981b4](https://github.com/jzaefferer/jquery-validation/commit/1a981b440346620964c87ebdd0fa03246348390e))
* Métodos: Adicionar aspas inglesas para pontuação do striphtml ([aa0d624](https://github.com/jzaefferer/jquery-validation/commit/aa0d6241c3ea04663edc1e45ed6e6134630bdd2f))
* Métodos: Corrigir método dateITA, evitando erros de horário de verão ([279b932](https://github.com/jzaefferer/jquery-validation/commit/279b932c1267b7238e6652880b7846ba3bbd2084))
* Métodos: Métodos localizados para a cultura chilena (es-CL) ([cf36b93](https://github.com/jzaefferer/jquery-validation/commit/cf36b933499e435196d951401221d533a4811810))
* Métodos: Atualizar o email para usar regex HTML5, remover método email2 ([#828](https://github.com/jzaefferer/jquery-validation/issues/828), [dd162ae](https://github.com/jzaefferer/jquery-validation/commit/dd162ae360639f73edd2dcf7a256710b2f5a4e64))
* Método de padrão: Remova delimitadores, pois as implementações de HTML5 não incluem nenhum deles. ([37992c 1](https://github.com/jzaefferer/jquery-validation/commit/37992c1c9e2e0be8b315ccccc2acb74863439d3e))
* Restringir o validador de cartão de crédito para incluir a verificação de comprimento. Fecha gh-772 ([f5f47c5](https://github.com/jzaefferer/jquery-validation/commit/f5f47c5c661da5b0c0c6d59d169e82230928a804))
* Atualizar messages_ko.js - fecha gh-715 ([5da3085](https://github.com/jzaefferer/jquery-validation/commit/5da3085ff02e0e6ecc955a8bfc3bb9a8d220581b))
* Atualizar messages_pt_BR.js. Fecha gh-782 ([4bf813b](https://github.com/jzaefferer/jquery-validation/commit/4bf813b751ce34fac3c04eaa2e80f75da3461124))
* Atualizar phonesUK e mobileUK para aceitar novos prefixos. Fecha gh-750 ([d447b41](https://github.com/jzaefferer/jquery-validation/commit/d447b41b830dee984be21d8281ec7b87a852001d))
* Verificar códigos postais de nove dígitos. Fecha gh-726 ([165005d](https://github.com/jzaefferer/jquery-validation/commit/165005d4b5780e22d13d13189d107940c622a76f))
* phoneUS: Adicione exclusões de N11. Fecha gh-861 ([519bbc6](https://github.com/jzaefferer/jquery-validation/commit/519bbc656bcb26e8aae5166d7b2e000014e0d12a))
* resetForm devem limpar quaisquer valores aria-invalid ([4f8a631](https://github.com/jzaefferer/jquery-validation/commit/4f8a631cbe84f496ec66260ada52db2aa0bb3733))
* valid(): Verifique se todos os elementos. Corrige #791 – valid() valida apenas o primeiro elemento (invalid) ([#791](https://github.com/jzaefferer/jquery-validation/issues/791), [6f26803](https://github.com/jzaefferer/jquery-validation/commit/6f268031afaf4e155424ee74dd11f6c47fbb8553))

<a name="1111--2013-03-22"></a>1.11.1 / 2013-03-22
==================

  * Reverter para converter também os parâmetros do método range para números. Fecha gh-702
  * Substituir grande parte do uso do PHP por manipuladores mockjax. Fazer uma limpeza de demonstração também, atualizar para plug-in masked-input mais recente. Manter demonstração de captcha em PHP. Corrige #662
  * Remover o realce de código embutido da demonstração milk. A exibição do código-fonte funciona bem.
  * Corrigir demonstração de totais dinâmicos cortando o espaço em branco do conteúdo do modelo antes de passar para o construtor de jQuery
  * Corrigir a validação mín/máx. Fecha gh-666. Corrige #648
  * Correção de "mensagens" exibidas como uma regra e que causam uma exceção após a atualização por meio de rules("add"). Fecha gh-670, Corrige #624
  * Adicionar localização de coreano (ko). Fecha gh-671
  * Aprimoramento do método de CEP do Reino Unido para filtrar códigos postais mais inválidos. Fecha #682
  * Atualizar messages_sv.js. Fecha #683
  * Alteração do link grunt para o site do projeto. Fecha #684
  * Movimentação do método remoto para o fim da lista a fim de ser executado por último, após todos os outros métodos serem aplicados a um campo. Corrige #679
  * Atualização da descrição do plugin.json, deve incluir a palavra "validar"
  * Correção de erros de ortografia
  * Correção do carregador jQuery para usar o próprio caminho. Correção de demonstrações aninhadas.
  * Atualização de grunt-contrib-qunit para utilizar o PhantomJS 1.8, quando instalado pelo módulo de nó "phantomjs"
  * Fazer valid() retornar um valor booliano em vez de 0 ou 1. Corrige #109 – valid() não retorna um valor booliano

<a name="1110--2013-02-04"></a>1.11.0 / 2013-02-04
==================

  * Remoção de limpeza como números das regras `min`, `max` e `range`. Corrige #455. Fecha gh-528.
  * Atualização de rótulos pré-existentes – Corrige #430 fecha gh-436
  * Corrigir $.validator.format para evitar a interpolação de grupo, em que pelo menos o IE8/9 substitui -bash pela correspondência. Corrige #614
  * Correção de expressão regular mimetype
  * Adição de manifesto de plug-in e atualização de cabeçalhos apenas para licença do MIT, descarte de licença dupla desnecessária (como jQuery).
  * Mensagens em hebraico: Removido pontos no final das frases – corrige gh-568
  * Tradução de francês para validação de require_from_group. Corrige gh-573.
  * Permitir que grupos sejam uma matriz ou uma cadeia de caracteres – Corrige #479
  * Espaços removidos com vários tipos MIME
  * Correção de algumas validações de data, erros de sintaxe JS.
  * Remoção do suporte para plug-in de metadados, substituição pelas propriedades data-rule- e data-msg- (adicionadas em 907467e8).
  * Sftp adicionado como um padrão de url válido
  * Adicionar localização de malaio (my)
  * Atualizar localization/messages_hu.js
  * Remover polyfill focusin/focusout. Corrige #542 – Inclusão do jquery.validate interfere com eventos focusin e focusout no IE9
  * Localização: Correção de erro de digitação na tradução do finlandês
  * Corrigir demonstração de RTM para mostrar o ícone inválido voltando de válido para inválido
  * Correção de retorno prematuro na função remota que impediu que a chamada do ajax seja feita no caso de uma entrada ser inserida muito rapidamente. Garante que a validação remota sempre valide o valor mais recente.
  * Desfazer a correção para #244. Corrige #521 – Validação de email acionada imediatamente quando o texto está no campo.

<a name="1100--2012-09-07"></a>1.10.0 / 2012-09-07
===================

  * Correção de cadeias de caracteres em francês para nowhitespace, phoneUS, phoneUK e mobileUK com base em comentários da comunidade.
  * renomear arquivos para language_REGION de acordo com o padrão ISO_3166-1 [http://en.wikipedia.org/wiki/ISO_3166-1), para o Taiwan, o idioma é o chinês (zh) e a região é Taiwan (TW)]
  * Otimizar padrões de RegEx, especialmente para números de telefone do Reino Unido.
  * Adicionar Nome do Idioma para cada arquivo, renomear o código do idioma de acordo com o padrão ISO 639 para estoniano, georgiano, ucraniano e chinês (http://en.wikipedia.org/wiki/List_of_ISO_639-1_codes)
  * Adição da localização de croata (HR)
  * Traduções existentes de francês foram editadas, e traduções de francês para os métodos adicionais foram adicionadas.
  * Mesclagem de alterações para especificar mensagens de erro personalizadas em atributos de dados
  * Regex de número de celular do Reino Unido atualizado para novos números. Corrige #154
  * Adição de elemento à chamada bem-sucedida com teste. Corrige #60
  * Correção de regex para o método de tempo adicional. Corrige #131
  * resetForm agora limpa previousValue antigo em elementos de formulário. Corrige #312
  * Adição de teste de caixa de seleção para require_from_group e require_from_group alterados a fim de usar elementValue. Corrige #359
  * Correção de problemas de resposta de DataFilter no jQuery 1.5.2+. Corrige #405
  * Adição de demonstração de jQuery Mobile. Corrige #249
  * Cancelar a otimização de findByName para manter a correção. Corrige #82 – $.validator.prototype.findByName quebra no IE7
  * Adição de suporte a código postal dos Estados Unidos e teste. Corrige #90
  * LastElement alterado para lastActive em keyup, ignorar a validação em guia ou elemento vazio. Corrige #244
  * Remoção de separação de número de stripHtml. Corrige #2
  * Correção de contagem inválida em validação remota de inválida para válida. Corrige #286
  * Adicionar link para file_input ao índice de demonstração
  * Movimentação do método antigo de aceitação para extensão de método adicional, adição de novo método de aceitação para tratar a filtragem de mimetype de navegador padrão. Corrige #287 e substitui #369
  * Desabilita desfoque eventos quando onfocusout estiver definido como false. Teste adicionado.
  * Correção de problema com valor para botões de opção e caixas de seleção. Corrige #363
  * Teste adicionado para rangeWords e correção de regex e limites no método. Corrige #308
  * Correção de demonstração de TinyMCE e adição de um link à página de demonstração. Corrige #382
  * Alteração da mensagem de localização para mín/máx. Corrige #273
  * Adição de pseudo seletor para tipos de entrada de texto a fim de corrigir o problema com o atributo de tipo vazio padrão. Testes adicionados e algumas marcações de teste. Corrige #217
  * Correção de bug delegado para demonstração de dinâmicas totais. Corrige #51
  * Correção de mensagem incorreta para o validador alfanumérico
  * Remoção da verificação de falso incorreto no atributo necessário
  * correção de atributo necessário para navegadores não html5. Corrige #301
  * Métodos adicionados "require_from_group" e "skip_or_fill_minimum"
  * Use o código iso correto para sueco
  * Arquivos HTML de demonstração atualizados para usar o doctype HTML5
  * Correção do problema de regex para decimais sem zeros à esquerda. Novos teste de métodos adicionado. Corrige #41
  * Introdução de um método elementValue que normaliza apenas os valores de cadeia de caracteres (não toca no valor de matriz de seleção múltipla). Corrige #116
  * Suporte para botões de envio adicionados dinamicamente, e caso de teste atualizado. Usa validateDelegate. Código de PR #9
  * Correção de aspas duplas incorretas em acessórios de teste
  * Correção do método maxWords para incluir o limite superior, não excluí-lo. Corrige #284
  * Correção de erro de gramática na mensagem de validador de intervalo alemão. Corrige #315
  * Correção do tratamento de vários nomes de classe para a opção errorClass. Teste por Max Lynch. Corrige #280
  * Correção do uso de jQuery.format, deve ser $.validator.format. Corrige #329
  * Métodos para "todos" os números de telefone do Reino Unido + códigos postais do Reino Unido
  * Método de padrão: Converta param string para RegExp. Corrige o problema #223
  * erro de gramática no arquivo de localização de alemão
  * Adição da localização para estoniano das mensagens
  * Aprimoramento da manipulação de dica de ferramenta na demonstração themerollered
  * Adição de type="text" aos campos de entrada sem o atributo type para atender ao qSA
  * Atualização da demonstração themerollered para usar a dica de ferramenta a fim de mostrar erros como sobreposição.
  * Atualização da demonstração themerollered para usar a interface de usuário mais recente de jQuery (junto com a versão mais recente de jQuery). Mudança do código para acelerar o carregamento da página.
  * Correção de mensagem de erro mínima quebrada em japonês.
  * Atualização do plug-in de formulário para a versão mais recente. Aprimoramento da demonstração ajaxSubmit.
  * Descarte dos métodos dateDE e numberDE de classRuleSettings, algo que ficou da mudança desses métodos localizados
  * Passagem do evento de envio para o retorno de chamada de submitHandler
  * Correção #219 - Correção de valid() em elementos com dependency-callback ou dependency-expression.
  * Aprimoramento do build para remover dist dir e garantir que apenas a versão atual seja compactada

<a name="190"></a>1.9.0
---
* Adição da localização de basco (EU)
* Adição da localização de esloveno (SL)
* Correção do problema #127 - Traduções do finlandês tem um : em vez de ;
* Correção da localização russa, problema de sintaxe secundário
* Adição do suporte para tipos de entrada HTML5, corrige #97
* Aprimoramento do suporte a HTML5 pela configuração do atributo novalidate no formulário, e leitura do atributo type.
* Correção de ShowLabel() removendo todas as classes do elemento de erro. Remova somente settings.validClass. Corrige #151.
* Adição de "pattern" aos métodos adicionais para validar em expressões regulares arbitrárias.
* Aprimoramento do método de e-mail para não permitir o ponto no final (válido por RFC, mas indesejado aqui). Corrige #143
* Correção de traduções suecas e norueguesas, mensagens mín/máx foram alternadas. Corrige #181
* Correção de #184 - resetForm: deve cancelar a definição de lastElement
* Correção do #71 - aprimoramento do método time existente e adição do método time12h para o formato de hora 12h am/pm
* Correção do #177 - Correção da validação de uma única entrada de caixa de opção ou de seleção
* Correção do #189 - : agora os elementos ocultos são ignorados por padrão
* Correção do #194 - Requires como atributo falha se o jQuery>=1.6 - Use .prop em vez de .attr
* Correção do #47, #39, #32 - Permissão para números de cartão de crédito conterem espaços e traços (espaços são normalmente inseridos pelos usuários).

<a name="181"></a>1.8.1
---
* Adição da localização do tailandês (TH), corrige o #85
* Adição da localização do vietnamita (VI), agradecimentos a Ngoc
* Correção do problema #78. Estilo de erro/válido se aplica a todos os botões de opção do mesmo grupo para validação obrigatória.
* Não use form.elements, pois ele não é mais compatível com o jQuery 1.6. Ele é cheio de bugs mesmo (IE6-8: form.elements === form).

<a name="180"></a>1.8.0
---
* Melhoria da localização do NL (http://plugins.jquery.com/node/14120)
* Adição da localização do georgiano (GE), agradecimentos a Avtandil Kikabidze
* Adição da localização do sérvio (SR), agradecimentos a Aleksandar Milovac
* Adição de ipv4 e ipv6 aos métodos adicionais, agradecimentos a Natal Ngétal
* Adição da localização do japonês (JA), agradecimentos a Bryan Meyerovich
* Adição da localização do catalão (CA), agradecimentos a Xavier de Pedro
* Correção de instruções var ausentes em loops for-in
* Correção para validação remota, em que uma mensagem formatada ficou embaralhada (https://github.com/jzaefferer/jquery-validation/issues/11)
* Correções de bug de compatibilidade com o jQuery 1.5.1, mantendo a compatibilidade com versões anteriores

<a name="17"></a>1.7
---
* Adição da localização do lituano (LT)
* Adição da localização do grego (EL) (http://plugins.jquery.com/node/12319)
* Adição da localização do letão (LV) (http://plugins.jquery.com/node/12349)
* Adição da localização do hebraico (HE) (http://plugins.jquery.com/node/12039)
* Correção da localização do espanhol (ES) (http://plugins.jquery.com/node/12696)
* Adição da demonstração themerolled da interface do usuário do jQuery
* Remoção de cmxform.js
* Correção de quatro ponto-e-vírgulas ausentes (http://plugins.jquery.com/node/12639)
* Mudança de nome de phone-method no additional-methods.js para phoneUS
* Adição dos métodos phoneUK e mobileUK a additional-methods.js (http://plugins.jquery.com/node/12359)
* Aprofundamento das opções de extensão para evitar modificar vários formulários ao usar o método rules em um único elemento (http://plugins.jquery.com/node/12411)
* Correções de bug de compatibilidade com o jQuery 1.4.2, mantendo a compatibilidade com versões anteriores

<a name="16"></a>1.6
---
* Adição da localização do árabe (AR), português (PTPT), persa (FA), finlandês (FI) e búlgaro (BR)
* Atualização da localização do sueco (SE) (alguns caracteres iso html estavam ausentes)
* Correção de $.validator.addMethod para lidar adequadamente com a cadeia de caracteres vazia versus indefinido para o argumento de mensagem
* Correção de duas variáveis globais acidentais
* Aprimoramento de min/max/rangeWords (em additional-methods.js) para retirar o html antes da contagem; válido ao contar palavras em um editor de richtext
* Adição de métodos localizados para DE, NL e PT, removendo os métodos dateDE e numberDE (use messages_de.js e methods_de.js com métodos de número e data)
* Correção da sincronização de envio de formulário de remoto, agradecimentos a Matas Petrikas
* Aprimoramento da validação de seleção interativa, validando agora também por clique (por meio de opção ou seleção, inconsistentes entre navegadores); não funciona no Safari, que não dispara um evento de clique em todos os elementos de seleção; corrige http://plugins.jquery.com/node/11520
* Atualização para o último plug-in de formulário (2.36), corrigindo http://plugins.jquery.com/node/11487
* Associação ao evento de desfoque para o destino equalTo, a fim de revalidar quando há alterações no destino; corrige http://plugins.jquery.com/node/11450
* Validação de seleção simplificada, delegando ao método val() do jQuery para obter o valor de seleção; deve corrigir http://plugins.jquery.com/node/11239
* Correção da mensagem padrão para dígitos (http://plugins.jquery.com/node/9853)
* Correção do problema com mensagens remotas armazenadas em cache (http://plugins.jquery.com/node/11029 e http://plugins.jquery.com/node/9351)
* Correção de um ponto-e-vírgula ausente em additional-methods.js (http://plugins.jquery.com/node/9233)
* Adição de detecção automática de parâmetros de substituição em mensagens, eliminando a necessidade de fornecer funções de formato (http://plugins.jquery.com/node/11195)
* Correção de um problema com :filled/:blank, de alguma forma causado pelo Sizzle (http://plugins.jquery.com/node/11144)
* Adição de um método de inteiro a additional-methods.js (http://plugins.jquery.com/node/9612)
* Correção do método errorsFor no qual o for-attribute contém caracteres que precisam de escape para serem válidos dentro de um seletor (http://plugins.jquery.com/node/9611)

<a name="155"></a>1.5.5
---
* Correção de http://plugins.jquery.com/node/8659
* Correção de vírgula à direita em messages_cs.js

<a name="154"></a>1.5.4
---
* Correção de um bug de método remoto (http://plugins.jquery.com/node/8658)

<a name="153"></a>1.5.3
---
* Correção de um bug relacionado à opção wrapper, em que todos os elementos ancestrais que corresponderam à opção wrapper foram selecionados (http://plugins.jquery.com/node/7624)
* Atualização da demonstração com várias partes do uso do accordion da interface do usuário mais recente do jQuery
* Adição dos métodos dateNL e time ao additionalMethods.js
* Adição da localização do chinês tradicional (Taiwan, tw) e do Cazaquistão (KK)
* Movimentação de jQuery.format (anteriormente String.format) para jQuery.validator.format; jQuery.format foi preterido e será removido no 1.6 (confira http://code.google.com/p/jquery-utils/issues/detail?id=15 para obter detalhes)
* Limpeza de messages_pl.js e messages_ptbr.js (mensagens ainda definidas para max/min/rangeValue, que foram removidos no 1.4)
* Correção de lógica booliana com falha no método valid-plugin para vários elementos; agora todos os elementos precisam ser válidos para obter um resultado booliano verdadeiro (http://plugins.jquery.com/node/8481)
* Aprimoramento de $. addmethod: Um indefinido terceiro message-argument não substituirá um existente (de mensagem http://plugins.jquery.com/node/8443)
* Aprimoramento da opção submitHandler: Quando usado, clique em eventos nos enviam botões são capturados e o botão de envio é inserido no formulário antes de chamar submitHandler e removido posteriormente; mantém (intactos de botões de envio http://plugins.jquery.com/node/7183#comment-3585)
* Adição de opção validClass, padrão "valid", que adiciona essa classe a todos os elementos válidos, após a validação (http://dev.jquery.com/ticket/2205)
* Adição do método creditcardtypes a additionalMethods.js, incluindo testes (por meio de http://dev.jquery.com/ticket/3635)
* Melhoria do método remoto para permitir a mensagem do lado do servidor como uma cadeia de caracteres, ou verdadeiro para válido, ou falso para inválido, usando a mensagem definida no lado do cliente (http://dev.jquery.com/ticket/3807)
* Melhoria do método Accept para aceitar também uma lista de valores separados por vírgula no estilo Drupal (http://plugins.jquery.com/node/8580)

<a name="152"></a>1.5.2
---
* Correção de mensagens additional-methods.js para maxWords, minWords e rangeWords, a fim de incluir a chamada para $.format
* Correção de valor transmitido para métodos a fim de excluir o retorno de carro (\r), é o mesmo que o val() do jQuery faz
* Adição da localização do eslovaco (sk)
* Adição de demonstração para integração com guias da interface do usuário de jQuery
* Adição de exemplo de selects-grouping para demonstração de guias (veja a segunda guia, o campo de data de nascimento)

<a name="151"></a>1.5.1
---
* Atualização da demonstração do marketo para o uso da opção invalidHandler, em vez de associar o evento invalid-form
* Adição de exemplo de integração TinyMCE
* Adição da localização do ucraniano (ua)
* Correção da validação de comprimento para trabalhar com o valor cortado (regressão do 1.5, no qual o corte geral antes da validação foi removido)
* Várias correções pequenas de compatibilidade com o 1.2.6 e o 1.3

<a name="15"></a>1.5
---
* Demonstração básica aprimorada, validação do campo confirm-password após a alteração da senha
* Correção da validação básica para passar o valor de entrada não cortado como o primeiro parâmetro para os métodos de validação, exige a alteração adequada; interrompe um método personalizado existente que dependem do corte
* Adição da localização do norueguês (no), italiano (it), húngaro (hu) e romeno (ro)
* Correção do #3195: Duas falhas na localização do sueco
* Correção do #3503: Estendida de extensão para aceitar a propriedade de mensagens: use para especificar adicionar mensagens personalizadas a um elemento por meio de regras ("add", {mensagens: {necessários: "Required! " } });
* Correção do #3356: Regressão do #2908 ao usar meta-option
* Correção do #3370: Opção adicionado ignoreTitle, definida para ignorar a leitura de mensagens do atributo title, ajuda a evitar problemas com a barra de ferramentas do Google; o padrão é false para fins de compatibilidade
* Correção do #3516: Evento invalid-form de gatilho, mesmo quando a validação remota estiver envolvida
* Adição da opção invalidHandler como um atalho para bind("invalid-form", function() {})
* Correção do problema do Safari de carregamento do indicador no ajaxSubmit-integration-demo (anexar a corpo primeiro, depois ocultar)
* Adição de teste para validação de cartão de crédito e mensagem padrão aprimorada
* Aprimoramento de validação remota, aceitando opções para passar $.ajax como parâmetro (cadeia de caracteres de url ou opções, incluindo a propriedade da url, além de tudo para o qual $.ajax dá suporte)

<a name="14"></a>1.4
---
* Correção do #2931, validar elementos na ordem do documento e ignorar entradas type=image
* Correção do uso das variáveis $ e jQuery, agora totalmente compatíveis com todas as variações de uso do noConflict
* Implementação do #2908, permitindo mensagens personalizadas por meio de metadados para class="{required:true,messages:{required:'required field'}}", adição de demo/custom-messages-metadata-demo.html
* Remoção dos métodos preteridos minValue (min), maxValue (max), rangeValue (rangevalue), minLength (minlength), maxLength (maxlength), rangeLength (rangelength)
* Corrigida regressão do #2215: Chamada de cancelamento de realce apenas para elementos atuais, nem tudo
* Implementação do #2989, permitindo que o botão da imagem cancele a validação
* Correção do problema no qual o IE valida incorretamente o maxlength=0
* Adição da localização do tcheco (cs)
* Redefinição de validator.submitted em validator.resetForm(), permitindo uma redefinição completa, quando necessário
* Correção do #3035, ignorando todos os atributos falsos ao ler as regras (0, cadeia indefinida, vazia), remoção de parte da solução maxlength (para 0)
* Adição da localização do holandês (nl) (#3201)

<a name="13"></a>1.3
---
* Correção de evento de forma inválida, disparado agora apenas quando o formulário for inválido
* Adicionada localização de espanhol (es), russo (ru), português do Brasil (ptbr), turco (tr) e polonês (pl)
* Adição de plug-in removeAttrs para facilitar a adição e remoção de vários atributos
* Adição da opção de grupos para exibir uma única mensagem para vários elementos, por meio de grupos: { arbitraryGroupName: "fieldName1 fieldName2[, fieldNameN"}
* Aprimoramento de rules() para adicionar e remover regras (estáticas): rules("add", "method1[, methodN]"/{method1:param[, method_n:param]}) e rules("remove"[, "method1[, method_n]")
* Aprimoramento de rules-option, aceita lista de métodos separados por espaço, por exemplo. {birthdate: "required date"}
* Correção de validação de grupo de caixa de seleção com regras embutidas: Desde que as regras são especificadas no primeiro elemento, o grupo agora é validado corretamente no clique
* Correção de #2473, ignorando todas as regras com um parâmetro explícito de boolean-false, por exemplo, required:false é o mesmo que não especificar required (foi tratado como required:true até o momento)
* Correção #2424, com um patch modificado de #2473: Métodos que retornam uma incompatibilidade de dependência não param de ser avaliado mais; outras regras ainda assim, o sucesso não é aplicado para campos opcionais
* Correção de validação de url e email para não usar valores cortados
* Correção de validação de cartão de crédito a fim de aceitar somente dígitos e traços ("asdf" não é um número válido de cartão de crédito)
* Permitir elementos de botão e de entrada para cancelar botões (por meio da classe = "cancel")
* Correção de #2215: Exibição de mensagem fixo para chamar cancelamento de realce como parte de mostrar e ocultar mensagens, não há mais efeitos visual colaterais durante a verificação de um elemento e validator. checkform extraído para validar um formulário sem exibição da interface do usuário
* Seletores personalizados reescritos (:blank, :filled, :unchecked) com funções para compatibilidade com AIR

<a name="121"></a>1.2.1
-----

* Plug-in delegado agrupado com plug-in de validação - é sempre necessário
* Aprimoramento da validação remota para incluir as partes do plug-in ajaxQueue para sincronização apropriada (sem a necessidade de plug-in adicional)
* Correção de StopRequest para evitar pendingRequest < 0
* Adição da propriedade jQuery.validator.autoCreateRanges, o padrão é false, permissão para converter mín/máx para range e minlength/maxlength para rangelength; isso corrige basicamente o problema, introduzido pela criação automática de intervalos no 1.2
* Correção de métodos opcionais para não realçar nada se o campo estiver em branco, ou seja, não disparar o êxito
* Permitir false/null para as opções de realçar/cancelar realce, em vez de forçar uma do-nothing-callback, mesmo quando nada precisar ser realçado
* Correção de chamada validate() sem elementos selecionados, retornando indefinido em vez de gerar um erro
* Demonstração aprimorada, substituindo metadados por classes/atributos para especificar regras
* Correção de erro quando nenhuma mensagem personalizada for usada para validação remota
* Modificação de validação de email e url para exigir o rótulo de domínio e rótulo superior
* Correção de validação de email e url para exigir o TLD (na verdade, para exigir o rótulo de domínio); versão 1.2 (TLD é opcional) foi movida para adições como url2 e email2
* Correção de demonstração de dinâmicas totais no IE6/7 e modelagem aprimorada, usando a área de texto para armazenar o modelo de várias linhas e a interpolação de cadeia de caracteres
* Adição de exemplo de formulário de logon com o link "Senha do email" que faz com que o campo de senha seja opcional
* Aprimoramento da demonstração de dinâmicas totais com um exemplo de uma única mensagem para dois campos

<a name="12"></a>1.2
---

* Adição de exemplo de validação de captcha do AJAX (baseado em http://psyrens.com/captcha/)
* Adição de remember-the-milk-demo (agradecemos à equipe de RTM pela permissão!)
* Adição de marketo-demo (Agradecemos a Glen Lipka!)
* Adição de suporte para validação de ajax, confira o método "remote"; serverside retorna JSON, true para elementos válidos, false ou uma String para inválida, String é usada como mensagem
* Adição de opções de realce e cancelamento de realce, por padrão, alterna errorClass no elemento, permite o realce personalizado
* Adição do método de plug-in valid() para a verificação programática fácil de formulários e campos sem a necessidade de usar a API do validador
* Adição do método de plug-in rules() para ler e gravar regras para um elemento (atualmente somente leitura)
* Substituição do regex para o método de email, graças à contribuição de Scott Gonzalez; confira http://projects.scottsplayground.com/email_address_validation/
* Reestruturação da arquitetura de evento para depender somente da delegação, melhorando o desempenho e facilitando o uso para o desenvolvedor (exige jquery.delegate.js)
* Movimentação da documentação de embutida para http://docs.jquery.com/Plugins/Validation – incluindo exemplos interativos para todos os métodos
* Remoção do validator.refresh(), a validação agora é completamente dinâmica
* Renomeação de minValue para min, maxValue para max e rangeValue para range, descartando os nomes anteriores (serão removidos na versão 1.3)
* Renomeação de minLength para minlength, maxLength para maxlength e rangeLength para rangelength, descartando os nomes anteriores (serão removidos na versão 1.3)
* Adição de recurso para mesclar min + max e range, e minlength + maxlength em rangelength
* Adição de suporte para parâmetros de regra dinâmica, permitindo especificar uma função como um parâmetro, por exemplo. para minlength, chamado ao validar o elemento
* Permite especificar null ou uma cadeia de caracteres vazia como uma mensagem para não exibir nada (confira a demonstração do marketo)
* Revisão das regras: Agora dá suporte à combinação de regras-option, metadados, classes (novas) e atributos (novos), confira Rules () para obter detalhes

<a name="112"></a>1.1.2
---

* Substituição do regex para o método de URL, graças à contribuição de Scott Gonzalez; confira http://projects.scottsplayground.com/iri/
* Aprimoramento do método de email para lidar melhor com caracteres unicode
* Correção de erro de contêiner para ocultar quando todos os elementos forem válidos, não apenas no envio do formulário
* Correção de String.format para jQuery.format (mudando para o namespace jQuery)
* Correção do método de aceitação para aceitar as extensões de maiúsculas e minúsculas
* Correção do método de plug-in validate() a fim de criar apenas uma instância de validação para uma determinada forma, e sempre retornar essa instância (evita associar várias vezes os eventos)
* Alteração do console do modo de depuração do nível "erro" para "aviso"

<a name="111"></a>1.1.1
-----

* Correção de XHTML inválido, impedindo a criação do rótulo de erro no IE desde o jQuery 1.1.4
* String. Format fixa e aprimorada: Pesquisar e substituir, melhor manipulação de argumentos de matriz global
* Correção do tratamento do botão de cancelamento para usar o validador de objeto a fim de armazenar o estado, em vez do elemento do formulário
* Correção de seletores de nome para lidar com nomes "complexos", por exemplo. contendo colchetes ("list[]")
* Adição de botão e desabilitação de elementos a serem excluídos da validação
* Movimentação de manipuladores de eventos do elemento para que a atualização possa adicionar manipuladores aos novos elementos
* Correção da validação de email para permitir domínios longos de nível superior (por exemplo. ".travel")
* Mudança de showErrors() de valid() para form()
* Adição de validator.size(): retorna o número de erros atuais
* Chame submitHandler com validador como escopo para facilitar o acesso de seus métodos, por exemplo. para localizar os rótulos de erro usando errorsFor(Element)
* Compatível com o jQuery 1.1.x e 1.2.x

<a name="11"></a>1.1
---

* Adição de validação em desfoque, keyup e clique (para caixas de seleção e botões de opção). Substitui event-option.
* Correção de resetForm
* Correção de custom-methods-demo

<a name="10"></a>1.0
---

* Aprimoramento dos métodos number and numberDE para verificar se há números decimais corretos com delimitadores
* Somente os elementos que têm regras são verificados (caso contrário, success-option é aplicada a todos os elementos)
* Adição de método de número de cartão de crédito (agradecemos ao Brian Klug)
* Adição de ignore-option, por exemplo, ignore: "[@type=hidden]", usando essa expressão para excluir elemento da validação. Padrão: nenhum, embora os botões de envio e redefinição sejam sempre ignorados
* Aprimoramento avançado de Functions-as-messages, fornecendo um auxiliar String.format flexível
* Aceitar Funções como mensagens, fornecendo mensagens de tempo de execução personalizadas
* Correção de exclusão de elementos sem regras da successList
* Correção de custom-method-demo, substituição do alerta por mensagem que exibe o número de erros
* Correção de form-submit-prevention ao usar submitHandler
* Dependência completamente removida nas IDs de elemento, embora ainda seja utilizada (quando houver) para vincular rótulos de erro às entradas. Obtenção por meio de uma matriz com {name, message, element} em vez de um objeto com pares de id:message para errorList interno.
* Adição de suporte para especificar regras simples como cadeias de caracteres simples, por exemplo, "required" é equivalente a {required: true}
* Adição de recurso: Adicione errorClass ao elemento pai de campo inválido s, tornando mais fácil para o contêiner do campo de rótulo/ou o rótulo para o campo de estilo.
* Adição de recurso: focusCleanup - se estiver habilitado, remove o errorClass dos elementos inválidos e oculta todas as mensagens de erro sempre que o elemento for focalizado.
* Adição da opção de sucesso para mostrar que um campo foi validado com êxito
* Correção de select-issue do Opera (evitando um attribute-collision)
* Correção de problemas de foco de elementos ocultos no IE
* Adição de recurso para ignorar a validação de botões de envio com a classe "cancel"
* Correção de possíveis problemas com a Barra de Ferramentas do Google, preferindo mensagens de opção de plug-in sobre o atributo de título
* submitHandler só é chamado quando um evento de envio real tiver sido tratado, validator.form() retorna false somente para formulários inválidos
* Agora, os elementos inválidos se concentram apenas no envio ou por meio de validator.focusInvalid(), evitando todos os problemas de foco em desfoque
* Problema de layout de contêiner de erro do IE6 resolvido
* Personalizar o elemento de erro por meio da opção errorElement
* Adição de validator.refresh() para localizar novas entradas no formulário
* Adição de método de validação de aceitação, verificações de extensões de arquivo
* Recurso de dependência aprimorado com a adição de duas expressões personalizadas: ":blank" para selecionar elementos com um valor vazio e :filled para selecionar os elementos com um valor, ambos excluindo espaços em branco
* Adicionado um método resetform () ao validador: Redefine cada elemento de formulário (usando o plug-in do formulário, se disponível), remove as classes em elementos inválidos e oculta todas as mensagens de erro
* Correção de documentos para validator.showErrors()
* Correção da criação de rótulo de erro para sempre usar html() em vez de text(), permitindo a passagem de HTML arbitrário como mensagens
* Correção de criação de rótulo de erro para usar classe de erro especificada
* Recurso de dependência adicionado: O método requires aceita String (expressões jQuery) e funções como o argumento
* Bastante aprimorada a personalização da exibição de mensagem de erro: Use mensagens normais e mostre/oculte um contêiner adicional; Substituir completamente a exibição de mensagem por um mecanismo próprio (enquanto pode delegar para o manipulador padrão; Personalizar a colocação de rótulos gerados (em vez do below-element padrão)
* Correção de dois bugs principais no IE (contêineres de erro) e Opera (metadados)
* Modificados os métodos de validação para aceitar os campos vazios como válido (exceção: de curso necessárias e também métodos equalto a fim)
* "min" renomeado para "minLength", "max" para "maxLength", "length" para "rangeLength"
* "minValue", "maxValue" e "rangeValue" adicionados
* API simplificada para oferecer suporte a diferentes eventos. O padrão, enviar, pode ser desabilitado. Se nenhum evento for especificado, ele será aplicado a cada elemento (em vez de todo o formulário). A combinação de keyup-validation com submit-validation agora é extremamente fácil de configurar
* Adição de suporte a uma mensagem por regra ao definir mensagens por meio de configurações do plug-in
* Adição de suporte para encapsular metadados em um elemento pai. Também será útil quando os metadados forem usados por outros plug-ins.
* Refatorado testes e demonstrações: Menos arquivos, demonstrações melhores
* Documentação aprimorada: Mais exemplos de métodos, mais textos explicando alguns aspectos básicos de referência
