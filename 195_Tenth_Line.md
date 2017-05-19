# Question
How would you print just the 10th line of a file?

For example, assume that **file.txt** has the following content:
```
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 7
Line 8
Line 9
Line 10
```
Your script should output the tenth line, which is:
```
Line 10
```

# Answer #1
**head + tail + 管道**，但若 **file.txt** 文件少于十行，则输出最后一行。
```bash
head -10 file.txt | tail -1
```

# Answer #2
```bash
sed -n '10 p' file.txt
```
