# 2020-11-install-opence
Open-CE is a project that helps you build deep learning frameworks on IBM Power platforms.  Whenever you need to run PyTorch or Tensorflow, using these build tools are highly recommended.

The instructions document what I had to do to get Open-CE up and running in my environment.  I did this on a Power8 / Minsky server with Cuda 10.2 in Nov 2020.  The main instructions for installing the Open-CE package are found here ->

# Additional links / content
[github repo]:
git clone https://github.com/open-ce/open-ce

[youtube video]:
https://youtu.be/--bREvi9LqY

# Prequisites
* Docker
There are a couple of ways to build the software.  The most reliable method I found was to use the repository's --docker_build flag.  Using this flag allows you to reduce the number of dependencies required to have a clean build.  In the end, you will be left with a directory named condabuild from which you can install all the software.

* Anaconda
All packages are pretty much based on the anaconda python distribution.  You will need to have anaconda installed prior to running these tools.

---
# Create Conda Environment to build software
I just used a seperate conda environment to build this software just in case there were any dependencies I needed
conda create -n opence-build python=3.7
conda install conda-build

# Install Anaconda
https://docs.anaconda.com/anaconda/install/linux-power8/#

# Install Docker Prerequisite
yum install docker
service docker start
# fix /var/run/docker* permissions
chown vanstee:docker ...

# Grab TensorRT dependency
https://developer.nvidia.com/nvidia-tensorrt-7x-download
TensorRT-7.0.0.11.CentOS-7.6.ppc64le-gnu.cuda-10.2.cudnn7.6.tar.gz

cd /gpfs/home/s4s004/vanstee/2020-09-opence/techu/open-ce
mkdir local_files
cp ../../TensorRT-7.* local_files/


# Build All
python open-ce/build_env.py --python_versions 3.6 --build_types cuda  envs/opence-env.yaml --docker_build | tee opence.py36.log  

# Build Pytorch
python open-ce/build_env.py --python_versions 3.7 --build_types cuda  open-ce/envs/pytorch-env.yaml --docker_build &> opence.py36.log  
python open-ce/build_env.py  --build_types cuda  open-ce/envs/pytorch-env.yaml --docker_build &> opence.py36.log  # works

# Create Conda Environment for Pytorch 1.6!
conda create -n oce-pytorch-1.6 python=3.6
conda activate oce-pytorch-1.6
conda install -c ./condabuild pytorch


# Install FastAI dependencies
conda install -y jupyter
conda install -y pandas
conda install -y spacy
conda install -y scikit-learn
pip install --no-deps fastai


pip install  dataclasses
pip install fastprogress
pip install fastcore
pip install nbdev

# Jupyter Notebook dependencies
conda install -y jupyterlab
conda install -y nodejs
jupyter labextension install @jupyterlab/toc
pip install jupyter_nbextensions_configurator

# Kaggle
pip install kaggle

============================
conda install -y -c anaconda nltk
conda install -y bottleneck
conda install -y beautifulsoup4
conda install -y numexpr
conda install -y nvidia-ml-py3 -c fastai
conda  install -y  packaging
conda install -c conda-forge seaborn

git clone https://github.com/fastai/fastai.git






-------------- Previous ---------------------------
[p10a114]
cd ~/2020-09-opence/open-ce/builder
mkdir local_files
cp ../../TensorRT-7.0.0.11.CentOS-7.6.ppc64le-gnu.cuda-10.2.cudnn7.6.tar.gz local_files/

python open-ce/builder/build_env.py --python_versions 3.7 --build_types cuda  open-ce/envs/pytorch-env.yaml  &> opence.log

[Related links that could help]

https://github.com/open-ce/open-ce/issues/7
https://developer.nvidia.com/nvidia-tensorrt-7x-download
