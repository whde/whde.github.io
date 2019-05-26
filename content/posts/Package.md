---
typora-root-url: ../../static/
title: "Package"
date: 2017-12-14T11:18:15+08:00
keywords: ["脚本打包","iOS"]
description: "Package"
tags: ["Package", "iOS"]
categories: ["iOS"]
author: "Whde"
---

# iOS脚本打包



### 使用方法

**1 : 项目主目录下创建 Package 文件夹, 将Package_test.sh DevelopmentExportOptionsPlist.plist放进文件下**
**2 : 打开Package_test.sh文件,修改 "项目自定义部分" 配置好项目参数**
**3 : 打开DevelopmentExportOptionsPlist.plist, 配置provisioningProfiles字段**
**4 : 打开终端, cd到Package文件夹**
**5 : 输入 sh Package_test.sh 命令,回车,开始执行此打包脚本**

### DevelopmentExportOptionsPlist.plist

文件下载地址：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>compileBitcode</key>
	<false/>
	<key>method</key>
	<string>development</string>
	<key>provisioningProfiles</key>
	<dict>
		<key>com.Whde.WhdeProjectName</key>
		<string>Wildcard</string>
	</dict>
	<key>signingCertificate</key>
	<string>iPhone Developer</string>
	<key>signingStyle</key>
	<string>manual</string>
	<key>stripSwiftSymbols</key>
	<true/>
	<key>teamID</key>
	<string>BVU65MZFLK</string>
	<key>thinning</key>
	<string>&lt;none&gt;</string>
</dict>
</plist>

```



### Package_test.sh

文件下载地址：

```shell
# !/bin/bash

# 使用方法:
# 1 : 项目主目录下创建 Package 文件夹, 将Package_test.sh DevelopmentExportOptionsPlist.plist放进文件下
# 2 : 打开Package_test.sh文件,修改 "项目自定义部分" 配置好项目参数
# 3 : 打开DevelopmentExportOptionsPlist.plist, 配置provisioningProfiles字段
# 4 : 打开终端, cd到Package文件夹
# 5 : 输入 sh Package_test.sh 命令,回车,开始执行此打包脚本

# ===============================项目自定义部分(自定义好下列参数后再执行该脚本)============================= #
# 计时
SECONDS=0
# 是否编译工作空间 (例:若是用Cocopods管理的.xcworkspace项目,赋值true;用Xcode默认创建的.xcodeproj,赋值false)
is_workspace="true"
# 指定项目的scheme名称
scheme_name="WhdeProjectName"
# 工程中Target对应的配置plist文件名称, Xcode默认的配置文件为Info.plist
info_plist_name="Info"
# 指定要打包编译的方式 : Release,Debug...
build_configuration="Release"


# ===============================自动打包部分(无特殊情况不用修改)============================= #
# 导出ipa所需要的plist文件路径 
ExportOptionsPlistPath="./Package/DevelopmentExportOptionsPlist.plist"
# 返回上一级目录,进入项目工程目录
cd ..
# 获取项目名称
project_name=`find . -name *.xcodeproj | awk -F "[/.]" '{print $(NF-1)}'`
# 获取版本号,内部版本号,bundleID
info_plist_path="$project_name/Default/$info_plist_name.plist"
/usr/libexec/PlistBuddy -c "Set :CFBundleIdentifier com.Whde.WhdeProjectName" "$info_plist_path"
buildNumber=$(/usr/libexec/PlistBuddy -c "Print CFBundleVersion" "$info_plist_path")
buildNumber=$(($buildNumber + 1))
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion $buildNumber" "$info_plist_path"
/usr/libexec/PlistBuddy -c "Set :CFBundleDisplayName Test($buildNumber)" "$info_plist_path"
bundle_version=`/usr/libexec/PlistBuddy -c "Print CFBundleShortVersionString" $info_plist_path`
bundle_build_version=`/usr/libexec/PlistBuddy -c "Print CFBundleVersion" $info_plist_path`
bundle_identifier=`/usr/libexec/PlistBuddy -c "Print CFBundleIdentifier" $info_plist_path`

# 删除旧.xcarchive文件
rm -rf ./Package/$scheme_name.xcarchive
rm -rf ./Package/$scheme_name.ipa
# 指定输出ipa路径
export_path=./Package
# 指定输出归档文件地址
export_archive_path="$export_path/$scheme_name.xcarchive"
# 指定输出ipa地址
export_ipa_path="$export_path"
# 指定输出ipa名称 : scheme_name + bundle_version
ipa_name="$scheme_name""_V$bundle_version($bundle_build_version)"


echo "\033[32m*************************  开始构建项目  *************************  \033[0m"
# 指定输出文件目录不存在则创建
if [ -d "$export_path" ] ; then
echo $export_path
else
mkdir -pv $export_path
fi

# 判断编译的项目类型是workspace还是project
if $is_workspace ; then
# 编译前清理工程
xcodebuild clean -workspace ${project_name}.xcworkspace \
                 -scheme ${scheme_name} \
                 -configuration ${build_configuration}

xcodebuild archive -workspace ${project_name}.xcworkspace \
                   -scheme ${scheme_name} \
                   -configuration ${build_configuration} \
                   -archivePath ${export_archive_path}
else
# 编译前清理工程
xcodebuild clean -project ${project_name}.xcodeproj \
                 -scheme ${scheme_name} \
                 -configuration ${build_configuration}

xcodebuild archive -project ${project_name}.xcodeproj \
                   -scheme ${scheme_name} \
                   -configuration ${build_configuration} \
                   -archivePath ${export_archive_path}
fi

#  检查是否构建成功
#  xcarchive 实际是一个文件夹不是一个文件所以使用 -d 判断
if [ -d "$export_archive_path" ] ; then
echo "\033[32;1m项目构建成功 \033[0m"
else
echo "\033[31;1m项目构建失败 \033[0m"
exit 1
fi

echo "\033[32m*************************  开始导出ipa文件  *************************  \033[0m"
xcodebuild  -exportArchive \
            -archivePath ${export_archive_path} \
            -exportPath ${export_ipa_path} \
            -exportOptionsPlist ${ExportOptionsPlistPath}
# 修改ipa文件名称
mv $export_ipa_path/$scheme_name.ipa $export_ipa_path/$ipa_name.ipa

# 检查文件是否存在
if [ -f "$export_ipa_path/$ipa_name.ipa" ] ; then
echo "\033[32;1m导出 ${ipa_name}.ipa 包成功 \033[0m"
# open $export_path
else
echo "\033[31;1m导出 ${ipa_name}.ipa 包失败  \033[0m"
exit 1
fi

# 输出打包总用时
echo "\033[36;1m打包总用时: ${SECONDS}s \033[0m"
```

