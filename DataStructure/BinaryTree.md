```
//普通的二叉树节点类
class SimpleBinaryTree{
    static class Node{
        //数据成员包括键值，左孩子，右孩子
        private int key, value;
        private Node left;
        private Node right;
        //两个构造函数，一个有键值初始化，一个没有
        Node(int k,int e){
            key=k;
            value =e;
            left=null;
            right=null;
        }
        Node(){
            left=null;
            right=null;
        }
        int value(){return value;}
        int key(){return key;}
        void setValue(int e){value =e;}
        void setKey(int k){key=k;}
        Node left(){return left;}
        Node right(){return right;}
        void setLeft(Node l){left=l;}
        void setRight(Node r){right=r;}
        boolean isLeaf(){ return left==null&&right==null;}
    }
}

//分离叶子和中间节点的类
class SimpleSeperateLeafAndInternalNodeTree{
    abstract static class BinaryNode{
        abstract boolean ifLeaf();
        int key,value;
        int value(){return value;}
        int key(){return key;}
        void setValue(int e){value =e;}
        void setKey(int k){key=k;}
    }
    static class Leaf extends BinaryNode{
        Leaf(int k,int v){key=k;value=v;}
        @Override
        boolean ifLeaf() {
            return true;
        }
    }
    static class InternalNode extends BinaryNode{
        private BinaryNode left,right;
        InternalNode(int k, int v){
            key=k;
            value=v;
            left=null;
            right=null;
        }
        InternalNode(){left=null;right=null;}
        BinaryNode left(){return left;}
        BinaryNode right(){return right;}
        void setLeft(BinaryNode l){left=l;}
        void setRight(BinaryNode r){right=r;}
        @Override
        boolean ifLeaf() {
            return false;
        }
    }
}
```
