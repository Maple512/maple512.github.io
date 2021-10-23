---
title: 模块化
date: 2021-10-20 23:34:00
tags:
  - csharp
  - framework
  - modularity
---

## 简介

> 通过使用模块系统，统一服务注册的动作。通过扫描程序集的方式，来达到自动发现并注册服务的目的

## 服务注册

> 将服务注册到容器中，将服务的创建动作交给容器，从而实现控制反转

### 生命周期

- `Singleton`：单例模式。每次容器中获取到的实例都是相同的
- `Scoped`：范围模式。每次同一个容器中获取到的实例是相同的
- `Transient`：瞬时模式。每次从容器中获取的都是新的实例

### 使用方式

#### 接口

继承以下接口

- `ISingletonService`
- `IScopedService`
- `ITransientService`

> 提示：

#### 特性

在服务上声明`ServiceDescribe`特性

```csharp
// 注册为单例
[ServiceDescribe(lifetime: ServiceLifetime.Singleton)]
public class YourService
{
  // some code
}
```

`ServiceDescribe`具体配置

- `Lifetime`：服务的生命周期
- `TryRegister`：尝试注册服务，如果已经注册过相同的服务，则放弃
- `IsDisabled`：禁止注册
- `ReplaceFirst`：替换从容器中查到的第一个相同类型的服务
- `IncludeSelf`：注册时包括服务实现类，而不只是注册接口。注册服务时，如果继承了接口，则不会注册服务实现
- `IncludeOnlySelf`：注册时，只注册服务实现类，而忽略继承的接口

## 模块系统

> 通常每个`csproj`都相当于一个模块，在项目里创建一个以`Module`为后缀的类来表示该项目是一个模块

### 如何使用

> 通过声明一个特性`ModuleDescribe`来达到使用模块系统的目的

该特性有两种使用方式

- 通过在类上声明，通常建议类使用`Module`作为后缀

```csharp YourModule.cs
[ModuleDescribe(typeof(DependedModule))]
public class YourModule
{
  // other code
}
```

- 直接在项目中的某个类中配置程序集特性

```csharp SomeClass.cs
[assembly: ModuleDescribe(typeof(DependedModule)))]
```
