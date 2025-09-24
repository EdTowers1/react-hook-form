# reactform

Proyecto de ejemplo: React + TypeScript + Vite que demuestra el uso de React Hook Form junto con Zod para validación.

Contenido principal
- Formularios controlados con React Hook Form
- Validación y esquemas con Zod
- Ejemplos de componentes reutilizables (CustomForm)

Requisitos
- Node.js 18+ (o Bun si prefieres)
- npm, pnpm o Bun como gestor de paquetes

Instalación

1. Clona el repositorio (si no lo has hecho):

```powershell
git clone <tu-repo-url>
cd reactform
```

2. Instala dependencias (elige el comando según tu gestor):

```powershell
# con npm
npm install

# con bun
bun install

# con pnpm
pnpm install
```

Scripts disponibles
- `dev`: inicia Vite en modo desarrollo (HMR)
- `build`: compila TypeScript y construye la app con Vite
- `lint`: ejecuta ESLint
- `preview`: sirve la build de producción localmente

Ejecutar scripts

```powershell
# desarrollo
npm run dev

# build
npm run build

# vista previa de la build
npm run preview

# lint
npm run lint
```

Dependencias principales
- react, react-dom
- vite
- typescript
- react-hook-form
- @hookform/resolvers
- zod

Uso rápido

Abre `src/components/CustomForm/customForm.tsx` para ver un formulario usando `react-hook-form` y `zod`.
El componente exporta `CustomForm` como default y se consume desde `src/components/CustomForm/index.ts`.

Problemas comunes vistos en este proyecto y soluciones

1) Error por diferencias solo en mayúsculas/minúsculas (Windows)

Mensaje: "File name '.../CustomForm/CustomForm.tsx' differs from already included file name '.../CustomForm/customForm.tsx' only in casing."

Por qué ocurre: en Windows el sistema de archivos no distingue entre mayúsculas y minúsculas. Si en el proyecto existen dos rutas o importaciones que solo difieren por el case (por ejemplo `CustomForm.tsx` vs `customForm.tsx`), TypeScript/IDE las puede listar ambas y provocar conflictos.

Cómo solucionarlo:
- Mantén un único archivo con un nombre consistente. Por ejemplo, usa `CustomForm.tsx` y elimina cualquier duplicado con distinto casing.
- Asegúrate de que todas las importaciones usen exactamente el mismo nombre de archivo:

```ts
// correcto
import CustomForm from './CustomForm';

// evitar
import CustomForm from './customForm';
```

Si ya limitaste las importaciones pero sigues viendo el error, reinicia el servidor de desarrollo y limpia la cache de dependencias:

```powershell
# eliminar node_modules y reinstalar (Windows PowerShell)
Remove-Item -Recurse -Force node_modules; npm install
# o con bun
bun install --force
```

2) Mensaje de Zod: "Invalid input: expected string, received undefined"

Por qué ocurre: Zod produce ese mensaje cuando el valor que valida es `undefined` (no existe la propiedad). Aunque configures mensajes personalizados en tus validaciones (por ejemplo `.min(1, 'El nombre es obligatorio')`), Zod mostrará el mensaje por defecto si el tipo no coincide (p. ej. en vez de recibir un string recibe undefined).

Solución práctica: proporciona `defaultValues` en `useForm` para asegurarte de que todos los campos existan y sean del tipo esperado:

```tsx
const { control, handleSubmit, formState: { errors } } = useForm<FormValues>({
  resolver: zodResolver(schema),
  defaultValues: {
    name: '',
    email: '',
    password: '',
    confirmPassword: ''
  }
});
```

Con `defaultValues` te asegurarás de que Zod reciba strings vacíos cuando el usuario no haya escrito nada y, por tanto, tus mensajes personalizados (por ejemplo, `min(1, 'El nombre es obligatorio')`) se aplicarán correctamente.

Consejos y buenas prácticas
- Mantén nombres de archivos e importaciones consistentes (camelCase o PascalCase según componente).
- Usa `defaultValues` en `useForm` para evitar mensajes por `undefined`.
- Añade tests o snapshots para los componentes críticos del formulario.

Contribuir

Si quieres contribuir:

1. Haz fork del repositorio
2. Crea una rama: `git checkout -b feature/mi-cambio`
3. Haz tus cambios y añade tests si aplica
4. Envía un pull request describiendo los cambios

Licencia

Este proyecto es una plantilla de ejemplo. Añade la licencia que prefieras (MIT, Apache-2.0, etc.) en un archivo `LICENSE` si vas a publicarlo.

Contacto

Para preguntas o dudas abre un issue en el repositorio.

