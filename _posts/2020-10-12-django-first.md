---
title: Django 되돌아보기 1
author: SeoWooSeok
layout: post
---
<h3> model 기본 규칙 </h3>
 - 데이터 베이스 테이블과 파이썬 클래스를 1:1로 매핑
1. 모델 클레스명은 단수형으로 지정(PascalCase 네이밍 사용)
2. 매핑되는 모델 클래스는 DB 테이블 필드 내역과 일치해야함
3. 모델을 만들기 전에는 서비스에 맞는 데이터베이스 설계가 필요

<h2> model 테이블 이름 선정 </h2>
- DB 테이블명 : 디폴트 "앱이름_모델명"

**예시))**

<h2>Blog App</h2> 
Post model -> "blog_post"

Comment model -> "comment_post"
<h2>Shop App</h2>
Item model -> "shop_item"

Review model -> "shop_review"

<h3> 주의 사항 </h3>
 - 설계한 데이터베이스 구조에 따라서 필드값을 타이트하게 지정해줘야 입력값 오류를 방지할 수 있음
 1. blank/null 최소화
 2. 프론트에서 유효성 검사는 사용자의 편의를 위해서지만 <b>백앤드 필수</b>입니다.
 3. 직접 유효성 로직을 만들지말고 이미 구성된 Features을 가져다 사용하자
 
<pre><code>
class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    message = models.TextField()
    photo = models.ImageField(blank=True, upload_to='instagram/post/%Y/%m/%d')
    tag_set = models.ManyToManyField('Tag', blank=True)
    is_public = models.BooleanField(default=False, verbose_name="공개여부")
    created_at = models.DateField(auto_now_add=True)
    update_at = models.DateField(auto_now=True)
    
class Comment(models.Model):
    post = models.ForeignKey(Post, on_delete=models.CASCADE,
                             limit_choices_to={'is_public': True})  # post_id field 생성
    message = models.TextField()
    created_at = models.DateField(auto_now_add=True)
    update_at = models.DateField(auto_now=True)

class Tag(models.Model):
    name = models.CharField(max_length=50, unique=True)

    def __str__(self):
        return self.name 
</code></pre>




