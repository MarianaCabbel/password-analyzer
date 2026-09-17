# Password Strength Analyzer

Herramienta de una sola página (HTML/CSS/JS puro, sin frameworks ni backend)
que analiza la fortaleza de una contraseña y verifica si aparece en brechas
de datos conocidas, sin enviar la contraseña completa a ningún servidor.

## Cómo verlo en tu PC

1. Abre esta carpeta en VS Code.
2. Clic derecho en `index.html` → "Open with Live Server".
3. Escribe cualquier contraseña de prueba en el campo — todo se calcula
   en vivo (fortaleza, entropía, checklist).

Prueba con algo obviamente débil primero, como `password123`, para ver
que sí lo detecta como filtrado y débil. Luego prueba algo largo y
aleatorio para ver la barra en verde.

## Cómo funciona (para explicar en entrevista)

**Fortaleza (client-side, sin red):**
- Longitud, mayúsculas, minúsculas, números, símbolos — checklist básico.
- Cálculo de **entropía en bits**: `longitud × log2(tamaño del alfabeto
  usado)`. Es la forma real en que se mide qué tan "adivinable" es una
  contraseña, no solo contar cuántos tipos de caracteres tiene.
- Detección de patrones débiles: secuencias (`1234`, `qwerty`) y
  caracteres repetidos (`aaa`, `111`).

**Verificación de brechas (k-anonymity):**
1. La contraseña se hashea con SHA-1 **en el navegador** — nunca sale
   de tu máquina en texto plano.
2. Solo se envían los **primeros 5 caracteres** del hash a la API pública
   de Have I Been Pwned (`api.pwnedpasswords.com`).
3. La API responde con todos los hashes que comparten ese prefijo
   (típicamente 400-800 resultados) — nunca sabe cuál buscabas tú.
4. La comparación final para ver si tu hash completo está en la lista
   pasa **localmente**, en tu navegador.

Este es exactamente el modelo que usan gestores de contraseñas reales
(1Password, Firefox Monitor) para verificar brechas sin comprometer la
privacidad del usuario — es un buen tema para explicar en una entrevista
técnica de por qué "simplemente mandar la contraseña al servidor" sería
un mal diseño de seguridad.

## Desplegarlo

Igual que el portafolio: sube esta carpeta a un repo de GitHub
(`password-analyzer`) y conéctalo en Vercel con Framework Preset "Other"
(no necesita build).

```bash
git init
git add .
git commit -m "Initial commit: password strength analyzer"
git branch -M main
git remote add origin https://github.com/MarianaCabbel/password-analyzer.git
git push -u origin main
```

Luego en vercel.com: "Add New Project" → selecciona el repo → Framework
Preset: Other → Deploy.
