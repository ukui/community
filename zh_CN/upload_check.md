每个软件包在release前，需要做好以下检查：
1. 纯净环境下是否编译通过:
[pbuilder](pbuilder.md) 或者 sbuild

2. lintian检查是否通过: 
* 自动调用
可以在pdebuild、debuild时自动调用，详见相关说明文件。

* 手动处理
debuild、pdebuild编包后会生成foo-version.amd64.changes文件,对其运行lintian, 解决全部报告的"E"和"W",其他的标注尽量解决(常见lintian错误和解决方法):
```
lintian -i -EvIL +pedantic --verbose foo-version.amd64.changes
```

3. copyright是否正确以及匹配
* .cpp .c .h .py等代码文件，需要在文件头部有相应的copyright声明
* 每个文件的copyright与debian/copyright要一一对应
* 可以通过`licensecheck -r .`查看头部有声明的文件的copyright.
