### 给自己做个炫一点的Button吧
#### 平时自己的button，总是长下面这个样子：
```css
.old-btn {
  width: 80px;
  height: 40px;
  text-align: center;
  line-height: 40px;
  border: 1px solid #ccc;
  border-radius: 4px;
  background: #b2f368; /* Old browsers */
  background: -moz-linear-gradient(top,  #b2f368 0%, #83d129 100%); /* FF3.6-15 */
  background: -webkit-linear-gradient(top,  #b2f368 0%,#83d129 100%); /* Chrome10-25,Safari5.1-6 */
  background: linear-gradient(to bottom,  #b2f368 0%,#83d129 100%); /* W3C, IE10+, FF16+, Chrome26+, Opera12+, Safari7+ */
  filter: progid:DXImageTransform.Microsoft.gradient( startColorstr='#b2f368', endColorstr='#83d129',GradientType=0 ); /* IE6-9 */

}
```

现在，利用box-shadow属性，可以做出很多花哨的button,比如：
```css
.btn{
   background-image:linear-gradient(0deg, #f7f7f7 0%, #e5e5e5 100%);
  box-shadow:0px 0px 1px 0px #ffffff, inset 0px 1px 3px 0px #4ea8ea, inset 0px -1px 1px 0px rgba(73,73,73,0.30);
  border-radius:100%;
  text-align: center;
  width:80px;
  height:80px;
  line-height: 80px;
}
```

预览效果请猛戳[这里](https://jsfiddle.net/hckjzb3e/1/)

---
button上的文字如果能换成icon,那就更好看了。