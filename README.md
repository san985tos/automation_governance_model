# Fedora Demo Platform — Modelo de gobierno de automatización

Workshop comunitario de Open Industries para institucionalizar el gobierno de Ansible / AAP en siete dominios. Identidad Fedora con fedora azul de Heber Romero. Material en español.

## Contenido
- [Inicio y preparación](docs/00-inicio.md)
- [Modelo de gobierno](docs/02-modelo.md) y [RACI global](docs/03-roles-raci.md)
- [Git y releases](docs/04-git.md), [AAP y ciclo de vida](docs/05-ciclo-vida.md)
- [Ejercicio por dominios](docs/07-ejercicio.md) y [roadmap](docs/08-roadmap.md)
- [Agenda y facilitación](docs/10-facilitacion.md)
- [Secrets](docs/09-secrets.md) y [publicación](docs/11-publicacion.md)
- [Presentación navegable e imprimible](slides/index.html)
- Plantillas editables en `templates/`, contratos de configuración en `aap/`
- Siete playbooks ejecutables de laboratorio y fichas de integración en `examples/`

## Roles por etapa del ciclo de vida
Resumen de quién rinde cuentas (A) y quién ejecuta (R) en cada etapa. El detalle está en [RACI global](docs/03-roles-raci.md) y [AAP y ciclo de vida](docs/05-ciclo-vida.md).

```text
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ 1 DEFINICIÓN    │    │ 2 DISEÑO        │    │ 3 DESARROLLO    │    │ 4 VALIDACIÓN    │    │ 5 REVISIÓN      │
│ A: Dueño        │───>│ A: Dueño        │───>│ A: Dev          │───>│ A: Dev          │───>│ A: Seguridad    │
│ R: Dueño        │    │ R: Dev          │    │ R: Dev          │    │ R: Dev          │    │ R: Seguridad    │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
                                                                                                     │
                                                                                                     v
┌─────────────────┐    ╔═════════════════╗    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ 9 SOLICITUD     │    ║ AUTORIZAR PROD. ║    │ 8 PRUEBAS       │    │ 7 AAP           │    │ 6 RELEASE       │
│ A: Solicitante  │<───║ A: Dueño        ║<───│ A: Dueño        │<───│ A: AAP Admin    │<───│ A: SCM          │
│ R: Ops          │    ║ R: Dueño        ║    │ R: Dev + Dom.   │    │ R: AAP Admin    │    │ R: Dev          │
└─────────────────┘    ╚═════════════════╝    └─────────────────┘    └─────────────────┘    └─────────────────┘
         │
         v
┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│ 10 EJECUCIÓN    │    │ 11 ENTREGA      │    │ 12 ACEPTACIÓN   │    │ 13 OPERACIÓN    │
│ A: Dueño        │───>│ A: Dueño        │───>│ A: Solicitante  │───>│ A: Dueño        │
│ R: Ops          │    │ R: Ops          │    │ R: Ops          │    │ R: Ops+AAP Adm* │
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

| Abreviatura | Significado |
|---|---|
| **A** / **R** | Accountable (rinde cuentas) / Responsible (ejecuta) |
| **Dueño** | Dueño del caso de uso |
| **Dev** | Desarrollo de playbooks |
| **Seguridad** | Seguridad y Compliance |
| **SCM** | Git y SCM |
| **Dom.** | Dominio técnico |
| **Ops** | Operaciones y SRE |
| **Solicitante** | Solicitante o consumidor |

- El recuadro de doble línea (**AUTORIZAR PROD.**) aparece solo en la matriz RACI, no como etapa del mapa de ciclo de vida; se ubica entre Pruebas y Solicitud.
- `*` En la etapa 13, AAP Admin es R solo en el retiro. En monitoreo y mejora, el R es solo Ops.

## Laboratorio
```bash
python3.11 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-dev.txt
ansible-playbook -i inventories/lab/hosts.yml playbooks/virtualizacion.yml
ansible-playbook -i inventories/lab/hosts.yml playbooks/virtualizacion.yml
```
La segunda ejecución debe tener `changed=0`. Los ejemplos representan recursos en archivos locales, no provisionan infraestructura real. AAP/AWX es opcional para el ejercicio local y necesario para validar el recorrido en controller.

## Portal
```bash
python scripts/build_site.py
python scripts/validate_content.py
python -m http.server 8000 --directory public
```
Abrir localhost:8000. Para publicar, el workflow Antora usa el tema Showroom con marca Fedora. El administrador debe habilitar Pages como GitHub Actions. La URL sólo queda activa después de un deployment exitoso. Consultar `docs/11-publicacion.md`.

## Verificación
```bash
yamllint .
ansible-lint playbooks roles molecule
molecule test
python scripts/validate_raci.py templates/raci-editable.csv
```
CI valida contenido, lint, Molecule y secrets. Las reglas de protección y equipos de CODEOWNERS se configuran en GitHub; no quedan activados por este README.

## Alcance
Agenda compacta de ocho horas con preparación previa y versión ampliada de 9 h 15 min–11 h 15 min más pausas. Fedora identifica la marca de este workshop comunitario. Los nombres técnicos de AAP y los enlaces a documentación oficial se conservan.
