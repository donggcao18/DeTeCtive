<div align="center">
<p align="center">
  <img src="./fig/overall.png"/>
</p>
</div>


<div align="center">
<h1>[NeurIPS 2024]  DeTeCtive: Detecting AI-generated Text via Multi-Level Contrastive Learning</h1>
</div>

## 🚀 Introduction

Recent advances in large language models (LLMs) have enabled the generation of text that closely resembles human writing, raising challenges for AI-generated text detection. **DeTeCtive** addresses these challenges with a multi-level contrastive learning framework, distinguishing nuanced writing styles to improve detection accuracy across various domains and models. This repository contains the code and models from our paper, [DeTeCtive: Detecting AI-generated Text via Multi-Level Contrastive Learning](https://arxiv.org/pdf/2410.20964).

## 📝 Dataset

Before starting, ensure that the required datasets are prepared correctly. This repository uses four primary datasets:

- **Deepfake**
- **SemEval2024-M4**
- **TuringBench**
- **OUTFOX**

You can download the pre-processed versions of these datasets from [Google Drive](https://drive.google.com/drive/folders/1FNfSmKFE40FHGBfGjypg_JS2aWO_G6gX). Once downloaded, unzip them into a single `datasets` directory, structured as follows:

```
$DATA/
|–– Deepfake/
|   |–– cross_domains_cross_models/
|   |–– cross_domains_model_specific/
|   |–– domain_specific_cross_models/
|   |–– domain_specific_model_specific/
|   |–– unseen_domains/
|   |–– unseen_models/
|–– SemEval2024-M4/
|   |–– SubtaskA/
|–– TuringBench/
|   |–– AA/
|–– OUTFOX/
    |–– chatgpt/
    |–– common/
    |–– dipper/
    |–– flan_t5_xxl/
    |–– text_davinci_003/
```

## 🔄 Installation

To set up the environment, you can use `requirements.txt` to install all necessary dependencies. Alternatively, if you already have a PyTorch environment, you can install the core libraries separately:

#### Using Conda:
```bash
conda create -n detective python=3.10
conda activate detective
pip install -r requirements.txt
```

#### Using Virtual Environment (venv):
```bash
python -m venv env
source env/bin/activate
pip install -r requirements.txt
```

#### Core Libraries
If PyTorch is already installed, you can simply install the two core libraries:
```bash
pip install lightning faiss-gpu
```

## 🔬 Testing

We have released checkpoint models on Hugging Face at [heyongxin233/DeTeCtive](https://huggingface.co/heyongxin233/DeTeCtive). Each dataset has an associated checkpoint that enables you to replicate the performance metrics reported in the paper. Download the checkpoint for the dataset you want to evaluate.

#### Available Checkpoints:
- `Deepfake_best.pth`
- `M4_monolingual_best.pth`
- `M4_multilingual_best.pth`
- `OUTFOX_best.pth`
- `TuringBench_best.pth`

After downloading, place the checkpoints in a directory accessible to the testing scripts.

Once the checkpoints are downloaded, use `script/test.sh` to run the evaluation. Modify the `DATA_PATH` and `Model_PATH` in the script to point to the location of your dataset and the downloaded checkpoint model. Choose the dataset you wish to evaluate, then run the script.

#### Running the Test Script
To initiate testing, execute:
```bash
bash script/test.sh
```

The `test.sh` script performs KNN classification by first embedding the training set and then running the test set. If the `--save_database` option is enabled, it will save the embedded database at the specified `save_path`. For future evaluations, you can skip the training embedding phase by loading the pre-saved database using `script/test_from_database.sh`.

#### Running from a Pre-saved Database
To evaluate directly from a saved database, use:
```bash
bash script/test_from_database.sh
```

This script operates similarly to `test.sh` but loads the training embeddings from the pre-saved database, reducing the computational load by skipping the embedding step for the training data.

## :computer: Training

To train the model, simply run the script `script/train.sh`. Before running, make sure to set the `DATA_PATH` parameter in the script to point to the location of your dataset. Then, select the dataset you want to use for training.

```bash
bash script/train.sh
```

#### Configurable Parameters

Some training parameters may require adjustment based on your setup. You can view and modify these parameters by referring to the help in `train_classifier.py`. Run the following command for more information on available parameters:

```bash
python train_classifier.py --help
```

This will display descriptions of the various configurable options that you can set to optimize training for your specific dataset and hardware configuration.

## 📚 Citation

If you use our code or findings in your research, please cite us as:
```
@misc{guo2024detectivedetectingaigeneratedtext,
      title={DeTeCtive: Detecting AI-generated Text via Multi-Level Contrastive Learning}, 
      author={Xun Guo and Shan Zhang and Yongxin He and Ting Zhang and Wanquan Feng and Haibin Huang and Chongyang Ma},
      year={2024},
      eprint={2410.20964},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2410.20964}, 
}
```
