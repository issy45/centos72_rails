# rails環境をansibleで書きました。

## 内容
- centos7.2
- Apache2.4
- rbenv
- MySQL5.7

立ち上げ直後、rbenveによりruby2.4.0がインストールされるようになっています。  
また、rails, bundler, passengerがgemで自動でインストールされます。

## 使い方
git cloneしてVagranfileのローカルipの設定をしたら「vagrant up」するだけです。

## vagrant up後の作業

### /home/vagrantにアプリを設置したい。
apacheがファイルにアクセスするために、

```
chmod 755 /home/vagrant
```
を実行。

### passengerの設定

```
passenger-install-apache2-module
```

Rubyを選択してひたすらEnter。
最後に出てくる
```
   LoadModule passenger_module /home/vagrant/.rbenv/versions/2.4.0/lib/ruby/gems/2.4.0/gems/passenger-5.1.2/buildout/apache2/mod_passenger.so
   <IfModule mod_passenger.c>
     PassengerRoot /home/vagrant/.rbenv/versions/2.4.0/lib/ruby/gems/2.4.0/gems/passenger-5.1.2
     PassengerDefaultRuby /home/vagrant/.rbenv/versions/2.4.0/bin/ruby
   </IfModule>
   ```
   みたいなやつを`/etc/httpd/conf.d/passenger.conf`を新しく作成して記入して保存。


   ### Apacheの設定

virtualhostの設定が必要。
`/etc/httpd/conf/vhost.conf`を作成して

```
<VirtualHost *:80>
    ServerName test.jp
    RailsEnv development
    # Be sure to point to 'public'!
    DocumentRoot /home/vagrant/sample/public
    <Directory /home/vagrant/sample/public>
        # Relax Apache security settings
        AllowOverride all
        Require all granted
        # MultiViews must be turned off
        Options -MultiViews
    </Directory>
</VirtualHost>
```

的なかんじのものを記述する。
