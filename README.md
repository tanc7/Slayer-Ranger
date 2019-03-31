http://www.primalsecurity.net/0x7-exploit-tutorial-bad-character-analysis/

The python script says in Assembly opcodes..

1. Nop Sled
2. Special Jump if greater than, it's looking at the position of the INSTRUCTION POINTER.
3. Load exact address
4. Mov to EIP register
5. Another Jump, if less than, because the instruction pointer just DECREMENTED BY ONE


It brute forces every hexidecimal opcode by trying it out in the EIP register (make it 32-bit i386 because x64 is RIP register)

Come up with a automatic way. Instead of manually comparing hex dumps, load the dumps into a file descriptor stream of unloaded and loaded instructions and compare line-by-line.

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


python ./slayer-ranger.py -i testshellcode -t targetip -p targetport -o renderedopcodes.txt

But this would only work if the listening machine is 32-bit? Or is there a way to force.

Make a 32-bit VM and listening service that starts as soon as Slayer-Ranger is ordered to test shell code.

Uses KVM
Listening IP: 192.168.122.200
Listening Socket: 666

# I don't need to show the user what I found, unless they use uh. -vv option to print discovered badchars

slayer-ranger.py -i shellcode.txt -t 192.168.122.200 -p 666 -o perfectshellcode
