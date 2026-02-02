<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Mi Sistema</title>

  <!-- ✅ Tus CSS normales (deja los tuyos si ya existen) -->
  <link href="css/app.css" rel="stylesheet" />

  <style>
    /* ===========================================================
       ✅ LOADER (NO CAMBIA TU DISEÑO, SOLO LA BARRA REAL)
       =========================================================== */

    /* Si ya tienes estas clases, NO las borres. Solo asegúrate
       de incluir estas reglas para la barra. */

    .bar{
      position: relative;   /* ✅ obligatorio */
      overflow: hidden;     /* ✅ obligatorio */
    }

    /* ✅ Esta es la CLAVE: la barra debe empezar vacía
       y NO debe tener inset:0; */
    .bar-fill{
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;              /* ✅ altura completa */
      width: 0%;              /* ✅ empieza vacía */

      border-radius: 999px;

      /* ✅ mismos colores */
      background: linear-gradient(90deg,
        rgba(0,183,196,0.25),
        #00B7C4,
        rgba(0,183,196,0.25)
      );
      background-size: 220% 100%;

      /* ✅ shimmer (si ya tienes el keyframe, se usa el tuyo) */
      animation: shimmerMove 1.35s linear infinite;

      /* ✅ suaviza el llenado */
      transition: width .25s ease;
    }

    /* ✅ Si ya tienes esto, déjalo como estaba.
       Si no lo tienes, aquí te lo dejo para que funcione el shimmer. */
    @keyframes shimmerMove{
      0% { background-position: 0% 50%; }
      100% { background-position: 220% 50%; }
    }
  </style>
</head>

<body>
  <!-- ===========================================================
       ✅ TU DISEÑO COMPLETO (LOADER)
       Aquí pega tu diseño EXACTO.
       Yo solo te dejo el ejemplo del loader con la barra.
       =========================================================== -->

  <div id="app">

    <!-- ✅ REEMPLAZA tu loader actual por este bloque
         (puedes conservar tus demás divs, logos, rings, texto, etc.)
         Lo único importante es la parte de .bar y .bar-fill con id="barFill". -->

    <div class="splash">
      <!-- TODO: aquí va tu logo, ring, texto, etc. (déjalo igual) -->

      <!-- ✅ Tu barra (vacía y llenándose) -->
      <div class="bar">
        <div class="bar-fill" id="barFill"></div>
      </div>

      <!-- TODO: si tienes textos debajo, déjalos igual -->
    </div>

  </div>

  <!-- ===========================================================
       ✅ JS PARA LLENAR LA BARRA POCO A POCO
       (Pégalo EXACTAMENTE aquí, antes de blazor.webassembly.js)
       =========================================================== -->
  <script>
    (function () {
      function startProgress() {
        const bar = document.getElementById("barFill") || document.querySelector(".bar-fill");
        if (!bar) return;

        // ✅ siempre inicia vacía
        bar.style.width = "0%";

        let p = 0;

        // 🔧 Ajusta velocidad aquí (más lento = interval más alto / pasos más pequeños)
        const intervalMs = 240;   // 240-320 recomendado
        const minStep = 0.3;      // mínimo salto
        const maxStep = 1.1;      // máximo salto
        const cap = 88;           // se queda en 88% mientras carga

        const timer = setInterval(() => {
          if (p < cap) {
            p += (Math.random() * (maxStep - minStep)) + minStep;
            if (p > cap) p = cap;
            bar.style.width = Math.floor(p) + "%";
          }
        }, intervalMs);

        // ✅ Al terminar de cargar la página, forzar 100%
        window.addEventListener("load", () => {
          clearInterval(timer);
          bar.style.width = "100%";
        });

        // ✅ Respaldo: si por cualquier razón no se dispara load, completa en 8s
        setTimeout(() => {
          if (parseInt(bar.style.width || "0", 10) < 100) {
            clearInterval(timer);
            bar.style.width = "100%";
          }
        }, 8000);
      }

      // ✅ Asegura que el DOM exista antes de buscar la barra
      if (document.readyState === "loading") {
        document.addEventListener("DOMContentLoaded", startProgress);
      } else {
        startProgress();
      }
    })();
  </script>

  <!-- ✅ Blazor WASM -->
  <script src="_framework/blazor.webassembly.js"></script>
</body>
</html>