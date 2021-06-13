# 修改

增加了读取json 文件设置端口转发的功能

json 路径为`/sdcard/vshell_cfg.json`。不用自己创建，一次运行之后便会生成此文件。

json 格式为

```json
{
    "webPort": 1080,
    "cdrom": "/storage/emulated/0/operating-system.iso",
    "hddName": "/storage/emulated/0/userdata.qcow2",
    "hostFWDS":[
        {
            "to":1080,
            "from":80 
        }, 
        {
            "to":10022,
            "from":22 
       }
    ]
}
```

读取Alpine Term 的qcow2 文件，需要手动下载
`https://github.com/xeffyr/alpine-term/releases/download/v16.0/alpine-term-hdd-image-2020.01.28.qcow2`，保存到指定位置（由上面的json文件指定）
（这个文件522m），如果不指定（手动移除），还是回去这里找`/storage/emulated/0/userdata.qcow2`。
如果没有这个文件，会直接退出。

因为我是Windows平台，所以直接使用了原作者的动态链接库

原[README](README_O.md)

## 使用方法

初次进入系统为live cd模式，需要输入`setup-alpine` 进行安装，具体安装教程自行搜索

安装之后，关闭此软件（彻底关闭，通知栏不能有通知），然后再次打开，会询问是否安装成功，选择OK 即可进入系统。以后再次打开不会再次询问是否安装成功。

## 关于qcow2

这是qemu要使用的文件，可以通过`qemu-img` 创建。[alpine qemu](https://wiki.alpinelinux.org/wiki/Qemu)

```shell script
qemu-img create -f qcow2 alpine.qcow2 8G
```

大小最好1G 以上

## 关于iso

这是系统镜像文件,可以到这里下载[download](https://www.alpinelinux.org/downloads/)
