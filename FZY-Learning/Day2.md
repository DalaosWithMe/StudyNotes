# 数组

- 序排列
- 引用数据类型的变量。数组的元素既可以是基本数据类型，也可以是引用数据类型
- 一整块连续空间
- 长度确定不能更改



## 一维数组的声明与初始化

​       

```java
int[] ids;//声明
```



- #### 静态初始化：数组的初始化和数组的元素的赋值操作同时进行

  ```java
  ids = new int[] {1,2,3,4};
  ```

  

- #### 动态初始化：数组的初始化和数组的元素的赋值操作分开进行

  ```java
  String[] names = new String[5];
  
  int[] arr = {1,2,3,4,5};//类型推断
  ```

  

## 一维数组元素的调用（略）

## 一维数组的长度和遍历（略）

## 一维数组的默认初始化值（略）



## 理解二维数组

如果一个一维数组的元素是一维数组，称该数组为二维数组（表述有问题，仅供理解）

## 二维数组的声明和初始化

- 静态初始化：

  ```java
  int[][] arr = new int[][]{{1,2,3},{4,5},{7,8}};
  ```

  

- 动态初始化：

  ```java
  String[][] arr = new String[3][2];
  
  String[][] arr = new String[3][];
  ```

  

## 二维数组元素的调用（略）

## 二维数组的长度和遍历（略）

以杨辉三角形为例

```java
int[][] arr = new int[5][];
for(i = 0;i < arr.length;i++){
    arr[i] = new int[i + 1];
    arr[i][0] = arr[i][i] = 1;
    for(j = 1; j < arr[i].length - 1;j++){
    arr[i][j] = arr[i - 1][j - 1] + arr[i - 1][j];
    }
}
for(i = 0;i < arr.length;i++){
    for(j = 0; j < arr[i].length;j++){
    	System.out.print(arr[i][j], " ");
    }
    System.out.println();
}
```



## 二维数组的默认初始化值（略）



## Arrays工具类：equals、toString、fill、sort



### 