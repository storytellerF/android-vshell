# 修改

增加了读取json 文件设置端口转发的功能

json 路径为`/sdcard/vshell_cfg.json`。不用自己创建，一次运行之后便会生成此文件。

json 格式为

```json
{
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
