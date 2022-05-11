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