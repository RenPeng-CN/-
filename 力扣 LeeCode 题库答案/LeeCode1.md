### [LeeCode](https://leetcode-cn.com/) 力扣题库答案，力求做到写法合理，运行速度快，内存占用少

### 第一题.两数之和  【难度:简单】
给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那 两个 整数，并返回他们的数组下标。

你可以假设每种输入只会对应一个答案。但是，你不能重复利用这个数组中同样的元素。

示例:

``` js
给定 nums = [2, 7, 11, 15], target = 9

因为 nums[0] + nums[1] = 2 + 7 = 9
所以返回 [0, 1]
```

``` js
// 解题
var twoSum = function(nums,target){
	const map = {}; // 初始化一个对象
	for(let i = 0; i<nums.length; i++){
		// 目标值减去 nums 数组的每个值，得出差
		let dif - target - nums[i]
		
		// 如果map 对象中有这个查
		if(map.hasOwnProperty(dif)){
			// 就返回 map对象这个插值的序列下标 和 nums 数组的第几位下标
			return [map[dif],i]
		}
		// 给 map 对象 赋值 并赋值相应的下标
		map[nums[i]] = i
	}
}
```

``` js
执行结果：通过
执行用时 :68 ms , 在所有 JavaScript 提交中击败了 80.58% 的用户
内存消耗 :34.2 MB, 在所有 JavaScript 提交中击败了93.56%的用户
```
-----------------------------------------------------------------
### 第二题. 两数相加  【难度: 中等】
给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。

如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。

您可以假设除了数字 0 之外，这两个数都不会以 0 开头。

``` js
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```

``` js
// 解题, 第一种方法、递归算法
var addTwoNumbers = function(l1, l2) {
    if(!l1) return l2; // 如果l1 链表不存在，就返回 l2 链表
    if(!l2) return l1; // 如果l2 链表不存在，就返回 l1 链表
    l1.val += l2.val; // 将值相加并赋值给 l1的值

    // 如果相加后的值 => 10，就需要向前进一位
    if(l1.val>9){
        l1.val -= 10 // 如果 5+6 = 11 ，那么 11-10 = 1，个位数就是 1，十位数就需要 加 1

        // 如果 l1.next 和 l2.next 都不等于 null，就将进位数据保存到 l1.next 或 l2.next 都行
        // 如果 l1.next 为null，进位数据则保存到 l1.next l2.next 也一样
        if(l1.next !== null && l2.next !== null) l1.next.val++ // l1.next.val 加一

        // 如果 l1.next为null(没有下一个了)，就将其实例化为新的 节点，并赋值1
        // 举例：55+50 ，这两个数都只有两位，相加后=105，1就是新多出来的一位数
        else if(l1.next === null) l1.next = new ListNode(1) 
        else if(l2.next === null) l2.next = new ListNode(1)
    }
    l1.next = addTwoNumbers(l1.next,l2.next) // 实现递归
    return l1
};
```

``` js
执行结果：通过  
执行用时 :116 ms, 在所有 JavaScript 提交中击败了97.66%的用户
内存消耗 :38.2 MB, 在所有 JavaScript 提交中击败了96.41%的用户
```

``` js
// 解题：第二种方法
var addTwoNumbers = function(l1, l2) {
    let result = new ListNode(null); // 实例化一个新的空节点
    let nextResult = result; // 赋值给一个新的变量
    let params = 0 // 此变量用来存储 传递给下一层级的值
    let sum = 0 // 传给当前层级的值

    while(l1 !== null || l2 !== null){
        let x = (l1 !== null) ? l1.val : 0;
        let y = (l2 !== null) ? l2.val : 0;

        sum = (x + y + params) % 10; // 两个值相加，再加上进位的数值 params, %10 是相加的值超过=> 10,将sum变成个位数
        params = Math.floor((x+y+params)/10);

        nextResult.next = new ListNode(sum)
        nextResult = nextResult.next;

        if(l1 !== null) l1 = l1.next
        if(l2 !== null) l2 = l2.next
    }
    if(params){
        nextResult.next = new ListNode(params)
    }
    return result.next
};
```

``` js
执行结果：通过
执行用时 :116 ms, 在所有 JavaScript 提交中击败了97.66%的用户
内存消耗 :38.2 MB, 在所有 JavaScript 提交中击败了94.94%的用户
```