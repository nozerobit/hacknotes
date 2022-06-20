# Disassemblers

## Disassemblers <a id="disassemblers"></a>

A **disassembler** is a tool that breaks down a compiled program into machine code.

### List of Common Disassemblers <a id="list-of-disassemblers"></a>

* IDA
* Binary Ninja
* GNU Debugger \(GDB\)
* radare2
* Hopper

#### IDA <a id="ida"></a>

The Interactive Disassembler \(IDA\) is the binary disassembly industry standard. IDA can disassemble "virtually any popular file format." This makes it extremely useful for security researchers and CTF players, who frequently need to analyze obscure files with no idea what they are or where they came from. IDA also includes the industry-leading Hex-Rays decompiler, which can convert assembly to machine code.

IDA also has a plugin interface, which has been used to develop some successful plugins that can help with reverse engineering:

* [https://github.com/google/binnavi](https://github.com/google/binnavi)
* [https://github.com/yegord/snowman](https://github.com/yegord/snowman)
* [https://github.com/gaasedelen/lighthouse](https://github.com/gaasedelen/lighthouse)
* [https://github.com/joxeankoret/diaphora](https://github.com/joxeankoret/diaphora)
* [https://github.com/REhints/HexRaysCodeXplorer](https://github.com/REhints/HexRaysCodeXplorer)
* [https://github.com/osirislab/Fentanyl](https://github.com/osirislab/Fentanyl)

#### Binary Ninja <a id="binary-ninja"></a>

Binary Ninja is a new disassembler that aims to take a more programmatic approach to reverse engineering. Binary Ninja improves the plugin API and adds modern features to reverse engineering. While not as well-known or as old as IDA, Binary Ninja \(also known as a ninja\) is quickly gaining traction and has a small community of dedicated users and followers.

Ninja also has some community-contributed plugins which are collected here: [https://github.com/Vector35/community-plugins](https://github.com/Vector35/community-plugins)

#### gdb <a id="gdb"></a>

The GNU Debugger is a free and open-source debugger that can also be used to disassemble programs. It can be used as a disassembler.

gdb is frequently used in conjunction with enhancement scripts such as [peda](https://github.com/longld/peda), [pwndbg](https://github.com/pwndbg/pwndbg), and [GEF](https://github.com/hugsy/gef)

