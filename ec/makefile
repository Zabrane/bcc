M=64
C=clang
O=-Os -m$M -std=gnu11 -DTST -pedantic -minline-all-stringops -fno-asynchronous-unwind-tables -fno-stack-protector -Wno-gnu-statement-expression -Wno-variadic-macros
SO=-shared -rdynamic -fPIC -nostdlib
EMOPT=${shell grep -v '@see' wop | xargs}
EMOPT2=${shell grep -v '@see' wop2 | xargs}

#C=$(shell env which gcc-9||env which gcc-8||echo gcc)
ifeq ($(shell uname),Darwin)
 O+= -I$(shell xcrun --show-sdk-path)/usr/include
endif

e: so
	@$C $O -UTST t.c -L. -le$M -o e$M
	@strip -K _main e$M
	@LD_LIBRARY_PATH=. ./e$M

t: tso
	$C $O t.c -L. -le$Mt -o t$M
	LD_LIBRARY_PATH=. ./t$M

so:
	@$C $O $(SO) -UTST e.c -o libe$M.so
	ls -lah libe$M.so

tso:
	$C $O $(SO) e.c -o libe$Mt.so

wso:
	emcc e.c -m32 -o e.wasm -Oz $(EMOPT)

w:
	emcc -DTST e.c t.c -m32 -o w.html -Oz $(EMOPT2)


goinstall:
	go install golang.org/x/crypto/curve25519 encoding/hex

go:
	go build e.go
	./e

all: t

.PHONY: all clean
