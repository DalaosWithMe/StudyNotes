```python
# Convolutional Neural Network

# Installing Theano
# pip install --upgrade --no-deps git+git://github.com/Theano/Theano.git

# Installing Tensorflow
# Install Tensorflow from the website: https://www.tensorflow.org/versions/r0.12/get_started/os_setup.html

# Installing Keras
# pip install --upgrade keras
import numpy as np
# Part 1 - Building the CNN
import matplotlib.pyplot as plt
# Importing the Keras libraries and packages
from keras.models import Sequential
from keras.layers import Convolution2D, Dropout, AveragePooling2D
from keras.layers import MaxPooling2D
from keras.layers import Flatten
from keras.layers import Dense

# Initialising the CNN
classifier = Sequential()

# Step 1 - Convolution
classifier.add(Convolution2D(32, 3, 3, input_shape = (64, 64, 3), activation = 'relu'))

# Step 2 - Pooling
classifier.add(AveragePooling2D(pool_size = (2, 2)))

# Adding a second convolutional layer
classifier.add(Convolution2D(32, 3, 3, activation = 'relu'))
classifier.add(AveragePooling2D(pool_size = (2, 2)))


# Step 3 - Flattening
classifier.add(Flatten())

# Step 4 - Full connection
classifier.add(Dense( units= 128, activation = 'relu'))
classifier.add(Dropout(0.2))
classifier.add(Dense( units= 1, activation = 'sigmoid'))

# Compiling the CNN
classifier.compile(optimizer = 'adam', loss = 'binary_crossentropy', metrics = ['accuracy'])

# Part 2 - Fitting the CNN to the images

from keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator(rescale = 1./255)

test_datagen = ImageDataGenerator(rescale = 1./255)

training_set = train_datagen.flow_from_directory('D:/360WiFi/training_set',
                                                 target_size = (64, 64),
                                                 batch_size = 32,
                                                 class_mode = 'binary')

test_set = test_datagen.flow_from_directory('dataset/test_set',
                                            target_size = (64, 64),
                                            batch_size = 32,
                                            class_mode = 'binary')

classifier.fit_generator(training_set,
                         steps_per_epoch = 250,
                         epochs = 30,
                         validation_data = test_set,
                         validation_steps = 62.5)

res = classifier.evaluate(test_set,batch_size=25)  # 返回损失和精度
#最后，使用evaluate方法来评估模型
#到这一步，我们创建了模型
# 接下来就是调用fit函数将数据提供给模型
print(classifier.metrics_names);'''。
这里还可以指定批次大小（batch size）、迭代次数、验证数据集等等。
其中批次大小、迭代次数需要根据数据规模来确定，并没有一个固定的最优值。'''
print(res)

prob = classifier.predict(test_set)
print(np.argmax(prob))
print(np.argmax(prob))
print(np.argmax(prob))
```