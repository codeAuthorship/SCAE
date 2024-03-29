# SCAE: Can Seq2Seq Code Transformation Evade Code Authorship Attribution?

## Overview
Code authorship attribution [[1]](https://dl.acm.org/doi/abs/10.1145/3243734.3243738) is the problem of identifying authors of programming language codes through the stylistic features in their codes, a topic that recently witnessed significant interest with outstanding performance. To defeat attribution, the state-of-the-art approach uses the Monte-Carlo Tree Search (MCTS) [[2]](https://www.usenix.org/conference/usenixsecurity19/presentation/quiring) for code transformation to obfuscate codes. Although effective in misleading code authorship attribution, MCTS is disadvantaged by exhaustive resources for identifying optimal code transformations. Can we efficiently evade code authorship attribution without MCTS?

We present SCAE, a code authorship obfuscation technique that leverages a Seq2Seq code transformer called StructCoder. Unlike MCTS, SCAE saves processing time while maintaining the performance of the transformation. SCAE customizes StructCoder [[3]](https://arxiv.org/abs/2206.05239), a system originally designed for function-level code translation from one language to another (e.g., Java to C#), using transfer learning. To alleviate the need for manually transformed training data, we leverage the outputs from the MCTS method to construct a source-target code pair dataset and use it to train/fine-tune StructCoder for C++ to C++ code transformation that maintains the stylistic patterns of the target code. Our evaluation shows that SCAE improved the efficiency at a slight degradation of accuracy compared to MCTS. We were able to reduce the processing time by approximately 68% while maintaining an 85% transformation success rate and up to 95.77% evasion success rate in the untargeted setting. We further show the limitations of Seq2Seq models in the targeted domain.


<p align="center">
    <img src="https://github.com/codeAuthorship/SCAE/blob/main/S2CAE.png" alt="SCAE" width="70%" height="70%">


## Setup the conda enviorment:
    conda env scoder create -f scoder.yml
    conda activate scoder    

## Data prepraration:
1. Untargeted Trasnformation.
- Move training, validation, and testing files from [Untargeted_transformation](https://github.com/codeAuthorship/SCAE/tree/main/data/Untatgeted_Transformation) to [data](https://github.com/codeAuthorship/SCAE/tree/main/src/data) in src folder.
- In [data](https://github.com/codeAuthorship/SCAE/tree/main/src/data), there should be train.cpp.text_1&2.cpp, valid.cpp.text_1&2.cpp, and test.cpp.text_1&2.cpp.

2. Targeted Trasnformation.
- Move training, validation, and testing files from [Targeted_transformation](https://github.com/codeAuthorship/SCAE/tree/main/data/Targeted_Transformation) to [data](https://github.com/codeAuthorship/SCAE/tree/main/src/data) in src folder.
- In [data](https://github.com/codeAuthorship/SCAE/tree/main/src/data), there should be train.cpp.text_1&2.cpp, valid.cpp.text_1&2.cpp, and test.cpp.text_1&2.cpp.

The names of data for Untargeted and Targeted transformation are the same. Therefore, we need to modify it to avoid conflict.

## Pre-trained StructCoder:
1. Download The pre-trained checkpoint of Structcoder [[3]](https://arxiv.org/abs/2206.05239) from [GoogleDrive](https://drive.google.com/file/d/1V98OciKJKftjR1ifm7elB1f3DO1UU7sp/view?usp=sharing).
- You can also download it from the original work.
2. Place it under [saved_models/pretrain](https://github.com/codeAuthorship/SCAE/tree/main/src/saved_models/pretrain) folder.



## Finetune on translation command:
    python3 run_translation.py --do_train --do_eval --do_test --source_lang cpp --target_lang cpp --alpha1_clip -4 --alpha2_clip -4


## MAA's Targeted Attack Analysis is presented in [here](https://github.com/codeAuthorship/MAA-Targeted-Attack)
