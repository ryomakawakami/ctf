# Last Hope
## Category: ML | Difficulty: Medium

> The quantum data came back and analyzed. DISASTER! Our best scientists all agree: Unfortunately our species and our whole culture are about to be eliminated. Due to abnormal behavior of the black hole's singularity our planet is about to get swallowed. Project "ONESHOT" is our last hope...

[ml_last_hope.zip](ml_last_hope.zip)

---

The only file we get is `quantum_artifact.qasm`. Looks like .qasm is some quantum programming language. My first thought was to go to [IBM Quantum Composer](https://quantum-computing.ibm.com/composer) and mess around with the file there. However, I quickly realized that there were way too many qubits to actually run simulations. Back to the drawing board.

If we open the file and we can see some patterns. Most significantly, one of the middle sections with lines like `cx q[X],q[176];` is the only section that skips over some numbers. For example, `cx q[1],q[176]` is missing.

Maybe we can construct a binary string, with 0 at the index of missing numbers and 1 at the other indices. Or it'll switched around. We'll see. Either way, the string will end up being 176 characters long.

We copy over the section of code into [temp.txt](temp.txt). Then we clean it up a bit, leaving only the numbers we're interested in. Now we can write up a script to construct the string.

```python
a = []
with open('temp.txt', 'r') as f:
    for line in f.readlines():
        a.append(int(line))

# This could be changed to O(n) but oh well
for i in range(176):
    if i in a:
        print('1', end='')
    else:
        print('0', end='')
```

Cool, now let's stick `10111110110011000000111000001100000101101111101001100110000011001111101011001100110011100000111010110110100011000011011011100110111110101000011011011110010000100010101000010010` into  [CyberChef](https://gchq.github.io/CyberChef) and select "From Binary" and see if we get our flag.

```
¾Ì...úf.úÌÎ.¶.6æú.ÞB*.
```

Definitely not right. Let's try to switch 1s and 0s.

```
A3ñóé..ó.31ñIsÉ..y!½Õí
```

Nope. Maybe we reverse the original string.

```
HTB{a_gl1mps3_0f_h0p3}
```

That's it.

---

Kind of related note.

I simplified the circuit down into [x.qasm](x.qasm) so I could actually see what was going on in IBM Quantum Composer. I saw that the output for qubit X was always `|1>` if there existed a line like `cx q[X],q[176];`. Otherwise, it was 0% `|1>`.
