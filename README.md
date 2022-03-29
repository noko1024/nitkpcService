## Nginx-ReverseProxy
[こちら](https://github.com/nginx-proxy/acme-companion)のイメージを利用しました。


## 環境変数
以下のものを環境変数として  
自身のコンテナに渡すかdocker-composeに記述してください。

### 必要
1. VIRTUAL_HOST=subdomain.example.com
2. LETSENCRYPT_HOST=subdomain.example.com

### 推奨
1. VIRTUAL_PORT=8080
2. LETSENCRYPT_EMAIL=Administrator@example.com

#### 変数の説明
> VIRTUAL_HOSTがリバースプロキシ側
> LETSENCRYPT_HOSTが証明書発行側を制御します
> それぞれの値は必ず名前解決が可能でなければなりません
> 到達できない場合証明書は発行されません

> VIRTUAL_PORTはプロキシサーバーが  
> どのポートに対してリクエストを転送するか決定します
> 明示しない場合**80**番に対してリクエストを転送します

> LETSENCRYPT_EMAILは証明書を発行する際に必要なメールアドレスを指定します
> 明示しない場合[docker-compose.yml](docker-compose.yml)内の**DEFAULT_EMAIL**を使用します。

## 例
```yml
#docker-compose.yml

MyHP:
    container_name: MyHomePage
    image: nginx:latest
    restart: always
    ports:
        - "8080:80"
    environment:
        - VIRTUAL_HOST=myhp.nitkpc.com
        - VIRTUAL_PORT=8080
        - LETSENCRYPT_HOST=my.hpnitkpc.com
    volumes:
        - ./HP/dist:/var/www/html
        - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
        - ./nginx/nginx.conf:/etc/nginx/conf.d/server.conf
```