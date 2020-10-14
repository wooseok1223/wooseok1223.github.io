---
title: Django (media 파일)
author: SeoWooSeok
layout: post
---
<h3> Static & Media File </h3>
- Static 파일
1. 개발 리소스로서의 정적인 파일 (js, css, image ) 등
2. 앱 / 프로젝트 단위로 저장 / 서빙
- Media 파일
1. FileField/ImageField를 통해 저장한 모든 파일
2. DB필드에는 저장경로를 저장하며, 파일은 파일 스토리지에 저장
** 실제로 문자열을 저장하는 필드 (중요) ** 
3. 프로젝트 단위로 저장/서빙

<h3> File procress </h3>
1. HttpRequest.Files를 통해 파일이 전달
2. 뷰 로직이나 폼로직을 통해 , 유효성 검증 수행
3. FileField/ImageField 필드에 "경로(문자열)"을 저장
4. settings.MEDIA_ROOT 경로에 파일 저장

<h3>Django Setting</h3>
- 장고의 메인 앱의 settings에 기본세팅이 필요
MEDIA_URL = '/media/'
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')

<h3> File Field and Image Field </h3>
<h4>-FileField-<h4>
File Storage API를 통해 파일을 저장
장고에서는 File System Storage만 지원. django-storages를 통해 확장 지원.
해당 필드를 옵션 필드로 두고자 할 경우, blank=True 옵션 적용
<h4>-ImageField (FileField 상속)-<h4>
Pillow (이미지 처리 라이브러리)를 통해 이미지 width/height 획득
Pillow 미설치 시에, ImageField를 추가한 makemigrations 수행에 실패합니다.
위 필드를 상속받은 커스텀 필드를 만드실 수도 있습니다.
ex) PDFField, ExcelField 등

<pre><code>
class Post(models.Model):
    photo = models.ImageField(blank=True, upload_to='instagram/post/%Y/%m/%d')
</code></pre>

위처럼 upload_to 속성을 사용하면 아래와 같이 이미지가 생성된다.
<span class="image right"><img src="{{ 'assets/images/2020-10-11.png' | relative_url }}" alt="" /></span>





