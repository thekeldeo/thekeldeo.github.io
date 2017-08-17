---
    layout: post
    title:  "Hướng dẫn SEO Web trong MEAN STACK"
    date:   2017-08-03 14:27:58 +0700
    categories: dev
---

MEAN stack là một combo tuyệt vời sử dụng SPA và client rendering, rất phù hợp với webapp cho admin, staff ,...
Nhưng vấn đề khá lớn của Mean Stack nói riêng và những framework client rendering nói chung đó là rất khó để những con bot như google có thể crawl được dữ liệu.

Hôm nay tôi sẽ hướng dẫn mọi người khắc phục vẫn đề này một phần nào đó !.

![](/assets/img/full-stack-developer.jpg)

## Vấn đề ##

**Client rendering** tức là **server** sẽ chỉ trả lại cho trình duyệt các file dữ liệu, việc thực thi các file này để render ra trang web là công việc của browsers. Điều này khá là tuyệt, sẽ giảm bớt công việc cho server của bạn, nhưng đồng thời làm những công cụ crawl dữ liệu như google sẽ chỉ nhận được các dòng javascript của bạn thay vì html hoàn chỉnh.
 
Nếu các bạn search trang web bằng angularjs của mình trên google có thể các bạn sẽ thấy google đọc description hay title của web thành ```{ { page_title } }``` , ```{ {page_description} }``` thay vì nội dung được render khi gọi api từ server về.
 
![angular-seo-problem](/assets/img/angular-seo/Screen%20Shot%202017-08-03%20at%2014.46.42.png)
 
## Giải pháp ##

Thật ra đây không phải là một cách tối ưu nhất, nếu bạn muốn chú trọng về seo, tốt nhất hãy sử dụng các framework rendering server, nhưng trong trường hợp này 
có thể khắc phục được khá nhiều đó. Chúng ta sẽ sử dụng [**Prerender.io**](https://prerender.io), một framework cho phép chúng ta cache lại web đã được render từ trước, để khi các bot crawl data sẽ lấy được data đã được render.

*Note: Prerender chỉ free cache lại 200 page đầu tiên thôi, nên nếu web bạn có nhiều hơn thì sẽ phải trả tiền đó.*

## 1. Cấu hình trên server ##

### Express ###

Hãy cài package của prerender trên project:

```npm install prerender-node --save```

Và settup express app: 

```app.use(require('prerender-node').set('prerenderToken', 'YOUR_TOKEN'));```


```$xslt
// server.js

var express = require('express');

var app = module.exports = express();

app.configure(function(){ 
  // Here we require the prerender middleware that will handle requests from Search Engine crawlers 
  // We set the token only if we're using the Prerender.io service 
  app.use(require('prerender-node').set('prerenderToken', 'YOUR-TOKEN-HERE')); 
  app.use(express.static("public")); app.use(app.router); 
});

// This will ensure that all routing is handed over to AngularJS 
app.get('*', function(req, res){ 
  res.sendfile('./public/index.html'); 
});

app.listen(8081); 
console.log("Go Prerender Go!");
```

### Nginx ###

Cấu hình lại file config và restart lại nginx. Bạn có thể config theo file [này](https://gist.github.com/thoop/8165802)

### Apache ###

Nếu bạn sử dụng Apache, hãy config file ```.htaccess``` theo file [này](https://gist.github.com/thoop/8072354) và restart lại apache.

## 2. Cấu hình ở client ##

Rất đơn giản, hãy thêm dòng code sau ở thẻ *head* ở file ```index.html```

```
<meta name="fragment" content="!">
```

Thẻ này sẽ làm cho các search engine nhận ra đây là một web có nội dung dynamic bằng javascript.

Sau khi hoàn thành config mọi thứ xong đây là quy trình sẽ crawl web của bạn: 

- Bot sẽ craw web của bạn ở url *http://localhost:3000/#!/home*
- Url sẽ được chuyển thành *http://localhost:3000/?<em>escaped_fragment</em>=/home*
- Prerender sẽ check nếu cache của địa chỉ này đã được lưu lại hay chưa, nếu có, nó sẽ gửi cho bot crawl, nếu không nó render và cache lại sau đó gửi cho bot crawl

*Note: Nếu bạn sử dụng html5mode, thì url từ ``` http://localhost:3000/home``` sẽ được chuyển thành ```http://localhost:3000/home/?<em>escaped_fragment</em>=```*

## 3. Kiểm tra mọi thứ đã hoạt động ổn định ##

Bạn có thể vào một page bất kì nào trên trang web của bạn và thêm param *```?<em>escaped_fragment</em>=```* để kiểm tra, ```ctrl + u``` để view source trang. Nếu thấy data hml được render thì chúc mừng  bạn đã thành công rồi đó.

Bạn cũng có thể vào dashboard của prerender để kiểm tra: 

![angular-seo](/assets/img/angular-seo/Screen%20Shot%202017-08-03%20at%2015.56.50.png)

Bạn cũng có thể thêm các param trong dashboard để khiên prerender không crawl những trang đó

![angular-seo](/assets/img/angular-seo/Screen%20Shot%202017-08-03%20at%2015.54.07.png)