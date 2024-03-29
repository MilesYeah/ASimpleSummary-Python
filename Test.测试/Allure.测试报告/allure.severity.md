# allure.severity 标记用例级别

平时写测试用例也会划分优先级

同样，allure 也提供用例级别，在 allure 报告可以清晰看到不同级别用例的缺陷数量 

severity 官方是翻译为优先级，但是如果自己去翻译软件翻译的话是严重程度，我个人更偏向于理解为优先级
会同时显示同一个优先级的失败、通过用例数，以及哪条用例是失败、通过的


## 用例等级介绍
allure 提供的枚举类
```py
class Severity(str, Enum):
    BLOCKER = 'blocker'
    CRITICAL = 'critical'
    NORMAL = 'normal'
    MINOR = 'minor'
    TRIVIAL = 'trivial'
```

### 等级介绍
blocker：阻塞缺陷（功能未实现，无法下一步）
critical：严重缺陷（功能点缺失）
normal： 一般缺陷（边界情况，格式错误）
minor：次要缺陷（界面错误与ui需求不符）
trivial： 轻微缺陷（必须项无提示，或者提示不规范）




### allure-severities 指定优先级运行
当然，也可以根据优先级选择需要运行的测试用例


具体栗子
仍然是上面的代码，打开 cmd
```sh
# 只运行 severity=blocker、critical 的测试用例
pytest test_severity.py -sq --alluredir=./allure --allure-severities=blocker,critical

# 写法二
pytest test_severity.py -sq --alluredir=./allure --allure-severities blocker,critical
```





## 栗子

```py
#!/usr/bin/env python
# -*- coding: utf-8 -*-

"""
__title__  =
__Time__   = 2020-04-19 14:50
__Author__ = 小菠萝测试笔记
__Blog__   = https://www.cnblogs.com/poloyy/
"""

import allure


def test_with_no_severity_label():
    pass


@allure.severity(allure.severity_level.TRIVIAL)
def test_with_trivial_severity():
    pass


@allure.severity(allure.severity_level.NORMAL)
def test_with_normal_severity():
    pass


@allure.severity(allure.severity_level.NORMAL)
class TestClassWithNormalSeverity(object):

    def test_inside_the_normal_severity_test_class(self):
        """ 测试类优先级 normal；看看测试用例是否会自动继承优先级 """
        print()

    @allure.severity(allure.severity_level.CRITICAL)
    def test_inside_the_normal_severity_test_class_with_overriding_critical_severity(self):
        """
        测试类优先级 normal
        测试用例优先级 critical
        """
        pass


@allure.severity("normal")
def test_case_1():
    """ normal 级别测试用例 """
    print("test case 11111111")


@allure.severity("critical")
def test_case_2():
    """ critical 级别测试用例 """
    print("test case 222222222")


@allure.severity("blocker")
def test_case_3():
    """ blocker 级别测试用例 """
    print("test case 4444444")


@allure.severity("minor")
def test_case_4():
    """ minor 级别测试用例 """
    print("test case 11111111")


def test_case_5():
    """ 没标记 severity 的用例默认为 normal"""
    print("test case 5555555555")

```





## ref
* [Pytest 系列（25）- @allure.severity 标记用例级别 _](https://www.cnblogs.com/poloyy/p/13889635.html)
* []()
* []()
* []()
* []()
* []()
* []()
* []()

