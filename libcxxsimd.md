### 时间

预期是 0924（差不多还有2个月）完成，留出一周缓冲

主要看的参考是 gcc 那边的测试实现，也会看下libcxx atomic 等库的测试实现。

### 主要构思

主要是优先完成 x86 的测试。gcc 的实现非常有意思，写了一个脚本来生成需要编译的平台代码（预定义的一些参数，读入后进行环境变量赋值，然后自动生成）



我们要支持3个不同平台，我目前有一台桌面版（图形界面）Ubuntu2004，后续用qemu 模拟另外两个平台（arm 和 power pc)，这部分工作量我留出一周，我看了主要是一些宏定义调整，以及两个平台一些小特性修改，应该影响不大。



### 需要覆盖的函数族

之前有一个 commit 汇合过所有函数，所以目前是按照注释给的函数集合来进行安排。





可向量化元素类型，和 abi tag 组合形式 5个 public abi 

test_util.h

提到最外层 -》 5   abi tag   duiyu 

[WIP]

### 期待测试形式

一周至少完成一个函数族，按序号递增

测试目标是覆盖所有函数，期待形式是**一个函数一个文件**的成果。

当前实现如果比较完善，就顺延下一个函数族

- [x] 还未实现 ``` simd abi```
- [ ] 9.4 ```simd traits```       1/   
- [ ] 9.5 ```simd.whereexpr``` 2/
- [ ] 9.6.1 ```simd class.overview``` 3/
  - 9.6.4 simd ctor
  - 9.6.5 simd copy function
  - 9.6.6 simd subscript operators
  - 9.6.7 simd unary operators
  - 9.7.1 simd binary operators
  - 9.7.2 simd compound assignment
  - 9.7.3 simd compare operators
- [ ] 9.7.4 ```simd.reductions``` 4/
- [ ] 9.7.6 ```simd.algorithm``` 5/
- [ ] 9.7.5 ```simd.casts```  6/ 
- [ ] 9.7.7 ```simd.math```  注意类型支持 float / double / long double 7/
- [ ] 9.8 ```simd.mask.class``` 8/
- [ ] ```simd.mask.reductions``` 9/
- [ ] 9.8.1 ```simd.mask.overview``` 10/
  - 9.8.3 ctor
  - 9.8.4 copy functions
  - 9.8.5 subscript operators
  - 9.8.6 unary operators
  - 9.9.1 binary operators
  - 9.9.2 compound assignment
  - 9.9.3 comparisons



关于多类型的支持 -> 

```
template<typename T>
void test() {
	ex::fixed_sized_simd<T, 3>();
}

int main() {
	test<char>();
	test<float>();
	-----
	在这边通过不同模板来实例化参数
}
```

