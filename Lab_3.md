# Lab Report 3
## Part 1
__failure-inducing input__
```
$  java -cp ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ArrayTests.java
JUnit version 4.13.2
.E
Time: 0.003
There was 1 failure:
1) initializationError(org.junit.runner.JUnitCommandLineParseResult)
java.lang.IllegalArgumentException: Could not find class [ArrayTests.java]
        at org.junit.runner.JUnitCommandLineParseResult.parseParameters(JUnitCommandLineParseResult.java:100)
        at org.junit.runner.JUnitCommandLineParseResult.parseArgs(JUnitCommandLineParseResult.java:50)
        at org.junit.runner.JUnitCommandLineParseResult.parse(JUnitCommandLineParseResult.java:44)
        at org.junit.runner.JUnitCore.runMain(JUnitCore.java:72)
        at org.junit.runner.JUnitCore.main(JUnitCore.java:36)
Caused by: java.lang.ClassNotFoundException: ArrayTests.java
        at java.base/jdk.internal.loader.BuiltinClassLoader.loadClass(BuiltinClassLoader.java:641)
        at java.base/jdk.internal.loader.ClassLoaders$AppClassLoader.loadClass(ClassLoaders.java:188)
        at java.base/java.lang.ClassLoader.loadClass(ClassLoader.java:521)
        at java.base/java.lang.Class.forName0(Native Method)
        at java.base/java.lang.Class.forName(Class.java:495)
        at java.base/java.lang.Class.forName(Class.java:474)
        at org.junit.internal.Classes.getClass(Classes.java:42)
        at org.junit.internal.Classes.getClass(Classes.java:27)
        at org.junit.runner.JUnitCommandLineParseResult.parseParameters(JUnitCommandLineParseResult.java:98)
        ... 4 more

FAILURES!!!
Tests run: 1,  Failures: 1
```
__input that doesn’t induce a failure__
```
$  java -cp ".;lib/junit-4.13.2.jar;lib/hamcrest-core-1.3.jar" org.junit.runner.JUnitCore ArrayTests
JUnit version 4.13.2
..
Time: 0.013

OK (2 tests)
```
__symptom__
![Image](failure.png)<br>
__before-and-after code__<br>
__before__
```
  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length; i += 1) {
      arr[i] = arr[arr.length - i - 1];
    }
  }
```
__after__
```
  // Changes the input array to be in reversed order
  static void reverseInPlace(int[] arr) {
    for(int i = 0; i < arr.length/2; i += 1) {
      int temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
  }
```
## Part 2
__command "find"__<br>
option 1: search by type<br>
_example 1: search all files_
```
[user@sahara ~/lab-report-3-folder]$ find -type f
./failure.png
./file_2
./file_1
```
_example 2: search all directories_
```
[user@sahara ~/lab-report-3-folder]$ find -type d
.
./folder_1
./folder_1/folder_2
```
option 2: search by size<br>
_example 1: search all with size greater than 10bytes_
```
[user@sahara ~/lab-report-3-folder]$ find -size +10
./failure.png
```
_example 2: search all with size less than 1MB_
```
[user@sahara ~/lab-report-3-folder]$ find -size -1M
./file_2
./file_1
```

option 3: search by time<br>
_exampe 1: search for files modified in 7 days_
```
[user@sahara ~/lab-report-3-folder]$ find -mtime -7
.
./folder_1
./folder_1/folder_2
./failure.png
./file_2
./file_1
```
_exampe 2: search for files changed in 7 days_
```
[user@sahara ~/lab-report-3-folder]$ find -ctime -7
.
./folder_1
./folder_1/folder_2
./failure.png
./file_2
./file_1
```
option 4: search with cimbined conditions<br>

_example 1: search all directories changed in 7 days_
```
[user@sahara ~/lab-report-3-folder]$ find -ctime -7 -and -type d
.
./folder_1
./folder_1/folder_2
```
_example 2: search all directories or files with size greater than 1 MB_
```
[user@sahara ~/lab-report-3-folder]$ find -type d -or -size +1M
.
./folder_1
./folder_1/folder_2
./8fc39ba7
```
__Get help from chatGPT__
