# Cálculo de tamaño de muestra en R

Tutorial práctico y reproducible para calcular tamaño de muestra y poder estadístico con el paquete [`pwr`](https://CRAN.R-project.org/package=pwr), orientado a investigación en salud pública y epidemiología.

**Ver el tutorial online: https://StephanoQuijano.github.io/tutorial-tamano-muestra/**

## Qué contiene

| Archivo | Descripción |
|---|---|
| `tutorial-tamano-muestra.qmd` | Código fuente del tutorial en Quarto |
| `index.html` | Sitio web del tutorial (lo que se ve en GitHub Pages) |
| `tutorial-tamano-muestra.pdf` | Tutorial en PDF, descargable desde el sitio |
| `styles.css` | Estilos del sitio |
| `figuras/` | Figuras generadas por el documento |

## Escenarios cubiertos

| # | Pregunta de investigación | Función |
|---|---|---|
| 1 | Una media vs un valor de referencia | `pwr.t.test(type = "one.sample")` |
| 2 | Una proporción vs un valor de referencia | `pwr.p.test()` |
| 3 | Dos medias en grupos independientes | `pwr.t.test(type = "two.sample")` |
| 4 | Dos proporciones en grupos independientes | `pwr.2p.test()` |
| 5 | Medias antes y después en los mismos sujetos | `pwr.t.test(type = "paired")` |
| 6 | Tres o más medias con efecto estandarizado | `pwr.anova.test()` |
| 7 | Tres o más medias con medias conocidas | `power.anova.test()` |
| 8 | Correlación entre dos variables numéricas | `pwr.r.test()` |
| 9 | Asociación entre variables categóricas | `pwr.chisq.test()` |
| 10 | Encuesta para estimar una proporción | fórmula clásica |

Además incluye los ajustes que casi siempre se olvidan: efecto de diseño (DEFF) para muestreo por conglomerados, pérdidas al seguimiento y no respuesta, la curva de poder, los errores más frecuentes y una plantilla para reportar el cálculo en la sección de métodos de un artículo.

## Cómo reproducirlo

Necesitas R (4.0 o superior), [Quarto](https://quarto.org/docs/get-started/) y una distribución de LaTeX (`quarto install tinytex`).

```r
install.packages(c("pwr", "pwr2"))
```

```bash
git clone https://github.com/StephanoQuijano/tutorial-tamano-muestra.git
cd tutorial-tamano-muestra
quarto render tutorial-tamano-muestra.qmd
```

El comando genera `index.html` (el sitio) y `tutorial-tamano-muestra.pdf` (la descarga). Para publicar el sitio, en GitHub: **Settings > Pages > Source: Deploy from a branch > main > / (root)**.

## Referencia rápida

```r
library(pwr)

# Una media vs referencia
pwr.t.test(d = 0.5, sig.level = 0.05, power = 0.80, type = "one.sample")

# Dos medias independientes (n es POR GRUPO)
pwr.t.test(d = 0.5, sig.level = 0.05, power = 0.80, type = "two.sample")

# Medias pareadas (n es el número de PARES)
pwr.t.test(d = 0.5, sig.level = 0.05, power = 0.80, type = "paired")

# Una proporción vs referencia
pwr.p.test(h = ES.h(0.15, 0.10), sig.level = 0.05, power = 0.80)

# Dos proporciones independientes
pwr.2p.test(h = ES.h(0.70, 0.55), sig.level = 0.05, power = 0.80)

# ANOVA de una vía
pwr.anova.test(f = 0.25, k = 3, sig.level = 0.05, power = 0.80)

# Correlación
pwr.r.test(r = 0.30, sig.level = 0.05, power = 0.80)

# Chi cuadrado (N es el TOTAL)
pwr.chisq.test(w = 0.30, df = 2, sig.level = 0.05, power = 0.80)
```

## Reglas de oro

1. El tamaño del efecto sale de la literatura, de un piloto o de la mínima diferencia clínicamente relevante. Nunca de lo que hace que el `n` sea cómodo.
2. Revisa siempre si el `n` que devuelve la función es por grupo o total.
3. Para proporciones usa `ES.h()`, nunca la resta directa.
4. En diseños pareados el denominador es la sd de las diferencias, no la de las mediciones.
5. Infla el `n` por pérdidas, no respuesta y efecto de diseño antes de escribirlo en el protocolo.
6. Redondea siempre hacia arriba con `ceiling()`.
7. El poder se calcula antes del estudio. El poder post hoc calculado con el efecto observado no aporta información.

## Licencia

MIT. Úsalo y adáptalo citando la fuente.

## Autor

Stephano Miguel Quijano Signori. Curso de Bioestadística.
