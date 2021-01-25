# Keep Walking
**Category:** [Programming](../README.md)

**Points:** 10

**Description:**

This is a challenge to test your basic programming skills.


Pseudo code:

Set X = 1

Set Y = 1

Set previous answer = 1


answer = X * Y + previous answer + 3


After that => X + 1 and Y + 1 ('answer' becomes 'previous answer') and repeat this till you have X = 525.


The final answer is the value of 'answer' when X = 525. Fill it in below.


Example:

5 = 1 * 1 + 1 + 3

12 = 2 * 2 + 5 + 3

24 = 3 * 3 + 12 + 3

........................

........................

**This flag is not in the usual format, you can enter it with or without the brixelCTF{flag} format**

## Write-up
This was simply a matter of converting the pseudocode into a program. We did this in C++:
```cpp
#include <iostream>

int main()
{
  unsigned int x = 1; // X and Y are always the same, so we don't need both
  unsigned int prev_ans = 1;

  while (x<=525)
  {
    unsigned int answer = x * x + prev_ans + 3;
    std::cout << answer << " = " << x << " * " << x << " + " << prev_ans << " + 3\n";
    ++x;
    prev_ans = answer;
  }
  return 0;
}
```
We compiled and ran this program, and that gave us the flag.
