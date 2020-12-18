---
title: "장고 커스텀 유저"
categories:
 - Django
last_modified_at: 2020-12-10
toc: true
toc_sticky: true
---

## 장고 커스텀 유저 만들기

장고가 제공하는 기본 유저 모델을 사용하지 않고 직접 만드는 유저를 만들어 보겠습니다.

### 1.

```shell
$ python manage.py startapp Author
```

Author 이라는 이름의 앱을 만들고, settings.py 에 추가합니다.

### 2.

Custom User Model 을 만들기 위해선 models.py 에 두 개의 클래스 UserManager,
BaseUser 를 만들어야 합니다.

먼저 장고 기본 유저 모델의 UserManager 부터 알아보겠습니다.

```python
class UserManager(BaseUserManager):
    use_in_migrations = True

    def _create_user(self, username, email, password, **extra_fields):
        """
        Create and save a user with the given username, email, and password.
        """
        if not username:
            raise ValueError('The given username must be set')
        email = self.normalize_email(email)
        username = self.model.normalize_username(username)
        user = self.model(username=username, email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_user(self, username, email=None, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', False)
        extra_fields.setdefault('is_superuser', False)
        return self._create_user(username, email, password, **extra_fields)

    def create_superuser(self, username, email=None, password=None, **extra_fields):
        extra_fields.setdefault('is_staff', True)
        extra_fields.setdefault('is_superuser', True)

        if extra_fields.get('is_staff') is not True:
            raise ValueError('Superuser must have is_staff=True.')
        if extra_fields.get('is_superuser') is not True:
            raise ValueError('Superuser must have is_superuser=True.')

        return self._create_user(username, email, password, **extra_fields)

    def with_perm(self, perm, is_active=True, include_superusers=True, backend=None, obj=None):
        if backend is None:
            backends = auth._get_backends(return_tuples=True)
            if len(backends) == 1:
                backend, _ = backends[0]
            else:
                raise ValueError(
                    'You have multiple authentication backends configured and '
                    'therefore must provide the `backend` argument.'
                )
        elif not isinstance(backend, str):
            raise TypeError(
                'backend must be a dotted import path string (got %r).'
                % backend
            )
        else:
            backend = auth.load_backend(backend)
        if hasattr(backend, 'with_perm'):
            return backend.with_perm(
                perm,
                is_active=is_active,
                include_superusers=include_superusers,
                obj=obj,
            )
        return self.none()
```
username 이 아닌 email 을 이용해 로그인하기 위해서 username 을 email 로 custom 하겠습니다.

```python
class AuthorUserManager(BaseUserManager):

    def create_user(self, email, password=None, **extra_fields):

        if not email:
            raise ValueError('이메일은 필수 항목입니다.')
        email = self.normalize_email(email)
        user = self.model(email=email, **extra_fields)
        user.set_password(password)
        user.save(using=self._db)
        return user

    def create_superuser(self, email, password, **extra_fields):
        user = self.create_user(email=email, password=password)
        user.is_admin = True
        user.save(using=self._db)
        return user
```

이제 AbstractBaseUser 를 상속받는 Author Model 을 만들면 끝!

```python
class Author(AbstractBaseUser):
    objects = AuthorUserManager()
    email = models.EmailField(verbose_name="이 메 일 !!!!!", max_length=64, unique=True)
    is_active = models.BooleanField(default=True)
    is_admin = models.BooleanField(default=False)
    is_superuser = models.BooleanField(default=False)

    USERNAME_FIELD = 'email'

    def __str__(self):
        return self.email

    def has_perm(self, perm, obj=None):
        return True

    def has_module_perms(self, app_label):
        return True

    @property
    def is_staff(self):
        return self.is_admin
```

settings.py 에서 AUTH_USER_MODEL = 'author.Author' 을 써주면 해당 프로젝트의 유저는 Author 가 된다!

