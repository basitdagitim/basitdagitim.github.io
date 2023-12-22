Dağıtım Nedir?
==============

 Linux kullanmaya başlayan kişilerin en çok karşılaştığı terimlerden birisi **dağıtım** kelimesidir.
Dağıtım şirketi, grup veya ekiplerden oluşan kişiler tarafından paketler derlenerek veya hazırlanmış bir linux sisteminin çeşitli düzenlemeler yapılarak bir isim altında oluşturulan **Linux Sistemi**'ne verilen addır.
Açık kaynak felsefesinde dağıtımlar ve uygulamalar belirli bir lisansla lisanslanarak yayınlanmaktadır. Genellikle lisansların bazı farklılıkları olsada "Al, kullan, değiştir ve kimseden izin almadan dağıt" şeklindedir.
Lisanslamadaki bu felsefeden dolayı çok fazla dağıtım oluşmuş ve oluşmaya devam etmektedir. 

 Linux dağıtımları genelde **kernel** ve uygulamalardan oluşur. Kernel ve uygulamaların kodları github, gitlab vb. ortamlarda paylaşıldığı için sıfırdan bir dağıtımda oluşturmak mümkündür. Bunu oluştururken yasal olmayan hiçbir işlem yapmamış oluruz. Çünkü  genel felsefe ***"Al, kullan, değiştir ve kimseden izin almadan dağıt"*** şeklinde olduğunu hatırlayalım.

 Bu doküman basit seviveyede bir dağıtım oluşturmak ve kurulabilir bir medya dosyası(iso dosya) nasıl hazırlanacağını anlatan bir rehber olacaktır. 
Bu dokümanı hazırlanmasında ve anlatılanları tecrübe ederek öğrenmeme katkısı olan **Turkman Linux** dağıtım ekibiden  @sulincix(Ali Rıza KESKİN) ve Celaleddin AKARSU'ya teşekkür ederim. 

Dağıtım Nasıl Hazırlanmalı?
===========================

Bir dağıtım hazırlamak için orta seviye linux komutları ve kavramları bilmeliyiz. Bu bağlamda bu dokümanı okurken yabancı olduğunuz terimleri araştırmanızı tavsiye ederim.
Bir dağıtım için bilinmesi gereken konuları maddeler halinde şöyle sıralayabiliriz.

1. Dağıtım Ortamının Hazırlanması
2. Derleme ve Bağımlılık
3. chroot Nedir?
4. Temel Paketleri Derleme
5. Paket Sistemi Tasarlama
6. initrd Hazırlama
7. Sistemin Hazırlanması
8. İsonun Kurulması


Burada sıralanan maddeler konu başlıkları olarak anlatılacaktır.

.. raw:: pdf

   PageBreak

