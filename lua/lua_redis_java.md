## Redis应用lua脚本
### 场景1
> 判断session是否存在,如果存在，则删除，否则设置相关参数  
KEYS[1]为usercode
ARGV[1]为shiro:session:SESSIONID

```lua
local function isNotEmpty(str)
  return str ~= nil and str ~= ''
end
local res
res = redis.call('get',KEYS[1])
if isNotEmpty(res)
  then redis.call('del',res)
end
redis.call('set',KEYS[1],ARGV[1])
```  

- 模拟示例:  

  ```shell
  set u4 s2
  set s2 u4
  redis-cli  -h 10.1.52.46 -a sinosoft --eval multiLogin.lua  u4 , s5
  get u4 s5
  ttl s2
  ```  
- Examples  

  ```shell
  redis-cli --eval myscript.lua key1 key2 , arg1 arg2 arg3
  ```
  ** 注意测试的时候，key value之间是用空格+逗号分隔 **
