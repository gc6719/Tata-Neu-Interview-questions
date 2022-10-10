# Tata Neu Interview questions

## Descriptive Questions

### **1. Can we nest the Scaffold widget? Why or Why not?**

>Scaffold is a Widget which cover full screen available and used to implement basic Material design like - App Bar , drawer , bottom navigation bar, floating action button , messaging bars etc on providing child widget to each of these properties
	Scaffold is just like any other widget, we can use nested scaffold widget but we end up rendering multiple times.Most apps use a single scaffold widget for each screen (App bar or bottom nav bar or drawer ).

    1. Scaffold Sample : 
    Scaffold(
      appBar: AppBar(
        title: const Text('App Title '),
      ),
      body: Center(child: Text('App Body code')),
      floatingActionButton: FloatingActionButton(
        onPressed:   actionFun,
        tooltip: 'PressMe',
        child: const Icon(Icons.add),
      ),
    );
    
    2. Nested Scaffold:
    Scaffold(
      appBar: AppBar(
        title: const Text('App Title '),
      ),
      body:Scaffold( 
         body: Center( child: RaisedButton( child: Text('Open route'), onPressed: () {}, ), ), ),
      floatingActionButton: FloatingActionButton(
        onPressed:   actionFun,
        tooltip: 'PressMe',
        child: const Icon(Icons.add),
      ),
    );



### **2. How to reduce the rebuilding of the widget?**

>Building or creation of a widget takes place with the help of the build method. This method is called multiple times in stateful widget - setState method , route push - pop , change device screen resize due to keyboard or screen orientation change etc.
	This can be avoided by moving the child widget into a separate stateful widget class along with “const” keyword for creation and using providers to handle state changes for widget.


### **3. How can I access platform(iOS or Android) specific code from Flutter?**
>Flutter allows us to call platform specific code like java/kotlin for android and obj-c/swift for IOS
We have to use Message-channel /MethodChannel .
	Method calls and responses are asynchronous in nature so we have to use async/ await or then method to handle

    Dart : 
    static const platform = const MethodChannel('app.native/helper');
    String result = await  platform.invokeMethod('callMyNativeCode');

Java:
	
	
	private static final String CHANNEL = "app.native/helper";
	
	new MethodChannel(getFlutterView(), CHANNEL).setMethodCallHandler(
        new MethodChannel.MethodCallHandler() {
          @Override
          public void onMethodCall(MethodCall call, MethodChannel.Result result) {
            if (call.method.equals("'callMyNativeCode'")) {
              String greetings = "Native code response";
              result.success(greetings);
            }
          }});



### **4. What is BuildContext? What is its importance?**
	

>Buildcontext is the location or identifier used to know where each widget in the tree is present and their position and it is the context in which a specific widget is built.
	BuildContext is passed to the build method of each widget, which will return a widget to the widget tree for rendering. The BuildContext of Widget and its child widget are different from each other and unique.
	
With Below example : Buildcontext at calling build method and inside MySampleWidget’s build method are different 

    Widget build(BuildContext context)=> MySampleWidget();
    
    class MySampleWidget extends StatelessWidget{
        @override 
        Widget build(BuildContext context)  
            return Container(
                child: Text("Child"),
        );
    }



## Coding Questions:

### **1. In the below code, list1 declared with var, list2 with final and list3 with const. What is the difference between these lists? Will the last two lines compile?**

- var is a keyword allowing to create a variable without specifying a type
- final keyword allows you to create variables that can only be assigned once at runtime.
- const keyword to create constant values , value of this variable must be known at compile time

		 var list1 = ['I' , 'Love' , 'Flutter'];
    	final list2 = list1;
    	list2[2] = 'Dart' ; // This line will compile and execute  
    	const list3 = list1; // This line of code will not compile 
        //Cannot compile
      //Error : Const variables must be initialized with a constant value

### **2. Identify the problem in the following code block and correct it.**

>Here code works properly but we have something like warring/info :
name non-constant identifiers using lowerCamelCase 
- Need to change LongOperationMethod  to longOperationMethod



### **3. What is the main difference between Mobx & Provider? Can we use both together? Write code to demonstrate it.**

>Provider is one of the state management packages for flutter. In Provider, widgets can listen to changes in state and get updates when notified. Instead of rebuilding the entire widget tree , we can change only the affected portion of the widget tree (particular widget only).

- Step 1: Implement Provider class by extending ChangeNotifier

    
      
      class Auth with ChangeNotifier {
	      String _token;
	      String get token {
	        return _token;
	      }
	      Future<void> fetchToken() async {
	        _token = await getTokenFromApi();
	        notifyListeners();
	      }
       }
   

- Step 2: Add provider instance at time of app start

    void main() => runApp(
    	  ChangeNotifierProvider<Auth>(
    	  create: (_) => Auth(),
    	  child: MyApp(),
    	  ));   
        
        class MyApp extends StatelessWidget {
        	     @override
        	     Widget build(BuildContext context) {
        	     return const MaterialApp(
        	      title: 'Material App',
        	      home: HomeScreen(),
        	     );
        	     }
            }

- Step 3: Access Provider Auth variable token in widget required
 

    Consumer<Auth>(
    	    builder: (context, provider, child) {
    	    return Text('Hi ' + provider.token,);
    })
	

##### I have not worked on Mobx but have worked with Providers
