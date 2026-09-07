# Dev_Ops
## Estrategia de Ramificación y Despliegue

Para este microservicio hemos seleccionado **Trunk-Based Development (TBD)** en combinación con **GitHub Actions** para gestionar nuestro ciclo de vida de desarrollo y automatización de despliegues.

### Justificación de la Elección
* Despliegue Continuo Eficiente (CD): Al ser un microservicio, la velocidad de iteración es clave. GitHub Actions reacciona instantáneamente a cada evento en la rama principal, eliminando la necesidad de ramas intermedias (como 'develop' o "release") y acelerando el proceso de actualizacion.
* Feedback Temprano (CI): Cada *Pull Request* activa pruebas automatizadas de manera inmediata. Esto garantiza que el código defectuoso se detecte antes de llegar a la rama principal.
* Reducción de problemas de fusion: Las ramas de características cortas evitan conflictos de código masivos y complejos, manteniendo el repositorio limpio y fácil de mantener.

---

### 🔄 Flujo de Trabajo (Workflow)

El ciclo de desarrollo y despliegue sigue un camino lineal y automatizado:

```mermaid
graph TD
    A-Nueva Característica --> (Crear Feature Branch Corta)
    B --> C(Abrir Pull Request a main)
    C --> D[GitHub Actions: CI / Ejecutar Tests y Linter]
    D -->|Aprobado & Merge| E(Rama main)
    E --> F[GitHub Actions: CD y Compilar y Desplegar Microservicio]
    F --> G[ Entorno de Producción ]
```

1. **Desarrollo:** El desarrollador crea una rama de vida corta ("feature/nombre-tarea") desde "main".
2. **Integración Continua (CI):** Al abrir un *Pull Request* hacia "main", **GitHub Actions** ejecuta automáticamente la suite de pruebas y validaciones de código (Linter).
3. **Revisión:** El equipo revisa el código y, tras la aprobación, se realiza el Merge.
4. **Despliegue Continuo (CD):** Al fusionarse el código en "main", un segundo flujo de GitHub Actions compila la aplicación, genera los artefactos (o contenedores) y realiza el *deploy* automático del microservicio.

---

###  Reglas de Protección de la Rama "main"
Para asegurar la estabilidad del microservicio, la rama "main" cuenta con las siguientes restricciones en GitHub:
* **Prohibido el Push Directo:** Todos los cambios deben ingresar exclusivamente mediante *Pull Request*.
* **Revisiones Obligatorias:** Se requiere la aprobación de al menos un par antes de fusionar.
* **Pasar los Checks de CI:** No se permite el *Merge* si el flujo de pruebas automatizadas de GitHub Actions falla.
