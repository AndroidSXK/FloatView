# ������Ŀ

## ��������
1. ���������ƶ�����
2. �������չ������
3. 3s���޲����������Զ��ڲ������ࣨ���ڸ���ֻ����һ��ͼƬ�����������ڲ�ʱֻ�ǿ����ˣ�

## Ч��ͼ
![demo](demo.gif)

## ʵ��ԭ��

### ��������
1. ��ȡWindowManagerϵͳ����
2. ���ø�����ʾ�Ĳ���WindowManager.LayoutParams
3. ��ȡ���겼�ֲ���ӵ�WindowManager����ȥ
4. ����ʾ����
```java
mMansger = (WindowManager)context.getApplicationContext().getSystemService(Activity.WINDOW_SERVICE);
mParams = new WindowManager.LayoutParams();
screen_widht = mMansger.getDefaultDisplay().getWidth();
screen_height = mMansger.getDefaultDisplay().getHeight();

mParams.format = PixelFormat.RGBA_8888;                                                     //ͼƬ��ʽΪ͸��
mParams.type = WindowManager.LayoutParams.TYPE_PHONE;                                       //��������Ӧ�ö��ˣ�״̬��֮��
mParams.flags = WindowManager.LayoutParams.FLAG_NOT_FOCUSABLE;                              //����ȡ����
mParams.gravity = Gravity.LEFT | Gravity.TOP;                                               //���϶���
mParams.x = float_x;
mParams.y = float_y;                                                                        //����������ԭ��
mParams.width = LayoutParams.WRAP_CONTENT;
mParams.height = LayoutParams.WRAP_CONTENT;

mFloatLayout = (LinearLayout)LayoutInflater.from(context).inflate(R.layout.float_window, null);
mMansger.addView(mFloatLayout, mParams);
```


### �����ƶ�
���ø����ƶ�����OnTuchListener,����������
1. ��ȡ��ǰ����λ�ã�������λ�ø���������updateViewLayout
2. ����ʾ����	
```java
floatImage.setOnTouchListener(new View.OnTouchListener() {
	@Override
	public boolean onTouch(View v, MotionEvent event) {
		isMove = true;
		if(event.getAction() == MotionEvent.ACTION_DOWN){

			return false;
		}

		//��������
		if(mFloatLayout != null){
			float_x = (int)event.getRawX();
			float_y = (int)event.getRawY();
			int width = mFloatLayout.getWidth();
			int height = mFloatLayout.getHeight();
			if(float_x + width > screen_widht){
				float_x = screen_height - width;
			}

			if(float_y + height > screen_height){
				float_y = screen_height - height;
			}

			//����Ĭ���Ǵ�view�����Ͻǻ����������ƶ�������������view  ����Ҫ���Ϻ���λ��ǰ�ƾ���������
			mParams.x = (float_x - width/2) > 0 ? float_x - width/2 : 0;
			mParams.y = (float_y - height/2) > 0 ? float_y - height/2 : 0;
			mMansger.updateViewLayout(mFloatLayout, mParams);
		}

		/**
		 * ̧��Ĭ�ϻ�������
		 */
		if(event.getAction() == MotionEvent.ACTION_UP){
			isMove = false;
			task_restore.postDelayed(retoreFloatView, 3000);                                //3s���޲����������ع���
		}

		return false;
	}
});
```

### �������
1. �������setOnclickListener������ܼ򵥾Ͳ�����˵����

### �Զ��ڲ�
1. ԭ��
   ����Handler��ʱ����ִ�и����ڲع�������ԭ������ڻ���ʱ̧�����͵���¼���ִ���ڲأ�ˢ�µ����һ���жϸ����Ƿ��в������в����Ͳ��ڲ��ˣ���֮���ڲ�
2. ����ʾ����

```java
 private Runnable retoreFloatView = new Runnable() {
	@Override
	public void run() {

		account_left.setVisibility(View.GONE);
		account_right.setVisibility(View.GONE);
		if(float_x > screen_widht / 2){
			floatImage.setImageDrawable(mContext.getDrawable(R.drawable.xy_icon));                                    //�����ұ߸���icon
			float_x = screen_widht - mFloatLayout.getWidth();
		}else{
			floatImage.setImageDrawable(mContext.getDrawable(R.drawable.xy_icon));
			float_x = 0;
		}
		mParams.x = float_x;
		if(isMove){
			return;
		}
		mMansger.updateViewLayout(mFloatLayout, mParams);
	}
};
```

## ʹ�÷���
1. clone����������
2. import as moudle
3. ����FloatUtil�༴��ʹ��
```java
FloatUtil.getInstance().createFloatView(this);
FloatUtil.getInstance().removeFloatView();
```
 
## License
```
Copyright 2016 jackzhous

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```
