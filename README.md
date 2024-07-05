# test-Deep-Stereo-Matching
测试双目匹配模型，资源列表取自 https://github.com/fabiotosi92/Awesome-Deep-Stereo-Matching
## 测试环境
- 主机 Ryzen 7 5800X, RTX 3090. OS: Ubuntu 20.04
- 图片使用双目相机在室内环境拍摄，经opencv标定并矫正，原生分辨率2592x1520
## 测试的模型
### Unimatch
- GMStereo-scale2-regrefine3-resumeflowthings-mixdata: [768,1024] 0.82s
  - 物体细节清晰
  - 地板的视差会按倒影内容生成
- scale1-sceneflow: [768,1024] 0.41s
  - 相对于`scale2-regrefine3`, 物体边缘细节出现更多的模糊
- 小分辨率下 [512,384] 视差图出现马赛克状分块  
- 未矫正的图片也能给出一定结果
### CREStereo
- 使用模型默认尺寸1024x1536，单次推理耗时约1.58s。
- 总体匹配效果很好，远近物体边缘清晰，视差符合实际情况
- 植物叶子具有清晰结构和边缘
- 墙面边缘和凸出物清晰
- 一部分的地砖（黑色，有一定反光，存在倒影）视差错误（小于实际）
- 未矫正的图片也能给出一定结果
### NMRF
- 视差上限192, sceneflow.pth [1296,760] 1.05s, [512,384] 0.93s,
- 物体细节清晰
- 地砖视差会按倒影内容生成  
### OpenStereo
- 视差上限192
- LightStereo-S-SceneFlow.ckpt [544,960] 0.91s
  - 部分物体大面积错误，平整的物体表面视差却不平整
  - 叶子形状模糊或丢失
### BGNet
- 视差上限192。缩放图片至648x380/1296x760，单次推理约0.23s
- 648x380下
  - 视差较小的远处物体基本全部丢失
  - 植物结构和叶子边缘形状丢失
  - 地砖部分完整
  - 墙体竖直边缘清晰
- 1296x760 相比于 648x380 有明显改善
  - 远处物体基本保留
  - 一部分植物结构和树叶边缘保留
### IINet
- 使用配置/模型 sceneflow.yaml/sceneflow.pth 因视差上限为192，图片缩放至648x380，单次推理约0.32s
- 视差图上边缘若干行有明显异常输出
- 远物体基本全部丢失
- 较远的小物体丢失
- 物体的结构（如镂空部分）被忽略
- 墙面上水平朝右凸出的物体丢失
- 地砖基本不正确
- 1296x760相比648x380没有明显改善
### TemporalStereo
- 使用sceneflow预训练模型，默认尺寸544x960，单次推理约0.46s
- 总体误差较大，明显差于其他模型，远处误差大于近处
- 相邻的近/远处物体之间的输出结果会互相传播干扰，导致大片错误，边缘不清
- 有不少植物结构和叶子边缘细节，也有不少丢失和错误
- 远处物体能显示不少，但不少物体和很多镂空部分丢失
- 地砖基本不正确
- 国际象棋标定板的每个相邻格子的视差都不同
### HITNet
- 使用hitnet_sf_finalpass.ckpt,width=1280，单次推理约0.22s
- 存在与TemporalStereo同样的问题，物体表面的错误部分更多且误差更大
### MObileStereoNet
- 2D/[288, 512] 单次推理1.27s 
- 没有物体细节
### Temporally Consistent Stereo Matching
- [960,544] 1.1s
### Selective Stereo
- Selective-IGEV `sceneflow.path`[960,544] 1.34s
### Parameterized Cost Volume for Stereo Matching
显存要求25g，跑不了
### AnyNet
代码不可用

