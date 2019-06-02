### Before Java8
filter a `list` Just like this.
```java
public class ArraysExample {

    public static void main(String[] args) {
        List<String> list = Arrays.asList("java", "python", "c", "php");
        //过滤元素
        List<String> newList = filter(list, "php");
        for (String str : newList) {
            System.out.println(str);
        }
    }

    private static List filter(List<String> list, String filterStr) {
        ArrayList<Object> newList = new ArrayList<>();
        for (String str :
                list) {
            if(!str.equals(filterStr)){
                newList.add(str);
            }
        }
        return newList;
    }
}
```  
Output
```txt
java
python
c
```
### After And Include Java8
- `stream.filter()`:filter a list  
- `stream.filter()`:convert stream into a list
```java
public static void filterAndCollect(){
       List<String> list = Arrays.asList("java", "python", "c", "php");
       //filter方法中：true为保留元素，false为删除元素
       List<String> newList = list.stream().filter(x -> !"php".equals(x)).collect(Collectors.toList());
       printResult(newList);

   }
```
Output
```java
java
python
c
```
