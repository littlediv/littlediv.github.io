# Java HashMap #

## 集合类的整体架构 ##

- Abstract Collection
	- Abstract List
		- Abstract Sequential List
			- Linked List  
		- Array List
	- Abstract Set
		- Hash Set
		- Tree Set
	- Abstract Queue
		- Priority Queue


- Abstract Map
	- Hash Map
	- Tree Map


## HashMap 详解 ##

HashMap 和 HashSet 是 Java Collection Framework 的两个重要成员，其中 HashMap 是 Map 接口的常用实现类，HashSet 是 Set 接口的常用实现类。虽然 HashMap 和 HashSet 实现的接口规范不同，但它们底层的 Hash 存储机制完全一样，甚至 ** HashSet 本身就采用 HashMap 来实现的(使用HashMap的key来存储HashSet的值，value是一个无意义的对象) **

	HashMap<String , Double> map = new HashMap<String , Double>();   
	map.put("高数" , 60.0);   
	map.put("大英" , 89.0);   
	map.put("大物" , 78.2);   

HashMap 采用一种所谓的“Hash 算法”来决定每个元素的存储位置。 

当程序执行 map.put("高数" , 60.0); 时，系统将调用"高数"的 hashCode() 方法得到其 hashCode 值——每个 Java 对象都有 hashCode() 方法，都可通过该方法获得它的 hashCode 值。得到这个对象的 hashCode 值之后，系统会根据该 hashCode 值来决定该元素的存储位置。 

## HashMap 将可变对象作为Key ##

### 可变对象 ###

可变对象是指创建后自身状态能改变的对象。换句话说，可变对象是该对象在创建后它的哈希值可能被改变。

### 解决方案 ###

重写hashcode 和 equal 

	@Override
    public int hashCode() {
        int result = 17;
        result = result * 31 + zoom;
        result = result * 31 + x;
        result = result * 31 + y;
        return result;
    }

    @Override
    public boolean equals(Object o) {
        if (o == this) {
            return true;
        }

        if (!(o instanceof LevelData))
        {
            return false;
        }

        LevelData other = (LevelData) o;

        return zoom == other.zoom && x == other.x && y == other.y;
    }


## 参考信息 ##

[java HashMap那点事](http://www.cnblogs.com/0201zcr/p/4769108.html)
[HashMap的key可以是可变的对象吗？？？](http://www.cnblogs.com/0201zcr/p/4810813.html?tvd)