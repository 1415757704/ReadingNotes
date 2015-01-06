##����##
����ڲ�����C++ Primer Plus�������棬���ȷ�Ǳ����飬���й�������ָ����½ڽ����ķǳ�������һ������ǰ�Ķദ����C++���Թ����У��ܶ����Թٶ�ϲ��������ָ����ص����⣬������֪����Щ����ָ�룿shared_ptr�����ԭ����ʲô����������Լ����һ������ָ�룬�������ɣ��ȵȡ����������ڿ���Դ��C++��Ŀʱ��Ҳ���洦��������ָ���Ӱ�ӡ���˵������ָ�벻�������Թٰ��ʵ���ģ����Ƿǳ���ʵ�ü�ֵ��

���������ڿ�����ָ��ʱ�����ıʼǣ�ϣ���ܹ�����������ָ���һЩ���š�

##Ŀ¼##
1. ����ָ�뱳������˼��
2. C++����ָ��򵥽���
3. Ϊʲô����auto_ptr��
4. unique_ptrΪ������auto_ptr��
5. ���ѡ������ָ�룿

##����##
####1. ����ָ�뱳������˼��####
����������һ���򵥵����ӣ�
```cpp
void remodel(std::string & str)
{
    std::string * ps = new std::string(str);
    ...
    if (weird_thing())
        throw exception();
    str = *ps; 
    delete ps;
    return;
}
```
�������쳣ʱ��weird_thing()����true����delete������ִ�У���˽������ڴ�й¶��
��α����������⣿���˻�˵���⻹���򵥣�ֱ����<code>throw exception();</code>֮ǰ����<code>delete ps;</code>�������ˡ��ǵģ��㱾Ӧ��ˣ������Ǻܶ��˶����������ʵ��ĵط�����delete��䣨�����������������Ǿ�delete���Ҳ���кܶ������ǰɣ��������Ҫ��һ���Ӵ�Ĺ��̽���review�����Ƿ�������Ǳ�ڵ��ڴ�й¶���⣬�Ǿ���һ�����ѣ�
��ʱ���ǻ��룺��remodel�����ĺ�����ֹ��������������ֹ���������ڳ������쳣����ֹ�������ر��������Զ���ջ�ڴ���ɾ�������ָ��psռ�ݵ��ڴ潫���ͷţ�**���psָ����ڴ�Ҳ���Զ��ͷţ��Ǹ��ж�ð���**
����֪������������������ܡ����ps��һ��������������������������ps����ʱ�Զ��ͷ���ָ����ڴ档��ps���������ڣ���ֻ��һ������ָ�룬���������������������ָ�롣�����ָ����Ƕ���������ڶ������ʱ����������������ɾ��ָ����ڴ档

������ auto_ptr��unique_ptr��shared_ptr�⼸������ָ�뱳������˼�롣�Ҽ򵥵��ܽ��¾��ǣ�**����������ָ���װΪ�����ָ�루�����϶��Ǹ�ģ�壬����Ӧ��ͬ�������͵����󣩣����������������дdelete���ɾ��ָ��ָ����ڴ�ռ䡣**

��ˣ�Ҫת��remodel()������Ӧ������3��������У� 
-  ����ͷ���memory������ָ�����ڵ�ͷ�ļ�����
- ��ָ��string��ָ���滻Ϊָ��string������ָ�����
- ɾ��delete��䡣

������ʹ��auto_ptr�޸ĸú����Ľ����
```cpp
# include <memory>
void remodel (std::string & str)
{
    std::auto_ptr<std::string> ps (new std::string(str))��
    ...
    if (weird_thing ())
        throw exception()�� 
    str = *ps�� 
    // delete ps�� NO LONGER NEEDED
    return;
}
```

####2. C++����ָ��򵥽���####
STLһ���������ṩ����������ָ�룺auto_ptr��unique_ptr��shared_ptr��weak_ptr���������ݲ����ۣ���
ģ��auto_ptr��C++98�ṩ�Ľ��������C+11�ѽ��������������ṩ���������ֽ��������Ȼ������Ȼauto_ptr��������������ʹ���˺ö��꣺ͬʱ��������ı�������֧���������ֽ��������auto_ptr����Ψһ��ѡ��

**ʹ��ע���**
- ���е�����ָ���඼��һ��explicit���캯������ָ����Ϊ����������auto_ptr����ģ��ԭ��Ϊ��
```cppp
templet<class T>
class auto_ptr {
    explicit auto_ptr(X* p = 0) ; 
    ...
};
```
��˲����Զ���ָ��ת��Ϊ����ָ����󣬱�����ʽ���ã�
```cpp
shared_ptr<double> pd; 
double *p_reg = new double;
pd = p_reg;                               // not allowed (implicit conversion)
pd = shared_ptr<double>(p_reg);           // allowed (explicit conversion)
shared_ptr<double> pshared = p_reg;       // not allowed (implicit conversion)
shared_ptr<double> pshared(p_reg);        // allowed (explicit conversion)
```
- ��ȫ����������ָ�붼Ӧ�����һ�㣺
```cpp
string vacation("I wandered lonely as a cloud.");
shared_ptr<string> pvac(&vacation);   // No
```
pvac����ʱ�����򽫰�delete��������ڷǶ��ڴ棬���Ǵ���ġ�

**ʹ�þ���**
```cpp
#include <iostream>
#include <string>
#include <memory>

class report
{
private:
    std::string str;
public:
 report(const std::string s) : str(s) {
  std::cout << "Object created.\n";
 }
 ~report() {
  std::cout << "Object deleted.\n";
 }
 void comment() const {
  std::cout << str << "\n";
 }
};

int main() {
 {
  std::auto_ptr<report> ps(new report("using auto ptr"));
  ps->comment();
 }

 {
  std::shared_ptr<report> ps(new report("using shared ptr"));
  ps->comment();
 }

 {
  std::unique_ptr<report> ps(new report("using unique ptr"));
  ps->comment();
 }
 return 0;
}
```

####3. Ϊʲô����auto_ptr��####
����������ĸ�ֵ���:
```cpp
auto_ptr< string> ps (new string ("I reigned lonely as a cloud.����;
auto_ptr<string> vocation; 
vocaticn = ps;
```
������ֵ��佫���ʲô�����أ����ps��vocation�ǳ���ָ�룬������ָ�뽫ָ��ͬһ��string�������ǲ��ܽ��ܵģ���Ϊ������ͼɾ��ͬһ���������Ρ���һ����ps����ʱ����һ����vocation����ʱ��Ҫ�����������⣬�����ж��֣�
- �����ֵ�������ʹִ֮����ơ���������ָ�뽫ָ��ͬ�Ķ������е�һ����������һ������ĸ�����ȱ�����˷ѿռ䣬��������ָ�붼δ���ô˷�����
- ��������Ȩ��ownership����������ض��Ķ���ֻ����һ������ָ���ӵ�У�����ֻ��ӵ�ж��������ָ��Ĺ��캯����ɾ���ö���Ȼ���ø�ֵ����ת������Ȩ�����������auto_ptr��uniqiie_ptr �Ĳ��ԣ���unique_ptr�Ĳ��Ը��ϸ�
- �������ܸ��ߵ�ָ�룬���������ض����������ָ���������Ϊ���ü��������磬��ֵʱ����������1����ָ�����ʱ����������1,������Ϊ0ʱ�ŵ���delete������shared_ptr���õĲ��ԡ�

��Ȼ��ͬ���Ĳ���Ҳ�����ڸ��ƹ��캯����
ÿ�ַ�����������;����Ϊ��˵Ҫ����auto_ptr�أ�
����ٸ�������˵����
```cpp
#include <iostream>
#include <string>
#include <memory>
using namespace std;

int main() {
  auto_ptr<string> films[5] =
 {
  auto_ptr<string> (new string("Fowl Balls")),
  auto_ptr<string> (new string("Duck Walks")),
  auto_ptr<string> (new string("Chicken Runs")),
  auto_ptr<string> (new string("Turkey Errors")),
  auto_ptr<string> (new string("Goose Eggs"))
 };
 auto_ptr<string> pwin;
 pwin = films[2]; // films[2] loses ownership. ������Ȩ��films[2]ת�ø�pwin����ʱfilms[2]�������ø��ַ����Ӷ���ɿ�ָ��

 cout << "The nominees for best avian baseballl film are\n";
 for(int i = 0; i < 5; ++i)
  cout << *films[i] << endl;
 cout << "The winner is " << *pwin << endl;
 cin.get();

 return 0;
}
```
�����·��ֳ�������ˣ�ԭ��������ע���Ѿ�˵�ĺ������films[2]�Ѿ��ǿ�ָ���ˣ�����������ʿ�ָ�뵱Ȼ������ˡ������������auto_ptr����shared_ptr��unique_ptr�󣬳���Ͳ��������ԭ�����£�
- ʹ��shared_ptrʱ������������Ϊshared_ptr�������ü�����pwin��films[2]��ָ��ͬһ���ڴ棬���ͷſռ�ʱ��Ϊ����Ҫ�ж����ü���ֵ�Ĵ�С��˲�����ֶ��ɾ��һ������Ĵ���
- ʹ��unique_ptrʱ���������auto_ptrһ����unique_ptrҲ��������Ȩģ�ͣ�����ʹ��unique_ptrʱ�����򲻻�ȵ����н׶α��������ڱ����������������г��ִ���
```cpp
unique_ptr<string> pwin;
pwin = films[2]; // films[2] loses ownership.
```
ָ���㷢��Ǳ�ڵ��ڴ����

�����Ϊ��Ҫ����auto_ptr��ԭ��һ�仰�ܽ���ǣ�**����Ǳ�ڵ��ڴ�������⡣**

####4. unique_ptrΪ������auto_ptr��####
���ܴ����Ϊǰ��������Ѿ�˵����unique_ptrΪ������auto_ptr��Ҳ���ǰ�ȫ���⣬����������������һ�㡣
�뿴��������:
```cpp
auto_ptr<string> p1(new string ("auto") �� //#1
auto_ptr<string> p2;                       //#2
p2 = p1;                                   //#3
```
�����#3�У�p2�ӹ�string���������Ȩ��p1������Ȩ�������ᡣǰ��˵�������Ǻ��£��ɷ�ֹp1��p2������������ͼ�hͬ��������
��������������ͼʹ��p1���⽫�Ǽ����£���Ϊp1����ָ����Ч�����ݡ�

��������ʹ��unique_ptr������� 
```cpp
unique_ptr<string> p3 (new string ("auto");   //#4
unique_ptr<string> p4��                       //#5
p4 = p3;                                      //#6
```
��������Ϊ���#6�Ƿ���������p3����ָ����Ч���ݵ����⡣��ˣ�unique_ptr��auto_ptr����ȫ��

**��unique_ptr���и������ĵط���**
��ʱ�򣬻Ὣһ������ָ�븳����һ������������Σ�յ�����ָ�롣���������º������壺 
```cpp
unique_ptr<string> demo(const char * s)
{
    unique_ptr<string> temp (new string (s))�� 
    return temp��
}
```
�������д�����´��룺
```cpp
unique_ptr<string> ps;
ps = demo('Uniquely special")��
```
demo()����һ����ʱunique_ptr��Ȼ��ps�ӹ���ԭ���鷵�ص�unique_ptr���еĶ��󣬶�����ʱ��ʱ�� unique_ptr �����٣�Ҳ����˵û�л���ʹ�� unique_ptr ��������Ч�����ݣ����仰��˵�����ָ�ֵ�ǲ�������κ�����ģ���û�����ɽ�ֹ���ָ�ֵ��ʵ���ϣ�������ȷʵ�������ָ�ֵ��������unique_ptr�������ĵط���

**��֮����������ͼ��һ�� unique_ptr ��ֵ����һ��ʱ�����Դ unique_ptr �Ǹ���ʱ��ֵ��������������ô�������Դ unique_ptr ������һ��ʱ�䣬����������ֹ��ô��**�����磺
```cpp
unique_ptr<string> pu1(new string ("hello world"));
unique_ptr<string> pu2;
pu2 = pu1;                                      // #1 not allowed
unique_ptr<string> pu3;
pu3 = unique_ptr<string>(new string ("You"));   // #2 allowed
```
����#1�������ҵ�unique_ptr(pu1)������ܵ���Σ������#2�����������ҵ�unique_ptr����Ϊ������ unique_ptr �Ĺ��캯�����ù��캯����������ʱ������������Ȩ�ø� pu3 ��ͻᱻ���١�**������������ѵ���Ϊ������unique_ptr �����������ָ�ֵ��auto_ptr ��**

��Ȼ��������ȷʵ��ִ��������#1�Ĳ����������Է����ܵķ�ʽʹ������������ָ��ʱ����������ʱ�������ָ�ֵ�Ų���ȫ��Ҫ��ȫ����������ָ�룬�ɸ�������ֵ��C++��һ����׼�⺯��std::move()�������ܹ���һ��unique_ptr������һ����������һ��ʹ��ǰ��demo()���������ӣ��ú�������һ��unique_ptr<string>����
ʹ��move��ԭ����ָ����ת������Ȩ��ɿ�ָ�룬���Զ������¸�ֵ��
```cpp
unique_ptr<string> ps1, ps2;
ps1 = demo("hello");
ps2 = move(ps1);
ps1 = demo("alexia");
cout << *ps2 << *ps1 << endl;
```

####5. ���ѡ������ָ�룿####
���������⼸������ָ��󣬴�ҿ��ܻ�����һ�����⣺��ʵ��Ӧ���У�Ӧʹ����������ָ���أ�
�����������ʹ��ָ�ϡ�

��1���������Ҫʹ�ö��ָ��ͬһ�������ָ�룬Ӧѡ��shared_ptr�����������������
- ��һ��ָ�����飬��ʹ��һЩ����ָ������ʾ�ض���Ԫ�أ�������Ԫ�غ���С��Ԫ�أ�
- �������������ָ������������ָ�룻
- STL��������ָ�롣�ܶ�STL�㷨��֧�ָ��ƺ͸�ֵ��������Щ����������shared_ptr������������unique_ptr������������warning����auto_ptr����Ϊ��ȷ�����������ı�����û���ṩshared_ptr����ʹ��Boost���ṩ��shared_ptr��

��2�����������Ҫ���ָ��ͬһ�������ָ�룬���ʹ��unique_ptr���������ʹ��new�����ڴ棬������ָ����ڴ��ָ�룬���䷵����������Ϊunique_ptr�ǲ����ѡ������������Ȩת�ø����ܷ���ֵ��unique_ptr����������ָ�뽫�������delete���ɽ�unique_ptr�洢��STL�������Ǹ���ֻҪ�����ý�һ��unique_ptr���ƻ򸳸���һ���㷨����sort()�������磬���ڳ�����ʹ������������Ĵ���Ρ�
```cpp
unique_ptr<int> make_int(int n)
{
    return unique_ptr<int>(new int(n));
}
void show(unique_ptr<int> &p1)
{
    cout << *a << ' ';
}
int main()
{
    ...
    vector<unique_ptr<int> > vp(size);
    for(int i = 0; i < vp.size(); i++)
        vp[i] = make_int(rand() % 1000);              // copy temporary unique_ptr
    vp.push_back(make_int(rand() % 1000));     // ok because arg is temporary
    for_each(vp.begin(), vp.end(), show);           // use for_each()
    ...
}
```
����push_back����û�����⣬��Ϊ������һ����ʱunique_ptr����unique_ptr������vp�е�һ��unique_ptr�����⣬�����ֵ�����ǰ����ø�show()���ݶ���for_each()���Ƿ�����Ϊ�⽫����ʹ��һ������vp�ķ���ʱunique_ptr��ʼ��pi�������ǲ�����ġ�ǰ��˵���������������ִ���ʹ��unique_ptr����ͼ��
��unique_ptrΪ��ֵʱ���ɽ��丳��shared_ptr�����뽫һ��unique_ptr����һ����Ҫ�����������ͬ����ǰ��һ����������Ĵ����У�make_int()�ķ�������Ϊunique_ptr<int>��
```cpp
unique_ptr<int> pup(make_int(rand() % 1000));   // ok
shared_ptr<int> spp(pup);                       // not allowed, pup as lvalue
shared_ptr<int> spr(make_int(rand() % 1000));   // ok
```
ģ��shared_ptr����һ����ʽ���캯���������ڽ���ֵunique_ptrת��Ϊshared_ptr��shared_ptr���ӹ�ԭ����unique_ptr���еĶ���
������unique_ptrҪ�������ʱ��Ҳ��ʹ��auto_ptr����unique_ptr�Ǹ��õ�ѡ�������ı�����û��unique_ptr���ɿ���ʹ��Boost���ṩ��scoped_ptr������unique_ptr���ơ�