# StableAnimator

<a href='https://francis-rings.github.io/StableAnimator'><img src='https://img.shields.io/badge/Project-Page-Green'></a> <a href='https://arxiv.org/abs/2411.17697'><img src='https://img.shields.io/badge/Paper-Arxiv-red'></a> <a href='https://huggingface.co/FrancisRing/StableAnimator/tree/main'><img src='https://img.shields.io/badge/HuggingFace-Model-orange'></a> <a href='https://www.youtube.com/watch?v=7fwFyFDzQgg'><img src='https://img.shields.io/badge/YouTube-Watch-red?style=flat-square&logo=youtube'></a> <a href='https://www.bilibili.com/video/BV1X5zyYUEuD'><img src='https://img.shields.io/badge/Bilibili-Watch-blue?style=flat-square&logo=bilibili'></a>

StableAnimator: High-Quality Identity-Preserving Human Image Animation
<br/>
*Shuyuan Tu<sup>1</sup>, Zhen Xing<sup>1</sup>, Xintong Han<sup>3</sup>, Zhi-Qi Cheng<sup>4</sup>, Qi Dai<sup>2</sup>, Chong Luo<sup>2</sup>, Zuxuan Wu<sup>1</sup>*
<br/>
[<sup>1</sup>Fudan University; <sup>2</sup>Microsoft Research Asia; <sup>3</sup>Huya Inc; <sup>4</sup>Carnegie Mellon University]

<p align="center">
  <img src="assets/figures/case-47.gif" width="256" />
  <img src="assets/figures/case-61.gif" width="256" />
  <img src="assets/figures/case-45.gif" width="256" />
  <img src="assets/figures/case-46.gif" width="256" />
  <img src="assets/figures/case-5.gif" width="256" />
  <img src="assets/figures/case-17.gif" width="256" />
  <br/>
  <span>Pose-driven Human image animations generated by StableAnimator, showing its power to synthesize <b>high-fidelity</b> and <b>ID-preserving videos</b>. All animations are <b>directly synthesized by StableAnimator without the use of any face-related post-processing tools</b>, such as the face-swapping tool FaceFusion or face restoration models like GFP-GAN and CodeFormer.</span>
</p>

<p align="center">
  <img src="assets/figures/case-35.gif" width="384" />
  <img src="assets/figures/case-42.gif" width="384" />
  <img src="assets/figures/case-18.gif" width="384" />
  <img src="assets/figures/case-24.gif" width="384" />
  <br/>
  <span>Comparison results between StableAnimator and state-of-the-art (SOTA) human image animation models highlight the superior performance of StableAnimator in delivering <b>high-fidelity, identity-preserving human image animation</b>.</span>
</p>


## Overview

<p align="center">
  <img src="assets/figures/framework.jpg" alt="model architecture" width="1280"/>
  </br>
  <i>The overview of the framework of StableAnimator.</i>
</p>

Current diffusion models for human image animation struggle to ensure identity (ID) consistency. This paper presents StableAnimator, <b>the first end-to-end ID-preserving video diffusion framework, which synthesizes high-quality videos without any post-processing, conditioned on a reference image and a sequence of poses.</b> Building upon a video diffusion model, StableAnimator contains carefully designed modules for both training and inference striving for identity consistency. In particular, StableAnimator begins by computing image and face embeddings with off-the-shelf extractors, respectively and face embeddings are further refined by interacting with image embeddings using a global content-aware Face Encoder. Then, StableAnimator introduces a novel distribution-aware ID Adapter that prevents interference caused by temporal layers while preserving ID via alignment. During inference, we propose a novel Hamilton-Jacobi-Bellman (HJB) equation-based optimization to further enhance the face quality. We demonstrate that solving the HJB equation can be integrated into the diffusion denoising process, and the resulting solution constrains the denoising path and thus benefits ID preservation. Experiments on multiple benchmarks show the effectiveness of StableAnimator both qualitatively and quantitatively.

## News
* `[2024-12-13]`:🔥 The training code and training tutorial are released! You can train/finetune your own StableAnimator on your own collected datasets! Other codes will be released very soon. Stay tuned!
* `[2024-12-10]`:🔥 The gradio interface is released! Many thanks to [@gluttony-10](https://space.bilibili.com/893892) for his contribution! Other codes will be released very soon. Stay tuned!
* `[2024-12-6]`:🔥 All data preprocessing codes (human skeleton extraction and human face mask extraction) are released! The training code and detailed training tutorial will be released before 2024.12.13. Stay tuned!
* `[2024-12-4]`:🔥 We are thrilled to release an interesting dance demo (🔥🔥APT Dance🔥🔥)! The generated video can be seen on [YouTube](https://www.youtube.com/watch?v=KNPoAsWr_sk) and [Bilibili](https://www.bilibili.com/video/BV1KczXYhER7).
* `[2024-11-28]`:🔥 The data pre-processing codes (human skeleton extraction) are available! Other codes will be released very soon. Stay tuned!
* `[2024-11-26]`:🔥 The project page, code, technical report and [a basic model checkpoint](https://huggingface.co/FrancisRing/StableAnimator/tree/main) are released. Further training codes, data pre-processing codes, the evaluation dataset and StableAnimator-pro will be released very soon. Stay tuned!

## To-Do List
- [x] StableAnimator-basic 
- [x] Inference Code
- [x] Evaluation Samples
- [x] Data Pre-Processing Code (Skeleton Extraction)
- [x] Data Pre-Processing Code (Human Face Mask Extraction)
- [x] Training Code
- [ ] Evaluation Dataset
- [ ] StableAnimator-pro
- [ ] Inference Code with HJB-based Face Optimization

## Quickstart

For the basic version of the model checkpoint, it supports generating videos at a 576x1024 or 512x512 resolution. If you encounter insufficient memory issues, you can appropriately reduce the number of animated frames.

### Environment setup

```
pip install torch==2.5.1 torchvision==0.20.1 torchaudio==2.5.1 --index-url https://download.pytorch.org/whl/cu124
pip install torch==2.5.1+cu124 xformers --index-url https://download.pytorch.org/whl/cu124
pip install -r requirements.txt
```

### Download weights
If you encounter connection issues with Hugging Face, you can utilize the mirror endpoint by setting the environment variable: `export HF_ENDPOINT=https://hf-mirror.com`.
Please download weights manually as follows:
```
cd StableAnimator
git lfs install
git clone https://huggingface.co/FrancisRing/StableAnimator checkpoints
```
All the weights should be organized in models as follows
The overall file structure of this project should be organized as follows:
```
StableAnimator/
├── DWPose
├── animation
├── checkpoints
│   ├── DWPose
│   │   ├── dw-ll_ucoco_384.onnx
│   │   └── yolox_l.onnx
│   ├── Animation
│   │   ├── pose_net.pth
│   │   ├── face_encoder.pth
│   │   └── unet.pth
│   ├── SVD
│   │   ├── feature_extractor
│   │   ├── image_encoder
│   │   ├── scheduler
│   │   ├── unet
│   │   ├── vae
│   │   ├── model_index.json
│   │   ├── svd_xt.safetensors
│   │   └── svd_xt_image_decoder.safetensors
│   └── inference.zip
├── models
│   │   └── antelopev2
│   │       ├── 1k3d68.onnx
│   │       ├── 2d106det.onnx
│   │       ├── genderage.onnx
│   │       ├── glintr100.onnx
│   │       └── scrfd_10g_bnkps.onnx
├── app.py
├── command_basic_infer.sh
├── inference_basic.py
├── requirement.txt 
```
<b>Notably, there is a bug in the automatic download process of Antelopev2, with the error details described as follows:</b>
```
Traceback (most recent call last):
  File "/home/StableAnimator/inference_normal.py", line 243, in <module>
    face_model = FaceModel()
  File "/home/StableAnimator/animation/modules/face_model.py", line 11, in __init__
    self.app = FaceAnalysis(
  File "/opt/conda/lib/python3.10/site-packages/insightface/app/face_analysis.py", line 43, in __init__
    assert 'detection' in self.models
AssertionError
```
This issue is related to the incorrect path of Antelopev2, which is automatically downloaded into the `models/antelopev2/antelopev2` directory. The correct path of Antelopev2 should be `models/antelopev2`. You can run the following commands to tackle this issue:
```
cd StableAnimator
mv ./models/antelopev2/antelopev2 ./models/tmp
rm -rf ./models/antelopev2
mv ./models/tmp ./models/antelopev2
```

### Evaluation Samples
The evaluation samples presented in the paper can be downloaded from [OneDrive](https://1drv.ms/f/c/becb962aad1a1f95/EubdzCAI7BFLhJff2LrHkt8BC9mOiwJ5V67t-ypxRnCK4Q?e=ElEmcn) or `inference.zip` in checkpoints. Please download evaluation samples manually as follows:
```
cd StableAnimator
mkdir inference
```
All the evaluation samples should be organized as follows:
```
inference/
├── case-1
│   ├── poses
│   ├── faces
│   └── reference.png
├── case-2
│   ├── poses
│   ├── faces
│   └── reference.png
├── case-3
│   ├── poses
│   ├── faces
│   └── reference.png
```

### Human Skeleton Extraction
We leverage the pre-trained DWPose to extract the human skeletons. In the initialization of DWPose, the pretrained weights should be configured in `/DWPose/dwpose_utils/wholebody.py`:
```
onnx_det = 'path/checkpoints/DWPose/yolox_l.onnx'
onnx_pose = 'path/checkpoints/DWPose/dw-ll_ucoco_384.onnx'
```
Given the target image folder containing multiple .png files, you can use the following command to obtain the corresponding human skeleton images:
```
python DWPose/skeleton_extraction.py --target_image_folder_path="path/test/target_images" --ref_image_path="path/test/reference.png" --poses_folder_path="path/test/poses"
```
It is worth noting that the .png files in the target image folder are named in the format `frame_i.png`, such as `frame_0.png`, `frame_1.png`, and so on. 
`--ref_image_path` refers to the path of the given reference image. The obtained human skeleton images are saved in `path/test/poses`. It is particularly significant that the target skeleton images should be aligned with the reference image regarding the body shape.

If you only have the target MP4 file (target.mp4), we recommend you to use `ffmpeg` to convert the MP4 file to multiple frames (.png files) without any quality loss.
```
ffmpeg -i target.mp4 -q:v 1 -start_number 0 path/test/target_images/frame_%d.png
```
The obtained frames are saved in `path/test/target_images`.

### Human Face Mask Extraction
Given the path to an image folder containing multiple RGB `.png` files, you can run the following command to extract the corresponding human face masks:
```
python face_mask_extraction.py --image_folder="path/StableAnimator/inference/your_case/target_images"
```
`path/StableAnimator/inference/your_case/target_images` contains multiple `.png` files. The obtained masks are saved in `path/StableAnimator/inference/your_case/faces`.

### Model inference
A sample configuration for testing is provided as `command_basic_infer.sh`. You can also easily modify the various configurations according to your needs.

```
bash command_basic_infer.sh
```
StableAnimator supports human image animation at two different resolution settings: 512x512 and 576x1024. You can modify "--width" and "--height" in `command_basic_infer.sh` to set the resolution of the animation. "--output_dir" in `command_basic_infer.sh` refers to the saved path of the generated animation. "--validation_control_folder" and "--validation_image" in `command_basic_infer.sh` refer to the paths of the given pose sequence and the reference image, respectively.
"--pretrained_model_name_or_path" in `command_basic_infer.sh` is the path of pretrained SVD. "posenet_model_name_or_path", "face_encoder_model_name_or_path", and "unet_model_name_or_path" in `command_basic_infer.sh` refer to paths of pretrained StableAnimator weights.
If you have enough GPU resources, you can increase the value (4=>8=>16) of "--decode_chunk_size" in `command_basic_infer.sh` to promote the temporal smoothness of the animation.

Tips: if your GPU memory is limited, you can reduce the number of animated frames. This command will generate two files: <b>animated_images</b> and <b>animated_images.gif</b>.
If you want to obtain the high quality MP4 file, we recommend you to leverage ffmpeg on the <b>animated_images</b> as follows:
```
cd animated_images
ffmpeg -framerate 20 -i frame_%d.png -c:v libx264 -crf 10 -pix_fmt yuv420p /path/animation.mp4
```
"-framerate" refers to the fps setting. "-crf" indicates the quality of the generated MP4 file, with smaller values corresponding to higher quality.
Additionally, you can also run the following command to launch a Gradio interface:
```
python app.py
```

### Model Training
For the training dataset, it has to be organized as follows:

```
animation_data/
├── rec
│   │  ├──00001
│   │  │  ├──images
│   │  │  │  ├──frame_0.png
│   │  │  │  ├──frame_1.png
│   │  │  │  ├──frame_2.png
│   │  │  │  └──...
│   │  │  ├──faces
│   │  │  │  ├──frame_0.png
│   │  │  │  ├──frame_1.png
│   │  │  │  ├──frame_2.png
│   │  │  │  └──...
│   │  │  └──poses
│   │  │  │  ├──frame_0.png
│   │  │  │  ├──frame_1.png
│   │  │  │  ├──frame_2.png
│   │  │  │  └──...
│   │  ├──00002
│   │  │  ├──images
│   │  │  ├──faces
│   │  │  └──poses
│   │  ├──00003
│   │  │  ├──images
│   │  │  ├──faces
│   │  │  └──poses
│   │  └──...
├── vec
│   │  ├──00001
│   │  │  ├──images
│   │  │  ├──faces
│   │  │  └──poses
│   │  ├──00002
│   │  │  ├──images
│   │  │  ├──faces
│   │  │  └──poses
│   │  ├──00003
│   │  │  ├──images
│   │  │  ├──faces
│   │  │  └──poses
│   │  └──...
├── video_rec_path.txt
└── video_vec_path.txt
```
StableAnimator is trained on mixed-resolution videos, with 512x512 videos stored in `animation_data/rec` and 576x1024 videos stored in `animation_data/vec`. Each folder in `animation_data/rec` or `animation_data/vec` contains three subfolders which contains multiple `.png` image files. 
All `.png` image files are named in the format `frame_i.png`, such as `frame_0.png`, `frame_1.png`, and so on.
`00001`, `00002`, `00003` indicate individual video information.
In terms of three subfolders, `images`, `faces`, and `poses` store RGB frames, corresponding human face masks, and corresponding human skeleton poses, respectively.
`video_rec_path.txt` and `video_vec_path.txt` record folder paths of `animation_data/rec` and `animation_data/vec`, respectively.
For example, the content of `video_rec_path.txt` is shown as follows:
```
path/StableAnimator/animation_data/rec/00001
path/StableAnimator/animation_data/rec/00002
path/StableAnimator/animation_data/rec/00003
path/StableAnimator/animation_data/rec/00004
path/StableAnimator/animation_data/rec/00005
path/StableAnimator/animation_data/rec/00006
...
```
If you only have raw videos, you can leverage `ffmpeg` to extract frames from raw videos and store them in the subfolder `images`.
```
ffmpeg -i raw_video_1.mp4 -q:v 1 -start_number 0 path/StableAnimator/animation_data/rec/00001/images/frame_%d.png
```
The obtained frames are saved in `path/StableAnimator/animation_data/rec/00001/images`.

For extracting the human skeleton poses, you can run the following command:
```
python DWPose/training_skeleton_extraction.py --root_path="path/StableAnimator/animation_data" --name="rec" --start=1 --end=500 
```
`--root_path` and `--name` refer to the root path of training datasets and the name of the dataset.
`--start` and `--end`  specify the starting and ending indices of the selected training dataset. For example, `--name="rec" --start=1 --end=500` indicates that the skeleton extraction will start at `path/StableAnimator/animation_data/rec/00001` and end at `path/StableAnimator/animation_data/rec/00500`.

For extraction details of corresponding face masks, please refer to the Human Face Mask Extraction section.
When your dataset is organized exactly as outlined above, you can easily train your StableAnimator by running the following command:
```
bash command_train.sh
```
For the parameter details of `command_train.sh`, `CUDA_VISIBLE_DEVICES` refers to gpu devices. In my setting, I use 4 NVIDIA A100 80G to train StableAnimator (`CUDA_VISIBLE_DEVICES=3,2,1,0`).
`--pretrained_model_name_or_path` and `--output_dir` refer to the pretrained SVD path and the checkpoint saved path of the trained StableAnimator.
`--data_root_path`, `--rec_data_path`, and `--vec_data_path` are the root path of datasets, the path of `video_rec_path.txt`, and the path of `video_vec_path.txt`, respectively.
`validation_image_folder`, `validation_control_folder`, and `validation_image` are paths of validation ground truths, validation driven skeleton poses, and the validation reference image.
`--sample_n_frames` is the number of frames that StableAnimator processes in a single batch. 
`--num_train_epochs` is the training epoch number. It is worth noting that the default number of training epochs is set to infinite. You can manually terminate the training process once you observe that your StableAnimator has reached its peak performance.
The overall file structure of StableAnimator at training is shown as follows:
```
StableAnimator/
├── DWPose
├── animation
├── animation_data
│   ├── rec
│   ├── vec
│   ├── video_rec_path.txt
│   └── video_vec_path.txt
├── validation
│   ├── ground_truth
│   │   ├── frame_0.png
│   │   ├── frame_1.png
│   │   ├── frame_2.png
│   │   └── ...
│   ├── poses
│   │   ├── frame_0.png
│   │   ├── frame_1.png
│   │   ├── frame_2.png
│   │   └── ...
│   └── reference.png
├── checkpoints
│   ├── DWPose
│   │   ├── dw-ll_ucoco_384.onnx
│   │   └── yolox_l.onnx
│   ├── Animation
│   │   ├── pose_net.pth
│   │   ├── face_encoder.pth
│   │   └── unet.pth
│   ├── SVD
│   │   ├── feature_extractor
│   │   ├── image_encoder
│   │   ├── scheduler
│   │   ├── unet
│   │   ├── vae
│   │   ├── model_index.json
│   │   ├── svd_xt.safetensors
│   │   └── svd_xt_image_decoder.safetensors
│   └── inference.zip
├── models
│   │   └── antelopev2
│   │       ├── 1k3d68.onnx
│   │       ├── 2d106det.onnx
│   │       ├── genderage.onnx
│   │       ├── glintr100.onnx
│   │       └── scrfd_10g_bnkps.onnx
├── app.py
├── command_basic_infer.sh
├── inference_basic.py
├── train.py
├── command_train.sh
└── requirement.txt 
```
<b>It is worth noting that training StableAnimator requires approximately 70GB of VRAM due to the mixed-resolution (512x512 and 576x1024) training pipeline. 
However, if you train StableAnimator exclusively on 512x512 videos, the VRAM requirement is reduced to approximately 40GB.</b>
Additionally, The backgrounds of the selected training videos should remain static, as this helps the diffusion model calculate accurate reconstruction loss.

### VRAM requirement and Runtime

For the 15s demo video (512x512, fps=30), the 16-frame basic model requires 8GB VRAM and finishes in 5 minutes on a 4090 GPU.

The minimum VRAM requirement for the 16-frame U-Net of the pro model is 10GB (576x1024, fps=30); however, the VAE decoder demands 16GB. You have the option to run the VAE decoder on CPU.

## Contact
If you have any suggestions or find our work helpful, feel free to contact me

Email: francisshuyuan@gmail.com

If you find our work useful, <b>please consider giving a star to this github repository and citing it</b>:
```bib
@article{tu2024stableanimator,
  title={StableAnimator: High-Quality Identity-Preserving Human Image Animation},
  author={Shuyuan Tu and Zhen Xing and Xintong Han and Zhi-Qi Cheng and Qi Dai and Chong Luo and Zuxuan Wu},
  journal={arXiv preprint arXiv:2411.17697},
  year={2024}
}
```
