Paket Sitemi
+++++++++++++

Dağıtımlarda uygulamalar paketler halinde hazırlanır. Bu paketleri dağıtımda kullanabilmek için temel işlemler şunlardır;

1. Paket Oluşturma
2. Paket Liste İndexi Güncelleme
3. Paket Kurma
4. Paket Kaldırma
5. Paket Yükseltme gibi işlemleri yapan uygulamların tamammı paket sistemi olarak adlandırılır.

Paket sistemide uygulama paketi haline getirilip sisteme kurulur. Genelde paket sistemi dağıtımın temel bir parçası olması sebebiyle üzerinde yüklü gelir.

Bazı dağıtımların kullandığı paket sistemeleri şunlardır.

- apt: Debian dağıtımının kullandığı paket sistemi.
- emerge :Gentoo dağıtımının kullandığı paket sistemi.
- ymp : Turkman Linux dağıtımının kullandığı paket sistemi.


bps Paket Sistemi
-----------------

Bu dokümanda hazırlalan dağıtımın paket sistemi için ise bps(basit/basic/base paket sistemi) olarak ifade edeceğimiz paket sistemi adını kullandık. Bps paket sistemindeki beş temel işlemin nasıl yapılacağı ayrı başlıklar altında anlatılacaktır. Paket sistemi delemeli bir dil yerine bash script ile yapılacaktır. Bu dokumanı takip eden orta seviye bilgiye sahip olan linux kullanıcısı yapılan işlemleri anlayacaktır.

.. raw:: pdf

   PageBreak

