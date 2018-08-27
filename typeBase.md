#go类型笔记

#### 切片


>截取数组获取切片

	arr1 := [...]int{0, 1, 2, 3, 4, 5, 6}
	s1 := arr1[2:4]
	s2 := s1[3:5]
	fmt.Printf("s1: %v, len: %d, cap: %d\n", s1, len(s1), cap(s1))
	fmt.Printf("s2 is %v", s2)
	
	# stdout
	s1: [2 3], len: 2, cap: 5
	s2 is [5 6]
	
	=>
	对于数组的切片，切片本身是个试图，有三部分组成：
	ptr，len， cap
	默认分片来说，ptr就是分片的起始位置，len代表可访问的长度，cap则是数组长度去除起始位置后仍可访问的最大长度；
	因此新切片的可切范围是：0 到 cap，不是len
	
>直接切片

	var s []int
	fmt.Println(s, len(s), cap(s))

	for i:=0;i<100;i++{
		s = append(s, i)
		fmt.Println( len(s), cap(s))
	}
	
	＃ stdout
	[] 0 0
	1 1
	2 2
	3 4
	4 4
	5 8
	6 8
	7 8
	8 8
	9 16

	＝》
	初始化切片默认是 nil，长度和容量都是0
	容量申请规则，不够的情况，容量直接翻一倍。（去掉初始化0）
	
> make 切片

	# 可以指定len，cap；没有设置值，初始化为0
	s2 := make([]int, 10, 32)
	
	
> 与数组的不同点

	数组： 是数值类型，直接穿入函数，会copy一份。想修改需要穿入数组指针
	切片： 是视图，直接修改就会生效
	
> 删除切片指定元素

	s1 := make([]int, 10, 16)
	copy(s1, []int{1, 2, 3, 4, 5})
	fmt.Printf("default s1: %v, len: %d, cap: %d\n", s1, len(s1), cap(s1))
	s1 = append(s1[:3], s1[4:]...)
	fmt.Printf("delete s1[3], s1: %v, len: %d, cap: %d", s1, len(s1), cap(s1))
	
	# stdout
	default s1: [1 2 3 4 5 0 0 0 0 0], len: 10, cap: 16
	delete s1[3], s1: [1 2 3 5 0 0 0 0 0], len: 9, cap: 16
	
	=>
	没有直接删除方法，相当于直接切片合并成新切片;len减少1，cap不变