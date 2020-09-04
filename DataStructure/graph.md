## 数据结构图
### 基于邻接矩阵实现的图
```
class graph{
    //顶点数和边数
    private int numv,nume;
    //邻接矩阵
    private int[][]matrix;
    private int[]mark;
    private void init(int n){
        int i;
        numv=n;
        mark=new int[n];
        matrix=new int[n][n];
    }
    graph(int n){init(n);}
    int n(){return numv;}
    int e(){return nume;}
    int first(int v){
        for (int i = 0; i < numv; i++) {
            if(matrix[v][i]!=0)return i;
        }
        return numv;
    }
    int next(int v,int w){
        for (int i = w+1; i < numv; i++) {
            if(matrix[v][i]!=0)return i;
        }
        return numv;
    }
    void setEdge(int v1,int v2,int w){
        if(matrix[v1][v2]==0)nume++;
        matrix[v1][v2]=w;
    }
    void delEdge(int v1,int v2){
        if(matrix[v1][v2]!=0)nume--;
        matrix[v1][v2]=0;
    }
    boolean isEdge(int i,int j){return matrix[i][j]!=0;}
    int weight(int v1,int v2){return matrix[v1][v2];}
    int getMark(int v){return mark[v];}
    void setMark(int v,int val){mark[v]=val;}
    //调试用，用来输入邻接矩阵的内容
    void show(){
        for (int i = 0; i < numv; i++) {
            System.out.println(Arrays.toString(matrix[i]));
        }
    }
    //从顶点i开始的深度优先搜索
    void dfs(int i){
        if(getMark(i)==0){
            //对顶点的操作
            //System.out.println(i);
            setMark(i,1);
        }
        int j=first(i);
        while(j!=n()){
            if(getMark(j)==0){
                //对边的操作
                //System.out.println(weight(i,j));
                dfs(j);
            }
            j=next(i,j);
        }
    }
    //从顶点i开始的广度优先搜索
    void bfs(int i){
        Queue<Integer>p=new LinkedList<>();
        p.add(i);
        int k;
        while(p.size()>0){
            k=p.remove();
            //访问点
            //System.out.println(k);
            setMark(k,1);
            int j=first(k);
            while(j!=n()){
                //访问边
                //int[]a={k,j};
                //System.out.println(Arrays.toString(a));
                if(getMark(j)==0){
                    p.add(j);
                    setMark(j,1);
                }
                j=next(k,j);
            }
        }
    }
    //拓朴排序,使用深度优先排序，但是最后要逆序
    void topsort(int i){
        if(getMark(i)==0)setMark(i,1);
        int j=first(i);
        while(j!=n()){
            if(getMark(j)==0)topsort(j);
            j=next(i,j);
        }
        System.out.println(i);
    }
}
```
### 基于邻接表实现的图
```
class edge implements Comparable<edge>{
    //顶点和权值
    private int v,w;
    edge(){v=-1;w=-1;}
    edge(int v,int w){this.v=v;this.w=w;}
    int vertex(){return v;}
    int weight(){return w;}

    @Override
    public int compareTo(edge temp) {
        return this.v-temp.v;
    }
}
class graph1{
    private int numv,nume;
    private int[]mark;
    private List<List<edge>>vertex;
    private void init(int n){
        numv=n;
        nume=0;
        mark=new int[n];
        vertex=new ArrayList<>();
        for (int i = 0; i < n; i++) {
            vertex.add(new ArrayList<edge>());
        }
    }
    graph1(int n){init(n);}
    int getMark(int v){return mark[v];}
    void setMark(int v,int val){mark[v]=val;}
    int n(){return numv;}
    int e(){return nume;}
    int first(int v){
        if(vertex.get(v).size()==0)return numv;
        return vertex.get(v).get(0).vertex();
    }
    //返回以i为起点，返回边(i,j)的下一条边
    int next(int i,int j){
        int temp=Collections.binarySearch(vertex.get(i),new edge(j,0));
        if(temp>=0&&temp+1<vertex.get(i).size())return vertex.get(i).get(temp+1).vertex();
        return numv;
    }
    boolean isEdge(int i,int j){
        return Collections.binarySearch(vertex.get(i),new edge(j,0))>=0;
    }
    //设置边，主要用于增加边，使用二分查找
    void setEdge(int i,int j,int w){
        int temp=Collections.binarySearch(vertex.get(i),new edge(j,0));
        if(temp>=0)vertex.get(i).set(temp,new edge(j,w));
        else{
            nume++;
            temp=-temp-1;
            vertex.get(i).add(temp,new edge(j,w));
        }
    }
    void delEdge(int i,int j){
        int temp=Collections.binarySearch(vertex.get(i),new edge(j,0));
        if(temp>=0){
            vertex.get(i).remove(temp);
        }
    }
    //获取边(i,j)的权值
    int weight(int i,int j){
        int temp=Collections.binarySearch(vertex.get(i),new edge(j,0));
        if(temp>=0)return vertex.get(i).get(temp).weight();
        else return 0;
    }
    //调试用，输出整个邻接表的内容
    void show(){
        System.out.println(nume);
        System.out.println(numv);
        for (int i = 0; i < numv; i++) {
            for (int j = 0; j < vertex.get(i).size(); j++) {
                System.out.print(vertex.get(i).get(j).vertex());
                System.out.print(",");
                System.out.print(vertex.get(i).get(j).weight());
                System.out.print("   ");
            }
            System.out.println();
        }
    }
    //从顶点i开始深度优先搜索
    void dfs(int i){
        if(getMark(i)==0){
            //对顶点的操作
            //System.out.println(i);
            setMark(i,1);
        }
        for (int j = 0; j < vertex.get(i).size(); j++) {
            if(getMark(vertex.get(i).get(j).vertex())==0){
                //对边的操作
                //System.out.println(weight(i,vertex.get(i).get(j).vertex()));
                dfs(vertex.get(i).get(j).vertex());
            }
        }
    }
    //从顶点k开始广度优先搜索, 对p进行初始化要把初始点加进去
    void bfs(int k){
        Queue<Integer>p=new LinkedList<>();
        p.add(k);
        while(p.size()>0){
            int i=p.remove();
            setMark(i,1);
            //访问点
            //System.out.println(i);
            for (int j = 0; j < vertex.get(i).size(); j++) {
                //访问边,这里与i相邻的点是vertex.get(i).get(j).vertex()，别搞错成j
                //int[]a={i,vertex.get(i).get(j).vertex()};
                //System.out.println(Arrays.toString(a));
                if(getMark(vertex.get(i).get(j).vertex())==0){
                    p.add(vertex.get(i).get(j).vertex());
                    setMark(vertex.get(i).get(j).vertex(),1);
                }
            }
        }
    }
    ////拓朴排序,使用深度优先排序，但是最后要逆序
    void topsort(int i){
        if(getMark(i)==0)setMark(i,1);
        for (int j = 0; j < vertex.get(i).size(); j++) {
            if(getMark(vertex.get(i).get(j).vertex())==0){
                topsort(vertex.get(i).get(j).vertex());
            }
        }
        System.out.println(i);
    }
}
```
