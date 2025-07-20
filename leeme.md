aerolineas
¡Excelente proyecto! Aquí tienes una guía completa y detallada para crear tu "Proyecto Aerolíneas" con Django y Bootstrap, siguiendo todos los puntos que solicitaste y basándome en las imágenes proporcionadas.

---

### **Introducción**

Este tutorial te guiará paso a paso para construir una aplicación web de gestión de aerolíneas. Aprenderás a configurar un entorno de desarrollo, crear un proyecto Django desde cero, definir modelos de datos, crear vistas para manejar la lógica de negocio (CRUD: Crear, Leer, Actualizar, Eliminar) y diseñar una interfaz de usuario moderna y funcional con Bootstrap.

---

### **Paso 1: Configuración del Entorno Virtual**

Un entorno virtual es una carpeta que contiene una instalación de Python independiente, permitiéndonos manejar las dependencias de nuestro proyecto sin afectar a otros proyectos o al sistema global.

1.  **Crear el Entorno Virtual:**
    Abre tu terminal o línea de comandos y navega a la carpeta donde quieres crear tu proyecto. Luego, ejecuta:
    ```bash
    # Para Windows
    python -m venv env

    # Para macOS/Linux
    python3 -m venv env
    ```
    Esto creará una carpeta llamada `env` con los archivos necesarios.

2.  **Activar el Entorno Virtual:**
    Para empezar a usarlo, debes activarlo.
    ```bash
    # Para Windows (cmd.exe)
    .\env\Scripts\activate

    # Para Windows (PowerShell)
    .\env\Scripts\Activate.ps1

    # Para macOS/Linux
    source env/bin/activate
    ```
    Una vez activado, verás `(env)` al principio de la línea de tu terminal.

3.  **Instalar Django:**
    Con el entorno activado, instala Django usando `pip`.
    ```bash
    pip install django
    ```

---

### **Paso 2: Creación del Proyecto y la Aplicación**

1.  **Crear el Proyecto Django:**
    Este comando crea la estructura principal del proyecto.
    ```bash
    django-admin startproject backend_aerolineas .
    ```
    *Nota: El `.` al final evita crear un subdirectorio extra, colocando `manage.py` en la raíz actual.*

2.  **Crear la Aplicación de Gestión:**
    Las aplicaciones son módulos que realizan una función específica.
    ```bash
    python manage.py startapp gestion_vuelos
    ```

### **Paso 3: Estructura de Carpetas y Archivos**

Después de los pasos anteriores, tu estructura de carpetas debería ser la siguiente. Crearemos manualmente las carpetas que faltan (`static` y `templates`).

```
/tu_proyecto/
├── backend_aerolineas/     <-- Carpeta de configuración del proyecto
│   ├── __init__.py
│   ├── asgi.py
│   ├── settings.py         <-- Archivo de configuración principal
│   ├── urls.py             <-- Archivo de URLs principal
│   └── wsgi.py
├── gestion_vuelos/         <-- Nuestra aplicación
│   ├── migrations/
│   ├── templates/          <-- ¡Crea esta carpeta!
│   │   └── gestion_vuelos/ <-- ¡Y esta subcarpeta!
│   │       ├── aeropuerto/ <-- ¡Y esta!
│   │       │   ├── form_aeropuerto.html
│   │       │   └── listado_aeropuertos.html
│   │       ├── pasajero/   <-- ¡Y esta!
│   │       │   ├── form_pasajero.html
│   │       │   └── listado_pasajeros.html
│   │       ├── vuelo/      <-- ¡Y esta!
│   │       │   ├── form_vuelo.html
│   │       │   └── listado_vuelos.html
│   │       ├── base.html
│   │       └── inicio.html
│   ├── __init__.py
│   ├── admin.py
│   ├── apps.py
│   ├── models.py           <-- Aquí definimos la base de datos
│   ├── tests.py
│   ├── urls.py             <-- ¡Crea este archivo!
│   └── views.py            <-- Aquí va la lógica de las vistas
├── static/                 <-- ¡Crea esta carpeta!
│   └── images/
│       └── airplane.svg    <-- (Opcional, para la imagen de inicio)
├── .env/                   <-- Entorno virtual
└── manage.py               <-- Utilidad de línea de comandos de Django
```

---

### **Paso 4: Configuración del Proyecto (`settings.py` y `urls.py`)**

1.  **Configurar `backend_aerolineas/settings.py`:**
    Abre este archivo y realiza las siguientes modificaciones:

    ```python
    # backend_aerolineas/settings.py

    # ... (otras configuraciones)
    import os # Asegúrate que 'os' está importado al inicio

    # Añade tu aplicación a la lista de aplicaciones instaladas
    INSTALLED_APPS = [
        'django.contrib.admin',
        'django.contrib.auth',
        'django.contrib.contenttypes',
        'django.contrib.sessions',
        'django.contrib.messages',
        'django.contrib.staticfiles',
        'gestion_vuelos', # <-- Añadir nuestra aplicación
    ]

    # ...

    # Configura la dirección de las plantillas (templates)
    TEMPLATES = [
        {
            'BACKEND': 'django.template.backends.django.DjangoTemplates',
            'DIRS': [os.path.join(BASE_DIR, 'templates')], # <-- Puedes dejarlo así o quitarlo si solo usas templates de apps
            'APP_DIRS': True, # <-- Esto es lo que busca la carpeta 'templates' dentro de cada app
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

    # ...

    # Configuración de archivos estáticos (CSS, JS, Imágenes)
    STATIC_URL = '/static/'

    # Añade la ruta a la carpeta 'static' que creamos en la raíz del proyecto
    STATICFILES_DIRS = [
        os.path.join(BASE_DIR, 'static'),
    ]
    ```

2.  **Configurar `backend_aerolineas/urls.py`:**
    Este archivo es el enrutador principal. Debemos decirle que incluya las URLs de nuestra aplicación `gestion_vuelos`.

    ```python
    # backend_aerolineas/urls.py
    from django.contrib import admin
    from django.urls import path, include # <-- Importar 'include'

    urlpatterns = [
        path('admin/', admin.site.urls),
        path('', include('gestion_vuelos.urls')), # <-- Incluir las URLs de nuestra app
    ]
    ```

---

### **Paso 5: Modelos y Migraciones**

1.  **Definir los Modelos (`gestion_vuelos/models.py`):**
    Copia y pega el código que proporcionaste en este archivo. Define la estructura de las tablas de tu base de datos.

    ```python
    # gestion_vuelos/models.py
    from django.db import models

    class Aeropuerto(models.Model):
        codigo = models.CharField(primary_key=True, max_length=3, verbose_name="Código") # primary_key=True es mejor para códigos únicos
        ciudad = models.CharField(max_length=64, verbose_name="Ciudad")

        class Meta:
            verbose_name = "Aeropuerto"
            verbose_name_plural = "Aeropuertos"

        def __str__(self):
            return f"{self.ciudad} ({self.codigo})"

    class Vuelo(models.Model):
        origen = models.ForeignKey(Aeropuerto, on_delete=models.CASCADE, related_name="salidas", verbose_name="Origen")
        destino = models.ForeignKey(Aeropuerto, on_delete=models.CASCADE, related_name="llegadas", verbose_name="Destino")
        duracion = models.IntegerField(verbose_name="Duración (minutos)")

        class Meta:
            verbose_name = "Vuelo"
            verbose_name_plural = "Vuelos"

        def __str__(self):
            return f"{self.id}: {self.origen} a {self.destino}"

    class Pasajero(models.Model):
        nombre = models.CharField(max_length=64, verbose_name="Nombre")
        apellido = models.CharField(max_length=64, verbose_name="Apellido")
        vuelos = models.ManyToManyField(Vuelo, blank=True, related_name="pasajeros", verbose_name="Vuelos")

        class Meta:
            verbose_name = "Pasajero"
            verbose_name_plural = "Pasajeros"

        def __str__(self):
            return f"{self.nombre} {self.apellido}"
    ```
    *Nota: He añadido `primary_key=True` al `codigo` de `Aeropuerto` ya que parece ser un identificador único, lo que es una buena práctica.*

2.  **Realizar Migraciones:**
    Las migraciones son la forma en que Django aplica los cambios de tus modelos a la base de datos.

    ```bash
    # Crea los archivos de migración basados en los cambios de models.py
    python manage.py makemigrations

    # Aplica las migraciones a la base de datos
    python manage.py migrate
    ```

---

### **Paso 6: Lógica de la Aplicación (URLs y Vistas)**

1.  **Crear URLs de la Aplicación (`gestion_vuelos/urls.py`):**
    Crea este archivo y define todas las rutas para el CRUD de cada modelo.

    ```python
    # gestion_vuelos/urls.py
    from django.urls import path
    from . import views

    urlpatterns = [
        path('', views.inicio, name='inicio'),

        # Rutas para Aeropuertos
        path('aeropuertos/', views.listado_aeropuertos, name='listado_aeropuertos'),
        path('aeropuertos/crear/', views.crear_aeropuerto, name='crear_aeropuerto'),
        path('aeropuertos/editar/<str:codigo>/', views.editar_aeropuerto, name='editar_aeropuerto'),
        path('aeropuertos/eliminar/<str:codigo>/', views.eliminar_aeropuerto, name='eliminar_aeropuerto'),

        # Rutas para Vuelos
        path('vuelos/', views.listado_vuelos, name='listado_vuelos'),
        path('vuelos/crear/', views.crear_vuelo, name='crear_vuelo'),
        path('vuelos/editar/<int:id>/', views.editar_vuelo, name='editar_vuelo'),
        path('vuelos/eliminar/<int:id>/', views.eliminar_vuelo, name='eliminar_vuelo'),

        # Rutas para Pasajeros
        path('pasajeros/', views.listado_pasajeros, name='listado_pasajeros'),
        path('pasajeros/crear/', views.crear_pasajero, name='crear_pasajero'),
        path('pasajeros/editar/<int:id>/', views.editar_pasajero, name='editar_pasajero'),
        path('pasajeros/eliminar/<int:id>/', views.eliminar_pasajero, name='eliminar_pasajero'),
    ]
    ```

2.  **Crear las Vistas (`gestion_vuelos/views.py`):**
    Aquí va toda la lógica. Cada función corresponde a una URL y se encarga de procesar la solicitud del usuario, interactuar con la base de datos y renderizar una plantilla.

    ```python
    # gestion_vuelos/views.py
    from django.shortcuts import render, redirect, get_object_or_404
    from .models import Aeropuerto, Vuelo, Pasajero

    # Vista de Inicio
    def inicio(request):
        return render(request, 'gestion_vuelos/inicio.html')

    # Vistas para Aeropuertos
    def listado_aeropuertos(request):
        aeropuertos = Aeropuerto.objects.all()
        return render(request, 'gestion_vuelos/aeropuerto/listado_aeropuertos.html', {'aeropuertos': aeropuertos})

    def crear_aeropuerto(request):
        if request.method == "POST":
            codigo = request.POST['codigo'].upper()
            ciudad = request.POST['ciudad']
            Aeropuerto.objects.create(codigo=codigo, ciudad=ciudad)
            return redirect('listado_aeropuertos')
        return render(request, 'gestion_vuelos/aeropuerto/form_aeropuerto.html')

    def editar_aeropuerto(request, codigo):
        aeropuerto = get_object_or_404(Aeropuerto, pk=codigo)
        if request.method == "POST":
            aeropuerto.ciudad = request.POST['ciudad']
            aeropuerto.save()
            return redirect('listado_aeropuertos')
        return render(request, 'gestion_vuelos/aeropuerto/form_aeropuerto.html', {'aeropuerto': aeropuerto})

    def eliminar_aeropuerto(request, codigo):
        aeropuerto = get_object_or_404(Aeropuerto, pk=codigo)
        aeropuerto.delete()
        return redirect('listado_aeropuertos')


    # Vistas para Vuelos
    def listado_vuelos(request):
        vuelos = Vuelo.objects.all()
        return render(request, 'gestion_vuelos/vuelo/listado_vuelos.html', {'vuelos': vuelos})

    def crear_vuelo(request):
        aeropuertos = Aeropuerto.objects.all()
        if request.method == "POST":
            origen_id = request.POST['origen']
            destino_id = request.POST['destino']
            duracion = request.POST['duracion']
            origen = get_object_or_404(Aeropuerto, pk=origen_id)
            destino = get_object_or_404(Aeropuerto, pk=destino_id)
            Vuelo.objects.create(origen=origen, destino=destino, duracion=duracion)
            return redirect('listado_vuelos')
        return render(request, 'gestion_vuelos/vuelo/form_vuelo.html', {'aeropuertos': aeropuertos})

    def editar_vuelo(request, id):
        vuelo = get_object_or_404(Vuelo, pk=id)
        aeropuertos = Aeropuerto.objects.all()
        if request.method == "POST":
            vuelo.origen = get_object_or_404(Aeropuerto, pk=request.POST['origen'])
            vuelo.destino = get_object_or_404(Aeropuerto, pk=request.POST['destino'])
            vuelo.duracion = request.POST['duracion']
            vuelo.save()
            return redirect('listado_vuelos')
        return render(request, 'gestion_vuelos/vuelo/form_vuelo.html', {'vuelo': vuelo, 'aeropuertos': aeropuertos})

    def eliminar_vuelo(request, id):
        vuelo = get_object_or_404(Vuelo, pk=id)
        vuelo.delete()
        return redirect('listado_vuelos')


    # Vistas para Pasajeros
    def listado_pasajeros(request):
        pasajeros = Pasajero.objects.all()
        return render(request, 'gestion_vuelos/pasajero/listado_pasajeros.html', {'pasajeros': pasajeros})

    def crear_pasajero(request):
        vuelos = Vuelo.objects.all()
        if request.method == "POST":
            nombre = request.POST['nombre']
            apellido = request.POST['apellido']
            pasajero = Pasajero.objects.create(nombre=nombre, apellido=apellido)
            vuelos_ids = request.POST.getlist('vuelos') # getlist para campos ManyToMany
            pasajero.vuelos.set(vuelos_ids)
            return redirect('listado_pasajeros')
        return render(request, 'gestion_vuelos/pasajero/form_pasajero.html', {'vuelos_disponibles': vuelos})

    def editar_pasajero(request, id):
        pasajero = get_object_or_404(Pasajero, pk=id)
        vuelos_disponibles = Vuelo.objects.all()
        if request.method == "POST":
            pasajero.nombre = request.POST['nombre']
            pasajero.apellido = request.POST['apellido']
            vuelos_ids = request.POST.getlist('vuelos')
            pasajero.vuelos.set(vuelos_ids)
            pasajero.save()
            return redirect('listado_pasajeros')
        return render(request, 'gestion_vuelos/pasajero/form_pasajero.html', {'pasajero': pasajero, 'vuelos_disponibles': vuelos_disponibles})

    def eliminar_pasajero(request, id):
        pasajero = get_object_or_404(Pasajero, pk=id)
        pasajero.delete()
        return redirect('listado_pasajeros')

    ```

---

### **Paso 7: Plantillas HTML con Bootstrap**

Estos archivos HTML definen la interfaz que verá el usuario. Usaremos la sintaxis de plantillas de Django (`{% ... %}` y `{{ ... }}`) para mostrar datos dinámicos.

#### **7.1 `base.html`**
La plantilla principal que heredarán todas las demás.

```html
<!-- gestion_vuelos/templates/gestion_vuelos/base.html -->
{% load static %}
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{% block title %}Aerolínea{% endblock %}</title>
    <!-- CDN de Bootstrap 5 -->
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/css/bootstrap.min.css" rel="stylesheet">
    <style>
        /* Estilos adicionales para que se parezca más a las capturas */
        .navbar-brand {
            font-weight: bold;
        }
        .main-content {
            padding-top: 2rem;
        }
    </style>
</head>
<body>

    <!-- Barra de navegación -->
    <nav class="navbar navbar-expand-lg navbar-dark bg-primary">
        <div class="container-fluid">
            <a class="navbar-brand" href="{% url 'inicio' %}">Aerolínea</a>
            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
                <span class="navbar-toggler-icon"></span>
            </button>
            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav">
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'inicio' %}">Inicio</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'listado_aeropuertos' %}">Aeropuertos</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'listado_vuelos' %}">Vuelos</a>
                    </li>
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'listado_pasajeros' %}">Pasajeros</a>
                    </li>
                </ul>
            </div>
        </div>
    </nav>

    <!-- Contenido principal de la página -->
    <main class="container main-content">
        {% block content %}
        <!-- El contenido de cada página específica irá aquí -->
        {% endblock %}
    </main>

    <!-- CDN de JS de Bootstrap (Opcional, pero recomendado) -->
    <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.2/dist/js/bootstrap.bundle.min.js"></script>
</body>
</html>
```

#### **7.2 `inicio.html`**
La página de bienvenida.

```html
<!-- gestion_vuelos/templates/gestion_vuelos/inicio.html -->
{% extends 'gestion_vuelos/base.html' %}
{% load static %} {# Cargar archivos estáticos #}

{% block title %}Bienvenido al Sistema de Aerolíneas{% endblock %}

{% block content %}
<div class="text-center">
    <h1 class="display-4">Bienvenido al Sistema de Aerolíneas</h1>
    <p class="lead">Usa el menú de navegación para gestionar aeropuertos, vuelos y pasajeros.</p>
    
    {# Imagen de avión. Descarga un SVG similar y guárdalo en static/images/airplane.svg #}
    <img src="{% static 'images/airplane.svg' %}" alt="Ilustración de un avión" class="img-fluid my-4" style="max-width: 300px;">
</div>
{% endblock %}
```

*(Para la imagen, puedes buscar "airplane illustration svg" en sitios como [undraw.co](https://undraw.co) y guardar el archivo en `static/images/`)*

#### **7.3 Plantillas de Aeropuerto**

**`listado_aeropuertos.html`**
```html
<!-- gestion_vuelos/templates/gestion_vuelos/aeropuerto/listado_aeropuertos.html -->
{% extends 'gestion_vuelos/base.html' %}

{% block title %}Listado de Aeropuertos{% endblock %}

{% block content %}
    <div class="d-flex justify-content-between align-items-center mb-4">
        <h1>Listado de Aeropuertos</h1>
        <a href="{% url 'crear_aeropuerto' %}" class="btn btn-success">Agregar</a>
    </div>

    <table class="table table-striped table-bordered">
        <thead class="table-dark">
            <tr>
                <th>Código</th>
                <th>Ciudad</th>
                <th>Acciones</th>
            </tr>
        </thead>
        <tbody>
            {% for aeropuerto in aeropuertos %}
            <tr>
                <td>{{ aeropuerto.codigo }}</td>
                <td>{{ aeropuerto.ciudad }}</td>
                <td>
                    <a href="{% url 'editar_aeropuerto' aeropuerto.codigo %}" class="btn btn-warning btn-sm">Editar</a>
                    <!-- Usamos un formulario para eliminar para más seguridad, aunque un link también funciona -->
                    <form action="{% url 'eliminar_aeropuerto' aeropuerto.codigo %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que deseas eliminar este aeropuerto?');">
                        {% csrf_token %}
                        <button type="submit" class="btn btn-danger btn-sm">Eliminar</button>
                    </form>
                </td>
            </tr>
            {% empty %}
            <tr>
                <td colspan="3" class="text-center">No hay aeropuertos registrados.</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>
{% endblock %}
```

**`form_aeropuerto.html`**
```html
<!-- gestion_vuelos/templates/gestion_vuelos/aeropuerto/form_aeropuerto.html -->
{% extends 'gestion_vuelos/base.html' %}

{% block title %}{% if aeropuerto %}Editar Aeropuerto{% else %}Agregar Aeropuerto{% endif %}{% endblock %}

{% block content %}
    <h1>{% if aeropuerto %}Editar Aeropuerto{% else %}Agregar Aeropuerto{% endif %}</h1>
    <hr>
    <form method="post">
        {% csrf_token %} <!-- Token de seguridad de Django, obligatorio para POST -->
        
        <div class="mb-3">
            <label for="codigo" class="form-label">Código (3 letras)</label>
            <!-- El campo código es la clave primaria, por lo tanto, no se puede editar. Se deshabilita si estamos editando. -->
            <input type="text" class="form-control" id="codigo" name="codigo" value="{{ aeropuerto.codigo|default:'' }}" maxlength="3" required {% if aeropuerto %}readonly{% endif %}>
        </div>

        <div class="mb-3">
            <label for="ciudad" class="form-label">Ciudad</label>
            <input type="text" class="form-control" id="ciudad" name="ciudad" value="{{ aeropuerto.ciudad|default:'' }}" required>
        </div>

        <button type="submit" class="btn btn-primary">Guardar</button>
        <a href="{% url 'listado_aeropuertos' %}" class="btn btn-secondary">Cancelar</a>
    </form>
{% endblock %}
```
*(Repite una estructura similar para Vuelos y Pasajeros, adaptando los campos del formulario y la tabla)*

#### **7.4 Plantillas de Vuelo (Ejemplo resumido)**

**`listado_vuelos.html`**: Similar a `listado_aeropuertos.html`, con columnas para `ID`, `Origen`, `Destino`, `Duración` y `Acciones`.

**`form_vuelo.html`**:
```html
<!-- gestion_vuelos/templates/gestion_vuelos/vuelo/form_vuelo.html -->
{% extends 'gestion_vuelos/base.html' %}
{% block content %}
    <h1>{% if vuelo %}Editar Vuelo{% else %}Agregar Vuelo{% endif %}</h1>
    <hr>
    <form method="post">
        {% csrf_token %}
        <div class="mb-3">
            <label for="origen" class="form-label">Origen</label>
            <select name="origen" id="origen" class="form-select" required>
                {% for aero in aeropuertos %}
                    <option value="{{ aero.codigo }}" {% if vuelo.origen.codigo == aero.codigo %}selected{% endif %}>{{ aero }}</option>
                {% endfor %}
            </select>
        </div>
        <div class="mb-3">
            <label for="destino" class="form-label">Destino</label>
            <select name="destino" id="destino" class="form-select" required>
                 {% for aero in aeropuertos %}
                    <option value="{{ aero.codigo }}" {% if vuelo.destino.codigo == aero.codigo %}selected{% endif %}>{{ aero }}</option>
                {% endfor %}
            </select>
        </div>
        <div class="mb-3">
            <label for="duracion" class="form-label">Duración (minutos)</label>
            <input type="number" name="duracion" id="duracion" class="form-control" value="{{ vuelo.duracion|default:'' }}" required>
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
        <a href="{% url 'listado_vuelos' %}" class="btn btn-secondary">Cancelar</a>
    </form>
{% endblock %}
```

#### **7.5 Plantillas de Pasajero (Ejemplo resumido)**

**`listado_pasajeros.html`**: Similar a los otros listados, con columnas para `Nombre`, `Apellido`, `Vuelos` y `Acciones`.

**`form_pasajero.html`**: La parte clave es el selector múltiple para los vuelos.
```html
<!-- gestion_vuelos/templates/gestion_vuelos/pasajero/form_pasajero.html -->
{% extends 'gestion_vuelos/base.html' %}
{% block content %}
    <h1>{% if pasajero %}Editar Pasajero{% else %}Agregar Pasajero{% endif %}</h1>
    <hr>
    <form method="post">
        {% csrf_token %}
        <div class="mb-3">
            <label for="nombre" class="form-label">Nombre</label>
            <input type="text" name="nombre" class="form-control" value="{{ pasajero.nombre|default:'' }}" required>
        </div>
        <div class="mb-3">
            <label for="apellido" class="form-label">Apellido</label>
            <input type="text" name="apellido" class="form-control" value="{{ pasajero.apellido|default:'' }}" required>
        </div>
        <div class="mb-3">
            <label for="vuelos" class="form-label">Vuelos Asignados</label>
            <select name="vuelos" id="vuelos" class="form-select" multiple size="5">
                {# Iteramos sobre todos los vuelos disponibles #}
                {% for vuelo in vuelos_disponibles %}
                    {# Verificamos si este vuelo está en los vuelos ya asignados al pasajero #}
                    <option value="{{ vuelo.id }}" {% if vuelo in pasajero.vuelos.all %}selected{% endif %}>
                        {{ vuelo }}
                    </option>
                {% endfor %}
            </select>
            <small class="form-text text-muted">Mantén presionada la tecla Ctrl (o Cmd en Mac) para seleccionar varios vuelos.</small>
        </div>
        <button type="submit" class="btn btn-primary">Guardar</button>
        <a href="{% url 'listado_pasajeros' %}" class="btn btn-secondary">Cancelar</a>
    </form>
{% endblock %}
```

---

### **Paso 8: Ejecución del Servidor**

Una vez que todo está configurado, puedes ejecutar el servidor de desarrollo de Django.

```bash
python manage.py runserver
```

Abre tu navegador y ve a la dirección **http://127.0.0.1:8000/**. Deberías ver tu página de inicio y poder navegar por las distintas secciones para crear, ver, editar y eliminar registros.

---

### **Términos Clave**

*   **Django:** Un framework de alto nivel para desarrollo web en Python que promueve el desarrollo rápido y un diseño limpio y pragmático.
*   **Bootstrap:** Un popular framework de CSS para diseñar sitios web responsivos y modernos con componentes predefinidos.
*   **Entorno Virtual (venv):** Un ambiente de desarrollo aislado que permite gestionar las dependencias de un proyecto específico.
*   **ORM (Object-Relational Mapping):** Técnica que permite interactuar con una base de datos (como si fueran objetos de Python) en lugar de escribir consultas SQL directamente. Los modelos de Django son su ORM.
*   **Migraciones:** Un sistema para versionar los cambios en el esquema de la base de datos, sincronizándolos con los modelos de Django.
*   **CRUD:** Acrónimo de las operaciones básicas en bases de datos: **C**reate (Crear), **R**ead (Leer), **U**pdate (Actualizar), **D**elete (Eliminar).
*   **Plantillas (Templates):** Archivos HTML con sintaxis especial de Django que permiten mostrar datos dinámicos y reutilizar código.
*   **Vistas (Views):** Funciones de Python que reciben una solicitud web y devuelven una respuesta web. Contienen la lógica de la aplicación.
*   **URLs (Enrutamiento):** El sistema que dirige las solicitudes entrantes a la vista correcta basándose en la URL solicitada.

### **Tecnologías Utilizadas**

*   **Lenguaje de Programación:** Python 3.x
*   **Framework Backend:** Django
*   **Framework Frontend:** Bootstrap 5
*   **Base de Datos:** SQLite (la base de datos por defecto de Django, ideal para desarrollo)
*   **Lenguajes de Marcado/Estilo:** HTML5, CSS3

### **Conclusiones**

Has construido con éxito una aplicación web funcional para la gestión de una aerolínea. Este proyecto cubre los fundamentos del desarrollo web con Django, desde la configuración inicial hasta la implementación de operaciones CRUD completas y la creación de una interfaz de usuario limpia. Has aprendido a separar la lógica (vistas), los datos (modelos) y la presentación (plantillas), siguiendo el patrón de diseño MTV (Model-Template-View) de Django. Este es un excelente punto de partida para añadir funcionalidades más complejas como autenticación de usuarios, búsquedas avanzadas o una API REST.

### **Licencia**

Este proyecto y su código fuente se distribuyen bajo la **Licencia MIT**.

```
Copyright (c) 2023 [Tu Nombre]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
