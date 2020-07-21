```python
# -*- coding: utf-8 -*-
import os
import urllib.request
import urllib.error
import time

from PIL import Image


def find_face(filepath):
    http_url = 'https://api-cn.faceplusplus.com/facepp/v3/detect'
    key = 'j1eIBCbzSTcwoXFyHdrVn-3RURnwP3Nu'
	secret = 'w5RPm5a43r5DthilCgZiZFsBQxC0N8Si'

    boundary = '----------%s' % hex(int(time.time() * 1000))
    data = []
    data.append('--%s' % boundary)
    data.append('Content-Disposition: form-data; name="%s"\r\n' % 'api_key')
    data.append(key)
    data.append('--%s' % boundary)
    data.append('Content-Disposition: form-data; name="%s"\r\n' % 'api_secret')
    data.append(secret)
    data.append('--%s' % boundary)
    fr = open(filepath, 'rb')
    data.append('Content-Disposition: form-data; name="%s"; filename=" "' % 'image_file')
    data.append('Content-Type: %s\r\n' % 'application/octet-stream')
    data.append(fr.read())
    fr.close()
    data.append('--%s' % boundary)
    data.append('Content-Disposition: form-data; name="%s"\r\n' % 'return_landmark')
    data.append('1')
    data.append('--%s' % boundary)
    data.append('Content-Disposition: form-data; name="%s"\r\n' % 'return_attributes')
    data.append(
        "gender,age,smiling,headpose,facequality,blur,eyestatus,emotion,ethnicity,beauty,mouthstatus,eyegaze,skinstatus")
    data.append('--%s--\r\n' % boundary)

    for i, d in enumerate(data):
        if isinstance(d, str):
            data[i] = d.encode('utf-8')

    http_body = b'\r\n'.join(data)

    # build http request
    req = urllib.request.Request(url=http_url, data=http_body)

    # header
    req.add_header('Content-Type', 'multipart/form-data; boundary=%s' % boundary)

    try:
        resp = urllib.request.urlopen(req, timeout=5)
        # urllib.request.close()
        qrcont = resp.read()
        qrcont = qrcont.decode('utf-8')
        dictinfo = eval(qrcont)
        FACE = dictinfo['faces']
        return len(FACE)
    except urllib.error.HTTPError as e:
        return str(e.read().decode('utf-8'))

if __name__ == '__main__':
    project_dir = os.path.dirname(os.path.abspath('D:\\数据集\\data_set\\data\\val\\animals'))
    input = os.path.join(project_dir, 'animals')
    os.chdir(input)
    output1 = os.path.join(project_dir, 'hunman')
    output2 = os.path.join(project_dir, 'maybe_anmials')
    output3 = os.path.join(project_dir, 'other')

    if os.path.isdir(output1):
        print('exists' + '\n')
    else:
        os.mkdir(output1)
    if os.path.isdir(output2):
        print('exists' + '\n')
    else:
        os.mkdir(output2)
    if os.path.isdir(output3):
        print('exists' + '\n')
    else:
        os.mkdir(output3)



    for image_name in os.listdir(os.getcwd()):
        if image_name.endswith('jpg'):
            fd = os.path.join(input, image_name)
            #print(image_name)
            file = fd
            try:
                num = int(find_face(file))
                if num >0:
                    #print('-------------识别到人了-------------'+'\n')# 可能是识别了很多人 但是读取不到精准信息 这里说出一共多少人
                    im = Image.open(os.path.join(input, image_name))
                    im.save(os.path.join(output1, image_name))
                else:
                   # print('=============没识别到人============='+'\n')
                    im = Image.open(os.path.join(input, image_name))
                    im.save(os.path.join(output2, image_name))
            except BaseException as e:
                #print('**************sth wrong**************'+'\n')
                im = Image.open(os.path.join(input, image_name))
                im.save(os.path.join(output3, image_name))


```

