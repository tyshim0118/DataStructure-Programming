# bt.h
~~~cpp
#ifndef BT_H
#define BT_H

#include <iostream>
#include <queue>
using namespace std;

template <class T> class Tree; //전방선언

template <class T>
class TreeNode{
    friend class Tree<T>;
    TreeNode(T d, TreeNode<T> *left=0, TreeNode<T> *right=0) : data(d), leftChild(left), rightChild(right) { }
private:
    TreeNode<T> *leftChild;
    T data;
    TreeNode<T> *rightChild;
};
template <class T>
class Tree {
public:
    Tree() { root = 0; } // empty tree
    void Insert(T &value) { Insert(root, value); }
    void Preorder() { Preorder(root); }
    void Inorder() { Inorder(root); }
    void Postorder() { Postorder(root); }
    void Levelorder();
    
private: // helper 함수들
    void Visit(TreeNode<T> *);
    void Insert(TreeNode<T>* &, T &);
    void Preorder(TreeNode<T> *);
    void Inorder(TreeNode<T> *);
    void Postorder(TreeNode<T> *);
    TreeNode<T> *root;
};

template <class T>
void Tree<T>::Visit(TreeNode<T> *ptr) { 
    cout << ptr->data << " "; 
}

template <class T>
void Tree<T>::Insert(TreeNode<T>* &ptr, T &value) { 
    //Insert의helper함수
    //ptr은root
    if (ptr == 0) ptr = new TreeNode<T>(value);
    else if (value < ptr->data) Insert(ptr->leftChild, value);
    else if (value > ptr->data) Insert(ptr->rightChild, value);
    else cout << endl << "Duplicate value " << value << " ignored\n";
}

template <class T>
void Tree<T>::Preorder(TreeNode<T> *currentNode) { 
    //Preorder의helper함수
    if (currentNode) {
        Visit(currentNode);
        Preorder(currentNode->leftChild);
        Preorder(currentNode->rightChild);
    }   
}

template <class T>
void Tree<T>::Inorder(TreeNode<T> *currentNode) {
    //LVR
    if (currentNode) {
        Inorder(currentNode->leftChild);
        Visit(currentNode);
        Inorder(currentNode->rightChild);
    }
}

template <class T>
void Tree<T>::Postorder(TreeNode<T> *currentNode) {
    //LRV
    if (currentNode) {
        Postorder(currentNode->leftChild);
        Postorder(currentNode->rightChild);
        Visit(currentNode);
    }
}

template <class T>
void Tree<T>::Levelorder() {
    //이진 트리의 레벨 순서 순회
    queue<TreeNode<T>*> q; // 큐를 사용하자
    TreeNode<T> *currentNode = root;
    while(currentNode){
        Visit(currentNode);
        if(currentNode->leftChild) q.push(currentNode->leftChild);
        if(currentNode->rightChild) q.push(currentNode->rightChild);
        if(q.empty()) return;
        currentNode = q.front(); // 큐에서 꺼내자.
        q.pop();
    }
}
//..... Levelorder(교재p266 참조하되 STL 큐를 이용)를 구현하시오.
#endif
~~~
# labbt.cpp
```cpp
#include "bt.h"
int main()
{
    Tree<double> tree;
    double dval;
    cout << "Enter doubles:\n";
    while (cin >> dval)
        tree.Insert(dval);
    cout << endl << "Preorder traversal: "; tree.Preorder();
    cout << endl << "Inorder traversal: "; tree.Inorder();
    cout << endl << "Postorder traversal: "; tree.Postorder();
    cout << endl << "Levelorder traversal: "; tree.Levelorder();
    cout << endl;
}
```
