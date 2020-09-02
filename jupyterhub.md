## 安装jupyterhub

使用root权限安装

```shell
sudo su
gedit ~/.bashrc
```

打开后添加

```shell
# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/opt/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/opt/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/opt/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/opt/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<
```

让修改生效

```shell
source ~/.bashrc
```

配环境

```shell
conda create -n jupyterhub python=3.6
source activate 
conda activate jupyterhub
conda install jupyterhub
conda install notebook
conda install nb_conda_kernels
conda install tensorflow=1.12
pip install jupyter-tensorboard
```

生成并修改jupyterhub配置文件

```shell
jupyterhub --generate-config
gedit /home/wh/jupyterhub_config.py
```

打开后修改默认的notebook目录为根目录

```shell
c.Spawner.notebook_dir = '/'
```

启动jupyterhub（如果出现http报错，`ps aux | grep configurable-http-proxy`，找到影响的进程杀死）

```shell
jupyterhub -f /home/wh/jupyterhub_config.py --ip 192.168.1.111
```



---



## 把jupyterhub挂在后台

```shell
screen -dmS jupyterhub
screen -ls
screen -r jupyterhub
```

关闭会话

```shell
screen -X -S jupyterhub quit
```



---



## 创建新用户

```shell
sudo useradd -r -m -s /bin/bash wh1h
sudo passwd wh1h
sudo adduser wh1h sudo
```



## 配置新用户

使用新用户登录

* 让新用户使用anaconda

  ```shell
  gedit ~/.bashrc
  ```

  打开后添加

  ```shell
  # >>> conda initialize >>>
  # !! Contents within this block are managed by 'conda init' !!
  __conda_setup="$('/opt/anaconda3/bin/conda' 'shell.bash' 'hook' 2> /dev/null)"
  if [ $? -eq 0 ]; then
      eval "$__conda_setup"
  else
      if [ -f "/opt/anaconda3/etc/profile.d/conda.sh" ]; then
          . "/opt/anaconda3/etc/profile.d/conda.sh"
      else
          export PATH="/opt/anaconda3/bin:$PATH"
      fi
  fi
  unset __conda_setup
  # <<< conda initialize <<<
  ```

  让修改生效

  ```shell
  source ~/.bashrc
  ```

* 让新用户使用tensorboard（重启服务后生效）

  ```shell
  jupyter tensorboard enable --user
  ```

* 让新用户的环境在jupyterhub中显示

  ```shell
  source activate 
  conda activate xxx
  conda install ipykernel
  ```
