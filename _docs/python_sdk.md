---
title: Python SDK
permalink: /docs/python_sdk/
redirect_from: /docs/index.html
---

本节主要介绍如何使用easypoint提供的python插件接口，利用python接口可以更方便地调用easypoint的功能进行数据处理，包括easypoint的插件。使用方法如下：
## 1.环境配置
环境配置需要设置easypoint的python包的路径以及dll相关文件的路径，可以通过如下函数进行配置，其中需要替换`ep_install_dir`为EasyPoint的安装目录。在运行其他代码前需要先运行函数`init_ep_python()`，以下代码在python3.8版本上测试通过，请安装python3.8使用，可以使用anoconda创建虚拟环境。
```
import sys
import os

ep_install_dir = r"D:\Program Files\EasyPoint"

def init_ep_python():
    py_path = os.path.join(ep_install_dir, "Build")
    sys.path.append(py_path)
    os.environ['PATH'] = py_path + os.pathsep + os.environ['PATH']

    # 必须添加QT_QPA_PLATFORM_PLUGIN_PATH环境变量
    qt_path = os.path.join(ep_install_dir, "ThirdParty", "bin", "Qt", "release", "platforms")
    os.environ['QT_QPA_PLATFORM_PLUGIN_PATH'] = qt_path

    # 第三方库添加环境变量
    third_party_bin_dir = os.path.join(ep_install_dir, "ThirdParty", "bin")
    for root, dirs, files in os.walk(third_party_bin_dir):
        for d in dirs:
            release_dir = os.path.join(root, d, 'release')
            if os.path.exists(release_dir):
                os.add_dll_directory(release_dir)
                os.environ['PATH'] = release_dir + os.pathsep + os.environ['PATH']
```

## 2. 调用插件对点云进行处理，以CSF点云滤波插件为例，代理示例如下：
```
# 初始化EasyPoint环境
init_ep_python()
# 导入EasyPoint模块，需要先执行init_ep_python()，再导入EPython模块
import EPython as EP

# 定义EasyPoint的安装路径和资源路径
py_path = os.path.join(ep_install_dir, "Build")
res_path = os.path.join(ep_install_dir, "Resource")
# 使用插件前必须激活
activated = EP.PluginManager.activate(os.path.join(py_path, r'license.lic'), os.path.join(py_path, r'EasyPoint_V1.0_PRIVATE.key'))

# 加载插件
# 首先加载IO插件，用于读写las文件
BIFIO = EP.PluginManager.loadDLLPlugin(os.path.join(py_path, r'BIFIO_PLUGIN.dll'))
lasFilter = EP.ExtensionFilter.getFilterFromExt('las').get()
print(lasFilter.getDetail())
# 加载GMFIO插件，用于读写obj文件
objIO = EP.PluginManager.loadDLLPlugin(os.path.join(py_path, r'GMFIO_PLUGIN.dll'))
objFilter = EP.ExtensionFilter.getFilterFromExt('obj').get()
print(objFilter.getDetail())
# 加载ACSF插件，用于布料滤波
ACSF_plugin = EP.PluginManager.loadDLLPlugin(os.path.join(py_path, r'ACSF_PLUGIN.dll'))
ACSF = EP.Cast.pluginAsStd(ACSF_plugin)
print(ACSF.getDetail())

# 读取las
input_las_path = os.path.join(res_path, "data", "pointcloud", "samp11.las")
las_rp = lasFilter.getDefaultReadFileParameters()
renderData = lasFilter.processReadFile(input_las_path, las_rp)

# 务必将数据添加到管理器中
EP.RenderDataManager.addData(renderData)

# 使用ACSF插件
ACSF_input_names = ACSF.getInputNames()
ACSF_inputs = {}
ACSF_params = {}
ACSF_inputs.update({ACSF_input_names[0]: str(renderData.getRenderID())})
param_names = ACSF.getParameterNames()
ACSF_params.update({param_names[0]: str(1)})  # [Postprocessing(是否后处理0/1):int]
ACSF_params.update({param_names[1]: str(1)})  # [Cloth_Resolution(布料分辨率):double]
ACSF_params.update({param_names[2]: str(200)})  # [Max_Iteration(最大迭代次数):int]
ACSF_params.update({param_names[3]: str(0.5)})  # [Class_Threshold(分类阈值):double]
ACSF_params.update({param_names[4]: str(1)})  # [Export_Cloth_Mesh(是否导出布料0/1):int]
ACSF_results = ACSF.process(ACSF_inputs, ACSF_params)

# 保存ACSF结果
save_ground_path = os.path.join(res_path, "data", "pointcloud", "samp11_ground.las")
save_nonground_path = os.path.join(res_path, "data", "pointcloud", "samp11_nonground.las")
save_cloth_path = os.path.join(res_path, "data", "pointcloud", "samp11_cloth.obj")
las_wp = lasFilter.getDefaultWriteFileParameters()
obj_wp = objFilter.getDefaultWriteFileParameters()
lasFilter.processWriteFile(save_ground_path, ACSF_results[0], las_wp)
lasFilter.processWriteFile(save_nonground_path, ACSF_results[1], las_wp)
objFilter.processWriteFile(save_cloth_path, ACSF_results[2], obj_wp)

# 从管理器中移除数据
EP.RenderDataManager.removeData(renderData.getRenderID())
```
