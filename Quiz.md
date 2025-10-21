# Luau Quiz

* All snippets should be ran in o2 for consistent results.

## Section 1 (Beginner)

### Question 1
Is ``continue`` syntax sugar in Luau or does it have an opcode?

### Question 2
Assuming ``n`` is not a constant, what is the performance difference in the snippets ``4 * n`` and ``n * 4``? Explain why.

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
* ``Vector2int16``

### Question 5
What is the maximum amount of entries in a table?

### Question 6
What is the output of the following snippet?
```lua
loadstring(`print( 0b{string.rep("1",300)} == 0b{string.rep("11",300)} )`)()
```

### Question 7
What is this going to print?
```lua
print(2^53+1==2^53)
print(2^53+3==2^53)
print(2^56+3==2^56)
```

### Question 8
What does this print?
```lua
print(#'\11\0')
```

## Section 2 (Intermediate)

### Question 1

Explain the behavior of __eq in this snippet, what it prints, and also why the output in Lua is different from the output in Luau in certain cases.
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
print(mul(CFrame.new(1,1,1), CFrame.new(1,1,1)).Position)
```
```lua
local _, add = getFn(function()
    return Vector3.new(1,1,1) + nil
end)
print(add(Vector3.new(1,1,1), Vector3.new(1,1,1)))
```

## Section 3 (Advanced)

### Question 1
What are the flaws with Luau's modulo operator? How can this flaw be corrected.

### Question 2
How much memory does a table and function take in bytes?

### Question 3

What is the output of this? Explain.
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

### Question 5
What is the output of the following snippet?
```lua
print(table.unpack({1,2,3,nil,5}))
print(table.unpack({1,2,3,nil,5,nil}))
print(table.unpack({1,2,3,nil,5,nil}, -2, 7))
```
## Section 4 (Expert)

### Question 1
What would these print? Explain.
```lua
local inext = ipairs{}
print(next({1,2,nil}, 2))
print(next({1,2,nil,4}, 2))
print(inext({1,2,nil,4}, 2))
```

### Question 2
What would this print? Explain.
```lua
local count = 0
local printingThread = coroutine.create(print)

local yieldedThreads = {}
local function yield()
    table.insert(yieldedThreads, coroutine.running())
    return coroutine.yield()
end

local done = 0

for i = 1, 100 do
    coroutine.wrap(function()
        count += yield() and 1 or 0
        done += 1
    end)()
end

for _, thread in yieldedThreads do
    coroutine.resume(thread, true)
    if done == 50 then
        coroutine.resume(printingThread, count)
        break
    end
end
```

