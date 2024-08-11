<h1>Lua Switch Case</h1>
Simple and efficient switch case approach in lua.

<h2>Explenation</h2>
<p>In short, a switch case is a better and faster version of nested if-else statements. They take a value, match it with a case that has the same value and execute the case. If the value doesn't match any case, it exectues the "default" case. In addition, if the switch case isn't broken, it falls through to the next case and executes it.</p>

<p>In a higher level (Java) this would be:</p>

```java
switch(5){
case 0:
    System.out.println("Case 0"); // this case falls through the next case
case 1:
    System.out.println("Case 0 or 1");
    break; // break the switch case so it doesn't fall through the default case
default:
    System.out.println("Default case");
case 5:
    System.out.println("Case 5");
}
```
<p>In a lower level, they take an integer value, index a constant jump table (only initialized once in the program) and then jump to the corresponding instruction address. This indexing is done in a constant time complexity meaning every case takes the same time to be matched.</p>

<h2>Lua Implementation</h2>
<p>The most similar approach in Lua (efficiency wise) is initializing a constant table with functions as values and handle fall-throughs recursively:</p>

```lua
-- function that takes a value and a cases table and executes the matching case
function doTableSwitch(value,cases)
	local case = cases[value];
	local function FallThrough(case)
		return cases[case](FallThrough);
	end
	return (case or cases.default)(FallThrough);
end

-- cases table
cases = {
	[0] = function(FallThrough)
		return FallThrough(1); -- explicitly fall through to the next case (case 1)
	end,
	[1] = function(FallThrough)
		print("Case 0 or 1"); -- no explicit fall through here so break
	end,
	default = function(FallThrough)
		print("Default case");
		return FallThrough(5);
	end,
	[5] = function(FallThrough)
		print("Case 5");
	end,
}

doTableSwitch(5,cases) 
```
