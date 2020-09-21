# 安装

下载安装包，解压至任意目录。

添加JAVA_HOME配置

添加PATH配置

# 设置

## 使用阿里镜像

## 修改本地仓库存储路径

## 修改IDEA关联maven

# 遗留问题

1. dependency未设置scope和version时，默认值怎么处理？
2. pom.xml中的引用是怎么处理的，spring boot可以在pom.xml中获取到properties中设置的值

# Q&A

1. IDEA启动不了，但是编译成jar包之后可以启动

   勾选选项 Delegate IDE build/run actions to Maven

   ![image-20200617151713272](maven学习笔记.assets/image-20200617151713272.png)

2. 收单

# 常用命令

## 清理maven仓库

进入仓库根目录后，执行

```bash
for /r %i in (*.lastUpdated) do del %i
```

