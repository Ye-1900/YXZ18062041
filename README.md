# YXZ18062041
C++ primer homework
# 第九章  
#### 9.1  

使用list，需要在中间插入，用list效率更高。  
使用deque。只在头尾进行操作，deque效率更高  
使用vector。排序需要不断调整位置，vector支持快速随机访问，用vector更适合。  
#### 9.20 
void test920()  
{
    list<int> ilist = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};  
    deque<int> even_deq;  
    deque<int> odd_deq;  

    for (auto i : ilist) {
        if (i % 2) {
            odd_deq.push_back(i);
        }
        else {
            even_deq.push_back(i);
        }
    }
}  
#### 9.29  
vec.resize(100)，会把75个值为0的值添加到vec的结尾。  
vec.resize(10)，会把vec末尾的90个元素删除掉  
#### 9.43  
void replaceStr(string& s, string& oldVal, string& newVal)  
{  
    int old_size = oldVal.size();  
    auto iter_s = s.begin();  
    auto iter_old = oldVal.begin();  
    auto iter_new = newVal.begin();  

    for (iter_s; iter_s <= (s.end()-oldVal.size()+1); ++ iter_s) {  
        if (s.substr((iter_s-s.begin()), old_size) == oldVal) {  
            iter_s = s.erase(iter_s, iter_s+old_size);  
            iter_s = s.insert(iter_s, newVal.begin(), newVal.end());        
            advance(iter_s, oldVal.size());  
        }  
    }  
}  
void test943()  
{  
    string str = "abc thru efg thru";  
    string oldStr = "thru";  
    string newStr = "through";  
    replaceStr(str, oldStr, newStr);  
    cout << str << endl;  
}  
#### 9.52  
