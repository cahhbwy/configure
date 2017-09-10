# raspberry 配置
___boot下建立ssh文件___

## 系统配置

> **更新软件源**
>> /etc/apt/sources.list
>> ```
>> deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free rpi
>> deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ jessie main contrib non-free rpi
>> ```

> __raspi-config__
>> 更改用户密码
>> 更改语言、时区、WiFi国家
>> 启用ssh
>> 扩展磁盘空间
>> 更新raspi-config

> **设置root密码，允许root以ssh登陆**
>> /etc/ssh/sshd_config
>>> 注释 _PermitRootLogin without-password_
>>> 改为 _PermitRootLogin yes_

> **Wifi配置**
>> /etc/network/interfaces
>> ```
>> allow-hotplug wlan0
>> iface wlan0 inet manual
>> 	wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf_
>> ```
>> 改为
>> ```
>> allow-hotplug wlan0
>> iface wlan0 inet dhcp
>>     wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf_
>> ```

>> /etc/wpa_supplicant/wpa_supplicant.conf
>> 追加
>> ```
>> network={
>>     ssid="xxxx"
>>     psk="*******"
>>     priority=255
>> }
>> ```

> **更新软件和系统**
>> ```bash
>> sudo apt update
>> sudo apt upgrade
>> ```

> **重启，以root身份登陆，更改用户**
> ```bash
> killall -u pi && usermod pi -l monk
> mv /home/pi /home/monk
> usermod monk -d /home/monk
> groupmod pi -n monk
> ```
> 设置swap
> ```
> sudo dd if=/dev/zero of=/swapfile bs=1024 count=2097152 # 2G swap
> sudo mkswap /swapfile
> sudo chmod 600 /swapfile
> sudo swapon /swapfile
> ```

> **重启，以monk登陆，设置ssh**
> ```
> ssh-keygen -t rsa -C cahhbwy@mail.ustc.edu.cn
> nano .ssh/authorized_keys
> chmod 700 .ssh
> chmod 600 .ssh/authorized_keys
> ```

## 软件配置
### 常用软件
```bash
sudo apt-get install htop screen git build-essential cmake python-virtualenv python-dev 
sudo pip3 install thefuck
echo "eval \$(thefuck --alias fuck)" >> ~/.bashrc
source ~/.bashrc
```
### conda
[Conda support for Raspberry Pi 2 and POWER8 LE](https://www.continuum.io/content/conda-support-raspberry-pi-2-and-power8-le)
```
conda install pillow conda-build numpy scipy cython ipython pandas
```

### opencv3
```bash
sudo apt-get install libjpeg-dev libpng12-dev libtiff5-dev libjasper-dev libdc1394-22-dev libxine2-dev libv4l-dev libgphoto2-dev libtesseract-dev libleptonica-dev libwebp-dev libprotobuf-dev libavcodec-dev libavformat-dev libswscale-dev libxvidcore-dev libx264-dev libopenblas-dev
wget https://github.com/opencv/opencv_contrib/archive/3.2.0.tar.gz -O opencv_contrib.tar.gz
wget https://github.com/opencv/opencv/archive/3.2.0.tar.gz -O opencv.tar.gz
# wget https://github.com/opencv/opencv_extra/archive/3.2.0.tar.gz -O opencv_extra.tar.gz
```
##### [TBB](https://www.threadingbuildingblocks.org/download) 编译安装
```bash
wget https://github.com/01org/tbb/archive/2017_U7.zip -O tbb.zip
unzip -q tbb.zip
cd tbb-2017_U7/
make -j2
cd build/*_release/
sudo cp lib*.so* /usr/local/lib/
sudo ldconfig
cd ../../include/
sudo cp -r serial /usr/local/include/
sudo cp -r tbb /usr/local/include/
# 测试
cd ../examples/pipeline/square/
make
```
##### [sfm模块](http://docs.opencv.org/3.1.0/db/db8/tutorial_sfm_installation.html)
```
sudo apt-get install libeigen3-dev libsuitesparse-dev
git clone https://ceres-solver.googlesource.com/ceres-solver
cd ceres-solver
mkdir build && cd build
cmake ..
make -j4
make test
sudo make install
```
#### 编译opencv3
```bash
mkdir opencv320_build
cd opencv320_build
cmake -DCMAKE_BUILD_TYPE=RELEASE \
	-DCMAKE_INSTALL_PREFIX=/usr/local \
	-DOPENCV_EXTRA_MODULES_PATH=../opencv_contrib/modules \
	-DWITH_XIMEA=ON -DWITH_OPENCL_SVM=ON -DWITH_OPENMP=ON -DWITH_UNICAP=ON -DWITH_TBB=ON -DWITH_XINE=ON -DBUILD_WITH_DYNAMIC_IPP=ON -DWITH_OPENGL=ON -DWITH_LIBV4L=ON \
	-DBUILD_opencv_world=OFF -DBUILD_opencv_contrib_world=ON \
	-DBUILD_SHARED_LIBS=ON -DBUILD_JAVA_SUPPORT=ON -DBUILD_EXAMPLES=ON -DBUILD_TESTS=ON -DBUILD_PERF_TESTS=ON -DWITH_CUDA=OFF \
	-DPYTHON2_EXECUTABLE=/usr/local/conda/bin/python2 -DPYTHON2_INCLUDE_DIR=/usr/local/conda/include/python2.7 -DPYTHON2_INCLUDE_DIR2=/usr/include/arm-linux-gnueabihf/python2.7 -DPYTHON2_LIBRARY=/usr/local/conda/lib/libpython2.7.so -DPYTHON2_NUMPY_INCLUDE_DIRS=/usr/local/conda/lib/python2.7/site-packages/numpy/core/include \
	../opencv
```

### opencv2
```bash
wget https://github.com/opencv/opencv/archive/2.4.13.3.tar.gz -O opencv.tar.gz
```

### caffe
```bash
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install --no-install-recommends libboost-all-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
conda install h5py networkx nose
pip install scikit-image matplotlib leveldb python-dateutil protobuf python-gflags pydot
cp Makefile.config.example Makefile.config
nano Makefile.config
'''
CPU_ONLY := 1
OPENCV_VERSION := 3
# PYTHON_INCLUDE := /usr/include/python2.7 \
#               /usr/lib/python2.7/dist-packages/numpy/core/include
PYTHON_INCLUDE := $(ANACONDA_HOME)/include \
                  $(ANACONDA_HOME)/include/python2.7 \
                  $(ANACONDA_HOME)/lib/python2.7/site-packages/numpy/core/include
# PYTHON_LIB := /usr/lib
PYTHON_LIB := $(ANACONDA_HOME)/lib
INCLUDE_DIRS := $(PYTHON_INCLUDE) /usr/local/include /usr/include /usr/include/arm-linux-gnueabihf
LIBRARY_DIRS := $(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/arm-linux-gnueabihf
'''
make all -j4
make test -j4
export LD_LIBRARY_PATH="/usr/local/lib:/usr/local/conda/lib"
make runtest
make pycaffe
make pytest
export PYTHOHPATH="
```

### caffe2
```bash
sudo apt-get install -y --no-install-recommends libgtest-dev openmpi-bin openmpi-doc python-pydot
sudo apt-get install cmake libgflags-dev libgoogle-glog-dev libprotobuf-dev libpython-dev python-pip python-numpy protobuf-compiler python-protobuf
conda install flask tornado
pip install future graphviz hypothesis jupyter pydot python-nvd3 pybind11
cd caffe2
nano caffe2/core/utils_test.cc
'''
# 删除函数DataRegistryMutex()和Data()
'''
nano caffe2/core/utils_test.h
'''
class VectorDB : public db::DB {
 public:
  VectorDB(const string& source, db::Mode mode)
      : DB(source, mode), name_(source) {}
  ~VectorDB() {
    Data().erase(name_);
  }
  void Close() override {}
  std::unique_ptr<db::Cursor> NewCursor() override {
    return make_unique<VectorCursor>(getData());
  }
  std::unique_ptr<db::Transaction> NewTransaction() override {
    CAFFE_THROW("Not implemented");
  }
  static void registerData(const string& name, StringMap&& data) {
    std::lock_guard<std::mutex> guard(DataRegistryMutex());
    Data()[name] = std::move(data);
  }
+  static std::mutex& DataRegistryMutex() {
+    static std::mutex dataRegistryMutexVar;
+    return dataRegistryMutexVar;
+  }
+  static std::map<string, StringMap>& Data() {
+    static std::map<string, StringMap> dataVar;
+    return dataVar;
+  }
 private:
  StringMap* getData() {
    auto it = Data().find(name_);
    CAFFE_ENFORCE(it != Data().end(), "Can not find ", name_);
    return &(it->second);
  }

 private:
  string name_;
- std::mutex& DataRegistryMutex();
- std::map<string, StringMap>& Data();
};
}
#endif // CAFFE2_CORE_TEST_UTILS_H_
'''
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE="Release" -DCMAKE_INSTALL_PREFIX:PATH="/usr/local" -DCMAKE_VERBOSE_MAKEFILE=1 -DCAFFE2_CPU_FLAGS="-mfpu=neon -mfloat-abi=hard" -DBENCHMARK_ENABLE_EXCEPTIONS=OFF -DUSE_NNPACK=OFF -DBENCHMARK_ENABLE_TESTING=OFF -DUSE_ROCKSDB=OFF -DUSE_CUDA=OFF -DUSE_NCCL=OFF ..
export LD_LIBRARY_PATH="/usr/local/lib"
make -j4
```
