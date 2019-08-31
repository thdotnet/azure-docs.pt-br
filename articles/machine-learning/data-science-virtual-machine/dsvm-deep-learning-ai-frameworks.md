---
title: Aprendizado profundo & estruturas de ia
titleSuffix: Azure Data Science Virtual Machine
description: Estruturas e ferramentas de aprendizado profundo disponíveis no Azure Máquina Virtual de Ciência de Dados, incluindo TensorFlow, PyTorch, Keras, Caffe, MXNet, Horovod, Theano, chainer e muito mais.
keywords: ferramentas de ciência de dados, máquina virtual de ciência de dados, ferramentas para ciência de dados, ciência de dados do linux
services: machine-learning
ms.service: machine-learning
ms.subservice: data-science-vm
author: vijetajo
ms.author: vijetaj
ms.topic: conceptual
ms.date: 08/11/2019
ms.openlocfilehash: bcf5228b6d6055c4b2e716ca6b34878a3bb3b870
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70192181"
---
# <a name="deep-learning-and-ai-frameworks-for-azure-data-science-vm"></a>Estruturas de aprendizado profundo e de ia para o Azure VM de Ciência de Dados
O [máquina virtual de ciência de dados](https://aka.ms/dsvm) (DSVM) dá suporte a várias estruturas de aprendizado profundo para ajudar a criar aplicativos de ia (inteligência artificial) com análise preditiva e recursos cognitivas, como reconhecimento de imagem e linguagem.

As estruturas de aprendizado profundo disponíveis no DSVM incluem:

+ TensorFlow
+ PyTorch
+ Keras
+ Caffe
+ Caffe2
+ Torch
+ Servidor de Modelo MXNet
+ MXNet
+ Horovod
+ Theano
+ Chainer
+ Deep Water
+ NVIDIA DIGITS
+ nvidia-smi
+ Fornecimento do TensorFlow
+ TensorRT
+ Kit de Ferramentas Cognitivas da Microsoft

|Ferramentas&nbsp;&nbsp;deDLnoDSVM&nbsp;|Windows|Linux|Notas&nbsp;de uso|
|---------|:-------------------:|:------------------:|-----|
|[TensorFlow](https://www.tensorflow.org/) | Sim (Windows 2016) | Sim |Instalado no Python 3,5 no [Linux e no windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e no Python 3,6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Os notebooks Jupyter de exemplo estão incluídos no DSVM.<br/><br/>**Para executá-lo**:<br/>* Terminal: Ative o ambiente correto e execute Python. <br/> Jupyter conecte-se ao [Jupyter](provision-vm.md#tools) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório TensorFlow para obter exemplos.  |
|[PyTorch](https://pytorch.org/)| Não | Sim |Instalado no [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). Os notebooks Jupyter de exemplo estão incluídos e os exemplos estão em/dsvm/Samples/pytorch.    <br/><br/>**Para executá-lo**<br/>* Terminal: Ative o ambiente correto e execute Python.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux): Conecte-se e, em seguida, abra o diretório PyTorch para obter exemplos.  |
|[Keras](https://keras.io/)| Sim | Sim |A API é instalada no Python 3,5 no [Linux e no windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e no Python 3,6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). [Amostras](https://github.com/fchollet/keras/tree/master/examples)<br/><br/>**Para executá-lo**:<br/>* Terminal: Ative o ambiente correto e execute Python. <br/> Jupyter baixe os exemplos da localização do GitHub, conecte-se ao [Jupyter](provision-vm.md#tools) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório de exemplo. |
|[Caffe](https://github.com/caffe2/caffe2) | Não |Sim (Ubuntu)|O Caffe é instalado em `/opt/caffe`.   Os exemplos estão `/opt/caffe/examples`em. <br/><br/>**Para executá-lo**, use X2Go para fazer logon em sua VM e, em seguida, inicie um novo terminal e digite:<br/>`cd /opt/caffe/examples`<br/>`source activate root`<br/>`jupyter notebook`<br/><br/>Uma nova janela do navegador é aberta com blocos de anotações de exemplo. Binários são instalados em /opt/caffe/build/install/bin.<br/><br/>A versão instalada do Caffe requer Python 2.7 e não funcionará com o Python 3.5 ativado por padrão. Para alternar para o Python 2,7, `source activate root` execute para alternar o ambiente Anaconda.|
|[Caffe2](https://github.com/caffe2/caffe2) | Não |Sim (Ubuntu)|O Caffe2 é instalado no [ambiente conda (raiz) do Python 2.7](dsvm-languages.md#python-linux-and-windows-server-2012-edition). A fonte está em `/opt/caffe2`.<br/>Exemplos de blocos de anotações estão incluídos no JupyterHub.<br/><br/>**Para executá-lo**:<br/>* No terminal: ative o [ambiente de Python raiz](dsvm-languages.md#python-linux-and-windows-server-2012-edition), inicie o Python e importe Caffe2. <br/> * No JupyterHub: [conecte-se ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, navegue até o diretório Caffe2 para localizar os blocos de anotações de exemplo. Alguns blocos de anotações exigem que a raiz Caffe2 seja definida no código Python; insira /opt/caffe2. |
|[Torch](http://torch.ch/) | Não |Sim (Ubuntu)|O Torch é instalado em `/dsvm/tools/torch`. O PyTorch é instalado em Python 2.7 (_raiz_), bem como no ambiente Python 3.5 (_py35_). Os exemplos de Torch `/dsvm/samples/torch` estão em e os exemplos `/dsvm/samples/pytorch`de PyTorch estão em. |
|[MXNet](https://mxnet.io/) | Sim (Windows 2016) | Sim|MXNet é instalado em `C:\dsvm\tools\mxnet` no Windows e `/dsvm/tools/mxnet` no Linux. As associações de Python estão instaladas em Python 3.5 no [Linux e Windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e Python 3.6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Associações do R também são instaladas no Ubuntu.<br/><br/>Exemplos de blocos de anotações do Jupyter estão incluídos. <br/><br/>**Para executá-lo**:<br/>Componentes Ative o ambiente correto e execute Python. <br/> Jupyter conecte-se ao [Jupyter](provision-vm.md#tools) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório mxnet para obter exemplos.|
|[Servidor de modelo MXNet](https://github.com/awslabs/mxnet-model-server) | Não | Sim |Um servidor para criar pontos de extremidade HTTP para modelos MXNet e ONNX. _mxnet-model-server_ está disponível no terminal. Exemplos na [página do servidor de modelos do MXNet](https://github.com/awslabs/mxnet-model-server).|
|[Horovod](https://github.com/uber/horovod) | Não | Sim (Ubuntu) |Estrutura de aprendizado profundo distribuída para TensorFlow. Horovod está instalado em Python 3.5 no [Ubuntu](dsvm-languages.md#python-linux-and-windows-server-2012-edition).  [Consulte os exemplos](https://github.com/uber/horovod/tree/master/examples)<br/><br/>**Para executá-lo**, ative o ambiente correto no terminal e execute Python. |
|[Theano](https://github.com/Theano/Theano) | Não | Sim (Ubuntu) |O Theano é instalado em Python 2.7 (_raiz_), bem como no ambiente Python 3.5 (_py35_).<br/><br/>**Para executá-lo**: <br/>Componentes Ative a versão do Python que você deseja (raiz ou py35), execute o Python e, em seguida, importe o Theano.<br/>Jupyter Selecione o kernel Python 2,7 ou 3,5 e importe Theano.  <br/>Para contornar um bug MKL recente, você precisa primeiro definir a camada de threading do MKL:<br/><br/>_export MKL_THREADING_LAYER=GNU_|
|[Chainer](https://chainer.org/) |Não | Sim |O Chainer é instalado em [Python 3.5](dsvm-languages.md#python-linux-and-windows-server-2012-edition). ChainerRL e ChainerCV também são instalados. <br/><br/>Exemplos de blocos de anotações estão incluídos no JupyterHub.<br/><br/>**Para executá-lo**: <br/>Componentes Ative o ambiente [python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) , execute _Python_e, em seguida, importe o Chainr. <br/> JupyterHub [Conecte-se ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)e navegue até o diretório do encadeamento para localizar os blocos de anotações de exemplo.|
|[Dígitos NVidia](https://github.com/NVIDIA/DIGITS) | Não | Sim (Ubuntu) |O sistema de aprendizado profundo da NVIDIA para treinar rapidamente modelos de aprendizado profundo. DIGITS está instalado em `/dsvm/tools/DIGITS` e disponível como um serviço, chamado _digits_.  <br/><br/>**Para executá-lo**: <br/>Faça logon na VM com X2Go. Em um terminal, inicie o serviço ```sudo systemctl start digits```. <br/><br/>O serviço demora aproximadamente um minuto para iniciar. Abra um navegador da Web e navegue até `http://localhost:5000`. Observe que os DIGITS não fornecem um logon seguro e não devem ser expostos fora da VM.|
|[CUDA, cuDNN, Driver NVIDIA](https://developer.nvidia.com/cuda-toolkit) |Sim | Sim | |
|nvidia-smi|Sim | Sim |Ferramenta NVIDIA para consultar a atividade de GPU. _nvidia-smi_ está disponível no caminho do sistema. <br/><br/>Inicie um prompt de comando (no Windows) ou um terminal (no Linux) e, em seguida, execute _nvidia-smi_.|
|[Serviço de TensorFlow](https://www.tensorflow.org/serving/) | Não | Sim |Um servidor para inferência em um modelo TensorFlow. _tensorflow_model_server_ está disponível no terminal. Exemplos estão disponíveis [online](https://www.tensorflow.org/serving/).|
|[TensorRT](https://developer.nvidia.com/tensorrt) |  Não | Sim (Ubuntu) |Um servidor inferência de aprendizagem profunda de NVIDIA. O TensorRT é instalado como um pacote _apt_. Exemplos estão disponíveis [online](https://docs.nvidia.com/deeplearning/sdk/tensorrt-developer-guide/index.html#samples).|
|[Microsoft Cognitive Toolkit (CNTK)](https://docs.microsoft.com/cognitive-toolkit/)|Sim | Sim | Instalado no Python 3,5 no [Linux e no windows 2012](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e no Python 3,6 no [Windows 2016](dsvm-languages.md#python-windows-server-2016-edition). Os notebooks Jupyter de exemplo estão incluídos no DSVM. <br/><br/>**Para executá-lo**: <br/>Componentes Ative o ambiente correto e execute o Python. <br/>Jupyter conecte-se ao [Jupyter](provision-vm.md#tools) ou [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux), em seguida, abra o diretório CNTK para obter exemplos. |
|Deep Water|Não | Sim (Ubuntu) |A estrutura de aprendizado profundo para H2O, água profunda é instalada no [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e também está `/dsvm/tools/deep_water`disponível no. Exemplos de blocos de anotações estão incluídos no JupyterHub. O Deep Water requer CUDA 8 com cuDNN 5.1. Este não está no caminho da biblioteca por padrão, como as outras estruturas de aprendizagem profunda usa CUDA 9 e cuDNN 7. Para usar CUDA 8 + cuDNN 5.1 para o Deep Water:<br/><br/>```export LD_LIBRARY_PATH=/usr/local/cuda-8.0/lib64:${LD_LIBRARY_PATH}```<br/>```export CUDA_ROOT=/usr/local/cuda-8.0```<br/><br/>Para usar o Deep Water:<br/>* Terminal: Ative o ambiente [Python 3,5](dsvm-languages.md#python-linux-and-windows-server-2012-edition) e execute _Python_. <br/>* JupyterHub: [Conecte-se ao JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-data-science-virtual-machine-for-linux)e, em seguida, navegue até o diretório Deep_Water para encontrar blocos de anotações de exemplo.|
