---
title: Python SDK
permalink: /docs/python_sdk/
redirect_from: /docs/index.html
---

本节主要介绍如何使用easypoint提供的python插件接口，利用python接口可以更方便地调用easypoint的功能进行数据处理，包括easypoint的插件。使用方法如下：
## 环境配置
环境配置需要设置easypoint的python包的路径以及dll相关文件的路径，可以通过如下函数进行配置，其中需要替换`ep_install_dir`为EasyPoint的安装目录。在运行其他代码前需要先运行函数`init_ep_python()`
```
def init_ep_python():
    ep_install_dir = r"D:\Program Files\EasyPoint"
    py_path = os.path.join(ep_install_dir, ep_install_dir)
    sys.path.append(py_path)
    third_party_bin_dir = os.path.join(ep_install_dir, "ThirdParty", "bin")

    # 必须添加QT_QPA_PLATFORM_PLUGIN_PATH环境变量
    qt_path = os.path.join(ep_install_dir, "ThirdParty", "bin", "Qt", "release", "platforms")
    os.environ['QT_QPA_PLATFORM_PLUGIN_PATH'] = qt_path
    # 第三方库添加环境变量
    for root, dirs, files in os.walk(third_party_bin_dir):
        for d in dirs:
            os.environ['PATH'] = os.path.join(root, d, 'release') + os.pathsep + os.environ['PATH']
            print(os.path.join(root, d, 'release') + os.pathsep + os.environ['PATH'])
```
