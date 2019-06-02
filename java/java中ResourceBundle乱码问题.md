## 为什么ResourceBundle资源文件格式是UTF-8,但读取的中文字符却是乱码呢？
因为ResoureBundle在创建新的Bundle时候,  
`Properties pros = new Properties()；pros.load(ins);`
 (ins是通过类加载器获取的资源文件流)  
关键在于load方法,load方法对字节流转换使用的编码是ISO-8859-1，  
所以即使文件格式本身的utf-8，也会导致该资源文件内容乱码。
#### load方法
```java
/**
Reads a property list (key and element pairs) from the input
byte stream. The input stream is in a simple line-oriented
format as specified in load(Reader) and is assumed to use the
ISO 8859-1 character encoding; that is each byte is one Latin1 character. Characters not in Latin1, and certain special
characters, are represented in keys and elements using Unicode
escapes as defined in section 3.3 of The Java™ Language
Specification.
*/
public void load(InputStream inStream) throws IOException  
```
### 解决乱码问题
#### 方式一:先使用ISO-8859-1编码获取字节数组，再使用UTF-8编码
```java
new String(“”.getBytes("ISO-8859-1"), "UTF-8");
```
#### 方式二: 使用 JAVA_HOME/bin/native2ascii 工具,将源文件替换为ascii格式文件即可
native2ascii SOURCE_FILE DEST_FILE
```shell
native2ascii LanguageBundle_zh.properties LanguageBundle_zh.properties.properties.TEMP
```



```
