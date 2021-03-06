#![Logo](https://github.com/xiaopansky/ViewPlayer/raw/master/res/drawable-mdpi/ic_launcher.png) ViewPlayer

这是Android上一个专门用于轮播View的组件，最低兼容Android2.2

![sample](https://github.com/xiaopansky/ViewPlayer/raw/master/docs/sample.png)

##Features
>* ViewPlayer继承于ViewPager，所以你可以自定义切换效果
>* 支持转圈循环播放和摇摆播放两种方式

## Sample Application
**[Get it on Google Play](https://play.google.com/store/apps/details?id=me.xiaopan.android.viewplayer)**

**[Download it on Github](https://github.com/xiaopansky/ViewPlayer/raw/master/releases/ViewPlayer_1.1.5.1.apk)**

##Usage Guide
###1.创建你的PagerAdapter适配器
实现方式有两种
>* 1.继承ViewPlayPagerAdapter或者ViewPlayFragmentStatePagerAdapter
>* 2.继承PagerAdapter并实现ViewPlayAdapterInterface接口（你必须实现此接口，否则将无法使用）

示例如下：
```java
/**
 * 图片播放适配器
 */
public class PicturePlayFragmentStatePagerAdapter extends ViewPlayFragmentStatePagerAdapter {
	private List<String> pictures;//图片列表

	public PicturePlayFragmentStatePagerAdapter(FragmentManager fm, List<String> pictureUrls) {
		super(fm);
		this.pictures = pictureUrls;
	}

	@Override
	public int getRealCount() {
		return pictures != null?pictures.size():0;
	}

	@Override
	public Fragment getRealItem(int position) {
		PictureFragment pictureFragment = new PictureFragment();
		Bundle bundle = new Bundle();
		bundle.putString(PictureFragment.PARAM_REQUIRED_STRING_PICTURE_URL, pictures.get(position));
		pictureFragment.setArguments(bundle);
		return pictureFragment;
	}
	
	public static class PictureFragment extends Fragment{
		public static final String PARAM_REQUIRED_STRING_PICTURE_URL = "PARAM_REQUIRED_STRING_PICTURE_URL";
		
		@Override
		public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
			ImageView imageView = new ImageView(getActivity().getBaseContext());
			imageView.setLayoutParams(new ViewGroup.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT));
			imageView.setScaleType(ScaleType.CENTER_CROP);
			ImageLoader.getInstance(getActivity()).display(getArguments().getString(PARAM_REQUIRED_STRING_PICTURE_URL), imageView, OptionType.VIEW_PLAYER);
			return imageView;
		}
	}
}
```

###2.创建ViewPlayer并设置适配器
```java
ArrayList<String> pictureUrls = new ArrayList<String>();
for(String url : getResources().getStringArray(R.array.autoPlayGallery_urls2)){
	pictureUrls.add(url);
}
ViewPlayer viewPlayer = new ViewPlayer(getContext());
viewPlayer.setAdapter(new PicturePlayFragmentStatePagerAdapter(getSupportFragmentManager(), pictureUrls));
viewPlayer.setPlayMode(ViewPlayMode.SWING);
```

###3.启动或停止播放
你需要在onResume()和onPause()的时候去启动和停止播放，示例如下：
```java
@Override
public void onResume() {
	viewPlayer.start();
	super.onPause();
}

@Override
public void onPause() {
	viewPlayer.stop();
	super.onPause();
}
```

###4.扩展功能
>* 调用start()方法启动播放
>* 调用stop()方法停止播放
>* 调用reset()方法充值播放位置
>* 调用setPlayMode()方法设置播放模式（有转圈和摇摆两种可选）
>* 调用isPlaying()判断是否正在播放
>* 调用getRealCount()方法获取真实的Item个数
>* 调用getRealCurrentItem()方法获取真实的位置
>* 调用setAnimationDuration()方法设置切换动画持续时间
>* 调用setSwitchSpace()方法设置切换间隔时间
>* 调用setOnSetAdapterListener()方法在监听适配器设置，并在之后进行一些初始化工作

##Downloads
**[android-view-player-1.1.7.jar](https://github.com/xiaopansky/ViewPlayer/raw/master/releases/android-view-player-1.1.7.jar)**

**[android-view-player-1.1.7-with-src.jar](https://github.com/xiaopansky/ViewPlayer/raw/master/releases/android-view-player-1.1.7-with-src.jar)**

##Change Log
###1.1.7
>* 修复当getCount()为0时调用getRealPosition()方法发生异常的BUG

###1.1.6
>* 优化自带ViewPlayIndicator的效果，默认居中，默认也不再设置内边距和外边距

###1.1.5
>* 优化getRealPosition()方法名称为getRealCurrentItem()
>* 加大无线循环时总数的大小由，之前的50倍改为1000倍

###1.1.4
>* 更新版权信息

###1.1.3
>* 修复当没有启动播放时，触摸界面后自动开启播放的BUG

##License
```java
/*
 * Copyright (C) 2013 Peng fei Pan <sky@xiaopan.me>
 * 
 * Licensed under the Apache License, Version 2.0 (the "License");
 * you may not use this file except in compliance with the License.
 * You may obtain a copy of the License at
 * 
 *   http://www.apache.org/licenses/LICENSE-2.0
 * 
 * Unless required by applicable law or agreed to in writing, software
 * distributed under the License is distributed on an "AS IS" BASIS,
 * WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
 * See the License for the specific language governing permissions and
 * limitations under the License.
 */
```
