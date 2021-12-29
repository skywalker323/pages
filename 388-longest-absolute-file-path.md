# 388 Longest Absolute File Path

### Problem:

Suppose we abstract our file system by a string in the following manner:

The string "dir\n\tsubdir1\n\tsubdir2\n\t\tfile.ext" represents:

```
dir
    subdir1
    subdir2
        file.ext
```

The directory dir contains an empty sub-directory subdir1 and a sub-directory subdir2 containing a file file.ext.

The string "dir\n\tsubdir1\n\t\tfile1.ext\n\t\tsubsubdir1\n\tsubdir2\n\t\tsubsubdir2\n\t\t\tfile2.ext" represents:

```
dir
    subdir1
        file1.ext
        subsubdir1
    subdir2
        subsubdir2
            file2.ext
```

The directory dir contains two sub-directories subdir1 and subdir2. subdir1 contains a file file1.ext and an empty second-level sub-directory subsubdir1. subdir2 contains a second-level sub-directory subsubdir2 containing a file file2.ext.

We are interested in finding the longest \(number of characters\) absolute path to a file within our file system. For example, in the second example above, the longest absolute path is "dir/subdir2/subsubdir2/file2.ext", and its length is 32 \(not including the double quotes\).

Given a string representing the file system in the above format, return the length of the longest absolute path to file in the abstracted file system. If there is no file in the system, return 0.

Note:  
The name of a file contains at least a . and an extension.  
The name of a directory or sub-directory will not contain a ..  
Time complexity required: O\(n\) where n is the size of the input string.

Notice that a/aa/aaa/file1.txt is not the longest file path, if there is another path aaaaaaaaaaaaaaaaaaaaa/sth.png.

### Solutions:

```java
public class Solution {
    int lengthLongestPath(string input)
    {
        // check the example:
        // 
        // line 0:  dir
        // line 1:  ⟶ subdir1
        // line 2:  ⟶ ⟶ file1.ext
        // line 3:  ⟶ ⟶ subsubdir1
        // line 4:  ⟶ subdir2
        // line 5:  ⟶ ⟶ subsubdir2
        // line 6:  ⟶ ⟶ ⟶ file2.ext
        //
        // we see each line there is only one name, either dir name or file name

        int level = 0;                              // how many tabs saw in current line
        int length = 0;                             // length of the dir/file name
        int isFile = false;                         // saw '.', used for 'xxx.txt' for file only
        int maxPathLength = INT_MIN;
        std::unordered_map<int, int> levelLength;   // name length for all prefix levels, get overriden when jumps from line 3 to line 4

        for(const auto ch: input){
            switch(ch){
                case '\n':
                    {
                        level = 0;
                        length = 0;
                        isFile = false;
                        break;
                    }
                case '\t':
                    {
                        level++;
                        break;
                    }
                default:
                    {
                        length++;
                        levelLength[level] = length;

                        if(ch == '.'){
                            isFile = true;
                        }

                        if(isFile){
                            int prefixLength = 0;
                            for(int i = 0; i < level; ++i){
                                prefixLength += levelLength.at(i);
                                prefixLength += 1; // for the delimiter '/'
                            }
                            maxPathLength = std::max<int>(maxPathLength, prefixLength + length);
                        }
                        break;
                    }
            }
        }
        return maxPathLength >= 0 ? maxPathLength : 0;
    }
}
```



