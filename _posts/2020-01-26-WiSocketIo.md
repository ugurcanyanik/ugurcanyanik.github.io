---
title: Socket.io nedir?
author: Uğurcan Yanık
layout: post
date:   2020-01-26
tags: [Socket.io]
image: "socket.jpg"
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

```javascript
io.on('connection', function(socket){ //io.on parçacığı yakalanacak yerdir. yani emit ile gelecek veriyi yakalar.
  socket.on('disconnect', function(){ });
});
```

**Özel ad alanları** ise **of** kullanılarak yapılır ve özel bir isim kullanılarak bağlantı yapmamızı sağlar.

```javascript
const nsp = io.of('/my-namespace');
nsp.on('connection', function(socket){
  console.log('someone connected');
});
nsp.emit('hi', 'everyone!');
```

## Oda kavramı
Oda kavramı ise her ad alanları içerisinde tanımlayabileceğiniz özel bir alandır. Bu alana giriş çıkış yapılabilir.

**Odaya katılmak ve ayrılmak**
 **Join** yapısı ile odaya giriş yapabilirsiniz.

 ```javascript
 io.on('connection', function(socket){
  socket.join('some room');
});
 ```
 **to ya da in** yapısı kullanarak da yayın yapabilirsiniz.
 ```javascript
 io.to('some room').emit('some event');
 ```

 Bir kanaldan **ayrılmak yani leave** yapısını kullanmak **join** ile benzerdir. Her iki yöntem de asenkrondur ve **callback** kabul eder.

 **Varsayılan oda** ise Socket.io tarafından atanan ve tahmin edilemeyen yani benzersiz bir Socket kimliği atar. Kolaylık sağlanması için de her socket otomatik olarak bu kimlikle tanımlı odaya dahil edilir.
 ```javascript
 io.on('connection', function(socket){
   socket.on('say to someone', function(id, msg){
     socket.broadcast.to(id).emit('my message', msg);
   });
 });
 ```
 **Bağlantıyı koparmak** ise socketler bütün bağlantıları vs kendisi otomatik bırakır ve size yalnızca çıkmak kalır.

## Socket.io Yükleyelim
Bu kütüphaneyi kurmak aslında çok basittir.
```ruby
npm install express
npm install socket.io
```
konsolda bu npm satırları ile kurulum yapabilirsiniz.

## Node http server ile kullanımı
**Server (app.js)**
```javascript
var app = require('http').createServer(handler)
var io = require('socket.io')(app);
var fs = require('fs');

app.listen(80);

function handler (req, res) {
  fs.readFile(__dirname + '/index.html',
  function (err, data) {
    if (err) {
      res.writeHead(500);
      return res.end('Error loading index.html');
    }

    res.writeHead(200);
    res.end(data);
  });
}

io.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
  socket.on('my other event', function (data) {
    console.log(data);
  });
});
```
**İstemci (index.html)**
```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io('http://localhost');
  socket.on('news', function (data) {
    console.log(data);
    socket.emit('my other event', { my: 'data' });
  });
</script>
```

## Express ile kullanımı
Zaten express kurulumu da yaptık yukarıda ve örnek bir sayfa da aşağıdaki gibidir.
**Server (app.js)**
```javascript
var app = require('express')();
var server = require('http').Server(app);
var io = require('socket.io')(server);

server.listen(80);
// WARNING: app.listen(80) will NOT work here!

app.get('/', function (req, res) {
  res.sendFile(__dirname + '/index.html');
});

io.on('connection', function (socket) {
  socket.emit('news', { hello: 'world' });
  socket.on('my other event', function (data) {
    console.log(data);
  });
});
```
**İstemci (index.html)**
```html
<script src="/socket.io/socket.io.js"></script>
<script>
  var socket = io('http://localhost');
  socket.on('news', function (data) {
    console.log(data);
    socket.emit('my other event', { my: 'data' });
  });
</script>
```
-- --
Umuyorumki anlaşılabilir ve güzel bir anlatım olmuştur. Türkçe çok kaynak olmamasına karşın kendimce bir şeyler anlatmaya ve birilerine faydalı olmaya çalıştım. Faydalı olduğuna inanıyorsanız ya da değiştirmemi istediğiniz bir yer varsa lütfen bana ulaşın. Hepinize iyi çalışmalar diliyorum.

## Basit bir mesajlaşma uygulaması için ***[Tıklayınız](https://socket.io/get-started/chat/)***.
-- -- -- -- --
**> Kaynakça**

- ***[Socket.io](https://socket.io/)***
- ***[Çağatay Çalı](https://cagatay.me/node-js-ile-anl%C4%B1k-mesajla%C5%9Fma-uygulamas%C4%B1-yap%C4%B1m%C4%B1-5-c9d3e910a96f)***
