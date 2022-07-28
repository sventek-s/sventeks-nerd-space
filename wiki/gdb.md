# GDB Cheat Sheet

> This are just important stuff that I have used.

- [Managing inputs for payload injection](https://reverseengineering.stackexchange.com/questions/13928/managing-inputs-for-payload-injection)

  - Getting inputs from **char \*argv[]** `$> ./program $(echo -ne "\xef\xbe\xad\xde")`
  
  ```sh
  (gdb) run $(echo -ne "\xef\xbe\xad\xde")
  ```

  - Getting inputs from a file `$> ./program ./myfile.txt`
  
  ```sh
  (gdb) run myfile.txt
  ```
  
  - Getting inputs from **stdin** `$> cat ./mycommands.txt | ./program || echo -ne "\xef\xbe\xad\xde" | ./program`
  
  ```sh
  (gdb) run < ./mycommands.txt
  
  # or
  
  run < <(echo -ne "\xef\xbe\xad\xde")
  ```
  
  - Getting inputs from network `$> echo -ne "\xef\xbe\xad\xde" | nc -vv localhost 666`
  
  ```sh
  # keep stdin open after injection
  (cat ./mycommands.txt; cat) | ./program
  
  # or
  $> (echo -ne "\xef\xbe\xad\xde"; cat) | ./program
  
  # or
  $> (echo -ne "\xef\xbe\xad\xde"; cat) | nc -vv localhost 666
  ```
