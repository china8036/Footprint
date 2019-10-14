

## TODO:

http://linuxtools-rst.readthedocs.io/zh_CN/latest/tool/index.html

## md5sum

http://man.linuxde.net/md5sum

- -b：二进制模式读取文件。
- -t或--text：把输入的文件作为文本文件看待。
- -c：从指定文件中读取MD5校验和，并进行校验。
- --status：验证成功时不输出任何信息。
- -w：当校验不正确时给出警告信息。

eg:
- 生成一个文件insert.sql的md5值：
  ```
  [root@localhost ~]# md5sum insert.sql
  bcda6cb5c704664f989703ac5a88f112  insert.sql
  ```
- 检查文件testfile是否被修改过：
  ```
  md5sum testfile > testfile.md5
  md5sum testfile -c testfile.md5
  ```

