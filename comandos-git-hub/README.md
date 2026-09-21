## Guía Definitiva de Git y GitHub

Esta es una referencia de los comandos esenciales para el flujo de trabajo en Git y GitHub.

## ⚙️ 1. Configuración Inicial (Una sola vez)

Vincular tu identidad local:
```bash
git config --global user.name "Tu Nombre"
git config --global user.email "tu@email.com"
```

Generar y configurar llave SSH (Seguridad recomendada):
```bash
ssh-keygen -t ed25519 -C "tu@email.com"
cat ~/.ssh/id_ed25519.pub # Copiar el resultado y pegar en GitHub
```

## 2. Iniciar un Proyecto Nuevo (De Local a GitHub)

Para inicializar una carpeta existente y subirla a un repositorio vacío:
```bash
git init
git add .
git commit -m "chore: inicializa proyecto base"
git branch -M main
git remote add origin git@github.com:tu-usuario/tu-repositorio.git
git push -u origin main
```

## 3. Flujo de Trabajo Diario

Ciclo estándar cada vez que modificas o creas archivos:
```bash
git pull origin main    # 1. Trae los últimos cambios del equipo
git status              # 2. Muestra qué archivos están modificados (U = Untracked, M = Modified)
git add .               # 3. Prepara todos los cambios (Pasan a color verde / estado 'A')
git commit -m "feat: descripción de los cambios"  # 4. Guarda localmente con un mensaje
git push                # 5. Sube los cambios a la nube
```

### Convenciones de Commits (Buenas Prácticas)
* `feat:` Nueva característica o funcionalidad.
* `fix:` Solución de un error o bug.
* `docs:` Cambios en la documentación (ej. este README).
* `refactor:` Mejoras de código sin cambiar la lógica funcional.

## 4. Trabajo con Ramas (Branches)

Para desarrollar características nuevas sin afectar el código estable (`main`):
```bash
git checkout -b feature/nombre-rama    # Crea una rama nueva y se cambia a ella
git branch                             # Lista todas las ramas locales
git checkout main                      # Vuelve a la rama principal
git push -u origin feature/nombre-rama # Sube la rama nueva a GitHub por primera vez
```

## 5. Unir Cambios (Merge Local)

Para integrar los cambios de una rama secundaria terminada a la rama principal:
```bash
git checkout main
git merge feature/nombre-rama
git push
```

## 6. Control de Daños

Comandos de emergencia si te equivocas:
```bash
git stash                 # Guarda cambios temporales sin hacer commit (baúl)
git stash pop             # Saca los cambios del baúl y los aplica
git reset --soft HEAD~1   # Deshace el último commit local sin borrar tus modificaciones
git revert <id_commit>    # Crea un commit que deshace otro que ya está en GitHub
git reflog                # Muestra el historial absoluto de movimientos para recuperar código
```