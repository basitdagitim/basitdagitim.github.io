Dağıtım Hazırlama
+++++++++++++++++

**initrd** hazırlama aşamaları **initrd** konu başlığında detaylıca anlatıldı.  Sistem hazırlanırken küçük farklılıklar olsada **initrd** hazırlamaya benzer aşamalar yapılacaktır. Sistemimin yani oluşacak **iso** dosyasının yapısı aşağıdaki gibi olacaktır. Aşağıda sadece **filesystem.squashfs** dosyasının hazırlanması kaldı.

.. code-block:: shell
	
	$HOME/distro/iso/boot/grub/grub.cfg
	$HOME/distro/iso/boot/initrd.img
	$HOME/distro/iso/boot/vmlinuz
	$HOME/distro/iso/live/filesystem.squashfs
	
**filesystem.squashfs Hazırlama**
---------------------------------

**filesystem.squashfs** dosyası **/initrd.img** dosyasına benzer yapıda hazırlanacak.
En büyük faklılık **init** çalışabilir dosya içeriğinde yapılmalı. Yapı **/initrd.img** dizin yapısı gibi hazırlandıktan sonra **filesystem.squashfs** oluşturulmalı ve **$HOME/distro/iso/live/filesystem.squashfs** konuma kopyalanmalıdır. Aşağıdaki komutlarla **filesystem.squashfs** hazırlanıyor ve  **$HOME/distro/iso/live/** konumuna taşınıyor.

.. code-block:: shell

	cd $HOME/distro/
	mksquashfs $HOME/distro/rootfs $HOME/distro/filesystem.squashfs -comp xz -wildcards
	mv $HOME/distro/filesystem.squashfs $HOME/distro/iso/live/filesystem.squashfs

.. raw:: pdf

   PageBreak

