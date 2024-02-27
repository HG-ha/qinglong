### 打包好的QEMU虚拟机环境（alpine-virt-3.18.0-x86_64.iso）

1. qinglong.qcow2
   基础的虚拟机环境，可以拿去自由开发或使用
   下载：[https://pan.wer.plus/f/aPf2/qinglong.qcow2](https://pan.wer.plus/f/aPf2/qinglong.qcow2)

2. qinglong.qcow2-hj
   安装了nodejs18，npm，python3和c++，make的开发环境，可以直接在此环境上安装青龙面板。
   
   下载：[https://pan.wer.plus/f/wmFy/qinglong.qcow2-hj](https://pan.wer.plus/f/wmFy/qinglong.qcow2-hj)

3. qinglong.qcow2-wz
   直接在虚拟机内构建的完整青龙面板，不要执行除了./start.sh以外的任何与面板有关的命令，否则会导致环境失效
   
   下载：[https://pan.wer.plus/f/KQco/qinglong.qcow2-wz](https://pan.wer.plus/f/KQco/qinglong.qcow2-wz)

4. qinglong.qcow2-docker-wz
   在虚拟机内使用docker构建的青龙面板，性能损耗极高，但运行十分稳定
   
   下载：[https://pan.wer.plus/f/Qauq/qinglong.qcow2-docker-wz](https://pan.wer.plus/f/Qauq/qinglong.qcow2-docker-wz)

5. qinglong.qcow2-wm
   直接在虚拟机内构建的完整青龙面板，相较于（3），整体环境更小，稳定性高，性能更高，非常推荐直接使用此版本
   
   下载：[https://pan.wer.plus/f/Jwsv/qinglong.qcow2-wm](https://pan.wer.plus/f/Jwsv/qinglong.qcow2-wm)

6. 度盘下载
   
   1. [百度网盘 p194](https://pan.baidu.com/s/1fzpT08VzZQG5Lx7rPzzYDQ?pwd=p194)

### 在Termux中运行青龙面板（使用qinglong.qcow2-wm）

1. 安装qemu-system-x86_64
   
   ```shell
   pkg install qemu-system-x86-64-headless wget -y
   ```

2. 下载虚拟机文件（失效从网盘下载）
   
   ```shell
   wget https://pan.wer.plus/f/Jwsv/qinglong.qcow2-wm
   ```

3. 启动虚拟机（分配2核CPU，1G RAM）
   
   ```shell
   # 可以将此写入脚本，方便一键启用
   qemu-system-x86_64 -smp 2 -m 1G \
     -drive file=qinglong.qcow2-wm,if=virtio \
     -netdev user,id=n1,hostfwd=tcp::5700-:5700 \
     -device virtio-net,netdev=n1 \
     -nographic
   ```

4. 启动青龙面板
   
   ```shell
   # 首次启动
   python -m venv myvenv
   echo 'source myvenv/bin/activate' >> /etc/profile
   source myvenv/bin/activate
   qinglong
   
   # 再次登录后可以直接启动
   qinglong
   ```

5. 关闭虚拟机
   
   ```shell
   poweroff
   ```
   
   

### 常见问题及解决方案

1. 依赖安装慢
   
   1. 如果使用的是高性能版本，那么可以考虑解决一下软件包管理器源的问题
      在更换软件包源之后，建议重启一下虚拟机
      
      1. pip
         pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
      
      2. pnpm
         pnpm config set registry https://registry.npm.taobao.org/
      
      3. npm
         npm config set registry https://registry.npm.taobao.org/
   
   2. 直接在虚拟机内运行一键安装脚本
      项目地址：[FlechazoPh/QLDependency: 青龙面板全依赖一键安装脚本 / Qinglong Pannel Dependency Install Scripts. (github.com)](https://github.com/FlechazoPh/QLDependency)
      
      ```shell
      curl -fsSL https://ghproxy.com/https://raw.githubusercontent.com/FlechazoPh/QLDependency/main/Shell/QLOneKeyDependency.sh | sh
      ```

2. 虚拟机中自行安装青龙面板或升级青龙面板过程中可能遇到的问题
   
   1. 运行命令qinglong，提示（Run “pnpm setup“ to create it automatically）
      解决方法
      
      ```shell
      pnpm setup
      source ~/.bashrc
      
      # 将.bashrc文件中 以# pnpm开头和结尾的中间行写入到/etc/profile
      sed -n '/# pnpm/,/# pnpm end/p' >> /etc/profile
      ```
   
   2. 运行命令qinglong，脚本停止在update_depend这行
      解决方法
      
      ```shell
      # 注释掉start.sh脚本中这一行
      # 通常在$QL_DIR/shell/start.sh路径
      
      sed -i 's/^update_depend//g' $QL_DIR/shell/start.sh
      ```
3. 校准系统时间
     ``` shell
     apk add openntpd
     rc-update add openntpd default
     /etc/init.d/openntpd start
     ```
      
      

### 请我喝杯水

| 支付宝                                                                                     | 微信                                                                                    | 群                |
| --------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- | ---------------- |
| <img src="https://github.com/HG-ha/qinglong/blob/main/zfb.jpg?raw=true" title="" alt="zfb" width="120px" height="120px"> | <img title="" src="https://github.com/HG-ha/qinglong/blob/main/wx.png?raw=true" alt="wx" width="120px" height="120px"> | 一铭API：1029212047 |
|                                                                                       |                                                                                       | 镜芯科技：376957298   |



### 其他项目

[一铭API](https://api.wer.plus)
