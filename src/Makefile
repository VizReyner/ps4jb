all: build/oldjb/c-code.js build/jb/c-code.js build/mira/mira.js build/mira/mira2.js build/mira/c-code.js build/fakeusb.js build/ftp.js copy_js

clean:
	rm -rf build inline inline_asm.c

8cc_setup:
	cd 8cc; make

build/oldjb/c-code.js: 8cc_setup oldkex.c
	mkdir -p build/oldjb
	8cc/8cc/python/rop-ps4-8cc /dev/stdout 8cc/librop/*.c 8cc/printf/*.c 8cc/ps4/*.c oldkex.c | python3 8cc/bad_hoist/rop/compiler.py /dev/stdin 8cc/bad_hoist/dumps/gadgets.txt > build/oldjb/c-code.js

build/jb/c-code.js: 8cc_setup kex.c inline_asm.c
	mkdir -p build/jb
	8cc/8cc/python/rop-ps4-8cc /dev/stdout 8cc/librop/*.c 8cc/printf/*.c 8cc/ps4/*.c kex.c | python3 8cc/bad_hoist/rop/compiler.py /dev/stdin 8cc/bad_hoist/dumps/gadgets.txt > build/jb/c-code.js

inline_asm.c: inline.asm inline_asm.py
	python3 inline_asm.py > inline_asm.c

build/mira/c-code.js: 8cc_setup miraldr.c
	mkdir -p build/mira
	8cc/8cc/python/rop-ps4-8cc /dev/stdout 8cc/librop/*.c 8cc/printf/*.c 8cc/ps4/*.c miraldr.c | python3 8cc/bad_hoist/rop/compiler.py /dev/stdin 8cc/bad_hoist/dumps/gadgets.txt > build/mira/c-code.js

miraldr.bin:
	echo 'Place MiraLoader payload at miraldr.bin'
	exit 1

mira.elf:
	echo 'Place Mira ELF here at miraldr.bin'
	exit 1

build/mira/mira.js: gen_mira_blob.py miraldr.bin
	mkdir -p build/mira
	python3 gen_mira_blob.py miraldr.bin > build/mira/mira.js

build/mira/mira2.js: gen_mira_blob_2.py mira.elf
	mkdir -p build/mira
	python3 gen_mira_blob_2.py mira.elf > build/mira/mira2.js

payloads/fakeusb/payload.bin:
	cd payloads/fakeusb; make

build/fakeusb.js: gen_mira_blob_2.py payloads/fakeusb/payload.bin
	python3 gen_mira_blob_2.py payloads/fakeusb/payload.bin > build/fakeusb.js

build/ftp.js: gen_mira_blob_2.py ../ftp.bin
	python3 gen_mira_blob_2.py ../ftp.bin > build/ftp.js

copy_js:
	mkdir -p build/common
	cp 8cc/bad_hoist/exploit.js 8cc/bad_hoist/helpers.js 8cc/bad_hoist/malloc.js build/common/
	cp 8cc/bad_hoist/rop/rop.js build/common/
	cp 8cc/bad_hoist/dumps/syscalls.txt build/common/syscalls.js
	cp 8cc/build/syscall_names.txt build/common/syscalls2.js
	cp ../index.html ../jb.html ../mira.html ../netcat.html ../ftp.html ../fakeusb.html build/
	cd build; python3 ../Patch.py
