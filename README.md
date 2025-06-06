<!-- GETTING STARTED -->

# mc-tit
Code for ACL 2023 paper: Exploring Better Text Image Translation with Multimodal Codebook.

Code for Neural Network 2025 paper: Towards Better Text Image Machine Translation with Multimodal Codebook and Multi-stage Training. [link](https://github.com/DeepLearnXMU/mc_tit/tree/main/timt-mcmt)

## OCRMT30K Dataset
Dataset can be found [here](https://github.com/DeepLearnXMU/mc_tit/tree/main/ocrmt30k_data)

## Install fairseq
```
cd mc_tit
pip install -e ./
```

## Training
### Text data process
Run `fiarseq-preprocess-pretrain.sh` to process text data.
see [fairseq](https://github.com/facebookresearch/fairseq) for more details
### Stage 1
Run `fairseq-pretrain.sh` to pretrain model on the wmt22 dataset.
### Stage 2
Run `get_codebook_datasest.sh` to get the multimodal codebook training data.
Then, run `fairseq-codebook-pretrain.sh` to pretrain model on monolingual data. 
### Stage 3
#### Data construction
The method of obtaining text data is the same as in stage 1.
Run `fairseq-preprocess-icdar-weak.sh` to handle text data. 
You can get image feature data in the following way:
1. Download the image data and organize the directory into the following form:

    ```
    icdar19_lsvt_weak
    ├─ iwslt-weak-images
    ├─ image_train.txt
    ├─ image_valid.txt
    ├─ train.txt
    └─ valid.txt
    ```
2. Run `img_feat_extract/get_img_feat.py` to extract image feature.
    ```
    python img_feat_extract/get_img_feat.py \
        --dataset train \
        --model vit_base_patch16_224 \
        --path ../icdar19_lsvt_weak
    ```
Note that the program needs to be modified here to match your directory structure.

Then, run `fairseq-ita-pretrain.sh` to pretrain model by ita task.

### Stage 4

This stage still needs to process image data like the stage 3, organize the data into the following form
```
ocrmt30k
├── ocrmt30k-images
├── image_test.txt
├── image_train.txt
├── image_valid.txt
├── test.tok.lower.en
├── test.tok.lower.zh
├── test.tok.lower.zh-c
├── train.tok.lower.en
├── train.tok.lower.zh
├── train.tok.lower.zh-c
├── valid.tok.lower.en
├── valid.tok.lower.zh
└── valid.tok.lower.zh-c
```
Run `fairseq-preprocess-finetune.sh` to handle text data.
2. Run `img_feat_extract/get_img_feat.py` to extract image feature.

```
python img_feat_extract/get_img_feat.py \
    --dataset train \
    --model vit_base_patch16_224  \
    --path ../ocrmt30k
```
Finally, run `fairseq-tit-stage4-finetune` to finetune the entire model.

## Inference
Run `fairseq-tit-generate` to test final model.

## Citation
```
@article{lan2023mctit,
  title={Exploring Better Text Image Translation with Multimodal Codebook},
  author={Zhibin Lan and Jiawei Yu and Xiang Li and Wen Zhang and Jian Luan and Bin Wang and Degen Huang and Jinsong Su},
  publisher={ACL},
  year={2023}
}

@article{lan2025timt-mcmt,
  title={Towards Better Text Image Machine Translation with Multimodal Codebook and Multi-stage Training},
  author={Zhibin Lan and Jiawei Yu and Shiyu Liu and Junfeng Yao and Degen Huang and Jinsong Su},
  publisher={Neural Network},
  year={2025}
}
```

<!-- ACKNOWLEDGMENTS -->
<!-- ## Acknowledgments

* []()
* []()
* []() -->

<p align="right">(<a href="#top">back to top</a>)</p>