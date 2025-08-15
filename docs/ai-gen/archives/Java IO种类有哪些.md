<think>

</think>

Java IO（输入/输出）体系提供了丰富的功能，支持多种数据源和操作。以下是Java IO的主要分类及其常用类：

### 1. **按流类型分类**
   - **字节流**：以字节为单位，适合处理二进制文件。
     - `InputStream`（抽象输入流）
     - `OutputStream`（抽象输出流）
   - **字符流**：以字符为单位，适合处理文本文件。
     - `Reader`（抽象字符输入流）
     - `Writer`（抽象字符输出流）

### 2. **按数据源类型分类**
   - **文件IO**：
     - `FileInputStream`/`FileOutputStream`（字节流，读写文件）
     - `FileReader`/`FileWriter`（字符流，读写文件）
   - **网络IO**：
     - `Socket`（客户端套接字，读写网络数据）
     - `ServerSocket`（服务器套接字，监听客户端连接）
   - **内存IO**：
     - `ByteArrayInputStream`/`ByteArrayOutputStream`（字节数组流）
     - `StringReader`/`StringWriter`（字符串流）

### 3. **按功能扩展分类**
   - **缓冲流**：提高读写效率。
     - `BufferedInputStream`/`BufferedOutputStream`（字节缓冲流）
     - `BufferedReader`/`BufferedWriter`（字符缓冲流）
   - **转换流**：实现编码和解码。
     - `InputStreamReader`（将字节流转为字符流）
     - `OutputStreamWriter`（将字符流转为字节流）

### 4. **其他功能类**
   - **随机访问流**：
     - `RandomAccessFile`（支持随机读写文件`)
   - **管道流**：
     - `PipedInputStream`/`PipedOutputStream`（进程间通信）
   - **打印流**：
     - `PrintStream`（格式化输出）
     - `PrintWriter`（字符格式化输出）

### 5. **NIO（非阻塞IO）**
   - **通道**：`Channel`
   - **缓冲区**：`Buffer`
   - **选择器**：`Selector`

### 总结
Java IO体系根据流类型、数据源和功能扩展提供了多种类，开发者可根据需求选择合适的流来处理不同的输入/输出操作。