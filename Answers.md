# Section 1

<details>
  <summary>Q1</summary>

  Yes, it is syntax sugar and does not have an opcode.
</details>


<details>
  <summary>Q2</summary>

  ``4 * n`` uses the ``ADD`` opcode while ``n * 4`` uses ``ADDK`` where the 4 is a constant, making ``n * 4`` slightly faster.
  <img width="862" height="77" alt="image" src="https://github.com/user-attachments/assets/85da8efc-62af-4dd4-a949-d6bdbab6d928" />
</details>

<details>
  <summary>Q3</summary>

  This is a trick question. They are all 16 bytes in size, so the size difference is 0. 
</details>

<details>
  <summary>Q4</summary>

  Vector3 is the smallest as it is a primitive, taking 16 bytes. Vector2 and Vector2int16 are userdata, meaning they take ``24 bytes + overhead``.
</details>

<details>
  <summary>Q5</summary>

  `2^26` for the array and hashpart.
</details>


<details>
  <summary>Q6</summary>

  ``true`` because it is a super large number
```lua 
>loadstring(`print( 0b{string.rep("1",300)} , 0b{string.rep("11",300)} )`)()
18446744073709552000 18446744073709552000
```
</details>

<details>
  <summary>Q7</summary>

  `true` `false` `true`. `F64` arithmetic starts to lose precision at `2^53` and gets worse the larger a number gets.
</details>


<details>
  <summary>Q8</summary>

 2 because Luau does not use null-terminated strings and `\11` and `\0` are just ASCII characters.
</details>

# Section 2

<details>
  <summary>Q1</summary>

  `false 0` because the function is not the same. `DUPCLOSURE` would not be present due to upvalues being mutated.
</details>

<details>
  <summary>Q2</summary>

  10, Luau uses a binary search for table length. Read https://github.com/Meifeng1510/LuauDocs/blob/main/Table/Guide/LuauTableLengthInDepth.md
</details>

<details>
  <summary>Q3</summary>

  Make sure you are in --!optimize 2 when testing this. The first one will print 3 for the `#testTable` while the second one will print 0. This is because Luau is smart enough to preallocate the table due to the for loop.
</details>

<details>
  <summary>Q4</summary>

  The `getFn` returns the `__mul` metamethod for cframe which would just be `print((CFrame.new(1,1,1) * CFrame.new(1,1,1)).Position)`, printing `2,2,2`. However, for the Vector3 one it would not be `__add` as Vector3 is a primative type and would just do normal addition like adding two numbers.
</details>

<details>
  <summary>Q5</summary>

   The first one would just print `1 2 4 5` as it would iterate the table normally. The `3` doesn't do anything special here.

   The second one would output nothing because it would be getting the ipairs iterator, then start after index 2, which is a hole, stopping the iteration.
</details>

# Section 3

<details>
  <summary>Q1</summary>

  Luau’s floating point modulo is computed as ``a - floor(a / b) * b``, which works for normal sized numbers. However, doubles lose precision when values get very large. As a result, the ``(floor(x / y) * y)`` term can round up to >= x, producing negative results.
  Lua 5.3 fixed this problem by redefining the modulo operation to use the C function ``fmod(a, b)`` instead of ``a - floor(a / b) * b``.

</details>

<details>
  <summary>Q2</summary>

  A table object is 48 bytes by default (with each TValue being 16 bytes and LuaNode being 32 bytes), and a function (closure object) is 48 bytes + 16 bytes per upvalue 
</details>

<details>
  <summary>Q3</summary>

   This would print ``100 0``. The allocated array size would be at 100 for the first print, while `x.y=2` triggers a rehash for everything (including the array part) causing the array part to shrink back to 0. As a result, the `x[100]=1` would end up in the hash part.
</details>

<details>
  <summary>Q4</summary>

   This prints 4 when a is localized and 3 when it is global. This is because when you localize a variable Luau tries to reuse the stack index as much as possible to avoid cloning. When you evaluate the call ``foo = lol`` it does not get ``foo`` and tries to use the original stack index which gets overwritten by the call.
</details>

<details>
  <summary>Q5</summary>

``` 
1	2	3	nil	5
1	2	3
nil	nil	nil	 1	2  3  nil  5  nil nil
```

Check the args of table.unpack().
</details>

# Section 4

<details>
  <summary>Q1</summary>

```
nil
4	4
```

  If you said `inext` returns `nil` you are wrong. It returns nothing when it reaches a hole.
</details>

<details>
  <summary>Q2</summary>

It would print 1.

The function above acts similarly to:

```lua
local count = 0
for i = 1, 100 do
    task.spawn(function()
        count += task.wait() and 1
    end)
end

task.wait(1)
    
print(count)
  ```

``count`` would be stored as 0 before being yielded meaning:

 ```lua
local count = 0
for i = 1, 100 do
    task.spawn(function()
        local temp = count -- 0
        task.wait()
        count = temp + 1 -- same as count = 0 + 1
    end)
end

task.wait(1)

print(count)
```


</details>
