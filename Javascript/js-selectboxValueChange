# SelectBox Value에 따라 Html 값 바꾸기
- 셀렉트 박스의 벨류 값의 따라 Html 변경하는 스크립트


```javascript
$(document).ready(function($) {
 
  $('#show_selected').change(function() {

    var pdf = document.getElementById("pdf") // 바꿀 값
    var url = document.getElementById("url") // 바꿀 값
    var target = document.getElementById("show_selected"); // 셀렉트 박스 
    var optionValue = target.options[target.selectedIndex].value; // 벨류 값
    // alert('선택된 옵션 value 값=' + target.options[target.selectedIndex].value);     // 벨류 값 알럿창

    if (optionValue === '1') {
      pdf.style.display = "revert";
      url.style.display = "none";
    } else if (optionValue === '2') {
      pdf.style.display = "none";
      url.style.display = "revert";
    } else {
      pdf.style.display = "none";
      url.style.display = "none";
    }

  });
});
```


```Html
<table class="table table-bordered">
  <tbody>
    <tr>
      <th width="200">제목</th>
      <td class="table_left">
        <input type='text' class="form-control">
      </td>
    </tr>
    <tr>
      <th>종류</th>
      <td class="table_left">
        <select class="form-control"  style="width:100px" id="show_selected" name="show_selected">
          <option value="0" selected>선택해주세요.</option>
          <option value="1">PDF</option>
          <option value="2">URL</option>
        </select>
        </td>
    </tr>
    <tr id="pdf">
      <th>PDF 첨부</th>
        <td class="table_left"><input type="file" class="form-control" style="width:500px" ></td>
      </th>
    </tr>
    <tr id="url">
      <th>
        URL
      </th>
      <td>
        <input type='text' class="form-control">
      </td>
    </tr>
  </tbody>
</table>
```
