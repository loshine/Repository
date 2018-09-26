# IPC

## 多进程

在 Android 中使用多进程只有一种方法，就是给四大组件（**Activity**、**Service**、**BroadcastReceiver**、**ContentProvider**）在 AndroidMenifest 中指定`android:process`属性。

> 还可以通过 JNI 在 native 层去 fork 一个新进程，此处不考虑。

如下是一个开启多进程的示例：

```xml
    <activity
        android:name="com.ryg.chapter_2.MainActivity"
        android:configChanges="orientation|screenSize"
        android:label="@string/app_name"
        android:launchMode="standard" >
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>
    <activity
        android:name="com.ryg.chapter_2.SecondActivity"
        android:configChanges="screenLayout"
        android:label="@string/app_name"
        android:process=":remote" />
    <activity
        android:name="com.ryg.chapter_2.ThirdActivity"
        android:configChanges="screenLayout"
        android:label="@string/app_name"
        android:process="com.ryg.chapter_2.remote" />
```

上例中分别为 SecondActivity 和 ThirdActivity 指定了 process 属性，其中 SecondActivity 所处的进程名为`com.ryg.chapter_2:remote`，而 ThirdActivity 所处的进程名为`com.ryg.chapter_2.remote`。

> 以`:`开头的进程属于私有进程，其它应用的组件不可以和它跑在一个进程里。而其它非`:`开头的进程则可以通过 ShareUID 且拥有相同的签名运行在同一个进程中。

## 进程间通信

IPC 即 **Inter-Process Communication**（进程间通信），Android 中的进程间通信包括以下几种：

1. Intent 和 Bundle 传输
2. 文件共享
3. Messenger
4. AIDL
5. ContentProvider
6. Socket

### 1. Intent 和 Bundle 传输

### 2. 文件共享

### 3. Messenger

使用 Messenger 可以在不同进程间传递 Message 对象，在 Message 中放入需要传递的数据，就可以实现跨进程通信了。

从其构造函数可以看出其实 Messenger 是基于 AIDL 的一种轻量级 IPC 实现。

```java
public Messenger(Handler target) {
    mTarget = target;
}

public Messenger(IBinder target) {
    mTarget = IMessenger.Stub.asInterface(target);
}
```

Messenger 的使用非常简单，而且因为它一次只处理一个请求，所以我们不需要考虑线程同步问题。

#### 服务端

在服务端创建一个 Service 来处理客户端的连接请求，同时创建一个 Handler 并通过它来创建一个 Messenger 对象，然后在 Service 的 onBind 中返回 Messenger 对象底层的 Binder。

```java

public class MessengerService extends Service {

    private static final String TAG = "MessengerService";

    private final Messenger mMessenger = new Messenger(new MessengerHandler());

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return mMessenger.getBinder();
    }

    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case Constants.MSG_FROM_CLIENT:
                    Log.i(TAG, "receive msg from client: " + msg.getData().getString("msg"));
                    Messenger replyTo = msg.replyTo;
                    Message message = Message.obtain();
                    Bundle bundle = new Bundle();
                    bundle.putString("reply", "收到消息了");
                    message.setData(bundle);
                    try {
                        replyTo.send(message);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                    break;
                default:
                    super.handleMessage(msg);
                    break;
            }
        }
    }
}

```

然后注册服务端，运行在单独的进程中。

#### 客户端

绑定远程 MessengerService，然后根据服务端返回的 binder 对象创建 Messenger 对象并使用此对象向服务端发送消息。

```java

public class MainActivity extends AppCompatActivity {

    private static final String TAG = "MainActivity";

    private Messenger mServiceMessenger;
    private MessengerHandler mHandler = new MessengerHandler();
    private Messenger mMessenger = new Messenger(mHandler);

    private ServiceConnection mConnection = new ServiceConnection() {

        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            mServiceMessenger = new Messenger(service);
            Message message = Message.obtain();
            message.what = Constants.MSG_FROM_CLIENT;
            message.replyTo = mMessenger; // 设置返回信息处理 Messenger
            Bundle bundle = new Bundle();
            bundle.putString("msg", "hello, this is a message from client.");
            message.setData(bundle);
            try {
                mServiceMessenger.send(message);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Intent intent = new Intent(this, MessengerService.class);
        bindService(intent, mConnection, BIND_AUTO_CREATE);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unbindService(mConnection);
    }

    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);
            Log.i(TAG, "receive msg from service: " + msg.getData().getString("reply"));
        }
    }
}
```

### 4. AIDL

AIDL是一个缩写，全称是 Android Interface Definition Language，也就是 Android 接口定义语言。设计这门语言的目的是为了**实现进程间通信，尤其是在涉及多进程并发情况下的进程间通信**。

AIDL 的语法与 Java 基本相似，扩展名是`.aidl`。

#### 支持的数据类型

AIDL 默认支持以下数据类型：

* `byte`
* `short`
* `int`
* `long`
* `float`
* `double`
* `boolean`
* `char`
* `String`
* `CharSequence`
* `List` : List 中的所有元素必须是 AIDL 支持的类型之一，或者是一个其他 AIDL 生成的接口，或者是定义的 parcelable，支持泛型。
* `Map` : Map 中的所有元素必须是 AIDL 支持的类型之一，或者是一个其他 AIDL 生成的接口，或者是定义的 parcelable，不支持泛型。

#### 定向 tag

AIDL 中的定向 tag 表示了在跨进程通信中数据的流向。

* `in` : 表示数据只能由客户端流向服务端
* `out` : 表示数据只能由服务端流向客户端
* `inout` : 表示数据可在服务端与客户端之间双向流通

数据流向是针对在客户端中的那个传入方法的对象而言的。in 为定向 tag 的话表现为服务端将会接收到一个那个对象的完整数据，但是客户端的那个对象不会因为服务端对传参的修改而发生变动；out 的话表现为服务端将会接收到那个对象的的空对象，但是在服务端对接收到的空对象有任何修改之后客户端将会同步变动；inout 为定向 tag 的情况下，服务端将会接收到客户端传来对象的完整信息，并且客户端将会同步服务端对该对象的任何变动。

> Java 中的基本类型和 String ，CharSequence 的定向 tag **默认且只能是 in** 。

#### 使用

##### 4.1. 需要被跨进程传输的数据类型如果不是以上支持的，需要实现`Parcelable`

```java
// Book.aidl
// 这个文件的作用是引入了一个序列化对象 Book 供其他的 AIDL 文件使用
// 注意：Book.aidl 与 Book.java 的包名应当是一样的
package com.lypeer.ipcclient;

//注意parcelable是小写
parcelable Book;
```

##### 4.2. 编写对应的 AIDL 文件

```java
// BookManager.aidl
package com.lypeer.ipcclient;
// 导入所需要使用的非默认支持数据类型的包
import com.lypeer.ipcclient.Book;

interface BookManager {

    // 所有的返回值前都不需要加任何东西，不管是什么数据类型
    List<Book> getBooks();
    Book getBook();
    int getBookCount();

    // 传参时除了 Java 基本类型以及 String，CharSequence 之外的类型
    // 都需要在前面加上定向 tag，具体加什么量需而定
    void setBookPrice(in Book book , int price)
    void setBookName(in Book book , String name)
    void addBookIn(in Book book);
    void addBookOut(out Book book);
    void addBookInout(inout Book book);
}
```

##### 4.3. 定义接口

##### 4.4. Build 项目生成对应的`.java`文件

##### 4.5. 编写服务端代码，在`onBind()`中返回对应 AIDL 的 Stub 并重写 Stub 中的方法

##### 4.6. 客户端绑定服务，使用`Stub.asInterface()`获取生成的 AIDL 的对应 Java 类

### 5. ContentProvider

ContentProvider 底层由 Binder 实现，天生支持进程间通信。

### 6. Socket

Socket 创建服务端和客户端连接通信即可。