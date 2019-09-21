# 2 pointers

### 双指针算法普遍分为三种 <a id="&#x53CC;&#x6307;&#x9488;&#x7B97;&#x6CD5;&#x666E;&#x904D;&#x5206;&#x4E3A;&#x4E09;&#x79CD;"></a>

* **对撞类**
  * **two sum 类，partition 类**
* **窗口类**
* **并行型**
* \*\*\*\*[**Princeton 公开课的课件，讲 2-way 和 3-way partitioning**](http://algs4.cs.princeton.edu/lectures/23DemoPartitioning.pdf)\*\*\*\*
* **掌握 3-way partition 和 k-way partition.**

### 对撞型指针的本质是，不需要扫描多余的状态。在两边指针的判断之后，可以直接跳过其中一个指针与所有另外一面 index 组成的 pair. <a id="&#x5BF9;&#x649E;&#x578B;&#x6307;&#x9488;&#x7684;&#x672C;&#x8D28;&#x662F;&#xFF0C;&#x4E0D;&#x9700;&#x8981;&#x626B;&#x63CF;&#x591A;&#x4F59;&#x7684;&#x72B6;&#x6001;&#x3002;&#x5728;&#x4E24;&#x8FB9;&#x6307;&#x9488;&#x7684;&#x5224;&#x65AD;&#x4E4B;&#x540E;&#xFF0C;&#x53EF;&#x4EE5;&#x76F4;&#x63A5;&#x8DF3;&#x8FC7;&#x5176;&#x4E2D;&#x4E00;&#x4E2A;&#x6307;&#x9488;&#x4E0E;&#x6240;&#x6709;&#x53E6;&#x5916;&#x4E00;&#x9762;-index-&#x7EC4;&#x6210;&#x7684;-pair"></a>

