# Luau Quiz

## Section 1 (Beginner)

### Question 1
Is ``continue`` syntax sugar in Luau or does it have an opcode?

### Question 2
Assuming ``n`` is a constant, what is the performance difference in the snippets ``4 * n`` and ``n * 4``? Explain why.

### Question 3
Explain the size difference of these types

* ``1``
* ``nil``
* ``true``
* ``Vector3``

### Question 4
Which one has the smallest size?

* ``Vector3``
* ``Vector2``
* ``Vector2int64``

## Section 2 (Intermediate)

### Question 1

Explain the behavior of __eq in this snippet, what it prints, and also why the output in Lua is different from the output in Luau.
```lua
local callCount = 0

local function example()
    return setmetatable({}, {__eq = function() 
        callCount += 1
        return true 
    end}) 
end
print(example() == example(), callCount)
```

### Question 2
What is the output of the following snippet? How do you calculate the length of any table in luau?
```lua
local tbl = {
    [0] = "r",
    [1] = "u",
    [2] = "m",
    [3] = "i",
    [5] = "n",
    [8] = "e",
    [9] = nil,
    [10] = "is",
}

print(#tbl)
```

### Question 3

What is the difference in the outputs of these 2 snippets? Explain why they have different behavior.
```lua
local testTable = {}
for i = 1, 3 do
    if i == 3 then
        testTable[i] = "hi"
        print(testTable)
        print(#testTable)
    end
end
```

```lua
local testTable = {}
for i = 1, 3 do
    if i == 3 then
        testTable[3] = "hi"
        print(testTable)
        print(#testTable)
    end
end
```

### Question 4
Given this function ``getFn``,

```lua
function getFn(expr)
    return xpcall(expr, function()
        return debug.info(2, "f")
    end)
end
```

What would the difference in the output of these 2 snippets be?

```lua
local _, mul = getFn(function()
    return CFrame.new(1,1,1) * nil
end)
print(mul(CFrame.new(1,1,1),CFrame.new(1,1,1)).Position)
```
```lua
local _, add = getFn(function()
    return Vector3.new(1,1,1) + nil
end)
print(add(Vector3.new(1,1,1),Vector3.new(1,1,1)))
```

## Section 3 (Advanced)

### Question 1
What are the flaws with Luau's modulo operator? How can this flaw be corrected.

### Question 2
How much memory does a table and function take in bytes?

### Question 3

What is the output of this? Explain
```lua
local x = table.create(100)
x[100] = 1
print(#x)
x.y = 2
print(#x)
```

### Question 4
Explain the difference in the output of the next 2 snippets and why it happens. 

```lua
local a = 1
local function b()
    a += 1
    return a
end
print(a + b())
```

```lua
a = 1
local function b()
    a += 1
    return a
end
print(a + b())
```

## Section 4 (Expert)

### Question 1
What is a downvalue?

### Question 2
How do you implement the 0 bandwidth method?

### Question 3
How do you overcome "skidding" and become immune to ever getting skid checked.