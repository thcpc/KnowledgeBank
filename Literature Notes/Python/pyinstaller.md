---
tags:
  - python
  - programming
description: 如何用pyinstaller打包
---
## 1.生成规范文件
```py
pyi-makespec main.py
```

## 2.含义


```python

block_cipher = None  #设置 加密，需要安装tinyaes第三方库，最多16位字符，此处在使用--key= 会有变化


a = Analysis(
    ['main.py'],  # 运行的所有py文件，包括依赖的py文件, 因为加载文件的时候会执行，所以除了主程序，其它程序不要有__main__
    pathex=[],  # 搜索导入的路径列表（此列表为项目绝对路径)，包括选项给出的路径--paths，项目需要从什么地方导入自定义库
    binaries=[],  # 脚本需要的非python模块，包括--add-binary选项给出的名称，二进制数据
    datas=[],  # 应用程序中包含的非二进制文件，包括--add-data选项给出的名称，项目需要用到什么数据，比如图片，视频等。里面格式为tuple，第一个参数是文件路径，第二个是打包后所在的路径，其为一个元组：('image/*.png','data/image')
    hiddenimports=[],   # 假如打包后打开exe显示module not found，就要把该库添加到hiddenimports里面了，主要是针对动态加载的类
    hookspath=[],  
    hooksconfig={},  # 挂钩配置选项由一个字典组成
    runtime_hooks=[],  
    excludes=[],  # 假如你用的python有很多库，但是你不需要用到某个，那么就把它添加到里面去，可以压缩文件大小
    win_no_prefer_redirects=False,
    win_private_assemblies=False,
    cipher=block_cipher,
    noarchive=False,
)
pyz = PYZ(a.pure, a.zipped_data, cipher=block_cipher)

exe = EXE(
    pyz,
    a.scripts,  # 打包成EXE的脚本文件
    # a.binaries,  # 如果是单文件模式，则需要添加；多文件也可以添加
   	# a.zipfiles,
    # a.datas,
    [],
    exclude_binaries=True,  # 是否排除二进制文件，为True时，为排除二进制的文件，当文件交大时包含二进制文件运行较快，如果是单文件，则没有这个选项
    name='main',  # 打包程序的名字
    debug=False,  # 是否启用调试功能
    bootloader_ignore_signals=False,
    # runtime_tmpdir=None,  # 生成单文件时需要这个参数，定义运行时的临时文件夹
    strip=False,
    upx=True,  # 打包的时候进行压缩，False表示不压缩；要用到一个压缩程序UPX，用于压缩文件，需要单独下载
    console=True,  # 打包后的可执行文件双击运行时屏幕会出现一个cmd窗口，不影响原程序运行，等于是是否加-w参数
    disable_windowed_traceback=False,  
    argv_emulation=False,
    target_arch=None,
    codesign_identity=None,
    entitlements_file=None,
    
    """添加选项，初始化时没有的"""
    icon="",  # 指定应用程序的图标，传入路径，可以相对路径
    
)
coll = COLLECT(
    """
    如果是单文件模式，不需要这个COLLECT类，同时需要将：
        a.binaries,
    	a.zipfiles,
    	a.datas,
    这些数据文件添加到EXE中
    """
    exe,
    a.binaries,
    a.zipfiles,
    a.datas,
    strip=False,
    upx=True,
    upx_exclude=[],
    name='main',
)

```

## 3. 打包命令
```shell
pyinstaller setup.spec
pyinstaller -F setup.spec
# 该命令会在项目路径下生成一个dist文件夹，该文件夹下有一个可执行文件。其中，-F参数表示只生成一个单独的可执行文件，setup.spec是PyInstaller的打包配置文件。
```

