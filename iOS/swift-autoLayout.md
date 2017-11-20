# AutoLayout
### Constraints:: UI요소들의 위치에 대한 규칙을 정의
* Leading:: 요소 왼쪽
* Trailing:: 요소 오른쪽
* Top:: 요소 상단
* Bottom:: 요소 하단
* Horizontal:: 수평
* Vertical:: 수직
* Width:: 요소 너비
* Height:: 요소 높
* CenterX:: 요소 X방향 중심위치 
* CenterY:: 요소 Y방향 중심위치
* Aspect Ratio:: 요소 XY 비율

### Pin button으로 고정
- Constraints를 상세히 고정시킬 수 있는 기능

### Size Inspector
- 이미 고정시켜 놓은 요소의 수치를 맞추고 싶을 때 사용

# 스크롤뷰 (Scroll View)
1. Main Storyboard에 스크롤뷰 추가
2. 스크롤뷰 Constraints 정의
    * Constraints:: Leading / Trailing / Top / Bottom 
3. View 추가
    * Constraints:: Leading / Trailing / Top / Bottom / ++ 추가로 Height를 스크롤뷰보다 + 1 크게 추가 한다.
    * View Height를 스크롤뷰보다 크게 해줘야 스크롤이 적용된다. (중요) 
    * View에 control + drag -> Equals widths 적용 
4. View안에 컨텐츠들도 Constraints를 걸어준다.
    * centerX Center / Top (Size Inspector로 세세하게 잡아준다.)
   
# 화면고정
- Developement Info의 Device Orientation:: Landscape Left / Landsacpe Right 해제
