# Skill Backend: Buenas Prácticas y Arquitectura

## 1. Buenas Prácticas de Desarrollo (Guía Básica Python/Django)
El backend del proyecto Mentora se rige por los estándares oficiales de Python (PEP 8) y las mejores prácticas de la comunidad de Django:
- **Nomenclatura (Naming Conventions):** Las clases usan `CamelCase` (ej. `AuthService`), las variables, módulos y funciones usan `snake_case` (ej. `authenticate_user`). Se utiliza idioma **inglés** para el código (*Clean Code*).
- **Manejo de Errores y Excepciones:** Se evita el uso de `try-except` genéricos. Las excepciones se atrapan de manera explícita (ej. `Usuario.DoesNotExist`, `jwt.ExpiredSignatureError`).
- **Linter y Formateo:** Todo el código pasa por `flake8` para garantizar métricas de calidad de código y un máximo de 120 caracteres por línea.
- **Testing Aislado:** Las pruebas unitarias se enfocan en la lógica de negocio (usando `pytest`) implementando `mocks` para **NO** escribir ni leer directamente de la base de datos, garantizando rapidez y determinismo.

## 2. Definición y Aplicación de la Arquitectura
El proyecto utiliza una **Arquitectura Monolítica** estructurada bajo el patrón MVC (Model-View-Controller), que en Django se conoce como MTV (Model-Template-View, donde las "Templates" son reemplazadas por Serializadores JSON para actuar como API REST).

### Dependencias y Estructura
- **Capa de Datos (Models):** Administrada por el ORM de Django (`models.py`). Las tablas tienen un `db_table` definido y llaves foráneas (`ForeignKey`) optimizadas con `on_delete=CASCADE` o `SET_NULL`.
- **Capa de Lógica de Negocio (Services):** Desacoplada de las vistas. Se utilizan archivos `services.py` (ej. `AuthService`) para mantener las vistas (Views) "limpias" y "delgadas" (*Thin Views, Fat Models/Services*).
- **Capa de Control y Presentación (Views/Serializers):** Se hace uso de Django Rest Framework (DRF) empleando `ModelViewSet` para generar automáticamente los controladores CRUD y `ListAPIView` para endpoints personalizados (ej. `StudentHistoryView`).

## 3. Revisión de Reglas: MUST y SHOULD Have

### MUST HAVE (Requisitos Críticos y Obligatorios)
- [x] **MUST** usar variables de entorno para datos sensibles (`SECRET_KEY`, credenciales de DB).
- [x] **MUST** implementar validación de roles en cada endpoint (`permission_classes = [IsTutor | IsAdmin]`).
- [x] **MUST** escribir pruebas unitarias usando *Mocks* para los accesos a la base de datos (0% interacción con BD en tests unitarios).
- [x] **MUST** respetar la sintaxis de Python (PEP-8) sin errores de sintaxis (bloqueantes para el build).

### SHOULD HAVE (Recomendaciones Fuertes)
- [x] **SHOULD** mantener las vistas ligeras, delegando lógica compleja a la capa de Servicios (`services.py`).
- [x] **SHOULD** incluir *Docstrings* en clases y métodos públicos para explicar su comportamiento (especialmente útil en endpoints personalizados).
- [x] **SHOULD** ejecutar el analizador estático (`flake8`) antes de enviar un Pull Request al repositorio.
