# Hover Effect
- hover할때만 버튼이 있고. hover 벗어날때에는 버튼 없이짐

```javascript
$(document).ready(function(){
  $('.reveal-btn-01').hide();

  $('.hvr-reveal').hover(function() {
    jQuery(this).find('.reveal-btn-01').show()
       }, function() {
           jQuery(this).find('.reveal-btn-01').hide();
       });
   });
```

```html
<li class="hvr-reveal"><img src="images/profile_img.png" alt="profile_img" class="middle mr10 radius_50">Bigs<a href="" class="reveal-btn-01"></a></li>
```