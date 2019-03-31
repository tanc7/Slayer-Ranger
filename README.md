# Slayer-Ranger, bad character eliminator for shellcode and shellcode generator for both 32 and 64 bit instructions

# Design draft

# Proof of concept reference. Checking entire range of hex combinations of opcodes against a CPU. But the elimination of badchar is manually performed.

http://www.primalsecurity.net/0x7-exploit-tutorial-bad-character-analysis/

'\x90' change memory address by +230 + '\x7d\x8d\xb6\x7c' + '\x43' change memory address by 366

The python script says in Assembly opcodes..

1. Nop Sled 0x90
2. Special Jump if greater than, it's looking at the position of the INSTRUCTION POINTER. 0x7d
3. Load exact address 0x8d
4. Mov to EIP register 0xb6
5. Another Jump, if less than, because the instruction pointer just DECREMENTED BY ONE 0x7c
6. Increment pointer of EBX register 0x43

It brute forces every hexidecimal opcode by trying it out in the EIP register (make it 32-bit i386 because x64 is RIP register)

# Come up with a automatic way. 

Instead of manually comparing hex dumps, load the dumps into a file descriptor stream of unloaded and loaded instructions and compare line-by-line.

1. Reads objdump into it's own array
2. Generates a CPU interpreted list of successful and unsuccesful opcodes, by reading whats loaded into memory, and then separating each hexidecimal digit line by line (so 0x90 has its own line, and so does 0x7c and so long)
3. Compares each opcode line-by-line from the unloaded shellcode and what was seen in memory
4. Appends to a list object (array of chars and strings) each non-matching opcode
5. Prints a badchar elimination string, for msfvenom it's "-b '\x00\xbc\... more bad chars'

But wait. fuck msfvenom. Rereads the elimination string and on next objdump -d eliminates those along with nullbytes in my shellcode generator.

I could... outdo? Can I? Rapid7 is a big company. Out-innovate in payload generation and debugging? With this script I saw up here?

Perfect payload and buffer of shellcode generator.

I sped up... exploit research and crafting. By shortening the time it takes to identify bad characters by taking someone else's idea of bruting opcodes. Ok. I made it better by automating his opcode bruting. 



Shellcode Generator should be streamlined and renamed, Slayer-Ranger. Perfect shellcode generator. With bad char brute-forcing with (for version one its just i386) but later versions can do EIP and RIP instruction sets. We gotta find the amd64 equivalent of that. But most CPUs can either downgrade to 32-bit or thunk down to 16-bit instructions.
For teaching purposes, Offensive Security actually refers only to x86 instructions for crafting the SLMail exploit. Because aside from different opcodes and renamed registers its mostly the same CONCEPT not the same process. 

# Design draft for test-server interpreting shellcode. Think about using QEMU because it can emulate a lot of architectures

`python ./slayer-ranger.py -i testshellcode -t targetip -p targetport -o renderedopcodes.txt`

But this would only work if the listening machine is 32-bit? Or is there a way to force.

Make a 32-bit VM and listening service that starts as soon as Slayer-Ranger is ordered to test shell code.

Uses KVM and the subnet enabled by virsh and virbr0
Listening IP: 192.168.122.200
Listening Socket: 666

Make it a tiny VM since its 32 bit, it'll just be like, 256MB of RAM? Is that enough? 

*Scratch that, a Docker container is more appropriate because its purpose built and stripped down. It has to be a 32-bit containerized app with listening sockets*

`slayer-ranger.py -i shellcode.txt -t 192.168.122.200 -p 666 -o perfectshellcode`

The listening socket has to directly enter into CPU register. Using C. It's like some sort of, assembly language shell. 

Holy fuck. It actually exists https://github.com/cch123/asm-cli, it sends opcodes to the CPU in multiple architectures.

We can make a docker container instead that just accepts and interprets opcodes and has listening sockets. And then it sends back what the opcode rendered into in RAM. So return 1 if same, return 0 if false. Then slayer-ranger just jots it down into a list of bad chars and reencodes it. Only null bytes \x00 can be eliminated. Other bad chars must be substituted.

Hold on, these are the architectures that Docker supports https://docs.docker.com/docker-for-mac/multi-arch/ We should use some sort of VM hosted on QEMU.

# Replacing bad opcodes with good ones

I need to know how msfvenom eliminates bad bytes and substitutes them using a encoder. I think they brute for another opcode representation of that instruction?

*Or they look at what returned from submitted the opcode into the EIP register and looks at the assembly instruction*

We do that. We return a list of substituted opcode instructions to replace bad chars from.


# Slayer-Ranger, shellcode tester and generator with retargetable CPU architectures. Coming to a Github Repo near you.

1. Slayer-Ranger Opcode Server with addr and socket in Docker container
2. Slayer-Ranger Bruter
3. Slayer-Ranger Encoder

To help generate new working shellcode. Then retests it.

