# Jupyter notebook “Schema Not Found” 404 错误

> 原文：[`techbyexample.com/jupyter-notebook-schema-not-found/`](https://techbyexample.com/jupyter-notebook-schema-not-found/)

# **概述**

如果你是通过 homebrew 安装了 Python，那么在运行下面的 jupyter notebook 命令时，可能会遇到以下错误：

```go
jupyter notebook
```

错误信息如下：

```go
Missing or misshapen translation settings schema:
    HTTP 404: Not Found (Schema not found: /opt/homebrew/Cellar/python@3.10/3.10.12/Frameworks/Python.framework/Versions/3.10/share/jupyter/lab/schemas/@jupyterlab/translation-extension/plugin.json)
```

# 解决方案

第一种方法是导出以下内容：

```go
export JUPYTER_PATH=/opt/homebrew/share/jupyter
export JUPYTER_CONFIG_PATH=/opt/homebrew/etc/jupyter
```

第二种方法是安装早期版本：

```go
python3 -m pip install "notebook<7"
```

希望这能解决问题。请再次尝试运行“jupyter notebook”命令。
