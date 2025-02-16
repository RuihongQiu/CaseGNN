# CaseGNN
Code for ECIR 2024 paper.

Title: [CaseGNN: Graph Neural Networks for Legal Case Retrieval with Text-Attributed Graphs](https://arxiv.org/abs/2312.11229)

Author: Yanran Tang, Ruihong Qiu, Yilun Liu, Xue Li and Zi Huang

# Installation
Requirements can be seen in `/requirements.txt`

# Dataset
Datasets can be downloaded from [COLIEE2022](https://sites.ualberta.ca/~rabelo/COLIEE2022/) and [COLIEE2023](https://sites.ualberta.ca/~rabelo/COLIEE2023/). 

Specifically, the downloaded COLIEE2023 folders `task1_train_files_2023` and `task1_test_files_2023` should be put into `/PromptCase/task1_train_2023/` and `/PromptCase/task1_test_2023/` respectively. 

The label file `task1_train_labels_2023.json` and `task1_test_labels_2023.json` shoule be put into folder `/label/`. 

COLIEE2022 folders should be set in a similar way. 

The final project file are as follows:

    ```
    $ ./CaseGNN/
    .
    ├── DATASET
    │   └── data_load.py
    ├── Grpah_generation
    │   ├── graph
    │   │   ├── graph_bin_2022
    │   │   └── graph_bin_2023
    │   └── TACG.py
    ├── Information_extraction  
    │   ├── coliee2022_ie    
    │   ├── coliee2023_ie
    │   ├── lexnlp             
    │   ├── stanford-openie
    │   ├── create_structured_csv.py
    │   ├── knowledge_graph.py
    │   └── relation_extractor.py             
    ├── label 
    │   ├── hard_neg_top50_train_2022.json
    │   ├── hard_neg_top50_train_2023.json
    │   ├── task1_test_labels_2022.json            
    │   ├── task1_test_labels_2023.json 
    │   ├── task1_train_labels_2022.json 
    │   ├── task1_train_labels_2023.json 
    │   ├── test_2022_candidate_with_yearfilter.json
    │   └── test_2023_candidate_with_yearfilter.json     
    ├── PromptCase
    │   ├── preprocessing
    │   │   ├── openaiAPI.py
    │   │   ├── process.py
    │   │   └── reference.py
    │   ├── promptcase_embedding
    │   ├── PromptCase_embedding_generation.py
    │   ├── task1_test_2022
    │   │   └── task1_test_files_2022
    │   ├── task1_test_2023
    │   │   └── task1_test_files_2023
    │   ├── task1_train_2022
    │   │   └── task1_train_files_2022
    │   └── task1_train_2023
    │       └── task1_train_files_2023
    ├── CaseGNN_2022.sh
    ├── CaseGNN_2023.sh
    ├── LegalFeatureExtraction_2022.sh
    ├── LegalFeatureExtraction_2023.sh
    ├── PromptcaseEmbeddingGeneration_2022.sh
    ├── PromptcaseEmbeddingGeneration_2023.sh
    ├── RelationExtraction_2022.sh
    ├── RelationExtraction_2023.sh
    ├── TACG_2022.sh
    ├── TACG_2023.sh
    ├── main.py
    ├── model.py
    ├── torch_metrics.py
    ├── train.py
    ├── requirements.txt
    └── README.md          
    ```

# 1. Information Extraction
- 1. Legal Feature Extraction

    - [PromptCase Preprocessing](https://github.com/yanran-tang/PromptCase?tab=readme-ov-file#preprocessing) is used to extracted the fact and issue from the cases. 

    - Run `. ./LegalFeatureExtraction_2023.sh` to generate files in the following three folders:
        - `/PromptCase/task1_test_2023/processed/`, 
        - `/PromptCase/task1_test_2023/processed_new/`, which is the legal issues of cases, 
        - `/PromptCase/task1_test_2023/summary_test_2023_txt/`, which is the legal facts of cases. 
    
    - The same process for COLIEE2022.


- 2. Relation Extraction
    - Run `. ./RelationExtraction_2023.sh`.

    - The final relation triplets are in the folder `/Information_extraction/coliee2023_ie/coliee2023train(or test)_sum(or fact)/result/`.

    - The same process for COLIEE2022.

    - The relation extraction is based on the [knowledge_graph_from_unstructured_text](https://github.com/varun196/knowledge_graph_from_unstructured_text) and [lexnlp](https://github.com/LexPredict/lexpredict-lexnlp/tree/master/lexnlp).

- Note: Legal feature extraction should be done first since the relation extraction is based on the extracted legal features.

- The extracted information can be also downloaded [here](https://drive.google.com/drive/folders/1Ck1KecF28xqsjDZK1fqVGF3BozmSsAb7?usp=sharing).


# 2. PromptCase Embedding Generation
- [PromptCase](https://github.com/yanran-tang/PromptCase/blob/main/PromptCase_model.py) is used to generate the case embedding (the feature of virtual global node)
    - Run `. ./PromptcaseEmbeddingGeneration_2023.sh`. 
    - The generated case embedding and the according index list of cases are saved in folder `/PromptCase/promptcase_embedding/`
    - The same process for COLIEE2022.
- The generated PromptCase embedding can be also downloaded [here](https://drive.google.com/drive/folders/1TYc3RM6vbldQNmM5aNawdYy-tFS6IWbu?usp=sharing).


# 3. TACG Constrction
- TACG constrction utilises the result of Information Extraction and PromptCase Embedding, please ensure the folders of  `coliee2023_ie/coliee2023train(or test)_sum(or fact)/result/` and `/PromptCase/promptcase_embedding/` have been generated or downloaded.
- Run `. ./CaseGNN_2023.sh`
- The TACG graphs are saved in folder `/Graph_generation/graph/`

- The same process for COLIEE2022.


# 4. CaseGNN Model Training
Run `. ./CaseGNN_2022.sh` and `. ./CaseGNN_2023.sh` for COLIEE2022 and COLIEE2023, respectively.


# Cite
If you find this repo useful, please cite
```
@article{CaseGNN,
  author    = {Yanran Tang, Ruihong Qiu, Yilun Liu, Xue Li and Zi Huang},
  title     = {CaseGNN: Graph Neural Networks for Legal Case Retrieval with Text-Attributed Graphs},
  journal   = {CoRR},
  volume    = {abs/2312.11229},
  year      = {2023},
}

@article{PromptCase,
  author    = {Yanran Tang and Ruihong Qiu and Xue Li},
  title     = {Prompt-based Effective Input Reformulation for Legal Case Retrieval},
  journal   = {CoRR},
  volume    = {abs/2309.02962},
  year      = {2023},
}
```