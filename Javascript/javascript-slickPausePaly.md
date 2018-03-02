# Slick slide Plugin pause and play script
- [링크참고](http://jsfiddle.net/wd3eapez/1/)

```javascript
$(".slider").slick({
    accessibility : true,
    autoplay : true,
    autoplaySpeed : 1000,
    pauseOnHover : false,
    fade : true,
    dots: true,
    appendArrows : $('.carousel-arrows-wrapper'),
    appendDots : $('.carousel-dots-wrapper')
});

$('.slick-pause').on('click', function(){
    var $pauseBtn = $(this);
    if ($pauseBtn.hasClass('paused')){
        $(".slider").slick('slickPlay');
        $pauseBtn.removeClass('paused');
    } else {
        $(".slider").slick('slickPause');
        $pauseBtn.addClass('paused');
    }
});
```