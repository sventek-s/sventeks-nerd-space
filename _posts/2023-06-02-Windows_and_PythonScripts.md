---
published: true
---

# Windows Python Scripts Execution from Anywhere

I have been using windows lately and from being a linux user most of my life been facing afew difficulties. Among them is the lack of having aliases. I won't go deep into it
but I had this difficulty where I had a python script that would find the file version of a DLL or binary file. I would like to run this script from anywhere so I thought it
would be easy as pie but NOP its windows fellah's it doesn't work like that. If I was using linux an alias would fix this but below is the solution I proceeded to use and fixes
for a peculiar error I faced.

1. First placed the script on a certain directory where I plan to use to place all of my scripts in the future. i.e: `C:\Users\%USER%\scripts\`
2. I proceeded to add that folder path to my current user environment variables. ***NB:*** Don't forget the last `\` slash after the folder name.
3. Make sure .py files are opened with Python.exe. You can do that by going to any `*.py` file properties and change the `Open With` application.

That should be it. But wait remember the peculiar error/information, at first this never worked, my script expected an argument to process the input data and even placing a
print function to print all arguments received, my script was only receiving the filename/script name which is usually `argv[0]`. Later on I learnt that one had to do a
registry edit for registry key: `HKEY_CLASSES_ROOT\Applications\python.exe\shell\open\command`.

- From: `"C:\Users\%USER%\AppData\Local\Programs\Python\Python311\python.exe" "%1"`
- To: `"C:\Users\%USER%\AppData\Local\Programs\Python\Python311\python.exe" "%1" "%*"`

I had to add  the `"%*"` to pass all arguments to `python.exe` as the above mentioned registry key controls the way `python.exe` shell. [Source](https://stackoverflow.com/questions/2640971/windows-is-not-passing-command-line-arguments-to-python-programs-executed-from-t) for solution.

This is messed up because I hate editing registry keys and worst of all registry keys under ***HKEY_CLASSES_ROOT***.

Also in the python script, if you will be handling files and the path of the file is required use the below code to your liking:

```py
import sys
import os

src = sys.argv[1].strip()         # strip leading/trailing whitespaces
file_path = os.path.abspath(src)  # get the absolute path of the file

if os.path.exists(file_path):
  # .....
  # process/workon file
  # .....
else:
  print(f"[!] Error: {src} file path not found.")
```

Bye, have fun.
