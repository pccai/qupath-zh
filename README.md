# qupath-zh
qupath中文汉化安装包

QuPath 0.7.0 中文补丁（Windows 64 位）

此 ZIP 可以直接分发给已经安装 QuPath 0.7.0 的用户。
增强版包含中文资源、界面适配层、缺失的 Java 语言模块、安装和恢复脚本。
不需要重新下载安装 QuPath，也不需要额外安装 Java、Python 或联网下载。
包含 894 项核心词条、333 项补充词条、235 项组件词条，共 1462 项。
新增覆盖标注/分类面板、项目面板、图像信息、工作流、常用右键菜单、
扩展管理器、DJL 引擎管理和界面导览等。详见“补译说明.txt”。
这不是全软件 100% 汉化：部分深层分析窗口、脚本编辑器和其他扩展仍有英文。

安装方法
1. 关闭 QuPath。
2. 将整个 ZIP 解压到普通文件夹，保留所有子目录。不要在压缩包里直接运行。
3. 双击 Install-Chinese.cmd。
4. 脚本会查找 QuPath 0.7.0；未找到时选择安装文件夹。
   该文件夹应同时包含 app、runtime 和 QuPath-0.7.0.exe。
5. 出现“安装成功”后，正常启动原来的 QuPath 快捷方式即可。

也可以把 QuPath 安装文件夹拖到 Install-Chinese.cmd 上。
如果 QuPath 装在 Program Files 等系统目录下，可能需要右键“以管理员身份运行”。
请使用实际运行 QuPath 的同一 Windows 账户执行，以便写入该用户的语言偏好。
其他 Windows 账户需要分别启用界面语言。

恢复方法
1. 关闭 QuPath。
2. 双击本包中的 Restore-Chinese.cmd，选择同一个安装目录。
3. 脚本会还原安装本补丁之前的文件和当前用户的界面语言设置。
   对原先为英文的安装，恢复后即为英文；对原先已有中文修改的安装，恢复为修改前状态。

自动备份存放在 QuPath 安装目录的 chinese-patch-backups 子目录中。
请保留该备份和本补丁包，以便恢复。同版本重复安装会重新校验，不会覆盖首次备份。
可直接升级上一版中文补丁；升级时保留备份链，一次恢复可返回首次安装前的状态。
安装失败时，脚本会自动恢复已修改的文件和界面语言偏好。

适用范围
- 官方 QuPath 0.7.0 Windows x64 安装包。
- 原安装使用裁剪后的 Eclipse Adoptium 25.0.2+10 运行时，缺少中文语言数据时，
  补丁会补充相同版本的 jdk.localedata 模块。
- 如果运行时本身已经支持中文，则直接使用已有支持。
- 自定义 Java 版本、已有自定义模块路径、macOS/Linux 或其他 QuPath 版本不保证适用。
  不兼容时脚本会终止，且不会保留本次安装产生的配置改动。

实际会修改哪些内容
1. 安装核心中文资源：
   app\localization\qupath-gui-strings_zh.properties
   同时安装补充词条和组件词条：
   app\localization\qupath-supplemental.properties
   app\localization\qupath-supplemental_zh.properties
   app\localization\qupath\ext\extensionmanager\strings_zh.properties
   app\localization\qupath\ext\djl\ui\strings_zh.properties
   app\localization\qupath\ext\training\ui\strings_zh.properties
   app\localization\qupath\ext\training\ui\tour_zh.properties
   app\localization\qupath\fx\localization\strings_zh.properties
2. 缺少语言模块时安装：
   app\locale-modules\jdk.localedata.jar
   app\locale-modules\source.json
3. 在以下两份原配置中添加语言目录和界面适配层的 classpath：
   app\QuPath-0.7.0.cfg
   app\QuPath-0.7.0 (console).cfg
   在 [Application] 段最前面加入：
   app.classpath=$APPDIR\localization
   app.classpath=$APPDIR\qupath-chinese-ui-0.7.0.jar
   缺少语言模块时，在 [JavaOptions] 段追加：
   java-options=--module-path=$APPDIR\locale-modules
   java-options=--add-modules=jdk.localedata
4. 设置当前用户的界面语言：
   HKCU\Software\JavaSoft\Prefs\io.github.qupath\0.7
   值名 locale/Display，Windows Java Preferences 编码后的值为 /Chinese。
5. 如果用户目录已经存在另一个同名中文资源文件，会先备份再更新它，
   避免旧翻译优先于安装目录的资源被加载。
6. 许可证放入安装目录的 chinese-patch-licenses 子目录。
7. 增加界面适配层 app\qupath-chinese-ui-0.7.0.jar。
   原来的 QuPath 和扩展 JAR 不会被覆盖；安装前会校验适配的 5 个原始程序库。

脚本按原配置追加参数，保留内存设置等已有选项。
只修改界面语言偏好，不导入提供者的注册表、用户路径、工程记录或其他偏好。
工程和图像数据不在补丁操作范围中。
默认分类在列表中显示中文，但 Tumor、Stroma、Positive 等内部名称保持原值；
原有脚本、分类器和工程仍使用这些名称。自定义名称及原始测量字段不会被改写。
切换语言或编辑词条后请重启 QuPath。

命令行用法
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Install-Chinese.ps1 -QuPathDirectory "D:\Apps\QuPath-0.7.0"
powershell.exe -NoProfile -ExecutionPolicy Bypass -File .\Restore-Chinese.ps1 -QuPathDirectory "D:\Apps\QuPath-0.7.0"

来源
中文项目：https://github.com/zindy/qupath-localization
资源提交：4562b7471a6419628f32e7f06ae8a180dc302dd7
Java 模块来源及许可证：参见 THIRD-PARTY.txt、licenses 和 payload 中的 source.json。
