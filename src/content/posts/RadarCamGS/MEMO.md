---
title: COLMAP、3DGS重建流程备忘
date: 2026-03-26T10:54:27.000Z
tags: [COLMAP,3DGS]
category: 个人工作备忘、总结
comments: true
summary: MEMO
---


# LOD-3DGS
流程 图片->colmap重建->DepthAnything提取深度图->train.py->render.py
（记得项目作者说用CUDA12.7跑会出错，我用的是11.8没啥问题）

**我的常用指令**

## COLMAP重建



基本步骤是 先抽帧 初步重建 后增加不抽帧的注册进初步重建的轨迹里

**为什么要抽帧**
A：colmap重建靠视角差异三角化来判断轨迹 图片太相近容易出现病态解 矩阵不正交
**为什么补注册**
A：抽帧之后太稀疏3DGS效果不好

1、colmap feature_extractor \ 
  --database_path database.db \
  --image_path images \
  --ImageReader.single_camera 1 \
  --ImageReader.camera_model PINHOLE \
  --ImageReader.camera_params 1439.289304,1451.979537,955.342315,607.614894

特征提取 PINHOLE模型（针孔） 内参系数少几个 但是LOD-3DGS只支持这个，目前跑下来问题不大

database.db是存储特征信息等文件，这一步执行会自动创建

上面命令的内参来自radelft dataset

2、特征匹配 有三种

colmap sequential_matcher \
    --database_path database.db \
    --SequentialMatching.overlap 20 
时间序列匹配，适合radelft等按时间顺序排列的视频帧 overlap是前后窗口
只会在前后20张匹配

colmap exhaustive_matcher \
    --database_path database.db \
    --SiftMatching.use_gpu=1 
穷举匹配 匹配慢 对radelft这种不走回头路的，穷举效果不好



OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 MKL_NUM_THREADS=1 \

colmap sequential_matcher \
  --database_path database.db \
  --SequentialMatching.overlap 30 \
  --SequentialMatching.loop_detection 1 \
  --SequentialMatching.loop_detection_period 10 \
  --SequentialMatching.loop_detection_num_images 10 \
  --FeatureMatching.guided_matching true
时间序列匹配加回环检测 上面一句也要一起输入不然报错

我一般是第一种第三种换着用

3、mapper
colmap mapper \
  --database_path database.db \
  --image_path images \
  --output_path sparse \
  --Mapper.multiple_models 0 \
  --Mapper.min_num_matches 15 \
  --Mapper.init_min_num_inliers 40 \
  --Mapper.init_max_error 5 \
  --Mapper.init_min_tri_angle 4 \
  --Mapper.init_max_forward_motion 0.3 \
  --Mapper.abs_pose_max_error 10 \
  --Mapper.abs_pose_min_num_inliers 40 \
  --Mapper.abs_pose_min_inlier_ratio 0.25 \
  --Mapper.filter_max_reproj_error 6 \
  --Mapper.filter_min_tri_angle 1 \
  --Mapper.ba_refine_focal_length 0 \
  --Mapper.ba_refine_principal_point 0 \
  --Mapper.ba_refine_extra_params 0 \
  --Mapper.ba_use_gpu 1 \
  --Mapper.ba_local_num_images 12 \
  --Mapper.ba_local_min_tri_angle 2 \
  --Mapper.ba_global_frames_freq 150 \
  --Mapper.ba_global_max_num_iterations 25 \
  --Mapper.max_reg_trials 5
跑下来比较宽松 不会导致场景分裂的一套参数

4、补注册

跑
colmap feature_extractor \
  --database_path database.db \
  --image_path images \
  --ImageReader.single_camera 1 \
  --ImageReader.camera_model PINHOLE \
  --ImageReader.camera_params 1439.289304,1451.979537,955.342315,607.614894

colmap sequential_matcher \
    --database_path database.db \
    --SequentialMatching.overlap 20 

把新图片移至images里面 新信息会增加到之前的database.db

mkdir -p /sparse/1 稀疏重建模型存储在sparse/（数字）的文件夹里，要给新模型建一个新文件夹（后面要删掉原来的sparse/0 把重建成功的sparse/1改成0，因为LOD-3DGS的loader里写的是0）
**唯一不同的一步如下**
colmap mapper \
    --database_path database.db \
    --image_path images \
    --input_path sparse/0 \
    --output_path sparse/1 \
    --Mapper.multiple_models 0 \
    --Mapper.min_num_matches 15 \
    --Mapper.init_min_num_inliers 40 \
    --Mapper.init_max_error 5 \
    --Mapper.init_min_tri_angle 4 \
    --Mapper.init_max_forward_motion 0.3 \
    --Mapper.abs_pose_max_error 10 \
    --Mapper.abs_pose_min_num_inliers 40 \
    --Mapper.abs_pose_min_inlier_ratio 0.25 \
    --Mapper.filter_max_reproj_error 6 \
    --Mapper.filter_min_tri_angle 1 \
    --Mapper.ba_refine_focal_length 0 \
    --Mapper.ba_refine_principal_point 0 \
    --Mapper.ba_refine_extra_params 0 \
    --Mapper.ba_use_gpu 1 \
    --Mapper.ba_local_num_images 12 \
    --Mapper.ba_local_min_tri_angle 2 \
    --Mapper.ba_global_frames_freq 150 \
    --Mapper.ba_global_max_num_iterations 25 \
    --Mapper.max_reg_trials 5

## 深度图



python3 run.py  --encoder vitl   --img-path /home/metaiot_guest/data/lxl_data/experiment3/images  
--outdir /home/metaiot_guest/data/lxl_data/experiment/depths  
--pred-only  \ (只出现深度图)
--grayscale  （要黑白的深度图）

## 训练


python train.py -s /home/metaiot_guest/data/lxl_data/VisualRecons/test2\
    --use_lod \
    --sh_degree 2 \ （或者3也可以）
    --depths depths \
    --eval 

# LiDAR-RT


安装文档等都在 docs下面的文档里
目前的代码改动是 新写了一个 lib/dataloader下面的kradar_loader

写的格式转换小脚本在practical_py里我一起传上去了

遇到的问题：

1、kradar激光雷达点云文件无强度 （用0代替了，效果还可以）
2、示例用的KITTI-360的 ego（车身）和lidar坐标原点不同 有转换矩阵，kradar没讲究这个
解决方案：把ego和lidar系看成同一个 转换矩阵就是np.eyes(4,4) 或者 认为lidar在ego正上方1m左右，跑出来效果都还可以 不是影响loss量级主要因素
3、用radelft复现lidarrt会遇到的问题
radelft用的雷达是robosence 是128线
kitti360和kradar都是velodyne64线 radelft的loader要重新改（我还没改