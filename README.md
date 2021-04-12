### COMMONAPI SOMEIP DEMO

可作为CommonAPI SomeIP的使用示例

#### Dependencies

安装依赖：

```bash
sudo apt-get install cmake cmake-qt-gui libexpat-dev expat default-jre
```

编译和安装 boost：

```bash
cd boost_1_57_0
./bootstrap.sh
./b2 link=shared
sudo ./b2 install
```

编译和安装 vsomeip：

```bash
git clone https://github.com/GENIVI/vsomeip.git
cd vsomeip
mkdir build
cd build
cmake -DENABLE_SIGNAL_HANDLING=1 -DDIAGNOSIS_ADDRESS=0x10 ..
make
sudo make install
```

编译和安装 CommonAPI Core Runtime：

```bash
git clone https://github.com/GENIVI/capicxx-core-runtime.git
cd capicxx-core-runtime/
mkdir build
cd build
cmake ..
make
sudo make install
```

编译和安装 CommonAPI SomeIP Runtime：

```bash
git clone https://github.com/GENIVI/capicxx-someip-runtime.git
cd capicxx-someip-runtime/
mkdir build
cd build
cmake -DUSE_INSTALLED_COMMONAPI=OFF ..
make
sudo make install
```

切换JAVA版本到Oracle jdk1.8：

```bash
sudo update-alternatives --install /usr/bin/java java /opt/jdk1.8.0_281/bin/java 700
sudo update-alternatives --install /usr/bin/javac javac /opt/jdk1.8.0_281/bin/javac 700
sudo update-alternatives --install /usr/bin/jar jar /opt/jdk1.8.0_281/bin/jar 700
sudo update-alternatives --config java # 选择jdk1.8
java -version # 检查是否配置成功
```

编译CommonAPI Core Runtime代码生成工具：

```bash
git clone https://github.com/GENIVI/capicxx-core-tools.git
cd capicxx-core-tools/org.genivi.commonapi.core.releng
mvn -Dtarget.id=org.genivi.commonapi.core.target clean verify
```

解压得到代码生成工具：

```bash
cd someip_dev
unzip -d ./commonapi_core_generator ./capicxx-core-tools/org.genivi.commonapi.core.cli.product/target/products/commonapi_core_generator.zip
chmod +x ./commonapi_core_generator/commonapi-core-generator-linux-x86_64
```

编译CommonAPI SomeIP Runtime代码生成工具：

```bash
git clone https://github.com/GENIVI/capicxx-someip-tools.git
cd capicxx-someip-tools/org.genivi.commonapi.someip.releng/
mvn -DCOREPATH=/home/lxl/Develop/capicxx-core-tools -Dtarget.id=org.genivi.commonapi.someip.target clean verify
```

解压得到代码生成工具：

```bash
org.genivi.commonapi.someip.cli.product/target/products/commonapi_someip_generator.zip
cd someip_dev
unzip -d ./commonapi_someip_generator ./capicxx-someip-tools/org.genivi.commonapi.someip.cli.product/target/products/commonapi_someip_generator.zip
chmod +x ./commonapi_someip_generator/commonapi-someip-generator-linux-x86_64
```

#### Compilation

创建工程目录：

```bash
cd someip_dev
mkdir commonapi_someip_demo
```

生成代码：

```bash
./../commonapi_core_generator/commonapi-core-generator-linux-x86_64 -sk ./fidl/HelloWorld.fidl
./../commonapi_someip_generator/commonapi-someip-generator-linux-x86_64 ./fidl/HelloWorld.fdepl
```

编译工程：

```bash
mkdir build
cd build
cmake ..
make
```

