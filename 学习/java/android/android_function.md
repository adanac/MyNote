##Android 利用发送Intent播放本地视频和网络视频
```


1. 播放本地视频

 

               Intent intent = new Intent(Intent.ACTION_VIEW);
               String type = "video/mp4";
               Uri uri = Uri.parse("file:///sdcard/test.mp4");
               intent.setDataAndType(uri, type);
               startActivity(intent);     

 

2. 播放网络视频

 

               Intent intent = new Intent(Intent.ACTION_VIEW);
                String type = "video/* ";

     Uri uri = Uri.parse("http://forum.ea3w.com/coll_ea3w/attach/2008_10/12237832415.3gp");
               intent.setDataAndType(uri, type);
               startActivity(intent);     

 

          注意红色部分，如果不设置 type 的话，这样写：

             Intent intent = new Intent(Intent.ACTION_VIEW);

   Uri uri = Uri.parse("http://forum.ea3w.com/coll_ea3w/attach/2008_10/12237832415.3gp");
              intent.setData (uri);
              startActivity(intent);   

这样会默认用浏览器打开这个 URL！




来源：  http://blog.sina.com.cn/s/blog_a000da9d01011y85.html





1. sdcard
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setDataAndType(Uri.parse("/sdcard/movieclip.flv"), "video/*");
startActivity(intent);

2.network
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setDataAndType(Uri.parse("http://********/movieclip.flv"), "video/*");
startActivity(intent);


来源：  http://bbs.gfan.com/android-346736-1-1.html
``` ##播放视频
xml文件
```
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout
  xmlns:android="http://schemas.android.com/apk/res/android"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:orientation="vertical">
  
  <VideoView android:id="@+id/videoview"
    android:layout_width="fill_parent"
      android:layout_height="wrap_content"
    android:layout_gravity="center_horizontal"
  /> </LinearLayout>  

```
实现类
```
/**
 * 功能：播放视频
 * 
 * @author adanac
 * @date 2015-12-17
 * @version 1.0
 */
public class VideoViewDemo extends Activity implements
        MediaPlayer.OnErrorListener, MediaPlayer.OnCompletionListener {
    public static final String TAG = "VideoPlayer";
    private VideoView mVideoView;
    private Uri mUri;
    private int mPositionWhenPaused = -1;
    private MediaController mMediaController;
    /*
     * (non-Javadoc)
     * 
     * @see android.app.Activity#onCreate(android.os.Bundle)
     */
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.video_videoview);
        this.setRequestedOrientation(ActivityInfo.SCREEN_ORIENTATION_LANDSCAPE);
        mVideoView = (VideoView) findViewById(R.id.videoview);
        // 文件路径
        mUri = Uri.parse(Environment.getExternalStorageDirectory()
                + "/媒介素养.flv");
        // Create media controller
        mMediaController = new MediaController(this);
        // 设置MediaController
        mVideoView.setMediaController(mMediaController);
    }
    // 监听MediaPlayer上报的错误信息
    @Override
    public boolean onError(MediaPlayer mp, int what, int extra) {
        return false;
    }
    // Video播完的时候得到通知
    @Override
    public void onCompletion(MediaPlayer mp) {
        this.finish();
    }
    // 开始
    public void onStart() {
        // Play Video
        mVideoView.setVideoURI(mUri);
        mVideoView.start();
        super.onStart();
    }
    // 暂停
    public void onPause() {
        // Stop video when the activity is pause.
        mPositionWhenPaused = mVideoView.getCurrentPosition();
        mVideoView.stopPlayback();
        Log.d(TAG, "OnStop: mPositionWhenPaused = " + mPositionWhenPaused);
        Log.d(TAG, "OnStop: getDuration  = " + mVideoView.getDuration());
        super.onPause();
    }
    public void onResume() {
        // Resume video player
        if (mPositionWhenPaused >= 0) {
            mVideoView.seekTo(mPositionWhenPaused);
            mPositionWhenPaused = -1;
        }
        super.onResume();
    } } 
``` 

## TextView显示插入的图片
Android系统默认给TextView插入图片提供了三种方式：
1、ImageSpan
2、Html.ImageGetter
3、TextView.setCompoundDrawables(left, top, right, bottom)
1、TextView使用ImageSpan显示图片
```
ImageSpan span = new ImageSpan(this, R.drawable.ic_launcher);
SpannableString spanStr = new SpannableString("http://orgcent.com");
spanStr.setSpan(span, spanStr.length()-1, spanStr.length(), Spannable.SPAN_INCLUSIVE_EXCLUSIVE);
mTVText.setText(spanStr);
```
PS:关于SpannableString相关的其他span，查看[Android教程]TextView使用SpannableString设置复合文本
2、使用Html.ImageGetter显示网页中的图片
查看文章：[Android教程]TextView显示Html类解析的网页和图片及自定义标签
3、在TextView四周显示图片
```
mTVText.setText("setCompoundDrawables");
Drawable d = getResources().getDrawable(R.drawable.ic_launcher);
d.setBounds(0, 0, 50, 50); //必须设置图片大小，否则不显示
mTVText.setCompoundDrawables(d , null, null, null); ```
比如实例如下：
```
case 1:
            String html = "<img src='" + R.drawable.map_mjsynh4 + "'/>";
            ImageGetter imgGetter = new ImageGetter() {
                @Override
                public Drawable getDrawable(String source) {
                    int id = Integer.parseInt(source);
                    Drawable d = getResources().getDrawable(id);
                    d.setBounds(0, 0, d.getIntrinsicWidth(),
                            d.getIntrinsicHeight());
                    return d;
                }
            };
            CharSequence charSequence = Html.fromHtml(html, imgGetter, null);
            textView.setText(charSequence);
            break;
        case 2:
            Drawable d = getResources().getDrawable(R.drawable.map_mjsynh);
            d.setBounds(0, 0, 500, 500); // 必须设置图片大小，否则不显示
            textView.setCompoundDrawables(d, null, null, null);
            // textView.setText("setCompoundDrawables");
            break;
        default:
            textView.setTextSize(20);
            textView.setText("\"大眼睛\"是这样造就的：一位感动了整个中国的摄影师——解海龙。\n访问\"解海龙与希望工程网站\" http://news.qq.com/photon/zaixianyingzhan/xiehailong.htm");             break; 
case2中的方式可以使图片放大或缩小而不失真，显然更好。 

``` 