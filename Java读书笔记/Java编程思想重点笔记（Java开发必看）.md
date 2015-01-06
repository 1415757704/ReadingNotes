### 1. Java�еĶ�̬����⣨ע����C++���֣�
 - **Java�г���static������final������private��������������final��������Ϊ���ܱ�������ʣ�֮�⣬�������еķ������Ƕ�̬��**������ζ��ͨ������£����ǲ����ж��Ƿ�Ӧ�ý��ж�̬�󶨡������Զ�������
     - final������ʹ���������ɸ���Ч�Ĵ��룬��Ҳ��Ϊʲô˵����Ϊfinal��������һ���̶���������ܣ�Ч�������ԣ���
     - ���ĳ�������Ǿ�̬�ģ�������Ϊ�Ͳ����ж�̬�ԣ�
    ```java
    class StaticSuper {
    	public static String staticGet() {
    		return "Base staticGet()";
    	}
    	
    	public String dynamicGet() {
    		return "Base dynamicGet()";
    	}
    }
    
    class StaticSub extends StaticSuper {
    	public static String staticGet() {
    		return "Derived staticGet()";
    	}
    	
    	public String dynamicGet() {
    		return "Derived dynamicGet()";
    	}
    }
    
    public class StaticPolymorphism {
    	
    	public static void main(String[] args) {
    		StaticSuper sup = new StaticSub();
    		System.out.println(sup.staticGet());
    		System.out.println(sup.dynamicGet());
    	}
    
    }
    ```
         �����
>Base staticGet()
Derived dynamicGet()

 - **���캯���������ж�̬�ԣ�����ʵ������static������ֻ������static��������ʽ�ġ�**��ˣ����캯�����ܹ���override��

 - **�ڸ��๹�캯���ڲ����þ��ж�̬��Ϊ�ĺ����������޷�Ԥ��Ľ��**����Ϊ��ʱ�������û��ʼ������ʱ�������෽������õ�������Ҫ�Ľ����
    ```java
    class Glyph {
    	void draw() {
    		System.out.println("Glyph.draw()");
    	}
    	Glyph() {
    		System.out.println("Glyph() before draw()");
    		draw();
    		System.out.println("Glyph() after draw()");
    	}
    }
    
    class RoundGlyph extends Glyph {
    	private int radius = 1;
    	
    	RoundGlyph(int r) {
    		radius = r;
    		System.out.println("RoundGlyph.RoundGlyph(). radius = " + radius);
    	}
    	
    	void draw() {
    		System.out.println("RoundGlyph.draw(). radius = " + radius);
    	}
    }
    
    public class PolyConstructors {
    
    	public static void main(String[] args) {
    		new RoundGlyph(5);
    
    	}
    
    }
    ```
�����
>Glyph() before draw()
RoundGlyph.draw(). radius = 0
Glyph() after draw()
RoundGlyph.RoundGlyph(). radius = 5

 Ϊʲô��������������Ҫ��ȷ����**Java�й��캯���ĵ���˳��**��
>��1���������κ����﷢��֮ǰ�������������Ĵ洢�ռ��ʼ���ɶ�����0��
��2�����û��๹�캯�����Ӹ���ʼ�ݹ���ȥ����Ϊ��̬�Դ�ʱ�������า�Ǻ��draw()������Ҫ�ڵ���RoundGlyph���캯��֮ǰ���ã������ڲ���1��Ե�ʣ����Ǵ�ʱ�ᷢ��radius��ֵΪ0��
��3��������˳����ó�Ա�ĳ�ʼ��������
��4������������Ĺ��캯����

 - ֻ�з�private�����ſ��Ա����ǣ����ǻ���Ҫ����ע�⸲��private������������ʱ��Ȼ���������ᱨ������Ҳ���ᰴ����������������ִ�У�������private������������˵��һ���µķ����������ط�������ˣ�**�������У��·�������ò�Ҫ������private������ȡͬһ���֣���Ȼû��ϵ����������⣬��Ϊ�ܹ����ǻ����private������**��

 - Java����������ķ��ʲ������ɱ�������������˲��Ƕ�̬�ġ�**����������ͬ�����Զ�����䲻ͬ�Ĵ洢�ռ�**�����£�
    ```java
    // Direct field access is determined at compile time.
    class Super {
    	public int field = 0;
    	public int getField() {
    		return field;
    	}
    }
    
    class Sub extends Super {
    	public int field = 1;
    	public int getField() {
    		return field;
    	}
    	public int getSuperField() {
    		return super.field;
    	}
    }
    
    public class FieldAccess {
    
    	public static void main(String[] args) {
    		Super sup = new Sub();
    		System.out.println("sup.filed = " + sup.field + 
    				", sup.getField() = " + sup.getField());
    		Sub sub = new Sub();
    		System.out.println("sub.filed = " + sub.field + 
    				", sub.getField() = " + sub.getField() + 
    				", sub.getSuperField() = " + sub.getSuperField());
    	}
    
    }
    ```
     �����
>sup.filed = 0, sup.getField() = 1
sub.filed = 1, sub.getField() = 1, sub.getSuperField() = 0

     Sub����ʵ����**������������Ϊfield����**��Ȼ��������Sub�е�fieldʱ��������Ĭ���򲢷�Super�汾��field�����Ϊ�˵õ�Super.field������**��ʽ��ָ��**super.field��

### 2. is-a��ϵ��is-like-a��ϵ
- is-a��ϵ���ڴ��̳У���ֻ���ڻ������Ѿ������ķ����ſ����������б����ǣ�����ͼ��ʾ��
![is-a.png](http://upload-images.jianshu.io/upload_images/46178-5fbaec520e33a8d3.png)
���������������ȫ��ͬ�Ľӿڣ�**��������ת��ʱ��Զ����Ҫ֪�����ڴ���Ķ����ȷ�����ͣ���ͨ����̬��ʵ�֡�**

- is-like-a��ϵ��������չ�˻���ӿڡ���������ͬ�Ļ����ӿڣ�**�������������ɶ��ⷽ��ʵ�ֵ��������ԡ�**
![has-a.png](http://upload-images.jianshu.io/upload_images/46178-335a5bca78e970b4.png)
ȱ����������нӿڵ���չ���ֲ��ܱ�������ʣ����**һ������ת�ͣ��Ͳ��ܵ�����Щ�·�����**

### 3. ����ʱ������Ϣ��RTTI + ���䣩
 - ����
RTTI������ʱ������Ϣʹ��������ڳ�������ʱ���ֺ�ʹ��������Ϣ��
 - ʹ�÷�ʽ
Java�����������������ʱʶ�����������Ϣ�ģ���Ҫ�����ַ�ʽ�����и����ĵ����ַ�ʽ��������������
     - һ���ǡ���ͳ�ġ�RTTI�����ٶ������ڱ���ʱ�Ѿ�֪�������е����ͣ�����`Shape s = (Shape)s1��`
     - ��һ����**�����䡱����**������������������ʱ���ֺ�ʹ�������Ϣ����ʹ��`Class.forName()`��
     - ��ʵ���е�������ʽ�����ǹؼ���`instanceof`��������һ��boolֵ�������������͵ĸ����ָ����**������������𣿻��������������������𣿡�**���������==��equals�Ƚ�ʵ�ʵ�Class���󣬾�û�п��Ǽ̳С������������ȷ�е����ͣ����߲��ǡ�

 - ����ԭ��
Ҫ���RTTI��Java�еĹ���ԭ��**���ȱ���֪��������Ϣ������ʱ����α�ʾ��**����������ɳ�Ϊ`Class����`�����������ɵģ��������������йص���Ϣ��Java��Class������ִ����RTTI��**ʹ�������������ϵͳʵ��**��

 ���ۺ�ʱ��ֻҪ����������ʱʹ��������Ϣ����**�������Ȼ�ö�ǡ����Class���������**����ȡ��ʽ�����֣�
 ��1�������û�г��и����͵Ķ�����`Class.forName()`����ʵ�ִ˹��ܵı��;����Ϊ������Ҫ������Ϣ��
 ��2��������Ѿ�ӵ����һ������Ȥ�����͵Ķ����ǾͿ���ͨ������`getClass()`��������ȡClass�����ˣ��������ر�ʾ�ö����ʵ�����͵�Class���á�Class�����������õķ��������磺
    ```java
    package rtti;
    interface HasBatteries{}
    interface WaterProof{}
    interface Shoots{}
    
    class Toy {
    	Toy() {}
    	Toy(int i) {}
    }
    
    class FancyToy extends Toy
    implements HasBatteries, WaterProof, Shoots {
    	FancyToy() {
    		super(1);
    	}
    }
    
    public class RTTITest {
    	
    	static void printInfo(Class cc) {
    		System.out.println("Class name: " + cc.getName() + 
    				", is interface? [" + cc.isInterface() + "]");
    		System.out.println("Simple name: " + cc.getSimpleName());
    		System.out.println("Canonical name: " + cc.getCanonicalName());
    	}
    	
    	public static void main(String[] args) {
    		Class c = null;
    		try {
    			c = Class.forName("rtti.FancyToy"); // ������ȫ�޶���������+������
    		} catch(ClassNotFoundException e) {
    			System.out.println("Can't find FancyToy");
    			System.exit(1);
    		}
    		printInfo(c);
    		
    		for(Class face : c.getInterfaces()) {
    			printInfo(face);
    		}
    		
    		Class up = c.getSuperclass();
    		Object obj = null;
    		try {
    			// Requires default constructor.
    			obj = up.newInstance();
    		} catch (InstantiationException e) {
    			System.out.println("Can't Instantiate");
    			System.exit(1);
    		} catch (IllegalAccessException e) {
    			System.out.println("Can't access");
    			System.exit(1);
    		}
    		printInfo(obj.getClass());
    	}
    
    }
    ```
�����
>Class name: rtti.FancyToy, is interface? [false]
Simple name: FancyToy
Canonical name: rtti.FancyToy
Class name: rtti.HasBatteries, is interface? [true]
Simple name: HasBatteries
Canonical name: rtti.HasBatteries
Class name: rtti.WaterProof, is interface? [true]
Simple name: WaterProof
Canonical name: rtti.WaterProof
Class name: rtti.Shoots, is interface? [true]
Simple name: Shoots
Canonical name: rtti.Shoots
Class name: rtti.Toy, is interface? [false]
Simple name: Toy
Canonical name: rtti.Toy

 ��3��Java���ṩ����һ�ַ��������ɶ�Class��������ã���ʹ��**�����泣��**����������ľ���������`FancyToy.class;`�����á�
�������������򵥣����Ҹ���ȫ����Ϊ��**�ڱ���ʱ�ͻ��ܵ���飨��˲���Ҫ����try�����У�**�������������˶�forName���������ã�����Ҳ����Ч�������泣����������Ӧ������ͨ���࣬Ҳ����Ӧ���ڽӿڡ������Լ������������͡�
 ---
 **ע��**����ʹ�á�.class����������Class���������ʱ��**�����Զ��س�ʼ����Class���󣬳�ʼ�����ӳٵ��˶Ծ�̬��������������ʽ���Ǿ�̬�ģ����߷�final��̬��ע��final��̬�򲻻ᴥ����ʼ�������������״�����ʱ��ִ�У�**����ʹ��Class.forNameʱ���Զ��ĳ�ʼ����

 Ϊ��ʹ���������׼������ʵ�ʰ����������裺
- ���أ����������ִ�С������ֽ��룬������Щ�ֽ����д���һ��Class����
- ���ӣ���֤���е��ֽ��룬Ϊ��̬�����洢�ռ䣬�����������Ļ�������������ഴ���Ķ���������������á�
- ��ʼ�������������г��࣬������ʼ����ִ�о�̬��ʼ�����;�̬��ʼ���顣

 ��һ��ǳ���Ҫ������ͨ��һ��ʵ����˵�������ߵ�����
```java
package rtti;
import java.util.Random;
class Initable {
    	static final int staticFinal = 47;
    	static final int staticFinal2 = ClassInitialization.rand.nextInt(1000);
	
    	static {
    		System.out.println("Initializing Initable");
    	}
}
class Initable2 {
    	static int staticNonFinal = 147;
    	
    	static {
    		System.out.println("Initializing Initable2");
    	}
}
class Initable3 {
    	static int staticNonFinal = 74;
    	
    	static {
    		System.out.println("Initializing Initable3");
    	}
}
public class ClassInitialization {

    	public static Random rand = new Random(47);
    	
    	public static void main(String[] args) {
    		// Does not trigger initialization
    		Class initable = Initable.class;
    		System.out.println("After creating Initable ref");
    		// Does not trigger initialization
    		System.out.println(Initable.staticFinal);
    		// Does trigger initialization(rand() is static method)
    		System.out.println(Initable.staticFinal2);
    		
    		// Does trigger initialization(not final)
    		System.out.println(Initable2.staticNonFinal);
    		
    		try {
    			Class initable3 = Class.forName("rtti.Initable3");
    		} catch (ClassNotFoundException e) {
    			System.out.println("Can't find Initable3");
    			System.exit(1);
    		}
    		System.out.println("After creating Initable3 ref");
    		System.out.println(Initable3.staticNonFinal);
    	}
}
 ```
 �����
>After creating Initable ref
47
Initializing Initable
258
Initializing Initable2
147
Initializing Initable3
After creating Initable3 ref
74
 ---

 - RTTI�����ƣ����ͻ�ƣ� �� �������
�����֪��ĳ�������ȷ�����ͣ�RTTI���Ը����㣬������һ��**���ƣ���������ڱ���ʱ������֪����������ʹ��RTTIʶ������Ҳ�����ڱ���ʱ������������֪������Ҫͨ��RTTI��������ࡣ**

   ����ͻ������������ǵģ�ͻ�����ľ���**�������**��
  `Class`����`java.lang.reflect`���һ��Է���ĸ��������֧�֣�����������`Field`��`Method`�Լ�`Constructor`�ࣨÿ���඼ʵ����`Member`�ӿڣ�����Щ���͵Ķ�������JVM������ʱ�����ģ����Ա�ʾδ֪�����Ӧ�ĳ�Ա��������Ϳ���ʹ��`Constructor`�����µĶ�����`get()/set()`������ȡ���޸���`Field`����������ֶΣ���`invoke()`����������`Method`��������ķ��������⣬�����Ե���`getFields()��getMethods()��getConstructors()`�Ⱥܱ����ķ������Է��ر�ʾ�ֶΡ������Լ��������Ķ�������顣������**�������������Ϣ����������ʱ����ȫȷ�����������ڱ���ʱ����Ҫ֪���κ����顣**
 
  ---
 ####������RTTI������
 ��ͨ��������һ��δ֪���͵Ķ���򽻵�ʱ��JVMֻ�Ǽ򵥵ؼ��������󣬿��������ĸ��ض����ࣨ����RTTI������������������������֮ǰ�����ȼ����Ǹ����`Class`������ˣ��Ǹ����`.class`�ļ�����JVM��˵�����ǿɻ�ȡ�ģ�Ҫô�ڱ��ػ����ϣ�Ҫô����ͨ������ȡ�á�����**RTTI�뷴��֮������������ֻ���ڣ���RTTI��˵���������ڱ���ʱ�򿪺ͼ��.class�ļ���Ҳ���ǿ�������ͨ�������ö�������з������������ڷ��������˵��.class�ļ��ڱ���ʱ�ǲ��ɻ�ȡ�ģ�������������ʱ�򿪺ͼ��.class�ļ���**

 ������������÷�����ƴ�ӡ��һ��������з����������ڻ����ж���ķ�������

    ```java
    package typeinfo;
    
    import java.lang.reflect.Constructor;
    import java.lang.reflect.Method;
    import java.util.regex.Pattern;
    
    // Using reflection to show all the methods of a class.
    // even if the methods are defined in the base class.
    public class ShowMethods {
    	private static String usage = 
    		"usage: \n" + 
    	    "ShowMethods qualified.class.name\n" +
    	    "To show all methods in class or: \n" +
    	    "ShowMethods qualified.class.name word\n" +
    	    "To search for methods involving 'word'";
    	
    	private static Pattern p = Pattern.compile("\\w+\\.");
    
    	public static void main(String[] args) {
    		if(args.length < 1) {
    			System.out.println(usage);
    			System.exit(0);
    		}
    		int lines = 0;
    		try {
    			Class<?> c = Class.forName(args[0]);
    			Method[] methods = c.getMethods();
    			Constructor[] ctors = c.getConstructors();
    			if(args.length == 1) {
    				for(Method method : methods) {
    					System.out.println(p.matcher(method.toString()).replaceAll(""));
    				}
    				for(Constructor ctor : ctors) {
    					System.out.println(p.matcher(ctor.toString()).replaceAll(""));
    				}
    				lines = methods.length + ctors.length;
    			} else {
    				for(Method method : methods) {
    					if(method.toString().indexOf(args[1]) != -1) {
    						System.out.println(p.matcher(method.toString()).replaceAll(""));
    						lines++;
    					}
    				}
    				for(Constructor ctor : ctors) {
    					if(ctor.toString().indexOf(args[1]) != -1) {
    						System.out.println(p.matcher(ctor.toString()).replaceAll(""));
    						lines++;
    					}
    				}
    			}
    		} catch (ClassNotFoundException e) {
    			System.out.println("No such Class: " + e);
    		}
    
    	}
    }
    ```

 �����
>public static void main(String[])
public final native void wait(long) throws InterruptedException
public final void wait() throws InterruptedException
public final void wait(long,int) throws InterruptedException
public boolean equals(Object)
public String toString()
public native int hashCode()
public final native Class getClass()
public final native void notify()
public final native void notifyAll()
public ShowMethods()

### 4. ����ģʽ��Java�еĶ�̬����
- ����ģʽ
���κ�ʱ�̣�ֻҪ����Ҫ������Ĳ����ӡ�ʵ�ʡ������з��뵽��ͬ�ĵط����ر��ǵ���ϣ���ܹ������׵������޸ģ���û��ʹ�ö������תΪʹ����Щ���������߷�����ʱ��������Եú����ã����ģʽ�Ĺؼ��Ƿ�װ�޸ģ������磬�����ϣ�����ٶ�ĳ�����з����ĵ��ã�����ϣ��������Щ���õĿ�������ô��Ӧ���������أ���Щ����**�϶����㲻ϣ������ϲ���Ӧ���еĴ��룬��˴���ʹ������Ժ����׵���ӻ��Ƴ����ǡ�**
    ```java
    interface Interface {
    	void doSomething();
    	void somethingElse(String arg);
    }
    
    class RealObject implements Interface {
    
    	@Override
    	public void doSomething() {
    		System.out.println("doSomething.");
    	}
    
    	@Override
    	public void somethingElse(String arg) {
    		System.out.println("somethingElse " + arg);
    	}
    }
    
    class SimpleProxy implements Interface {
    	
    	private Interface proxy;
    	
    	public SimpleProxy(Interface proxy) {
    		this.proxy = proxy;
    	}
    
    	@Override
    	public void doSomething() {
    		System.out.println("SimpleProxy doSomething.");
    		proxy.doSomething();
    	}
    
    	@Override
    	public void somethingElse(String arg) {
    		System.out.println("SimpleProxy somethingElse " + arg);
    		proxy.somethingElse(arg);
    	}
    }
    
    public class SimpleProxyDemo {
    	
    	public static void consumer(Interface iface) {
    		iface.doSomething();
    		iface.somethingElse("bonobo");
    	}
    
    	public static void main(String[] args) {
    		consumer(new RealObject());
    		consumer(new SimpleProxy(new RealObject()));
    	}
    
    }
    ```
�����
> doSomething.
somethingElse bonobo
SimpleProxy doSomething.
doSomething.
SimpleProxy somethingElse bonobo
somethingElse bonobo

- ��̬����
Java�Ķ�̬����ȴ����˼�����ǰ������һ������Ϊ������**��̬�ش���������̬�ش�������������ĵ��á�**
    ```java
    import java.lang.reflect.InvocationHandler;
    import java.lang.reflect.Method;
    import java.lang.reflect.Proxy;
    
    class DynamicProxyHandler implements InvocationHandler {
    	
    	private Object proxy;
    	
    	public DynamicProxyHandler(Object proxy) {
    		this.proxy = proxy;
    	}
    	
    	@Override
    	public Object invoke(Object proxy, Method method, Object[] args)
    			throws Throwable {
    		System.out.println("*** proxy: " + proxy.getClass() +
    				". method: " + method + ". args: " + args);
    		if(args != null) {
    			for(Object arg : args)
    				System.out.println(" " + arg);
    		}
    		return method.invoke(this.proxy, args);
    	}
    }
    
    public class SimpleDynamicProxy {
    	
    	public static void consumer(Interface iface) {
    		iface.doSomething();
    		iface.somethingElse("bonobo");
    	}
    
    	public static void main(String[] args) {
    		RealObject real = new RealObject();
    		consumer(real);
    		// insert a proxy and call again:
    		Interface proxy = (Interface)Proxy.newProxyInstance(
    				Interface.class.getClassLoader(), 
    				new Class[]{ Interface.class },
    				new DynamicProxyHandler(real));
    		
    		consumer(proxy);
    	}
    
    }
    ```
�����
> doSomething.
somethingElse bonobo
\*\*\* proxy: class typeinfo.\$Proxy0. method: public abstract void typeinfo.Interface.doSomething(). args: null
doSomething.
\*\*\* proxy: class typeinfo.\$Proxy0. method: public abstract void typeinfo.Interface.somethingElse(java.lang.String). args: [Ljava.lang.Object;@6a8814e9
 bonobo
somethingElse bonobo

### 5. ��ʱ���������� �� JIT
Java�����������฽�Ӽ������������ٶȣ��������������������صģ�����Ϊ����ʱ����Just-In-Time��JIT���������ļ�����**���ּ������԰ѳ���ȫ���򲿷ַ���ɱ��ػ����루�Ȿ����JVM�Ĺ����������������ٶ���˵���������**����Ҫװ��ĳ����ʱ�������������ҵ���.class�ļ���Ȼ�󽫸�����ֽ���װ���ڴ档��ʱ�������ַ����ɹ�ѡ��
��1��һ�־����ü�ʱ�������������д��롣����������������ȱ�ݣ����ּ��ض���ɢ���������������������ڣ��ۼ�����Ҫ������ʱ�䣻���һ����ӿ�ִ�д���ĳ��ȣ��ֽ���Ҫ�ȼ�ʱ������չ����ı��ػ�����С�ࣩܶ���⽫����ҳ����ȣ��Ӷ����ͳ����ٶȡ�
��2����һ��������Ϊ����������lazy evaluation������˼�Ǽ�ʱ������ֻ�ڱ�Ҫ��ʱ��ű�����룬�������Ӳ��ᱻִ�еĴ���Ҳ���ѹ�����ᱻJIT�����롣**�°�JDK�е�Java HotSpot�����Ͳ��������Ʒ���**������ÿ�α�ִ�е�ʱ�򶼻���һЩ�Ż�������ִ�еĴ���Խ�࣬�����ٶȾ�Խ�졣

### 6. ���ʿ���Ȩ��
- Java����Ȩ�����δʣ�**public��protected��������Ȩ�ޣ�Ĭ�Ϸ���Ȩ�ޣ���ʱҲ��friendly����private��**
- ������Ȩ�ޣ���ǰ���е�������������Ǹ���Ա���з���Ȩ�ޣ������������֮��������࣬�����Աȴ��private��
- protected���̳з���Ȩ�ޡ���ʱ����Ĵ����߻�ϣ����ĳ���ض���Ա���Ѷ����ķ���Ȩ�޸�������������������ࡣ�����Ҫprotected�������һ������protectedҲ�ṩ������Ȩ�ޣ�Ҳ����˵����ͬ���ڵ������඼���Է���protectedԪ�ء�protectedָ��**�������û����ԣ�����private�ģ��������κμ̳��ڴ���ĵ�����������κ�λ��ͬһ�����ڵ�����˵����ȴ�ǿ��Է��ʵġ�**�����磺
���ࣺ
    ```java
    package access.cookie;
    public class Cookie {
        public Cookie() {
            System.out.println("Cookie Constructor");
        }
     
        void bite() {  // ������Ȩ�ޣ���������ʹ������Ҳ���ܷ�����
            System.out.println("bite");
        }
    }
    ```
���ࣺ
    ```java
    package access.dessert;
    import access.cookie.Cookie;

    public class ChocolateChip extends Cookie {

    	public ChocolateChip() {
    		System.out.println("ChocolateChip constructor");
    	}
    	
    	public void chomp() {
    		bite();  // error, the method bite() from the type Cookie is not visible
    	}
    }
    ```
���Է������ಢ���ܷ��ʻ���İ�����Ȩ�޷�������ʱ���Խ�Cookie�е�biteָ��Ϊpublic����**���������е��˾Ͷ����˷���Ȩ�ޣ�Ϊ��ֻ����������ʣ����Խ�biteָ��Ϊprotected���ɡ�**

### 7. ��Ϻͼ̳�֮���ѡ��
- ��Ϻͼ̳�**���������µ����з����Ӷ���**���������ʽ�������������̳�������ʽ������
- **��ϼ���ͨ����������������ʹ��������Ĺ��ܶ������Ľӿ���������**������������Ƕ��ĳ����������ʵ������Ҫ�Ĺ��ܣ���������û�������ֻ��Ϊ����������Ľӿڣ�������Ƕ�����Ľӿڡ�Ϊȡ�ô�Ч������Ҫ��������Ƕ��һ���������private���󡣵�**��ʱ����������û�ֱ�ӷ��������е���ϳɷ��Ǽ�������ģ�������Ա��������Ϊpublic**�������Ա�������������˾���ʵ�֣���ô���������ǰ�ȫ�ġ����û��ܹ��˽⵽��������װһ�鲿��ʱ����ʹ�ö˿ڸ���������⡣����Car�������public��Engine����Wheel����Window�����Door������ϡ������Ҫ�ǵ��������һ��������**һ�������Ӧ��ʹ���Ϊprivate**��
- �ڼ̳е�ʱ��ʹ��ĳ�������࣬������һ����������汾��ͨ����**����ζ������ʹ��һ��ͨ���࣬��Ϊ��ĳ��������Ҫ���������⻯��**��΢˼��һ�¾ͻᷢ�֣���һ������ͨ���ߡ�����������һ�������ӡ��Ǻ�������ģ���Ϊ�����ӡ�������������ͨ���ߡ���������һ�ֽ�ͨ���ߣ�is-a��ϵ����
- **��is-a������һ�����Ĺ�ϵ���ü̳������ģ�����has-a������һ�����Ĺ�ϵ���������������**��
- �����Ǹ�����ϻ��Ǽ̳У�һ�����������жϷ���������һ���Լ��Ƿ���Ҫ������������������ת�ͣ���Ҫ�Ļ����ü̳У�����Ҫ�Ļ�������Ϸ�ʽ��

### 8. final�ؼ���
- ��final�ؼ��ֵ����
��final���ε��ǻ�����������ʱ����ָ������ֵ�㶨���䣨���Ǳ����ڳ����������static final���Σ���ǿ��ֻ��һ�ݣ������Զ������ö����ǻ�����������finalʱ���京�����һ�������Ի���Ϊ���ڶ�������ʱ��finalʹ���ú㶨���䣬һ�����ñ���ʼ��ָ��һ�����󣬾��޷��ٰ���ָ����һ������Ȼ����**����������ȴ�ǿ��Ա��޸ĵ�**��Java��δ�ṩʹ�κζ���㶨�����;�����������Լ���д����ȡ��ʹ����㶨�����Ч��������һ����ͬ���������飬��Ҳ�Ƕ���
- **ʹ��final������Ŀ�����߳���Ч����**
��һ���������final�󣬱������Ϳ��԰Ѷ��Ǹ����������е��ö����롰Ƕ�롱�����ֻҪ����������һ��final�������ã��ͻᣨ�������Լ����жϣ�����Ϊִ�з������û��ƶ���ȡ�ĳ��������뷽�������Ա���ѹ���ջ�������������벢ִ�������������������ջ�Ա��������Է���ֵ���д������෴��**�����÷���������ʵ�ʴ����һ���������滻��������**���������ɱ��ⷽ������ʱ��ϵͳ��������Ȼ�����������̫����ô����Ҳ����Ӻ�ף������ܵ�������Ƕ��������������κ�������������Ϊ�κ������������ڷ����ڲ���ʱ������ˡ�

 �������Java�汾�У���������ر���hotspot���������Զ������Щ���������Ϊ�����ǡ��ؾ����Ƿ�Ƕ��һ��final ������Ȼ������û��ǲ�Ҫ��ȫ���ű���������ȷ�����������жϡ�ͨ����**ֻ���ڷ����Ĵ������ǳ��٣���������ȷ��ֹ���������ǵ�ʱ�򣬲�Ӧ���ǽ�һ��������Ϊfinal��**

 ��������private �������Զ���Ϊfinal���������ǲ��ܷ���һ��private ���������������Բ��ᱻ�����������ǣ���ǿ���������������������������ʾ������Ϊһ��private�������finalָʾ������ȴ����Ϊ�Ǹ������ṩ�κζ���ĺ��塣

### 9. �������ģʽ��������ģʽ������
- �������ģʽ
����һ���ܹ����������ݵĲ�������Ĳ�ͬ�����в�ͬ��Ϊ�ķ���������Ϊ�������ģʽ�����෽��������Ҫִ�е��㷨�й̶�����Ĳ��֣��������ԡ������仯�Ĳ��֡����Ծ��Ǵ��ݽ�ȥ�Ĳ�������������Ҫִ�еĴ��롣
- ������ģʽ
�����޷��޸�����Ҫʹ�õ���ʱ������ʹ��������ģʽ���������еĴ��뽫��������ӵ�еĽӿڣ�������������Ҫ�Ľӿڡ�

### 10. �ڲ���
- �ڲ������������ȫ��ͬ�ĸ����һ�����Ҫ��
- Ϊʲô��Ҫ�ڲ��ࣿ �� **��Ҫ�ǽ���˶�̳е����⣬�̳о���������**
    - һ����˵���ڲ���̳���ĳ�����ʵ��ĳ���ӿڣ��ڲ���Ĵ����������������Χ��Ķ������Կ�����Ϊ**�ڲ����ṩ��ĳ�ֽ�������Χ��Ĵ��ڡ�**
    - �ڲ����������˵�ԭ���ǣ�ÿ���ڲ��඼�ܶ����ؼ̳���һ�����ӿڵģ�ʵ�֣�����������Χ���Ƿ��Ѿ��̳���ĳ�����ӿڵģ�ʵ�֣������ڲ��඼û��Ӱ�졣
    - ���û���ڲ����ṩ�ġ����Լ̳ж������Ļ��������������һЩ�����������ͺ��ѽ����������Ƕȿ����ڲ���ʹ�ö��ؼ̳еĽ����������������ӿڽ���˲������⣬���ڲ�����Ч��ʵ���ˡ����ؼ̳С���Ҳ����˵��**�ڲ�������̳ж���ǽӿ����͡�**

     ��������һ�����Σ����������һ��������ĳ�ַ�ʽʵ�������ӿڡ����ڽӿڵ�����ԣ���������ѡ��ʹ�õ�һ�����ʹ���ڲ��ࡣ�����ӵ�е��ǳ������������࣬�����ǽӿڣ��Ǿ�ֻ��ʹ���ڲ������ʵ�ֶ��ؼ̳С�

 ʹ���ڲ��࣬�����Ի������һЩ���ԣ�
    - �ڲ�������ж��ʵ����ÿ��ʵ�������Լ���״̬��Ϣ������������Χ��������Ϣ�໥������
    - �ڵ�����Χ���У������ö���ڲ����Բ�ͬ�ķ�ʽʵ��ͬһ���ӿڻ�̳�ͬһ���ࡣ
    - �����ڲ�������ʱ�̲�����������Χ�����Ĵ�����
    - �ڲ��ಢû�������Ի��is-a��ϵ��������һ��������ʵ�塣

### 11. String���� �� ���ɱ�
- **����String�ġ�+���롰+=����Java�н��е��������ع��Ĳ���������Java�����������Ա�����κβ�������**
- ���ǵ�Ч�����أ����������String�Ķ��+���������Ż����Ż�ʹ��`StringBuilder`������`javap -c class�ֽ����ļ��� ����`�鿴�����Ż����̣�����������ÿ�������ʹ��String���󣬷�����������Ϊ���Զ����Ż����ܡ������������Ż���ʲô�̶Ȼ�����˵����һ�����Ż���ʹ��StringBuilder����String��ͬ��Ч�������磺
    ```java
    public class WitherStringBuilder {
    	public String implicit(String[] fields) {
    		String result = "";
    		for(int i = 0; i < fields.length; i++)
    			result += fields[i];
    		return result;
    	}
    	
    	public String explicit(String[] fields) {
    		StringBuilder result = new StringBuilder();
    		for(int i = 0; i < fields.length; i++)
    			result.append(fields[i]);
    		return result.toString();
    	}
    }
    ```
����`javap -c WitherStringBuilder`�����Կ�������������Ӧ���ֽ��롣
**implicit������**
> public java.lang.String implicit(java.lang.String[]);
   Code:
      0: ldc #16 // String
      2: astore_2
      3: iconst_0
      4: istore_3
      **5: goto 32
      8: new #18 // class java/lang/StringBuilder**
     11: dup
     12: aload_2
     13: invokestatic #20 // Method java/lang/String.valueOf:(
Ljava/lang/Object;)Ljava/lang/String;
     16: invokespecial #26 // Method java/lang/StringBuilder."<
init>":(Ljava/lang/String;)V
     19: aload_1
     20: iload_3
     21: aaload
     22: invokevirtual #29 // Method java/lang/StringBuilder.ap
pend:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     25: invokevirtual #33 // Method java/lang/StringBuilder.to
String:()Ljava/lang/String;
     28: astore_2
     29: iinc 3, 1
     32: iload_3
     33: aload_1
     34: arraylength
     35: if_icmplt 8
     38: aload_2
     39: areturn
 public java.lang.String implicit(java.lang.String[]);
   Code:
      0: ldc #16 // String
      2: astore_2
      3: iconst_0
      4: istore_3
      5: goto 32
      8: new #18 // class java/lang/StringBuilder
     11: dup
     12: aload_2
     13: invokestatic #20 // Method java/lang/String.valueOf:(
Ljava/lang/Object;)Ljava/lang/String;
     16: invokespecial #26 // Method java/lang/StringBuilder."<
init>":(Ljava/lang/String;)V
     19: aload_1
     20: iload_3
     21: aaload
     22: invokevirtual #29 // Method java/lang/StringBuilder.ap
pend:(Ljava/lang/String;)Ljava/lang/StringBuilder;
     25: invokevirtual #33 // Method java/lang/StringBuilder.to
String:()Ljava/lang/String;
     28: astore_2
     29: iinc 3, 1
     32: iload_3
     33: aload_1
     34: arraylength
     35: if_icmplt 8
     38: aload_2
     39: areturn

 ���Է��֣�StringBuilder����ѭ��֮�ڹ���ģ�����ζ��ÿ����ѭ��һ�Σ��ͻᴴ��һ���µ�StringBuilder����

 **explicit������**
  >public java.lang.String explicit(java.lang.String[]);
    Code:
       0: **new #18 // class java/lang/StringBuilder**
       3: dup
       4: invokespecial #45 // Method java/lang/StringBuilder."<
init>":()V
       7: astore_2
       8: iconst_0
       9: istore_3
      **10: goto 24**
      13: aload_2
      14: aload_1
      15: iload_3
      16: aaload
      17: invokevirtual #29 // Method java/lang/StringBuilder.ap
pend:(Ljava/lang/String;)Ljava/lang/StringBuilder;
      20: pop
      21: iinc 3, 1
      24: iload_3
      25: aload_1
      26: arraylength
      27: if_icmplt 13
      30: aload_2
      31: invokevirtual #33 // Method java/lang/StringBuilder.to
String:()Ljava/lang/String;
      34: areturn
}

 ���Կ���������ѭ�����ֵĴ������̡����򵥣�������ֻ������һ��StringBuilder������ʽ�Ĵ���StringBuilder��������Ԥ��Ϊ��ָ����С��**������Ѿ�֪�����յ��ַ�������ж೤����Ԥ��ָ��StringBuilder�Ĵ�С���Ա��������·��仺�塣**
 
  --- 
 ####�ܽ�
 ��ˣ�����Ϊһ������дtoString()����ʱ������ַ��������Ƚϼ򵥣��ǾͿ�������������������Ϊ�����ع������յ��ַ�����������ǣ������Ҫ��toString()������ʹ��ѭ������ô����Լ�����һ��StringBuilder�����������������յĽ����
 
 ---

- `System.out.printf()`��`System.out.format()`����ģ����C��`printf`�����Ը�ʽ���ַ�������������ȫ�ȼ۵ġ�

 Java�У������µĸ�ʽ�����ܶ���`java.util.Formatter`�ദ��
`String.format()`�����ο���C�е�`sprintf()`�����������ɸ�ʽ����String������һ��static��������������`Formatter.format()`����һ���Ĳ�����������һ��String���󡣵���ֻ��ʹ��`format()`����һ�ε�ʱ�򣬸÷����ܷ��㡣
    ```java
    import java.util.Arrays;
    import java.util.Formatter;
    
    public class SimpleFormat {
    
    	public static void main(String[] args) {
    		int x = 5;
    		double y = 5.324667;
    		System.out.printf("Row 1: [%d %f]\n", x, y);
    		System.out.format("Row 1: [%d %f]\n", x, y);
    		
    		Formatter f = new Formatter(System.out);
    		f.format("Row 1: [%d %f]\n", x, y);
    		
    		String str = String.format("Row 1: [%d %f]\n", x, y);
    		System.out.println(str);
    		
    		Integer[][] a = {
    			{1, 2, 3}, {4, 5, 6},
    			{7, 8, 3}, {9, 10, 6}
    		};
    		System.out.println(Arrays.deepToString(a));
    	}
    }
    ```

### 12. ���л�����
- �����Ƕ����л����п���ʱ������ĳ���ض��Ӷ�������Java���л������Զ�������ָ�������Ӷ����ʾ�������ǲ�ϣ���������л���������Ϣ�������룩��ͨ�������������������ʹ�����е���Щ��Ϣ��private���ԣ�һ�����л��������ǾͿ���ͨ����ȡ�ļ������������紫��ķ�ʽ�����ʵ����������ְ취���Է�ֹ��������в��ֱ����л���
    - ʵ��`Externalizable`����ʵ��`Serializable`�ӿ��������л����̽��п��ƣ�`Externalizable`�̳���`Serializable`�ӿڣ�ͬʱ����������������`writeExternal()`��`readExternal()`��

     **�����ڷ����л�ʱ������**��
     - ��Serializable�������л�ʱ��**����Serializable������ȫ�����洢�Ķ�����λΪ���������죬��˲���������κι��캯����**���Serializable������Ĭ�Ϲ��캯�������ǵ�Serializable��ĸ���û��ʵ��Serializable�ӿ�ʱ�������л����̻���ø����Ĭ�Ϲ��캯������˸ø��������Ĭ�Ϲ��캯������������쳣��
     - ��Externalizable�������л�ʱ��**���ȵ�����Ĳ��������Ĺ��췽��**�������б���Ĭ�Ϸ����з�ʽ�ġ��������Ĳ��������Ĺ��췽��ɾ�������߰Ѹù��췽���ķ���Ȩ������Ϊprivate��Ĭ�ϻ�protected���𣬻��׳�`java.io.InvalidException: no valid constructor`�쳣�����**Externalizable���������Ĭ�Ϲ��캯�������ұ�����public�ġ�**
 - `Externalizable`�������������������ر���ʵ��Externalizable�ӿڣ���ô������һ�ַ��������ǿ���ʵ��`Serializable`�ӿڣ������`writeObject()`��`readObject()`�ķ�����һ���������л���������װ�䣬�ͻ�ֱ����������������Ҳ����˵��**ֻҪ�ṩ���������������ͻ�����ʹ�����ǣ���������Ĭ�ϵ����л����ơ�**

     ��Щ�������뺬������׼ȷ��ǩ����
    ```java
    private void writeObject(ObjectOutputStream stream) 
            throws IOException;
    private void readObject(ObjectInputStream stream)
            throws IOException, ClassNotFoundException
    ```
 - ������`transient`�ؼ�������ֶεعر����л���������˼�ǡ�**�����鷳�㱣���ָ����ݡ����Լ��ᴦ���**��������Externalizable������Ĭ������²��������ǵ��κ��ֶΣ�����**transient�ؼ���ֻ�ܺ�Serializable����һ��ʹ�á�**