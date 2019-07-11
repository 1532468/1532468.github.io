---
title : "Anaconda"
categories:
  - Linux
---
<!-- <figure>
  <img src="/assets/images/2019-07-11-Anaconda/logo.png">
  <figcaption></figcaption>
</figure> -->

Anaconda 가상환경과 jupyterlab을 만들어 봅시다

- Anaconda 설치
~~~
wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh

bash ./Anaconda3-2019.03-Linux-x86_64.sh
~~~

- conda env
~~~
> conda create -n test_env python=3.7

> conda activate test_env 
~~~
- kernel
~~~
> conda install -y -c conda-forge jupyterlab ipykernel

> python -m ipykernel install --user --name test_env --display-name "TEST_Kernel"
~~~
- 실행
~~~
> nohup jupyter-lab \
--no-browser \
--ip 0.0.0.0 \
--port 0000 \
--notebook-dir [폴더 지정] \
'--KernelSpecManager.whitelist=["test_env"]' \
>> ~/jupyter.log 2>&1 &
~~~