1
https://blog.csdn.net/qq_33254440/article/details/107731031

2
https://github.com/zhaolongkzz/DeepMimic_configuration

3
from . import _DeepMimicCore ImportError: libGLEW.so.2.1: cannot open shared object file: No such file or directory
 解决方法：添加软连接，让程序找到所引用位置:
 sudo ldconfig /usr/lib64
