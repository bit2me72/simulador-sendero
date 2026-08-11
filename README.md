# Sendero Residencial Abasolo · Simulador de crédito

Versión web del archivo `Sendero_Simulador_Credito_13_julio_2026.xlsx`. Es un solo archivo
(`index.html`) sin dependencias ni servidor: toda la lógica del Excel está reescrita en
JavaScript y se recalcula al momento en que el cliente cambia cualquier dato.

## Las tres hojas, ahora tres pestañas

| Hoja de Excel  | Pestaña      | Qué hace |
|---|---|---|
| `Cotizador`    | Cotizador    | Captura, condiciones calculadas, pago mensual y gastos iniciales. |
| `Amortizacion` | Amortización | Tabla mes a mes con el ajuste del último pago, totales y gráfica. |
| `Comparativo`  | Comparativo  | Tres escenarios independientes para comparar niveles de enganche. |

## Dónde se cambia la política comercial

Todo vive en el bloque `CONFIG`, al inicio del `<script>` de `index.html`. Es lo único
que hay que tocar si cambian las condiciones. Los porcentajes van como fracción:

```js
const CONFIG = {
  casa:    { tasaAnual: 0.10, engancheMinimo: 0.15 },
  terreno: { tasaAnual: 0.12, engancheMinimo: 0.25 },
  plazoMaximoMeses: 24,
  ...
};
```

Estos parámetros ya no aparecen en la página. Para revisarlos o probar variantes sin
tocar el código, abre la misma liga agregando `?interno=1` al final:

```
https://TU-USUARIO.github.io/simulador-sendero/?interno=1
```

Eso muestra un panel oscuro con las tasas, los enganches mínimos y el plazo máximo. Los
cambios solo viven en ese navegador y en esa visita: no alteran lo que ven los clientes ni
quedan guardados. Es una comodidad para el equipo de ventas, no un control de acceso: quien
conozca la liga puede abrirla. Para cambios permanentes, edita `CONFIG`.

## Candados de la política

El simulador no permite capturar condiciones fuera de política, no solo advierte:

- **Enganche.** El deslizador arranca en el mínimo del producto (15% casa, 25% terreno) y
  la casilla numérica se ajusta sola si escriben menos. Al cambiar de casa a terreno, un
  enganche de 15% brinca automáticamente a 25% y se avisa por qué. Hacia arriba no hay tope
  de política; el deslizador llega a 90%.
- **Monto del crédito.** En el modo de captura por monto, el crédito se limita al máximo
  financiable (85% del valor en casa, 75% en terreno). Si bajan el valor del inmueble, el
  monto se reajusta solo al nuevo tope.
- **Plazo.** Entre 1 y 24 meses; cualquier otra cifra se corrige al salir del campo.

El aviso de validación se conserva como red de seguridad y como confirmación en positivo
("Cumple con la política: enganche de 20%, por arriba del mínimo de 15%").

## Publicar en GitHub Pages

1. Crea un repositorio público, por ejemplo `simulador-sendero`.
2. Sube `index.html` a la raíz del repositorio.
3. **Settings → Pages**. En *Source* elige **Deploy from a branch**, rama `main`,
   carpeta `/ (root)`. Guarda.
4. En un par de minutos queda en `https://TU-USUARIO.github.io/simulador-sendero/`.

## Enlazarlo desde la página del condominio

```html
<a href="https://TU-USUARIO.github.io/simulador-sendero/" target="_blank" rel="noopener">
  Simula tu crédito
</a>
```

Incrustado en una página existente:

```html
<iframe src="https://TU-USUARIO.github.io/simulador-sendero/"
        style="width:100%;height:1400px;border:0" loading="lazy"
        title="Simulador de crédito Sendero"></iframe>
```

En WordPress, Wix o Squarespace el `iframe` va en un bloque de HTML personalizado.
Alternativa más limpia: subir `index.html` al propio hosting del sitio, por ejemplo en
`/simulador/`, y evitar el iframe.

## Logo e identidad

El logo va incrustado dentro de `index.html` en base64: no hay que subir ningún archivo de
imagen aparte ni configurar rutas, y no se rompe si mueves la página de carpeta. Aparece en
el encabezado a 148 px de alto en escritorio, junto al nombre del desarrollo, y también se
usa como ícono de pestaña del navegador.

El logo blanco carga sobre el plano verde. Al imprimir, se convierte a negro
automáticamente para que se vea en papel.

Para reemplazarlo por otra versión: convierte el PNG a base64 y sustituye el texto que sigue
a `data:image/png;base64,` en la etiqueta `<img class="logo">`. Conviene recortar antes los
márgenes transparentes y dejarlo en unos 640 px de ancho.

## Colores

Definidos como variables CSS al inicio de `index.html`, en `:root`:

- `--verde: #1E5B45` · verde Sendero, es el color base de toda la interfaz
- `--verde-hondo: #123B2C` · barra de pestañas y detalles
- `--verde-vivo: #3F8F6C` · foco y acentos

Si el verde institucional tiene un valor exacto distinto, cambia esos tres hex y la página
entera se actualiza. La barra de composición del costo se mantiene en azul a propósito, para
que se lea como una gráfica y no como parte de la identidad.

## Notas

- El campo de nombre del cliente va vacío: el Excel traía capturado un nombre de prueba.
- **Imprimir o guardar PDF** imprime solo la pestaña abierta, en hoja blanca y sin controles.
- El aviso legal del pie repite el del Excel: material informativo, no una oferta vinculante
  ni una aprobación de crédito.
