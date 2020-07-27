## 新包入库检查
1. 程序可以正常编译和运行
2. 配置好debian目录下各文件(Todo: #debian文件初始化配置)
3. 配置好主目录下各文件(README.md, COPYING, CHNAGELOG, AUTHORS, NEWS)
4. 给所有代码文件(.c/.cpp/.h/.py等)头部添加相应copyright头(Todo: #常见copyright头)
5. 通过命令`cme update dpkg-copyright`[<sup>1</sup>](#参考)更新debian/copyright文件，确保所有文件的copyright与debian/copyright里的条目是一一对应的
   ```
   // 主题命令的输出，有的文件可能有问题，需要手动修复，比如：
   Adding dummy global license text for license GPL for path panel/customstyle.cpp panel/customstyle.h
   Warning in 'License:GPL text': License contains copyright scanner boilerplate. Please update this field with the actual license text (this cannot be fixed with 'cme fix' command)
   Offending value: 'Please fill license GPL from header of panel/customstyle.cpp panel/customstyle.h'
   
   // 并且生成格式有问题(Copyright:后面多了一个LGPL2+)，需要手动修改下，比如：
   Files: panel/common/ukuiscreensaver.h
   Copyright: LGPL2+ / 2019-2020, UKUI team / 2016, Luís Pereira <luis.artur.pereira@gmail.com> / 2010-2011, Razor team
   License: LGPL-2.1+
   需要修改成：
   Files: panel/common/ukuiscreensaver.h
   Copyright: 2019-2020, UKUI team
              2016, Luís Pereira <luis.artur.pereira@gmail.com>
              2010-2011, Razor team
   License: LGPL-2.1+
   ```

## 每个软件包在release前，需要做好以下检查：
1. 纯净环境下是否编译通过(后续全部通过CI配置):
[pbuilder](pbuilder.md) 或者 sbuild

2. lintian检查是否通过: 
* 自动调用
可以在pdebuild、debuild时自动调用，详见相关说明文件。

* 手动处理
debuild、pdebuild编包后会生成foo-version.amd64.changes文件,对其运行lintian, 解决全部报告的"E"和"W",其他的标注尽量解决([常见lintian错误和解决方法](#常见lintian错误和解决办法)):
```
lintian -i -EvIL +pedantic --verbose foo-version.amd64.changes
```

3. copyright是否正确以及匹配
* 总体原则是“.cpp .c .h .py”等代码文件，需要在文件头部有相应的copyright声明并且与debian/copyright要一一对应
* 可以通过`cme update dpkg-copyright`更新debian/copyright文件，注意事项见#新包入库检查

## 附录
### 常见copyright头
#### GPL-2
```
/*
 * Copyright (C) 2002  KylinSoft Co., Ltd.
 * 
 * This program is free software; you can redistribute it and/or
 * modify it under the terms of the GNU General Public License
 * as published by the Free Software Foundation; either version 2
 * of the License, or (at your option) any later version.
 * 
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 * 
 * You should have received a copy of the GNU General Public License
 * along with this program; if not, write to the Free Software
 * Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.
 */
```

#### LGPL-2.1
```
 * Copyright: 2020 KylinSoft Co., Ltd.
 *
 * This program or library is free software; you can redistribute it
 * and/or modify it under the terms of the GNU Lesser General Public
 * License as published by the Free Software Foundation; either
 * version 2.1 of the License, or (at your option) any later version.
 *
 * This library is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
 * Lesser General Public License for more details.
 *
 * You should have received a copy of the GNU Lesser General
 * Public License along with this library; if not, write to the
 * Free Software Foundation, Inc., 51 Franklin Street, Fifth Floor,
 * Boston, MA 02110-1301 USA
 */
```

#### GPL-3
```
/*
 * Copyright (C) 2020, KylinSoft Co., Ltd.
 *
 * This program is free software: you can redistribute it and/or modify
 * it under the terms of the GNU General Public License as published by
 * the Free Software Foundation, either version 3 of the License, or
 * (at your option) any later version.
 *
 * This program is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
 * GNU General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this program.  If not, see <https://www.gnu.org/licenses/>.
 */
```

#### LGPL-3
```
/*
 * Copyright (C) 2020, KylinSoft Co., Ltd.
 *
 * This library is free software; you can redistribute it and/or
 * modify it under the terms of the GNU Lesser General Public
 * License as published by the Free Software Foundation; either
 * version 3 of the License, or (at your option) any later version.
 *
 * This library is distributed in the hope that it will be useful,
 * but WITHOUT ANY WARRANTY; without even the implied warranty of
 * MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the GNU
 * Lesser General Public License for more details.
 *
 * You should have received a copy of the GNU General Public License
 * along with this library.  If not, see <https://www.gnu.org/licenses/>.
 */
```

### 常见lintian错误和解决办法

#### sciprt-without-interpreter
```
E: ukui-panel: script-without-interpreter control/postinst
N: 
N:    This file starts with the #! sequence that identifies scripts, but it
N:    does not name an interpreter.
N:    
N:    Severity: error
N:    
N:    Check: scripts
```

解决方法：在control/*.postinst添加解释头，最好同时加上"set -e"
```
+ #!/bin/sh

+ set -e

glib-compile-schemas /usr/share/glib-2.0/schemas/ 
```

#### maintainer-script-lacks-debhelper-token
```
W: ukui-panel source: maintainer-script-lacks-debhelper-token debian/ukui-panel.postinst
N: 
N:    This package is built using debhelper commands that may modify
N:    maintainer scripts, but the maintainer scripts do not contain the
N:    "#DEBHELPER#" token debhelper uses to modify them.
N:    
N:    Adding the token to the scripts is recommended.
N:    
N:    Severity: warning
N:    
N:    Check: debhelper
```
解决方法：debian/ukui-panel.postinst添加#DEBHELPER#
```
#!/bin/sh

set -e

glib-compile-schemas /usr/share/glib-2.0/schemas/ 

+ #DEBHELPER#
```

#### file-without-copyright-information
```
W: ukui-panel source: file-without-copyright-information .github/workflows/build.yml
N: 
N:    The source tree contains a file which was not matched by any of the
N:    Files paragraphs in debian/copyright. Either adjust existing wildcards
N:    to match that file or add a new Files paragraph.
N:    
N:    Refer to
N:    https://www.debian.org/doc/packaging-manuals/copyright-format/1.0/ for
N:    details.
N:    
N:    Severity: warning
N:    
N:    Check: debian/copyright
```
解决方法：在debian/copyright里按此文件的开源协议，添加到对应的条目中
```
// 假如“.github/workflows/build.yml”的copyright与“panel/comm_func.*”文件一样，是“2020 KylinSoft Co., Ltd.“
Files: panel/comm_func.*
       panel/iukuipanel.*
       panel/iukuipanelplugin.h
       panel/main.cpp
       panel/panelpluginsmodel.
+      .github/workflows/build.yml
Copyright: 2020 KylinSoft Co., Ltd.
License: LGPL-2.1+
```

#### spelling-error-in-binary
```
I: ukui-panel: spelling-error-in-binary usr/bin/ukui-panel deleteed deleted
N: 
N:    Lintian found a spelling error in the given binary. Lintian has a list
N:    of common misspellings that it looks for. It does not have a dictionary
N:    like a spelling checker does.
N:    
N:    If the string containing the spelling error is translated with the help
N:    of gettext or a similar tool, please fix the error in the translations
N:    as well as the English text to avoid making the translations fuzzy. With
N:    gettext, for example, this means you should also fix the spelling
N:    mistake in the corresponding msgids in the *.po files.
N:    
N:    You can often find the word in the source code by running:
N:    
N:     grep -rw <word> <source-tree>
N:    
N:    This tag may produce false positives for words that contain non-ASCII
N:    characters due to limitations in strings.
N:    
N:    Severity: info
N:    
N:    Check: binaries
```
解决方法：有拼写错误，通过”grep -rw <word> ."搜索到错误字符后，修正
   
## 参考
- [1] [Debian Copyright Review Tools](https://wiki.debian.org/CopyrightReviewTools)
