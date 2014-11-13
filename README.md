# LibMasm16
----
Intel Assembly Language Library based on Masm in Real Address Mode

## COPYRIGHT
This library is published under GPLv3.

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
