# number repeat
- 화면에 숫자가 무한 반복됨

```javascript
var list = '';
var i;
for (i = 0; i < 28; i++) {
    list += '<div class="changeText" id="changeText' + i + '">0</div>';

}
document.getElementById("timeLoop").innerHTML = list;

for (i = 0; i < 13; i++) {
    list += '<div class="changeText" id="changeText' + i + '">0</div>';
}
document.getElementById("timeLoop2").innerHTML = list;

var textArray = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "0", ""];
var index = 0;

function repeat(idx) {
    var changeText = $('#timeLoop #changeText' + idx);
    var changeText2 = $('#timeLoop2 #changeText' + idx);
    setInterval(function () {
        $(changeText).add(changeText2).animate({
            opacity: .5

        }, function () {
            if (textArray.length > index) {
                var rand = Math.floor(Math.random() * textArray.length);
                $(this).text(textArray[rand]).animate({opacity: .5})
                index++;
                repeat(idx + 1)
                repeat(idx + 2)
            }
            else
                index = 0;
        });
    }, 0);
}
repeat(0);
```