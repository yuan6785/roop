conda activate py310_gpt4free
一般不要用界面来生成，用命令行生成更方便

生成视频(target可以为gif也可以为视频, 但重要： target必须是偶数分辨率):
python run.py --source /Users/yuanxiao/Downloads/0daifang.jpg --target /Users/yuanxiao/Downloads/0yaonv.mp4  --output /Users/yuanxiao/Downloads/output3.mp4

生成gif(记得配置--out-type和--gif-dur参数（# 每张图显示的时间）   ---这两个参数是我加的):
python run.py --source /Users/yuanxiao/Downloads/0daifang.jpg --target /Users/yuanxiao/Downloads/0009.gif  --output /Users/yuanxiao/Downloads/output3.gif --out-type gif --gif-dur 0.1


cloab笔记本参考（有显卡，可以换比较大的视频，一般的gif本地mac即可）： ufyx_换脸.ipynb