# Movie Review 리펙토링 - 객체지향

> 리펙토링에 사용되는 코드는 제 첫 프로젝트인 `영화 커뮤니티 사이트`입니다.
>
> 본 리펙토링 과정은 3 단계로 진행할 예정이며 객체지향, 테스트코드, 아키텍쳐 순으로 진행합니다.
>
> 객체지향의 의미를 생각하며 코드 리뷰 및 리펙토링을 진행합니다.



## 사전 설명

### 기술 스택

해당 프로젝트의 `Back-end` 시스템을 리펙토링 합니다.

사용된 기술은 다음과 같습니다.

- Django
- Django Rest Framework
- JWT



### 프로젝트 설명

영화 데이터를 스크래핑하여 DB에 저장하고 이를 바탕으로 한 커뮤니티 사이트 입니다.

대략적으로 다음과 같은 코드 구조로 이루어져있습니다.

1. 유저 데이터 CRUD
2. 영화 데이터 스크랩
3. 영화 데이터 CRUD
4. 영화 평점 CRUD
5. 영화 댓글 CRUD
6. 영화 좋아요

모델과 뷰에 대한 자세한 설명은 리펙토링 과정에서 이루어집니다.



## Accounts (User 관리)

### models

```python
# accounts.models
from django.db import models
from django.contrib.auth.models import AbstractUser


class User(AbstractUser):
    username = models.CharField(primary_key=True, max_length=50)
```

`User`는 커뮤니티 사용자를 의미합니다.

`AbstractUser`에 기본적인 User fields가 존재하기 때문에 User의 field는 다음과 같습니다.

```
password, last_login, is_superuser, first_name, last_name, email, is_staff, is_active, date_joined, username
```



### urls

__before__

```python
# accounts.urls
from django.urls import path
from . import views

from rest_framework_jwt.views import obtain_jwt_token, refresh_jwt_token, verify_jwt_token

urlpatterns = [
    path('get-users/', views.get_users),
    
    path('signup/', views.signup),
    path('api-token-auth/', obtain_jwt_token),
    path('token-verify/', refresh_jwt_token),
    path('token-refresh/', verify_jwt_token),

    path('user_update_delete/', views.user_update_delete),
    path('profile/', views.profile),
]
```

URI의 상태가 __RESTful__과는 거리가 멀어 수정할 여지가 있습니다.

- 모든 `User`의 목록을 반환: `get-users/` -> `GET users/`
- 새로운 `User`를 생성(회원가입): `signup/` -> `POST users/`
- `User` 데이터를 변경 또는 삭제: `user_update_delete/` -> `PATCH users/{user_id}`, `DELETE users/{user_id}`
- 한 `User`의 데이터를 반환: `profile` -> `GET users/{user_id}`

또한 본 코드에서는 `djangorestframework-jwt`를 이용해 __authorization__ 처리를 했으나, 이 라이브러리는 최신버전의 __python, Django, DRF__를 지원하지 않습니다.[Django REST framework JWT (jpadilla.github.io)](https://jpadilla.github.io/django-rest-framework-jwt/) 

![image-20220118213222319](movie_review_refectoring1.assets/image-20220118213222319.png)

따라서 __DRF__ 공식문서에서 제안하는 `djangorestframework-simplejwt`로 대체합니다. 이 과정은 객체지향적으로 리펙토링하는 본 문서의 목적과 관련이 적으므로 생략합니다.

새로운 URI를 적용한 코드는 다음과 같습니다.

  

__after__

```python
# accounts.urls
from django.urls import path
from rest_framework_simplejwt.views import (
    TokenObtainPairView,
    TokenRefreshView,
    TokenVerifyView
)

from . import views


urlpatterns = [
    path('users/', views.UserList.as_view()),
    path('users/<str:username>/', views.UserDetail.as_view()),

    path('login/', TokenObtainPairView.as_view()),
    path('token/refresh/', TokenRefreshView.as_view()),
    path('token/verify/', TokenVerifyView.as_view()),
]
```

기존 `get-users/`, `signup/`, `user_update_delete/`, `profile/`로 구분된 URI가 `users/`와 `users/{user_id}/`로 응축되었습니다. 또한 자원중심적인 URI로 __RESTful__ 에 더 알맞다고 할 수 있습니다.

__CBV__ 의 이름 `UserList`와 `UserDetail`은 __DRF__ 공식 문서에서 사용한 예제를 바탕으로 만든 것으로, 도메인에 맞는 직관적인 이름이 아닐 수 있습니다. [3 - Class based views - Django REST framework (django-rest-framework.org)](https://www.django-rest-framework.org/tutorial/3-class-based-views/)  
좀 더 경험을 쌓고 더 좋은 네이밍을 떠올려 적용해보려고 합니다.



> __User 대신에 get_user_model()을 사용하는 이유__
>
> 만약 `User`를 직접 참조하면, `settings.AUTH_USER_MODEL`이 변경되었을 때 변경을 반영할 수 없습니다.
>
> 본 프로젝트에서도 `settings.AUTH_USER_MODEL`은 기본값인 `django.contrib.auth.models.User`가 아닌 `accounts.models.User` 를 가리킵니다.
>
> ```python
> >>> from django.contrib.auth.models import User
> >>> User
> <class 'django.contrib.auth.models.User'>
> >>> from django.contrib.auth import get_user_model
> >>> get_user_model()
> <class 'accounts.models.User'>
> ```
>
> [Customizing authentication in Django | Django documentation | Django (djangoproject.com)](https://docs.djangoproject.com/en/4.0/topics/auth/customizing/#referencing-the-user-model)



### views

#### User 목록 반환, 회원가입

__before__

```python
# User 목록 반환
@api_view(['GET'])
def get_users(request):
    users = User.objects.all()
    serializer = UserSerializer(users, many=True)
    return Response(serializer.data)
```

```python
# 회원가입
@api_view(['POST'])
def signup(request):
	#1-1. Client에서 온 데이터를 받아서
    password = request.data.get('password')
    password_confirmation = request.data.get('passwordConfirmation')
		
	#1-2. 패스워드 일치 여부 체크
    if password != password_confirmation:
        return Response({'error': '비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
		
	#2. UserSerializer를 통해 데이터 직렬화
    serializer = UserSerializer(data=request.data)
    
	#3. validation 작업 진행 -> password도 같이 직렬화 진행
    if serializer.is_valid(raise_exception=True):
        user = serializer.save()
        #4. 비밀번호 해싱 후 
        user.set_password(request.data.get('password'))
        user.save()
    # password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
    return Response(serializer.data, status=status.HTTP_201_CREATED)
```

하나의 객체에서 __GET, POST__ 요청을 모두 처리하기 위해 위의 두 함수를 하나의 __CBV(Class Based View)__ 로 통합합니다.

이 때 __CBV__ 로 작성하는 방법에는 세 가지가 있는데 각각 소개하겠습니다.

  

__after 1 (APIView 사용)__

```python
from rest_framework.views import APIView

class UserList(APIView):
    """
    모든 User 목록을 반환하거나, 새로운 User를 등록할 수 있습니다.
    """

    def get(self, request):
        User = get_user_model()
        users = User.objects.all()
        serializer = UserSerializer(users, many=True)
        return Response(serializer.data)

    def post(self, request):
        password = request.data.get('password')
        password_confirmation = request.data.get('passwordConfirmation')
        if password != password_confirmation:
            res = Response({'error': '비밀번호가 일치하지 않습니다.'},
                           status=status.HTTP_400_BAD_REQUEST)
            return res
            
        serializer = UserSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            user = serializer.save()
            user.set_password(password)
            user.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

객체 하나에서 __GET, POST__ 를 분기하여 처리가 가능합니다.

  


__after 2 (Mixin 사용)__

```python
from rest_framework import mixins, generics

class UserList(mixins.ListModelMixin,
               mixins.CreateModelMixin,
               generics.GenericAPIView):
    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer

    def get(self, request, *args, **kwargs):
        return self.list(request, *args, **kwargs)
    
    def post(self, request, *args, **kwargs):
        return self.create(request, *args, **kwargs)
```

__GenericAPIView__ 를 이용하면 __Mixin__ 을 이용해 코드를 간결하게 만들 수 있습니다.

각 `Mixin`의 역할은 다음과 같습니다.

- __ListModelMixin__ : `get` 메서드에서 해당 object 전체를 반환합니다.
- __CreateModelMixin__ : `post` 메서드에서 해당 object를 생성합니다.

__GenericAPIView__ 는 `queryset`, `serializer_class`, 필요에 따라서는 `lookup_field`, `lookup_url_kwarg` 를 설정하여 사용합니다. 참고자료:[django-rest-framework/generics.py at master · encode/django-rest-framework (github.com)](https://github.com/encode/django-rest-framework/blob/master/rest_framework/generics.py)

또한 해당 링크에서 __Mixin__ 의 소스코드를 볼 수 있습니다. [django-rest-framework/mixins.py at master · encode/django-rest-framework (github.com)](https://github.com/encode/django-rest-framework/blob/master/rest_framework/mixins.py)

  


__after 3 (generic view 사용)__

```python
class UserList(generics.ListCreateAPIView):
    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer
```

__ListCreateAPIView__ 제네릭 뷰를 이용하면 그보다 더 간결한 코드가 작성됩니다.  
일반적인 목록조회와 객체생성 기능이 손쉽게 가능한 장점이 있지만 기존 코드( __password confirmation__ )를 적용하는데 어려움이 있습니다.

따라서 __Mixin__ 과 __Generic View__ 를 적절히 조합하여 리펙토링을 진행합니다.

  


__after__

```python
class UserList(generics.ListAPIView):
    """
    모든 User 목록을 반환하거나, 회원가입을 합니다.
    """

    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer

    def post(self, request):
        password = request.data.get('password')
        password_confirmation = request.data.get('passwordConfirmation')
        if password != password_confirmation:
            return Response({'error': '비밀번호가 일치하지 않습니다.'},
                            status=status.HTTP_400_BAD_REQUEST)

        serializer = UserSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            saved_user = serializer.save()
            saved_user.set_password(password)
            saved_user.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
```

회원가입은 __password confirmation__ 과 __password 암호화__ 로직을 수행하기 위해 상속받지 않았습니다.

  

#### 프로필 보기, 회원정보 변경, 회원탈퇴

__before__

```python
# 프로필 보기
@api_view(['GET'])
@authentication_classes([JSONWebTokenAuthentication])
@permission_classes([IsAuthenticated])
def profile(request):
    User = get_user_model()
    person = get_object_or_404(User, username=request.user)
    serializer = UserSerializer(person)
    return Response(serializer.data)
```

```python
# 회원정보 변경, 회원탈퇴
@api_view(['PUT', 'DELETE'])
def user_update_delete(request):
    if request.method == 'PUT':
        #1-1. Client에서 온 데이터를 받아서
        old_password = request.data.get('oldPassword')
            
        #1-2. 패스워드 일치 여부 체크
        if old_password != request.user.password:
            return Response({'error': '이전 비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
            
        #2. UserSerializer를 통해 데이터 직렬화
        serializer = UserSerializer(data=request.data)
        
        #3. validation 작업 진행 -> password도 같이 직렬화 진행
        if serializer.is_valid(raise_exception=True):
            user = serializer.save()
            #4. 비밀번호 해싱 후 
            user.set_password(request.data.get('newPassword'))
            user.save()
        # password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
        return Response(serializer.data, status=status.HTTP_201_CREATED)
    else:
        user = User.objects.get(pk=request.user)
        user.delete()
        return Response({'id가 삭제되었습니다'}).
```

앞서 만들었던 `UserList` 클래스의 경우와 마찬가지로, __CBV__ 로 통합하여 __Request Method__ 로 구분합니다.

  


__after 1 (APIView 사용)__

```python
class UserDetail(APIView):
    """
    User의 프로필 보기, 회원정보수정, 회원탈퇴를 합니다.
    """

    def _get_object(self, username: str):
        User = get_user_model()
        return get_object_or_404(User, username=username)
            
    def get(self, request, username: str):
        user = self._get_object(username)
        serializer = UserSerializer(user)
        return Response(serializer.data)

    @authentication_classes([JSONWebTokenAuthentication])
    @permission_classes([IsAuthenticated])
    def patch(self, request, username: str):
        old_password = request.data.get('oldPassword')
        new_password = request.data.get('newPassword')
        if old_password != request.user.password:
            res = Response({'error': '비밀번호가 일치하지 않습니다.'},
                           status=status.HTTP_400_BAD_REQUEST)
            return res

        user = self._get_object(username)
        serializer = UserSerializer(user, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            user = serializer.save()
            if new_password:
                user.set_password(new_password)
                user.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)
    
    @authentication_classes([JSONWebTokenAuthentication])
    @permission_classes([IsAuthenticated])
    def delete(self, request, username: str):
        user = self._get_object(username)
        if user != request.user:
            res = Response({'error': '사용자가 일치하지 않습니다.'},
                           status=status.HTTP_401_UNAUTHORIZED)
        user.delete()
        res = Response({'success': f"'{username}' 유저가 삭제되었습니다."})
        return res
```

여기서는 기존 `put` 메서드를 `patch` 로 변경하였습니다.  
현재는 비밀번호 변경 기능밖에 없지만, 추후 email, 이름 데이터 등을 변경할 수 있도록 확장성을 고려해 작성했습니다.

이를 __generic view__ 와 __Mixin__ 을 이용하여 리펙토링 합니다.

  

__after 1__

```python
class UserDetail(mixins.DestroyModelMixin,
                 generics.RetrieveAPIView):
    """
    User의 프로필 보기, 회원정보수정, 회원탈퇴를 합니다.
    """

    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer
    lookup_field = 'username'

    @permission_classes([IsAuthenticated])
    def patch(self, request, username: str):
        user = get_object_or_404(self.User, username=username)
        if user != request.user:
            return Response({'error': '사용자 권한이 없습니다.'},
                            status=status.HTTP_403_FORBIDDEN)

        old_password = request.data.get('oldPassword')
        new_password = request.data.get('newPassword')
        if not check_password(old_password, request.user.password):
            return Response({'error': '비밀번호가 일치하지 않습니다.'},
                            status=status.HTTP_400_BAD_REQUEST)

        serializer = UserSerializer(user, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            saved_user = serializer.save()
            if new_password:
                saved_user.set_password(new_password)
                saved_user.save()
        return Response(serializer.data)
    
    @permission_classes([IsAuthenticated])
    def delete(self, request, username: str):
        user = get_object_or_404(self.User, username=username)
        if user != request.user:
            return Response({'error': '사용자 권한이 없습니다.'},
                            status=status.HTTP_401_UNAUTHORIZED)
        return self.destroy(request, username)
```

기존의 __authentication__ 데코레이터는 있지만 __authorization__ 데코레이터는 없어졌기 때문에 코드의 길이와 가독성이 크게 향상되지는 않았습니다.  
그래서 __authorization__ 로직을 수행하는 __decorator__ 를 만들어서 적용해봅니다.

```python
# module/custom_decorator.py
from django.shortcuts import get_object_or_404
from rest_framework.response import Response


def authorization(func):
    def wrapper(self, request, *args, **kwargs):
        user = get_object_or_404(self.User, *args, **kwargs)
        if user != request.user:
            return Response({'error': '사용자 권한이 없습니다.'})
        return func(self, request, *args, **kwargs)
    return wrapper
```

이 __decorator__ 는 다른 앱에도 사용해야하기 때문에 모듈화 하여 `~/module/custom_decorator.py` 경로에 저장하였습니다. 여기서 `~` 는 `manage.py` 가 있는 경로입니다.

적용이 완료된 코드는 아래와 같습니다.

  

__after__

```python
from module import custom_decorator


class UserDetail(mixins.DestroyModelMixin,
                 generics.RetrieveAPIView):
    """
    User의 프로필 보기, 회원정보수정, 회원탈퇴를 합니다.
    """

    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer
    lookup_field = 'username'

    @permission_classes([IsAuthenticated])
    @custom_decorator.authorization
    def patch(self, request, username: str):
        old_password = request.data.get('oldPassword')
        new_password = request.data.get('newPassword')
        if not check_password(old_password, request.user.password):
            return Response({'error': '비밀번호가 일치하지 않습니다.'},
                            status=status.HTTP_400_BAD_REQUEST)

        user = get_object_or_404(self.User, username=username)
        serializer = UserSerializer(user, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            saved_user = serializer.save()
            if new_password:
                saved_user.set_password(new_password)
                saved_user.save()
        return Response(serializer.data)
    
    @authentication_classes([IsAuthenticated])
    @custom_decorator.authorization
    def delete(self, request, username: str):
        return self.destroy(request, username)
```

  

### 정리

이상으로 `accounts/` 앱에 대한 리펙토링을 마쳤습니다. 최대한 객체지향을 생각하며 코드를 작성해보려고 했는데 __DRF__ 에 라이브러리에 이미 __Mixin__ , __Generic View__ 등 많은 기능들이 이미 내장되어있었고, 이를 활용하려다보니 직접 작성한 코드의 비중이 줄었습니다.

또, __객체지향__ 보다 __RESTful__ 에 더 초첨이 맞춰진 것 같아 아쉬웠습니다.

그래도 __django-rest-framework__ 와 __djangorestframework-simplejwt__ 의 github 코드를 보며 객체지향적인 코드를 많이 보고 배웠습니다.

다음 포스트에는 영화 데이터를 컨트롤하는 `movies` 앱에 대해 코드 리펙토링을 진행해보겠습니다.

감사합니다.

  

#### 참고자료

- [3 - Class based views - Django REST framework (django-rest-framework.org)](https://www.django-rest-framework.org/tutorial/3-class-based-views/)
- [django-rest-framework/rest_framework at master · encode/django-rest-framework (github.com)](https://github.com/encode/django-rest-framework/tree/master/rest_framework)
- [djangorestframework-simplejwt/rest_framework_simplejwt at master · jazzband/djangorestframework-simplejwt (github.com)](https://github.com/jazzband/djangorestframework-simplejwt/tree/master/rest_framework_simplejwt)

  

#### 결과

- 리펙토링 전

```python
from rest_framework.permissions import IsAuthenticated
from rest_framework_jwt.authentication import JSONWebTokenAuthentication
from .models import User
from django.shortcuts import get_object_or_404
from rest_framework import status
from rest_framework.decorators import api_view, authentication_classes, permission_classes
from rest_framework.response import Response
from .serializers import UserSerializer
from django.contrib.auth import get_user_model


@api_view(['GET'])
@authentication_classes([JSONWebTokenAuthentication])
@permission_classes([IsAuthenticated])
def profile(request):
    User = get_user_model()
    person = get_object_or_404(User, username=request.user)
    serializer = UserSerializer(person)
    return Response(serializer.data)


# 유저정보 가져오기
@api_view(['GET'])
def get_users(request):
    users = User.objects.all()
    serializer = UserSerializer(users, many=True)
    return Response(serializer.data)


@api_view(['POST'])
def signup(request):
	#1-1. Client에서 온 데이터를 받아서
    password = request.data.get('password')
    password_confirmation = request.data.get('passwordConfirmation')
		
	#1-2. 패스워드 일치 여부 체크
    if password != password_confirmation:
        return Response({'error': '비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
		
	#2. UserSerializer를 통해 데이터 직렬화
    serializer = UserSerializer(data=request.data)
    
	#3. validation 작업 진행 -> password도 같이 직렬화 진행
    if serializer.is_valid(raise_exception=True):
        user = serializer.save()
        #4. 비밀번호 해싱 후 
        user.set_password(request.data.get('password'))
        user.save()
    # password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
    return Response(serializer.data, status=status.HTTP_201_CREATED)


@api_view(['PUT', 'DELETE'])
def user_update_delete(request):
    if request.method == 'PUT':
        #1-1. Client에서 온 데이터를 받아서
        old_password = request.data.get('oldPassword')
            
        #1-2. 패스워드 일치 여부 체크
        if old_password != request.user.password:
            return Response({'error': '이전 비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
            
        #2. UserSerializer를 통해 데이터 직렬화
        serializer = UserSerializer(data=request.data)
        
        #3. validation 작업 진행 -> password도 같이 직렬화 진행
        if serializer.is_valid(raise_exception=True):
            user = serializer.save()
            #4. 비밀번호 해싱 후 
            user.set_password(request.data.get('newPassword'))
            user.save()
        # password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
        return Response(serializer.data, status=status.HTTP_201_CREATED)
    else:
        user = User.objects.get(pk=request.user)
        user.delete()
        return Response({'id가 삭제되었습니다'})
```

  

- 리펙토링 후

```python
from django.contrib.auth import get_user_model
from django.contrib.auth.hashers import check_password
from django.shortcuts import get_object_or_404
from rest_framework import status, mixins, generics
from rest_framework.decorators import authentication_classes, permission_classes
from rest_framework.permissions import IsAuthenticated
from rest_framework.response import Response

from .serializers import UserSerializer
from module import custom_decorator


class UserList(generics.ListAPIView):
    """
    모든 User 목록을 반환하거나, 회원가입을 합니다.
    """

    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer

    def post(self, request):
        password = request.data.get('password')
        password_confirmation = request.data.get('passwordConfirmation')
        if password != password_confirmation:
            return Response({'error': '비밀번호가 일치하지 않습니다.'},
                            status=status.HTTP_400_BAD_REQUEST)

        serializer = UserSerializer(data=request.data)
        if serializer.is_valid(raise_exception=True):
            saved_user = serializer.save()
            saved_user.set_password(password)
            saved_user.save()
        return Response(serializer.data, status=status.HTTP_201_CREATED)


class UserDetail(mixins.DestroyModelMixin,
                 generics.RetrieveAPIView):
    """
    User의 프로필 보기, 회원정보수정, 회원탈퇴를 합니다.
    """

    User = get_user_model()
    queryset = User.objects.all()
    serializer_class = UserSerializer
    lookup_field = 'username'

    @permission_classes([IsAuthenticated])
    @custom_decorator.authorization
    def patch(self, request, username: str):
        old_password = request.data.get('oldPassword')
        new_password = request.data.get('newPassword')
        if not check_password(old_password, request.user.password):
            return Response({'error': '비밀번호가 일치하지 않습니다.'},
                            status=status.HTTP_400_BAD_REQUEST)

        user = get_object_or_404(self.User, username=username)
        serializer = UserSerializer(user, data=request.data, partial=True)
        if serializer.is_valid(raise_exception=True):
            saved_user = serializer.save()
            if new_password:
                saved_user.set_password(new_password)
                saved_user.save()
        return Response(serializer.data)
    
    @authentication_classes([IsAuthenticated])
    @custom_decorator.authorization
    def delete(self, request, username: str):
        return self.destroy(request, username)
```
