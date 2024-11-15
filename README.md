## نصب پنل مرزبان و تمام جزییات و نکات روی سرور ابونتو نسخه 22 و تانل گاست بین سرور ایران و خارج
<br></br>
### آدرس گیت هاب مرزبان
    https://github.com/Gozargah/Marzban/blob/master/README-fa.md
### دستور نصب مرزبان :
    sudo bash -c "$(curl -sL https://github.com/Gozargah/Marzban-scripts/raw/master/marzban.sh)" @ install
### ساخت یوزر ادمین در مرزبان :
    marzban cli admin create –sudo
> [!NOTE]
### نکته : پنل مرزبان بدون سرتیفیکیت و تنها با ای پی سرور اجرا نمی شود.
<br></br>
### نصب سرتیفیکیت :
    apt-get install certbot -y
### گرفتن سرتیفیکیت : (اگر آدرس سابسکرایب با پنل متفاوت باشد باید برای دو دامنه سرتیفیکیت گرفت)
    certbot certonly --standalone --agree-tos --register-unsafely-without-email -d MyDomain
### کپی گرفتن از سرتیفیکیت و انتقال به مسیر مشخص در سرور
    mkdir /var/lib/marzban/certs
    cp /etc/letsencrypt/live/fi3.nitroband.ir/fullchain.pem /var/lib/marzban/certs/fullchain.pem
    cp /etc/letsencrypt/live/fi3.nitroband.ir/privkey.pem /var/lib/marzban/certs/privkey.pem
### مسیری که در فایل .env  باید جایگذاری شود : (آدرس فایل در /opt/marzban/ هست)
    /var/lib/marzban/certs/fullchain.pem
    /var/lib/marzban/certs/privkey.pem
### ریست کردن مرزبان بعد از انجام تغییرات :
    marzban restart
### _تا این مرحله پنل مرزبان با موفقیت نصب شده و با زدن آدرس دامنه و پورت 8000 می توان وارد پنل شد_    
    https://mydomain:8000/dashboard

### برای نصب اتوماتیک بک اپ گرفتن روی تلگرام از فایلهای مرزبان :
    https://github.com/AC-Lover/backup/blob/main/README-fa.md


### _مرحله اول : ایجاد ipv6 به صورت مجازی رو دو سرور_
### دستورات سرور ایران :
### ایجاد و ویرایش فایل سرویس:
    sudo nano /etc/systemd/system/private.service
### افزودن محتویات زیر به فایل : (به جای ip_iran و ip_kharej مقادیر مناسب جایگذاری کنید)
    [Unit]
    Description=My Custom Commands
    After=network.target
    [Service]
    ExecStart=/bin/bash -c "/sbin/ip tunnel add MR_KAKO mode sit remote ip_kharej local ip_Iran && /sbin/ip -6 addr add 0ac7:9180:0cac:db48:47cc::1/64 dev MR_KAKO && /sbin/ip link set MR_KAKO mtu 1280 && /sbin/ip link set MR_KAKO up" [Install]
    WantedBy=default.target
### فعال و شروع به کار کردن سرویس :
    sudo systemctl enable private
    sudo systemctl start private
### دستورات سرور خارج :
### ایجاد و ویرایش فایل سرویس :
    sudo nano /etc/systemd/system/private.service
### افزودن محتویات زیر به فایل : (به جای ip_iran و ip_kharej مقادیر مناسب جایگذاری کنید)
    [Unit]
    Description=My Custom Commands 2
    After=network.target
    [Service]
    ExecStart=/bin/bash -c "/sbin/ip tunnel add MR_KAKO mode sit remote ip_iran local ip_kharej && /sbin/ip -6 addr add 0ac7:9180:0cac:db48:47cc::2/64 dev MR_KAKO && /sbin/ip link set MR_KAKO mtu 1280 && /sbin/ip link set MR_KAKO up"
    [Install]
    WantedBy=default.target
### فعال و شروع به کار کردن سرویس :
    sudo systemctl enable private
    sudo systemctl start private
### آدرس گیت هاب تانل گاست :
    https://github.com/go-gost/gost
### _مرحله دوم : نصب گوست روی ایران (آخرین نسخه از گوست در گیت هاب در لینک زیر جا گذاری شود) :_
    wget -O /tmp/gost.tar.gz https://github.com/go-gost/gost/releases/download/v3.0.0-nightly.20241107/gost_3.0.0-nightly.20241107_linux_amd64v3.tar.gz
    tar -xvzf /tmp/gost.tar.gz -C /usr/local/bin/
    chmod +x /usr/local/bin/gost
### ادیت کردن سرویس گوست :
    sudo nano /usr/lib/systemd/system/gost.service
### دستورات : (در این قسمت باید پورت های مورد نظر برای تانل شدن را در متن زیر تغییر داد یا اضافه کرد)
    [Unit]
    Description=GO Simple Tunnel
    After=network.target
    Wants=network.target
    [Service]
    Type=simple
    Environment="GOST_LOGGER_LEVEL=fatal"
    ExecStart=/usr/local/bin/gost -L=tcp://:8080/[0ac7:9180:0cac:db48:47cc::2]:8080 -L=tcp://:8081/[0ac7:9180:0cac:db48:47cc::2]:8081 -L=tcp://:443/[0ac7:9180:0cac:db48:47cc::2]:443 -L=tcp://:2053/[0ac7:9180:0cac:db48:47cc::2]:2053 
    [Install]
    WantedBy=multi-user.target
### فعال و شروع به کار کردن سرویس :
    sudo systemctl enable gost 
    sudo systemctl start gost




> [!NOTE]
> Highlights information that users should take into account, even when skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]
> Crucial information necessary for users to succeed.

> [!WARNING]
> Critical content demanding immediate user attention due to potential risks.

> [!CAUTION]
> Negative potential consequences of an action.

