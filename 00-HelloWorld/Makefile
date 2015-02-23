CROSS_COMPILE ?= arm-none-eabi-
CC := $(CROSS_COMPILE)gcc
CFLAGS = -fno-common -O0 \
	 -mcpu=cortex-m3 -mthumb \
	 -T hello.ld -nostartfiles \

QEMU ?= /usr/local/bin/qemu-system-arm

TARGET = hello.bin
all: $(TARGET)

$(TARGET): hello.c startup.c
	$(CC) $(CFLAGS) $^ -o hello.elf
	$(CROSS_COMPILE)objcopy -Obinary hello.elf hello.bin
	$(CROSS_COMPILE)objdump -S hello.elf > hello.list

print:
	$(CROSS_COMPILE)objdump -s hello.elf

disassemble:
	$(CROSS_COMPILE)objdump -d -l hello.elf

print_symbol:
	$(CROSS_COMPILE)nm hello.elf

get_all_strings:
	$(CROSS_COMPILE)strings hello.elf

qemu: $(TARGET)
	@$(QEMU) -M ? | grep stm32-p103 >/dev/null || exit
	@echo "Press Ctrl-A and then X to exit QEMU"
	@echo
	$(QEMU) -M stm32-p103 -nographic -kernel hello.bin

clean:
	rm -f *.o *.bin *.elf *.list
