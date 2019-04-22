# Intent-WebView
自定义WebView验证隐式Intent的使用
## 通过自定义WebView加载URL来验证隐式Intent的使用。
## 实验包含两个应用：
第一个应用：获取URL地址并启动隐式Intent的调用。

第二个应用：自定义WebView来加载URL
## 一、新建一个工程用来获取URL地址并启动Intent
### 布局代码(.xml):
    <EditText
        android:id="@+id/et"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="http://www.baidu.com"
        app:layout_constraintLeft_toRightOf="parent"
        app:layout_constraintRight_toLeftOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
    <Button
        android:id="@+id/b"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="浏览该网页"
        android:onClick="onClick"
        app:layout_constraintTop_toBottomOf="@id/et"
        app:layout_constraintRight_toLeftOf="parent"
        app:layout_constraintLeft_toRightOf="parent"/>
### 点击启动Intent:
    public void onClick(View view){
        Intent intent=new Intent();
        editText=findViewById(R.id.et);
        intent.setAction("android.intent.action.VIEW");
        String url=editText.getText().toString();
        intent.putExtra("address",url);
        startActivity(intent);
    }
## 二、新建一个工程使用WebView来加载URL
### 自定义WebView:
    <WebView
        android:id="@+id/wv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"

        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
### MyBrowser(.java)
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        webView=findViewById(R.id.wv);
        webView.setWebViewClient(new WebViewClient());
        Intent intent=getIntent();
        Bundle bundle=intent.getExtras();
        if(bundle!=null) {
            String address=bundle.getString("address");
            webView.loadUrl(address);
        }
    }
## 注意事项：
### 从Android 9.0（API级别28）开始，默认情况下禁用明文支持。因此http的url均无法在webview中加载。
### 解决办法：在AndroidManifest.xml添加如下代码
1、添加网络权限：在manifest标签中添加

    <uses-permission android:name="android.permission.INTERNET"/>
2、在application中添加

    android:usesCleartextTraffic="true"
## 结果截图如下：
<img width="200" src="https://github.com/dj-jun/Intent-WebView/blob/master/pictures/1.png"/>

<img width="200" src="https://github.com/dj-jun/Intent-WebView/blob/master/pictures/2.png"/>

<img width="200" src="https://github.com/dj-jun/Intent-WebView/blob/master/pictures/3.png"/>
