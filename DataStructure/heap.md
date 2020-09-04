# 数据结构堆
## 堆是一种完全二叉树数据结构，每一次能把整个数组中的最值保留在数组的顶端
### 完整的堆实现如下
```
public class heap {//这是一个大顶堆的例子
    //堆数组h，数组最大容量maxsize，当前容量n
    private int[]h;
    private int maxsize,n;
    private void swap(int i,int j){
        int temp=h[i];
        h[i]=h[j];
        h[j]=temp;
    }
    //把小的元素往下压到合适的位置而使得父节点比孩子节点大
    private void siftdown(int pos){
        while(!isLeaf(pos)){
            int l=leftchild(pos);
            int r=rightchild(pos);
            //如果右孩子的值比左孩子的大，那就让l=r，其实就是让局部变量
            //l指向左右孩子中最大的一个
            if(r<n&&h[r]>h[l])l=r;
            //如果父节点比左右孩子最大的那个都大那就没问题
            if(h[pos]>h[l])return;
            //否则就交换父节点和那个较大的元素，即把大元素往上提，小元素往下压
            swap(pos,l);
            pos=l;
        }
    }
    //从倒数第二层也就是第一层非叶子节点
    private void buildheap(){
        for (int i = n/2-1; i >= 0; i--)siftdown(i);
    }
    public heap(int[]hh,int num,int max){h=hh;n=num;maxsize=max;buildheap();}
    //判断该节点是否是叶子
    public boolean isLeaf(int pos){return (pos>=n/2)&&(pos<n);}
    //对于一个坐标，返回它的左孩子，右孩子，父节点
    public int leftchild(int pos){return 2*pos+1;}
    public int rightchild(int pos){return 2*pos+2;}
    public int parent(int pos){return (pos-1)/2;}
    //当前堆的大小
    public int size(){return n;}
    //如果对于某个i对应的节点发生了变化，要调整堆的结构
    //向上提函数
    void siftup(int i){
        //若i不是根节点而且它的父节点的值没它大就交换它和它的父节点
        while(i!=0&&h[i]>h[(i-1)/2]){
            swap(i,(i-1)/2);
            i=(i-1)/2;
        }
    }
    //往堆中插入元素
    boolean insert(int x){
        //如果说数组已经满了那就不能再增加
        if(n<maxsize){
            //否则把新增的元素放到堆尾然后向上提
            int curr=n++;
            h[curr]=x;
            siftup(curr);
            return true;
        }
        return false;
    }
    //删除堆顶元素并返回
    int remove(){
        if(n>0){
            //交换堆顶元素和堆尾元素，然后调整堆结构
            swap(0,--n);
            if(n!=0)siftdown(0);
            return h[n];
        }
        else return 0;
    }
    //删除特定位置的元素并返回
    int remove(int pos){
        if(pos>=0&&pos<n){
            if(pos==n-1)n--;
            else{
                //交换堆尾元素和特定位置的元素
                swap(pos,--n);
                //调整堆结构，由于不知道这个元素是大是小因此要向上提，向下压
                //其实向上提和往下压只会有一个可能发生
                //若与堆尾交换后pos位置是大元素，那么只会向上提，向上提了以后上面的大元素下来也不会往下压
                //若它是个小元素，由于把他往下压后上来的是小元素，同理也不会触发向上提
                siftup(pos);
                if(n!=0)siftdown(pos);
            }
            return h[n];
        }
        return 0;
    }
}
```
