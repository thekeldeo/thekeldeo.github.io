---
    layout: post
    title:  "Chia sẻ một số cấu hình nginx hữu ích"
    date:   2017-08-03 16:15:00 +0700
    categories: ops
---

Nginx là một web serve rất mạnh được dùng bởi cả youtube, facebook, ... Tuy nhiên việc cấu hình nginx không phải là một việc dễ dàng.
Ở bài post này, tôi sẽ chia sẻ một số config mà tôi thấy hữu ích.

![nginx vs apache](/assets/img/apache_nginx_tw.png)

## Trước khi bắt đầu, hãy làm quen một chút với nginx ##

Để cài nginx, các bạn hãy cài thông qua yum hay apt-get, ví dụ: 

```
sudo yum install nginx
```

- Khởi động nginx: ```sudo service nginx start```
- Dừng nginx: ```sudo service nginx stop```
- Restart: ```sudo service nginx restart``` hoặc ```sudo service nginx reload```

File config của nginx sẽ nằm ở ```/etc/nginx/conf.d/default.config``` và nginx sẽ log ở ```/var/log/nginx/```

Hãy mở ```default.config``` để tiếp tục nào và các file log của nginx sẽ giúp bạn khi có lỗi đó !

## Cấu hình nginx để tạo ứng dụng của bạn chạy ở cổng 80 ##

Ví dụ bạn muốn home page của bạn chạy http mặc định ở cổng 80 hãy làm như sau:

Tạo một upstream 
```
upstream homepage {
    server your_ip_server:your_port; 
    #example: 125.212.233.50:8080;
}

```
Sau đó sửa phần ```server``` như sau

```
server {

    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;
    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
        proxy_pass  http://homepage/;
    }
}

```

Save lại và restart để xem thành quả ~

## Cấu hình một ứng dụng chạy trên router con của trang chủ ##

Tôi có app chính chạy ở port 8080 --> ```http://yourdomain.com```. Tôi có thêm một app blog nữa chạy ở port 8088. Để App blog chạy ở router con  ```http://yourdomain.com/blog``` thì cấu hình như sau.

Tạo thêm một upstream nữa cho blog

```
upstream blog {
    server 125.212.233.50:8088; 
}

```

Ở phần ```server```, thêm location mới với path là router con mà bạn muốn :
```
server {

    listen 80 default_server;
    listen [::]:80 default_server ipv6only=on;
    #charset koi8-r;

    #access_log  logs/host.access.log  main;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
        proxy_pass  http://homepage/;
    }
    location /blog {        #ADD THIS
        proxy_pass  http://blog/;
        try_files $uri $uri/ /blog/;
    }
}

```

## Cấu hình để chạy app php ##

Để cấu hình cho php (chạy bằng ```php-cgi```) thêm một location nữa ở trong ```server```:

```
location ~ \.php$ {
    try_files $uri =404;
    root           /var/www/html;
    fastcgi_pass   127.0.0.1:9000;
    fastcgi_index  index.php;
    #fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
    fastcgi_split_path_info ^(.+\.php)(/.+)$;
    fastcgi_param   PATH_INFO         $fastcgi_path_info;
    fastcgi_param   SCRIPT_FILENAME   $document_root$fastcgi_script_name;
    include        fastcgi_params;
}
```

Cấu hình thêm một router con cho app php, tiếp tục sửa phần ```server``` nhé:


```
location /php {
    root /var/www/html;   #thư mục chứa project php của bạn
    index index.php index.html index.htm;
    try_files $uri $uri/ /php/index.php?$args;
}
```

Save và restart nhé.

## Cấu hình với chạy app trên subdomain ##

Bạn muốn app của mình chạy trên subdomain như ```http://blog.yourdomain.com``` hãy làm như sau: 

+ Tạo một bản ghi trên domain dashboard của bạn với giá trị như sau: 
    - **Host record**: tên sub domain, ở đây là ```blog```
    - **Address**: địa chỉ ip của server
    - **Record type**: ```A(Address)```
    - **TTL**: ```360```

+ Thêm đoạn code sau trong file config nginx: 

```
server {
    listen       80;
    server_name  blog.yourdomain.com;

    location / {
	proxy_set_header X-Forwarded-For $remote_addr;
        proxy_pass http://localhost:8088; # 8088 : port chạy app blog
    }
}
```

Xong Save và Restart nginx ~

## Cấu hình một domain mới chạy vào một app khác ##

Bây giờ nếu tôi muốn app blog chạy trên một domain mới như *```http://yournewdomain.com```*, làm như sau: 

- Tạo bản ghi mới trên dashboard của domain mới với giá trị như sau: 
    - **Host record**: ```@```
    - **Address**: địa chỉ ip của server
    - **Record type**: ```A(Address)```
    - **TTL**: ```360```
    
- Thêm một server mới trong file config nginx: 

````
server {
    listen       80;
    server_name  yournewdomain.com;

    location / {
        proxy_pass http://localhost:8088; #8088 là port của app blog
    }
}
````

Save và Restart nginx. 

*Lưu ý sẽ phải mất tầm 15p để DNS của domain nhận ip của server đó nhé*


### Trên đây là những kiến thức của mình với nginx, hy vọng sẽ giúp mọi người phần nào đó. Nếu có thắc mắc hãy liên hệ với mình nhé ###