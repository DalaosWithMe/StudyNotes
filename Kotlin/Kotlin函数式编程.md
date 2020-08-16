# Kotlin函数式	

## with和apply

这两个方法可以使得在书写lambda表达式的时候可以省略对象名，比如要对视图绑定属性：

```kotlin
val titleTV = findViewById<View>(R.id.title)

with(bean){
  titleTV.text = title //bean的title
  titleTV.textSize = content
}
```

apply类似：

```kotlin
val titleTV = xxx

bean.apply{
	titleTV.text = title
  titleTV.textSize = content
}
```

