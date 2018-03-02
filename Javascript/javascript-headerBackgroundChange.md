# Header Background Color Change
- 모바일이나 웹 header가 스크롤에 따라 background값이 바뀌게 할때 쓰는 스크립트!
background로 이미지 url를 삽입해야 변경이 가능하다.
- [링크참고](http://stackoverflow.com/questions/28266651/change-header-background-colour-when-page-scrolls)


```javascript
$(window).on("scroll", function() {
    if($(window).scrollTop() > 50) {
        $("header").addClass("active");
        $("header > h1 > a.home_title").addClass("active");
        $(".nav-toggle").addClass("active");
        $(".nav-search").addClass("active");
    } else {
        //remove the background property so it comes transparent again (defined in your css)
       $("header").removeClass("active");
       $("header > h1 > a.home_title").removeClass("active");
       $(".nav-toggle").removeClass("active");
       $(".nav-search").removeClass("active");
    }
});
```


```css
.nav-toggle{
    display: block;
    width: 20px;
    height: 15px;
    margin: 17px 0 0 10px;
    color: #e64a33;
    background: url(/images/common/head_btn_side_white_2.png) no-repeat center center;
    background-size: cover;
}
.nav-toggle.active{
  display: block;
  width: 20px;
  height: 15px;
  margin: 17px 0 0 10px;
  color: #e64a33;
  background: url(/images/common/head_btn_side_white_2_bk.png) no-repeat center center;
  background-size: cover;
}
```