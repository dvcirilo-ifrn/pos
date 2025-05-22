# Tutorial Django REST Framework

Nesse tutorial criaremos um projetinho básico de cadastro de Bandas/Músicas/Álbuns chamado *spotifake*.

## Preparativos

Inicie o projeto criando um diretório (pasta) com o nome do projeto (*spotifake*).

Dentro da pasta, crie um ambiente virtual (venv)
```sh
python -m venv venv
```

Ative o ambiente virtual (no Windows). Não é necessário entrar na pasta, mas se entrar, lembre-se de voltar para a pasta raiz do projeto (*spotifake*):
```sh
.\venv\Scripts\Activate.ps1
```

Com o `venv` ativado, instale o *Django*:
```sh
pip install django djangorestframework
```

Inicie um projeto *Django* chamado `config`. Atente ao `.` no final. Essa pasta `config` contém os arquivos básicos gerados pelo *Django*. Perceba que ele também cria um arquivo `manage.py` que vai ser responsável pelos comandos de gerenciamento do projeto daqui pra frente.
```sh
django-admin startproject config .
```

Inicie um aplicativo para seu projeto chamado `api`. Vai aparecer uma pasta chamada `api` com os arquivos básicos do aplicativo, como `models.py`, `views.py`, etc.
```sh
python manage.py startapp api
```

O novo app deve ser adicionado ao `INSTALLED_APPS` do arquivo `settings.py`, na pasta `config`. Além disso já vamos adicionar o app `rest_framework` ao `settings.py`

```py
# Application definition

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'api',
]

```

Já que iniciamos os arquivos básicos, podemos iniciar um projeto Git na pasta também, porém para que nenhum arquivo desnecessário vá para o repositório, criaremos um arquivo `.gitignore` com o conteúdo abaixo. 
```
venv
__pycache__
*.sqlite3
```

Para iniciar o projeto Git:
```sh
git init
```

E o primeiro *commit*, adicionando todos os arquivos do diretório, exceto os que estão no `.gitignore`:
```sh
git add .
git commit -m "Commit inicial"
```

## Desenvolvimento da API

Iniciaremos o projeto definindo os Models, responsáveis pela estrutura de dados do sistema.
Nosso arquivo `models.py` terá o seguinte conteúdo:

```py
from django.db import models

class Artista(models.Model):
    nome = models.CharField(max_length=100)
    local = models.CharField(max_length=100)
    ano_criacao = models.IntegerField()

class Album(models.Model):
    artista = models.ForeignKey(Artista, on_delete=models.CASCADE)
    nome = models.CharField(max_length=100)
    ano = models.IntegerField()

class Musica(models.Model):
    album = models.ForeignKey(Album, on_delete=models.CASCADE)
    nome = models.CharField(max_length=100)
    segundos = models.IntegerField()
```

Com os Models definidos, vamos escrever no banco de dados usando as migrations. Para criar as migrations:
```sh
python manage.py makemigrations
```

E para aplicar:
```
python manage.py migrate
```

Faremos isso sempre que os Models forem modificados, ou quando vamos inicializar o projeto em outro computador.

Para o DRF (*Django REST Framework*) vamos criar o arquivo `serializers.py` (dentro do diretório do app). Os *serializers* são responsáveis por traduzir os objetos do nosso sistema em *strings* JSON, que serão enviadas ao cliente e vice-versa.

```py
from rest_framework import serializers
from .models import Artista, Album, Musica

class ArtistaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Artista
        fields = '__all__'

class AlbumSerializer(serializers.ModelSerializer):
    class Meta:
        model = Album
        fields = '__all__'

class MusicaSerializer(serializers.ModelSerializer):
    class Meta:
        model = Musica
        fields = '__all__'
```

Agora podemos criar as *views*, que são responsáveis pela comunicação com o cliente.

```py
from django.shortcuts import render
from rest_framework import viewsets
from .models import Artista, Album, Musica
from .serializers import ArtistaSerializer, AlbumSerializer, MusicaSerializer

class ArtistaViewSet(viewsets.ModelViewSet):
    """
    Permite a manipulação de dados de Artistas
    """
    queryset = Artista.objects.all()
    serializer_class = ArtistaSerializer

class AlbumViewSet(viewsets.ModelViewSet):
    """
    Permite a manipulação de dados de Albuns
    """
    queryset = Album.objects.all()
    serializer_class = AlbumSerializer

class MusicaViewSet(viewsets.ModelViewSet):
    """
    Permite a manipulação de dados de Músicas
    """
    queryset = Musica.objects.all()
    serializer_class = MusicaSerializer
```

Por último, vamos configurar as rotas/*URLs* do sistema, em uma API REST também chamados de *endpoints*.
O arquivo `urls.py` da pasta `config` deve ser:

```py
from django.contrib import admin
from django.urls import path, include
from rest_framework import routers

from api import views

router = routers.DefaultRouter()
router.register(r'artistas', views.ArtistaViewSet)
router.register(r'albuns', views.AlbumViewSet)
router.register(r'musicas', views.MusicaViewSet)

# Wire up our API using automatic URL routing.
urlpatterns = [
    path('admin/', admin.site.urls),
    path('', include(router.urls)),
]

urlpatterns += router.urls
```

Nossa API está *pronta*. Obviamente esse é um exemplo bem básico, mas já é possível fazer *requests* e testar a API usando:
```sh
python manage.py runserver
```

E acessando o serviço em http://127.0.0.1:8000/. Não esqueça de adicionar e fazer o *commit* dos arquivos criados.

### Opcional: Configurando o Admin

Primeiro crie um *superuser*:
```sh
python manage.py createsuperuser
```

Configure o arquivo `admin.py` para mostrar todos os *Models*.
```py
from django.contrib import admin

from .models import Artista, Album, Musica

admin.site.register(Artista)
admin.site.register(Album)
admin.site.register(Musica)
```

Para uma melhor visualização, é interessante criar os métodos `__str__()` para cada *Model*:
```py
def __str__(self):
    return self.nome
```

### Opcional: Rotas Aninhadas (Nested Routes)
O DRF não aceita rotas aninhadas por padrão, por exemplo, para ver os albuns do artista com id 1, não poderíamos usar: `/artista/1/albuns`.

Para obter essa funcionalidade, use o pacote [drf-nested-routers](https://github.com/alanjds/drf-nested-routers).
```sh
pip install drf-nested-routers
```

Veja a documentação [aqui](https://github.com/alanjds/drf-nested-routers) para mais detalhes.

### Opcional: CORS Headers

A política de Cross Origin Resource Sharing dos bloqueia requisições entre serviços diferentes, a não ser que explicitamente habilitados pelos *headers* (cabeçalhos) CORS.
O objetivo é aumentar a segurança evitando a comunicação com cliente não habilitados.
Para habilitar os CORS headers no DRF usamos o pacote [django-cors-header](https://github.com/adamchainz/django-cors-headers)
Siga a documentação para instalar e habilitar e configure a variável `CORS_ALLOWED_ORIGINS` ou `CORS_ALLOW_ALL_ORIGINS` em `config/settings.py`

