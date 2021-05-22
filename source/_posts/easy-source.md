---
title: easy_source
feature: false
cover: https://ww1.sinaimg.cn/large/005ZbjyVly1gqonibddpsj30xc0hgabu.jpg
tags:
  - PHP
  - Source
categories:
  - CTF
keywords: 'CTF,PHP'
description: 全国大学生竞赛 CTF 阶段题目：php_source 的解题思路（Write_Up）
date: 2021-05-16 09:21:32
---
<meta name="referrer" content="no-referrer"/>

# easy_source

## 题目界面

![2E2143C3C748A6D925A98385EDAF8D46.jpg](https://i.loli.net/2021/05/16/EW79ZTbQqfBOzSU.jpg)

## 访问界面

![2977F1BF3A2786C2D6C49F7D5D9489DC.png](https://i.loli.net/2021/05/16/GgQxcE49HfOo3LF.png)

## 解题思路

页面中只有一句话 **你能发现我吗**。
看不到？ 隐藏文件，搜索编辑器的缓存临时文件，发现存在一个名为`.index.php.swo`的文件，文件内容如下

```php
<html><head></head><body><pre style="word-wrap: break-word; white-space: pre-wrap;">
本题目没有其他代码了噢，就只有这一个文件，虽然你看到的不完全，但是你觉得我会把 flag 藏在哪里呢，仔细想想文件里面还有什么？

<?php
class User
{
    private static $c = 0;

    function a()
    {
        return ++self::$c;
    }

    function b()
    {
        return ++self::$c;
    }

    function c()
    {
        return ++self::$c;
    }

    function d()
    {
        return ++self::$c;
    }

    function e()
    {
        return ++self::$c;
    }

    function f()
    {
        return ++self::$c;
    }

    function g()
    {
        return ++self::$c;
    }

    function h()
    {
        return ++self::$c;
    }

    function i()
    {
        return ++self::$c;
    }

    function j()
    {
        return ++self::$c;
    }

    function k()
    {
        return ++self::$c;
    }

    function l()
    {
        return ++self::$c;
    }

    function m()
    {
        return ++self::$c;
    }

    function n()
    {
        return ++self::$c;
    }

    function o()
    {
        return ++self::$c;
    }

    function p()
    {
        return ++self::$c;
    }

    function q()
    {
        return ++self::$c;
    }

    function r()
    {
        return ++self::$c;
    }

    function s()
    {
        return ++self::$c;
    }

    function t()
    {
        return ++self::$c;
    }
    
}

$rc=$_GET["rc"];
$rb=$_GET["rb"];
$ra=$_GET["ra"];
$rd=$_GET["rd"];
$method= new $rc($ra, $rb);
var_dump($method-&gt;$rd());

</pre></body></html>
```

看到提示信息：**本题目没有其他代码了噢，就只有这一个文件，虽然你看到的不完全，但是你觉得我会把 flag 藏在哪里呢，仔细想想文件里面还有什么？**
代码文件看不到的就是文件注释了。

在 PHP 中是真的存在读取文件注释（DocString，函数注释）内容的内置方法的。

文件中的
```php
$rc=$_GET["rc"];
$rb=$_GET["rb"];
$ra=$_GET["ra"];
$rd=$_GET["rd"];
$method= new $rc($ra, $rb);
var_dump($method-&gt;$rd());
```

可以构造指定的函数
```php
$d = new ReflectionMethod("User","b");
var_dump($d -> getDocComment());
```

所以构造好的 Payload 为：`?rc=ReflectionMethod&ra=User&rb=q&rd=getDocComment`

:::tips 注意

这里的 rb 参数不知道具体的参数，可以爆破 `a->t` 最后发现参数为 q 

:::

![Screenshot_2021-05-15_06-04-11.png](https://i.loli.net/2021/05/16/XRNhe8g6rxdPGoa.png)

可以获取到 flag

## ReflectionClass API

---

```php

$ref = new ReflectionClass(B::class);
//print_r(ReflectionClass::export(demo::class));
print_r($ref->getProperties()); // 获取一级属性，可以传参数过滤，返回 ReflectionProperty 对象的数组。
var_dump($ref->getConstructor()); // 获取构造函数，未定义返回 null
var_dump($ref->inNamespace()); // 是否在命名空间中
var_dump($ref->getConstants()); // 获取所有定义的常量
var_dump($ref->getConstant('TEST_1')); // 获取某个常量
print_r($ref->getDefaultProperties()); // 获取默认属性，返回数组，包括父类的属性
var_dump($ref->getDocComment()); // 获取类文档注释，不包含属性和方法的注释，无注释返回 false
var_dump($ref->getExtension()); // 获取获取最后一行的行数
var_dump($ref->getFileName()); // 获取定义类的文件名，返回绝对路径
var_dump($ref->getInterfaceNames()); // 获取接口名称，返回索引数组，值为接口名称，未实现接口返回空数组
var_dump($ref->getInterfaces()); // 获取接口，返回关联数组，name=>ReflectionClass 实例，未实现接口返回空数组
var_dump($ref->getMethods()); // 指获取类方法 ReflectionMethod。
var_dump($ref->getMethod('foo4')); // 获取一个类方法的 ReflectionMethod。如果方法不存在会抛出异常，需要配合 try catch 一起用
var_dump($ref->getName()); // 获取类名，包含命名空间
var_dump($ref->getNamespaceName()); // 获取命名空间的名称，没有返回空
var_dump($ref->getParentClass()); // 获取父类 reflectionClass 的实例，没有父类返回 false
var_dump($ref->getProperty('prop3')); // 获取一个属性，返回 ReflectionProperty 实例，属性不存在会抛出异常，需配合 try catch 使用
var_dump($ref->getShortName()); // 获取类名，不包含命名空间
var_dump($ref->getStartLine()); // 获取起始行号
print_r($ref->getStaticProperties()); // 获取静态属性
print_r($ref->getStaticPropertyValue('prop_static')); // 获取静态属性值，未定义的属性会报致命错误
print_r($ref->getTraitAliases()); // 返回 trait 别名的一个数组
print_r($ref->getTraitNames()); // 返回 trait 别名的一个数组
print_r($ref->getTraits()); // 返回这个类所使用的 traits 数组
var_dump($ref->hasConstant('AB')); // 检查常量是否已经定义
var_dump($ref->hasMethod('AB')); // 检查方法是否已经定义
var_dump($ref->hasProperty('AB')); // 检查属性是否已定义
var_dump($ref->implementsInterface('reflection\Abc')); // 检查是否实现了某个接口，注意需要带上命名空间
var_dump($ref->isAbstract()); // 检查类是否是抽象类（abstract）
var_dump($ref->isAnonymous()); // 检查类是否是匿名类
var_dump($ref->isCloneable()); // 返回了一个类是否可复制
var_dump($ref->isFinal()); // 检查类是否声明为 final
var_dump($ref->isInstance($obj)); // 检查一个变量是否此类的实例
var_dump($ref->isInstantiable()); // 检查类是否可实例化
var_dump($ref->isInterface()); // 检查类是否是一个接口（interface）
var_dump($ref->isInternal()); // 检查类是否由扩展或核心在内部定义，和 isUserDefined 相对
var_dump($ref->isIterateable()); // 检查此类是否可迭代，实现了 Iterator 接口即可迭代
var_dump($ref->isSubclassOf(A::class)); // 是否是某一个类的子类
var_dump($ref->isTrait()); // 返回了是否为一个 trait
var_dump($ref->isUserDefined()); // 检查是否由用户定义的类 和 isInternal 相对

// 从指定的参数创建一个新的类实例，创建类的新的实例。给出的参数将会传递到类的构造函数。
// 接受可变数目的参数，用于传递到类的构造函数，和 call_user_func() 很相似。
var_dump($ref->newInstance());
// 从指定的参数创建一个新的类实例，创建类的新的实例。给出的参数将会传递到类的构造函数。
//这个参数以 array 形式传递到类的构造函数。
var_dump($ref->newInstanceArgs([]));
var_dump($ref->newInstanceWithoutConstructor()); // 创建一个新的实例而不调用他的构造函数
$ref->setStaticPropertyValue ('prop_static', '222'); // 设置静态属性的值，无返回值
var_dump($ref->__toString ()); // 返回 ReflectionClass 对象字符串的表示形式。

/*
ReflectionMethod::__construct — ReflectionMethod 的构造函数
ReflectionMethod::export — 输出一个回调方法
ReflectionMethod::getClosure — 返回一个动态建立的方法调用接口，译者注：可以使用这个返回值直接调用非公开方法。
ReflectionMethod::getDeclaringClass — 获取被反射的方法所在类的反射实例
ReflectionMethod::getModifiers — 获取方法的修饰符
ReflectionMethod::getPrototype — 返回方法原型 （如果存在）
ReflectionMethod::invoke — Invoke
ReflectionMethod::invokeArgs — 带参数执行
ReflectionMethod::isAbstract — 判断方法是否是抽象方法
ReflectionMethod::isConstructor — 判断方法是否是构造方法
ReflectionMethod::isDestructor — 判断方法是否是析构方法
ReflectionMethod::isFinal — 判断方法是否定义 final
ReflectionMethod::isPrivate — 判断方法是否是私有方法
ReflectionMethod::isProtected — 判断方法是否是保护方法 (protected)
ReflectionMethod::isPublic — 判断方法是否是公开方法
ReflectionMethod::isStatic — 判断方法是否是静态方法
ReflectionMethod::setAccessible — 设置方法是否访问
ReflectionMethod::__toString — 返回反射方法对象的字符串表达
*/
ReflectionMethod extends ReflectionFunctionAbstract implements Reflector {
    /* 常量 */
    const integer IS_STATIC = 1 ;
    const integer IS_PUBLIC = 256 ;
    const integer IS_PROTECTED = 512 ;
    const integer IS_PRIVATE = 1024 ;
    const integer IS_ABSTRACT = 2 ;
    const integer IS_FINAL = 4 ;
    /* 属性 */
    public $name ;
    public $class ;
    /* 方法 */
    public __construct ( mixed $class , string $name )
    public static export ( string $class , string $name [, bool $return = false ] ) : string
    public getClosure ( object $object ) : Closure
    public getDeclaringClass ( ) : ReflectionClass
    public getModifiers ( ) : int
    public getPrototype ( ) : ReflectionMethod
    public invoke ( object $object [, mixed $parameter [, mixed $... ]] ) : mixed
    public invokeArgs ( object $object , array $args ) : mixed
    public isAbstract ( ) : bool
    public isConstructor ( ) : bool
    public isDestructor ( ) : bool
    public isFinal ( ) : bool
    public isPrivate ( ) : bool
    public isProtected ( ) : bool
    public isPublic ( ) : bool
    public isStatic ( ) : bool
    public setAccessible ( bool $accessible ) : void
    public __toString ( ) : string
    /* 继承的方法 */
    final private ReflectionFunctionAbstract::__clone ( ) : void
    public ReflectionFunctionAbstract::getClosureScopeClass ( ) : ReflectionClass
    public ReflectionFunctionAbstract::getClosureThis ( ) : object
    public ReflectionFunctionAbstract::getDocComment ( ) : string
    public ReflectionFunctionAbstract::getEndLine ( ) : int
    public ReflectionFunctionAbstract::getExtension ( ) : ReflectionExtension
    public ReflectionFunctionAbstract::getExtensionName ( ) : string
    public ReflectionFunctionAbstract::getFileName ( ) : string
    public ReflectionFunctionAbstract::getName ( ) : string
    public ReflectionFunctionAbstract::getNamespaceName ( ) : string
    public ReflectionFunctionAbstract::getNumberOfParameters ( ) : int
    public ReflectionFunctionAbstract::getNumberOfRequiredParameters ( ) : int
    public ReflectionFunctionAbstract::getParameters ( ) : array
    public ReflectionFunctionAbstract::getReturnType ( ) : ReflectionType
    public ReflectionFunctionAbstract::getShortName ( ) : string
    public ReflectionFunctionAbstract::getStartLine ( ) : int
    public ReflectionFunctionAbstract::getStaticVariables ( ) : array
    public ReflectionFunctionAbstract::hasReturnType ( ) : bool
    public ReflectionFunctionAbstract::inNamespace ( ) : bool
    public ReflectionFunctionAbstract::isClosure ( ) : bool
    public ReflectionFunctionAbstract::isDeprecated ( ) : bool
    public ReflectionFunctionAbstract::isGenerator ( ) : bool
    public ReflectionFunctionAbstract::isInternal ( ) : bool
    public ReflectionFunctionAbstract::isUserDefined ( ) : bool
    public ReflectionFunctionAbstract::isVariadic ( ) : bool
    public ReflectionFunctionAbstract::returnsReference ( ) : bool
    abstract public ReflectionFunctionAbstract::__toString ( ) : void
}

​```
