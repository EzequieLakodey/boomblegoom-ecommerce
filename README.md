# 🛒 BoombleGoom Ecommerce

Ecommerce personal de **BoombleGoom**: un marketplace donde las personas pueden
comprar y vender productos físicos, servicios y productos digitales por internet.
Este es el frente de usuario (frontend) del proyecto, construido con **React 18**,
**Vite**, **Material-UI (MUI)** y **Firebase** como backend.

> Desarrollado como proyecto universitario / portfolio personal. El creador es
> Ezequiel (ella.webdeveloper@gmail.com), developer full stack en formación.

---

## ✨ ¿Qué hace?

- **Catálogo de productos**: listado en grid con cards, filtrado por categoría y
  búsqueda. Cada producto tiene ficha de detalle con imágenes, precio, descripción
  y botón de agregar al carrito.
- **Carrito de compras**: persistente en `localStorage`, con capacidad de
  incrementar / decrementar cantidades y botón de checkout.
- **Formulario de pedido** (`OrderForm`): captura de datos del cliente (nombre,
  email, dirección) usando **Formik** con validación **Yup** (campos obligatorios,
  formato de email, longitud de texto).
- **Autenticación**: login y registro con **Auth0** (`@auth0/auth0-react`). El
  usuario logueado ve opciones de cierre de sesión y sus datos de perfil.
- **Panel de usuario** (`UserData`): muestra la información almacenada y permite
  actualizar los datos personales.
- **Navegación lateral** (`Navigator`): acceso rápido a Inicio, Catálogo, Carrito,
  Pedidos y Perfil, con rutas protegidas.
- **Estado global**: carrito y usuario gestionados mediante **React Context** (Cart
  Context + TanStack Query para fetching de productos).
- **Consultas optimizadas**: **TanStack React Query** (`useQuery`, `useMutation`)
  para obtener productos de Firestore con paginación y filtrado por categoría.

---

## 🧱 Stack técnico

| Tecnología | Uso |
|---|---|
| **React 18** | Librería principal de UI (funciones + hooks) |
| **Vite** | Bundler y servidor de desarrollo (HMR rápido) |
| **Material-UI (MUI) v5** | Componentes de interfaz (buttons, grids, dialogs, etc.) |
| **React Router DOM v6** | Rutas protegidas y navegación SPA |
| **TanStack React Query v4** | Fetch y caché de productos en Firestore |
| **Formik v2** + **Yup v1** | Formularios y validación en OrderForm |
| **Auth0** | Autenticación de usuarios (login/registro/logout) |
| **Firebase Firestore** | Base de datos NoSQL para productos y pedidos |
| **localStorage** | Persistencia del carrito de compras en el cliente |
| **SCSS** | Estilo modular (variables, anidación) en `src/scss/` |
| **ESLint** | Configurado con `eslint-plugin-react` y `eslint-plugin-react-hooks` |

---

## 📁 Estructura

```
boomblegoom-ecommerce/
├── .gitignore                  # Ignora node_modules/, .env/, dist/
├── index.html                  # Punto de entrada HTML
├── package.json                # Dependencias y scripts
├── vite.config.js              # Config de Vite (React plugin, aliases)
├── .eslintrc.cjs               # Linting config
├── README.md                   # Este archivo
└── src/
    ├── main.jsx               # Entry point: Router + Context + Provider
    ├── App.jsx                # Layout: Router + Routes + CartProvider
    ├── App.scss               # Import principal SCSS
    ├── main.scss              # Variables y mixins globales
    ├── pages/
    │   ├── Home/Home.jsx      # Página principal con catálogo
    │   ├── ItemDetail/ItemDetail.jsx  # Ficha de producto
    │   ├── Cart/Cart.jsx      # Carrito de compras
    │   ├── OrderForm/OrderForm.jsx  # Formulario de pedido
    │   └── User/UserData.jsx      # Panel de usuario
    ├── components/
    │   ├── Navigator/Navigator.jsx  # Menú lateral y rutas
    │   └── ... (otros componentes puros)
    ├── context/
    │   ├── Cart/CartContext.jsx       # Contexto del carrito (localStorage)
    │   └── ... (otros contextos)
    ├── pages/ItemsCategories/ItemsCateogries.jsx  # Catálogo por categoría
    ├── scss/                    # Archivos SCSS parciales
    ├── utils/                 # Funciones auxiliares (si las hay)
    └── firebase/              # Configuración inicial de Firebase (api keys)
```

---

## 🚀 Clonar y preparar

Requisitos: **Node.js 18+** y **npm** (o yarn/pnpm).

```bash
# 1. Clonar
git clone https://github.com/EzequieLakodey/boomblegoom-ecommerce.git
cd boomblegoom-ecommerce

# 2. Instalar dependencias
npm install

# 3. Configurar Firebase
# --------------------
# Crear una cuenta en https://firebase.google.com y un proyecto nuevo.
# En la consola de Firebase: Authentication → Email/Password y Firestore Database.
# Copiar la configuración de firebaseConfig y pegarla en src/firebase/init.jsx
# (o el archivo que corresponda; este proyecto usa las credenciales en el
# código cliente, propio de la modalidad educativa/copia local).

# 4. Poner en marcha
npm run dev          # Vite: http://localhost:5173
npm run build        # Vite build a ./dist
npm run preview      # Servir build en localhost:4173
```

### Configuración de Firebase (pasos detallados)

1. Íntevate a la [Firebase Console](https://console.firebase.google.com/).
2. Crear un nuevo proyecto (nombre: `boomblegoom` o el que prefieras).
3. En **Authentication**, agregar proveedor **Email/Password**.
4. En **Firestore Database**, crear base de datos en modo "Modo prueba" y
   región elegida.
5. Ir a **Configuración del proyecto** → **SDK web** y copiar el objeto
   `firebaseConfig` que incluye `apiKey`, `authDomain`, `projectId`, `storageBucket`,
   `messagingSenderId`, `appId`.
6. Pegar ese objeto en el archivo `src/firebase/init.jsx` (o crear si no existe),
   quedando algo como:

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { getAuth } from "firebase/auth";

const firebaseConfig = {
  apiKey: "TU_API_KEY_AQUÍ",
  authDomain: "TU_PROYECTO.firebaseapp.com",
  projectId: "TU_PROYECTO",
  storageBucket: "TU_PROYECTO.appspot.com",
  messagingSenderId: "TU_MESSAGING_SENDER_ID",
  appId: "TU_APP_ID_AQUÍ"
};

const app = initializeApp(firebaseConfig);
export const db = getFirestore(app);
export const auth = getAuth(app);
```

> Nota: Este proyecto almacena las credenciales de Firebase en el frontend
> (propia de la modalidad educativa). En producción se recomienda usar las
> funciones server-side de Firebase o un backend propio para proteger las keys.

---

## 🔌 Rutas (React Router DOM v6)

| Ruta | Componente | Descripción |
|---|---|---|
| `/` | `Home` | Catálogo principal, filtrado y búsqueda |
| `/page/:pageNum` | `Home` | Paginación opcional (número de página) |
| `/category/:categoryId` | `Home` | Filtrar productos por categoría |
| `/cart` | `Cart` | Ver y modificar carrito |
| `/order` | `OrderForm` | Formulario de datos del cliente |
| `/user` | `UserData` | Datos y perfil del usuario logueado |

---

## 🔌 API / Puntos de integración

- **Firestore colecciones**:
  - `productos`: documentos con `id`, `nombre`, `categoria`, `precio`, `imagen`,
    `descripción`, `stock`.
  - `pedidos`: documentos auto-generados con `id`, `usuario`, `productos[]`,
    `total`, `fecha`, `estado`.
- **Auth0** gestiona el login/logout; el contexto de usuario está disponible en
  todo el árbol mediante `useAuth0()`.

---

## 🗺️ Roadmap / ideas futuras

- [ ] Migrar Auth0 a Firebase Auth nativo o a un backend propio con JWT.
- [ ] Implementar carrito persistente en Firestore en lugar de `localStorage`.
- [ ] Agregar pasarela de pago (Stripe / PayPal) en el flujo de `OrderForm`.
- [ ] Añadir carrusel de imágenes en `ItemDetail` con zoom.
- [ ] Hacer el diseño 100% responsive para móvil (ya es responsive pero probar
  en dispositivos reales).
- [ ] Añadir tests unitarios con React Testing Library.
- [ ] Migrar a React 19 / Vite 5 y aprovechar las nuevas features.

---

## 📄 Licencia

Sin licencia definida todavía. Uso educativo / portfolio personal.