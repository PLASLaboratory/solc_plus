# Solidity Extension (Sol+)

This extension of Solidity allows additional customized keywords.
The very first keyword is "NonFallBack".
For instace, following codelet will check if any function call in  the call chain from f() is a fallback function; if so, it will stop the execution, rewind the callchain.

...
NonFallBack;
f();

For details, please contact us.
