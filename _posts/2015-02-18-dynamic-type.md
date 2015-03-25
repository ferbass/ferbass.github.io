---
layout: post
title: ! 'Dynamic Type'
description: Com a chegada do iOS 7 foi introduzido uma opção de acessibilidade onde o usuário pode configurar o tamanho da fonte padrão do sistema para melhorar sua leitura, essa opção é conhecida como Dynamic Type, vamos conhecer para que serve e como podemos aplicar isso em nossos aplicativos.
keywords: Swift, Dynamic Type, iOS 7, iOS 8, iodevspot, ferbass, UILabel, NSNotificationCenter, Objective-C
feature-img: "img/posts/dynamic-types/featured.png"
author: ferbass
categories:
- ios
- Objective-C
- Swift
tags:
- UIFont
- Guidelines
- Dynamic
- iOS
- Swift
- Objective-C
status: publish
type: post
published: true
---

A partir do iOS 7 novas opções de usabilidade chegaram ao OS e uma delas é o Dynamic Type, onde o usuário pode ajustar o tamanho padrão da fonte do sistema para melhor leitura. 
Essas opções podem ser vistas em: **`Ajustes > Acessibilidade > Texto Maior`**

<center>
<img src="/img/posts/dynamic-types/IMG_1720.PNG" alt="Ajustes" width="20%" height="20%">
<img src="/img/posts/dynamic-types/IMG_1721.PNG" alt="Dynamic Type" width="20%" height="20%">
<img src="/img/posts/dynamic-types/IMG_1722.PNG" alt="Texto Maior" width="20%" height="20%">
</center>


Legal, né? Mas o que isso influencia quando estamos desenvolvendo nossos aplicativos?

Bem, por padrão nossos aplicativos não implementam o Dynamic Type em seus componentes, com excessão da `UITableView` quando utilizada com as celulas padrões do sistema.

<center>
<img src="/img/posts/dynamic-types/tableview1.png" alt="TableView" width="20%" height="20%">
<img src="/img/posts/dynamic-types/tableview2.png" alt="Ajuste Dynamic Type" width="20%" height="20%">
<img src="/img/posts/dynamic-types/tableview3.png" alt="TableView Dynamic Type" width="20%" height="20%">
</center>

Se você não está muito preocupado com a `typografia` de seus aplicativos e quer implementar o `Dynamic Type` por padrão em todos os seus componentes, você pode fazê-lo através do `Storyboard` sem nenhuma dificuldade, basta você adicionar o componente desejado, por exemplo um `UILabel` e em seguida em suas opções de fonte selecionar os tipos dinâmicos como você pode ver na imagem abaixo:

<center>
<img src="/img/posts/dynamic-types/label_dynamic1.png" alt="UILabel" width="20%" height="20%">
<img src="/img/posts/dynamic-types/label_dynamic2.png" alt="UILabel Ajuste Dynamic Type" width="20%" height="20%">
</center>

Fazendo isso, toda vez que seu aplicativo ou a `View` for recarregada ele vai ler as preferências do usuário para saber se as preferências de font foram alteradas para aplicar em seu layout.
Uma coisa importante para observar é quando estamos selecionando o `Text Styles` via `Storyboard`, é que existem 6 tipos `constants` para trabalhar, vamos ver na tabela abaixo quais são esses tipos e o que eles representam.


| Constant        | Uso           |
| ------------- |:-------------:|
| [UIFontTextStyleHeadline](http://goo.gl/3dL1z2)     | Utilizado para titulos |
| [UIFontTextStyleSubheadline](http://goo.gl/qMZpco)  | Utilizado para sub titulos     |
| [UIFontTextStyleBody](http://goo.gl/5w6x86)         | Utilizado para texto de conteudo      |
| [UIFontTextStyleFootnote](http://goo.gl/1mnywl)     | Utilizado para textos de rodapé |
| [UIFontTextStyleCaption1](http://goo.gl/C8QWPY)     | Utilizado para textos em geral |
| [UIFontTextStyleCaption2](http://goo.gl/KMM8TD)     | Utilizado para textos em geral |

Seguindo essa tabela você tem uma visão melhor de onde e qual tipo você deve utilizar para melhor adaptar o `Dynamic Type` em seu aplicativo. Mas não termina por ai, o que vimos até agora foi só uma ideia do que seria o `Dynamic Type` e uma forma simples de ver esse sistema em ação, agora vamos ver como utilizados em cenários reais do nosso dia-a-dia.

---

Primeiro ponto, vamos ver como utilizar o `Dynamic Type` programaticamente e entender seu comportamento.

Imagine um cenário onde você tem um `UILabel` e quer que ele adote o `Dynamic Type` e ao mesmo tempo ajuste o tamanho em qualquer momento que o usuário altere o tamanho da fonte, sem ter que forçar o aplicativo ou a `UIView` recarregar como comentei acima,
nesse caso temos que pensar em 2 pontos:

1. Defirnir em nosso `UILabel` qual constant do `Dynamic Type` ele vai implementar
2. Identificar quando o usuário alterar o tamanho da font sem ter que recarregar o aplicativo ou a `UIView`

O primeiro caso é bem simples de resolver, podemos definir qual tipo vamos utilizar em nosso `UILabel` via `Storyboard` ou programaticamente, vamos ver como fazer isso programticamente.

vamos definir um label com `UIFontTextStyleBody`

Código em Objective-C

<script src="https://gist.github.com/ferbass/0f249460fdee1ca6d705.js"></script>

Código em Swift

<script src="https://gist.github.com/ferbass/d77242d8b20416f45e46.js"></script>

Com isso resolvemos o primeiro problema, agora precisamos atualizar o tamanho do texto de acordo com a preferência do usuário sem ter que recarregar o aplicativo ou a `UIView`, para isso vamos implementar uma `Notification` que ficará escutando a notificação do sistema `UIContentSizeCategoryDidChangeNotification`

Código em Objective-C

Coloque esse código no seu ViewDidLoad

<script src="https://gist.github.com/ferbass/d11fc82d18eb06701c70.js"></script>

implemente esse método que vai responder a notifação definida acima

<script src="https://gist.github.com/ferbass/09515c8c865dd4ae55c4.js"></script>

Código em Swift

Coloque esse código no seu ViewDidLoad

<script src="https://gist.github.com/ferbass/9705e0d2120c8c4ad452.js"></script>

implemente esse método que vai responder a notifação definida acima

<script src="https://gist.github.com/ferbass/fab00e7e3da33f5656df.js"></script>

Seguindo o exemplo acima, quando o usuário alterar a preferência de font no ajuste vai disparar a notificação do sistema `UIContentSizeCategoryDidChangeNotification` para qual criamos o `NSNotificationCenter` que fica escutando esse evento,
feito isso ele vai disparar o método `didReceiveUIContentSizeCategoryDidChangeNotification` que por usa vez vai atualizar todos os elementos definidos que utilizam o `Dynamic Type`, lembrando que você precisa passar novamente os atributos na notificação.

---

Outro ponto muito importante é que dificilmente vamos utilizar as fontes padrões do sistema e vamos utilizar fontes que melhor representam a identidade de nossa aplicação, para isso precisamos definir alguns pontos.

Para isso uma das soluções é definir o tamanho da fonte a ser utilizada de acordo com o `pointSize` da constant utilizada, uma das maneiras de fazer isso é utilizar um pequeno truque seguindo o exemplo abaixo:

Código em Objective-C

<script src="https://gist.github.com/ferbass/7052c2c24102892999db.js"></script>

Código em Swift

<script src="https://gist.github.com/ferbass/62eec96021fd2b8e1b93.js"></script>

Utilizando o código acima você consegue implementar o `Dynamic Type` utilizando a fonte de sua preferência.

---

Você pode fazer download do exemplo de `Dynamic Type` em `Swift` no link [https://github.com/ferbass/dynamic-type](https://github.com/ferbass/dynamic-type)

* [Documentação oficial da Apple](https://developer.apple.com/library/ios/documentation/StringsTextFonts/Conceptual/TextAndWebiPhoneOS/CustomTextProcessing/CustomTextProcessing.html#//apple_ref/doc/uid/TP40009542-CH4-SW74)

Espero que tenham gostado do conteúdo do post e em breve mais material e novidades.

Grande abraço.
