# Rube Codeberg FSM (Finite State Machine)

During [PyConlineAU 2020](https://2020.pycon.org.au/), 
[Lilly Ryan](https://twitter.com/attacus_au) challenged attendees to
create a "Rube Codeberg" program that used an unnecessarily elaborate
method to acomplish a simple task.

This program, whlie not entered as part of the challenge (it was
inspired during the week after conference, and implemented the weekend
following the conference) is inpsired by this PyConlineAU 2020 challenge.

## Usage

    $ python3 hello
    Hello World!
    $ python3 hello --help
    usage: hello [-h] [--asm] [--bin BIN] [--html]

    optional arguments:
      -h, --help  show this help message and exit
      --asm       Output assembly
      --bin BIN   Output binary to file
      --html      Output HTML
    $ 

When run without arguments, it will execute the program.  When run
with `--html` it will output the example HTML that is described in
the program, for viewing separately; when run with `--asm` it will
output the example assembly code for editing or assembling separately;
when run with `--bin PROGRAM.com` it will output a binary `PROGRAM.com`
containing the assembled code.

## Competition Rules

The [PyConlineAU 2020 Rube Codeberg Competition 
Rules](https://2020.pycon.org.au/program/sun/) were fairly simple, but
somewhat constraining:

With the Rube Codeberg competition, we want you to be as elaborate as
possible. But! There are rules!

Your Program must:

*  Print "Hello world!" on the screen (somehow - we don't care how - 
   doesn't have to be the console) as a result of being executed

*  Be written for this competition (and to prove it, you must import 
   and use the Beautiful Soup library in your entry)

*  Be under 200 lines (all comments are part of the line count)

*  Be understandable/easy to parse by humans

*  Actually run successfully

*  Be written in Python 3

*  Abide by the Code of Conduct, as with all conference activities. 
   This means no malware, no offensive language or imagery, etc

## Thanks To

Thanks to [Lilly Ryan](https://twitter.com/attacus_au) for the inspiration,
and [Christopher Neugebauer](https://twitter.com/chrisjrn) for the 
[original Python `switch`/`case` 
implementation](https://chrisjrn.com/2020/09/04/practicality-beats-purity/).

## Examples

Running the program is simple:

```
$ python3 hello
Hello World!
$
```

And it can even output the embedded example assembly code separately:

```
$ python3 hello --asm
.ORG    0x100

        LD HL,text
        LD DE,key
decode: LD A,(HL)
        OR A
        JR Z,print
        LD B,A
        LD A,(DE)
        XOR B
        LD (HL),A
        INC DE
        INC HL
        JR decode

print:  LD HL,text 
pc:     LD A,(HL)
        OR A
        JR Z, done
        LD E,A
        LD C,2
        PUSH HL
        CALL 5
        POP HL
        INC HL
        JR pc

done:   RST 0

text:   DB 0x18, 0x1c, 0x2f, 0x03, 0x01, 0x4c, 0x3e

        DB 0x01, 0x17, 0x4c, 0x56, 0x11, 0x3f, 0x3a

        DB 00

key:    DB 0x50, 0x79, 0x43, 0x6f, 0x6e, 0x6c, 0x69

        DB 0x6e, 0x65, 0x20, 0x32, 0x30, 0x32, 0x30 

        DB 00
$ 
```

To output the embedded example binary separately:

    $ python3 hello --bin hello.com
    $ ls -l hello.com
    -rw-r--r-- 1 ewen ewen 67 Sep 13 13:53 hello.com
    $ hexdump -C hello.com
    00000000  21 25 01 11 34 01 7e b7  28 08 47 1a a8 77 13 23  |!%..4.~.(.G..w.#|
    00000010  18 f4 21 25 01 7e b7 28  0b 5f 0e 02 e5 cd 05 00  |..!%.~.(._......|
    00000020  e1 23 18 f1 c7 18 1c 2f  03 01 4c 3e 01 17 4c 56  |.#...../..L>..LV|
    00000030  11 3f 3a 00 50 79 43 6f  6e 6c 69 6e 65 20 32 30  |.?:.PyConline 20|
    00000040  32 30 00                                          |20.|
    00000043
    $ 

That example binary can be run on several machines from the 1980s, but which
machines is left as an exercise for the reader.

And for certainty, it is (just) within the required length:

    $ wc -l hello
    199 hello
    $ 

## Bugs

Readability suffers somewhat from needing to compress the code into a mere
200 lines of Python 3.  Who knew printing a simple string was so complicated?!
I believe the implementation can be understood by humans, despite being
somewhat compact.

The FSM implementation is incomplete: it only implements the subset that
is needed for the original program to operate, and certain liberties are
taken with instruction implementation where the details do not matter to
the original program.
