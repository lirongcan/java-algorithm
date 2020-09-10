```
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode myhead=new ListNode(0);
        myhead.next=head;
        ListNode temp1=myhead,temp2=myhead;
        int i=1;
        while(i<=n){
            temp1=temp1.next;
            i++;
        }
        while(temp1.next!=null){
            temp1=temp1.next;
            temp2=temp2.next;
        }
        temp2.next=temp2.next.next;
        if(myhead.next==null)return null;
        else return myhead.next;
    }
}
```
