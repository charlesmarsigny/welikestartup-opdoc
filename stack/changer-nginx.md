# Changer Nginx





```text
sudo sh -c "echo 'deb http://download.opensuse.org/repositories/home:/rtCamp:/EasyEngine/xUbuntu_18.04/ /' > /etc/apt/sources.list.d/home:rtCamp:EasyEngine.list"
sudo wget -nv https://download.opensuse.org/repositories/home:rtCamp:EasyEngine/xUbuntu_18.04/Release.key -O Release.key
sudo apt-key add - < Release.key
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 3050AC3CD2AE6F03
sudo apt-get update

sudo apt-get remove nginx*
sudo apt-get install nginx-custom
```







Sources :

{% embed url="https://easyengine.io/wordpress-nginx/tutorials/single-site/fastcgi-cache-with-purging/" %}

{% embed url="https://software.opensuse.org/download.html?project=home%3ArtCamp%3AEasyEngine&package=nginx" %}

{% embed url="https://community.easyengine.io/t/repo-key-expired-error-for-easyengine-on-ubuntu-16-04/9227/10" %}



