# Don't Be Salty
**Category:** [Cryptography](../README.md)

**Points:** 20

**Description:**

Our l33t hackers hacked a bulletin board and gained access to the database. We need to find the admin password.

The user's database info is:

Username:admin

Passwordhash:2bafea54caf6f8d718be0f234793a9be

Salt:04532@#!!


We know from the source code that the salt is put AFTER the password, then hashed. We also know the user likes to use lowercase passwords of only 5 characters long.

The flag is the plaintext password.

**This flag is not in the usual format, you can enter it with or without the brixelCTF{flag} format**

## Write-up
> TL;DR: The steps will describe the way we got to the answer for our own learning records. If you just want to know what the answer was, jump to the [solution](#solution)

### Steps
From the description, we can see that we need to work out a password of 5 lowercase characters that when the salt is added to the end, the MD5 sum of the string give the hash 2bafea54caf6f8d718be0f234793a9be. For example, the passoword could be `passw`, with the salt it becomes `passw04532@#!!`, and then the MD5 sum should be calculated and tested for a match to the given MD5. In a bash scripts we could write:
```bash
pass="passw04532@#!!"
expected=2bafea54caf6f8d718be0f234793a9be

# MD5 sum always outputs a dash after the MD5 sum,
# so cut that off
answer=$(echo $pass | md5sum | cut -d ' ' -f1)
if [ $expected == $answer ]
then
  echo "Yay!"
else
  echo "Boo!"
fi
```
Of course, the above returns `Boo!` when run, because the password isn't `passw`.

We wrote a *bash* script to run through each letter of the alphabet for the 5 characters in the password, add the salt to each combination, and calculate the MD5. This *bash* script was very, very slow, however.

To speed it up, a C++ program was written instead, which called into the system for the `md5sum` command. The function to execute the `md5sum` and get the return value was:
```cpp
std::string exec(const std::string &cmd) 
{
  std::array<char, 128> buffer;
  std::string result;
  std::unique_ptr<FILE, decltype(&pclose)> pipe(popen(cmd.c_str(), "r"), pclose);
  if (!pipe) 
  {
    throw std::runtime_error("popen() failed!");
  }
  
  while (fgets(buffer.data(), buffer.size(), pipe.get()) != nullptr) 
  {
    result += buffer.data();
  }

  return result;
}
```
This function was taken from [here](https://stackoverflow.com/a/478960).

Which was passed a command similar to the one from the bash script above, but for each combination of the 5 characters. However, this was still slow, so we went the whole hog, and did the MD5 calculation in C++ as well.

# Solution
After finding the *bash* script and the C++ program calling into *bash* too slow, we decided to write a fully C++ program to calculate the result. This used MD5 code taken from [here](http://www.zedwood.com/article/cpp-md5-function).

The final C++ code to solve the problem was:
```cpp
#include <iostream>
#include <string>
#include "md5.h"

bool incrementPass(std::string &pass)
{
  bool finished = false;
  pass[4] += 1;
  if (pass[4] == 123)
  {
    pass[4] = 'a';
    pass[3] += 1;
    if (pass[3] == 123)
    {
      pass[3] = 'a';
      pass[2] += 1;
      if (pass[2] == 123)
      {
        pass[2] = 'a';
        pass[1] += 1;
        if (pass[1] == 123)
        {
          pass[1] = 'a';
          pass[0] += 1;
          // Output when the first character is updated
          // for a 'progress' report
          std::cout << "Current password: " << pass << "\n";
          if (pass[0] == 123)
          {
            finished=true;
          }
        }
      }
    }
  }
  return finished;
}

int main()
{
  const std::string hexdigits="0123456789abcdef";

  std::string pass="aaaaa";
  std::string salt="04532@#!!";
  std::string answer="2bafea54caf6f8d718be0f234793a9be";
  bool finished=false;

  while (!finished)
  {
    std::string passsalt = pass + salt;
    std::string result=md5(passsalt);

    if (result == answer)
    {
      std::cout << "Password " << pass << " (with salt: " << passsalt << ")\n";
      std::cout << "Matches hash: " << result << "\n";
    }

    finished = incrementPass(pass);
  }
  
  return 0;
}
```
This was compiled and run with:
```
> g++ get_flag.cpp md5.cpp -o get_flag
> ./get_flag
Current password: baaaa
Password <flag_was_here> (with salt: <flag_was_here>04532@#!!)
Matches hash: 2bafea54caf6f8d718be0f234793a9be
Current password: caaaa
Current password: daaaa
Current password: eaaaa
Current password: faaaa
Current password: gaaaa
Current password: haaaa
Current password: iaaaa
Current password: jaaaa
Current password: kaaaa
Current password: laaaa
Current password: maaaa
Current password: naaaa
Current password: oaaaa
Current password: paaaa
Current password: qaaaa
Current password: raaaa
Current password: saaaa
Current password: taaaa
Current password: uaaaa
Current password: vaaaa
Current password: waaaa
Current password: xaaaa
Current password: yaaaa
Current password: zaaaa
Current password: {aaaa
```
This gave us the flag.