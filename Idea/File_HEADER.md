## 设置文件头信息
1. Preferences->Editor->File and Code Templates->Includes->File Header
2. 编辑如下信息
```java
/**
 * Class Describe
 *
 * @author ${USER}
 * @date ${YEAR}/${MONTH}/${DAY}
 */
```
3. 其中USER变量默认读取的是os.user。  
通过更改jvm启动参数，添加-Duser.name=USERNAME，可以做到不修改系统用户名而更改${USER}的目的。  
具体修改方式如下：  

  - Help->Edit Custom VM Options
  - 添加参数
  ```java
  -Duser.name=Tobby Quinn
  ```
