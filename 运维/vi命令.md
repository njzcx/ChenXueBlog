

- 打开文件：`vi filename`

命令模式下： 

- 转到文件结尾: `G` 

- 转到第9行: `9G` 

- 删除所有内容(先用G转到文件尾) : `1,.d` 

- 删除第9行到第200行的内容(先用200G转到第200行) : `9,.d`, 说明：这是在vi中 ，“.”当前行 ，“1,.”表示从第一行到当前行 ，“d”删除，3dd代表删除三行。

- 删除光标所在处字符: `x`

- 删除光标所在前字符（大写 X ）: `X`

- 删除到下一个单词开头: `dw`

- 删除到本单词末尾: `de`

- 删除到本单词末尾包括标点在内: `dE`

- 删除到前一个单词: `db`

- 删除到前一个单词包括标点在内: `dB`

- 删除一整行: `dd`

- 删除光标位置到本行结尾: `d$`

- 删除光标位置到本行开头: `d0`

