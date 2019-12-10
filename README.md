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
void test952()  
{
    string str = "2+(1+3)";  
    int result = 0;  
    bool flag = false;  
    stack<char> st;  

    for (int i = 0; i < str.size(); i++) {  
        if (str[i] == '(') {  
            flag = true;  
            continue;  
        } else if (str[i] == ')') {  
            flag = false;  
        }

        if (true == flag) {
            st.push(str[i]);
        }  
    }  

    char ch;  
    int a, b;  
    string expression;  
    while (!st.empty()) {  
        ch = st.top();  
        st.pop();  
        expression += ch;  
    }  
    a = stoi(expression.substr(0, 1));  
    string symbol = expression.substr(1, 1);  
    b = stoi(expression.substr(2, 1));  
    if (symbol == "+") {  
        result = a+b;  
    }  
    st.push(result);  
    cout << result << endl;  
}  
# 第十章  
#### 10.3  
void test1003()  
{  
    int ia[] = {1, 2, 2, 4, 3, 51, 2};  
    vector<int> ivec(ia, ia+7);  

    int sum = accumulate(ivec.begin(), ivec.end(), 0);  
    cout << sum << endl;  
}  
#### 10.15  
auto sum = [a](const int b) { return a+b; }  
####10.34  
void test1034()  
{  
    vector<int> ivec = {0, 1, 2, 3, 4, 5, 6};  

    for (auto riter = ivec.crbegin(); riter != ivec.crend(); ++ riter) {  
        cout << *riter << " ";  
    }  
    cout << endl;  
}  
#### 10.42  
void test1042()  
{  
    list<int> ilst = {0, 1, 1,2, 3, 4, 5, 6, 0, 1, 3};  
    ilst.sort();  
    ilst.unique();  

    for (auto i : ilst) {  
        cout << i << " ";  
    }  
    cout << endl;  
}  
# 第十一章  
#### 11.12  
vectoe<pair<string, int>> pvec;  
#### 11.17  
copy(v.begin(), v.end(), inserter(c, c.end()));   
// 将v中的元素拷贝到c中。使用合法，可以使用inserter将关联容器当作一个目的位置。  
copy(v.begin(), v.end(), back_inserter(c));  
// 意义同上，但是不合法，因为multiset没有push_back方法，不能调用back_inserter  
copy(c.begin(), c.end(), inserter(v, v.end()));  
// 将c中的元素拷贝到v中，使用合法，vector可以使用inserter。  
copy(c.begin(), c.end(), back_inserter(v));  
// 意义同上，合法，vector有push_back方法，可以使用back_inserter.  
#### 11.38  
void test1138()  
{  
    unordered_map<string, size_t> word_count;  
    string word;  

    while (cin >> word) {  
        ++ word_count[word];  
    }

    for (const auto i : word_count) {  
        cout << i.first << " occurs " << i.second << " times." << endl;  
    }  
}  
# 第十三章  
#### 13.12  
bool fcn (const Sales_data *trans, Sales_data accum)  
{  
    Sales_data item1 (*trans), item2 (accum);  
    return item1.isbn() != item2.isbn();  
} // 退出作用域时，对item1和item2调用析构函数。  
// 参数accum在函数结束时会调用吗？  
// 当一个对象的引用或指针离开作用域，并不会执行析构。  
#### 13.18  
class Employee  
{  
public:  
    Employee () = default;  
    Employee (const string& n) : name (n)  
    {
        employee_id = ++ increment_number;  
    }

private:  
    static int increment_number;  
    int employee_id;  
    string name;  
};  
#### 13.46  
int f();  
vector<int> vi(100);  
int? r1 = f();              // int &&r = f(); f()的返回值相当于一个常量，只能做右值引用或const引用。  
int? r2 = vi[0];            // 下标运算返回左值，所以应该用int &r = vi[0]   
int? r3 = r1;               // r1此时相当与变量，int &r3 = r1;  
int? r4 = vi[0] * f();       // 算术运算产生右值，int &&r4 = vi[0] * f();  

#### 13.49  
// Str.h文件中加：  
StrVec (StrVec&& rhs) noexcept;  
StrVec& operator= (StrVec&& rhs) noexcept;  
// StrVec.cpp中加：  
StrVec::StrVec(StrVec&& rhs) noexcept : elements (rhs.elements), first_free (rhs.first_free), cap (rhs.cap)  
{
    rhs.elements = rhs.first_free = rhs.cap = nullptr;  
}

StrVec& StrVec::operator=(StrVec&& rhs) noexcept  
{
    if (this != &rhs) {  
        free ();  
        elements = rhs.elements;  
        first_free = rhs.first_free;  
        cap = rhs.cap;  
        rhs.elements = rhs.first_free = rhs.cap = nullptr;  
    }  
    return *this;  
}  
# 第十四章  
#### 14.3  
(a) "cobble" == "stone"         // string重载的“==”  
(b) svec1[0] == svec2[0]        // string重载的“==”  
(c) svec1 == svec2              // vector重载的“==”  
(d) "svec1[0] == "stone"        // string重载的“==”  
#### 14.38  
class IsInRange  
{  
public:  
    IsInRange (std::size_t n) : sz (n) {}  
    bool operator() (const std::string& s)  
    {  
        return s.size() == sz;  
    }  
    size_t getSize() { return sz; }  
private:  

    std::size_t sz;  
};  
void test1428()  
{  
    vector<IsInRange> predicates;  
    map<size_t, size_t> len_cnt_table;  

    for (size_t i = 1; i < 11; ++i) {  
        len_cnt_table[i] = 0;  
        predicates.push_back(IsInRange(i));  
    }

    ifstream fi ("C:/Users/tutu/Documents/code/cpp_primer/ch14/storyDataFile.txt");  
    string word;  
    while (fi >> word) {  
        for (auto i : predicates) {  
            if (i(word)) {  
                ++ len_cnt_table[i.getSize()];  
            }  
        }  
    }  

    for (auto i : len_cnt_table) {  
        cout << i.first << " " << i.second << endl;  
    }  
}  
#### 14.52  
struct LongDouble {  
    LongDouble operator+ (const SmallInt&);   
};
LongDouble operator+(LongDouble&, double);    
SmallInt si;  
LongDouble ld;  
ld = si + ld;  
ld = ld + si;  
# 第十五章  
#### 15.12  
有必要，override意味着重载父类中的虚函数，final意味着禁止子类重载该虚函数。两个用法并不冲突。  
#### 15.16   
class Limit_quote : public Disc_quote  
{  
public:  
    Limit_quote() = default;  
    Limit_quote(const std::string& b, double p, std::size_t max, double disc):  
        Disc_quote(b, p, max, disc)  {   }  

    double net_price(std::size_t n) const override  
    { return n * price * (n < quantity ? 1 - discount : 1 ); }  
};  
#### 15.30  
#include <iostream>  
#include <memory>  
#include <set>  
#include "Quote.h"  

using namespace std;  

class Basket  
{  
public:  
    void add_item(const shared_ptr<Quote> &sales)  
    {  
        items.insert(sales);  
    }  
    double total_receipt (std::ostream&) const;     //   
private:  
    static bool compare(const std::shared_ptr<Quote> &lhs, const std::shared_ptr<Quote> &rhs)  
    {  
        return lhs->isbn() < rhs->isbn();  
    }  
    
    std::multiset<std::shared_ptr<Quote>, decltype(compare)*> items{compare};  
};  

double Basket::total_receipt(std::ostream &os) const  
{  
    double sum = 0.0;  

    for (auto iter = items.cbegin(); iter != items.cend(); iter=items.upper_bound(*iter))  
    {  
        sum += print_total (os, **iter, items.count(*iter));  
    }  
    os << "Total Sale: " << sum << endl;  
    return  sum;  
}  


