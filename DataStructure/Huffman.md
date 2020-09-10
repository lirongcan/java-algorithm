```
public class HuffmanTree {
    //节点的顶层类
    static abstract class Node{
        abstract boolean isLeaf();
        abstract int key();
    }
    //叶子节点类
    static class Leaf extends Node{
        private int key,value;
        Leaf(int key,int val){
            this.key=key;
            this.value=val;
        }
        int key(){return key;}
        int value(){return value;}
        boolean isLeaf(){return true;}
    }
    //中间节点类
    static class InternalNode extends Node{
        private Node left,right;
        private int key;
        boolean isLeaf(){return false;}
        int key(){return key;}
        InternalNode(Node l,Node r){
            key=l.key()+r.key();
            left=l;
            right=r;
        }
        Node left(){return left;}
        void setLeft(Node l){left=l;}
        Node right(){return right;}
        void setRight(Node r){right=r;}
    }
    //用来辅助构建哈夫曼树的小顶堆
    static class Heap{
        private HuffNode[]heap;
        private int n;
        private void swap(int i,int j){
            HuffNode temp=heap[i];
            heap[i]=heap[j];
            heap[j]=temp;
        }
        private void siftdown(int pos){
            while(!isLeaf(pos)){
                int l=2*pos+1;
                int r=2*pos+2;
                if(r<n&&heap[r].key()<heap[l].key())l=r;
                if(heap[pos].key()<heap[l].key())return;
                swap(l,pos);
                pos=l;
            }
        }
        private void siftup(int pos){
            while(pos!=0&&heap[pos].key()<heap[(pos-1)/2].key()){
                swap(pos,(pos-1)/2);
                pos=(pos-1)/2;
            }
        }
        private void buildheap(){
            for (int i = n/2-1; i >= 0; i--) {
                siftdown(i);
            }
        }
        Heap(HuffNode[] h, int n){
            this.n=n;
            heap=h;
            buildheap();
        }
        boolean isLeaf(int pos){return pos>=n/2&&pos<n;}
        HuffNode remove(){
            if(n>0){
                swap(0,--n);
                if(n!=0)siftdown(0);
                return heap[n];
            }
            return null;
        }
        void insert(HuffNode i){
            heap[n]=i;
            siftup(n++);
        }
    }
    //哈夫曼树的节点
    static class HuffNode{
        private Node root;
        HuffNode(int key, int value){
            root=new Leaf(key,value);
        }
        HuffNode(HuffNode l, HuffNode r){
            root=new InternalNode(l.root,r.root);
        }
        Node root(){return root;}
        int key(){return root.key();}
    }
    //构建哈夫曼编码树的函数
    HuffNode buildHuff(HuffNode[]h){
        Heap forest=new Heap(h,h.length);
        HuffNode temp1,temp2,temp3=null;
        while(forest.n>1){
            temp1=forest.remove();
            temp2=forest.remove();
            temp3=new HuffNode(temp1,temp2);
            forest.insert(temp3);
        }
        return temp3;
    }
}
```
