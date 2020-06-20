# 맛집 공유 사이트

![res_share](https://user-images.githubusercontent.com/51525202/85197729-e1169b00-b31d-11ea-9bd3-948794071750.png)

## 학습 내용

- django 프로젝트 세팅 및 CRUD의 구현
- templates
- stmp library를 사용한 메일 발신
- pythonanywhere를 통한 depoly


## Templates  

``` python
# settings.py

TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]
```

python에서는 여러 템플릿 엔진을 사용할 수 있다. 
위는 django의 기본 설정이며, JSP의 scriptlet처럼 서버에서 HTML에 컨텐츠를 동적으로 생성할 수 있다.

``` html
<ul class="restaurantListDiv nav nav-pills nav-stacked">
	{% for category in categories %}
	<li class="category deactive">{{ category.category_name}}</li>
	<ul class="restaurantList">

		{% for restaurant in restaurants %}
		{% if restaurant.category == category %}
		<div class="input-group">
			<span class="input-group-addon">
				<input name="checks" id="check{{restaurant.id}}" type="checkbox" value="{{restaurant.id}}">
			</span>
			<a href="restaurantDetail/{{restaurant.id}}"><input name="res{{restaurant.id}}" id="res{{restaurant.id}}" type="text" class="form-control" disabled style="cursor: pointer;" value="{{restaurant.restaurant_name}}"></a>
		</div>
		{% endif %}
		{% endfor %}

	</ul>
	{% endfor %}
</ul>
```


## E-mail 발신

파이썬에서는 STMP 관련된 stmplib을 제공하는데, django에서는 이를 이용하여 가벼운 wrapper를 제공하고 있다.

``` python
from django.core.mail import send_mail

# send_mail(subject, message, from_email, recipient_list, fail_silently=False, auth_user=None, auth_password=None, connection=None, html_message=None)[source]
send_mail(
    'Subject here',
    'Here is the message.',
    'from@example.com',
    ['to@example.com'],
    fail_silently=False,
)
```


``` python
# settings.py
# STMP 설정 관련 정보를 세팅한다.

# Email
EMAIL_BACKEND = 'django.core.mail.backends.smtp.EmailBackend'
EMAIL_HOST = 'smtp.gmail.com'
EMAIL_USE_TLS = True
EMAIL_PORT = 587 # google stmp 권장 port
EMAIL_HOST_USER = ''
EMAIL_HOST_PASSWORD = ''
```

## pythonanywhere

pythonanywhere은 python으로 구축된 웹 서버를 무료로 호스팅할 수 있게 paas service를 제공해준다. 
제공하는 shell에서 git remote 연동 및 환경 구성 후 배포 가능하다.

