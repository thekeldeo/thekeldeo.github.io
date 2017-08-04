---
    layout: post
    title:  "8 NPM trick sẽ hữu dụng cho bạn!"
    date:   2017-08-03 11:30:00 +0700
    categories: trick dev
---

8 trick npm bạn có thể sử dụng để gây ấn tượng với đồng nghiệp của mình.

Khi bạn nhìn những người đồng mình của mình code, bạn có thể thấy họ sử dụng phím tắt hoặc một số thủ thuật trông rất _cool ngầu_ và rất muốn học theo.

Trong bài này, Chúng ta sẽ tìm hiểu một số trick rất hữu ích với npm. Có quá nhiều thứ để chúng ta có thể biết, vậy nên tôi sẽ tập trung vào những thủ thuật hữu ích và gần gũi nhất thường thấy với các lập trình viên.

## Một số cú pháp ngắn trước khi chúng ta bắt đầu ##

Để tất cả mọi người đều hiểu, đặc biệt là những người mới sử dụng npm, hãy tìm hiểu tổng quan về cú pháp tắt và đảm bảo không ai bỏ lỡ những thứ đơn giản này: 

### Cài đặt một package: ###
 ```npm install pkg```, Tắt: ```npm i pkg```

### Cài đặt một package cục bộ: ###
```npm i --global pgk```, Tắt: ```npm i –g pkg```

### Cài đặt một package và lưu nó vào dependency: ###
```npm i –save pkg```, Tắt: ```npm i –S pkg```

### Cài đặt một package và lưu nó vào devDependency: ###
```npm i –save-dev pkg```, Tắt: ```npm i –D pkg```

Để tìm hiểu thêm về các cú pháp viết tắt khác hãy đọc [npm shorthand list](https://docs.npmjs.com/misc/config#shorthands-and-other-cli-niceties).

Hãy bắt đầu với những điều thú vị ngay bây giờ.

## 1. Khởi tạo package mới ##
Chúng ta đều biết ```npm init```, là dòng lệnh đầu tiên để tạo ra một package mới.

![](https://cdn-images-1.medium.com/max/1600/1*KUNw_1A1fsRr9NmT3LwYYw.gif)

Nhưng, tất cả câu hỏi gây khác nhiều khó chịu và chúng ta sẽ sửa nó trong tương lai, vậy tại sao không bỏ qua chúng?

```npm init –y``` và ```npm init –f``` là câu trả lời cho bạn

![](https://cdn-images-1.medium.com/max/1600/1*ulJ_vkyVLK1XJV8vQACY5w.gif)

## 2. Chạy test ##
 Câu lệnh chúng ta sẽ sử dụng đó là ```npm test```. Hầu hết chúng ta sẽ sử dụng chúng mỗi ngày, nhiều lần trong một ngày.
 
![](https://cdn-images-1.medium.com/max/1600/1*tIo4abpF5oV20lEED8C7Aw.gif)

Nếu  tôi bảo bạn có thể thực hiện câu lệnh test với ít hơn 40% kí tự thì sao? Chúng ta sẽ sử dụng câu lệnh test rất nhiều, vậy nên nó rất hữu ích.
Câu lệnh ```npm t```, sẽ làm đúng như vậy.

![](https://cdn-images-1.medium.com/max/1600/1*_KEOMP_Ht8_1YBCAfpRQXg.gif)

## 3. Danh sách những câu lệnh có thể thực hiện ##
Chúng ta có một project mới và không biết làm cách nào để bắt đầu. Chúng ta thường sẽ tự thắc mắc một số vấn đề như: Làm thế nào để tôi chạy đc nó? Những câu lệnh nào có thể dùng?

Và một cách làm việc đó là mở file ```package.json``` và kiểm tra phần scripts.

![](https://cdn-images-1.medium.com/max/1600/1*nssr6lw0OPnTTeT0SAzlyw.gif)

Chúng ta có thể làm việc đó tốt hơn, chỉ đơn giản chạy ```npm run``` và sẽ lấy được danh sách các câu lệnh có thể dùng.

![](https://cdn-images-1.medium.com/max/1600/1*0BIxAq0Q8B2J26Gyf9nvFg.gif)

Một cách khác đó là cài packge ```ntl``` (```npm i –g ntl```), và chạy ```ntl``` ở folder của project. Nó cũng sẽ cho phép chạy những câu lệnh, sẽ tiện hơn rất nhiều. 

![](https://cdn-images-1.medium.com/max/1600/1*XjqtKLbzaIkUwSXPL92Mtg.gif)

## 4. Danh sách những package đã cài ##

Giống với những câu lệnh có thể sử dụng, chúng cũng thường thắc mắc những package dependency nào đã có trong project. 
Chúng ta có thể mở file ```package.json``` và kiểm tra, nhưng có thể làm nó đơn giản hơn.

```
Npm ls –depth 0
```

![](https://cdn-images-1.medium.com/max/1600/1*MEg_C4RKrXQuh_RqgW9Whg.gif)

Để kiểm tra các package cục bộ, hãy chạy lệnh tương tự và thêm ```–g```

```
npm ls –g –depth 0
```

![](https://cdn-images-1.medium.com/max/1600/1*3t0VjKip9sWm9symlyrtig.gif)

## 5. Chạy các chương trình cục bộ ##

Khi chúng ta cài một package lên project, nó sẽ đi theo một câu lệnh, nhưng nó chỉ hoạt động khi chúng ta chạy qua câu lệnh ```npm```. Bạn có từng thắc mắc tại sao, và làm cách nào để vượt qua nó?

Đầu tiên, chúng ta phải hiểu khi chúng ta thực hiện một câu lệnh ở terminal ( cmd ), thì giống với việc chạy một chương trình với cùng tên ở mọi nơi trong đường dẫn mà đã được khai báo ở ```PATH enviroment variable```. Đó là lý do bạn có thể thực hiện nó ở bất cứ đâu. Các package cục bộ đăng kí câu lệnh của chúng cục bộ, vậy nên chúng không được liệt kê ở ```PATH``` và sẽ không tìm thấy.

Vậy chúng hoạt động thế nào khi chúng ta thực hiện qua câu lệnh npm ? Câu hỏi hay đó !Bởi vì khi chúng chạy bằng cách này, npm sẽ thực hiện một việc là thêm thư mục package đó trong PATH, <project-directory>/node_modules/.bin

Bạn có thể thấy nó bằng việc chạy lệnh ```npm run env | grep “$PATH”```. Bạn cũng có thể chỉ cần chạy lệnh ```npm run env``` để thấy tất cả các biến môi trường khả dụng, npm sẽ thêm một số thứ thú vị đó.

```Node_modules/.bin```, nếu bạn thắc mắc, thì nó chính xác là nơi các package cục bộ thay thế câu lệnh chạy của chúng.

Hãy chạy ```./node_modules/.bin/mocha``` trong thư mục project để thấy nó hoạt động.

![](https://cdn-images-1.medium.com/max/1600/1*5EaZSvL7T0aDCGGDB4GYjA.gif)

Đơn giản nhỉ? Chỉ cần chạy ```./node_modules/.bin/<command>``` ở bất cứ đâu khi bạn muốn chạy một package cục bộ.

## 6. Tìm package của bạn trên internet ##

Sẽ có lúc bạn tìm trong ```repository``` và ở file ```package.json``` sẽ thấy một đống package lạ và thắc mắc: “package này thích hợp cho việc gì?”.

Để trả lời câu hỏi này, hãy chạy ```npm repo``` và đợi nó tự mở trình duyệt của bạn.

Cũng như vậy, để vào ```homepage``` của ```package``` đó hãy chạy ```npm home```.

Nếu bạn muốn mở package của bạn trên [npmjs.com](https://npmjs.com), có một câu lệnh tắt , npm docs

## 7. Chạy câu lệnh trước và sau câu lệnh khác. ##
Bạn có thể quen thuộc với các câu lệnh như ```pretest```, để bạn có thể định nghĩa code sẽ chạy trước câu lệnh ```test```.

Bạn có thể ngạc nhiên khi tìm ra, rằng có thể có câu lệnh trước và sau cho tất cả các câu lệnh, kể cả câu lệnh custom của bạn.

![](https://cdn-images-1.medium.com/max/1600/1*T29YCsfoUA-nauh0mPXvMA.gif)

Nó sẽ rất hữu ích cho project nếu bạn sử dụng ```npm``` làm công cụ build và có quá nhiều câu lệnh cần sắp xếp.

## 8. Thay đổi version package. ##
Bạn có một package, bạn muốn thay đổi version của nó, bạn cần một version mới trước khi release.

Một cách để làm việc này đó là mở file ```package.json``` và chỉnh tay version, nhưng chúng ta ở đây không cần phải làm như vậy

Một cách đơn gỉan hơn là chạy npm version với ```major```, ```minor``` hoặc ```patch```.

![](https://cdn-images-1.medium.com/max/1600/1*pC68Ok-wR4Wm0HdWpQlCrg.gif)

Đó là tất cả.
Tôi hy vọng bạn đã học được thức gì mới và tìm thấy ít nhất một trong số trick trên hữu ích cho công việc hàng ngày của bạn, và lý tưởng bạn cũng biết rõ npm hơn và có một số ý tưởng cho cách bạn sử dụng chúng tốt hơn trong công việc.

Tạo ấn tượng cho đồng nghiệp của bạn rất tốt, nhưng học được gì mới và trở nên chuyên nghiệp  thậm chí còn tốt hơn nhiều.