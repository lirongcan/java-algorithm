```
public class BinarySearchTree {
    //树的节点
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

    private Node root;
    private int nodecount;
    //找到想要的节点
    private Node findhelp(Node root,int k){
        if(root==null)return null;
        if(k<root.key)return findhelp(root.left,k);
        else if(k>root.key)return findhelp(root.right,k);
        else return root;
    }
    //插入函数，注意这里的形参root和实参root是两个不同的东西（两个一级指针），直接对形参root
    //new的话只会改变形参root的指向而不改变实参root对应的内容，因此要返回赋值
    private Node inserthelp(Node root, int key, int value){
        //二叉搜索树每次插入一个节点都是插入一个叶子节点
        if(root==null){
            root=new Node(key,value);
        }
        //对左子树进行操作时要返回并赋值左节点
        else if(key<root.key)root.left=inserthelp(root.left,key,value);
        else root.right=inserthelp(root.right,key,value);
        //返回赋值
        return root;
    }
    //找到最小值的节点并删除它
    private Node deletemin(Node root){
        //只要左子树不存在必然要删除根节点这样无论右子树存不存在都直接返回右子树就好了
        if(root.left==null)return root.right;
        else{
            //返回赋值
            root.left=deletemin(root.left);
            return root;
        }
    }
    //获取最小值
    private Node getmin(Node root){
        if(root.left==null)return root;
        else return getmin(root.left);
    }
    private Node removehelp(Node root,int key){
        //空树就返回null
        if(root==null)return null;
        //要返回赋值防止结构出问题
        else if(key<root.key)root.left=removehelp(root.left,key);
        else if(key>root.key)root.right=removehelp(root.right,key);
        else{
            //键匹配的情况
            Node temp=root;
            //左子树空的话直接接上右子树
            if(root.left==null){
                root=root.right;
            }
            //右子树空的话直接接上左子树
            else if(root.right==null){
                root=root.left;
            }
            else{
                //左右子树都不空找到右子树最小的替换当前节点然后删除右子树最小的
                Node i=getmin(root.right);
                root.value=i.value;
                root.key=i.key;
                root.right=deletemin(root.right);
            }
        }
        return root;
    }
    public BinarySearchTree(){root=null;nodecount=0;}
    private void insert(int key, int value){
        if(root==null){
            //防止形参指针改变指向要让root非空的进入递归函数inserthelp
            root=new Node(key,value);
        }
        else inserthelp(root,key,value);
        nodecount++;
    }
    public Node find(int key){
        return findhelp(root,key);
    }
    private Node remove(int key){
        Node temp=removehelp(root,key);
        //没找到元素删除的话也不用对数量减一
        if(temp!=null){nodecount--;}
        return temp;
    }
    public int size(){return nodecount;}
    //按层打印二叉树，调试用
    private void show(){
        class temp{
            private Node root;
            private int rank;
            private temp(Node root, int rank){
                this.root=root;
                this.rank=rank;
            }
        }
        List<temp>p=new LinkedList<>();
        p.add(new temp(root,0));
        int i=-1;
        List<List<Integer>>l=new ArrayList<>();
        while(p.size()>0){
            temp y=p.remove(0);
            if(y.root.left!=null)p.add(new temp(y.root.left,y.rank+1));
            else if(y.root.key!=-1)p.add(new temp(new Node(-1,0),y.rank+1));
            if(y.root.right!=null)p.add(new temp(y.root.right,y.rank+1));
            else if(y.root.key!=-1)p.add(new temp(new Node(-1,0),y.rank+1));
            if(y.rank>i){
                i++;
                l.add(new ArrayList<>());
            }
            l.get(i).add(y.root.key);
        }
        System.out.println(Arrays.deepToString(l.toArray()));
    }
    public void test(){
        insert(3,0);
        insert(1,0);
        insert(5,0);
        insert(0,0);
        insert(2,0);
        insert(3,0);
        insert(4,0);
        insert(6,0);
        remove(5);
        show();
    }
}
```
