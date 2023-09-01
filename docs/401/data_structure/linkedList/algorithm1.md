# 链表相关算法

## 一、双指针

>两个指针同时遍历链表，一个指针较快，另一个指针较慢

### 1 寻找倒数第k个节点

>先让快的指针先遍历k-1个节点后，快和慢的指针按相同速度遍历，当快的指针指向链表尾时，慢的指针指向倒数第k个指针

### 2 寻找中间节点

>快和慢指针同时遍历链表，快的指针每次遍历2个元素，慢的指针每次遍历1个元素，当快指针指向链表尾时，慢指针指向中间元素。

### 2 链表有环

>**判断：** 快和慢指针同时遍历链表，如果有环时，快的绝对会追上慢的。（和长跑比赛一样，当快一圈时；链表中当快追上慢的时候，遍历数只差就是环形元素长度k）
>**环形入口：** 带环形的链表中，快指针先遍历环形长度k后，快慢指针同时按同样步长遍历链表，快慢指针相遇的节点便是链表中环形的入口。

## 二、反转链表

### 1 链表反转

>原理：将指向调转即可，最终拿到的尾结点便是新的头结点

```java
public ListNode reverseList(ListNode head){
    ListNode prev = null;
    ListNode cur = head;
    while(cur != null){
        ListNode next = cur.next;
        cur.next = prev;
        prev = cur;
        cur = next;
    }
    return prev;
}
```

### 2 反转链表应用场景

>**链表中的数字相加：**
>原理：反转链表，遍历相同索引值相加得到运算结果的反转链表，运算非常简便

>**重排链表：**
>原理:将链表对半分开，把后半部分反转，遍历两链表获得重排链表结果

>**回文链表：**
>原理:将链表对半分开，把后半部分反转，如果前后部分链表长度相同，同时遍历两链表，相同索引结点值相同则原链表为回文链表，如果前后部分链表长度相差1，只需要比较n-1个结点

## 三、循环链表与双向链表

### 1 定义

>循环链表：尾节点指向头节点，所有节点组成的环，并且每一个节点都可当成头节点。

>双向链表：每个节点既指向前一结点又指向后一结点

### 2 场景

#### 2.1 展平多级双向链表

>关键字：**双向链表、迭代**
>伪代码：（1）循环链表，当前结点为p （2）判断是否有子链表。（2.1）如果有则按照（1）循环子链表（2.2）如果没有子链表，继续循环下去（3）直到p->null时，退出循环

```java
public Node flatten(Node head){ 
    flattenGetTail(head);
    return head;
    }
private Node flattenGetTail(Node head){
    Node node = head;
    Node tail = null;
    while(node !=null) {
       Node next = node.next;
        if(node.child != null) {
            Node child=node.child;
            Node childTail = flattenGetTail(node.child);
            node.child=null;
            node.next = child;
            child.prev=node;
            childTail.next =next;
            if(next != null){
                next.prev=childTail;
            }
            tail =childTail;
        } else{
            tail =node;
            }
            node = next;
       }
     return tail;
}
```

#### 2.2 排序的循环链表

>问题：请往节点值递增排序的循环链表中插入一个节点后，使得该链表仍是有序的。

## 四、哨兵节点