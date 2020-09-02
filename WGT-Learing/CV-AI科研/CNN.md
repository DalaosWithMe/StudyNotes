# [keras conv2D参数](https://www.cnblogs.com/yjybupt/p/11646846.html)

keras.layers.Conv2D(filters, kernel_size, strides=(1, 1), padding='valid', data_format=None, dilation_rate=(1, 1), activation=None, use_bias=True, kernel_initializer='glorot_uniform', bias_initializer='zeros', kernel_regularizer=None, bias_regularizer=None, activity_regularizer=None, kernel_constraint=None, bias_constraint=None)

 

该层创建了一个卷积核， 该卷积核对层输入进行卷积， 以生成输出张量。 如果 `use_bias` 为 True， 则会创建一个偏置向量并将其添加到输出中。 最后，如果 `activation` 不是 `None`，它也会应用于输出。

当使用该层作为模型第一层时，需要提供 `input_shape` 参数

比如训练样本是（6000，28，28，1）, 则input_shape=(28,28,1)

- **filters**: 整数，输出空间的维度 （即卷积中滤波器的数量）。

- **kernel_size**: 一个整数，或者 2 个整数表示的元组或列表， 指明 2D 卷积窗口的宽度和高度。 可以是一个整数，为所有空间维度指定相同的值。

- **strides**: 一个整数，或者 2 个整数表示的元组或列表， 指明卷积沿宽度和高度方向的步长。 可以是一个整数，为所有空间维度指定相同的值。 指定任何 stride 值 != 1 与指定 `dilation_rate` 值 != 1 两者不兼容。

- **padding**: `"valid"` 或 `"same"` (大小写敏感)。  valid padding就是不padding，而same padding就是指padding完尺寸与原来相同
  图像识别一般来说都要padding，尤其是在图片边缘的特征重要的情况下。padding多少取决于我们需要的输出是多少

- **data_format**: 字符串， `channels_last` (默认) 或 `channels_first` 之一，表示输入中维度的顺序。 `channels_last` 对应输入尺寸为 `(batch, height, width, channels)`， `channels_first` 对应输入尺寸为 `(batch, channels, height, width)`。 它默认为从 Keras 配置文件 `~/.keras/keras.json` 中 找到的 `image_data_format` 值。 如果你从未设置它，将使用 `channels_last`。

- **dilation_rate**: 一个整数或 2 个整数的元组或列表， 指定膨胀卷积的膨胀率。 可以是一个整数，为所有空间维度指定相同的值。 当前，指定任何 `dilation_rate` 值 != 1 与 指定 stride 值 != 1 两者不兼容。

- **activation**: 要使用的激活函数 (详见 [activations](https://keras.io/zh/activations/))。 如果你不指定，则不使用激活函数 (即线性激活： `a(x) = x`)。

- **use_bias**: 布尔值，该层是否使用偏置向量。

- **kernel_initializer**: `kernel` 权值矩阵的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。

- **bias_initializer**: 偏置向量的初始化器 (详见 [initializers](https://keras.io/zh/initializers/))。

- **kernel_regularizer**: 运用到 `kernel` 权值矩阵的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。

  - L2正则会整体的把w变小。

  - L1正则会倾向于使得w要么取1，要么取0 ，稀疏矩阵 ，可以达到降维的角度。

- **bias_regularizer**: 运用到偏置向量的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。

- **activity_regularizer**: 运用到层输出（它的激活值）的正则化函数 (详见 [regularizer](https://keras.io/zh/regularizers/))。

- **kernel_constraint**: 运用到 `kernel` 权值矩阵的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))。

- **bias_constraint**: 运用到偏置向量的约束函数 (详见 [constraints](https://keras.io/zh/constraints/))。

**输出尺寸**

- 如果 data_format='channels_first'， 输出 4D 张量，尺寸为 `(samples, filters, new_rows, new_cols)`。
- 如果 data_format='channels_last'， 输出 4D 张量，尺寸为 `(samples, new_rows, new_cols, filters)`。

**输入尺寸**

- 如果 data_format='channels_first'， 输入 4D 张量，尺寸为 `(samples, channels, rows, cols)`。
- 如果 data_format='channels_last'， 输入 4D 张量，尺寸为 `(samples, rows, cols, channels)`。



# keras fit_generator()参数

**参数：**

- **generator**：一个生成器，或者一个 Sequence (keras.utils.Sequence) 对象的实例。这是我们实现的重点，后面会着介绍生成器和sequence的两种实现方式。
- **steps_per_epoch**：这个是我们在每个epoch中需要执行多少次生成器来生产数据，fit_generator函数没有batch_size这个参数，是通过steps_per_epoch来实现的，每次生产的数据就是一个batch，因此steps_per_epoch的值我们通过会设为（样本数/batch_size）。如果我们的generator是sequence类型，那么这个参数是可选的，默认使用len(generator) 。
- **epochs**：即我们训练的迭代次数。
- **verbose**：0, 1 或 2。日志显示模式。 0 = 安静模式, 1 = 进度条, 2 = 每轮一行
- **callbacks**：在训练时调用的一系列回调函数。
- **validation_data**：和我们的generator类似，只是这个使用于验证的，不参与训练。
- **validation_steps**：和前面的steps_per_epoch类似。
- **class_weight**：可选的将类索引（整数）映射到权重（浮点）值的字典，用于加权损失函数（仅在训练期间）。 这可以用来告诉模型「更多地关注」来自代表性不足的类的样本。（感觉这个参数用的比较少）
- **max_queue_size**：整数。生成器队列的最大尺寸。默认为10.
- **workers**：整数。使用的最大进程数量，如果使用基于进程的多线程。 如未指定，workers 将默认为 1。如果为 0，将在主线程上执行生成器。
- **use_multiprocessing**：布尔值。如果 True，则使用基于进程的多线程。默认为False。
- **shuffle**：是否在每轮迭代之前打乱 batch 的顺序。 只能与Sequence(keras.utils.Sequence) 实例同用。
- **initial_epoch**: 开始训练的轮次（有助于恢复之前的训练）