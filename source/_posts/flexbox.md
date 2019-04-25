---
title: CSS弹性盒模型flex
---

### 版本更迭
从2007年07月，flex第一版本的工作草案发布，到2012年09月，flex最新版本成为候选推荐。flex主要经历了三个版本。

#### 旧版本 display: box; display: inline-blox;
> IE不支持

> 安卓支持： 2.1 ～ 4.3，IOS支持： 3.2 ～ 6.1，需加 -webkit- 前缀

#### 混合版本 display: flexbox; display: inline-flexbox;
> 只有IE10支持，需加 -ms- 前缀

#### 新版本 display: flex; display: inline-flex;
> IE11+、Google、Firefox、Safari 6.1+、Opera

> IOS 7.1 ~ 8.4 需加 -webkit- 前缀

> 安卓支持：4.4+

``` bash
	.display-flex {
	  display: -webkit-box;
	  display: -ms-flexbox;
	  display: -webkit-flex;
	  display: flex;
	}

	//子元素必须为块级元素
	.display-flex > *{
	  display: block;
	}

	.display-inline-flex {
	  display: -webkit-inline-box;
	  display: -ms-inline-flexbox;
	  display: -webkit-inline-flex;
	  display: inline-flex;
	}

	.display-inline-flex > * {
	  display: block;
	}
```

#### 不影响弹性盒子的属性

1. 多栏布局模块的 column-* 属性对弹性项目无效
1. float、clear、vertical-align 对弹性项目无效。使用 float 将使元素的 display 属性计为block
1. vertical-align 对弹性项目的对齐无效

---

### 基本概念

![基本概念](https://mdn.mozillademos.org/files/12998/flexbox.png)

##### 弹性容器(Flex container)
包含着弹性项目的父元素。通过设置 display: flex 或 diplay: inline-flex;

##### 弹性项目(Flex item)
弹性容器的每个子元素都称为弹性项目。

##### 轴(Axis)
每个弹性框布局分为两个轴。
弹性项目沿其依次排列的那根轴成为主轴（main axis）。
垂直于主轴的那根轴称为侧轴（cross axis）。

##### 方向(Direction)
弹性容器的主轴起点(main start)/主轴终点(main end)和侧轴起点(cross start)/侧轴终点(cross end)描述了弹性项目排布的起点与终点。
它们具体取决于弹性容器的主轴与侧轴中，由 writing-mode 确立的方向（从左到右、从右到左，等等）。

##### 行(Line)
根据 flex-wrap 属性，弹性项目可以排布在单个行或者多个行中。此属性控制侧轴的方向和新行排列的方向。

##### 尺寸(Dimension)
根据弹性容器的主轴与侧轴，弹性项目的宽和高中，对应主轴的称为主轴尺寸(main size) ，对应侧轴的称为 侧轴尺寸(cross size)。


### 容器属性

1.  方向： 指定了内部元素是如何在 flex 容器中布局的， 定义了主轴的方向(正方向或反方向)。
``` bash	
	//伸缩流方向: | 水平方向 | 反向水平 | 垂直方向 | 反向垂直
	flex-drection: | row[默认] | row-reverse | column | column-reverse

	//旧版本: | 水平 | 垂直 | 内联轴方向 | 块级轴方向 |
	box-orient: | horizontal | vertical | inline-axis[默认] | block-axis |

	//旧版本: | 正常 | 反向 |
	box-direction: | normal | reverse |

	.display-flex-column {
	   -webkit-box-orient: vertical;
	   -ms-flex-direction: column;
	   -webkit-flex-direction: column;
	   flex-direction: column;
	}
```

	1. 伸缩流方向与 direction 和 writing-mode 有关系。
``` bash	
	direction: | ltr | rtl | inherit |

	writing-mode: | horizontal-tb | vertical-rl | vertical-lr |
```

1. 主轴对齐方式： 用来设置伸缩容器当前行伸缩项目在主轴方向的对齐方式，指定如何在伸缩项目之间分布伸缩容器额外空间
``` bash
	//主轴对齐方式: | 左对齐 | 居中对齐 | 右对齐 | 两端对齐 | 扩散对齐 |
	//space-between: 均匀排列每个元素, 首个元素放置于起点，末尾元素放置于终点
	//space-around: 均匀排列每个元素, 每个元素周围分配相同的空间

	//新版本
	justify-content: | flex-start[默认] | center | flex-end | space-between | space-around |

	//混合版本
	flex-pack: | start[默认] | center | end | justify | distribute |

	//旧版本
	box-pack: | start[默认] | center | end | justify | N/A |

	.justify-content-center {
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    -webkit-justify-content: center;
    justify-content: center;
	}

	.justify-content-end {
    -webkit-box-pack: end;
    -ms-flex-pack: end;
    -webkit-justify-content: flex-end;
    justify-content: flex-end;
	}

	.justify-content-justify {
    -webkit-box-pack: justify;
    -ms-flex-pack: justify;
    -webkit-justify-content: space-between;
    justify-content: space-between;
	}
```

1. 侧轴对齐: 用来设置伸缩容器当前行在侧轴方向的对齐方式
``` bash
	//侧轴对齐方式: | 顶边对齐 | 中间对齐 | 底部对齐 | 基线对齐 | 伸缩项目拉伸填充整个伸缩容器 |

	//新版本
	align-items: | flex-start | center | flex-end | baseline | stretch[默认] |

	//混合版本
	flex-align: | start | center | end | baseline | stretch[默认] |

	//旧版本
	box-align: | start | center | end | baseline | stretch[默认] |

	.align-items-start {
     -webkit-box-align: start;
     -ms-flex-align: start;
     -webkit-align-items: flex-start;
     align-items: flex-start;
	}

	.align-items-center {
     -webkit-box-align: center;
     -ms-flex-align: center;
     -webkit-align-items: center;
     align-items: center;
	}

	.align-items-end {
     -webkit-box-align: end;
     -ms-flex-align: end;
     -webkit-align-items: flex-end;
     align-items: flex-end;
	}

	.align-items-baseline {
     -webkit-box-align: baseline;
     -ms-flex-align: baseline;
     -webkit-align-items: baseline;
     align-items: baseline;
	}
```

1. 伸缩流换行: 指定伸缩项目溢出伸缩容器时是否换行
``` bash
	//伸缩行换行: | 不换行 | 换行 | 反转换行

	//新版本同混合版本
	flex-wrap: | nowrap[默认] | wrap | wrap-reverse |

	//旧版本，没有浏览器支持box-lines属性，所以在旧版本中无法实现伸缩项目换行显示
	box-lines: | single[默认] | multiple | N/A |
```

1. 伸缩流: 伸缩流方向与伸缩行换行的缩写
``` bash
	//伸缩流: | 伸缩流方向 | 伸缩行换行

	//新版本同混合版本
	flex-flow: | <flex-direction>  <flex-wrap> |

	flex-flow: | row nowrap [默认值]|

	//旧版本无对应属性
```

1. 堆栈伸缩行: 指定多个伸缩项目行在侧轴的对齐方式
``` bash
	//侧轴对齐方式: | 顶边对齐 | 中间对齐 | 底部对齐 | 两端对齐 | 扩散对齐 | 伸缩项目拉伸填充整个伸缩容器 |

	//新版本
	align-content: | flex-start | center | flex-end | space-between | space-around | stretch[默认] |

	//混合版本
	flex-line-pack: | start | center | end | justify | distribute | stretch[默认] |

	//旧版本无对应属性
```

### 项目属性

1. 自身侧轴对齐方式: 单个伸缩项目在侧轴的对齐方式，该属性可以覆盖伸缩容器的侧轴对齐方式
 
  1. 对于匿名伸缩项目，align-self的值永远与其关联的伸缩容器的align-items的值相同
  1. 如果align-self的值为auto，则其计算值为伸缩项目的伸缩容器的align-items值
  1. 如果伸缩项目的任一个侧轴上的外边距为auto，则该伸缩项目在伸缩容器的剩余空间内居中对齐，且align-self没有效果
``` bash
	//侧轴对齐方式: | 自动 | 顶边对齐 | 中间对齐 | 底部对齐 | 基线对齐 | 伸缩项目拉伸填充整个伸缩容器 |

	//新版本
	align-self: | auto[默认] | flex-start | center | flex-end | baseline | stretch |

	//混合版本
	flex-item-align: | auto[默认] | start | center | end | baseline | stretch |

	//旧版本无对应属性
```

1. 伸缩基准值: 伸缩项目在主轴方向上的初始大小

	1. 如果flex-basis的值为0，表示伸缩项目在主轴方向上的初始大小为0，分配所有空间；如果flex-basis的值为auto，表示伸缩项目在主轴方向上的初始大小为设置宽度(如果没有设置宽度，则为内容宽度)，再分配剩余空间
	1. flex-basis的<length>值可以是一个数字后面跟着px、em等单位，也可以是一个百分数，相对于其父伸缩容器的主轴长度
``` bash
	//新版本
	flex-basis: <length> | auto[默认]
	
	//混合版本
	positive-flex: <number>[默认为1]
	
	//旧版本无对应属性
```

1. 扩展比率: 当伸缩容器的额外空间为正值时，此伸缩项目相对伸缩容器里其他伸缩项目能扩展的空间比例

	1. 若flex-grow的值为0表示即使存在剩余空间也不放大；若所有项目的flex-grow属性都为1，则它们将等分剩余空间(如果有的话)；
	1. 若一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍
``` bash
	//新版本
	flex-grow: <number>[默认为0]
	
	//混合版本
	positive-flex: <number>[默认为0]
	
	//旧版本无对应属性
```

1. 收缩比率: 当伸缩容器的额外空间为负值时，此伸缩项目相对于伸缩容器里其他伸缩项目能收缩的空间比例

	1. 如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。

	1. 伸缩基准值、扩展比率和收缩比率都可以为小数，但不能为负数

	1. 伸缩性:是扩展比率、收缩比率和伸缩基准值的缩写
``` bash
	flex: none => flex: 0 0 auto; //表示宽度为原始宽度，不发生扩展或收缩
  
  flex: auto => flex: 1 1 auto; //表示除了占据原先的宽度外，还要分配剩余宽度(包括扩展或收缩)
  
  flex: 0 => flex: 0 1 0%; //表示收缩为最小宽度
  
  flex: 1 => flex: 1 1 0%; //表示分配所有宽度(包括扩展或收缩)

  flex: 0 auto => flex: 0 1 auto; (默认值)//表除了占据原先的宽度外，还要分配剩余宽度(只收缩，不扩展)

  flex: 0 1 => flex: 0 1 0%;
```

	1. 当flex为关键字none或存在auto时，flex-basis为auto；若flex只有数字值，则flex-basis为0%；
``` bash
	//新版本
	flex: none | [<flex-grow> <flex-shrink>? || <flex-basis>]
	
	//混合版本
	flex: none | [<pos-flex> <neg-flex>? || <preferred-size>]
	
	//旧版本
	box-flex: <number>

	.flex-auto {
     -webkit-box-flex: auto;
     -ms-flex: auto;
     -webkit-flex: auto;
     flex: auto;
	}

	.flex-1 {
     -webkit-box-flex: 1;
     -ms-flex: 1;
     -webkit-flex: 1;
     flex: 1;
	}
```

1. 显示顺序: 定义伸缩项目的排列顺序，数值越小，排列越靠前

	1. 伸缩容器中的伸缩项目默认显示顺序是遵循文档在源码中出现的先后顺序(HTML文档的DOM结构中的先后顺序)
	1. order的属性值可以是负数，但不能是小数
``` bash
	//新版本
	order: <number>[默认为0]

	//混合版本
	flex-order: <number>[默认为0]

	//旧版本
	box-ordinal-group: <integer>[默认为1]

	.order-2 {
    -webkit-box-ordinal-group: 2;
    -ms-flex-order: 2;
    -webkit-order: 2;
    order: 2;
	}
```


