<div align="center" style="display: flex; align-items: center; justify-content: center;">
  <a href="https://github.com/batmen-lab/BioMANIA" target="_blank">
    <img src="./images/BioMANIA.png" alt="BioMANIA Logo" style="width: 80px; height: auto; margin-right: 10px;">
  </a>
  <h1 style="margin: 0; white-space: nowrap;">BioMANIA</h1>
</div>

[![Demo](https://img.shields.io/badge/Demo-BioMANIA-blue?style=flat&logo=appveyor)](https://biomania.ngrok.io/en)
[![Docker Version](https://img.shields.io/badge/Docker-v1.1.12-blue?style=flat&logo=docker)](https://hub.docker.com/repositories/chatbotuibiomania)
[![Paper](https://img.shields.io/badge/Paper-burgundy?style=flat&logo=arxiv)](https://www.biorxiv.org/content/10.1101/2023.10.29.564479)
[![GitHub stars](https://img.shields.io/github/stars/batmen-lab/BioMANIA?style=social)](https://github.com/batmen-lab/BioMANIA)
[![License](https://img.shields.io/badge/license-Apache%203.0-blue?style=flat&logo=open-source-initiative)](https://github.com/batmen-lab/BioMANIA/blob/main/LICENSE)
[![Documentation Status](https://img.shields.io/readthedocs/biomania/latest?style=flat&logo=readthedocs&label=Doc)](https://biomania.readthedocs.io/en/latest/?badge=latest)
<!--[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/14K4562oeesEz5qMoXmjv9gW_4VeLh6_U?usp=sharing)-->
<!--[![Railway](https://img.shields.io/badge/Railway-purple?style=flat&logo=railway)](https://railway.app/template/qaQEvv)-->
<!--[![Python unit tests](https://github.com/batmen-lab/BioMANIA/actions/workflows/python-test-unit.yml/badge.svg)](https://github.com/batmen-lab/BioMANIA/actions/workflows/python-test-unit.yml)-->


Welcome to the BioMANIA! This guide provides detailed instructions on how to set up, run, and interact with the BioMANIA chatbot interface, which connects seamlessly with various APIs to deliver information across numerous libraries and frameworks.


Project Overview:

<!--![](./images/overview_v2.jpg)-->
<!--![](./images/Motivation.jpg)-->
<!--![](./images/Methods.jpg)-->
![](./images/overview.jpg)


🌟 We warmly invite you to share your trained models and datasets in our [issues section](https://github.com/batmen-lab/BioMANIA/issues/2), making it easier for others to utilize and extend your work, thus amplifying its impact. Feel free to explore and provide feedback on tools shared by other contributors as well! 🚀🔍

We welcome 🤗 you to refer to the [Q&A](./docs/Q&A.md) section if you encounter any problems during your exploration and contribute some issues for discussion! 🧐 👨‍💻

# Video demo

Our demonstration showcases how to utilize a chatbot to simultaneously use scanpy and squidpy in a single conversation, including loading data, invoking functions for analysis, and presenting outputs in the form of code, images, and tables

<img src="examples/video_demo.gif" style="width:800px;height:400px;animation: play 0.05s steps(100) infinite;">

We also offer a command-line interface (CLI) demo through the terminal.

<img src="examples/cli.gif" style="width:800px;height:500px;animation: play 0.05s steps(100) infinite;">

<!--We also offer a GPTs demo (under developing).

<img src="examples/GPTs.gif" style="width:800px;height:450px;animation: play 1s steps(10) infinite;">-->

# Web access online demo

We provide [![Online Demo](https://img.shields.io/badge/Demo-BioMANIA-blue?style=flat&logo=appveyor)](https://biomania.ngrok.io/en)
 hosted on our server! 

(240929-For Online Demo, note that when multiple user are using, there might be delay in connection. We will check the demo running everyday, issue (if any) will be fixed in the next day. It is recommended to ask question in English in this time, as the corpus is designed for English and thus results will be more accurate.)

# Quick start

We provide several ways to run the service: python script, terminal CLI, Docker, colab demo. Among those, terminal CLI is the easiest way to start. \

## Setup dataset and models
```bash
# setup the environment
pip install git+https://github.com/batmen-lab/BioMANIA.git  --index-url https://pypi.org/simple
# setup OPENAI_API_KEY
echo 'OPENAI_API_KEY="sk-proj-xxxx"' >> .env
# (optional) setup github token
echo "GITHUB_TOKEN=your_github_token" >> .env
# download data, retriever, and resources from drive, and put them to the 
# - data/standard_process/{LIB} and 
# - hugging_models/retriever_model_finetuned/{LIB} and 
# - ../../resources/
pip install gdown
gdown https://drive.google.com/uc?id=1nT28pIJ_dsdvi2yD8ffWt_aePXsSWdqI
sh download_data_model.sh
# setup the PYTHONPATH
export PYTHONPATH=$PYTHONPATH:$(pwd)
```

## Run with terminal CLI or gradio app (stable on Linux)

```bash
# CLI service quick start!
pip install gradio
python -m BioMANIA.deploy.cli_demo
# or gradio app. (TODO 240509: Images showing are under developing!)
#python -m BioMANIA.deploy.cli_gradio
```

## Run with Docker

For ease of use, we provide Docker image containing scanpy, squidpy, ehrapy, snapatac2. You can refer the detailed tools list from [dockerhub](https://hub.docker.com/repositories/chatbotuibiomania).

```bash
# Pull back-end service and front-end UI service with:
# 241016 updated
sudo docker pull chatbotuibiomania/biomania-together:v1.1.12-cuda12.6-ubuntu22.04
```

Start service with
```bash
# run on gpu
sudo docker run -e LIB=scanpy -e OPENAI_API_KEY=[your_openai_api_key] -e GITHUB_TOKEN=[github_pat_xxx] --gpus all -d -p 3000:3000 chatbotuibiomania/biomania-together:v1.1.12-cuda12.6-ubuntu22.04
# or on cpu
sudo docker run -e LIB=scanpy -e OPENAI_API_KEY=[your_openai_api_key] -e GITHUB_TOKEN=[github_pat_xxx] -d -p 3000:3000 chatbotuibiomania/biomania-together:v1.1.12-cuda12.6-ubuntu22.04
```

Then check UI service with `http://localhost:3000/en`.

Important Tips for Running Docker Without Bugs:
- To run docker on GPU, you need to install `nvidia-docker` and [`nvidia container toolkit`](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html). Run `docker info | grep "Default Runtime"` to check if your device can run docker with gpu.
- Feel free to adjust the [cuda image version](https://hub.docker.com/r/nvidia/cuda/tags?page=1) inside the `Dockerfile` to configure it for different CUDA settings which is compatible for your device.

We understand the desire to run the service on a server and visualize locally. You can initiate the [ngrok service](https://ngrok.com/docs/getting-started/) by running this script on your server:
```bash
ngrok http 3000
```

then get the url like `https://[ngrok_id].ngrok-free.app` and copy it to chrome to start!

<!--## Run with Railway

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/qaQEvv)

To use railway, you'll need to fill in the `OpenAI_API_KEY` in the Variables page of the biomania-backend service. Then, manually enable `Public Domain` in the Settings/Networking session for both front-end and back-end service. Copy the url from back-end as `https://[copied url]` and paste it in `BACKEND_URL` in front-end Variables page. For front-end url, paste it to the browser to access the frontend.-->

## Run with script

This section is provided for user who want DIY more flexible function.

For instance, let's take `scanpy` as an example. Detailed library support information can be found in the [Q&A](./docs/Q&A.md)

### Setting up for environment
To prepare your environment for the BioMANIA project, follow these steps:

1. Clone the repository and install dependencies:
```bash
git clone https://github.com/batmen-lab/BioMANIA.git
cd BioMANIA
conda create -n biomania python=3.9
conda activate biomania
pip install -r requirements.txt --index-url https://pypi.org/simple
export PYTHONPATH=$PYTHONPATH:$(pwd)
```

2. Set up your OpenAI API key in the `BioMANIA/.env` file.
```bash
echo 'OPENAI_API_KEY="sk-proj-xxxx"' >> .env
```

- For inference purposes, a standard OpenAI API key is sufficient.
- If you intend to use functionalities such as instruction generation or GPT API predictions, a paid OpenAI account is required as it may reach rate limit. 
- **Feel free to switch to `model_name='gpt-3.5-turbo-0125'` or `gpt-4-0125-preview` in `src/models/model.py` if you want.**

### Prepare for Data and Model
Download the necessary data and models from our [Google Drive link](https://drive.google.com/drive/folders/1BRoq007udu8QH-lwTwCkaFZfG69amB01?usp=drive_link). For those library data, you can download only the one you need.

We provide a script for downloading models and datas from Google Drive for scanpy as an example. This works if you are accessible to google.
```bash
gdown https://drive.google.com/uc?id=1nT28pIJ_dsdvi2yD8ffWt_aePXsSWdqI
sh download_data_model.sh
```

Organize the downloaded files at `BioMANIA/data` or `BioMANIA/hugging_models` as follows (`base` are necessary):
```
data
├── conversations
├── others-data
└── standard_process
    ├── base
    │   ├── API_composite.json
    │   └── ...
    ├── scanpy
    │   ├── API_composite.json
    │   └── ...
    ├── {LIB}
    │   ├── API_composite.json
    │   └── ...
    └── ...

hugging_models
└── retriever_model_finetuned
    ├── {LIB}
    └── ...

../../resources
```

By meticulously following the steps above, you'll have all the essential data and models perfectly organized for the project.

We also offer some demo chat, you can find them in [`./examples`](https://github.com/batmen-lab/BioMANIA/blob/main/examples). Notice that these demo chat are converted from the PyPI readthedoc tutorials. You can check the original tutorial link through the `tutorial_links.txt`.

![](./images/demo_full.jpg)

### Prepare for front-end UI service

This is compatible with Node.js version 19.
```bash
# Under folder BioMANIA/chatbot_ui_biomania
npm install && npm run build
```

### Inference with pretrained models

Start both services for back-end and front-end UI with:
```bash
# Under folder `BioMANIA/`
# backend, in one terminal
python -m src.deploy.inference_dialog_server
# frontend, in another terminal
cd chatbot_ui_biomania/
npm run dev 
```

Your chatbot server is now operational at `http://localhost:3000/en`, primed to process user queries.

> **When selecting different libraries on the UI page, the retriever's path will automatically be changed based on the library selected**

### DIY

For users who wish to customize functionality more deeply, we provide a script example that demonstrates direct interaction with the BioMANIA library via a Python script. In this example, users can 
- switch different initial loaded library
- change the llm type by either ollama supported models i.e. llama3, or openai supported models i.e. gpt-3.5-turbo
- manage the conversation state, either continue the previous saved session, or start a new conversation
This method is particularly suited for developers and researchers who want to quickly adjust and test different data processing strategies based on specific research needs.

```bash
# under BioMANIA/
from src.deploy.model import Model
conversation_started = True
model = Model(logger=None, device='cpu', model_llm_type='llama3')
user_input = "Could you load the built in dataset?"
library = "scanpy"
# for the first turn of a dialog, use conversation_started=True, then use conversation_started=False for the following dialogs
# if you want to use previous session, use the same session_id as before and conversation_started = False
model.run_pipeline(user_input, library, top_k=1, files=[], conversation_started=conversation_started, session_id="")
```

## Build your APP!

Please refer to the separate README for tutorials that supporting converting different coding tools to our APP.
- [For PyPI Tools](./docs/PyPI2APP.md)
- [For Python Source Code from Git Repo](./docs/Git2APP.md)
- [For R Package](./docs/R2APP.md)

## Share your APP!

If you want to share your pretrained APP to others, there are two ways.

### Share docker

You can build docker and push to dockerhub, and share your docker image url in [our issue](https://github.com/batmen-lab/BioMANIA/issues/2). For environment setting of your tool, please refer to `BioMANIA/docker_utils/{LIB}/` to add the env files, or modify the Dockerfile to build your environment.
```bash
# cd BioMANIA
sudo docker build --build-arg LIB=[your_tool_name] -t [docker_image_name] -f Dockerfile ./
# (optional)push to docker
sudo docker push [your_docker_repo]/[docker_image_name]:[tag]
```

Notice if you want to include some data inside the docker, please modify the `Dockerfile` carefully to copy the folders to `/app`. Also add your PyPI or Git pip install url to the `requirements.txt` before your packaging for docker.

### Share data/models

You can just share your `data` and `hugging_models` folder and `logo` image by drive link to [our issue](https://github.com/batmen-lab/BioMANIA/issues/2).

## Reference and Acknowledgments

We extend our gratitude to the following references:
- [Toolbench](https://github.com/OpenBMB/ToolBench) 
- [Chatbot-UI](https://github.com/mckaywrigley/chatbot-ui)
- [SentenceTransformers](https://github.com/UKPLab/sentence-transformers)
- [Topical-Chat-data](https://github.com/alexa/Topical-Chat)
- [ChitChat-data](https://github.com/microsoft/botframework-cli/blob/main/packages/qnamaker/docs/chit-chat-dataset.md)
- [lit-llama](https://github.com/Lightning-AI/lit-llama)
- [ollama](https://github.com/ollama/ollama)

Thank you for choosing BioMANIA. We hope this guide assists you in navigating through our project with ease.


## **Version History**
- v1.1.12 (2024-10-16)
  - Update code scripts & upload data and models & update docker which are aligned with paper.
  - Will renew the scripts for generating report, documents for Git2APP, R2APP soon.
  - Update report generation.
  - Update R2APP and Git2APP document.

view [version_history](./docs/version_history.md) for more details!

## **Star History**

[![Star History Chart](https://api.star-history.com/svg?repos=batmen-lab/BioMANIA&type=Date)](https://star-history.com/#batmen-lab/BioMANIA&Date)

## **Citation**

Please cite our paper if you fine our data, model or code useful.

```
@article{dong2023biomania,
  title={BioMANIA: Simplifying bioinformatics data analysis through conversation},
  author={Dong, Zhengyuan and Zhong, Victor and Lu, Yang},
  journal={bioRxiv},
  pages={2023--10},
  year={2023},
  publisher={Cold Spring Harbor Laboratory}
}
```
