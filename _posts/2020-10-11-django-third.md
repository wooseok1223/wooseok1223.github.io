---
title: Django (admin)
author: SeoWooSeok
layout: post
---
<h3> django admin </h3>
1. django.contrib.admin 앱을 통해 제공
2. 모델 클래스 등록을 통해, 조회/추가/수정/삭제 웹UI를 제공
3. 내부적으로 Django Form을 적극적으로 사용

<h3> 모델 클래스를 admin에 등록하기 </h3>
<pre><code>
from django.contrib import admin 
from .models import Item

--방법 1--
admin.site.register(Item)
--방법 2--
class ItemAdmin(admin.ModelAdmin):
    pass
admin.site.register(Item, ItemAdmin)
--방법 3--
@admin.register(Item)
class ItemAdmin(admin.ModelAdmin):
    pass
</code></pre>

<h3> admin code </h3>
- list_display = 모델리스트에 출력할 컬럼지정
- ist_display_links = list_display 지정된 이름 중에, detail 링크를 걸 속성 리스트
- search_fields = admin내 검색UI를 통해, DB를 통한 where 쿼리 대상 필드 리스트
<pre><code>
@admin.register(Post) #Wrapping
class PostAdmin(admin.ModelAdmin):
    list_display = ["id", "photo_tag", "message", "message_length", "is_public", "created_at", "update_at"]
    list_display_links = ["message"]
    list_filter = ["created_at", "is_public"]
    search_fields = ["message"]

    def photo_tag(self, post):
        if post.photo:
            return mark_safe(f"<img src = '{post.photo.url}' style='width:72px;'/>")
        return None
    def message_length(self, post):
        return f"{len(post.message)}글자"

    message_length.short_description = "메세지 글자수"

@admin.register(Comment) #Wrapping
class CommentAdmin(admin.ModelAdmin):
    pass

@admin.register(Tag) #Wrapping
class TagAdmin(admin.ModelAdmin):
    pass
</code></pre>




