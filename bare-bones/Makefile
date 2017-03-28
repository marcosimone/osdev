boot.o: boot.s
	/home/marco/opt/cross/bin/i686-elf-as boot.s -o boot.o

kernel.o: kernel.c
	/home/marco/opt/cross/bin/i686-elf-gcc -c kernel.c -o kernel.o -std=gnu99 -ffreestanding -O2 -Wall -Wextra

myos.bin: boot.o kernel.o
	/home/marco/opt/cross/bin/i686-elf-gcc -T linker.ld -o myos.bin -ffreestanding -O2 -nostdlib boot.o kernel.o -lgcc
	grub-file --is-x86-multiboot myos.bin || (echo "multiboot header check failed")
	
myos.iso: myos.bin
	cp myos.bin isodir/boot/myos.bin
	cp grub.cfg isodir/boot/grub/grub.cfg
	grub-mkrescue -o myos.iso isodir	

run: myos.iso
	qemu-system-i386 -curses -cdrom myos.iso
clean:
	rm *.o myos.bin
