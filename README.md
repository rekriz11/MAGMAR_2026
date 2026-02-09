# MAGMAR_2026


# MAGMAR Shared Task

Welcome to the MAGMaR (Multimodal Augmented Generation via MultimodAl Retrieval) shared task! This task focuses on developing systems that can effectively retrieve and generate information from multilingual video content.

For more information, visit the official website: https://nlp.jhu.edu/magmar/

## Overview

The MAGMaR shared task consists of two main components:

1. **Retrieval Task**: Retrieve relevant videos in response to information needs
2. **Generation Task**: Generate comprehensive responses synthesizing information from multiple sources

## Task Descriptions

For both tasks, the queries can be found in the `MAGMaR2026_queries.jsonl` file. Each query has the following structure:
```json
{
    "query_id": "1", 
    "query_type": "unbiased", 
    "language": "english", 
    "title": "2025_canadian_federal_election", 
    "persona_title": "Statistician for North American Elections", 
    "background": "I am a statistician who monitors and analyzes federal elections in Canada, Mexico, and the United States. Following major federal elections in these countries, I am generally responsible for compiling and verifying detailed statistics for each party involved in the election. These statistics are mostly used as additional data for building models to forecast future elections; my job does not involve making political or policy commentary on the basis of these results, and it's not my area of expertise anyway. Since the audience for my work is generally other statisticians and technical people within my department, the written work I produce tends to be fairly technical.", 
    "query": "Help me compile parliamentary and vote share statistics on the 2025 Canadian Federal elections. This should include how many seats each party won or lost, how many total seats each party now has, any information on total (popular) vote share available for the major parties and for any specific candidates for which it is available. Any demographic breakdowns of support for particular parties or candidates would also be helpful. For all statistics, please also include as much detail as possible on their source, as this helps me get a sense for how credible they are."
}
```
- `query_id`: Unique identifier for the query
- `query_type`: Type of query (e.g., unbiased, biased)
- `language`: Language of the videos for the query
- `title`: Title of topic
- `persona_title`: Title of the persona for whom the information is being retrieved/generated
- `background`: Background information about the persona
- `query`: The actual information need to be addressed


### Retrieval Task


#### Submission Format

Submissions should be provided as a JSON file with the following structure:
```json
{
  "query_id_1": [
    {"video_id": "abc123", "relevance": 0.95},
    {"video_id": "def456", "relevance": 0.87},
    {"video_id": "ghi789", "relevance": 0.72}
  ],
  "query_id_2": [
    {"video_id": "jkl012", "relevance": 0.91},
    {"video_id": "mno345", "relevance": 0.65}
  ]
}
```

- Each key represents a query ID
- The value is a ranked list of retrieved videos
- Each video entry contains:
  - `video_id`: Unique identifier for the video segment
  - `relevance`: Relevance score (higher scores indicate greater relevance)

Videos should be ranked in descending order by relevance score.

### Generation Task

The generation task requires systems to produce factual, well-supported responses to information needs by synthesizing content from retrieved sources.

#### Submission Format

Submissions should be provided as a JSONL (JSON Lines) file, with one JSON object per line. Each entry should follow this structure:
```json
{"metadata": {"run_id": "system_name-config", "query_id": "1", "team_id": "your_team", "task": "[oracle|rag]"}, "responses": [{"text": "sentence 1", "citations": ["video_id_1"]}, {"text": "sentence 2.", "citations": ["video_id_1
", "video_id_2"]}], "references": ["video_id_1", "video_id_2"]}
```

Each entry contains:

- **metadata**: Information about the submission
  - `run_id`: Identifier for your system run
  - `query_id`: The query/topic being addressed
  - `team_id`: Your team identifier
  - `task`: Task type, either oracle or retrieval-augmented generation (rag)

- **responses**: Array of generated statements
  - `text`: A factual claim or piece of information
  - `citations`: Array of video IDs supporting this claim

- **references**: Complete list of all source documents/videos used across all responses

**NOTE**: you don't need to decompose your sentences because we will do that for you. 

## Evaluation

Systems will be evaluated on:

- **Retrieval**: The ranked lists will be scored using standard information retrieval metrics such as nDCG@k, recall@k.
- **Generation**: The generated articles will be evaluated using [MiRAGE](xhttps://arxiv.org/abs/2510.24870) and a human evaluation. Because MiRAGE is automatic, the leader board will track this until the deadline. Human evaluations will be on the top 2 submissions of each team. 

## Getting Started

1. Download the dataset from the MAGMAR website
2. Develop your retrieval and/or generation system
3. Format your outputs according to the specifications above
4. Submit your results through the official submission portal

### Retrieval Setup
#### Download Instructions
Download instructions
The dataset can be found on huggingface at https://huggingface.co/datasets/hltcoe/MultiVENT2.0. However, you can't use the datasets library to access the videos, as all videos are tarred. Instead, you need to locally download the dataset before untar the videos (and corresponding audio track if you use those).

##### Step 1: Install git-lfs
As the files are large, before cloning you must ensure git lfs is installed
```bash
git lfs install
```

##### Step 2: Clone the dataset
After enabling git-lfs, you can now pull the dataset from huggingface.
```bash
git clone https://huggingface.co/datasets/hltcoe/MultiVENT2.0
```
Using tmux is recommended, as downloading all videos will take a while

##### Step 3: Combine MAGMaR 2026 videos with MultiVENT 2.0 test collection
Make sure you are retrieving from the full set of videos for this task. The full MultiVENT2.0 dataset contains the distractors for the MAGMaR2026 test set. 


### Generation Setup 
The generation task has two options: retrieval augmented generation and oracle generation. In RAG, you use a ranked list from a retrieval system to generate your responses. These ranked lists can come from your own retrieval system or from our provided baselines. In oracle generation, you are provided with the ground truth relevant videos for each topic. 

#### Download Instructions
##### RAG
Follow the above download instructions to download all the videos for the RAG task. 

##### Oracle
You can download only the oracle relevant videos by downloading the vidoes in this subset of the MultiVENT2.0 dataset: https://huggingface.co/datasets/hltcoe/MultiVENT2.0/tree/main/MAGMaR2026_test



### Other Data
We provide some additional data to help you get started:

1. Baseline retrieval runs: `baselines/retrieval`
2. Video captions: MultiVENT2.0: https://huggingface.co/datasets/hltcoe/MultiVENT2.0/blob/main/features/test/Qwen3-Omni-30B-A3B-Instruct_captions.jsonl, MAGMaR2026: https://huggingface.co/datasets/hltcoe/MultiVENT2.0/tree/main/MAGMaR2026_test
3. Extra training queries: https://huggingface.co/datasets/hltcoe/RankVideo-Dataset. If using this, please cite [RankVideo](https://arxiv.org/abs/2602.02444)


## Contact
If you have any issues or questions, join the slack chanel [invite]() or email the organizers. 


## Citations
If you use any of the datasets, baselines, or other resources provided for this shared task, please cite the corresponding papers as follows:
```
## MultiVENT2.0 Citation
@misc{kriz2025multivent20massivemultilingual,
      title={MultiVENT 2.0: A Massive Multilingual Benchmark for Event-Centric Video Retrieval}, 
      author={Reno Kriz and Kate Sanders and David Etter and Kenton Murray and Cameron Carpenter and Kelly Van Ochten and Hannah Recknor and Jimena Guallar-Blasco and Alexander Martin and Ronald Colaianni and Nolan King and Eugene Yang and Benjamin Van Durme},
      year={2025},
      eprint={2410.11619},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2410.11619}, 
}

## WikiVideo Citation
@misc{martin2025wikivideoarticlegenerationmultiple,
      title={WikiVideo: Article Generation from Multiple Videos}, 
      author={Alexander Martin and Reno Kriz and William Gantt Walden and Kate Sanders and Hannah Recknor and Eugene Yang and Francis Ferraro and Benjamin Van Durme},
      year={2025},
      eprint={2504.00939},
      archivePrefix={arXiv},
      primaryClass={cs.CV},
      url={https://arxiv.org/abs/2504.00939}, 
}

## MiRAGE Citation
@misc{martin2025seeingmirageevaluatingmultimodal,
      title={Seeing Through the MiRAGE: Evaluating Multimodal Retrieval Augmented Generation}, 
      author={Alexander Martin and William Walden and Reno Kriz and Dengjia Zhang and Kate Sanders and Eugene Yang and Chihsheng Jin and Benjamin Van Durme},
      year={2025},
      eprint={2510.24870},
      archivePrefix={arXiv},
      primaryClass={cs.CL},
      url={https://arxiv.org/abs/2510.24870}, 
}

## OmniEmbed Citation
@misc{ma2025tevatron20unifieddocument,
      title={Tevatron 2.0: Unified Document Retrieval Toolkit across Scale, Language, and Modality}, 
      author={Xueguang Ma and Luyu Gao and Shengyao Zhuang and Jiaqi Samantha Zhan and Jamie Callan and Jimmy Lin},
      year={2025},
      eprint={2505.02466},
      archivePrefix={arXiv},
      primaryClass={cs.IR},
      url={https://arxiv.org/abs/2505.02466}, 
}

## RankVideo Citation
@misc{skow2026rankvideoreasoningrerankingtexttovideo,
      title={RANKVIDEO: Reasoning Reranking for Text-to-Video Retrieval}, 
      author={Tyler Skow and Alexander Martin and Benjamin Van Durme and Rama Chellappa and Reno Kriz},
      year={2026},
      eprint={2602.02444},
      archivePrefix={arXiv},
      primaryClass={cs.IR},
      url={https://arxiv.org/abs/2602.02444}, 
}
```