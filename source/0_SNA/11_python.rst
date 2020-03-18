^^^^^^^^^^^^^^^
Python
^^^^^^^^^^^^^^^


安装与配置
*************

Pip 与conda
################

Pip 更换源
==================
* 清华镜像站 : https://mirror.tuna.tsinghua.edu.cn/help/pypi/

.. code-block:: bash
    :linenos:

    # 临时使用
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package

    # 设为默认
    pip install pip -U # 升级 
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

