Kernel Nedir?
++++++++++++
Linux sistemlerinin temel dosyasıdır.

Kernel Derleme
--------------
# Linux çekirdeğinin kaynak kodunu https://kernel.org üzerinden indirin.

wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.6.2.tar.xz
tar -xvf linux-6.6.2.tar.xz
cd linux-6.6.2
make defconfig
make bzImage

Derleme tamamlandığında Kernel: arch/x86/boot/bzImage is ready (#1) şeklinde bir satır yazmalıdır. Kernelimizin düzgün derlenip derlenmediğini anlamak için aşağıdaki komutu kullanabilirsiniz.

file arch/x86/boot/bzImage 
arch/x86/boot/bzImage: Linux kernel x86 boot executable bzImage, version 6.6.2 (etapadmin@etahta) #1 SMP PREEMPT_DYNAMIC Sat Nov 25 19:53:19 +03 2023, RO-rootFS, swap_dev 0XC, Normal VGA


