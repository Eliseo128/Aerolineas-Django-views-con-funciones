¡Por supuesto! Aquí tienes los códigos completos y comentados para `listado_vuelos.html` y `listado_pasajeros.html`, diseñados para replicar exactamente las imágenes que proporcionaste, utilizando Bootstrap 5 y la sintaxis de plantillas de Django.

---

### Código para `listado_vuelos.html`

Este archivo muestra la tabla con todos los vuelos registrados. Se encarga de iterar sobre la lista de vuelos que le pasa la vista y mostrar cada uno en una fila, junto con los botones de acción.

**Ruta del archivo:** `gestion_vuelos/templates/gestion_vuelos/vuelo/listado_vuelos.html`

```html
{% extends 'gestion_vuelos/base.html' %} {# Hereda la estructura base (navegación, footer, etc.) #}

{% block title %}Listado de Vuelos{% endblock %} {# Título que aparecerá en la pestaña del navegador #}

{% block content %} {# Inicio del bloque de contenido específico para esta página #}

    <!-- Encabezado de la página con el título y el botón de agregar -->
    <div class="d-flex justify-content-between align-items-center mb-4">
        <h1>Listado de Vuelos</h1>
        <a href="{% url 'crear_vuelo' %}" class="btn btn-success">Agregar Vuelo</a>
    </div>

    <!-- Tabla para mostrar los datos de los vuelos -->
    <table class="table table-striped table-bordered">
        <!-- Cabecera de la tabla con estilo oscuro -->
        <thead class="table-dark">
            <tr>
                <th scope="col">ID</th>
                <th scope="col">Origen</th>
                <th scope="col">Destino</th>
                <th scope="col">Duración</th>
                <th scope="col" style="width: 15%;">Acciones</th> {# Damos un ancho fijo a la columna de acciones #}
            </tr>
        </thead>
        <tbody>
            {# Bucle para recorrer cada objeto 'vuelo' en la lista 'vuelos' que viene de la vista #}
            {% for vuelo in vuelos %}
            <tr>
                <!-- Mostramos los datos de cada vuelo en las celdas de la tabla -->
                <td>{{ vuelo.id }}</td>
                <td>{{ vuelo.origen }}</td>  {# Django usará el método __str__ del modelo Aeropuerto #}
                <td>{{ vuelo.destino }}</td> {# Ídem para el destino #}
                <td>{{ vuelo.duracion }} min</td> {# Añadimos 'min' para mayor claridad #}
                <td>
                    <!-- Botón para editar, que lleva a la URL de edición con el ID del vuelo -->
                    <a href="{% url 'editar_vuelo' vuelo.id %}" class="btn btn-warning btn-sm">Editar</a>
                    
                    <!-- Formulario para el botón de eliminar. Es más seguro usar POST para acciones destructivas -->
                    <form action="{% url 'eliminar_vuelo' vuelo.id %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que deseas eliminar este vuelo?');">
                        {% csrf_token %} {# Token de seguridad de Django #}
                        <button type="submit" class="btn btn-danger btn-sm">Eliminar</button>
                    </form>
                </td>
            </tr>
            {# Bloque 'empty' que se muestra si la lista 'vuelos' está vacía #}
            {% empty %}
            <tr>
                <td colspan="5" class="text-center">No hay vuelos registrados.</td>
            </tr>
            {% endfor %} {# Fin del bucle #}
        </tbody>
    </table>

{% endblock %} {# Fin del bloque de contenido #}
```

---

### Código para `listado_pasajeros.html`

Este archivo es un poco más complejo porque debe mostrar una lista de vuelos para cada pasajero. Para ello, utilizamos un bucle anidado dentro de la columna "Vuelos".

**Ruta del archivo:** `gestion_vuelos/templates/gestion_vuelos/pasajero/listado_pasajeros.html`

```html
{% extends 'gestion_vuelos/base.html' %} {# Hereda la estructura base #}

{% block title %}Listado de Pasajeros{% endblock %}

{% block content %}

    <!-- Encabezado de la página -->
    <div class="d-flex justify-content-between align-items-center mb-4">
        <h1>Listado de Pasajeros</h1>
        <a href="{% url 'crear_pasajero' %}" class="btn btn-success">Agregar Pasajero</a>
    </div>

    <!-- Tabla para mostrar los datos de los pasajeros -->
    <table class="table table-striped table-bordered">
        <thead class="table-dark">
            <tr>
                <th scope="col">Nombre</th>
                <th scope="col">Apellido</th>
                <th scope="col">Vuelos</th> {# Columna para los vuelos asociados #}
                <th scope="col" style="width: 15%;">Acciones</th>
            </tr>
        </thead>
        <tbody>
            {# Bucle principal para recorrer cada pasajero #}
            {% for pasajero in pasajeros %}
            <tr>
                <td>{{ pasajero.nombre }}</td>
                <td>{{ pasajero.apellido }}</td>
                <td>
                    {# Comprobamos si el pasajero tiene vuelos asociados #}
                    {% if pasajero.vuelos.all %}
                        {# Bucle anidado para recorrer cada vuelo del pasajero #}
                        {% for vuelo in pasajero.vuelos.all %}
                            <!-- Mostramos cada vuelo en una línea separada -->
                            <div>{{ vuelo }}</div> {# Django usará el método __str__ del modelo Vuelo #}
                        {% endfor %}
                    {% else %}
                        <span class="text-muted">Sin vuelos asignados</span> {# Mensaje si no tiene vuelos #}
                    {% endif %}
                </td>
                <td>
                    <!-- Botones de acción para editar y eliminar el pasajero -->
                    <a href="{% url 'editar_pasajero' pasajero.id %}" class="btn btn-warning btn-sm">Editar</a>
                    
                    <form action="{% url 'eliminar_pasajero' pasajero.id %}" method="post" class="d-inline" onsubmit="return confirm('¿Estás seguro de que deseas eliminar a este pasajero?');">
                        {% csrf_token %}
                        <button type="submit" class="btn btn-danger btn-sm">Eliminar</button>
                    </form>
                </td>
            </tr>
            {# Mensaje si no hay ningún pasajero en la base de datos #}
            {% empty %}
            <tr>
                <td colspan="4" class="text-center">No hay pasajeros registrados.</td>
            </tr>
            {% endfor %}
        </tbody>
    </table>

{% endblock %}
```
