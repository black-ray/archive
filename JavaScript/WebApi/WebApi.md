

















## 













# 事件



- ##### 事件函数内的`this`指向事件函数的调用者















































# 网页特效



## 常见网页特效案例

### 倒计时

- ##### 功能需求

  <img src="WebApi.assets/image-20211206225213878.png" alt="image-20211206225213878" style="zoom: 50%;" /> 

- ##### 实现方案

  ```html
   <div>
  	<span class="hour">1</span>
  	<span class="minute">2</span>
  	<span class="second">3</span>
   </div>
  ```

  ```javascript
  // 1. 获取元素 
  var hour = document.querySelector('.hour'); // 小时的黑色盒子
  var minute = document.querySelector('.minute'); // 分钟的黑色盒子
  var second = document.querySelector('.second'); // 秒数的黑色盒子
  var inputTime = +new Date('2019-5-1 18:00:00'); // 返回的是用户输入时间总的毫秒数
  countDown(); // 第一次执行也是间隔毫秒数，我们先调用一次这个函数，防止第一次刷新页面有空白 
  // 2. 开启定时器
  setInterval(countDown, 1000);
  function countDown() {
      var nowTime = +new Date(); // 返回的是当前时间总的毫秒数
      var times = (inputTime - nowTime) / 1000; // times是剩余时间总的秒数 
      var h = parseInt(times / 60 / 60 % 24); //时
      h = h < 10 ? '0' + h : h;
      hour.innerHTML = h; // 把剩余的小时给 小时黑色盒子
      var m = parseInt(times / 60 % 60); // 分
      m = m < 10 ? '0' + m : m;
      minute.innerHTML = m;
      var s = parseInt(times % 60); // 当前的秒
      s = s < 10 ? '0' + s : s;
      second.innerHTML = s;
  }
  ```



### 返回顶部

- ##### 功能需求

  1. 原先侧边栏是绝对定位
  2. 当页面滚动到一定位置，侧边栏改为固定定位
  3. 页面继续滚动，会让 返回顶部显示出来

- ##### 实现方案

  ```html
  <div class="slider-bar">
  	<span class="goBack">返回顶部</span>
  </div>
  <div class="header w">头部区域</div>
  <div class="banner w">banner区域</div>
  <div class="main w">主体部分</div>
  ```

  ```javascript
  // 获取元素
  var sliderbar = document.querySelector('.slider-bar');
  var banner = document.querySelector('.banner');
  var main = document.querySelector('.main');
  var goBack = document.querySelector('.goBack');
  
  // 侧边栏发生变化时的被卷去头部的大小 要写到滚动的外面
  var bannerTop = banner.offsetTop
  // 侧边栏固定定位之后，要变化成的定位数值
  var sliderbarTop = sliderbar.offsetTop - bannerTop;
  var mainTop = main.offsetTop;
  
  // 页面滚动事件 scroll
  document.addEventListener('scroll', function() {
      
      // 当页面滚动到banner盒子，此时侧边栏就要改为固定定位 
      if (window.pageYOffset >= bannerTop) {
          sliderbar.style.position = 'fixed';
          sliderbar.style.top = sliderbarTop + 'px';
      } else {
          sliderbar.style.position = 'absolute';
          sliderbar.style.top = '300px';
      }
      // 当页面滚动到main盒子，就显示 goback模块
      if (window.pageYOffset >= mainTop) {
          goBack.style.display = 'block';
      } else {
          goBack.style.display = 'none';
      }
  });
  
  // 点击返回顶部模块，就让窗口滚动的页面的最上方
  goBack.addEventListener('click', function() {
      animate(window, 0);
  });
  
  // 动画函数
  function animate(obj, target, callback) {
      // 先清除以前的定时器，只保留当前的一个定时器执行
      clearInterval(obj.timer);
      obj.timer = setInterval(function() {
          // 步长为一个慢慢变小的值
          // 步长公式：(目标值 - 现在的位置) / 10
          var step = (target - window.pageYOffset) / 10;
  		// 步长值改为整数
          // 如果是正值，则步长 往大了取整
          // 如果是负值，则步长 向小了取整
          step = step > 0 ? Math.ceil(step) : Math.floor(step);
          if (window.pageYOffset == target) {
  			// 停止动画
              clearInterval(obj.timer);
              // 回调函数写到定时器结束里面
              callback && callback();
          }
  		window.scroll(0, window.pageYOffset + step);
      }, 15);
  }
  ```

  

###  悬停效果

- ##### 功能需求

  1. 鼠标经过某个li， 悬停图片跟这到当前li位置

  2. 鼠标离开某个li， 悬停图片复原为原来的位置

  3. 鼠标点击了某个li， 悬停图片就会留在点击这的个li 的位置

     <img src="WebApi.assets/bh3rc-8ords.gif" alt="bh3rc-8ords" style="zoom:50%;" /> 

- ##### 实现方案

  ```javascript
  // 获取元素
  var cloud = document.querySelector('.cloud');
  var c_nav = document.querySelector('.c-nav');
  var lis = c_nav.querySelectorAll('li');
  
  // 给所有的li绑定事件
  // current做为筋斗云的起始位置
  var current = 0;
  for (var i = 0; i < lis.length; i++) {
      // 鼠标经过把当前li 的位置做为目标值
      lis[i].addEventListener('mouseenter', function() {
          animate(cloud, this.offsetLeft);
      });
      
      // 鼠标离开就回到起始的位置
      lis[i].addEventListener('mouseleave', function() {
  		animate(cloud, current);
  	});
      
      // 当我们鼠标点击，就把当前位置做为目标值
      lis[i].addEventListener('click', function() {
  		current = this.offsetLeft;
  	});
  }
  
  function animate(obj, target, callback) {
      clearInterval(obj.timer);
      obj.timer = setInterval(function() {
          var step = (target - obj.offsetLeft) / 10;
          step = step > 0 ? Math.ceil(step) : Math.floor(step);
          if (obj.offsetLeft == target) {
              clearInterval(obj.timer);
              callback && callback();
          }
          obj.style.left = obj.offsetLeft + step + 'px';
      }, 15);
  }
  ```





### 放大镜效果

- ##### 功能需求

  <img src="WebApi.assets/dm06q-at0rj.gif" alt="dm06q-at0rj" style="zoom: 33%;" /> 

- ##### 实现方案

  ```javascript
  // 小图片盒子
  var preview_img = document.querySelector('.preview_img');
  // 黄色遮挡层
  var mask = document.querySelector('.mask');
  // 大图片盒子
  var big = document.querySelector('.big');
  // 大图片
  var bigIMg = document.querySelector('.bigImg');
  
  // 鼠标经过小图片盒子， 黄色的遮挡层 和 大图片盒子显示
  preview_img.addEventListener('mouseover', function() {
  	mask.style.display = 'block';
  	big.style.display = 'block';
  });
  // 离开隐藏2个盒子功能
  preview_img.addEventListener('mouseout', function() {
  	mask.style.display = 'none';
  	big.style.display = 'none';
  });
  
  // 黄色的遮挡层跟随鼠标功能。
  preview_img.addEventListener('mousemove', function(e){
      
      // 先计算出鼠标在盒子内的坐标
  	var x = e.pageX - this.offsetLeft;
  	var y = e.pageY - this.offsetTop;
      
      // 减去盒子高度的一半 就是我们mask 的最终 left 和top值了
      var maskX = x - mask.offsetWidth / 2;
      var maskY = y - mask.offsetHeight / 2;
      
      // 遮挡层的最大移动距离 小图片盒子宽度 减去 遮挡层盒子宽度
      var maskMax = preview_img.offsetWidth - mask.offsetWidth;
      
      // 遮挡层不能超出小图片盒子范围
      // 如果x 坐标小于了0 就让他停在0 的位置
      // 如果大于遮挡层最大的移动距离，就把坐标设置为最大的移动距离
  	if (maskX <= 0) {
  		maskX = 0;
  	} else if (maskX >= maskMax) {
  		maskX = maskMax;
  	}
  	if (maskY <= 0) {
  		maskY = 0;
  	} else if (maskY >= maskMax) {
  		maskY = maskMax;
  	}
      
      // 遮挡层移动
      mask.style.left = maskX + 'px';
      mask.style.top = maskY + 'px';
      
      // 大图片最大移动距离
      var bigMax = bigIMg.offsetWidth - big.offsetWidth;
      
  	// 大图片的移动距离 = 遮挡层移动距离 * 大图片最大移动距离 / 遮挡层的最大移动距离
      var bigX = maskX * bigMax / maskMax;
      var bigY = maskY * bigMax / maskMax;
      
      // 大图片跟随移动
      bigIMg.style.left = -bigX + 'px';
      bigIMg.style.top = -bigY + 'px';
  });
  ```



### 移动端拖动元素

- ##### 实现方案

  ```html
  <style>
  	div {
  	    position: absolute;
  	    left: 0;
  	    width: 100px;
  	    height: 100px;
  	    background-color: pink;
  	}
  </style>
  
  <body>
      <div></div>
  </body>
  ```

  ```javascript
  var div = document.querySelector('div');
  
  //手指初始坐标
  var startX = 0;
  var startY = 0;
  
  //盒子原来的位置
  var x = 0;
  var y = 0;
  
  // 触摸元素 touchstart：  获取手指初始坐标，同时获得盒子原来的位置
  div.addEventListener('touchstart', function(e) {
  	//  获取手指初始坐标
      startX = e.targetTouches[0].pageX;
      startY = e.targetTouches[0].pageY;
      x = this.offsetLeft;
      y = this.offsetTop;
  });
  
  // 移动手指 touchmove：  计算手指的滑动距离，并且移动盒子
  div.addEventListener('touchmove', function(e) {
      //  计算手指的移动距离： 手指移动之后的坐标减去手指初始的坐标
      var moveX = e.targetTouches[0].pageX - startX;
      var moveY = e.targetTouches[0].pageY - startY;
      
      // 移动盒子 盒子原来的位置 + 手指移动的距离
      this.style.left = x + moveX + 'px';
      this.style.top = y + moveY + 'px';
      // 阻止屏幕滚动的默认行为
      e.preventDefault();
  });
  ```

  

### 移动端轮播图

- ##### 功能需求

  1. 自动播放图片

  2. 手指可以拖动播放轮播图

     <img src="WebApi.assets/t0mt9-p41wi.gif" alt="t0mt9-p41wi" style="zoom: 80%;" /> 

- ##### 实现方案

  ```css
  .focus {
      position: relative;
      padding-top: 44px;
      overflow: hidden;
  }
  
  .focus img {
      width: 100%;
  }
  
  .focus ul {
      overflow: hidden;
      width: 500%;
      margin-left: -100%;
  }
  
  .focus ul li {
      float: left;
      width: 20%;
  }
  
  .focus ol {
      position: absolute;
      bottom: 5px;
      right: 5px;
      margin: 0;
  }
  
  .focus ol li {
      display: inline-block;
      width: 5px;
      height: 5px;
      background-color: #fff;
      list-style: none;
      border-radius: 2px;
      transition: all .3s;
  }
  
  .focus ol li.current {
      width: 15px;
  }
  ```

  ```html
  <!-- 焦点图模块 -->
  <div class="focus">
      <ul>
          <li><img src="upload/focus3.jpg" alt=""></li>
          <li><img src="upload/focus1.jpg" alt=""></li>
          <li><img src="upload/focus2.jpg" alt=""></li>
          <li><img src="upload/focus3.jpg" alt=""></li>
          <li><img src="upload/focus1.jpg" alt=""></li>
      </ul>
      <!-- 小圆点 -->
      <ol>
          <li class="current"></li>
          <li></li>
          <li></li>
      </ol>
  </div>
  ```

  ```javascript
  // 获取元素
  var focus = document.querySelector('.focus');
  var ul = focus.children[0];
  var ol = focus.children[1];
  
  // 获得focus 的宽度
  var w = focus.offsetWidth;
  
  // 利用定时器自动轮播图图片
  var index = 0;
  var timer = setInterval(function() {
      index++;
      // 索引号乘以宽度 去滚动图片
      var translatex = -index * w;
      // 过渡效果
      ul.style.transition = 'all .3s';
      // 移动端移动，可以使用translate 移动
      ul.style.transform = 'translateX(' + translatex + 'px)';
  }, 2000);
  
  // 监听过渡完成的事件 transitionend
  // 判断条件是要等到图片滚动完毕再去判断，就是过渡完成后判断
  ul.addEventListener('transitionend', function() {
      
      if (index >= 3) {
          // 如果索引号等于 3 说明走到最后一张图片，此时 索引号要复原为 0
          index = 0;
          // 去掉过渡效果 让ul快速的跳到目标位置
          ul.style.transition = 'none';
          // 利用最新的索引号乘以宽度 去滚动图片
          var translatex = -index * w;
          ul.style.transform = 'translateX(' + translatex + 'px)';
          
      } else if (index < 0) {
          // 如果索引号小于0，说明是倒着走，索引号等于2
          index = 2;
          ul.style.transition = 'none';
          var translatex = -index * w;
          ul.style.transform = 'translateX(' + translatex + 'px)';
      }
      // 小圆点跟随变化
      // 把ol里面li带有current类名的选出来去掉类名
      ol.querySelector('.current').classList.remove('current');
      // 让当前索引号的li 加上 current
      ol.children[index].classList.add('current');
  });
  
  // 触摸元素 touchstart： 获取手指初始坐标
  var startX = 0;
  var moveX = 0;
  // 手指是否移动
  var flag = false;
  
  // 触摸元素 touchstart： 获取手指初始坐标
  ul.addEventListener('touchstart', function(e) {
  	startX = e.targetTouches[0].pageX;
      // 手指触摸的时候就停止定时器
      clearInterval(timer);
  });
  
  // 移动手指 touchmove： 计算手指的滑动距离， 并且移动盒子
  ul.addEventListener('touchmove', function(e) {
  	// 计算移动距离
      moveX = e.targetTouches[0].pageX - startX;
      // 移动盒子：  盒子原来的位置 + 手指移动的距离 
      var translatex = -index * w + moveX;
      // 手指拖动的时候，不需要动画效果所以要取消过渡效果
      ul.style.transition = 'none';
      ul.style.transform = 'translateX(' + translatex + 'px)';
      // 如果用户手指移动过再去判断，否则不做判断效果
      flag = true;
      // 阻止滚动屏幕的行为
      e.preventDefault();
  });
  
  // 离开手指 touchend: 根据滑动的距离分不同的情况
  ul.addEventListener('touchend', function(e) {
      if (flag) {
          // 如果移动距离大于50像素我们就播放上一张或者下一张
          if (Math.abs(moveX) > 50) {
              // 如果是右滑就是 播放上一张 moveX 是正值
              if (moveX > 0) {
              	index--;
              } else {
  				// 如果是左滑就是 播放下一张 moveX 是负值
  				index++;
  			}
              var translatex = -index * w;
              ul.style.transition = 'all .3s';
              ul.style.transform = 'translateX(' + translatex + 'px)';
          } else {
              // 如果移动距离小于50像素我们就回弹
              var translatex = -index * w;
              ul.style.transition = 'all .1s';
              ul.style.transform = 'translateX(' + translatex + 'px)';
          }
      }
      
      // 手指离开的时候就重新开启定时器
      clearInterval(timer);
  	timer = setInterval(function() {
  	    index++;
  	    var translatex = -index * w;
  	    ul.style.transition = 'all .3s';
  	    ul.style.transform = 'translateX(' + translatex + 'px)';
  	}, 2000);
  });
  ```

  

