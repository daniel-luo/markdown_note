##jQuery 使用 On 绑定 hover 事件##
注意：在 jQuery 里 `hover` 的效果和 `mouseover`,`mouseout` 的组合是有区别的;
`hover` 中鼠标事件是 `mouseenter` 和 `mouseleave`,使用 `on` 绑定的时候应该通过 `event.type` 判断 

**eg:**

```html
<div class=".demo">
	<span class="target_class"></span>
</div>
```
**注意:** 这里的 `.demo` 应该是一直存在的，`.target_class` 可以是 `AJAX` 加载的

```javascript
$('.demo').on('hover', 'target_class', function(event){
	if (event.type == 'mouseenter') {
    	// input your mouseover logic
    } else {
    	// input your mouseout logic
    }
});
```