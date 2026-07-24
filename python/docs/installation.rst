安装指南
========

系统要求
--------

- Python 3.8 或更高版本
- Linux ARM64 架构（Hailo-15, RK3588, Jetson）
- AIPC Platform 运行环境

依赖项
------

SDK 依赖以下 Python 包：

- ``grpcio >= 1.50.0`` - gRPC 通信
- ``protobuf >= 4.21.0`` - Protocol Buffers
- ``numpy >= 1.20.0`` - 数组处理
- ``Pillow >= 9.0.0`` - 图像处理

安装方法
--------

从 PyPI 安装（推荐）
~~~~~~~~~~~~~~~~~~~~

.. code-block:: bash

   pip install hailo-ipc-sdk

从源码安装
~~~~~~~~~~

.. code-block:: bash

   git clone https://github.com/camthink-ai/ne503-aipc-sdks.git
   cd ne503-aipc-sdks/python
   pip install -e .

构建 Wheel 包
~~~~~~~~~~~~~

.. code-block:: bash

   git clone https://github.com/camthink-ai/ne503-aipc-sdks.git
   cd ne503-aipc-sdks/python
   python -m pip install --upgrade build
   python -m build --wheel
   ls dist/*.whl

生成的 ``.whl`` 文件位于 ``dist/`` 目录，可直接安装验证：

.. code-block:: bash

   pip install dist/hailo_ipc_sdk-*.whl

从 Tarball 安装
~~~~~~~~~~~~~~~

.. code-block:: bash

   # 下载 SDK 包
   wget https://github.com/camthink-ai/ne503-aipc-sdks/releases/download/v0.3.0/ne503-aipc-sdk-0.3.0-arm64.tar.gz

   # 解压并安装
   tar -xzf aipc-sdk-0.2.0-arm64.tar.gz
   cd aipc-sdk-0.2.0-arm64
   pip install .

验证安装
--------

.. code-block:: python

   import hailo_ipc_sdk
   print(hailo_ipc_sdk.__version__)
   # 输出: 0.2.0

开发环境安装
------------

如果你需要开发或测试 SDK，可以安装开发依赖：

.. code-block:: bash

   pip install -e ".[dev]"

这将安装额外的工具：

- ``pytest`` - 单元测试
- ``pytest-cov`` - 测试覆盖率
- ``black`` - 代码格式化
- ``flake8`` - 代码检查
- ``mypy`` - 类型检查

Docker 环境
-----------

使用预构建的 Docker 镜像：

.. code-block:: bash

   docker pull registry.local/aipc-sdk:0.2.0
   docker run -it --rm registry.local/aipc-sdk:0.2.0 python3

或者构建自己的镜像：

.. code-block:: dockerfile

   FROM python:3.10-slim
   RUN pip install hailo-ipc-sdk
   WORKDIR /app
   COPY app.py .
   CMD ["python3", "app.py"]

故障排除
--------

权限问题
~~~~~~~~

如果遇到 Unix socket 权限错误：

.. code-block:: bash

   # 确保用户在 aipc 组中
   sudo usermod -aG aipc $USER

   # 重新登录或刷新组
   newgrp aipc

gRPC 连接失败
~~~~~~~~~~~~~

检查平台服务是否运行：

.. code-block:: bash

   # 检查服务状态
   systemctl status ai-runtime
   systemctl status event-bus
   systemctl status device-control

   # 检查 socket 文件
   ls -l /run/aipc/*.sock

依赖冲突
~~~~~~~~

如果遇到依赖版本冲突，建议使用虚拟环境：

.. code-block:: bash

   python3 -m venv venv
   source venv/bin/activate
   pip install hailo-ipc-sdk
