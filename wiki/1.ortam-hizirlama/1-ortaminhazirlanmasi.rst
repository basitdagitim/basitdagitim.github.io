Dağıtım İçin Ortamın Hazırlanması
=================================

Dağıtım hazırlarken sistemin derlenmesi ve gerekli ayarlamaların yapılabilmesi için bir linux dağıtımı gerekmektedir.
Bu hangi dağıtım olacağına tecrübeli olduğunuz dağıtımı seçmenizi tavsiye ederim. Fakat seçilecek dağıtım Gentoo olması daha hızlı ve sorunsuz sürece devam etmenizi sağlayacaktır.
Bu dağıtımı hazırlaken Debian dağıtımı kullanıldı. Bazı paketle için özellikle bağımlılık sorunları yaşanan paketler için ise Gentoo kullanıldı.

Bir dağıtım hazırlamak için çeşitli paketler lazım. Bu paketler;

- debootstrap	: Dağıtım hazırlaken kullanılacak chroot uygulaması bu paket ile gelmektedir. chroot ayrı bir konu başlığıyla anlatılacaktır.
- make		: Paket derlemek için uygulama
- squashfs-tools	: Hazırladığımız sistemi sıkıştırılmış dosya halinde sistem görüntüsü oluşturmamızı sağlayan paket.
- gcc		: c kodlarımızı derleyeceğimiz derleme aracı.
- wget		: tarball vb. dosyaları indirmek için kullanılacak uygulama.
- unzip		: Sıkıştırmış zip dosyalarını açmak için uygulama
- xz-utils	: Yüksek sıkıştırma yapan sıkıştırma uygulaması
- tar		: tar uzantılı dosya sıkıştırma ve açma içiçn kullanılan uygulama.
- zstd		: Yüksek sıkıştırma yapan sıkıştırma uygulaması 
- grub-mkrescue : Hazırladığımız iso dizinini iso yapmak için kullanılan uygulama
qemu-system-x86	: iso dosyalarını test etmek ve kullanmak için sanal makina emülatörü uygulaması.


sudo apt update
sudo apt install debootstrap make squashfs-tools gcc wget unzip xz-utils tar zstd -y

Paket kurulumu yapıldıktan sonra kurulum için bir yeri(hedefi) belirlemelliyiz. Bu dokümanda sistem için kurulum dizini $HOME/rootfs olarak kullanacağız.

.. raw:: pdf

   PageBreak

