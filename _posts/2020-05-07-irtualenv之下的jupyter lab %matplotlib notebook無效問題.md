---
layout: post
title:  "virtualenv之下的jupyter lab `%matplotlib notebook`無效問題"
categories: Python環境設定
published: true
date: 2020-05-07T14:00:00+08:00
---

### 問題描述
使用jupyter lab欲透過magic function 產生互動式視窗，若使用`%matplotlib notebook`會出現`Javascript Error: IPython is not defined in JupyterLab`；若使用`%matplotlib widget`會出現`Canvas(toolbar=Toolbar(toolitems=[('Home', 'Reset original view', 'home', 'home'), ('Back', 'Back to previous …`這樣的output，而不會產生視窗。

### 環境
macOS Catalina 10.15.3
Pyhton 3.7.6
jupyter            1.0.0       
jupyter-client     6.0.0       
jupyter-console    6.1.0       
jupyter-core       4.6.3       
jupyterlab         2.0.1       
jupyterlab-server  1.0.7

#### 解法
整個解法的主要流程是根據[這篇](https://stackoverflow.com/a/56416229)及[這篇](https://github.com/matplotlib/ipympl/issues/148#issuecomment-546994990)的回覆，提到的步驟分是：
```bash
pip install ipympl
# If using JupyterLab
conda install nodejs
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib
```
```bash
conda install -y nodejs
pip install --upgrade jupyterlab
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib
jupyter nbextension enable --py widgetsnbextension
```
#### 整合步驟
整合一下，我們要的步驟應該是
```bash
pip install ipympl
# If using JupyterLab
conda install nodejs
pip install --upgrade jupyterlab
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib
jupyter nbextension enable --py widgetsnbextension
```

### 困難排除
然而不是一切都適用我的環境，主要是因爲我是採用virtualenv來管你虛擬環境，而非conda
故流程稍微改變如下
```bash
cd $path_to_your_vertual_env
source $path_to_your_vertual_env/bin/activation
pip install ipympl
```

接下來原本的步驟是`conda install nodejs`，但因為是在virtualenv之下，故採用[這篇在virtualenv之下安裝node.js](https://calvinx.com/2013/07/11/python-virtualenv-with-node-environment-via-nodeenv/)的步驟(內文的'Â'應該是指`Enter`)
```bash
cd $path_to_your_vertual_env
source $path_to_your_vertual_env/bin/activation
pip install nodeenv
nodeenv -p
```
若在`nodeenv -p`出現`urllib.error.URLError: <urlopen error [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed: unable to get local issuer certificate (_ssl.c:1076)>`，則請參考[這篇](https://stackoverflow.com/a/58525755)針對macOS的解法，去`Applications`資料夾，雙擊執行`install Certificates.command`即可

繼續原本的步驟，因為更新了node.js的版本，所以要rebuild你的jupyter，否則啟用jupyter lab時會出現`JupyterLab build is suggested`這句話

```bash
pip install --upgrade jupyterlab
jupyter lab build
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib
jupyter nbextension enable --py widgetsnbextension
```

### 結論
完整流程為：
```bash
cd $path_to_your_vertual_env
source $path_to_your_vertual_env/bin/activation
pip install ipympl # If using JupyterLab
pip install nodeenv
nodeenv -p # may encounter ssl sertificate verify faild problem
pip install --upgrade jupyterlab
jupyter lab build
jupyter labextension install @jupyter-widgets/jupyterlab-manager
jupyter labextension install jupyter-matplotlib
jupyter nbextension enable --py widgetsnbextension
```
