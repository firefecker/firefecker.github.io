---
layout：post-light-feature
title：Android 通过Coordinatorlayout实现滑动toolBar渐变
descritpion："在Android开发过程中或许会用到很多的滑动效果，其中Cooldinatorlayout结合AppBarlayout实现滑动渐变的协调布局非常常见，此次就是结合这个控件实现摩拜单车个人中心的效果"
category：articles
tags：[Android,Cooldinatorlayout,协调布局]
---

首先想要实现这种效果就必须得了解这个控件，Coordinatorlayout是Android Material Design风格中的一个重要控件，它基本实现两个功能
1、作为顶层布局
2、协调子布局

---
Coordinatorlayout的原理：
        CoordinatorLayout使用新的思路通过协调调度子布局的形式实现触摸影响布局的形式产生动画效果。CoordinatorLayout通过设置子View的 Behaviors来调度
---

### 效果图如下所示：

<figure>
	<img src="http://onvxkl171.bkt.clouddn.com/Screenshot_2017-04-04-19-43-54.png">
	<figcaption>图片1，展开的时候</figcaption>
</figure>

<figure>
	<img src="http://onvxkl171.bkt.clouddn.com/Screenshot_2017-04-04-19-44-14.png">
	<figcaption>图片2，滑动过程中的时候</figcaption>
</figure>

<figure>
	<img src="http://onvxkl171.bkt.clouddn.com/Screenshot_2017-04-04-19-44-04.png">
	<figcaption>图片3，收缩的时候</figcaption>
</figure>

### 相关代码如下：

---
布局：

{% highlight html linenos %}
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:background="@color/transparent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="false">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/main_abl_app_bar"
        android:layout_width="match_parent"
        android:layout_height="300dp">

        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:background="@color/main_color"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">

            <RelativeLayout
                android:id="@+id/main_fl_title"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:orientation="vertical">

                <ImageView
                    android:layout_width="match_parent"
                    android:layout_height="180dp"
                    android:layout_alignParentBottom="true"
                    android:layout_gravity="bottom"
                    android:background="@color/main_color" />

                <de.hdodenhof.circleimageview.CircleImageView
                    android:id="@+id/head"
                    android:layout_width="80dp"
                    android:layout_height="80dp"
                    android:layout_centerHorizontal="true"
                    android:layout_marginTop="80dp"
                    android:src="@drawable/com_default_pic"
                    app:border_color="@android:color/white"
                    app:border_width="2dp" />

                <TextView
                    android:id="@+id/mobile"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_below="@id/head"
                    android:layout_centerHorizontal="true"
                    android:layout_marginTop="16dp"
                    android:text="昵称"
                    android:textColor="@color/black" />

                <TextView
                    android:layout_width="wrap_content"
                    android:layout_height="30dp"
                    android:layout_below="@id/mobile"
                    android:layout_centerHorizontal="true"
                    android:layout_marginTop="16dp"
                    android:background="@drawable/shape_credit_score"
                    android:gravity="center_vertical"
                    android:paddingLeft="32dp"
                    android:paddingRight="32dp"
                    android:text="介绍↓自己"
                    android:textColor="@color/white" />
            </RelativeLayout>

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:background="@color/main_color"
                app:contentInsetStart="0dp"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/OverflowMenuStyle"
                app:theme="@style/ThemeOverlay.AppCompat.Dark">

                <RelativeLayout
                    android:layout_width="match_parent"
                    android:layout_height="match_parent"
                    android:gravity="center_vertical"
                    android:orientation="horizontal">

                    <ImageView
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:paddingLeft="@dimen/dp_14"
                        android:visibility="gone"
                        android:layout_centerVertical="true"
                        android:src="@drawable/ic_arrow_back_white" />

                    <TextView
                        android:id="@+id/main_tv_toolbar_title"
                        android:layout_width="wrap_content"
                        android:layout_height="wrap_content"
                        android:layout_centerInParent="true"
                        android:textColor="@android:color/white"
                        android:textSize="20sp" />

                </RelativeLayout>

            </android.support.v7.widget.Toolbar>
        </android.support.design.widget.CollapsingToolbarLayout>


    </android.support.design.widget.AppBarLayout>


    <android.support.v4.widget.NestedScrollView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scrollbars="none"
        android:background="@color/transparent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">

        <LinearLayout
            android:background="@color/transparent"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:paddingBottom="@dimen/dp_60"
            android:orientation="vertical">

            <LinearLayout
                android:id="@+id/layout_cache"
                style="@style/mineStyle">

                <TextView
                   android:text="@string/clearcache"
                    style="@style/textStyle" />

                <TextView
                    android:id="@+id/tv_cache"
                    android:layout_width="wrap_content"
                    android:layout_height="wrap_content"
                    android:layout_marginRight="8dp"
                    android:textColor="@color/text_color2" />

            </LinearLayout>

            <View style="@style/lineStyle"/>

            <LinearLayout
                android:id="@+id/layout_comment"
                style="@style/mineStyle">

                <TextView
                    android:text="@string/comment_score"
                    style="@style/textStyle" />

                <ImageView style="@style/imageindictorStyle"/>
            </LinearLayout>

            <View style="@style/lineStyle"/>

            <LinearLayout
                android:id="@+id/layout_share"
                style="@style/mineStyle">

                <TextView style="@style/textStyle"
                    android:text="@string/share_app"/>

                <ImageView style="@style/imageindictorStyle"/>
            </LinearLayout>

            <View
                android:background="@color/gray1"
                android:layout_width="match_parent"
                android:layout_height="@dimen/dp_36"/>

            <LinearLayout
                android:id="@+id/layout_feedback"
                style="@style/mineStyle">

                <TextView style="@style/textStyle"
                    android:text="@string/feedback"/>

                <ImageView style="@style/imageindictorStyle"/>
            </LinearLayout>

            <View style="@style/lineStyle"/>

            <LinearLayout
                android:id="@+id/layout_post"
                style="@style/mineStyle">

                <TextView style="@style/textStyle"
                    android:text="@string/callme"/>

                <ImageView style="@style/imageindictorStyle"/>
            </LinearLayout>

            <View style="@style/lineStyle"/>

            <LinearLayout
                android:id="@+id/layout_update"
                style="@style/mineStyle">

                <TextView style="@style/textStyle"
                    android:text="@string/updateapp"/>

                <ImageView style="@style/imageindictorStyle"/>
            </LinearLayout>

            <View style="@style/lineStyle"/>

        </LinearLayout>
    </android.support.v4.widget.NestedScrollView>
</android.support.design.widget.CoordinatorLayout>


{% endhighlight %}


---
###  注意：
1.在AppBarLayout中，添加CollapsingToolbarLayout控件，在ColllapsingToolbarLayout容器里面一部分可以滑动消失，一部分可以固定位置。
---
2.AppBarLayout的子布局有5种滚动标识(就是上面代码CollapsingToolbarLayout中配置的app:layout_scrollFlags属性)：
---
(1).scroll:将此布局和滚动时间关联。这个标识要设置在其他标识之前，没有这个标识则布局不会滚动且其他标识设置无效。

(2).enterAlways:任何向下滚动操作都会使此布局可见。这个标识通常被称为“快速返回”模式。

(3).enterAlwaysCollapsed：假设你定义了一个最小高度（minHeight）同时enterAlways也定义了，那么view将在到达这个最小高度的时候开始显示，并且从这个时候开始慢慢展开，当滚动到顶部的时候展开完。

(4).exitUntilCollapsed：当你定义了一个minHeight，此布局将在滚动到达这个最小高度的时候折叠。
snap:当一个滚动事件结束，如果视图是部分可见的，那么它将被滚动到收缩或展开。例如，如果视图只有底部25%显示，它将折叠。相反，如果它的底部75%可见，那么它将完全展开

{% highlight html linenos %}
app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
{% endhighlight %}

3.相对于CollapsingToolbarLayout这个容器，CollapsingToolbarLayout的子布局有3种折叠模式（Toolbar中设置的app:layout_collapseMode）
--
(1).off：这个是默认属性，布局将正常显示，没有折叠的行为。

(2).pin：CollapsingToolbarLayout折叠后，此布局将固定在顶部。toolbar就是添加了这个属性来固定在顶部

(3).parallax：CollapsingToolbarLayout折叠时，此布局也会有视差折叠效果.

4.当CollapsingToolbarLayout的子布局设置了parallax模式时，我们还可以通过app:layout_collapseParallaxMultiplier设置视差滚动因子，值为：0~1
---


最后在AppBarLayout同一层级添加一个可滑动的控件或者容器，再添加app:behavior属性来响应滑动

如代码中的NestedScrollView中
{% highlight html linenos %} 
app:layout_behavior="@string/appbar_scrolling_view_behavior"
{% endhighlight %}

就是添加了CooldinatorLayout的默认的滑动的behavior来响应滑动


### 最后在代码中实现部分：

大部分的实现实在布局里面，难点都实现了离成功还远吗

{% highlight html linenos %} 
mAblBar.addOnOffsetChangedListener(new AppBarLayout.OnOffsetChangedListener() {
            @Override
            public void onOffsetChanged(AppBarLayout appBarLayout, int verticalOffset) {
                int halfScroll = appBarLayout.getTotalScrollRange() / 2;
                int offSetAbs = Math.abs(verticalOffset);
                float percentage;
                if (offSetAbs < halfScroll) {
                    title.setText(getString(R.string.app_name));
                    percentage = 1 - (float) offSetAbs / (float) halfScroll;
                } else {
                    title.setText(getString(R.string.userinfo_center));
                    percentage = (float) (offSetAbs - halfScroll) / (float) halfScroll;
                }
                toolbar.setAlpha(percentage);
            }
        });
{% endhighlight %}


---
直接给AppBarLayout添加滑动监听来设置固定的ToolBar的alpha值并且修改title

