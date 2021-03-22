1. 拷贝yolov5到home目录
2. 进入py38
3. 关闭vpn
4. 下载requirments.txt所需：pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r requirements.txt 
5. pycocotools无法下载。。。先把pycocotools注释了，安装后在解注，再安装



Run the script:

$ cd "/usr/local/zed/"
$ python get_python_api.py

    # The script displays the detected platform versions
    CUDA 10.0
    Platform ubuntu18
    ZED 3.1
    Python 3.7
    # Downloads the whl package
    Downloading python package from https://download.stereolabs.com/zedsdk/3.1/ubuntu18/cu100/py37 ...

    # Gives instruction on how to install the downloaded package
    File saved into pyzed-3.1-cp37-cp37m-linux_x86_64.whl
    To install it run :
      python3 -m pip install pyzed-3.1-cp37-cp37m-linux_x86_64.whl

Now install the downloaded package with pip:


conda activate py38


$ python3 -m pip install pyzed-3.1-cp37-cp37m-linux_x86_64.whl

    Processing ./pyzed-3.1-cp37-cp37m-linux_x86_64.whl
    Installing collected packages: pyzed
    Successfully installed pyzed-3.1

That’s it ! The Python API is now installed. To get started, check out our Tutorials and Code Samples.
