Agollo - Go Client for Apollo
================

[![golang](https://img.shields.io/badge/Language-Go-green.svg?style=flat)](https://golang.org)
[![Build Status](https://github.com/qshuai/agollo/actions/workflows/go.yml/badge.svg)](https://github.com/qshuai/agollo/actions/workflows/go.yml)
[![Go Report Card](https://goreportcard.com/badge/github.com/qshuai/agollo)](https://goreportcard.com/report/github.com/qshuai/agollo)
[![codebeat badge](https://codebeat.co/badges/bc2009d6-84f1-4f11-803e-fc571a12a1c0)](https://codebeat.co/projects/github-com-apolloconfig-agollo-master)
[![Coverage Status](https://coveralls.io/repos/github/qshuai/agollo/badge.svg?branch=master)](https://coveralls.io/github/qshuai/agollo?branch=master)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![GoDoc](http://godoc.org/github.com/qshuai/agollo?status.svg)](http://godoc.org/github.com/qshuai/agollo)
[![GitHub release](https://img.shields.io/github/release/qshuai/agollo.svg)](https://github.com/qshuai/agollo/releases)
[![996.icu](https://img.shields.io/badge/link-996.icu-red.svg)](https://996.icu)

方便Golang接入配置中心框架 [Apollo](https://github.com/ctripcorp/apollo) 所开发的Golang版本客户端。

# Features

* 支持多 IP、AppID、namespace
* 实时同步配置
* 灰度配置
* 延迟加载（运行时）namespace
* 客户端，配置文件容灾
* 自定义日志，缓存组件
* 支持配置访问秘钥

# Usage

## 快速入门

### 导入 agollo

```
go get -u github.com/qshuai/agollo/v4@latest
```

### 启动 agollo

```
package main

import (
	"fmt"
	"github.com/apolloconfig/agollo/v4"
	"github.com/apolloconfig/agollo/v4/env/config"
)

func main() {
	c := &config.AppConfig{
		AppID:          "testApplication_yang",
		Cluster:        "dev",
		IP:             "http://106.54.227.205:8080",
		NamespaceName:  "dubbo",
		IsBackupConfig: true,
		Secret:         "6ce3ff7e96a24335a9634fe9abca6d51",
	}

	client, _ := agollo.StartWithConfig(func() (*config.AppConfig, error) {
		return c, nil
	})
	fmt.Println("初始化Apollo配置成功")

	//Use your apollo key to test
	cache := client.GetConfigCache(c.NamespaceName)
	value, _ := cache.Get("key")
	fmt.Println(value)
}
```

### 动态namespace特性
支持使用单个或者多个正则表达式来表示namespace的名称，实现在程序运行中读取新创建namespace的配置信息；增加的新配置项如下所示：
```
func main() {
	c := &config.AppConfig{
		AppID:          "testApplication_yang",
		Cluster:        "dev",
		IP:             "http://106.54.227.205:8080",
		IsBackupConfig: true,
		Secret:         "6ce3ff7e96a24335a9634fe9abca6d51",
		NamespaceName:  ".*\\.yaml",
		Dynamic                 bool          `json:"dynamic"`                 // 是否开启正则表达式形式的namespace配置
		SyncNamespaceInterval   time.Duration `json:"sync_namespace_interval"` // 拉取最新namespace的时间间隔，最小1s
		AuthorizationToken      string        `json:"authorization_token"`     // 自定义的认证（如果没有特殊授权需求，则不用设置）
	}

	...
```

## 更多用法

***使用Demo*** ：[agollo_demo](https://github.com/zouyx/agollo_demo)

***其他语言*** ： [agollo-agent](https://github.com/zouyx/agollo-agent.git) 做本地agent接入，如：PHP

欢迎查阅 [Wiki](https://github.com/apolloconfig/agollo/wiki) 或者 [godoc](http://godoc.org/github.com/zouyx/agollo) 获取更多有用的信息

如果你觉得该工具还不错或者有问题，一定要让我知道，可以发邮件或者[留言](https://github.com/apolloconfig/agollo/issues)。

# User

* [使用者名单](https://github.com/apolloconfig/agollo/issues/20)

# Contribution

* Source Code: https://github.com/apolloconfig/agollo/
* Issue Tracker: https://github.com/apolloconfig/agollo/issues

# License

The project is licensed under the [Apache 2 license](https://github.com/apolloconfig/agollo/blob/master/LICENSE).

# Reference

Apollo : https://github.com/ctripcorp/apollo
