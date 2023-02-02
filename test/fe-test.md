# 浅谈 2022 前端工作流中全流程多层次的五款测试工具

## 项目工作流

在大四毕业应届生找工作的时候，我们经常会见到一条招聘要求：**要求实习经历**。

为什么要求你必须有实习经历？

熟悉公司中成熟的规范化的整个开发流程，重点在于**规范化**。

在大学中，当你编写网站时，你大概率编写的是一个较为简单的网站，几乎一人可以搞定所有工作。设计，开发，测试，上线，等等，一人全包。

但是在实际工作中，商业产品的高复杂性意味着几乎不可能一个人完成整个项目从立项，到开发测试，以及上线的整个流程。

而仅仅对于前端来说，工作流一般包括以下几部分：

+ 产品对接
+ 设计对接
+ 开发(前端主工作流)
+ 后端对接
+ 测试对接
+ 上线

## 前端开发工作流

在以上所有工作流中，占用时间最多，也是最为重要的环节是开发环节。

前端基础设施建设，并不一定由前端开发者搭建完成。

+ 前端运维化基建：为前端服务的 npm 私有仓库搭建，以及同样也为后端服务的 Gitlab 私有仓库，Docker 镜像仓库搭建，CI Pipeline 工具搭建等。这将决定你能否很舒服地去迭代，测试以及上线前端项目。如果这一步不完善，你很可能经常在公司加班，甚至熬夜。
+ 前端基建：组件库，脚手架，以及一系列 npm 私有仓库
+ 业务开发：上对产品及设计，下接后端与运维

## 前端开发工作流中的测试环节

在前端工作流中的每一步，都需要随后的测试步骤，进行质量的检查回顾。我们以一个简单的段子，浅显易懂地看看测试是如何工作的。

+ 一个侧试工程师走进一家酒吧，要了一杯啤酒；
+ 一个测试工程师走进一家酒吧，要了一杯咖啡；
+ 一个侧试工程师走进一家酒吧，要了 0 . 7 杯啤酒；
+ 一个侧试工程师走进一家酒吧，要了 -1 杯啤酒；
+ 一个测试工程师走进一家酒吧，要了 232 杯啤酒；
+ 一个测试工程师走进一家酒吧，要了一杯洗脚水；
+ 一个测试工程师走进一家酒吧，要了一杯晰蝎；
+ 一个测试工程师走进一家酒吧，要了一份 asdfas0fas8fasdf
+ 一个测试工程师走进一家酒吧，什么也没要；

在前端开发的工作流中，可简单分为以下几个阶段的测试：

+ 单元测试
+ Component 测试
+ 端对端测试
+ API 接口测试
+ API 压力测试

## 单元测试

在前端开发中，单元测试是必不可少的，在了解单元测试之前，我们先了解一个术语：**断言（assertion）**。

断言是一个结果为真或假的逻辑判断式，目的为了表示程序的实际计算结果与预期结果是否一致。

我们以一个简单的示例了解下是什么是断言，在 JavaScript 语言中，我们可以使用专业的断言库 [chai](https://github.com/chaijs/chai)。

![](https://static.shanyue.tech/images/23-02-02/clipboard-8573.f8492c.webp)

以下是为了测试 `sum` 求和函数的断言。

``` js
// 断言 sum(10,11) === 21
expect(sum(10, 11)).to.equal(21)

// 断言 sum(-10,11) === 1
expect(sum(-10, 12)).to.equal(1)
```

而我们的测试，是基于每一个断言而完成的，我们将测试同一功能的断言集合起来，使用测试框架进行测试。而单元测试是应用于项目基于断言用来测试某单一模块的最小可测单元。见维基百科解释。

> 在计算机编程中，单元测试（Unit Testing）又称为模块测试，是针对程序模块（软件设计的最小单位）来进行正确性检验的测试工作。程序单元是应用的最小可测试部件。在过程化编程中，一个单元就是单个程序、函数、过程等；对于面向对象编程，最小单元就是方法，包括基类（超类）、抽象类、或者派生类（子类）中的方法。

我们可以使用 mocha 等测试框架用以维护项目的所有单元测试。以下是一个来自于 mocha 官方的测试套件，用来测试 `Array.prototype.indexOf()` 函数。

![](https://static.shanyue.tech/images/23-02-02/clipboard-5789.b21b3d.webp)

``` js
// 用以测试 Array 的一系列测试用例
describe('Array', function () {
  // 用以测试 Array.prototype.indexOf 的一系列测试用例
  describe('#indexOf()', function () {
    // 某一测试用例
    it('should return -1 when the value is not present', function () {
      assert.equal([1, 2, 3].indexOf(4), -1);
    });
  });
});
```

执行测试脚本，我们可以得到关于该项目的所有单元测试的测试结果。

``` bash
$ ./node_modules/mocha/bin/mocha

  Array
    #indexOf()
      ✓ should return -1 when the value is not present


  1 passing (9ms)
```

## 组件测试

基于组件的前端系统开发，像是搭建积木一样搭建页面。如果想测试某一页面是否可以正常工作，可查看搭建页面的积木，即单一组件是否正常运行。

![](https://static.shanyue.tech/images/23-02-02/clipboard-3315.3b6909.webp)

以 React 为例，在 React 中，可以使用 `React Testing Library` 对 React Component 进行测试。

![](https://static.shanyue.tech/images/23-02-02/clipboard-8736.b39dfd.webp)

## API 测试

API 是介于前后端间的沟通桥梁，前端项目中的数据几乎全部通过 API 获取数据。因此，保障 API 中的数据准确性，是保障前端项目质量极为重要的一环。

API 测试，推荐最近较为流行的 API 工具：[Apifox](https://www.apifox.cn/a1shanyue)。

![](https://static.shanyue.tech/images/23-02-02/clipboard-3051.59cf3d.webp)

![](https://static.shanyue.tech/images/23-02-02/clipboard-6737.51980a.webp)

## 小结