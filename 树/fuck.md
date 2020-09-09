```
class Solution {
    public int minCameraCover(TreeNode root) {
        int[]a={0,0};
        dfs(root,a);
        return a[0];
    }
    private boolean dfs(TreeNode root,int[] i){
        boolean result=false;
        boolean l=false,r=false;
        if(root.left!=null){
            if(isLeaf(root.left))result=true;
            else l=dfs(root.left,i);
        }
        if(root.right!=null){
            if(isLeaf(root.right))result=true;
            else r=dfs(root.right,i);
        }
        if(result){
            i[0]++;
            return true;
        }
        if(!l&&root.right==null)return false;
        if(!r&&root.left==null)return false;
        if(!l&&!r){
            i[0]++;
            return true;
        }
        return false;
    }
    private boolean isLeaf(TreeNode root){
        return root.left == null && root.right == null;
    }
}
```
