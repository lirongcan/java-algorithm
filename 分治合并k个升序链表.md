## 问题描述
#### 给你一个链表数组，每个链表都已经按升序排列。请你将所有链表合并到一个升序链表中，返回合并后的链表。
```
ListNode mergeKLists(ListNode[] lists) {
        if(lists.length==0)return null;
        return merge(lists,0,lists.length-1);
    }
    ListNode merge(ListNode[]lists,int left,int right){
        if(left>=right)return lists[left];
        int mid=left+((right-left)>>>1);
        ListNode templ=merge(lists,left,mid);
        ListNode tempr=merge(lists,mid+1,right);
        ListNode temp=new ListNode();
        ListNode temptemp = temp;
        while(tempr!=null){
            while(templ!=null&&templ.val<tempr.val){
                temptemp=temptemp.next=new ListNode(templ.val);
                templ=templ.next;
            }
            temptemp=temptemp.next=new ListNode(tempr.val);
            tempr=tempr.next;
        }
        while(templ!=null){
            temptemp=temptemp.next=new ListNode(templ.val);
            templ=templ.next;
        }
        return temp.next;
    }
```
