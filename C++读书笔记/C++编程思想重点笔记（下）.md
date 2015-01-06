### ��ƪ�뿴��[C++���˼���ص�ʼǣ��ϣ�](http://blog.csdn.net/lanxuezaipiao/article/details/41647333)

1. ��ĺô��뻵��
 - ��ĺô���#��##��ʹ��
>�������õ��������ַ������塢�ַ��������ͱ�־ճ����
 
     **�ַ�������**���������#ָʾ����������һ����ʶ��������ת��Ϊ�ַ�����Ȼ��**�ַ�������**�����ڵ��������ڵ��ַ���û�зָ���ʱ��������������ַ��������һ����д���Դ���ʱ�������������Ƿǳ���Ч�ġ� 
    ```cpp
    #define DEBUG(X) cout<<#X " = " << X << endl
    ```
     ��������������Դ�ӡ�κα�����ֵ��
     ����Ҳ���Եõ�һ��������Ϣ���ڴ���Ϣ���ӡ������ִ�е���䡣
     ```cpp
     #define TRACE(S) cout << #S << endl; S
     ```
`#S`������Ҫ�������䡣��2��S��������䣬���������䱻ִ�С���Ȼ������ܻ�������⣬��������һ��forѭ���С� 
    ```cpp
    for (int i = 0 ; i < 100 ; i++ )
        TRACE(f(i)) ;
    ```
     ��Ϊ��TRACE( )����ʵ������������䣬����һ��forѭ��ִֻ�е�һ����
     ```cpp
        for (int i = 0 ; i < 100 ; i++ )
            cout << "f(i)" << endl; 
            f(i);  // �ڶ������������forѭ�������ִ�в��� 
    ```
     ���������**�ں����ö��Ŵ���ֺ�**�� 

     **��־ճ��**��д����ʱ�Ƿǳ����õģ���##��ʾ������������������ʶ����������ճ����һ���Զ�����һ���µı�ʶ�������磺
     ```cpp
     #define FIELD(A) char *A##_string;int A##_size  
     ```
     ��ʱ����Ĵ��룺
     ```cpp
     class record{  
           FIELD(one);  
           FIELD(two);  
           FIELD(three);  
           //...  
     };
     ```
     ���൱������Ĵ��룺
     ```cpp
     class record{  
           char *one_string,int one_size;  
           char *two_string,int two_size;  
           char *three_string,int three_size;    
           //...  
     };
     ```
 - ��Ĳ��ã����׳���
����ٸ����Ӽ���˵����
        ```cpp
        #define band(x) (((x)>5 && (x)<10) ? (x) :0)
        int main() {
          for(int i = 4; i < 11; i++) {
            int a = i;
            cout << "a = " << a << "\t";
            cout << "band(++a)" << band(++a) << "\t";
            cout << "a = " << a << endl;
          }
        
         return 0;
        }
        ```
�����
>a = 4��band(++a)0��a = 5
a = 5��band(++a)8��a = 8
a = 6��band(++a)9��a = 9
a = 7��band(++a)10��a = 10
a = 8��band(++a)0��a = 10
a = 9��band(++a)0��a = 11
a = 10��band(++a)0��a = 12

2. �洢����ָ����
���õ���`static`��`extern`��
�����õ���������һ��`auto`�����Ǽ�������������Ϊ�����߱���������һ���ֲ�������ʵ���ϱ��������ǿ��Դ� ��������ʱ�����������жϳ�����һ���ֲ�����������`auto`�Ƕ���ġ�����һ����`register`����Ҳ�Ǿֲ��������������߱������������ı���Ҫ�����õ������Ա�����Ӧ�þ����ܵ����������ڼĴ����С��������Ż����롣���ֱ��������������͵ı�������ʽҲ������ͬ��������ʱ��������ִ洢���͵�ָ����һ�㣬���Ҫ�õ���������ĵ�ַ�� `register`ָ����ͨ�����ᱻ���ԡ�Ӧ�ñ�����`register`���ͣ���Ϊ���������Ż����뷽��ͨ�����������ø��á�

3. **λ������bitcopy����ֵ���������𣨺���Ҫ��**
��1��������˵����һ�������κ�ʱ��֪�������ڶ��ٸ����󣬿���ͨ������һ��static��Ա�����������´�����ʾ��
    ```cpp
    #include <iostream>
    using namespace std;
    class test {
        static int object_count;
    public:
        test() {
            object_count++;
            print("test()");
        }
        static void print(const char *msg = 0) {
            if(msg) cout << msg << ": ";
            cout << "object_count = " << object_count << endl;
        }
        ~test() {
            object_count--;
            print("~test()");
        }
    };
    int test::object_count = 0;
    // pass and return by value.
    test f(test x) {
        x.print("x argument inside f()");
        return x;
    }
    int main() {
        test h;
        test::print("after construction of h");
        test h2 = f(h);
        test::print("after call to f()");
        return 0;
    }
    ```
Ȼ���������������������������
>test(): object_count = 1
after construction of h: object_count = 1
x argument inside f(): object_count = 1
~test(): object_count = 0
after call to f(): object_count = 0
~test(): object_count = -1
~test(): object_count = -2

 ��h�����Ժ󣬶�������1�����ǶԵġ�����ϣ����f()���ú��������2����Ϊh2Ҳ�ڷ�Χ�ڡ�Ȼ������������0������ζ�ŷ��������صĴ�����ӽ�β������������ִ�к�ʹ�ö�������Ϊ��������ʵ�õ�ȷ�ϣ���Щ�¸����Ͳ�Ӧ�÷����� 

 ����������һ�º���**f()ͨ����ֵ��ʽ���������һ��**��ԭ���Ķ���h�����ں������֮�⣬ͬʱ�ں���������������һ���������������**��ֵ��ʽ����Ķ���Ŀ�����������λ���������õ���Ĭ�Ͽ������캯���������ǵ��ù��캯��**��Ȼ����**�����Ĵ�����ʹ��C��ԭʼ��λ�����ĸ���**����test��**��Ҫ�����ĳ�ʼ����ά������������**�����ԣ�ȱʡ��λ�������ܴﵽԤ�ڵ�Ч���� 

 ���ֲ�������˵��õĺ���f()��Χʱ�����������ͱ����ã���������ʹobject_count��С�� ���ԣ��ں������棬 object_count����0��h2����Ĵ���Ҳ����λ���������ģ�Ҳ�ǵ���Ĭ�Ͽ������캯���������ԣ����캯��������Ҳû�е��á�������h��h2�������ǵ����÷�Χʱ�����ǵ�����������ʹobject_countֵ��Ϊ��ֵ��
 >**�ܽ᣺**
 - λ�����������ǵ�ַ��Ҳ��ǳ����������ֵ�����򿽱��������ݣ��������
 - �����ǳ�������Լ����Ϊ��**���һ����ӵ����Դ���������Ķ��������ƹ��̵�ʱ����Դ���·��䣬������̾����������֮��û�����·�����Դ������ǳ������**
 - Ĭ�ϵĿ������캯�����͡�ȱʡ�ĸ�ֵ�����������á�λ���������ǡ�ֵ�������ķ�ʽ��ʵ�֣��������к���ָ�����������������ע��������

     ����λ������ֵ���������������Բο���ƪ���£�[C++�е�λ������ֵ����ǳ̸](http://blog.csdn.net/wangqiulin123456/article/details/8464082)

 Ϊ�˴ﵽ����������Ч�������Ǳ����Լ����忽�����캯����
 ```cpp
test(const test& t) {
  object_count++;
  print("test(const test&)");
 }
 ```
�����������ȷ��
>test(): object_count = 1
after construction of h: object_count = 1
test(const test&): object_count = 2
x argument inside f(): object_count = 2
test(const test&): object_count = 3
~test(): object_count = 2
after call to f(): object_count = 2
~test(): object_count = 1
~test(): object_count = 0
  
 ###����
 - �����main�м�һ�䡰f(h);���������Է���ֵ����ô���ص�ʱ�򻹻���ÿ������캯����
**���ǣ������**����ʱ������һ����ʱ�������ڸ���ʱ����û���ô�����˻����ϵ��������������ٵ�����ʱ������ͻ�������������
>test(): object_count = 1
after construction of h: object_count = 1
test(const test&): object_count = 2
x argument inside f(): object_count = 2
test(const test&): object_count = 3
~test(): object_count = 2
after call to f(): object_count = 2
test(const test&): object_count = 3
x argument inside f(): object_count = 3
test(const test&): object_count = 4
~test(): object_count = 3
~test(): object_count = 2
~test(): object_count = 1
~test(): object_count = 0

 - ���һ����������������Ķ�����϶��ɣ������ʱ����û���Զ��忽�����캯����**��ô�������ݹ��Ϊ���еĳ�Ա����ͻ�������ÿ������캯��**�������Ա����Ҳ���б�Ķ�����ô���ߵĿ������캯��Ҳ�������á�
 - ����������ÿ������캯��������׼���ô�ֵ�ķ�ʽ���������ʱ������Ҫ�������캯���������ֽ��������
     - ��ֹ��ֵ��������
��һ���򵥵ļ�����ֹͨ����ֵ��ʽ���ݣ�**����һ��˽��private�������캯����**������������ȥ���������������ǵĳ�Ա��������Ԫ������Ҫִ�д�ֵ��ʽ�Ĵ��ݡ�����û���ͼ�ô�ֵ��ʽ���ݻ򷵻ض��󣬱��������ᷢ��һ��������Ϣ��������Ϊ�������캯����˽�еġ���Ϊ��������ʽ���������ǽӹ�������������Ա��������ٴ���ȱʡ�Ŀ������캯����
    ```cpp
    class noCC {
        int i;
        noCC(const noCC&); // private and no definition
    public:
        noCC(int I = 0) : i(I) {}
    };
    void f(noCC);
    main() {
        noCC n;
    //! f(n);        // error: copy-constructor called
    //! noCC n2 = n; // error: c-c called
    //! noCC n3(n);  // error: c-c called
    }
    ```
         ע������`n2 = n`Ҳ���ÿ������캯����ע������Ҫ�͸�ֵ�������֡�
     - �ı��ⲿ����ĺ���
ʹ�����ô��ݣ�����`void get(const Slice&);`

4. ���Զ��̳еĺ���
���캯�������������͸�ֵ������operator=�����ܱ��̳С�

5. ˽�м̳е�Ŀ��
`private`�̳е�Ŀ����ʲô����Ϊ������ѡ�񴴽�һ��`private`�����ƺ������ʡ�**��private�̳а����ڸ�������ֻ��Ϊ�����Ե�������**�����ǣ����û���������ɣ���Ӧ�����ٻ���������**ͨ��������`private`��Ա������`private`�̳�**��
Ȼ����`private`�̳�Ҳ����һ���ô���
�������żȻ�����������**����������������ӿ�һ���Ľӿڣ�����������ö�������������һ����private�̳��ṩ���������**��

 ###����
�ܶ�˽�м̳г�Ա���л���
��˽�м̳�ʱ�����������public��Ա�������private�����ϣ�������е��κ�һ���ǿ��ӵģ����԰쵽�𣿴��ǿ��Եģ�**ֻҪ���������publicѡ���������ǵ����ּ��ɣ��µı�׼��ʹ��using�ؼ��֣���**
    ```cpp
    #include <iostream>
    class base {
    public:
        char f() const { return 'a'; }
        int g() const { return 2; }
        float h() const { return 3.0; }
    };
    class derived : base {
    public:
        using base::f; // Name publicizes member
        using base::h; 
    };
    int main() {
        derived d;
        d.f();
        d.h();
     //	d.g(); // error -- private function
        return 0;
    }
    ```
  ������**�����Ҫ���������Ļ��ಿ�ֵĹ��ܣ���`private`�̳������õ�**��

6. ���ؼ̳�ע������ӳ��Ķ����ԡ�����base���и�f()�������������Ӷ���d1��d2���Ҷ���д��base��f()��������ʱ����dd���Ҳ��f()��������ͬʱ�̳���d1��d2����Ϊf()�������ڶ����ԣ���֪���ü̳��ĸ�f()������
**��������Ƕ�dd���е�f()�������¶���������������**��������ȷָ��ʹ��d1��f()������
��ȻҲ���ܽ�dd������ӳ��Ϊbase�࣬�����ͨ��ʹ����̳н�����ؼ���virtual��base�е�f()�����ĳ��麯����d1��d2�ļ̳ж���Ϊ��̳У���Ȼdd�̳�d1��d2��public�̳м��ɡ�

7. C��������ιر�assert���Թ��ܣ�
`ͷ�ļ���<assert.h>��<cassert>`
�ڿ��������У�ʹ�����ǣ���ɺ���#define NDEBUGʹ֮ʧЧ���Ա��Ƴ���Ʒ��**ע�������ͷ�ļ�֮ǰ�رղ���Ч��**
    ```cpp
    #define NDEBUG
    #include <cassert>
    ```
8. **C++���ʵ�ֶ�̬���󣿡�����̬��ʵ�֣�����Ҫ��**
C++��Ϊ��ʵ�ֶ�̬����������ÿ�������麯�����ഴ��һ������ΪVTABLE��������� VTABLE�У������������ض�����麯����ַ����ÿ�������麯�������У����������ܵ���һָ�룬��Ϊvpointer����дΪVPTR����ָ����������VTABLE��ͨ������ָ�����麯������ʱ��Ҳ��������̬����ʱ������������̬�ز���ȡ�����VPTR������VTABLE���в��Һ�����ַ�Ĵ��룬�������ܵ�����ȷ�ĺ���ʹ����������
**Ϊÿ��������VTABLE����ʼ��VPTR��Ϊ�麯�����ò�����룬������Щ�����Զ������ģ�**�������ǲ��ص�����Щ�������麯�����������ĺ��ʵĺ������ܱ����ã������ڱ���������֪�����������ض����͵�����¡�

 ��vtable���У���������������������л������Ļ���������������Ϊ`virtual`�ĺ����ĵ�ַ������������������û�ж��ڻ���������Ϊ`virtual`�ĺ����������¶��壬��������ʹ�û��������麯����ַ��
����ٸ�����˵����
    ```cpp
    #include <iostream>
    enum note { middleC, Csharp, Cflat };
    
    class instrument {
    public:
        virtual void play(note) const {
            cout << "instrument::play" << endl;
        }
        virtual char* what() const {
            return "instrument";
        }
        // assume this will modify the object:
        virtual void adjust(int) {}
    };
    
    class wind : public instrument {
    public:
        void play(note) const {
            cout << "wind::play" << endl;
        }
        char* what() const {
            return "wind";
        }
        void adjust(int) {}
    };
    
    class percussion : public instrument {
    public:
        void play(note) const {
            cout << "percussion::play" << endl;
        }
        char* what() const {
            return "percussion";
        }
        void adjust(int) {}
    };
    
    class string : public instrument {
    public:
        void play(note) const {
            cout << "string::play" << endl;
        }
        char* what() const {
            return "string";
        }
        void adjust(int) {}
    };
    
    class brass : public wind {
    public:
        void play(note) const {
            cout << "brass::play" << endl;
        }
        char* what() const {
            return "brass";
        }
    };
    
    class woodwind : public wind {
    public:
        void play(note) const {
            cout << "woodwind::play" << endl;
        }
        char* what() const {
            return "woodwind";
        }
    };
    
    instrument *A[] = {
        new wind,
        new percussion,
        new string,
        new brass
    };
    ```
 ��ͼ������ָ������A[]��
 ![ָ������A](http://upload-images.jianshu.io/upload_images/46178-f0af7473199ad9d5.png)
 ���濴������ͨ��instrumentָ�����brass����adjust()��instrument���ò������½����
 ![��̬��](http://upload-images.jianshu.io/upload_images/46178-b26ec8f92af336fd.png)
 �����������instrumentָ�뿪ʼ�����ָ��ָ������������ʼ��ַ�����е�instrument�������instrument�����Ķ��������ǵ�VPTR��**���ڶ������ͬ��λ�ã������ڶ���Ŀ�ͷ��**�����Ա������ܹ�ȡ����������VPTR��VPTRָ��VTABLE�Ŀ�ʼ��ַ��**���е�VTABLE����ͬ��˳�򣬲��ܺ������͵Ķ���** play()�ǵ�һ����what()�ǵڶ�����adjust()�ǵ����������Ա�����֪��adjust()��������VPTR + 2�������������ǡ���instrument :: adjust��ַ��������������������������Ǵ����������ǲ������룬����VPTR + 2�������������������ΪVPTR��Ч����ʵ�ʺ�����ַ��ȷ������������ʱ�����������͵õ�����ϣ�����������������������Ϣ����������ܶ϶���Ӧ����ʲô��
   
 ###���� �� ������Ƭ###
����̬�ش������ʱ��**����ַ�봫ֵ�����ԵĲ�ͬ**�������������Ѿ����������Ӻͽ��ῴ�������Ӷ��Ǵ���ַ�ģ������Ǵ�ֵ�ġ�������Ϊ��ַ������ͬ�ĳ��ȣ����������ͣ���ͨ���Դ�һЩ������ĵ�ַ�ʹ����ࣨ��ͨ��Сһ�㣩����ĵ�ַ����ͬ�ġ���ǰ����͵ģ�ʹ�ö�̬��Ŀ�����öԻ����������Ĵ���Ҳ�ܲ������������ 
**���ʹ�ö��������ʹ�õ�ַ�����ý�������ӳ�䣬�����������ʹ���ǳԾ���������� ������Ƭ����ֱ����ʣ���������ʺ���Ŀ�ĵ��Ӷ���**�����������п��Կ���ͨ������������ĳ�����Ƭʣ�����Ĳ��֡�
    ```cpp
    #include <iostream>
    using namespace std;
    class base {
        int i;
    public:
        base(int I = 0) : i(I) {}
        virtual int sum() const { return i; }
    };
    class derived : public base {
        int j;
    public:
        derived(int I = 0, int J = 0) : base(I), j(J) {}
        virtual int sum() const { return base::sum() + j; }
    };
    void call(base b) {
        cout << "sum = " << b.sum() << endl;
    }
    main() {
        base b(10);
        derived d(10, 47);
        call(b);
        call(d);
    }
    ```
����call( )ͨ��**��ֵ����**һ������Ϊbase�Ķ���Ȼ�������base��������麯��sum( )�� ���ǿ���ϣ����һ�ε��ò���10���ڶ��ε��ò���57��ʵ���ϣ����ζ�����10�� ����������У�**���������鷢����**��
 - ��һ��call( )���ܵ�ֻ��һ��base������������������������ڵĴ��붼��ֻ������base��ص����� ��call( )���κε��ö�������һ����base��С��ͬ�Ķ���ѹջ���ڵ��ú����������ζ�ţ����һ����base����������󱻴���call������������������ֻ������������Ӧ��base�Ĳ��֣��г����������������֣���ͼ��
![������Ƭ](http://upload-images.jianshu.io/upload_images/46178-fb36e393c8f71e57.png)
���ڣ����ǿ��ܶ�����麯�����øе���֣��������麯����ʹ����base�����Դ��ڣ��� ��ʹ����derived�Ĳ��֣�derived���ٴ����ˣ���Ϊ������Ƭ���� ��ʵ�����Ѿ��������б���ȳ����������������ȫ����ֵ���ݡ���Ϊ��ʱ��������Ϊ��֪����������ȷ�е����ͣ��������Ķ����������õ��κ���Ϣ���Ѿ�ʧȥ����
 - ���⣬��ֵ����ʱ������base����ʹ�ÿ������캯�����ù��캯����ʼ��vptrָ��base vtable������ֻ������������base���֡�����û����ʽ�Ŀ������캯�������Ա������Զ���Ϊ���Ǻϳ�һ��������������ԭ�������������Ƭ�ڼ�����һ��base���� 

    ������Ƭʵ������ȥ���˶����һ���֣���������ʹ��ָ������������򵥵ظı��ַ�����ݡ���ˣ���������ӳ�䲻��������ʵ�ϣ�ͨ��Ҫ������ֹ���ֲ��������ǿ���ͨ���ڻ� ���з��ô��麯������ֹ������Ƭ����ʱ������ж�����Ƭ�ͽ��������ʱ�ĳ�����Ϣ��

    `���ע�⣺������ڹ��캯���в����������ڹ��캯���е����麯��û�н����`

9. **RTTI������ʱ����ʶ�𣨺���Ҫ��** 
 - ����
>����ʱ����ʶ��Run-time type identification, RTTI����**������ֻ��һ��ָ������ָ�������ʱȷ��һ�������׼ȷ����**��

 - ʹ�÷���
һ������£����ǲ�����Ҫ֪��һ�����ȷ�����ͣ��麯�����ƿ���ʵ���������͵���ȷ��Ϊ��������Щʱ��**������ָ��ĳ������Ļ���ָ�룬ȷ���ö����׼ȷ�����Ǻ����õġ�**
RTTI���쳣һ��������פ�����麯�����е�������Ϣ��**�����ͼ��һ��û���麯����������RTTI���͵ò���Ԥ�ڵĽ����**

     RTTI������ʹ�÷���
     - ��һ��ʹ��`typeid()`������`sizeof()`һ�������϶���һ����������ʵ���������ɱ�����ʵ�ֵġ�`typeid()`����һ����������������һ���������û�ָ�룬����ȫ��`typeinfo`��ĳ��������һ�����á��������������==���͡�!=��������Ƚ���Щ����Ҳ������`name()`��������͵����ơ�ע�⣬�����`typeid( )`����һ��`shape*`�Ͳ�����������Ϊ����Ϊ`shape*`�����������֪��һ��ָ����ָ����ľ�ȷ���ͣ����Ǳ��������������ָ�롣���磬s�Ǹ�`shape* `, ��ô��
    ```cpp
    cout << typeid(*s).name()<<endl;
    ```
         ����ʾ��s��ָ��Ķ������͡�
Ҳ������`before(typeinfo&)`��ѯһ��`typeinfo`�����Ƿ�����һ`��typeinfo`�����ǰ�棨�Զ���ʵ�ֵ�����˳�򣩣���������true��false�����д�� 
    ```cpp
    if(typeid(me).before(typeid(you))) //...
    ```
         ��ô��ʾ�������ڲ�ѯme������˳�����Ƿ���you֮ǰ��
     - RTTI�ĵڶ����÷��С���ȫ��������ӳ�䡱��ʹ��`dynamic_cast<>`ģ�塣
     
     **���ַ�����ʹ�þ������£�**
    ```cpp
    #include <iostream>
    #include <typeinfo>
    using namespace std;
    class base {
        int i;
    public:
        base(int I = 0) : i(I) {}
        virtual int sum() const { return i; }
    };
    class derived : public base {
        int j;
    public:
        derived(int I = 0, int J = 0) : base(I), j(J) {}
        virtual int sum() const { return base::sum() + j; }
    };
    main() {
        base *b = new derived(10, 47);
        // rtti method1
        cout << typeid(b).name() << endl; // P4base
        cout << typeid(*b).name() << endl; // 7derived
        if(typeid(b).before(typeid(*b)))
            cout << "b is before *b" << endl;
        else
            cout << "*b is before b" << endl;
        // rtti method2
        derived *d = dynamic_cast<derived*>(d);
        if(d) cout << "cast successful" << endl;
    }
    ```
     **ע��1��**�������û�ж�̬���ƣ���RTTI�������еĽ������������Ҫ�ģ��������û���麯������������������ʾbase��һ��ϣ��RTTI���ڶ�̬�ࡣ
**ע��2��**����ʱ���͵�ʶ���һ��`void`��ָ�벻�����á�`void *`ȷʵ��ζ�š�����û��������Ϣ����
    ```cpp
    void *v = new stimpy;
    stimpy* s = dynamic_cast<stimpy*>(v);  // error
    cout << typeid(*v).name() << endl;     // error
    ```
 - **RTTI��ʵ��**
**���͵�RTTI��ͨ����VTABLE�з�һ�������ָ����ʵ�ֵġ����ָ��ָ��һ���������ض����͵�typeinfo�ṹ��ÿ������ֻ����һ��typeinfo��ʵ����������typeid( )���ʽ������ʵ���Ϻܼ򵥡�**VPTR����ȡtypeinfo��ָ�룬Ȼ�����һ�����typeinfo�ṹ��һ�����á�����һ�������ԵĲ��衪�����Ѿ�֪����Ҫ������ʱ�䡣

     ����`dynamic_cast<Ŀ��* > <Դָ��>`������������Ǻ����׵ģ��Ȼָ�Դָ���RTTI��Ϣ��ȡ��`Ŀ��*`������RTTI��Ϣ��Ȼ����ÿ��е�һ�������ж�Դָ���Ƿ���`Ŀ��*`��ͬ������`Ŀ��*`���͵Ļ��ࡣ�����ܶԷ��ص�ָ������һ��С�ĸĶ�����ΪĿ��ָ������ܴ��ڶ��ؼ̳е��������Դָ�����Ͳ�����������ĵ�һ�����ࡣ�ڶ��ؼ̳�ʱ������ø���Щ����Ϊһ�������ڼ̳в���п��ܳ���һ�����ϣ����ҿ���������ࡣ 
���ڶ�̬ӳ��Ŀ����̱�����һ�������Ļ����б����Զ�̬ӳ��Ŀ�����`typeid()`Ҫ�󣨵�Ȼ���ǵõ�����ϢҲ��ͬ����������ǵ�������˵���ܹܺؼ������������Ƿ�ȷ���Եģ���Ϊ����һ������Ҫ�Ȳ���һ�������໨�����ʱ�䡣���⶯̬ӳ���������ǱȽ��κ����ͣ���������ͬһ���̳в���бȽ������ࡣ��ʹ�ö�̬ӳ����õĿ����̿��������ˡ�
        | ӳ������        | ���� |
        | --------   | -----:  |
        | static_cast | Ϊ�ˡ���Ϊ���á��͡���Ϊ�Ϻá���ʹ�õ�ӳ�䣬����һЩ���ǿ������ڲ��õ�ӳ�䣨������ӳ����Զ�����ת���� |
        | const_cast | ����ӳ�䳣���ͱ�����const��volatile�� |
        | const_cast | Ϊ�˰�ȫ���͵�����ӳ�䣨����ǰ���Ѿ����ܣ� |
        | reinterpret_cast |  Ϊ��ӳ�䵽һ����ȫ��ͬ����˼������ؼ�����������Ҫ������ӳ���ԭ������ʱҪ�õ���������ӳ�䵽�����ͽ�����Ϊ�˹�Ū���������Ŀ�ġ���������ӳ������Σ�յ� |

     **ע�⣺**������һ��const ת��Ϊ��const��**���һ��volatileת����һ����volatile�����������������**����Ҫ�õ�const_cast�����ǿ�����const_cast��Ψһת�����������������ת��ǣ�������������ֿ���ָ�����������һ���������