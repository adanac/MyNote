##onClick、onLongClick与onTouchEvent
>  可以理解为一次 Click ，也可以理解为发生了一次 ACTION_DOWN 和 ACTION_UP ，那么 Android 是如何理解和处理的呢？
在 Android 中， onClick 、 onLongClick 的触发是和 ACTION_DOWN 及 ACTION_UP 相关的，在时序上，如果我们在一个 View 中同时覆写了 onClick 、 onLongClick 及 onTouchEvent 的话， onTouchEvent 是最先捕捉到 ACTION_DOWN 和 ACTION_UP 事件的，其次才可能触发 onClick 或者 onLongClick 。


`getApplicationContext () 返回应用的上下文，生命周期是整个应用，应用摧毁它才摧毁`



`Activity
.
this
的
context 
返回当前
activity
的上下文，属于
activity 
，
activity 
摧毁他就摧毁`

`getBaseContext
()
  
返回由构造函数指定或
setBaseContext
()设置的上下文`


```

@Override

	
public
 boolean onTouchEvent
(
MotionEvent
 e
)
 
{

		
TextView
 str0 
=
 
(
TextView
)
 findViewById
(
R
.
string
.
view1_str0
);

		str0
.
setOnClickListener
(
new
 
View
.
OnClickListener
()
 
{


			
@Override

			
public
 
void
 onClick
(
View
 v
)
 
{

				
ComponentName
 componentname 
=
 
new
 
ComponentName
(

						
MainContent
.
this
,

						
"com.adanac.fakeweixin.activity.MainContent"
);

				
Intent
 intent 
=
 
new
 
Intent
();

				intent
.
setComponent
(
componentname
);

				startActivity
(
intent
);


			
}

		
});

		
return
 
true
;


	
}

```
## 动态更改屏幕方向
```
//获取资源 
     bt1  = (Button )findViewById( R.id . bt1 ) ; 
     //增加按钮事件 
    bt1.setOnClickListener( new   Button .OnClickListener(){ 
         @Override
         public   void   onClick(View v)  {
             // 如果是竖排,则改为横排
             if  (getRequestedOrientation() == ActivityInfo. SCREEN_ORIENTATION_LANDSCAPE ) {
                setRequestedOrientation(ActivityInfo. SCREEN_ORIENTATION_PORTRAIT );
            }
    
             // 如果是横排,则改为竖排
    
             else   if  (getRequestedOrientation() == ActivityInfo. SCREEN_ORIENTATION_PORTRAIT ) {
                setRequestedOrientation(ActivityInfo. SCREEN_ORIENTATION_LANDSCAPE );
            }
        }     });  
``` 
