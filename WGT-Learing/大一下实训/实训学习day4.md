```python
#base_model = ResNet50('D:\\数据集\\animals_else.h5')
#image_path = 'D:\\数据集\\ss\\00000000.jpg'
import os

import os
from PIL import Image
import matplotlib
from matplotlib.image import imread

matplotlib.use('Agg')
import os
from keras.models import load_model
import numpy as np
from PIL import Image
import cv2
#加载模型h5文件
model = load_model("D:\\数据集\\animals_else.h5")
model.summary()
#规范化图片大小和像素值
def cv_imread(filePath):
    cv_img=cv2.imdecode(np.fromfile(filePath,dtype=np.uint8),-1)
    ## imdecode读取的是rgb，如果后续需要opencv处理的话，需要转换成bgr，转换后图片颜色会变化
    #cv_img=cv2.cvtColor(cv_img,cv2.COLOR_RGB2BGR)
    return cv_img

def get_inputs(src=[]):
    pre_x = []
    for s in src:
        input = cv_imread(s)
        input = cv2.resize(input, (150, 150))
        input = cv2.cvtColor(input, cv2.COLOR_BGR2RGB)
        pre_x.append(input)  # input一张图片
    pre_x = np.array(pre_x)/255
    return pre_x
#要预测的图片保存在这里
predict_dir = 'D:\\数据集\\ss'
#这个路径下有两个文件，分别是cat和dog
test = os.listdir(predict_dir)
#打印后：['cat', 'dog']
print(test)
#新建一个列表保存预测图片的地址
images = []
#获取每张图片的地址，并保存在列表images中
for testpath in test:
    if testpath.endswith('jpg'):
        fd = os.path.join(predict_dir, testpath)
        print(fd)
        images.append(fd)
#调用函数，规范化图片
pre_x = get_inputs(images)
#预测
pre_y = model.predict(pre_x)
print(pre_y)
```

