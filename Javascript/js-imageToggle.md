# Images Toggle

```javascript
$(".arrow_down").click(function() {
   var _this = $(this);
   var current = _this.attr("src");
   var swap = _this.attr("data-swap");
 _this.attr('src', swap).attr("data-swap",current);
});
```

```html
<img src="/images/common/arrow_down.png" alt="arrow_down" style="width:20px;" class="arrow_down" data-swap='/images/common/arrow_top.png' data-src='/images/common/arrow_down.png' />
```


### IE에도 가능한 이미지 토글

```javascript
var play2 = '/images/event_play.png';
    var pause2 = '/images/event_pause.png';
    
    $('#event_btn_pause').click(function() {
      if ($('.event_pause').attr('src') === play2) {
        $('.event_pause').attr('src', pause2);
      } else {
        $('.event_pause').attr('src', play2)
      }
});
```

```html
<button type="button" id="event_btn_pause"><img src="/images/event_pause.png" class="event_pause" data-swap='/images/event_play.png' data-src='/images/event_pause.png' /></button>
```

