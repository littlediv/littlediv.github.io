# 排序 Collections 中 Comparator 作用猜想#

Collections 默认排序为升序：

	List<String> names = Arrays.asList("peter", "anna", "mike", "xenia");
	Collections.sort(names);
	
结果： anna, mike, peter, xeria

Collections 实现自定义比较方法：

	Collections.sort(names1, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                int i = o1.compareTo(o2);
                Log.e(TAG, "o1(+"+o1+").compareTo(o2 "+o2 +")"+ i );
                return i;
            }
        });

结果： anna, mike, peter, xeria

原理猜想：

public int compare(String o1, String o2)

返回值 int : 大于等于0 次序不变 小于0 次序颠倒

参数 o1: 列表后一个值

参数 o2：列表前一个值

过程：原列表次序 "peter", "anna", "mike", "xenia"

1. o1(+anna).compareTo(o2 peter)-15 次序颠倒 anna peter
2. o1(+mike).compareTo(o2 anna)12 次序正常 anna mike
3. o1(+mike).compareTo(o2 peter)-3 mike再和前一个Peter比 次序颠倒 mike peter
4. o1(+mike).compareTo(o2 anna)12 次序不变 anna mike
5. o1(+xenia).compareTo(o2 mike)11 次序不变 mike xeria
6. o1(+xenia).compareTo(o2 peter)8 次序不变 peter xenia
7. 最后结果 anna mike peter xenia


so 升序排列

peter(o2) anna(o1) 

第一种 o2.compareTo(o1) 返回负比较值

第二种 o1.compareTo(o2) 返回正常比较值








