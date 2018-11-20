# Solidity Extension (Sol+)

This extension of Solidity allows additional customized keywords.
The very first keyword is `NonFallBack`.
For instace, following codelet will check if any function call in  the call chain from `f()` is made to a fallback function; if so, it will stop the execution, rewind the callchain.

    ...
    NonFallBack;
    f();
    ...

We assume the new keyword generates SETNONFALLBACK instruction for the EVM byte code (for 0x25 for the binary code,) which is again newly introduced.

Example)
A Sol+ source file (including "NonFallBack" keyword)
<pre> <code>
    pragma solidity ^0.4.0;
    contract SimpleStorage{
        function get() public view returns (uint retVal){
                NonFallBack;
                return storedData;
        }
        uint storedData;
        function set (uint x) public payable {
                storedData = x;
        }
    }
</code> </pre>

The result byte code will be as follows;

<pre> <code>
USH1 0x80 PUSH1 0x40 MSTORE CALLVALUE DUP1 ISZERO PUSH2 0x10 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP PUSH1 0xE2 DUP1 PUSH2 0x1F PUSH1 0x0 CODECOPY PUSH1 0x0 RETURN INVALID PUSH1 0x80 PUSH1 0x40 MSTORE PUSH1 0x4 CALLDATASIZE LT PUSH1 0x49 JUMPI PUSH1 0x0 CALLDATALOAD PUSH29 0x100000000000000000000000000000000000000000000000000000000 SWAP1 DIV PUSH4 0xFFFFFFFF AND DUP1 PUSH4 0x60FE47B1 EQ PUSH1 0x4E JUMPI DUP1 PUSH4 0x6D4CE63C EQ PUSH1 0x79 JUMPI JUMPDEST PUSH1 0x0 DUP1 REVERT JUMPDEST PUSH1 0x77 PUSH1 0x4 DUP1 CALLDATASIZE SUB PUSH1 0x20 DUP2 LT ISZERO PUSH1 0x62 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST DUP2 ADD SWAP1 DUP1 DUP1 CALLDATALOAD SWAP1 PUSH1 0x20 ADD SWAP1 SWAP3 SWAP2 SWAP1 POP POP POP PUSH1 0xA1 JUMP JUMPDEST STOP JUMPDEST CALLVALUE DUP1 ISZERO PUSH1 0x84 JUMPI PUSH1 0x0 DUP1 REVERT JUMPDEST POP PUSH1 0x8B PUSH1 0xAB JUMP JUMPDEST PUSH1 0x40 MLOAD DUP1 DUP3 DUP2 MSTORE PUSH1 0x20 ADD SWAP2 POP POP PUSH1 0x40 MLOAD DUP1 SWAP2 SUB SWAP1 RETURN JUMPDEST DUP1 PUSH1 0x0 DUP2 SWAP1 SSTORE POP POP JUMP JUMPDEST PUSH1 0x0 SETNONFALLBACK PUSH1 0x0 SLOAD SWAP1 POP SWAP1 JUMP INVALID LOG1 PUSH6 0x627A7A723058 KECCAK256 CREATE2 0x2f RETURN 0xe5 LOG1 0x2e 0xd2 ISZERO CREATE 0x46 BALANCE SIGNEXTEND PUSH14 0xB54A072D724F41DED0A47F3066EE SDIV DUP3 MSTORE CALLDATASIZE 0xf9 STOP 0x29
<code> </pre>

Modified C/C++ source files of Solidity repository are;

1. libevmasm/Instruction.cpp
2. libevmasm/Instruction.h
3. libsolidity/ast/AST_accept.h
4. libsolidity/ast/ASTForward.h
5. libsolidity/ast/AST.h
6. libsolidity/ast/ASTJsonConverter.cpp
7. libsolidity/ast/ASTJsonConverter.h
8. libsolidity/ast/ASTPrinter.cpp
9. libsolidity/ast/ASTPrinter.h
10. libsolidity/ast/ASTVisitor.h
11. libsolidity/codegen/CompilerContext.cpp
12. libsolidity/codegen/ContractCompiler.cpp
13. libsolidity/parsing/Parser.cpp
14. libsolidity/parsing/Token.h

For more details, please contact us.
