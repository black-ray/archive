

 

# 边框

- ##### 边框`border : border-width || border-style || border-color;`

  - 边框 `border-style`：实线`solid`，虚线`dashed`, 点线`dotted`

  - 复合写法： `border: 1px solid red;` 没有顺序

  - 边框分开写法 例如只设置上边框 `border-top: 1px solid red;`

  - 边框会影响盒子的大小

  - 三角形

    ```css
    div {
        width: 0;
        height: 0;
        line-height: 0;
        font-size: 0;
        border: 50px solid transparent;
        border-left-color: pink;
    }
    ```

  - 直角三角形

    ```css
    div {
    	width: 0;
        height: 0; 
        /* 只保留右边的边框有颜色 */
        border-color: transparent red transparent transparent;
        border-style: solid;
        /* 上边框宽度要大，右边框宽度稍小，其余边框为0 */
        border-width: 22px 8px 0 0;
    }
    ```

- ##### 边框合并`border-collapse:collapse;`

  - 相邻边框合并在一起

  

 

# 内边距，外边距



- ##### 外边距`margin`

  - 清除内外边距

    - 网页元素很多带有默认的内外边距
    - `* { padding: 0; margin: 0; }`
    - 行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距，但是转换为块级和行内块元素就可以了



