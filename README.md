# kdio

A Java Direct IO framework which is very simple to use.

# sample

### notice

```
    // file path should be specific since the different file path determine whether your system support direct io
    public static DirectIOLib directIOLib = DirectIOLib.getLibForPath("/");
    // you should always write into your disk the Integer-Multiple of block size through direct io.
    // in most system, the block size is 4kb
    private static final int BLOCK_SIZE = 4 * 1024;
```

> Usually, only specific linux system support direct io. You should own a linux server.

### write

```java
    private static void write() throws IOException {
        if (DirectIOLib.binit) {
            ByteBuffer byteBuffer = DirectIOUtils.allocateForDirectIO(directIOLib, 4 * BLOCK_SIZE);
            for (int i = 0; i < BLOCK_SIZE; i++) {
                byteBuffer.putInt(i);
            }
            byteBuffer.flip();
            DirectRandomAccessFile directRandomAccessFile = new DirectRandomAccessFile(new File("./database.data"), "rw");
            directRandomAccessFile.write(byteBuffer, 0);
        } else {
            throw new RuntimeException("your system do not support direct io");
        }
    }
```

### read

```java
    public static void read() throws IOException {
        if (DirectIOLib.binit) {
            ByteBuffer byteBuffer = DirectIOUtils.allocateForDirectIO(directIOLib, 4 * BLOCK_SIZE);
            DirectRandomAccessFile directRandomAccessFile = new DirectRandomAccessFile(new File("./database.data"), "rw");
            directRandomAccessFile.read(byteBuffer, 0);
            byteBuffer.flip();
            for (int i = 0; i < BLOCK_SIZE; i++) {
                System.out.print(byteBuffer.getInt() + " ");
            }
        } else {
            throw new RuntimeException("your system do not support direct io");
        }
    }
```

### install & run

```
mvn clean install -DskipTests
```

you will get the sample of direct io test: DirectIOTest.jar

deploy it to a linux server and use

```java
java -jar DirectIOTest.jar
```

you can try it.

# License

kdio is under the Apache 2.0 license.