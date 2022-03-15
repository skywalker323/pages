# 158 Read N Characters Given Read4 II – Call multiple times – Hard

### Problem:

The API: int read4\(char \*buf\) reads 4 characters at a time from a file.

The return value is the actual number of characters read. For example, it returns 3 if there is only 3 characters left in the file.

By using the read4 API, implement the function int read\(char \*buf, int n\) that reads n characters from the file.

Note:  
The read function may be called multiple times.

### Thoughts

The difference between this question and the first version is that the read\(\) function will be called multiple times.  
The trouble here will be as the following example if using the first version solution:  
file: “abcdefg”  
read\(3\) read\(2\) read\(2\) should be “abc” “de” “fg”  
but using first version solution it will print “abc” “ef” “”

This is because when you use read4\(\) to read, the pointer to read file has already moved to “e” after the first call of read4\(\). So it’s not correct any more.

In order to solve, we need to persist the characters that has been already read by using read4 but it’s not put into the result of read\(\).

In the solution below, I am using a buf4\[\] to store the characters read by using read4 and also a buf4Size and buf4Index to keep track of the size of the buf4 and the next character to use in buf4\[\].

### Solutions:

```java
 char buf4[4];
    int buf4_i = -1, buf4_n = -1;
    int read(char *buf, int n) {
        int read = 0;
        while (buf4_i < buf4_n and read < n) {
            *(buf++) = buf4[buf4_i++];
            read++;
        }
        while (read < n) {
            int actually_read = read4(buf4);
            for (buf4_i = 0; buf4_i<actually_read and read < n ; buf4_i++, read++)
                *(buf++) = buf4[buf4_i];
            buf4_n = actually_read;
            if (actually_read < 4) break;
        }
        return read;
    }
```



