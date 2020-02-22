# 建立一棵二叉搜索樹
```c++
tree* Insert(tree* root,int key){
    if (!root) {
        //root = (tree *)malloc(sizeof(tree));
        root = new tree();
        root->data = key;
    }
    else{
        if (key < root->data) {
            root->left = Insert(root->left, key);
        }
        else
            if (key > root->data) {
                root->right = Insert(root->right, key);
            }
    }
    return root;
}
```
---
# 是否完全二叉樹
```C++
bool isCompleteBST(tree* root){
    if (!root) {
        return true;
    }
    if ((root->left && !root->right) || (!root->left && root->right)) {
        return false;
    }
    if (!isCompleteBST(root->left) || !isCompleteBST(root->right)) {
        return false;
    }
    return true;
}
```
---
# 樹的遍歷
- **層次遍歷**
```C++
void Hierarchical_traversal(tree* root){
    tree* temp = NULL;
    queue<tree *> Q;
    if (!root) {
        return;
    }
    Q.push(root);
    while (!Q.empty()) {
        temp = Q.front();
        Q.pop();
        cout<<temp->data<<" ";
         if (temp->left) {
            Q.push(temp->left);
        }
        if (temp->right) {
            Q.push(temp->right);
        }
    }
}
```
- **先序遍歷**
```C++
void Preorder_traversal(AVLTree* root){
    if (root) {
        cout<<root->data<<" ";
        Preorder_traversal(root->left);
        Preorder_traversal(root->right);
    }
}

//非递归
void PreorderTraversalNotRecursion(tree* root){
    stack<tree* >nodeStack;
    tree* pointer = root;
    while (!nodeStack.empty() || pointer != NULL) {
        if (pointer != NULL) {
            cout<<pointer->data<<" ";
            if (pointer->right != NULL) {
                nodeStack.push(pointer->right);
            }
            pointer = pointer->left;
        }
        else{
            pointer = nodeStack.top();
            nodeStack.pop();
        }
    }
}
```
- **中序遍歷**
```C++
void In_order_traversal(AVLTree* root){
    if (root) {
        Preorder_traversal(root->left);
        cout<<root->data<<" ";
        Preorder_traversal(root->right);
    }
}

//非递归
void InorderTraversalNotRecursion(tree* root){
    if (root == NULL) {
        return;
    }
    stack<tree* >nodeStack;
    tree* p = root;
    nodeStack.push(root);
    while (!nodeStack.empty()) {
        if (p != NULL && p->left != NULL) {
            nodeStack.push(p->left);
            p = p->left;
        }
        else{
            p = nodeStack.top();
            nodeStack.pop();
            cout<<p->data<<" ";
            if (p != NULL && p->right != NULL) {
                nodeStack.push(p->right);
                p = p->right;
            }
            else{
                p = NULL;
            }
        }
    }
}
```
- **後序遍歷**
```C++
void Postorder_traversal(tree* root){
    if (root) {
        Postorder_traversal(root->left);
        Postorder_traversal(root->right);
        cout<<root->data<<" ";
    }
}

//非递归
void PostorderTraversalNotRecursion(tree* root){
    if (root) {
        stack<tree*> st;
        tree* p = root,*r = NULL;
        while (p || !st.empty()) {
            if (p) {
                st.push(p);
                p = p->left;
            }
            else {
                p = st.top();
                if(p->right!=NULL&&p->right != r){
                p = p->right;
            }
                else {
                    st.pop();
                    cout << p->data << " ";
                    r = p;
                    p = NULL;
                }
            }
        }
    }
}
```
---
# 求一棵完全二叉樹的左子樹結點個數
```C++
//求解最容易錯的就是最底層可能有右子樹的結點，所以需要求得最底層左子樹結點的最大值進行對比

int left_child_number(int total){
    int n = log(total) / log(2);    //除了最底層以外的層數（從0開始編號）
    int bottom = total - (pow(2, n) - 1);   //最底層的結點數，其中(pow(2, n) - 1)是除了底層以外的結點數
    int left_bottom_max = pow(2, n - 1);    //左子樹最底層的最大結點數（第0層除外）
    int left_branch_number = pow(2, n - 1) - 1;     //左子樹除了最底層的結點個數
    if (bottom >= left_bottom_max) {
        return left_bottom_max + left_branch_number;
    }
    else
        return bottom + left_branch_number;
}
```
---
# 二叉树的删除(未完善)
```c++
tree* Delete(tree* root, int key){
    
    tree* temp = NULL;
    if (root) {
        if (key < root->data) {
            root = Delete(root->left, key);
        }
        else
            if (key > root->data) {
                root = Delete(root->right, key);
            }
        else
            if (root->left && root->right) {
                temp = root->right;
                while (temp->left) {
                    temp = temp->left;
                }//find the first element in in-order traversal queue of what you want to delete
                root->data = temp->data;
                root->right = Delete(root->right, root->data);
            }
            else{
                
            }
    }
    return root;
}

```
---
# AVL Tree
### There are four situation of rotation
- **LL Rotation**
![LL](https://images0.cnblogs.com/i/497634/201403/281626153129361.jpg)
```C++
AVLTree* left_left_rotation(AVLTree* k2){
    AVLTree* k1;
    k1 = k2->left;
    k2->left = k1->right;
    k1->right = k2;
    k2->height = Max(HEIGHT(k2->left), HEIGHT(k2->right)) + 1;
    k1->height = Max(HEIGHT(k1->left), k2->height) + 1;
    return k1;
}
```
- **RR Rotation**
![RR](https://images0.cnblogs.com/i/497634/201403/281626410316969.jpg)
```C++
AVLTree* right_right_rotation(AVLTree* k1){
    AVLTree* k2;
    k2 = k1->right;
    k1->right = k2->left;
    k2->left = k1;
    k1->height = Max( HEIGHT(k1->left), HEIGHT(k1->right)) + 1;
    k2->height = Max( HEIGHT(k2->right), k1->height) + 1;
    return k2;
}

```
- **LR Rotation**
![LR](https://images0.cnblogs.com/i/497634/201403/281627088127150.jpg)
```C++
AVLTree* left_right_rotation(AVLTree* k3){
    
    k3->left = right_right_rotation(k3->left);
    return left_left_rotation(k3);
}
```
- **RL Rotation**
![RL](https://images0.cnblogs.com/i/497634/201403/281628118447060.jpg)
```C++
AVLTree* right_left_rotation(AVLTree* k1){
    
    k1->right = left_left_rotation(k1->right);
    return right_right_rotation(k1);
}
```
### Some functions
```C++
struct AVLTree {
    int data;
    int height = 0;
    AVLTree* left = NULL;
    AVLTree* right = NULL;
};

int HEIGHT(AVLTree* root){
    if (!root) {
        return -1;
    }
    return root->height;
}

int Max(int a, int b){
    return a > b ? a : b;
}
```
### Insert operation
```C++
AVLTree* Insert(AVLTree* root, int key){
    if (root == NULL) {
        root = new AVLTree();
        root->data = key;
    }
    else
        if (key < root->data) {
            root->left = Insert(root->left, key);  //插入結點後，AVLTree失去平衡，需作出相應調整
            if (HEIGHT(root->left) - HEIGHT(root->right) == 2) {
                if (key < root->left->data) {
                    root = left_left_rotation(root);
                }
                else
                    root = left_right_rotation(root);
            }
        }
    else
        if (key > root->data) {
            root->right = Insert(root->right, key);
            if (HEIGHT(root->right) - HEIGHT(root->left) == 2) {
                if (key > root->right->data) {
                    root = right_right_rotation(root);
                }
                else
                    root = right_left_rotation(root);
            }
        }
    root->height = Max(HEIGHT(root->left), HEIGHT(root->right)) + 1;
    
    return root;
}
```
---
# 小项堆
```c++
int a[1010];
void MIN_HEAPIFY(int value,int N){
    a[N] = value;
    while (N/2) {
        if (a[N/2] > value) {
            a[N] = a[N/2];
            a[N/2] = value;
        }
        N /= 2;
    }
}
```
---
