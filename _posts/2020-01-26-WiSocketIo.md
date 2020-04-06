---
title: Socket.io nedir?
author: Uğurcan Yanık
layout: post
date:   2020-01-26
tags: [Socket.io]
image: "socket.jpeg"
---
İlk yazım olması sebebi ile biraz heyecanlıyım aslında. Hem boş zamanımı değerlendirdiğimi düşünüyorum hem de bu sıkıcı tatil gününmü de dolu geçirmek için uğraşmak hoşuma gidiyor aslında. Bugün Socket.io hakkında biraz konuşmak istiyorum. Yapabileceğimiz bir sürü projenin olduğu ve çalışma stilini de çok sevdiğim bir kütüphanedir. Aslında Socket.io ile tanışmam bir arkadaşım sayesinde oldu. Onun websitesini de kaynakçama ekleyeceğim. İncelemenizi kesinlikle öneririm.

Haydi başlayalım artık 😛, nedir bu Socket.io?

## Bu Socket.io nedir? Ne işe yarar?
Socket.io bir tarayıcı sayfası ile arka planda çalışan sunucunuzun arasında olan ve sürekli bir döngüde olan; çift taraflı ve bir olay tabanlı kütüphanedir. Yani bir **HTML** sayfası ile sunucuyu, örneğin **JS** sayfası düşünelim. Tarayıcı sayfamızdaki olan olaylarda anlık olarak arka planda bir işlem yaptırmak ya da bu sayfayı kullanan tüm kullanıcılarla iletişime geçmek isteyebiliriz. Bu gibi durumda çok basit bir işlem ile anlık olarak arka plan ile haberleşmemizi sağlayan bir yapı karşımıza çıkıyor. **Socket.io** sunucu ile bize bir çift yönlü köprü sağlıyor. Tabiki bu köprü bazen kopabiliyor. Ama başta da dediğim gibi bu bir döngü ve sürekli yeniden bağlanmaya çalışacaktır. Bu sebeple de gerçek zamanlı diye adlandırıyoruz.

<p align="center">
	<img style="max-width: 100%; height: auto;"  alt="responsive-gif" src="https://miro.medium.com/max/812/1*7xzCJROjOV6AiaSKTmmlBg.jpeg" width="650">
</p>

## Ad Alanları yani NameSpaces nedir? Oda kavramı ne işe yarar?
Ad alanları aslında dünya üzerindeki mahalleler gibi düşünülebilir. Her mahallenin kendi adı olmalıdır. Bu alan adları içerisinde ise özel odalar olabilir. Yani apartmanlar. Bu apartmanların da kendi ismi olabilir ve bu apartman sakinlerine özel mesajlar yollanabilir. Bu odaya yani apartmana katılmak ve ayrılmak isteyebilirsiniz. Bu kanal size, özel bir mesaj ya da istek yollamanıza olanak sağlar. Tabiki bu odada yer alıyorsunuz diye tek bir apartmana bağlı kalmanıza gerek yok. Diğer apartmanlara yani dış dünyaya da mesaj yollayabilirsiniz. Hemen bakalım nasıl oluyor bu **Socket.io** işlemleri;

**Varsayılan ad alanları** ise **/** ile otomatik olarak karşılıklı default bir bağlantı sağlandığını gösterir.

<script src="https://gist.github.com/ugurcanyanik/ef5e4b6e30ac28781f3e9ba996a273d7.js"></script>

Burada ise her bağlantıda **socket** parametreli bir olayı yayımlar.

<script src="https://gist.github.com/ugurcanyanik/25cb7400a0ee2f2a378d2ad230bb8f67.js"></script>

**Özel ad alanları** ise **of** kullanılarak yapılır ve özel bir isim kullanılarak bağlantı yapmamızı sağlar.

<script src="https://gist.github.com/ugurcanyanik/fac6137af1b6da6bae03201f8947e169.js"></script>

## Oda kavramı
Oda kavramı ise her ad alanları içerisinde tanımlayabileceğiniz özel bir alandır. Bu alana giriş çıkış yapılabilir.

**Odaya katılmak ve ayrılmak**
 **Join** yapısı ile odaya giriş yapabilirsiniz.

<script src="https://gist.github.com/ugurcanyanik/849a24ba870385feffb49a450dc933d6.js"></script>
 **to ya da in** yapısı kullanarak da yayın yapabilirsiniz.
<script src="https://gist.github.com/ugurcanyanik/bca1c02711df688129cf54ae59207a27.js"></script>

 Bir kanaldan **ayrılmak yani leave** yapısını kullanmak **join** ile benzerdir. Her iki yöntem de asenkrondur ve **callback** kabul eder.

 **Varsayılan oda** ise Socket.io tarafından atanan ve tahmin edilemeyen yani benzersiz bir Socket kimliği atar. Kolaylık sağlanması için de her socket otomatik olarak bu kimlikle tanımlı odaya dahil edilir.
<script src="https://gist.github.com/ugurcanyanik/0d861ac1281a7635e9d7addb27b860c2.js"></script>

 **Bağlantıyı koparmak** ise socketler bütün bağlantıları vs kendisi otomatik bırakır ve size yalnızca çıkmak kalır.

## Socket.io Yükleyelim
Bu kütüphaneyi kurmak aslında çok basittir.

<script src="https://gist.github.com/ugurcanyanik/7a9bd4292506b0d855a8c9eeca93b336.js"></script>

konsolda bu npm satırları ile kurulum yapabilirsiniz.

## Node http server ile kullanımı
**Server (app.js)**

<script src="https://gist.github.com/ugurcanyanik/a08bb1cf79486b4e5d621b5ab5692a31.js"></script>

**İstemci (index.html)**

<script src="https://gist.github.com/ugurcanyanik/b3c3d99191919151192d6e328b5c11ef.js"></script>

## Express ile kullanımı
Zaten express kurulumu da yaptık yukarıda ve örnek bir sayfa da aşağıdaki gibidir.
**Server (app.js)**
<script src="https://gist.github.com/ugurcanyanik/716bec7e36b471a63ee3b65cfdd13ef6.js"></script>
**İstemci (index.html)**
<script src="https://gist.github.com/ugurcanyanik/a878d2f12db98728d19b215760bcc88f.js"></script>
-- --
Umuyorumki anlaşılabilir ve güzel bir anlatım olmuştur. Türkçe çok kaynak olmamasına karşın kendimce bir şeyler anlatmaya ve birilerine faydalı olmaya çalıştım. Faydalı olduğuna inanıyorsanız ya da değiştirmemi istediğiniz bir yer varsa lütfen bana ulaşın. Hepinize iyi çalışmalar diliyorum.

## Basit bir mesajlaşma uygulaması için ***[Tıklayınız](https://socket.io/get-started/chat/)***.
-- -- -- -- --
**> Kaynakça**

- ***[Socket.io](https://socket.io/)***
- ***[Çağatay Çalı](https://cagatay.me/node-js-ile-anl%C4%B1k-mesajla%C5%9Fma-uygulamas%C4%B1-yap%C4%B1m%C4%B1-5-c9d3e910a96f)***

<a href="https://twitter.com/share" class="twitter-share-button" data-url="" data-size="large" data-count="none"></a>
<script>!function (d, s, id) {
var js, fjs = d.getElementsByTagName(s)[0]; if (!d.getElementById(id)) {
js = d.createElement(s); js.id = id;
js.src = "//platform.twitter.com/widgets.js";
 fjs.parentNode.insertBefore(js, fjs);
}
}(document, "script", "twitter-wjs");
</script>
