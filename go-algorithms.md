1. 翻书问题或者跳台阶问题。每次可以翻1页书或者2页书，一本N页的书共有多少中翻法。

```go
package main

import (
        "fmt"
)

// O(2^N)
func Fibonacci(n int) int {
        if n == 0 || n == 1 {
                return 1
        }
        if n > 1 {
                return Fibonacci(n-1) + Fibonacci(n-2)
        }
        return 0
}

// O(N)
// dynamic programming
func Fibonacci1(n int) int {
        array := make([]int, n+1)

        array[0] = 1
        array[1] = 1
        i := 2
        for {
                array[i] = array[i-1] + array[i-2]
                i++
                if i > n {
                        break
                }
        }

        return array[n]
}

func main() {
        fmt.Println(Fibonacci1(45))
}

// Fibonacci(100) = Fibonacci(99) + Fibonacci(98) = Fibonacci(98) + Fibonacci(97) + Fibonacci(98)
```

2. 已知一颗二叉树的先序遍历序列为ABCDEFG，中序遍历为CDBAEGF，能否唯一确定一颗二叉树？如果可以，请画出这颗二叉树。

```go
/**
 * Definition for a binary tree node.
 * type TreeNode struct {
 *     Val int
 *     Left *TreeNode
 *     Right *TreeNode
 * }
 */

func buildTree(pre, in []int) *TreeNode {

	if len(in) == 0 {
		return nil
	}

	res := &TreeNode{
		Val: pre[0],
	}

	if len(in) == 1 {
		return res
	}

	idx := indexOf(res.Val, in)

	res.Left = buildTree(pre[1:idx+1], in[:idx])
	res.Right = buildTree(pre[idx+1:], in[idx+1:])

	return res
}

func indexOf(val int, nums []int) int {
	for i, v := range nums {
		if v == val {
			return i
		}
	}

	return 0
}
```

3. 二分查找及其变种

```go
package main

import (
	"fmt"
	"time"
)

func makeRange(min, max int) []int {
	a := make([]int, max-min+1)
	for i := range a {
		a[i] = min + i
	}
	return a
}

func LinearSearch(array []int, t int) bool {
	i := 0
	for i < len(array) {
		if array[i] == t {
			return true
		}
		i++
	}
	return false
}

func BinarySearch(array []int, t int) bool {
	left := 0
	right := len(array) - 1

	for left <= right {
		mid := (left + right) / 2
		if array[mid] < t {
			left = mid + 1
		} else if array[mid] > t {
			right = mid - 1
		} else {
			return true
		}
	}

	return false
}

func main() {
	array := makeRange(0, 1000000000)
	time1 := time.Now()
	bool := LinearSearch(array, 1000000001)
	time2 := time.Now()
	fmt.Println("time2-time1: ", time2.Sub(time1))
	fmt.Println("bool: ", bool)
	time3 := time.Now()
	bool1 := BinarySearch(array, 1000000001)
	time4 := time.Now()
	fmt.Println("time4-time3: ", time4.Sub(time3))
	fmt.Println("bool1: ", bool1)
}
```

```go
package main

import "fmt"

func searchMatrix(matrix [][]int, t int) bool {
        i := 0
        j := len(matrix[0]) - 1
        for i < len(matrix) && j >= 0 {
                if matrix[i][j] == t {
                        return true
                } else if matrix[i][j] > t {
                        j -= 1
                } else {
                        i += 1
                }
        }
        return false
}

func main() {
        matrix := [][]int{
                []int{1, 3, 5, 7},
                []int{10, 11, 16, 20},
                []int{23, 30, 34, 50},
        }
        fmt.Println(searchMatrix(matrix, 16))
}
```

4. 翻转链表

```go
package main

import (
	"fmt"
)

type ListNode struct {
	Val  int
	Next *ListNode
}

func reverseList(head *ListNode) *ListNode {
	if nil == head {
		return nil
	}

	dummy := head
	tmp := head

	for head != nil && head.Next != nil {
		dummy = head.Next
		head.Next = dummy.Next
		dummy.Next = tmp
		tmp = dummy
	}

	return dummy
}

func recursive(head *ListNode) {
	if head == nil {
		return
	}

	fmt.Println(head.Val)
	recursive(head.Next)
}

func recursiveArray(array []int) {
	if len(array) == 0 {
		return
	}

	fmt.Println(array[0])
	recursiveArray(array[1:])
}

func main() {
	head := &ListNode{1, nil}
	head.Next = &ListNode{2, nil}
	head.Next.Next = &ListNode{3, nil}
	tmp := reverseList(head)
	for tmp != nil {
		fmt.Println(tmp.Val)
		tmp = tmp.Next
	}
	recursive(tmp)
	array := []int{1, 2, 3, 4, 5}
	recursiveArray(array)
}
```

5. 快速排序

```go
package main

import (
	"fmt"
)

func partition(A []int, p, r int) int {
	x := A[r]
	i := p - 1
	j := p
	for j >= p && j < r {
		if A[j] <= x {
			i++
			A[i], A[j] = A[j], A[i]
		}
		j++
	}
	A[i+1], A[r] = A[r], A[i+1]
	return i + 1
}

func quickSort(A []int, p, r int) {
	if p < r {
		q := partition(A, p, r)
		quickSort(A, p, q-1)
		quickSort(A, q+1, r)
	}
}

func main() {
	A := []int{2, 8, 7, 1, 3, 5, 6, 4}
	quickSort(A, 0, 7)
	fmt.Println(A)
}
```

6. 快速选择

```go
package main

import (
	"fmt"
)

func partition(A []int, p, r int) int {
	x := A[r]
	i := p - 1
	j := p
	for j >= p && j < r {
		if A[j] >= x {
			i++
			A[i], A[j] = A[j], A[i]
		}
		j++
	}
	A[i+1], A[r] = A[r], A[i+1]
	return i + 1
}

func findKthLargest(A []int, p, r, k int) int {
	if p <= r {
		q := partition(A, p, r)
		if k-1 == q {
			return A[q]
		} else if k-1 < q {
			return findKthLargest(A, p, q-1, k)
		} else {
			return findKthLargest(A, q+1, r, k)
		}
	}
	return -1000000
}

func main() {
	A := []int{2, 8, 7, 1, 3, 5, 6, 4}
	ret := findKthLargest(A, 0, 7, 8)
	fmt.Println("ret: ", ret)
}
```

7. 使用队列实现栈

```go
package main

import "fmt"

type MyStack struct {
	val []int
}

func ConstructorStack() MyStack {
	return MyStack{[]int{}}
}

func (this *MyStack) Push(x int) {
	this.val = append([]int{x}, this.val...)
}

func (this *MyStack) Pop() int {
	tmp := this.val[0]
	this.val = this.val[1:]
	return tmp
}

func (this *MyStack) Peek() int {
	return this.val[0]
}
func (this *MyStack) Size() int {
	return len(this.val)
}

func (this *MyStack) Empty() bool {
	if 0 == len(this.val) {
		return true
	}

	return false
}

type MyQueue struct {
	stack1 MyStack
	stack2 MyStack
}

/** Initialize your data structure here. */
func Constructor() MyQueue {
	var queue MyQueue
	queue.stack1 = ConstructorStack()
	queue.stack2 = ConstructorStack()

	return queue
}

/** Push element x to the back of queue. */
func (this *MyQueue) Push(x int) {
	this.stack1.Push(x)
}

/** Removes the element from in front of queue and returns that element. */
func (this *MyQueue) Pop() int {
	if !this.stack2.Empty() {
		return this.stack2.Pop()
	} else {
		for !this.stack1.Empty() {
			tmp := this.stack1.Pop()
			this.stack2.Push(tmp)
		}
		return this.stack2.Pop()
	}
}

/** Get the front element. */
func (this *MyQueue) Peek() int {
	if !this.stack2.Empty() {
		return this.stack2.Peek()
	} else {
		for !this.stack1.Empty() {
			tmp := this.stack1.Pop()
			this.stack2.Push(tmp)
		}
		return this.stack2.Peek()
	}
}

/** Returns whether the queue is empty. */
func (this *MyQueue) Empty() bool {
	return this.stack1.Empty() && this.stack2.Empty()
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 * param_3 := obj.Peek();
 * param_4 := obj.Empty();
 */

func main() {
	obj := Constructor()
	obj.Push(10)
	obj.Push(11)
	param_2 := obj.Pop()
	fmt.Println("param_2: ", param_2)
	param_3 := obj.Peek()
	fmt.Println("param_3: ", param_3)
	param_4 := obj.Empty()
	fmt.Println("param_4: ", param_4)
}
```

8. 使用栈实现队列

```go
package main

import (
	"fmt"
)

// MyQueue 是利用 栈 实现的队列
type MyQueue struct {
	a, b *Stack
}

// Constructor Initialize your data structure here.
func Constructor() MyQueue {
	return MyQueue{a: NewStack(), b: NewStack()}
}

// Push element x to the back of queue.
func (mq *MyQueue) Push(x int) {
	mq.a.Push(x)
}

// Pop Removes the element from in front of queue and returns that element.
func (mq *MyQueue) Pop() int {
	if mq.b.Len() == 0 {
		for mq.a.Len() > 0 {
			mq.b.Push(mq.a.Pop())
		}
	}

	return mq.b.Pop()
}

// Peek Get the front element.
func (mq *MyQueue) Peek() int {
	res := mq.Pop()
	mq.b.Push(res)
	return res
}

// Empty returns whether the queue is empty.
func (mq *MyQueue) Empty() bool {
	return mq.a.Len() == 0 && mq.b.Len() == 0
}

/**
 * Your MyQueue object will be instantiated and called as such:
 * obj := Constructor();
 * obj.Push(x);
 * param_2 := obj.Pop();
 * param_3 := obj.Peek();
 * param_4 := obj.Empty();
 */

func main() {
	obj := Constructor()
	obj.Push(10)
	param_2 := obj.Pop()
	fmt.Println("param_2: ", param_2)
	param_3 := obj.Peek()
	fmt.Println("param_3: ", param_3)
	param_4 := obj.Empty()
	fmt.Println("param_4: ", param_4)
}

// Stack 是用于存放 int 的 栈
type Stack struct {
	nums []int
}

// NewStack 返回 *kit.Stack
func NewStack() *Stack {
	return &Stack{nums: []int{}}
}

// Push 把 n 放入 栈
func (s *Stack) Push(n int) {
	s.nums = append(s.nums, n)
}

// Pop 从 s 中取出最后放入 栈 的值
func (s *Stack) Pop() int {
	res := s.nums[len(s.nums)-1]
	s.nums = s.nums[:len(s.nums)-1]
	return res
}

// Len 返回 s 的长度
func (s *Stack) Len() int {
	return len(s.nums)
}

// IsEmpty 反馈 s 是否为空
func (s *Stack) IsEmpty() bool {
	return s.Len() == 0
}
```
