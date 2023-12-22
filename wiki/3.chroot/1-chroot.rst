Chroot Nedir?
=============

chroot komutu çalışan sistem üzerinde belirli bir klasöre root yetkisi verip sadece o klasörü sanki linux sistemi gibi çalıştıran bir komuttur. Sağladığı avantajlar çok fazladır. Bunlar;

    - Sistem tasarlama
    - Sitem üzerinde yeni dağıtımlara müdahale etme ve sorun çözme
    - Kullanıcının yetkilerini sınırlandırma.
    - Kullanıcıyı sistemden yalıtma.
    - Güvenlik.
    - Kullanıcı kendine özel geliştirme ortamı oluşturabilir.
    - Yazılım bağımlıkları sorunlarına çözüm olabilir.
    - Kullanıcıya sadece kendisine verilen alanda sınırsız yetki verme vb.

.. image:: /_static/images/chroot-1.png
  :width: 600


Yukarıdaki resimde user1 altında wrk dizini altına yeni bir sistem kurulmuş gibi yapılandırmayı gerçekleştirmiş.
Bu işlem için küçük bir örnek yapalım.
çalıştığımız dizin wrk dizin. 

Kök Dizin Oluşturma;
--------------------

.. code-block:: shell

	sudo chroot /home/user1/wrk seklinde yapabiliriz.
	
Sistemi Kaldırmak;
------------------

.. code-block:: shell

	sudo rm -rf /home/user1/wrk komutuyla kaldırabiliriz.

Bu yapının oluşturulması için temel komutları ve komut yorumlayıcının olması gerekmektedir. Bunun için bize gerekli olan komutları bu yapının içine koymamız gerekmektedir.
Örneğin ls komutu için doğrudan çalışıp çalışmadığını ldd komutu ile kontrol edelim.

.. image:: /_static/images/chroot-2.png
  :width: 600



Görüldüğü gibi ls komutunun çalışması için bağımlı olduğu kütüphane dosyaları bulunmaktadır. Bu dosyaları yeni oluşturduğumuz wrk klasörüne aynı dizin yapısında kopyalamamız gerekmektedir.
Bu dosyalar eksiksiz olursa ls komutu çalışacaktır. Fakat bu işlemi tek tek yapmamız çok zahmetli bir işlemdir. Bu işşi halledecek script dosyası aşağıda verilmiştir.

Bağımlılık Scripti
------------------

lldscript.sh

.. code-block:: shell

	#!/bin/bash

	if [ ${#} != 2 ]
	then
	    echo "usage $0 PATH_TO_BINARY target_folder"
	    exit 1
	fi

	path_to_binary="$1"
	target_folder="$2"

	# if we cannot find the the binary we have to abort
	if [ ! -f "${path_to_binary}" ]
	then
	    echo "The file '${path_to_binary}' was not found. Aborting!"
	    exit 1
	fi

	# copy the binary itself
	echo "---> copy binary itself"
	cp --parents -v "${path_to_binary}" "${target_folder}"

	# copy the library dependencies
	echo "---> copy libraries"
	ldd "${path_to_binary}" | awk -F'[> ]' '{print $(NF-1)}' | while read -r lib
	do
	    [ -f "$lib" ] && cp -v --parents "$lib" "${target_folder}"
	done

Basit Sistem Oluşturma
----------------------

Bu örnekte masaüstünde test dizini oluşturup ve işlemleryapıldı. ls, rmdir, mkdir ve bash komutlarından oluşan sistem hazırlama.

ls Komutu
----------

.. code-block:: shell

	bash lldscript.sh /bin/ls $PWD/test/ #komutunu kullanmalıyız.

.. image:: /_static/images/chroot-3.png
  :width: 600


Bu işlemi diğer komutlar içinde sırasıyla yapmamız gerekmektedir.
rmdir Komutu
------------

.. code-block:: shell

	bash lldscript.sh /bin/rmdir $PWD/test/ #komutunu kullanmalıyız.

.. image:: /_static/images/chroot-4.png
  :width: 600

mkdir Komutu
------------

.. code-block:: shell

	bash lldscript.sh /bin/mkdir $PWD/test/ #komutunu kullanmalıyız.

.. image:: /_static/images/chroot-5.png
  :width: 600

bash Komutu
------------

.. code-block:: shell

	bash lldscript.sh /bin/bash $PWD/test/ #komutunu kullanmalıyız.

.. image:: /_static/images/chroot-6.png
  :width: 600


chroot sistemde Çalışma
------------------------

.. code-block:: shell

	sudo chroot $PWD/test komutunu kullanmalıyız.

.. image:: /_static/images/chroot-7.png
  :width: 600

çıkış için ise ***exit*** komutunu kullanmalıyız.


Kaynak:
https://stackoverflow.com/questions/64838052/how-to-delete-n-characters-appended-to-ldd-list


.. raw:: pdf

   PageBreak

