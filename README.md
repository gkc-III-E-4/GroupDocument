# 配置环境

- 安装docker
- 编译dockerfile
  ```bash
  docker build -t fedlab .
  ```
- 进入docker中
  ```bash
  sed -i 's/http:\/\/archive.ubuntu.com/http:\/\/mirror.sjtu.edu.cn/g' /etc/apt/sources.list
  apt update
  apt install -y git
  git clone https://github.com/gkc-III-E-4/FedLab
  git clone https://github.com/gkc-III-E-4/FedLab-benchmarks
  cd ~/FedLab
  pip install -r requirements.txt
  python setup.py install
  pip install .
  cd ~/FedLab-benchmarks
  pip install -r requirements.txt
  ```

- 测试
  ```bash
  cd ~/FedLab
  python test_bench.py
  ```

- 运行样例
  - 通信样例
    ```bash
    cd ~/Fedlab/examples # 参照相关的内容即可，用docker就不会出错，可能和torch.distributed有关
    ```
  - 优化器样例
	```bash
	cd ~/Fedlab-benchmarks/fedavg_v1.2.0
	bash run.sh # 不要先去leaf或者datasets下载数据，不然会出错
	```

- 可能遇到的错误
  - mnist访问错误：提前下载了数据，导致数据没下载到正确的位置&没下载好，把项目删了重新clone就好（其实是删掉gitignore中对应的数据所在位置）
  - 地址绑定出错：如下所示，需要执行`export GLOO_SOCKET_IFNAME={}`，{}里填`ifconfig`里看到的有ip地址的interface名称，一般是en0、eth0、ens3等
	```
	[W ProcessGroupGloo.cpp:684] Warning: Unable to resolve hostname to a (local) address. Using the loopback address as fallback. Manually set the network interface to bind to with GLOO_SOCKET_IFNAME. (function operator())
	```
  - torch.distributed.send & recv 报错：我也不知道为啥，但是换成docker就没这问题了，唯一一个相关资料在[这项目下面的html文件](error.html)里。
	```
	libc++abi: terminating with uncaught exception of type gloo::EnforceNotMet: [enforce fail at /Users/distiller/project/conda/conda-bld/pytorch_1595629430416/work/third_party/gloo/gloo/transport/uv/pair.cc:248] op.nread == op.preamble.nbytes.
	```