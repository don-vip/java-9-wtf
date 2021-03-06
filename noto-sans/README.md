# Performance Regression with Noto Sans

Working with Noto Sans fonts on Java 9 takes considerably longer than on Java 8 (at least on Linux).
The root seems to be the creation of the `T2KFontScaler`, which happens deep in the bowels of computing a component's preferred size:

```java
Font2D font = FontUtilities.getFont2D(new Font("Noto Sans CJK JP Black", 0, 12));
Constructor<?> constructor = Class
		.forName("sun.font.T2KFontScaler")
		.getConstructor(Font2D.class, int.class, boolean.class, int.class);
constructor.setAccessible(true);
// for this to not end in a heap dump you need to have Noto Sans CJK JP Black
// installed on your machine
Object scaler = constructor.newInstance(font, 0, true, 18604592);
```

If this happens in a tight loop (e.g. to display the size of a list of labels, each sporting a different font), the regression is so devastating that the only solution is to exclude these fonts.

[The test](src/test/java/wtf/java9/noto_sans/NotoSansTest.java) tries to demonstrate the performance variance by failing the test if it runs longer than half a second but whether that is enough to succeed on Java 8 and fail on Java 9 of course depends on the machine.

(Last checked: 8u131 and 9-ea+172-jigsaw)

Two JDK-issues ([JDK-8168288](https://bugs.openjdk.java.net/browse/JDK-8168288) and [JDK-8074562](https://bugs.openjdk.java.net/browse/JDK-8074562)) seem related but are supposed to be resolved and available in ea-170 - so how is this still a thing?
