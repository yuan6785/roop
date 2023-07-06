conda activate py310_gpt4free
ffmpeg version 5.1.2
mac
换脸的模型(models/inswapper_128.onnx)我保存在百度网盘（机器学习资料＞换脸roop的模型保存）---------

一般不要用界面来生成，用命令行生成更方便
--frame-processor 可以用了选择是换脸还是人脸增强

生成视频(target可以为gif也可以为视频, 但重要： target必须是偶数分辨率):
--keep-fps --keep-frames 保持原视频的帧率和帧数
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yaonv.mp4  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.mp4

注意: 生成gif的target的帧数图片不要太多（否则内存会扛不住，一般30帧的视频控制在一分钟以内, 2000张图片以内）:
生成gif(target可以为gif也可以为视频, 记得配置--out-type和--gif-dur参数 --gif-frames参数（# 每张图显示的时间）   ---这两个参数是我加的)----
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0009.gif  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif --out-type gif --gif-dur 0.1 --many-faces --gif-frames -1

生成gif不换脸，只是人脸增强(一般用于换脸后的增强)(不需要source参数，只需要target参数)---也可以用于视频（如果是用于换脸后的增强，--gif-dur参数和换脸的--gif-dur最好一致）:
python run.py  --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3_enhancer.gif --out-type gif --gif-dur 0.1 --many-faces --frame-processor face_enhancer


视频换脸生成gif:
# 这里需要注意根据target的帧数和时长来计算生成的gif的gif-dur参数, 例如target有100帧, 时长为10s, 那么gif-dur就是10/100=0.1
# 确定是视频生成gif太大了，6M的视频生成的gif居然有100M
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yaonv.mp4  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.gif --out-type gif --gif-dur 0.0333 --many-faces --gif-frames -1



生成png图片:
python run.py --source /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0daifang.jpg --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/0yx.jpeg  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.png 

生成png图片的增强:
python run.py  --target /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3.png  --output /Users/yuanxiao/workspace/0yxgithub/roop/yxsamples/output3_enhancer.png --many-faces --frame-processor face_enhancer


cloab笔记本参考（有显卡，可以换比较大的视频，一般的gif本地mac即可）： ufyx_换脸.ipynb

换脸后再增强人脸的效果更好


后期要做:
     https://blog.csdn.net/weixin_42357472/article/details/131104449   
     https://huaweicloud.csdn.net/638088d7dacf622b8df89c0c.html
     有道搜索[人脸识别]--里面有上面两个文章的保存，和roop用的统一模型，还可以实时检测摄像头人脸


     ###############核心算法
     接下来编写recognition()函数实现人脸识别，首先获取每张人脸的特征embedding，其中使用人脸识别的就是通过欧氏距离来对比人脸库中的人脸特征，默认如何它们的欧氏距离小于1.24，我们就可以认为他们是同一个人。
    def recognition(self, image):
        faces = self.model.get(image)
        results = list()
        for face in faces:
            # 开始人脸识别
            embedding = np.array(face.embedding).reshape((1, -1))
            embedding = preprocessing.normalize(embedding)
            user_name = "unknown"
            for com_face in self.faces_embedding:
                r = self.feature_compare(embedding, com_face["feature"], self.threshold) # threshold=1.24
                if r:
                    user_name = com_face["user_name"]
            results.append(user_name)
        return results

     上面使用到的欧氏距离计算方式如下。
     @staticmethod
     def feature_compare(feature1, feature2, threshold):
          diff = np.subtract(feature1, feature2)
          dist = np.sum(np.square(diff), 1)
          if dist < threshold:
               return True
          else:
               return False
     ###############