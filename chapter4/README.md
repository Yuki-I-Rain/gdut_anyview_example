# 第四章
* 1. [DC04PE04 哈希表中关键字的有序输出](#DC04PE04)
* 2. [DC04PE15 链地址哈希表的构造](#DC04PE15)
* 3. [DC04PE30 计算链地址哈希表中的冲突次数](#DC04PE30)
* 4. [DC04PE35 在链地址哈希表中查找关键字，并记录查找过程中发生的冲突次数](#DC04PE35)

##  1. <a name='DC04PE04'></a>DC04PE04  哈希表中关键字的有序输出
```C
void PrintKeys(HashTable ht, void(*print)(StrKeyType)){
  int index, tempIndex;
  for (char c = 'A' ; c <= 'Z'  ; c++) {//按字母顺序从A到Z
    index = (c - 'A') % ht.size;//哈希函数H(key)为关键字的首字母在字母表中的序号，用除留余数法
    tempIndex = index;
    while(ht.rcd[index].tag != UNSUCCESS)//情况为SUCCESS或DUPLICATE时进入循环
    {
      if(ht.rcd[index].tag == SUCCESS && ht.rcd[index].key[0] == c)
        print(ht.rcd[index].key);//输出关键字
      index = (index + 1) % ht.size;//线性探测
      if(tempIndex==index)
        break;
    }
  }
}
```
##  2. <a name='DC04PE15'></a>DC04PE15  链地址哈希表的构造
```C
int BuildHashTab(ChainHashTab &H, int n, HKeyType es[])
{  
    int index;//在哈希表中的索引
    int i=0;//遍历es中的记录
    while(i<n)
    {
      //1.先创建和初始化结点
      HLink pNode = (HLink)malloc(sizeof(HNode));
      //if(pNode==nullptr)
      //  return EOVERFLOW;
      pNode->data = es[i];
      pNode->next = nullptr;
      //2.找到关键字对应的索引位置
      index = Hash(H, es[i]);
      //3.判断Hash表相应位置是否已经有相同关键字存在
      HLink p=H.rcd[index];
      if(p!=nullptr)
      {
        do
        {
          if(p->data==es[i])//已经存在此关键字
          {
            goto nloop;
          }
        }while(Collision(H, p)==SUCCESS);
      }
      //4.若没有该关键字，则放入该关键字
      pNode->next=H.rcd[index];
      H.rcd[index]=pNode;
nloop: i++;
    }
    return SUCCESS;
}
```
##  3. <a name='DC04PE30'></a>DC04PE30  计算链地址哈希表中的冲突次数
```C
int countConflics(LHashTable H) {
    if (H.rcd == NULL || H.size <= 0) return 0;

    int conflicts = 0;

    for (int i = 0; i < H.size; i++) {
        Node *p = H.rcd[i];
        int len = 0;

        while (p != NULL) {
            len++;
            p = p->next;
        }

        if (len > 1) {
            conflicts += (len - 1);
        }
    }

    return conflicts;
}
```
##  4. <a name='DC04PE35'></a>DC04PE35  在链地址哈希表中查找关键字，并记录查找过程中发生的冲突次数
```C
Node* searchLHash(LHashTable H, KeyType key, int &c) {
    c = 0;
    if (H.rcd == NULL || H.size <= 0 || H.hash == NULL) return NULL;

    int index = H.hash(key, H.size);  // 计算哈希地址
    if (index < 0 || index >= H.size) return NULL;

    Node *p = H.rcd[index];

    while (p != NULL) {
        if (p->r.key == key) {
            return p;
        }
        c++;        // 每检查一个不匹配结点计一次冲突
        p = p->next;
    }

    return NULL;    // 查找失败
}
```
