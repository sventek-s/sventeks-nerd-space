# Vim Cheat Sheet

> This is not a holy grail, just a dump of notes.

- [How to comment multiple lines at once](https://linuxhint.com/comment-multiple-lines-vim/#:~:text=Using%20the%20up%20and%20down,out%20all%20the%20highlighted%20lines.)

  ```sh
  # my best option visual mode
  1. enter visual mode
  2. select lines you want to comment
  3. press SHIFT + i to enter insert mode
  4. enter the comment symbol (note it will comment all the lines once you have pressed ESC key)
  5. press ESC key.
  ```

- After copy pasting codes in vim, the indentation is usually messed up alot of times. Here is a way to unindent multiple lines.

  ```sh
  1. enter visual mode
  2. select lines you want to unindent
  3. press the < shift to left
  4. press . to repeat the indent
  ```
- [Quick Movement](https://vim.fandom.com/wiki/Moving_around)

  ```sh
  ^: move to the begining of the line
  $: move to the end of of the line
  ```

- Increament and Decrement number... [VIM Tips](https://youtu.be/sQA63cI1khU)

  ```sh
  CTRL+a: ++
  CTRL+x: --
  ```
