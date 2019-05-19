---
layout: page
title: 자료구조 (Data Structure)
comments: false
categories: Programming
---

* content
{:toc}

# 자료구조 (Data Structure)

컴퓨터가 데이터를 어떻게 표현하는지, 어떻게 효율적으로 저장하고 처리할 것인지를 생각하고 구현하는 기초 이론.

______

## 1) 단순구조

프로그래밍 언어의 정수형 실수형 문자 문자열 등의 데이터 타입에 해당하는 단순 구조.

ex) C의 int, float, char...

___

## 2) 선형구조

- 리스트(List)란 데이터를 나열한 목록을 의미하고, 선형 리스트는 이렇게 나열한 원소들이 순서(순차)를 가지고 있는 형태이다. 원소들의 순서와 메모리의 순서가 일치한다. 프로그래밍으로 선형리스트를 대표적으로 표현하는 방법은 **배열(Array)**이 있다.

- 원소의 순서를 표시할 필요가 없고, 인덱스 번호를 이용해 특정 원소에 쉽게 엑세스가 가능하다. 구현하기도 쉽고 간단하며, 데이터를 탐색하는데 걸리는 시간도 가장 빠르다.

- 단점으로는 원소 삽입,삭제시 앞 뒤 원소들을 밀어내거나 당길 필요가 있음으로 연산이 많아진다. 따라서 **삽입 삭제가 많은 프로그램에서 순차 자료구조는 비효율적**이다.

- **관리할 데이터에 순서가 있고 신규 데이터의 추가 삭제가 자주 발생하지 않는다면 선형 리스트로 구현하는게 좋다**

  ````c
  // C언어 동적할당 이용해 배열 삽입 삭제의 함수를 만들었다.
  // 배열의 원소가 많아지면 많아질수록 삽입,삭제시 연산이 많아지는걸 확인할수 있다.
  // 메모리 크기가 고정 되있기 때문에 삽입,삭제 할때마다 계속해서 할당, 해제가 필요함
  #include <stdio.h>
  #include <stdlib.h> // malloc, free 사용
  
  void print_arr(int **arr,int *arr_size) {
      for(int i=0;i<*arr_size;i++) {
          printf("%d ",(*arr)[i]);
      }
  }
  
  void insert_arr(int **arr,int *arr_size, int value) {
      int* temp = (int*)malloc(sizeof(int) * (*arr_size+1));
      
      for(int i=0;i<*arr_size;i++) {
          *(temp+i) = (*arr)[i];
      }
      
      *(temp+*arr_size) = value;
      free(*arr);
      *arr = temp;
      *arr_size+=1;
  }
  
  void delete_arr(int **arr,int *arr_size, int position) {
      int* temp = (int*)malloc(sizeof(int) * (*arr_size-1));
      
      for(int i=0;i<position;i++) {
          *(temp+i) = (*arr)[i];
      }
      for(int i=position;i<*arr_size-1;i++) {
          *(temp+i) = (*arr)[i+1];
      }
      free(*arr);
      *arr = temp;
      *arr_size-=1;
  }
  
  int main(void) {
      
      // 한칸 짜리 int형 배열 동적 할당
      int *arr = (int*)malloc(sizeof(int) * 1);
      int arr_size = 1;
      arr[0] = 0;
      
      insert_arr(&arr,&arr_size,1); // 배열에 value 1을 추가
      insert_arr(&arr,&arr_size,3);
      insert_arr(&arr,&arr_size,5);
      print_arr(&arr,&arr_size);
      
      printf("\n");
      delete_arr(&arr,&arr_size,2); // arr[2]를 삭제
      print_arr(&arr,&arr_size);
      
      free(arr);
      return 0;
  }
  
  ````

  

- 다항식의 순차 자료 구조 표현 

  4x^3 + 3x^2 + 4 이라는 다항식을 선형구조에서 표현한다 했을때, 배열의 인덱스는 **지수**를, 배열 원소에는 **항의 값**을 저장해준다. 

  ex) arr[0] = 4,   arr[1] = 3,   arr[2] = 0,   arr[3] = 4

  

- 희소 다항식의 순차 자료 구조 표현

  4x^1000 + x + 4 라는 다항식을 위와 같이 배열의 인덱스를 지수로 표현하면 998개의 배열 공간이 낭비된다. 따라서 이와 같은 희소 다항식의 경우는 **2차원 배열에서 <지수,계수>쌍을 저장**하는 경우가 좋다

  ex) arr 0 0 = 1000	arr 0 1 = 3

  ​      arr 1 0 = 1	      arr 1 1 = 1

  ​      arr 2 0 = 0	      arr 2 1 = 4

  

- 행렬의 순차 자료 구조 표현

  행의 개수가m 열의 개수가n인 m x n 행렬을 표현할때는 일반적으로 2차원 배열을 사용하면 된다.

  

- 희소 행렬의 순차 자료 구조 표현

  원소의 대부분이 0 (혹은 특정한 같은 숫자)인 경우, 2차원 배열으로 행렬을 만들면 필요없는 공간이 많아진다

  ex) // 4 x 6 행렬에서 20칸의 공간이 필요없는 공간

  0 0 0 0 0 0

  6 0 0 0 4 0

  0 0 0 0 0 0

  0 3 0 0 0 1

  

  arr 0 0 = 4(전체 행 개수),   arr 0 1 = 7(전체 열 개수),   arr 0 2 = 4(원소 개수)

  arr 1 0 = 1,   arr 1 1 = 0,   arr 1 2 = 6

  arr 2 0 = 1,   arr 2 1 = 4,   arr 2 2 = 4 ….

  

_____

## 3) 비선형구조

- 각 원소가 **다음 원소의 주소를 갖고있어 순서**가 연결되는 방식이다. 선형 리스트(순차 자료구조)가 가지고 있는 메모리 사용의 비효율성, 삽입 삭제 연산의 오버헤드 문제에서 자유롭다.

- 원소는 데이터(data field)와 다음 원소의 주소(link field)를 가지고 있으며 이러한 단위 구조를 **노드(node)**라고 한다. 데이터는 하나일수도 복수일수도 있다.

  

- 자유 공간 리스트

  연결 리스트에서 삽입 연산을 위해서는 공백 노드를 가져오고 삭제된 노드에 대한 메모리 해제도 행해야만 한다.

  만약에 완전히 새로운 원소가 아닌 미리 노드 구조로 만들어놓을수 있는 원소를 **자유 공간 리스트(free space list)** 형태로 만들어 놓고 이 노드를 사용할때는 메인 리스트에 할당, 사용하지 않을때는 자유공간 리스트에 반환하며 메모리 관리를 효율적으로 행할수 있다.


  ![linkedlist](https://github.com/songwonjun08/songwonjun08.github.io/blob/master/images/linkedlist.jpeg?raw=true){: width="100%" height="100%"}

  

- 단순 연결 리스트(singly linked list)

  노드가 하나의 링크 필드에 의해서 다음 노드와 연결되는 구조를 가진 연결리스트

  

- 원형 연결 리스트(circular linked list)

  단순 연결 리스트에서 마지막 노드가 리스트의 첫번째 노드를 가리키게 하여 원형 구조로만든 연결리스트

  

- 이중 연결 리스트(doubly linked list)

  링크를 두개 만들어 양쪽 방향 어느쪽으로 순회할수 있도록 노드를 연결한 리스트.

  

- 이중 원형 연결 리스트(doubly circular linked list)

  이중 연결 리스트에서 마지막 노드가 리스트의 첫번째 노드를 가리키게 하여 원형구조로 만든 연결 리스트



````C
// 단순 연결 리스트 예제
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct ListNode {
    char data[10];
    struct ListNode* link;
} listNode;

typedef struct {
    listNode* head;
} linkedList_h;

linkedList_h* createLinkedList_h(void) {
    linkedList_h* L;
    L = (linkedList_h*)malloc(sizeof(linkedList_h));
    L -> head = NULL;
    return L;
}

void addLastNode(linkedList_h* L, char* x) {
    listNode* newNode;
    listNode* p;
    newNode = (listNode*)malloc(sizeof(listNode));
    strcpy(newNode->data, x);
    newNode->link = NULL;
    if(L->head == NULL) {
        L->head = newNode;
        return;
    }
    p = L->head;
    while(p->link != NULL) {
        p = p->link;
    }
    p->link = newNode;
}

void deleteLastNode(linkedList_h *L) {
    listNode* previous;
    listNode* current;
    if(L->head == NULL) return;
    
    if(L->head->link == NULL) {
        free(L->head);
        L->head = NULL;
        return;
    }
    else {
        previous = L->head;
        current = L->head->link;
        while(current->link != NULL) {
            previous = current;
            current = current->link;
        }
        free(current);
        previous->link = NULL;
    }
}

void freeLinkedList_h(linkedList_h* L) {
    listNode *p;
    while(L->head != NULL) {
        p = L->head;
        L->head = L->head->link;
        free(p);
        p = NULL;
    }
}

void printList(linkedList_h* L) {
    listNode *p;
    printf("L = (");
    p = L->head;
    while(p != NULL) {
        printf("%s", p->data);
        p = p->link;
        if(p != NULL) {
            printf(", ");
        }
    }
    printf(") \n");
}

int main(int argc, const char * argv[]) {
    linkedList_h* L;
    L = createLinkedList_h();
    printf("공백 리스트 생성\n");
    printList(L);
    
    printf("리스트에 노드 추가\n");
    addLastNode(L, "월");
    addLastNode(L, "수");
    addLastNode(L, "금");
    printList(L);
    
    printf("리스트 마지막 노드 삭제\n");
    deleteLastNode(L);
    printList(L);
    
    
    printf("리스트 공간 해제\n");
    freeLinkedList_h(L);
    printList(L);
    
    return 0;
}
````



_____

## 4) 파일구조







____

