# Solidity Extension (Sol+)

This extension of Solidity allows additional customized keywords.
The very first keyword is `NonFallBack`.
For instace, following codelet will check if any function call in  the call chain from `f()` is made to a fallback function; if so, it will stop the execution, rewind the callchain.

    ...
    NonFallBack;
    f();
    ...

We assume the new keyword generates SETNONFALLBACK instruction for the EVM byte code (for 0x25 for the binary code,) which is again newly introduced.
For details, please contact us.
