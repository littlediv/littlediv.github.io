# Intent 传递数据

## activity 之间通过intent传递TreeMap时出现 java.lang.ClassCastException: java.util.HashMap cannot be cast to java.util.TreeMap

The first link that Emmanuel provided is the exact same issue you are having. It doesn't specifically mention that exact error but it is implied.

The issue is that when your TreeMap is serialized it will come back as a HashMap. Completely explained by this answer which Emmanuel also provided.

So simply doing the follow shall resolve your issue:

	TreeMap<Long, Long> results = new TreeMap<Long, Long>((HashMap<Long, Long>) getIntent().getSerializableExtra("results"));

Basically populates your results TreeMap with the HashMap returned from the bundle.

Also upvote those answers in the links provided if this resolves your issue.


## 参考信息

[java.lang.ClassCastException: java.util.HashMap cannot be cast to java.util.TreeMap [duplicate]](http://stackoverflow.com/questions/22867427/java-lang-classcastexception-java-util-hashmap-cannot-be-cast-to-java-util-tree)