# bst.h
~~~cpp
#ifndef BST_H
#define BST_H

#include <iostream>
#include <string>
#include <queue>
using namespace std;

template <class K, class E> class BST; //전방선언

template <class K, class E>
class TreeNode {
friend class BST<K,E>;
    TreeNode(K ky, E el, TreeNode<K, E> *left=0, TreeNode<K, E> *right=0) : key(ky), element(el), leftChild(left), rightChild(right) { }
private:
    TreeNode<K, E> *leftChild;
    K key;
    E element;
    TreeNode<K, E> *rightChild;
};
template <class K, class E>
class BST {
public:
    BST() { root = 0; } // empty BST
    void Insert(K &newkey, E &el) { Insert(root, newkey, el); }
    void Preorder() { Preorder(root); }
    void Inorder() { Inorder(root); }
    void Postorder() { Postorder(root); }
    void Levelorder();
private: // helper 함수들
    void Visit(TreeNode<K, E> *);
    void Insert(TreeNode<K, E>* &, K &, E &);
    void Preorder(TreeNode<K, E> *);
    void Inorder(TreeNode<K, E> *);
    void Postorder(TreeNode<K, E> *);
    
    TreeNode<K, E> *root;
};
template <class K, class E>
void BST<K, E>::Visit(TreeNode<K, E> *ptr){
    cout << ptr->key << ":" << ptr->element << " " ;
}

template <class K, class E>
void BST<K, E>::Insert(TreeNode<K, E>* &ptr, K &newkey, E &el) { 
    //Insert의 helper함수
    if (ptr==0) ptr = new TreeNode<K, E>(newkey, el);
    else if (newkey < ptr->key) Insert(ptr->leftChild, newkey, el);
    else if (newkey > ptr->key) Insert(ptr->rightChild, newkey, el);
    else ptr->element = el; // Update element
}

template <class K, class E>
void BST<K, E>::Preorder(TreeNode<K, E> *currentNode) { //Preorder의helper함수
    if (currentNode) {
        Visit(currentNode);
        Preorder(currentNode->leftChild);
    Preorder(currentNode->rightChild);
    }
}

template <class K, class E>
void BST<K, E>::Inorder(TreeNode<K, E> *currentNode) { //Inorder의helper함수
    if (currentNode) {
        Inorder(currentNode->leftChild);
        Visit(currentNode);
        Inorder(currentNode->rightChild);
    }
}

template <class K, class E>
void BST<K, E>::Postorder(TreeNode<K, E> *currentNode) { //Postorder의helper함수
    if (currentNode) {
        Postorder(currentNode->leftChild);
        Postorder(currentNode->rightChild);
        Visit(currentNode);
    }
}

template <class K, class E>
void BST<K, E>::Levelorder() {
    //이진 트리의 레벨 순서 순회
    queue<TreeNode<K, E>*> q; // 큐를 사용하자
    TreeNode<K, E> *currentNode = root;
    while(currentNode){
        Visit(currentNode);
        if(currentNode->leftChild) q.push(currentNode->leftChild);
        if(currentNode->rightChild) q.push(currentNode->rightChild);
        if(q.empty()) return;
        currentNode = q.front(); // 큐에서 꺼내자.
        q.pop();
    }
}
#endif
~~~
# labbst.cpp
~~~cpp
//C311090 심태용
#include "bst.h"
int main()
{
    BST<string, double> bst;
    string str;
    double dval;
    cout << "Enter string & double pairs:\n";
    while (cin >> str >> dval) bst.Insert(str, dval);
    cout << endl << "Preorder traversal: "; bst.Preorder();
    cout << endl << "Inorder traversal: "; bst.Inorder();
    cout << endl << "Postorder traversal: "; bst.Postorder();
    cout << endl << "Levelorder traversal: "; bst.Levelorder();
    cout << endl;
}
~~~
