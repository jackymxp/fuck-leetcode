---
title: 通过链表学递归
date: 2020.3.15
tags:
 - 链表
 - 递归

categories: 
 - leetcode
 - 编程

keywords: [链表, 递归]
description: #如果不写，自动选取文章中的部分作为描述
top_img: 
comments: true  #是否顯示評論（除非設置false,可以不寫）
cover: 
toc: true #是否顯示toc （除非特定文章設置，可以不寫）
toc_number: true #是否顯示toc數字 （除非特定文章設置，可以不寫）
copyright: #是否顯示版權 （除非特定文章設置，可以不寫）
mathjax: false
katex: true
hide:
---

# 为什么要写博客

技术博客的内容固然重要，当然写作的背景和目的甚至比内容更重要。这就如同技术一样，知道技术本身重要，但是了解技术的提出背景和能够解决什么问题更重要。在文章的最后，总结博客的知识点以及存在的问题，这样方便引出下文，使得整套博客的内容有始有终，读起来有剧情感。

当然写博客是一个总结的过程，回顾自己的成长之路，看到自己努力的成果，这是一个正反馈的过程。回想一年多以前的自己，链表翻转的问题都不一定能够完整的写出来，但是经过一年多的刷题，自己再面对`Leetcode`上的题时，即使做不出来，也是会有思路的。

在这里讲一下自己的刷题过程，总体的策略就是按照`tag`进行刷题。具体来讲，每天早上在床上的时候，开始看一下题目，之后在洗漱、吃饭、走路的时候就可以想思路，甚至是实现的细节。有了电脑后去实现，如果成果就思考一下优化的方案；没成功的话，看看思路是不是正确，只会在接下来的空闲时间里去思考问题出在了哪里。在晚上洗脚时调试，临近睡觉之前看评论区的大神代码，毕竟代码一般都不会超过$40$行代码。第二天早上尝试不同的方案去解题。这个过程虽是一个自信心受挫但是成长蛮快的阶段。

半年前就已经将**链表**专题的内容几乎做完，之后又做了**二叉树**专题，后续又做了**递归回溯**、**动态规划**等专题。经过不断的总结，把自己的做题的套路和心得记录下来，总结分享。

这篇博客主要介绍如何去使用**递归**解决**链表**问题，由浅入深，后续的博客中会使用**递归**解决**二叉树**等问题。

![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/00831rSTly1gcvfra4larj30u017de1t-20200322192003869.jpg)

# 解决链表问题的一般技巧

链表是通过带有指针域的节点相互连接成的一种线性结构，由于链表的这种天然的特性，一般都是通过修改链表中节点的指向完成链表的**增删改查**问题。修改链表节点的指针指向一般有非递归方法和递归法。

## 1. 非递归法

**通过修改指针的方式一般有以下几种技巧**：

-   设置虚拟头节点，但在函数最后返回之前，记得释放内存。

-   使用快慢指针，一般用于定位到中间节点或者判断链表是否有环问题。

## 2. 递归法

-   **递归法的原理**

    由于链表是通过指针域连接的线性结构，所以可以把一个完整的链表看成若干个结构相同但是长更短的链表构成，这就相当于在递归函数中缩减了函数的规模。

    当链表的长度短到一定的程度，就相当于递归函数的退出条件。

    当对更短的节点修改了指针后，链表的局部达到了期望的结构。其余的链表也使用同样的方法即调用同一个函数来修改节点的指向，这样就完成了整体链表结构的修改。

    这就是通过递归修改链表的原理，通过这个原理总结了编写递归函数的

-   **编写递归函数的三个步骤：**

    1.  定义了一个递归函数`fun(ListNode*)`，一定要**明确递归函数的功能，以及递归函数的返回值**。这样是当传入头节点`head`并且执行完`fun(head)`函数后，链表的结构就应该改变成希望的样子。这个就是递归的功能。
    2.  **明确递归函数的退出条件**。否则递归函数找不到出口，就一直递归下去，爆栈。
    3.  将链表拆成若干部分，一部分的头节点是`head`，另外一部分的头节点是`newHead`，**根据定义的函数的功能，推理当执行完`fun(newHead)`的时候，链表变成了什么样子，这时`newHead`部分已经变成我们期望的链表结构。接下来就是应该如何修改`head`为头节点的链表的的指针，使得整个链表的结构和期望的结构相同**。

    **理解递归的本质就是学会放弃思考，放弃你刨根问底的想法，切记千万不要进入递归函数中探索太多的细节，只要理解递归两层之间接口之间的关系，以及递归退出的条件**。人脑的栈太小，探索不了太多细节就乱了。实在想探索递归函数运行的细节，就在纸上画图，画出每层递归函数的入口参数及返回值。**至于为什么我们需要放弃思考而信任递归的结果，那是因为我们更相信数学归纳法。**

好了，给出我的心得之后，但是这些就像是数学公式一样，如果不通过例题是很难知道怎么用这些公式一样。所以接下来就通过一些例题来体会一下如何写好递归函数。**记住想要理解递归，就要放弃思考，放弃刨根问底的钻研精神。**

# 翻转链表题

## [206. 反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)

1. **定义递归函数`ListNode* reverseList(ListNode* head)`功能以及返回值**：

    定义功能为：翻转以`head`为头节点的链表，并返回翻转后的链表的头节点。

    根据定义的递归函数功能，首先将`reverseList`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`reverseList`的作用范围，也就是将`head`作为参数传递给`reverseList(head)`作用的范围。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gd2x4tff4nj318e0620u7.jpg)

    当把`head`作为参数传递给递归函数`reverseList`作用后，根据定义的递归函数功能，这个函数就完成了整个链表的翻转，也就是期望的最终答案。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gd2x4twjlmj315604kmxx.jpg)

2. **寻找递归函数退出条件和返回值**

    当链表中只有一个节点或者没有节点的时候，不需要任何反转，就可以直接返回。

    ```cpp
    if(!head || !head->next)
        return head;
    ```

3. **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩小递归函数的规模，所以要将链表分成了两个部分，由于是每一个节点都要进行反转，所以要将链表分成长度分别为$1$和$k-1$的两个链表，其中$k$是原链表的长度。两部分链表的头节点分别为`head`和`newHead`。当缩小递归函数的范围的时候，即将`newHead`作为参数传入到`reverseList`函数中，其中递归函数的作用范围表示成图。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322101626.png)

    当递归函数`reverseList(newHead)`执行完成后，根据递归函数的功能，这样就完成了长度为$k-1$部分链表的反转，除头节点外的其余节点都符合期望的结构。整个链表变成如下这个样子。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322102043.png)

    写成代码的形式：

    ```cpp
    ListNode* newHead = next->next;
    head->next = nullptr; //将链表分成两个部分
    ListNode* retHead = reverseList(newHead);
    ```

    通过观察发现，只要在修改`head`和`newHead`的指针就完成了这个链表的反转。

    **最后不要忘记让头节点`head->next`指向`nullptr`，否则递归函数找不到空节点，递归超时。这样就完成了翻转**。写成代码如下：

    ```cpp
    newHead->next = head;
    head->next = nullptr;
    ```

    当然除了上面一个一个减少递归的规模的话，我们还可以一半一半的减少，比如，我们每次翻转链表的一半，最后再将两部分合并起来。

    问题的关键，我们如何把一个链表拆成两部分，上面的策略是将链表拆成了长度为$1$和长度为$k-1$两个部分，现在将链表拆成长度相等的两个部分，所以问题是，如何定位到链表的中间节点，这里使用快慢指针定位到链表的中间节点。

    ```cpp
    ListNode* fast = head;
    ListNode* slow = head;
    ListNode* mid = NULL;
    while(fast && fast->next)
    {
        fast = fast->next->next;
        mid = slow;
        slow = slow->next;
    }
    mid->next = NULL;
    ```

    当定位到中间节点后，我们对每一部分都进行翻转

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200403182532.png)

    根据递归函数的功能，当执行完`reverseList(head)`和`reverseList(slow)`之后，每一部分都进行了翻转。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200403183121.png)

    这时候后只要改变`slow->next = tmp`就可以。最后返回`tmp2`。

    ```cpp
    ListNode* tmp = reverseList(head);
    ListNode* retNode = reverseList(slow);
    slow->next = tmp;
    ```

4. **递归代码**

    ```cpp
    class Solution {
    public:
        ListNode* reverseList(ListNode* head) {
            //当链表中只有一个节点或者没有节点的时候，直接退出
            if(!head || !head->next)
                return head;
            //分成两个部分
            ListNode* newHead = head->next;
            head->next = nullptr;
            //完成 k - 1 部分的翻转
            ListNode* retHead = reverseList(newHead);
            //修改指针，是最后一部分完成翻转
            newHead->next = head;
            head->next = nullptr;
            return retHead;
        }
    };
    ```

5. **迭代代码**

    ```cpp
    class Solution {
    public:
        ListNode* reverseList(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode* pre = nullptr;
            ListNode* cur = head;  //pre -> cur -> next   ===>>>   pre <- cur <- next
            while(cur)
            {
                ListNode* next = cur->next; //保存下一个节点
                cur->next = pre; //修改next指针是其指向 pre
                //pre和next都向后移动一位
                pre = cur;  
                cur = next;
            }
            return pre;
        }
    };
    ```

---

## [24. 两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)

还是按照递归的套路，分三步。

1. **定义递归函数`ListNode* swapPairs(ListNode* head)`功能以及返回值**

    定义递归函数功能：每两个翻转链表中的节点，并将翻转后新的链表节点作为返回值。

    根据定义的递归函数功能，首先将`swapPairs`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`swapPairs`的作用范围，也就是将就是将`head`作为参数传递给`swapPairs(head)`作用的范围。
    
    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcvk3rsggjj318e06imxj.jpg)
    
    当把`head`作为参数传递给递归函数`swapPairs`作用后，根据定义的递归函数功能，这个函数就完成**两两交换其中相邻的节点**，并返回了交换后的链表的头节点`retHead`。
    
    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322105029.png)
    
2. **寻找递归函数退出条件和返回值**

    当链表中只有一个节点或者没有节点的时候，不需要任何反转，就可以直接返回。

    ```cpp
    if(head == nullptr || head->next == nullptr)
        return head;
    ```

3. **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩小递归函数的规模，所以要将链表分成了两个部分，由于是每一个节点都要进行反转，所以要将链表分成长度分别为$2$和$k-2$的两个链表，其中$k$是原链表的长度。两部分链表的头节点分别为`head`和`newHead`。当缩小递归函数的范围的时候，即将`newHead`作为参数传入到`swapPairs`函数中，其中递归函数的作用范围表示成图。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322105844.png)

    当递归函数`swapPairs(newHead)`执行完成后，根据递归函数的功能，除第一个和第二个节点没有符合期望的结构，其余节点都符合期望的结构。整个链表变成如下这个样子。

    ![image-20200322110545833](https://tva1.sinaimg.cn/large/00831rSTly1gd2kg8bafij31100803za.jpg)

    写成代码的形式：

    ```cpp
    //将链表分为两个部分
    ListNode* newHead = head->next->next;
    head->next->next = nullptr;
    //递归反转长度为 k - 2 的链表部分
    ListNode* retNode = swapPairs(newHead);
    ```

    最后和预期的结构相对比，只要修改`head`、`newHead`的指针域，即可完成整个链表的两两节点的交换，写成代码：

    ```cpp
    //对照图片，完成长度为 2 的链表的节点交换
    ListNode* retHead = head->next;
    retHead->next = head;
    head->next = retNode;
    return retHead;
    ```

4. **递归代码**

    ```cpp
    class Solution {
    public:
        ListNode* swapPairs(ListNode* head) {
            //递归退出条件，只有一个节点，或者没有节点的时候
            if(!head || !head->next)
                return head;
            //将链表分为两个部分
            ListNode* newHead = head->next->next;
            head->next->next = nullptr;
            //递归反转长度为 k - 2 的链表部分
            ListNode* retNode = swapPairs(newHead);
            //通过修改长度为2部分链表的指针，完成这个链表的翻转
            ListNode* retHead = head->next;
            retHead->next = head;
            head->next = retNode;
            return retHead;
        }
    };
    ```

5. **迭代代码**

    ```cpp
    class Solution {
    public:
        ListNode* swapPairs(ListNode* head) {
            if(!head || !head->next)
                return head;
            ListNode *first = head;
            ListNode *second = head->next;
            while(1){
                ListNode *third = second->next;
                //交换
                swap(second->val,first->val);
                if(!third || !third->next)return head;
                //向前前进两步
                first = third;
                second = third->next;
                third = third->next->next;
            }
            return head;
        }
    };
    ```

---

## [25. K 个一组翻转链表](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)

这道题在`leetcode`中被定义为难题，但是有前面的基础，当$k=2$的时候，就是[两两交换链表中的节点](https://leetcode-cn.com/problems/swap-nodes-in-pairs/)题；当$k$等于链表长度时，就是[反转链表](https://leetcode-cn.com/problems/reverse-linked-list/)题，所以做这个题就不是很难了，只需要做一小部分的改动就可以。下面根据递归套路的三个步骤，完成这个题。

1.  **定义递归函数`ListNode* reverseKGroup(ListNode* head, int k)`功能以及返回值**

    定义递归函数功能：在链表中，每$k$个节点翻转一次，并将新的链表节点作为返回值。

    根据定义的递归函数功能，首先将`reverseKGroup`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`reverseKGroup`的作用范围，也就是将`head`作为参数传递给`reverseKGroup(head)`作用的范围。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322120919.png)

    当把`head`作为参数传递给递归函数`reverseKGroup`作用后，根据定义的递归函数功能，这个函数就完成以$k$个为一组进行翻转链表，并返回翻转后的链表头节点。假设这里$k = 3$。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322120958.png)

2.  **寻找递归函数退出条件和返回值**

    当链表中的节点的个数小于$k$的时候，不够一组，可以不需要做任何的翻转操作直接返回。

    ```cpp
    if(NodeNumLessK(head, k) == true)
        return head;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩小递归函数的规模，所以要将链表分成了两个部分，由于是每$k$节点反转，所以将链表分成长度分别为$k$和$n-k$的两个链表，其中$n$是原链表的长度。两部分链表的头节点分别为`head`和`newHead`。当缩小递归函数的范围的时候，即将`newHead`作为参数传入到`reverseKGroup`函数中，其中递归函数的作用范围表示成图。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322121211.png)

    当递归函数`reverseKGroup(newHead)`执行完成后，根据递归函数的功能，除前$k$个链表没有符合期望的结构，其余节点都符合期望的结构。整个链表变成如下这个样子。

    ![image-20200322121314111](https://tva1.sinaimg.cn/large/00831rSTly1gd2x4ueqt7j31cm07o75j.jpg)

    写成代码的形式：

    ```cpp
    //定位到第 k 个节点
    ListNode* newHead = head;
    ListNode* retNode;
    for(int i = 0; i < k; i++){
        retNode = newHead;
        newHead = newHead->next;
    }
    //将链表分成两个部分
    retNode->next = nullptr;
    //递归反转长度为 n - k 的链表部分
    ListNode* retHead = reverseKGroup(newHead, k);
    ```

    最后和预期的结构相对比，只要翻转前$k$个链表的节点，再改变指针的指向就完成了所有节点按照$k$个一组翻转链表。写成代码：

    ```cpp
reverseList(head);
    //将第一部分和第二部分连接起来
    head->next = retHead;
    return retNode;
    ```
    
4.  **递归代码**

    **PS:** 这段代码虽然有近$50$行，但是抛出递归退出条件的判断和前面遇到的翻转链表的代码，当然还有很多注释之外，核心的代码不到$10$行，所以结合前面的图和注释很容易理解。

    ```cpp
    class Solution {
    public:
        bool NodeNumLessK(ListNode* p, int k){
            for(int i = 0; i < k; i++)
            {
                if(p == nullptr)
                    return true;
                p = p->next;
            }
            return false;
        }
        ListNode* reverseList(ListNode* head) {
            //当只有一个节点或者没有节点，递归退出条件
            if(!head || !head->next)
            return head;
            //缩小递归规模
            ListNode* newHead = reverseList(head->next);
            //改变层与层之间的关系
            //当head->next部分完成反转后，改变指针指向，完成整个链表的翻转
            head->next->next = head;
            //将head->next置空，否则找不到空节点，递归超时
            head->next = nullptr;
            return newHead;
        }
        ListNode* reverseKGroup(ListNode* head, int k) {
            //当链表节点小于k的时候，直接退出
            if(NodeNumLessK(head, k))
                return head;
    
            //定位到第 k 个节点
            ListNode* newHead = head;
            ListNode* retNode;
            for(int i = 0; i < k; i++){
                retNode = newHead;
                newHead = newHead->next;
            }
            //将链表分成两个部分
            retNode->next = nullptr;
            //递归反转长度为 n - k 的链表部分
            ListNode* retHead = reverseKGroup(newHead, k);
    
            reverseList(head);
            //将第一部分和第二部分连接起来
            head->next = retHead;
            return retNode;
        }
    };
    ```

---

# 合并链表题

翻转链表类型的题目，涉及的都是直接修改链表的指针指向，在合并链表的过程中，涉及的还是修改指针的指向，唯一不同的是，链表翻转是一个链表的，而合并链表的题可能涉及到多个链表，但是也只是单纯的改变指针指。

## [21. 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)

还是按照递归的三个步骤

1.  **定义递归函数`ListNode* mergeTwoLists(ListNode* l1, ListNode* l2)`功能以及返回值**

    合并两个已经排序的链表，并且返回排序后链表的头节点。

    根据这个定义，当传入两个如下所示的链表，应该返回的是这样的链表。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcwo90kk3fj31ly0pqmz2.jpg)

2.  **寻找递归函数退出条件和返回值**

    当两个链表中有一个链表为空的时候，不管另外一个链表是否为空，都直接返回另外一个链表。

    ```cpp
    if(!l1 || !l2)
        return l1 ? l1 : l2;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩小递归函数的规模，所以要将链表分成了两个部分，由于是合并链表，所以将链表分成已排序的和未排序的链表两个部分。合并的过程从两个链表的头节点开始，`p1`的头节点值小于`p2`的值，所以`p1`的头节点就是合并后的链表的头节点。 将`p1`节点单独拿出来，并且保存`p1`节点的下一个节点为`pn`。此时`p1`节点已经是有序的。

    继续合并链表中的剩余节点，发现`pn`和`p2`依然是一个合并有序链表的过程，还是依然比较两个头节点的值，所以这个过程依然可以调用`mergeTwoLists(pn, p2)`完成链表剩余部分的排序。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcwoqo4jfvj312u0k0my8.jpg)

    当递归函数 `mergeTwoLists(pn, p2)` 执行完成后，根据递归函数的功能，`pn`和`p2`链表已经形成一个有序的链表，所以整个链表变成如下这个样子。

    将链表的两部分连接起来，所以将`p1->next`指向`retNode`。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322203408.png)

    将上述过程写成代码：

    ```cpp
    if(p1->val < p2->val)
    {
        ListNode* pn = l1->next;
        l1->next = nullptr;
        ListNode* retNode = mergeTwoLists(pn, p2);
        p1->next = retNode;
        return p1;
    }
    ```

    当`p2`的值小于`p1`的值时，合并的过程还是这个样子的。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322205036.png)

4.  **完整代码**

    ```cpp
    class Solution {
    public:
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            //当有一个链表为空的时候，直接返回另外一个链表，不管另外一个是否为空
            if(!l1 || !l2)
                return l1 ? l1 : l2;
            //当l1->val 小于l2->val, 直接将l1->next指向递归的下一层
            if(l1->val < l2->val)
            {
                l1->next = mergeTwoLists(l1->next, l2);
                return l1;
            }
            else
            {
                l2->next = mergeTwoLists(l1, l2->next);
                return l2;
            }
        }
    };
    ```

## [23. 合并K个排序链表](https://leetcode-cn.com/problems/merge-k-sorted-lists/)

有了[合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)基础，解决这道题的基本思路还是一样，这次不画图，只是根据定义的递归函数`ListNode* mergeKLists(vector<ListNode*>& lists)`和上一题的`ListNode* mergeTwoLists(ListNode* l1, ListNode* l2)`的函数基础，单纯的推导出如何合并$k$个链表。

还是按照递归的三个步骤继续走起。

1.  **定义递归函数`ListNode* mergeKLists(vector<ListNode*>& lists)`功能以及返回值**

    定义递归函数功能：按照从小达到的顺序合并$k$个有序链表，并返回排序后链表的头节点。

2.  **寻找递归函数退出条件和返回值**

    想一下，这时候的递归的退出条件是什么？很简单，无非就是讨论$k$的值。

    -   当$k = 0$时，不需要返回任何链表，所以直接返回`nullptr`。
    -   当$k = 1$时，只有一个链表，不需要合并操作，直接返回`lists[0]`。
    -   当$k = 2$时，这个就是合并两个有序链表，直接调用`ListNode* mergeTwoLists(lists[0], lists[1])`完成合并。

    ```cpp
    if(k == 0)
        return nullptr;
    else if(k == 1)
        return lists[0];
    else if(k == 2)
        return mergeTwoLists(lists[0], lists[1]);
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩小递归函数的规模，所以要将$k$个链表分成了两个部分，但是每个部分有多少个链表，这里可以有不同的分配策略。

    **策略1：**

    将$k$个链表拆分成两部分，其中一部分的链表规模是$1$，另外一部分链表的规模时$k-1$。当有$k$个链表的时候，调用的是`mergeKLists`函数，那么当$k = k-1$时并且$k-1 > 2$时，依然可以调用`mergeKLists(lists[0...k-2])`函数合并$k-1$个链表，并返回一个排序的链表头节点`node`，再和规模为$1$的第$k$个链表做合并两个排序链表的过程，即调用`mergeTwoLists(node, lists[k-1])`，再返回。

    对应的代码

    ```cpp
    ListNode* node = mergeKLists(vector<ListNode*>(lists.begin(), lists.end()-1));
    return mergeTwoLists(node, lists[lists.size()-1]);
    ```

    这样的合并策略由于每次只能合并一条链表，所以递归深度比较深，所以性能开销比较大。

    **策略2:**

    仿照归并排序，将$k$个链表分成$2$个部分，每部分有$k/2$个链表。当$k = k/2$时并且$k/2 > 2$时，依然可以调用`mergeKLists(lists[0...k/2))`和`mergeKLists(lists[k/2...k))`函数合并$k/2$个链表，每部分都会返回排序$k/2$个链表的头节点`node1`和`node2`，这时候再调用`mergeTwoLists(node1, node2)`，再返回。

    对应的代码

    ```cpp
    vector<ListNode*> front(lists.begin(), lists.begin()+len/2);
    vector<ListNode*> back(lists.begin()+len/2, lists.end());
    //合并前k/2个链表
    ListNode* node1 = mergeKLists(front);
    //合并后k/2个链表
    ListNode* node2 = mergeKLists(back);
    //将两个链表合并的结果使用合并两个链表函数合并
    return mergeTwoLists(node1, node2);
    ```

4.  **完整代码**

    ```cpp
    class Solution {
        ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
            //当有一个链表为空的时候，直接返回另外一个链表，不管另外一个是否为空
            if(!l1 || !l2)
                return l1 ? l1 : l2;
            //当l1->val 小于l2->val, 直接将l1->next指向递归的下一层
            if(l1->val < l2->val)
            {
                l1->next = mergeTwoLists(l1->next, l2);
                return l1;
            }
            else
            {
                l2->next = mergeTwoLists(l1, l2->next);
                return l2;
            }
        }
    public:
        ListNode* mergeKLists(vector<ListNode*>& lists) {
            int len = lists.size();
            if(len == 0)
                return nullptr;
            else if(len == 1)
                return lists[0];
            else if(len == 2)
                return mergeTwoLists(lists[0], lists[1]);
            else
            {
                vector<ListNode*> front(lists.begin(), lists.begin()+len/2);
                vector<ListNode*> back(lists.begin()+len/2, lists.end());
                ListNode* node1 = mergeKLists(front);
                ListNode* node2 = mergeKLists(back);
                return mergeTwoLists(node1, node2);
            }         
        }
    };
    ```

## [148. 排序链表](https://leetcode-cn.com/problems/sort-list/)

这个题要求使用`O(nlogn)`的时间复杂度，只有快速排序、堆排序、归并排序符合这个时间的复杂度为了复用上面实现的合并两个有序链表的代码， 采用**归并排序**。

还是原来的套路，分三步继续走。Let's go。

1.  **定义递归函数`ListNode* sortList(ListNode* head)`功能以及返回值**：

    根据定义的递归函数功能，首先将`sortList`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`sortList`的作用范围，也就是将`head`作为参数传递给`sortList(head)`作用的范围。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322222549.png)

    当把`head`作为参数传递给递归函数`sortList`作用后，根据定义的递归函数功能，这个函数就完成了整个链表的排序，也就是期望的最终答案。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322222859.png)

2.  **寻找递归函数退出条件和返回值**

    当链表长度为$1$或者$0$时，就不用排序，直接返回就可以。

    ```cpp
    if(!head || !head->next)
        return head;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    在这里思考，如何缩小递归的规模，也就是让`sortList`中传入的链表长度减少，那就是将链表分为两个部分，分别对链表的每一个部分进行排序。具体怎么分有不同的策略，也可以仿照上题的策略，但是比较高效的方法是每部分链表长度分别为原来的$1/2$。也就是从中间断开链表。所以涉及到的问题是，如何寻找一个链表的中点？

    这里使用快慢指针，快指针一次走两步，慢指针一次走一步，当快指针到尾部的时候，慢指针刚好走到链表的中点。

    ```cpp
    ListNode* fast = head;
    ListNode* slow = head;
    ListNode* mid = nullptr;
    while(fast && fast->next)
    {
        fast = fast->next->next;
        mid = slow;
        slow = slow->next;
    }
    //为了使的链表成为两个独立的部分，将slow的next置空
    slow->next = nullptr;
    //此时[head, slow] 是链表的前一个部分
    //[mid, end)是链表的后一个部分。
    ```

    找到中点后，可以调用`p1 = sortList(head)` 和`p2 = sortList(mid)`分别对原链表的两部分进行了排序。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322223155.png)

    根据定义递归函数的功能，就是排序一个链表，所以当`p1 = sortList(head)` 和`p2 = sortList(mid)`执行之后，整个链表变成如下这个样子。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322223416.png)

    那么接下来的问题时，就是如何合并两个已经排序的链表呢？咦，这不是[ 合并两个有序链表](https://leetcode-cn.com/problems/merge-two-sorted-lists/)这个题嘛。所以直接调用`mergeTwoLists(node1, node2)`合并两个排序链表。

    最后上一张图来表示一下这一个过程。其中绿色的阴影部分表示的是使用`sortList`函数，粉色的阴影表示的是`mergeTwoLists`作用范围。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcx9ggnj0dj31r70u07bp.jpg)

4.  **完整代码**

    ```cpp
    class Solution {
        ListNode* merge(ListNode* p1, ListNode* p2)
        {
            if(!p1 || !p2)
                return p1 ? p1 : p2;
            if(p1->val < p2->val)
            {
                p1->next = merge(p1->next, p2);
                return p1;
            }
            else 
            {
                p2->next = merge(p1, p2->next);
                return p2;
            }
        }
    public:
        ListNode* sortList(ListNode* head) {
            if(!head || !head->next)
                return head;
            //定位到mid
            ListNode* fast = head;
            ListNode* slow = head;
            ListNode* mid = nullptr;
            while(fast && fast->next)
            {
                fast = fast->next->next;
                mid = slow;
                slow = slow->next;
            }
            mid->next = nullptr;
            //分别对链表的两个部分进行排序。
            ListNode* node1 = sortList(head);
            ListNode* node2 = sortList(slow);
            //对两个已经排好序的链表进行合并。
            return merge(node1, node2);
        }
    };
    ```


## [2. 两数相加](https://leetcode-cn.com/problems/add-two-numbers/)

为什么我把这个题归结于合并链表题呢？其实上面三道题可以看作以链表的值的大小进行合并的一种方式，这个题不是根据大小进行合并，而是以“加法”操作对链表进行合并。那么我们看下在链表中如何按照"加法"这种操作对链表进行操作。

还是按照递归的三个步骤进行。

1.  **定义递归函数`ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) `功能以及返回值**：

    定义递归函数功能：对链表`l1`和`l2`按照题目给定的规则进行合并，并返回合并后链表的头节点。

    根据我们定义的递归的函数的功能，首先将`addTwoNumbers`作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`addTwoNumbers`的作用范围，也就是将`p1`和`p2`作为参数传递给`addTwoNumbers(p1, p2)`作用的范围。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322224858.png" style="zoom:50%;" />

    当把 `p1`和`p2` 作为参数传递给递归函数 `addTwoNumbers` 作用后，根据定义的递归函数功能，这个函数就完成了整个链表的相加，也就是期望的最终答案。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322224942.png" style="zoom:50%;" />

2.  **寻找递归函数退出条件和返回值**

    当有一个链表为空的时候，直接返回另外一个链表，不管另外一个链表是否为空。

    ```cpp
    if(!l1 || !l2)
        return l1 ? l1 : l2;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    如何缩小递归函数的规模呢，在这里只有一种选择，就是每次将`p1`和`p2`的指针后移一位，再次调用`node = addTwoNumbers(pm, pn)`完成剩余部分的加法合并。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322225336.png" style="zoom:50%;" />

    当执行完`addTwoNumbers(pm, pn)`后，整个链表变成如下结构：

    <img src="https://tva1.sinaimg.cn/large/00831rSTly1gd33kz06wmj30pw0hi3z4.jpg" style="zoom:50%;" />

    为了将已经相加的链表和未相加的链表连接在一起，令`sum = p1->val + p2->val` 。

    这里就需要对`sum`的值进行考虑，如果$sum < 10$，如上图所示，直接重新分配一个值为$sum$的节点`head`，并直接修改指针`head->next = node`即可。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322230427.png" style="zoom:50%;" />

    当$sum < 10$的逻辑代码如下：

    ```cpp
    if(sum < 10)
    {
        ListNode* node = addTwoNumbers(p1->next, p2->next);
        ListNode* head = new ListNode(sum);
        head->next = node;
    	return node;
    }
    ```

    但是当$sum >= 10$是，要考虑到进位，这才是本题的难点。

    如果$sum >= 10$时，新分配一个值为$sum \; mod \; 10$的节点`head`是不够的， 还需要分配一个值为$1$的节点`flag`，在这里我们要如何处理这个进位操作呢？

    其实很简单，只需要让$1$与已经相加的`node`链表再做一次相加操作，即只需要让调用`addTwoNumbers(node, flag)`操作即可。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200322231503.png" style="zoom:50%;" />

    <img src="https://tva1.sinaimg.cn/large/00831rSTly1gd3473mmy6j30o00eqdgl.jpg" style="zoom:50%;" />

    为了看的更加清楚，我将其画在了一起，其中绿色的部分代表递归已经完成了，其实应该等价于一个链表`next`，在这里为了看清楚，没有将其化为一个链表。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcxb7z847xj31rw0skq54.jpg)

    这样当$sum >= 10$大于$10$时的逻辑如下：

    ```cpp
    if(sum >= 10)
    {
        ListNode* node = addTwoNumbers(p1->next, p2->next);
        ListNode* head = new ListNode(sum % 10);
        head->next = addTwoNumbers(node, new ListNode(1));
        return head;
    }
    ```

4.  **完整代码**

    ```cpp
    class Solution {
    public:
        ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
            //递归退出条件，当有一个为空或者两个都为空
            if(!l1 || !l2)
                return l1 ? l1 : l2;
            //这里l1和l2都不为空，所以可以直接访问l1->val 和 l2->val
            int sum = l1->val + l2->val;
            ListNode* newNode = new ListNode(sum % 10);
            //保存l1->next和l2->next的addTwoNumbers结果
            ListNode* nextNode = addTwoNumbers(l1->next, l2->next);
            //当sum小于10的时候，不会产生进位，所以直接赋值给next就可以
            if(sum < 10)
                newNode->next = nextNode;
            else//sum >= 10,则需要将产生的结果和1做加法的结果赋值给next
                newNode->next = addTwoNumbers(nextNode, new ListNode(1));
            return newNode;
        }
    };
    ```

    ---

    

# 在链表中移除元素

上面的一些题，都是通过递归修改链表内部的指针，从而得到期望的链表结构。其实这些题目用普通方法都是可以解答的，但是使用普通的方法很容易出错，修改修改指针就乱了，或者产生内存泄漏。

但是如果理解递归的基本思路，就可以编写出更简单的代码，也更容易理解。下面就继续通过递归的方式，去完成几个在链表中删除元素的题。继续理解上面说的递归的三个步骤。

## [203. 移除链表元素](https://leetcode-cn.com/problems/remove-linked-list-elements/)

1.  **定义递归函数`ListNode* removeElements(ListNode* head, int val)`功能以及返回值**：

    定义递归函数功能：删除链表中值等于`val`的节点，并返回新的头节点。

    根据定义的递归函数功能，首先将`removeElements`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`reverseKGroup`的作用范围，也就是将`head`作为参数传递给`reverseKGroup(head, val)`作用的范围。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gcw39mcqbnj31k606kgm1.jpg)

    在这里设定$k = 6$，当把`head`作为参数传递给递归函数作`removeElements`用后，根据定义的递归函数功能，这个函数就完成删除链表中所有值为$6$的节点，并返回删除值为$6$的链表后的链表头节点。

    <img src="https://tva1.sinaimg.cn/large/00831rSTly1gcw3ecip1mj314k052wer.jpg" style="zoom: 50%;" />

2.  **寻找递归函数退出条件和返回值**

    当链表中不存在节点的时候，不需要删除任何节点，就直接返回。

    ```cpp
    if(!head)
        return nullptr;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩减递归函数的规模，所以要将链表拆分为两个部分，由于要判断链表中的每个值是否等于`val`，所以要将链表分成长度分别为$1$和长度为$k-1$的两个链表，其中$k$是原链表的长度。假设两部分链表的头节点分别是`head`和`newHead`。当函数缩减规模的时候，即将`newHead`作为参数传递给`removeElements`，其中用图表示如下：

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323063910.png)

    当`removeElements(newHead, val)`执行完后，根据我们定义的递归的功能，即删除了上图绿色阴影中值为`val`的节点，整个链表变成如下图。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323064820.png)

    这时要判断当前节点`head->val`的值是否等于`val`，在这里就要分情况讨论了：

    -   如果`head->val != val`，表示的是不需要删除当前节点。为了将`head`与`retNode`串联起来，直接`head->next = retNode`即可。并且返回`head`。具体逻辑如上图所示。

    -   如果`head->val == val`，表示的是需要删除当前节点，并且直接返回`retNode`。

        用图形表示，当`head->val == 6`时，递归的情况如下：

        ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323120659.png)

        当执行完`removeElements(newHead, val)`后，根据定义的递归功能，应该删除`newHead`后所有值为$6$的节点，当执行完`removeElements(newHead, val)`后，整体的链表结构如下。

        ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323120443.png)

        为了删除值为$6$的节点，所以使用`delete head`即可，并且直接返回`retNode`即可。

    整体的代码逻辑如下：

    ``` cpp
    ListNode* newHead = head->next;
    //将链表分割成两个部分
    head->next = nullptr;
    ListNode* retNode = removeElements(newHead, val);
    if(head->val == val)
    {
        delete head;
        return retNode;
    }
    else
    {
        head->next = retNode;
        return head;
    }
    ```
```

4.  **完整代码**

    ```cpp
    class Solution {
    public:
        ListNode* removeElements(ListNode* head, int val) {
            if(!head)
                return head;
            ListNode* newHead = head->next;
            //断开为两个链表
            head->next = nullptr;
            //newHead 链表部分已经删除值为val的节点
            ListNode* retNode = removeElements(newHead, val);
            if(head->val == val){
                delete head;
                return retNode;
            }
            else{
                head->next = retNode;
                return head;
            }
        }
    };
```

## [83. 删除排序链表中的重复元素](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list/)

>   给定一个排序链表，删除所有重复的元素，使得每个元素只出现一次。
>
>   示例 1:
>
>   输入: 1->1->2
>   输出: 1->2
>   示例 2:
>
>   输入: 1->1->2->3->3
>   输出: 1->2->3

1.  **定义递归函数`ListNode* deleteDuplicates(ListNode* head)`功能以及返回值**

    首先定义递归函数的功能是删除链表中值重复的元素，使重复的元素只出现一次。根据定义的递归函数功能，首先将`deleteDuplicates`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`deleteDuplicates`的作用范围，也就是将`head`作为参数传递给`deleteDuplicates(head)`作用的范围。

    ![](/Users/mengxiangpeng/Documents/blog/HexoBlog-Butterfly-BackUp/source/_posts/fucking-leetcode/01-链表递归专题.assets/00831rSTly1gd3r6br4tlj311u05maa9.jpg)

    当把`head`作为参数传递给递归函数`deleteDuplicates`作用后，根据定义的递归函数功能，这个函数就完成了整个链表的翻转，也就是期望的最终答案。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323122819.png" style="zoom:50%;" />

2.  **寻找递归函数退出条件和返回值**

    当链表中不存在节点或者只有一个节点的时候，直接退出，因为一个节点不存在重复的节点。

    ```cpp
    if(!head || !head->next)
        return head;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    为了缩减递归函数的规模，所以要将链表拆分为两个部分，具体怎么拆分呢？这直接影响递归函数的编写。在这里我们将所有重复的节点看成是一部分，剩下的所有节点是另外一部分。具体解释一下。

    这里将重复的节点看成是一组，重复节点的个数可能是$1$或者更多。这里将原链表按照如下的方式进行划分为两个链表。也就是找到第一个和`head->val`值不同的节点，作为`newHead`。当缩小递归函数的范围的时候，即将`newHead`作为参数传入到`deleteDuplicates`函数中，其中递归函数的作用范围表示成图。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323130416.png)

    当递归函数`ListNode* retNode = deleteDuplicates(newHead)`执行完成后，根据递归函数的功能，这样就完成图中绿色阴影部分删除重复的节点。这样除前面重复的节点没有删除，后其余部分都符合题目要求，整个链表变成如下这个样子。

    <img src="https://tva1.sinaimg.cn/large/00831rSTly1gd3znjd91ej30uc07yjrv.jpg" style="zoom:50%;" />

    这时只要将重复的节点删除，**只保留一个重复的节点**。为了和已经完成的部分`retNode`连接起来，所以删除后的节点的`next`指针指向`retNode`。

    将上述过程翻译成代码：

    ```cpp
    ListNode* newHead = head->next;
    while(newHead && newHead->val == head->val)
    {
        ListNode* next = newHead->next;
        delete newHead;
        newHead = next;
    }
    //将链表分为两个部分。·
    head->next = nullptr;
    ListNode* retNode = deleteDuplicates(newHead);
    
    head->next = retNode;
    ```

4.  **递归代码**

    ```cpp
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            //递归退出条件
            if(!head || !head->next)
                return head;
    
            ListNode* newHead = head->next;
            while(newHead && newHead->val == head->val)
            {
                ListNode* next = newHead->next;
                delete newHead;
                newHead = next;
            }
            //将链表分为两个部分。
            head->next = nullptr;
            //对 newHead部分按照递归函数定义的功能删除重复节点，且只保留一个
            ListNode* retNode = deleteDuplicates(newHead);
            //链接链表的两个部分
            head->next = retNode;
            return head;
        }
    };
    ```

## [82. 删除排序链表中的重复元素 II](https://leetcode-cn.com/problems/remove-duplicates-from-sorted-list-ii/)

>   给定一个排序链表，删除所有含有重复数字的节点，只保留原始链表中 没有重复出现 的数字。
>
>   示例 1:
>
>   输入: 1->2->3->3->4->4->5
>   输出: 1->2->5
>   示例 2:
>
>   输入: 1->1->1->2->3
>   输出: 2->3

1.  **定义递归函数`ListNode* deleteDuplicates(ListNode* head)`功能以及返回值**

    首先定义递归函数的功能是删除链表中值重复的所有元素，也就是不保留任何值重复的元素。根据定义的递归函数功能，首先将`deleteDuplicates`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`deleteDuplicates`的作用范围，也就是将`head`作为参数传递给`deleteDuplicates(head)`作用的范围。

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323184315.png)

    当把`head`作为参数传递给递归函数`deleteDuplicates`作用后，根据定义的递归函数功能，这个函数就完成了整个链表的翻转，也就是期望的最终答案。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323184654.png" style="zoom:50%;" />

2.  **寻找递归函数退出条件和返回值**

    当链表中不存在节点或者只有一个节点的时候，直接退出，因为一个节点不存在重复的节点。

    ```cpp
    if(!head || !head->next)
        return head;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    在这里还是要思考如何减少递归函数的作用范围呢，仿照上面题的策略，将链表中的节点的值分成两组讨论，一组是只出现一次的节点的值；另外一组是链表中值重复的一组。

    -   如果值没有重复，则不用删除，链表的剩下部分头节点为`newHead`，删除从`newHead`开始链表的重复节点仍然是一个删除链表中重复节点的过程，依然可以直接使用`deleteDuplicates(newHead)`完成剩余的部分，使用图表示如下图所示。

        ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323190109.png)

        当执行完`deleteDuplicates(newHead)`后，整个链表变成如下结构，再将两个部分连接起来。

        <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323191339.png" style="zoom:50%;" />

        当不存在重复节点时的代码逻辑如下：

        ```cpp
        if(head->val != head->next->val){//链表中至少存在两个节点，所以可以直接访问head->next并不为空
            head->next = deleteDuplicates(head->next);
            return head;
        }   
        ```

    -   如果出现重复的节点就需要将`newHead`定位到下一个大于待删除的节点，再将所有值相同的节点全部释放掉。链表`newHead`部分仍然可以看作是删除值重复节点的过程，可以调用`deleteDuplicates(newHead)`完成剩下部分的操作。

        ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323195350.png)

        存在重复节点时的代码逻辑如下：

        ```cpp
        if(head->val == head->next->val)
        {
            //定位到newHead节点
            ListNode* newHead = head;
            int val = head->val;
           	while(newHead && newHead->val == val)
            {
                ListNode* next = newHead->next;
                delete newHead;
                newHead = next;
            }
            //将 newHead 后的链表删除重复节点，并将删除后的头节点返回
            return deleteDuplicates(newHead);
        }
        ```

4.  **递归代码**

    ```cpp
    class Solution {
    public:
        ListNode* deleteDuplicates(ListNode* head) {
            //递归退出条件，当链表长度小于2的时候
            if(!head || !head->next)
                return head;
            if(head->val != head->next->val){
                head->next = deleteDuplicates(head->next);
                return head;
            }
            else{
                ListNode* newHead = head;
                int val = head->val;
                while(newHead && newHead->val == val)
                {
                    ListNode* next = newHead->next;
                    delete newHead;
                    newHead = next;
                }
                return deleteDuplicates(newHead);
            }
        }
    };
    ```

## [19. 删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)

>   给定一个链表，删除链表的倒数第 n 个节点，并且返回链表的头结点。
>
>   示例：
>
>   给定一个链表: 1->2->3->4->5, 和 n = 2.
>
>   当删除了倒数第二个节点后，链表变为 1->2->3->5.
>

对于这道题，拿到手里来，是不是没有任何思路。但是只要记得上面的套路，这道题依然可以用递归解决。问题的难点是如何使用递归定位到倒数第$n$个节点。以倒数第$n$个节点作为分隔节点，将链表分成两个部分。接下来还是三步走。

1.  **定义递归函数`ListNode* removeNthFromEnd(ListNode* head, int n)`功能以及返回值**

    根据定义的递归函数功能，首先将`removeNthFromEnd`的作用范围放在整个链表，图中有绿色的阴影部分代表递归函数`reverseList`的作用范围，也就是将`head`作为参数传递给`removeNthFromEnd(head, n)`作用的范围。

    ![](https://tva1.sinaimg.cn/large/00831rSTly1gd2x4tff4nj318e0620u7.jpg)

    当把`head`作为参数传递给递归函数`removeNthFromEnd`作用后，根据定义的递归函数功能，这个函数就完成了删除倒数第n个节点，也就是期望的最终答案。

    <img src="https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323210439.png" style="zoom:50%;" />

2.  **寻找递归函数退出条件和返回值**

    当链表中没有节点的时候，不能进行删除操作，就可以直接返回。

    ```cpp
    if(!head)
        return head;
    ```

3.  **缩小递归函数的规模，寻求层与层之间的关系**

    这里我们似乎找不到任何递归函数如何缩减规模的策略，因为这个题，很难通过递归找到倒数第$n$个节点。在这里我选择通过介绍递归的原理，来讲解这个题。

    我们定义整个链表的尾节点为第$0$个节点，倒数第一个为$1$个节点，那么问题来了，我们如何通过递归定位到最后一个节点呢？

    因为最后一个节点有特殊标记`nullptr`，所以通过递归遍历整个链表，直到定位到`nullptr`节点，标记此时的标志为$th \;= \;0$。那么在递归到`nullptr`的过程中，这个过程叫做递。

    当在归的时候，每次归的时候将`th++`，这样当`th == n`时候，就可以删除该节点了。

    无图无真相：

    ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200323213633.png)

4.  **完成代码**

    ```cpp
    class Solution {
        int th;
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            
            if(head == nullptr)
            {
                th = 0;
                return nullptr;
            }
            //递的过程
            head->next = removeNthFromEnd(head->next, n);
            //每次归的过程，都将标识位做一次加加操作
            th++;
            if(th == n)
            {
                ListNode* next = head->next;
                delete head;
                return next;
            }
            return head;
        }
    };
    ```

#其他类型

## [430. 扁平化多级双向链表](https://leetcode-cn.com/problems/flatten-a-multilevel-doubly-linked-list/)

>   当有这样一个链表，扁平化后变成下图所示的链表。
>
>   ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200408074102.png)
>
>   ![](https://cdn.jsdelivr.net/gh/jackymxp/image-bed/leetcode/20200408082602.png)

首先我们分析，能不能使用递归去解题，也就是看看在原数据中，存不存在数据规模更小的，但是和原数据规模一样的操作的，如果有，我们就可以使用递归解决。我们把链表节点$3$这里拆开，发现以节点$7$为开始的链表又是可以使用原操作进行的，所以我们可以使用递归

使用递归法解题还是按照三个步骤。

# 总结

这一篇博客中，都是使用递归解决和链表相关的问题。我们可以总结出使用递归解决链表相关问题的套路。

1.  首先定义递归函数`fun(ListNode* head, ...)`的具体功能。
2.  根据具体的语境，找到递归函数的出口。
3.  将一个链表分成两个部分，假设链表两部分的头节点分别是`head`和`newHead`，其中在`newHead`部分的链表操作和整个链表的结构是相同的，只是规模更小。也就是说当执行完`fun(newHead, …)`后，根据我们定义的递归函数的功能，链表变成了什么样子。
4.  将`fun(newHead, …)`执行后的链表的返回值与`head`部分联系起来，找到如何变换`head`部分，才能让其达到我们期望的结构。这不基本是递归函数的难点。

在上面解题的过程中，为了将递归函数的功能表示的明确些。刻意将链表分成两个部分，其实有很多题是不需要明确的分成两个部分，而直接改变指针的指向会简化很多代码，但是递归的逻辑就会稍微模糊一点。

在[删除链表的倒数第N个节点](https://leetcode-cn.com/problems/remove-nth-node-from-end-of-list/)题中，讨论了递归函数的原理，什么时候递，什么时候归，通过上面一些题的例子，我们应该知道那里是递的过程，哪里是归的过程，理解了递和归的过程，对于理解递归的原理还是会有很多帮助的。

当然能写出递归函数都是在以基本功为基础的，比如定位到链表的中间节点。做链表题的时候，更重要的是对代码鲁棒性的考察，比如当链表没有节点的时候，或者只有一个节点的时候，我们写的函数能不能安全运行，又比如想判断`head->val`的值，首先我们要保证`head != nullptr`，所以在写代码的时候，一般写成`if(head && head->val == val)`等，这些都是我们需要注意的。