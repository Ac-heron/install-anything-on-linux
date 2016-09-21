
在linux上安装nodejs

# 在linux上安装node.js

   - 官网： https://nodejs.org

   - 下载：wget https://nodejs.org/dist/v4.4.5/node-v4.4.5-linux-x64.tar.xz

   - 解压：

      ```
      xz -d node-v4.4.5-linux-x64.tar.xz
      tar -xvf node-v4.4.5-linux-x64.tar
      ```
   
   - 配置环境变量：vim /etc/profile

      ```
      export NODEHOME=/usr/program/node-v4.4.5 
      export PATH=$PATH:$NODEHOME/bin 
      export NODEPATH=$NODEHOME/lib/node_modules
      ```

    NODE_HOME设置成你自己的路径，后面两个export直接copy就行了


   - 生效： source /etc/profile

   - 查看是否安装： node -v

