[1]: https://rosettacode.org/wiki/FTP

# [FTP][1]

```raku
use Net::FTP;
 
my $ftp = Net::FTP.new(:host('mirrors.sohu.com'), :passive);
 
$ftp.login();
 
say $_<name>for $ftp.ls();
 
$ftp.get( 'index.html', :binary );
```

#### Output:
```
CPAN
FOOTER.html
FreeBSD
HEADER.html
OpenBSD
anthon
apache
archlinux
centos
cygwin
dag
debian
debian-backports
debian-cd
debian-multimedia
debian-security
deepin
deepin-cd
fedora
fedora-epel
gentoo
help
images
index.html
mysql
nginx
opensuse
php
python
qt-all
raspbian
rsync
ubuntu
ubuntu-cn
ubuntu-releases
```