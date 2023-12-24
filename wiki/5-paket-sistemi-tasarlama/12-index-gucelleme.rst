Paket Liste İndexi Güncelleme
+++++++++++++++++++++++++++++

Dağıtımlarda uygulamalar paketler halinde hazırlanır. Bu paketleri isimleri, versiyonları ve bağımlılık gibi temel bilgileri barındıran liste halinde tutan bir dosya oluşturulur. Bu dosyaya **index.lst** isim verebiliriz. Bu dokümanda bu listeyi tutan  **index.lst** doyası kullanılmıştır. Paket sisteminde güncelleme aslında **index.lst** dosyanın en güncellen halinin sisteme yüklenmesi olayıdır.

bps paketleme sisteminde **bpsupdate** scripti hazırlanmıştır. Bu script **index.lst** dosyasının paketlerimizin en güncel halini sistemimize yükleyecektir. Bu dağıtımda paketlerimizi github.com üzerinde oluşturulan bir repostory üzerinden çekilmektedir. Paket listemiz ise yapılan her yeni paketi yükleme sırasında güncellenmektedir.

Paket güncelleme için iki script kullanmaktayız. Bunlar;

**index.lst** Dosyasını Oluşturma
---------------------------------

.. code-block:: shell
	
	#index alma scripti
	#!/bin/sh
	set -ex
	>index.lst
	find ./* -type f -name *bps |
		    while IFS= read file_name; do
		    	tar -xf ${file_name} bpsbuild
		    	version=$(cat bpsbuild|grep version=)
		    	name=$(cat bpsbuild|grep name=)
		      	depends=$(cat bpsbuild|grep depends=)
		       	echo "$name:$version:$depends">>index.lst
		    done
	rm -rf bpsbuild
	mkdir /output -p
	cp -rf index.lst /output 

Bu script bps paket dosyalarımızın olduğu dizinde tüm paketleri açarak içerisinden **bpsbuild** dosyalarını çıkartarak paketle ilgili bilgileri alıp **index.lst** dosyası oluşturmaktadır. istersek paketler local ortamdada index oluşturabiliriz. Bu dokümanda github üzerinde oluşturacak şekilde anlatılmıştır.Paket indeksi oluşturan **index.lst** dosyası aşağıdaki gibi olacaktır. Listede name, version ve depends(bağımlı olduğu paketler) bilgileri bulunmaktadır. Bilgilerin arasında **:** karekteri kullanılmıştır.


.. code-block:: shell

	name="glibc":version="2.38":depends=""
	name="gmp":version="6.3.0":depends="glibc,readline,ncurses"
	name="grub":version="2.06":depends="glibc,readline,ncurses"
	name="kmod":version="31":depends="glibc,zlib"


**index.lst** Dosyasını Güncelleme
----------------------------------

**bpsupdate** dosya içeriği 

.. code-block:: shell
	
	#!/bin/sh
	./indirgentoo /tmp/index.lst https://bayramkarahan.github.io/distro-binary-package/index.lst
	


   
**index.lst** dosyamızı github üzerinden indiren scriptimiz tek bir satırdan oluşmaktadır. **indirgentoo** dosyamız gento üzerinde curl kütüphanesi kullanan bir c kodundan oluşmaktadır. 

.. raw:: pdf

   PageBreak

İndirme Uygulaması
------------------

İndirme dosyamız **indirgentoo** dur. Adının böyle olmasının sebebi bağımlılık sorunlarını en aza indirmek için static olarak **gentoo** ortamında derlendiği için böyle adlandırıldı.  **indirgentoo** kodları aşağıdadır görülmektedir.

.. code-block:: shell

	#include <stdio.h>
	#include <curl/curl.h>
	struct FtpFile { const char *filename;  FILE *stream;};
	static size_t my_fwrite(void *buffer, size_t size, size_t nmemb,void *stream){
	  struct FtpFile *out = (struct FtpFile *)stream;
	  if(!out->stream) {
	    out->stream = fopen(out->filename, "wb");
	    if(!out->stream) return -1; /* failure, cannot open file to write */
	  }
	  return fwrite(buffer, size, nmemb, out->stream);
	}
	int main(int argc, char **argv)	{
	  const char *outname;	argv++;  outname = *argv;
	  const char *fileaddress; argv++; fileaddress=*argv;
	  printf("adı:%s",outname);
	  printf("adres:%s",fileaddress);
	  CURL *curl;
	  CURLcode res;
	  struct FtpFile ftpfile = {
	    outname, /* name to store the file as if successful */
	    NULL
	  };

	  curl_global_init(CURL_GLOBAL_DEFAULT);
	  curl = curl_easy_init();
	  if(curl) {
	    curl_easy_setopt(curl, CURLOPT_URL,fileaddress);
	    curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, my_fwrite);/*Define our callback to get called when there's data to be written */
	    curl_easy_setopt(curl, CURLOPT_WRITEDATA, &ftpfile);/* Set a pointer to our struct to pass to the callback */
	    curl_easy_setopt(curl, CURLOPT_USE_SSL, CURLUSESSL_ALL);/* We activate SSL and we require it for both control and data */
	    curl_easy_setopt(curl, CURLOPT_FOLLOWLOCATION, 1L);
	    curl_easy_setopt(curl, CURLOPT_VERBOSE, 1L);/* Switch on full protocol/debug output */
	    res = curl_easy_perform(curl);
	    curl_easy_cleanup(curl);	    /* always cleanup */
	    if(CURLE_OK != res) { /* we failed */    fprintf(stderr, "curl told us %d\n", res);  }
	  }

	  if(ftpfile.stream)
	    fclose(ftpfile.stream); /* close the local file */
	  curl_global_cleanup();
	  return 0;
	}

**indirgentoo** çalışabilir dosyamız iki parametre almaktadır. ilk parametre indirilecek dosyanın nereye ve hangi adla kaydedileceği belirtiliyor. İkinci parametre ise hangi adresten inecekse ilgili adres bilgidir.

.. code-block:: shell

	./indirgentoo /tmp/index.lst https://bayramkarahan.github.io/distro-binary-package/index.lst

Bu komut https://bayramkarahan.github.io/distro-binary-package/index.lst adresindeki dosyayı index.lst dosyasını //tmp/index.lst konumuna indirecektir.


.. raw:: pdf

   PageBreak

