conda activate py310_gpt4free
ffmpeg version 5.1.2
mac
换脸的模型(models/inswapper_128.onnx)我保存在百度网盘（机器学习资料＞换脸roop的模型保存）---------

一般不要用界面来生成，用命令行生成更方便
--frame-processor 可以用了选择是换脸还是人脸增强

生成视频(target可以为gif也可以为视频, 但重要： target必须是偶数分辨率):
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yaonv.mp4  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.mp4

注意: 生成gif的图片不要太多(不能超过250张):
生成gif(target可以为gif也可以为视频, 记得配置--out-type和--gif-dur参数 --gif-frames参数（# 每张图显示的时间）   ---这两个参数是我加的)----
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0009.gif  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif --out-type gif --gif-dur 0.1 --many-faces --gif-frames -1

# 这里需要注意根据target的帧数和时长来计算生成的gif的gif-dur参数, 例如target有100帧, 时长为10s, 那么gif-dur就是10/100=0.1
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yaonv.mp4  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif --out-type gif --gif-dur 0.0333 --many-faces --gif-frames -1

生成gif不换脸，只是人脸增强(不需要source参数，只需要target参数)---也可以用于视频:
python run.py  --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0009.gif  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif --out-type gif --gif-dur 0.1 --many-faces --frame-processor face_enhancer

生成png图片:
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yx.jpeg  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.png 


cloab笔记本参考（有显卡，可以换比较大的视频，一般的gif本地mac即可）： ufyx_换脸.ipynb