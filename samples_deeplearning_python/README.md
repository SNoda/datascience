# samples_deeplearning_python

## Description

Pythonによるディープラーニング関連のサンプルコード集.  
基本的にjupyter notebookで実行プログラムと結果を保存する.  
各ノートブックにはPythonのバージョンとライブラリバージョンおよびGPU周辺バージョンを確認できるよう以下の実行セルを含めること.

Pythonバージョン
```
!python --version
```

ライブラリバージョン
```
!pip freeze
```

NVIDIAドライババージョン
```
!cat /proc/driver/nvidia/version
```

cudaバージョン
```
!nvcc -V
```

## How to run

Use nvidia-docker.  
Set up commands are

```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update

sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd

# Test nvidia-smi
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

See https://github.com/NVIDIA/nvidia-docker

Start command is

```
docker build -t samples_deeplearning_python docker/.
```

Using container image is https://github.com/ufoym/deepo  

If use jupyter notebook, 

```
docker run --runtime=nvidia -v `pwd`:/tmp/work -w=/tmp/work -p 8888:8888 --rm -it samples_deeplearning_python jupyter notebook --no-browser --ip=* --notebook-dir=/tmp/work --allow-root
```

If use bash,

```
docker run --runtime=nvidia -v `pwd`:/tmp_work -w=/tmp/work --rm -it samples_deeplearning_python bash
```

Using chainer pre-trained model, for example resnet, download weight files see, https://github.com/KaimingHe/deep-residual-networks
Make dir and put weight files to host `root/.chainer/dataset/pfnet/chainer/models/`, 

```
docker run --runtime=nvidia -v `pwd`:/tmp/work -v /root:/root -w=/tmp/work -p 8888:8888 --rm -it samples_deeplearning_python jupyter notebook --no-browser --ip=* --notebook-dir=/tmp/work --allow-root
```

if use Plotly, add jupyter notebook option `--NotebookApp.iopub_data_rate_limit=1.0e10`.
