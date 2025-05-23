# kruskal.cpp
~~~cpp
#include "mstree.h"
priority_queue< Edge, vector<Edge>, Compare > PQ; //Compare 값에 따라 Max, MinHeap으로 바뀜

void kruskal() {
	Sets sets(NNODES);
	int nedges = 0;  // #edges found up to now.
	while ((nedges < NNODES - 1) && !PQ.empty()) {
		// 최소비용 간선e 선택
		Edge e = PQ.top();
		// e를 PQ에서 삭제
		PQ.pop();
		// 간선e의 각 점v1 과 v2가 어느 집합에 속했는지 Find.
		int v1root = sets.Find(e.v1);
		int v2root = sets.Find(e.v2);

		// e가 사이클을 만들지 않는다면
		if (v1root != v2root) {
			// 두 집합을 WeightedUnion, nedges 갯수 추가, 간선e 출력
			sets.WeightedUnion(v1root, v2root);
			nedges++;
			cout << e;
		}
	}
}

void ReadEdges4kruskal(istream& is) {
	Edge e;
	while (GetEdge(is, e)) PQ.push(e);
}

int main(int argc, char* argv[]) {
	ifstream is;
	if (argc == 1)
		is.open("kin.txt");
	else
		is.open(argv[1]);
	if (!is) {
		cerr << "No such input file\n";
		exit(1);
	}

	is >> NNODES;

	if (NNODES < 2) {
		cerr << "#nodes must be 2.." << endl;
		exit(1);
	}
	try {
		ReadEdges4kruskal(is);
		kruskal();
	}
	catch (char const* str) {
		cerr << "Exception: " << str << endl;
		exit(1);
	}
}
~~~

# mstree.h

~~~cpp
//C311090 심태용
#ifndef MSTREE_H
#define MSTREE_H
#include <fstream>
#include <iostream>
#include <queue>
using namespace std;

class Sets {
   public:
    Sets(int);
    void WeightedUnion(int i, int j);
    int Find(int i) { //root를 찾는 함수
        while (parent[i] >= 0) i = parent[i];
        return i;
    }
   private:
    int* parent;
    int n;
};  // 5장의 program 5.23, 5.24

Sets::Sets(int numberOfElements) {
    if (numberOfElements < 2) throw "Must have at least 2 elements.";
    n = numberOfElements;
    parent = new int[n];
    fill(parent, parent + n, -1);
}

void Sets::WeightedUnion(int i, int j) {
    int temp = parent[i] + parent[j];
    if (parent[i] > parent[j]) {  // i has fewer nodes
        parent[i] = j;
        parent[j] = temp;
    } else {  // j has fewer nodes or the same number of nodes
        parent[j] = i;
        parent[i] = temp;
    }
}
int NNODES;  // #nodes(vertices) in the graph

struct Edge {
    int v1, v2;
    double weight;
    bool operator==(Edge& e2) { return (v1 == e2.v1 && v2 == e2.v2); }
    bool operator!=(Edge& e2) { return (v1 != e2.v1 || v2 != e2.v2); }
};

ostream& operator<<(ostream& os, Edge& e) {
    os << "(" << e.v1 << "," << e.v2 << ") : " << e.weight << endl;
    return os;
}

bool GetEdge(istream& is, Edge& e) {
    // make sure that node number is 0 to NNODES-1
    is >> e.v1 >> e.v2 >> e.weight;
    if (!is.good()) return false;
    if (e.v1 < 0 || e.v1 >= NNODES || e.v2 < 0 || e.v2 >= NNODES || e.v1 == e.v2) throw "Incorrect Edge";
    if (e.v2 < e.v1) swap(e.v1, e.v2);  // low-numbered vertex first
    return true;
}

class Compare {
   public:
    bool operator()(Edge e1, Edge e2) { return e1.weight > e2.weight; }
};
#endif
~~~
