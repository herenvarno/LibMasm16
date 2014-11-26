# LibMasm16
----
Intel Assembly Language Library based on Masm in Real Address Mode

## COPYRIGHT
This library is published under __DO WHAT THE FUCK YOU WANT TO PUBLIC LICENSE__.
You can find more detail in file _LICENSE_.

## USAGE
To use it, include this file in your assembly program.

``` Assembly
	.model small
	.stack
	.data
	....
	.code
	include		LibMasm16.inc
	.startup
	....
	.exit
	end
```
