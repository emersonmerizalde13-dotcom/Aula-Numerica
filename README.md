# Aula-Numerica  Python 3.9+, sin dependencias externas."""
import json, math, random, webbrowser, threading
from http.server import BaseHTTPRequestHandler, ThreadingHTTPServer
from pathlib import Path

CURSOS = [f'{i}.º EGB' for i in range(5, 11)] + [f'{i}.º Bachillerato' for i in range(1, 4)]
TEMAS = []
def tema(curso, materia, nombre, regla, pregunta, funcion):
    TEMAS.append(dict(curso=CURSOS[curso], materia=materia, nombre=nombre, regla=regla, pregunta=pregunta, funcion=funcion))

def m(c, n, r, p, f): tema(c, 'Matemáticas', n, r, p, f)
def fis(c, n, r, p, f): tema(c, 'Física' if c >= 6 else 'Ciencias Naturales · Física', n, r, p, f)

# Orden propuesto: números y álgebra, geometría y medida, estadística.
m(0,'Suma de naturales','Suma las cantidades.','Tienes {a} monedas y ganas {b}. ¿Cuántas tienes?',lambda a,b:a+b)
m(0,'Resta de naturales','Cantidad final = inicial − retirada.','De {a}+{b} fichas retiras {b}. ¿Cuántas quedan?',lambda a,b:a)
m(0,'Multiplicación','Multiplicar suma grupos iguales.','Hay {a} cajas con {b} piezas cada una. ¿Cuántas piezas?',lambda a,b:a*b)
m(0,'División exacta','Divide el total entre el número de grupos.','Reparte {a}×{b} puntos entre {b} jugadores. ¿Cuántos recibe cada uno?',lambda a,b:a)
m(0,'Fracciones de una cantidad','Divide por el denominador y multiplica por el numerador.','¿Cuánto es 1/{b} de {a}×{b}?',lambda a,b:a)
m(0,'Decimales','Alinea las posiciones decimales.','Calcula {a}/10 + {b}/10. Responde con decimal.',lambda a,b:(a+b)/10)
m(0,'Perímetro del rectángulo','P = 2(largo + ancho).','Rectángulo de {a} m por {b} m. ¿Perímetro en m?',lambda a,b:2*(a+b))
m(0,'Medidas de longitud','1 m = 100 cm.','Convierte {a} metros a centímetros.',lambda a,b:100*a)
m(0,'Lectura de datos','El total es la suma de las frecuencias.','Un gráfico muestra {a} votos rojos y {b} azules. ¿Total?',lambda a,b:a+b)
m(1,'Operaciones combinadas','Resuelve la multiplicación antes de sumar.','Calcula {a} + {b} × 3.',lambda a,b:a+3*b)
m(1,'Múltiplos','Los múltiplos se obtienen multiplicando por enteros.','¿Cuál es el múltiplo número {b} de {a}? Empieza a contar en {a}.',lambda a,b:a*b)
m(1,'Divisores y MCD','El MCD es el mayor divisor común.','Calcula el MCD de {a} y {b}.',lambda a,b:math.gcd(a,b))
m(1,'Mínimo común múltiplo','MCM(a,b) = a×b/MCD(a,b).','Calcula el MCM de {a} y {b}.',lambda a,b:a*b//math.gcd(a,b))
m(1,'Suma de fracciones','Con igual denominador, suma los numeradores.','Calcula {a}/10 + {b}/10. Responde como fracción o decimal.',lambda a,b:(a+b)/10)
m(1,'Producto de decimales','Multiplica y ubica los decimales.','Calcula ({a}/10) × {b}.',lambda a,b:a*b/10)
m(1,'Área del triángulo','A = base×altura/2.','Base {a} cm y altura {b} cm. ¿Área en cm²?',lambda a,b:a*b/2)
m(1,'Área del paralelogramo','A = base×altura.','Base {a} cm, altura perpendicular {b} cm. ¿Área en cm²?',lambda a,b:a*b)
m(1,'Media aritmética','Media = suma de datos/cantidad.','Calcula la media de {a} y {b}.',lambda a,b:(a+b)/2)
m(2,'Potencias','Una potencia multiplica la base por sí misma.','Calcula {a}².',lambda a,b:a*a)
m(2,'Raíces cuadradas','La raíz positiva de n² es n cuando n es positivo.','Calcula √({a}²).',lambda a,b:a)
m(2,'División de fracciones','Multiplica por la fracción inversa.','Calcula ({a}/2) ÷ ({b}/2).',lambda a,b:a/b)
m(2,'Proporcionalidad directa','Mantén constante la razón.','Una entrada cuesta {a} dólares. ¿Costo de {b} entradas?',lambda a,b:a*b)
m(2,'Porcentajes','Porcentaje = total×porcentaje/100.','Calcula el {b}% de {a} dólares.',lambda a,b:a*b/100)
m(2,'Área del círculo','A = πr². Usa π = 3,14 en este juego.','Radio {a} cm. ¿Área en cm²? Usa π=3,14.',lambda a,b:3.14*a*a)
m(2,'Volumen del prisma','V = largo×ancho×alto.','Prisma de {a} cm × {b} cm × 3 cm. ¿Volumen en cm³?',lambda a,b:a*b*3)
m(2,'Probabilidad simple','P = casos favorables/casos posibles.','Hay {a} bolas rojas y {b} azules. ¿Probabilidad de sacar una roja?',lambda a,b:a/(a+b))
m(3,'Números enteros','Sumar un negativo equivale a restar.','Calcula −{a} + {b}.',lambda a,b:b-a)
m(3,'Números racionales','Usa un denominador común.','Calcula {a}/3 − {b}/4.',lambda a,b:a/3-b/4)
m(3,'Potencias y exponentes','a²×a³ = a⁵.','Calcula 2² × 2³.',lambda a,b:32)
m(3,'Expresiones algebraicas','Sustituye x y calcula.','Evalúa 3x + {b} si x={a}.',lambda a,b:3*a+b)
m(3,'Ecuaciones de primer grado','Resta el término independiente.','Resuelve x + {b} = {a}+{b}.',lambda a,b:a)
m(3,'Ángulos de un triángulo','Los ángulos interiores suman 180°.','Dos ángulos miden {a}° y {b}°. ¿Cuánto mide el tercero?',lambda a,b:180-a-b)
m(3,'Mediana','Ordena los datos y toma el central.','Calcula la mediana de {a}, {a}+1 y {a}+2.',lambda a,b:a+1)
m(4,'Números reales y radicales','√(a²b²)=ab para a,b positivos.','Simplifica √({a}²×{b}²).',lambda a,b:a*b)
m(4,'Productos notables','(x+y)² = x²+2xy+y².','Calcula ({a}+{b})².',lambda a,b:(a+b)**2)
m(4,'Factorización','x²−y²=(x−y)(x+y).','Evalúa ({a}−{b})({a}+{b}).',lambda a,b:a*a-b*b)
m(4,'Sistemas de ecuaciones','Suma las ecuaciones para eliminar y.','x+y={a}+{b}; x−y={a}−{b}. ¿Valor de x?',lambda a,b:a)
m(4,'Función lineal','Evalúa f(x)=mx+b.','f(x)=2x+{b}. Calcula f({a}).',lambda a,b:2*a+b)
m(4,'Teorema de Pitágoras','Hipotenusa = √(cateto₁²+cateto₂²).','Catetos 3×{a} y 4×{a} cm. ¿Hipotenusa en cm?',lambda a,b:5*a)
m(4,'Estadística: rango','Rango = máximo − mínimo.','Calcula el rango de {a}, {a}+{b}, {a}+2×{b}.',lambda a,b:2*b)
m(5,'Ecuaciones cuadráticas','Si x²=a², las soluciones son ±a.','Resuelve x²={a}². Escribe solo la solución positiva.',lambda a,b:a)
m(5,'Función cuadrática','Sustituye x en la expresión.','f(x)=x²+{b}. Calcula f({a}).',lambda a,b:a*a+b)
m(5,'Semejanza y escala','Longitud real = longitud dibujada×escala.','Mapa 1:{a}. Una línea mide {b} cm. ¿Longitud real en cm?',lambda a,b:a*b)
m(5,'Trigonometría','sen(θ) = cateto opuesto/hipotenusa.','Opuesto {a} cm e hipotenusa {a}+{b} cm. Calcula sen(θ).',lambda a,b:a/(a+b))
m(5,'Pendiente','m=(y₂−y₁)/(x₂−x₁).','Recta por (0,0) y ({a},{b}). ¿Pendiente?',lambda a,b:b/a)
m(5,'Probabilidad complementaria','P(no A)=1−P(A).','P(ganar)={a}/({a}+{b}). ¿P(no ganar)?',lambda a,b:b/(a+b))
m(6,'Funciones y dominio','Un denominador debe ser distinto de cero.','f(x)=1/(x−{a}). ¿Qué valor de x se excluye?',lambda a,b:a)
m(6,'Función afín e inversa','Si y=2x+b, x=(y−b)/2.','f(x)=2x+{b}. Si f(x)=2×{a}+{b}, ¿x?',lambda a,b:a)
m(6,'Funciones cuadráticas: vértice','El vértice de (x−h)²+k tiene abscisa h.','f(x)=(x−{a})²+{b}. ¿Abscisa del vértice?',lambda a,b:a)
m(6,'Sistemas lineales','Despeja o elimina una variable.','2x+y=2×{a}+{b}; y={b}. ¿x?',lambda a,b:a)
m(6,'Vectores en el plano','Suma componente por componente.','u=({a},2), v=({b},3). ¿Componente x de u+v?',lambda a,b:a+b)
m(6,'Distancia entre puntos','d=√((Δx)²+(Δy)²).','Distancia entre (0,0) y (3×{a},4×{a}).',lambda a,b:5*a)
m(6,'Dispersión: varianza poblacional','Varianza = media de desviaciones cuadradas.','Varianza poblacional de {a}−{b} y {a}+{b}.',lambda a,b:b*b)
m(7,'Funciones exponenciales','Evalúa la potencia.','Calcula f(3) si f(x)={a}^x.',lambda a,b:a**3)
m(7,'Logaritmos','log base b de b^n = n.','Calcula log₂(2^{a}).',lambda a,b:a)
m(7,'Trigonometría: identidad','sen²θ+cos²θ=1.','Si sen²θ={a}/({a}+{b}), calcula cos²θ.',lambda a,b:b/(a+b))
m(7,'Progresiones aritméticas','aₙ=a₁+(n−1)d.','Primer término {a}, diferencia {b}. ¿Quinto término?',lambda a,b:a+4*b)
m(7,'Progresiones geométricas','aₙ=a₁r^(n−1).','Primer término {a}, razón 2. ¿Cuarto término?',lambda a,b:8*a)
m(7,'Cónicas: circunferencia','x²+y²=r² para centro (0,0).','x²+y²={a}². ¿Radio?',lambda a,b:a)
m(7,'Combinatoria','Combinaciones de n objetos en parejas: n(n−1)/2.','¿Cuántas parejas distintas se forman con {a}+2 estudiantes?',lambda a,b:(a+2)*(a+1)/2)
m(8,'Límites','En polinomios, sustituye el valor.','Límite de x²+{b} cuando x tiende a {a}.',lambda a,b:a*a+b)
m(8,'Derivadas de polinomios','d(x²)/dx=2x.','f(x)=x²+{b}. Calcula f′({a}).',lambda a,b:2*a)
m(8,'Optimización','En −(x−h)²+k el máximo se alcanza en x=h.','f(x)=−(x−{a})²+{b}. ¿Qué x maximiza f?',lambda a,b:a)
m(8,'Integrales definidas','La integral de una constante es constante×longitud.','Integral de {a} desde x=0 hasta x={b}.',lambda a,b:a*b)
m(8,'Vectores: producto escalar','u·v=u₁v₁+u₂v₂.','u=({a},2), v=({b},3). Calcula u·v.',lambda a,b:a*b+6)
m(8,'Matrices','Suma las entradas correspondientes.','A₁₁={a}, B₁₁={b}. ¿Entrada (1,1) de A+B?',lambda a,b:a+b)
m(8,'Probabilidad binomial','Para dos ensayos, P(dos éxitos)=p².','Dos ensayos independientes con p=1/2. ¿P(dos éxitos)?',lambda a,b:0.25)

fis(0,'Longitud y unidades','1 km = 1000 m.','Convierte {a} km a metros.',lambda a,b:1000*a)
fis(0,'Tiempo','1 minuto = 60 segundos.','Convierte {a} minutos a segundos.',lambda a,b:60*a)
fis(0,'Movimiento cotidiano','Distancia = rapidez×tiempo.','Caminas a {a} m/s por {b} s. ¿Distancia en m?',lambda a,b:a*b)
fis(1,'Masa','1 kg = 1000 g.','Convierte {a} kg a gramos.',lambda a,b:1000*a)
fis(1,'Volumen y capacidad','1 litro = 1000 mililitros.','Convierte {a} litros a mililitros.',lambda a,b:1000*a)
fis(1,'Temperatura','El cambio es final menos inicial.','Temperatura inicial {a} °C y final {a}+{b} °C. ¿Cambio en °C?',lambda a,b:b)
fis(2,'Rapidez media','v=d/t.','Recorres {a}×{b} m en {b} s. ¿Rapidez en m/s?',lambda a,b:a)
fis(2,'Fuerzas en la misma dirección','Suma fuerzas que actúan en el mismo sentido.','Dos fuerzas de {a} N y {b} N actúan a la derecha. ¿Resultante en N?',lambda a,b:a+b)
fis(2,'Energía y consumo','Energía = potencia×tiempo.','Equipo de {a} W durante {b} s. ¿Energía en J?',lambda a,b:a*b)
fis(3,'Sistema internacional','1 hora = 3600 segundos.','Convierte {a} horas a segundos.',lambda a,b:3600*a)
fis(3,'Densidad','ρ=m/V.','Masa {a} kg y volumen {b} m³. ¿Densidad en kg/m³?',lambda a,b:a/b)
fis(3,'Presión','P=F/A.','Fuerza {a} N sobre {b} m². ¿Presión en Pa?',lambda a,b:a/b)
fis(4,'Aceleración media','a=(v final−v inicial)/t.','Parte del reposo y llega a {a} m/s en {b} s. ¿Aceleración en m/s²?',lambda a,b:a/b)
fis(4,'Fuerza y masa','F=ma.','Masa {a} kg, aceleración {b} m/s². ¿Fuerza en N?',lambda a,b:a*b)
fis(4,'Máquinas simples: palanca','En equilibrio: F₁d₁=F₂d₂.','F₁={a} N, d₁={b} m y d₂=2 m. ¿F₂ en N?',lambda a,b:a*b/2)
fis(5,'Trabajo mecánico','W=Fd cuando fuerza y desplazamiento son paralelos.','Fuerza {a} N en el sentido de un desplazamiento de {b} m. ¿Trabajo en J?',lambda a,b:a*b)
fis(5,'Electricidad: ley de Ohm','V=IR.','I={a} A, R={b} Ω. ¿Voltaje en V?',lambda a,b:a*b)
fis(5,'Calor sensible','Q=mcΔT.','m={a} kg, c=100 J/(kg·°C), ΔT={b} °C. ¿Q en J?',lambda a,b:a*100*b)
fis(6,'Vectores y desplazamiento','Suma desplazamientos con signo.','Avanzas {a} m y retrocedes {b} m. ¿Desplazamiento en m?',lambda a,b:a-b)
fis(6,'Movimiento rectilíneo uniforme','x=vt desde x inicial cero.','v={a} m/s, t={b} s. ¿Posición en m?',lambda a,b:a*b)
fis(6,'Movimiento uniformemente acelerado','Desde reposo: x=at²/2.','a={a} m/s², t={b} s. ¿Desplazamiento en m?',lambda a,b:a*b*b/2)
fis(6,'Caída libre','Desde reposo: velocidad=g×t. Usa g=9,8 m/s².','Caída durante {a} s sin resistencia del aire. ¿Rapidez en m/s?',lambda a,b:9.8*a)
fis(6,'Movimiento parabólico','Sin rozamiento, x=v horizontal×tiempo.','Velocidad horizontal {a} m/s durante {b} s. ¿Desplazamiento horizontal en m?',lambda a,b:a*b)
fis(6,'Leyes de Newton','F neta=ma.','m={a} kg, a={b} m/s². ¿F neta en N?',lambda a,b:a*b)
fis(6,'Rozamiento','F de rozamiento=μN.','μ=0,2 y N={a} N. ¿Rozamiento en N?',lambda a,b:0.2*a)
fis(7,'Movimiento circular','v=ωr.','ω={a} rad/s, r={b} m. ¿Rapidez tangencial en m/s?',lambda a,b:a*b)
fis(7,'Energía cinética','Ec=mv²/2.','m={a} kg, v={b} m/s. ¿Ec en J?',lambda a,b:a*b*b/2)
fis(7,'Energía potencial gravitatoria','Ep=mgh. Usa g=9,8 m/s².','m={a} kg, h={b} m. ¿Ep en J?',lambda a,b:9.8*a*b)
fis(7,'Cantidad de movimiento','p=mv.','m={a} kg, v={b} m/s. ¿p en kg·m/s?',lambda a,b:a*b)
fis(7,'Impulso','I=FΔt para fuerza constante.','F={a} N durante {b} s. ¿Impulso en N·s?',lambda a,b:a*b)
fis(7,'Gravitación','F=Gm₁m₂/r².','Usa G=6,67×10⁻¹¹; masas {a} kg y {b} kg; r=1 m. ¿F en N?',lambda a,b:6.67e-11*a*b)
fis(7,'Termodinámica','ΔU=Q−W; W positivo si lo realiza el sistema.','Recibe {a} J de calor y realiza {b} J de trabajo. ¿ΔU en J?',lambda a,b:a-b)
fis(8,'Ondas','v=fλ.','f={a} Hz, λ={b} m. ¿Rapidez de onda en m/s?',lambda a,b:a*b)
fis(8,'Período y frecuencia','T=1/f.','f={a} Hz. ¿Período en s?',lambda a,b:1/a)
fis(8,'Óptica: reflexión','Ángulo de reflexión = ángulo de incidencia, respecto a la normal.','Incidencia {a}°. ¿Ángulo de reflexión en grados?',lambda a,b:a)
fis(8,'Electrostática','F=k|q₁q₂|/r².','k=9×10⁹; q₁={a} μC, q₂={b} μC, r=1 m. ¿Magnitud de F en N?',lambda a,b:0.009*a*b)
fis(8,'Circuitos en serie','Resistencia equivalente = suma de resistencias.','R₁={a} Ω, R₂={b} Ω en serie. ¿R equivalente en Ω?',lambda a,b:a+b)
fis(8,'Circuitos en paralelo','R equivalente=R₁R₂/(R₁+R₂).','R₁={a} Ω, R₂={b} Ω en paralelo. ¿R equivalente en Ω?',lambda a,b:a*b/(a+b))
fis(8,'Potencia eléctrica','P=VI.','V={a} V, I={b} A. ¿Potencia en W?',lambda a,b:a*b)
fis(8,'Magnetismo','F=qvB para velocidad perpendicular al campo.','q={a} C, v={b} m/s, B=0,5 T; velocidad perpendicular. ¿Magnitud de F en N?',lambda a,b:a*b*0.5)
fis(8,'Inducción electromagnética','Magnitud de fem = |Δflujo|/Δt en una espira.','Cambio de flujo {a} Wb en {b} s. ¿Magnitud de fem en V?',lambda a,b:a/b)
fis(8,'Física moderna: fotones','E=hf.','h=6,626×10⁻³⁴ J·s; f={a}×10¹⁴ Hz. ¿E en J?',lambda a,b:6.626e-20*a)

def banco(seed=None):
    rng=random.Random(seed)
    datos=[]
    for i,t in enumerate(TEMAS):
        qs=[]
        for _ in range(30):
            a,b=rng.randint(2,20),rng.randint(2,12)
            valor=float(t['funcion'](a,b))
            qs.append(dict(texto=t['pregunta'].format(a=a,b=b),respuesta=valor))
        datos.append({k:v for k,v in t.items() if k not in ('funcion','pregunta')} | dict(id=i,preguntas=qs))
    return datos

HTML = r'''<!doctype html><html lang="es"><meta charset="utf-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>Aula Numérica</title>
<style>body{margin:0;background:#101827;color:#e8efff;font:16px system-ui}main{max-width:1100px;margin:auto;padding:24px}h1{font-size:34px;color:#79e7ce}p{line-height:1.5}.panel{background:#1c2940;border:1px solid #344763;border-radius:16px;padding:22px;margin:16px 0}select,input,button{font:inherit;border-radius:8px;padding:12px;margin:5px;border:1px solid #63728b}button{background:#79e7ce;color:#102238;cursor:pointer;font-weight:700}button:disabled{opacity:.5;cursor:default}select,input{background:#101827;color:white}input{max-width:90%}.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(260px,1fr));gap:12px}.card{background:#23344e;text-align:left;color:white;padding:18px}.small{font-size:14px;color:#bbcae0}.tag{color:#79e7ce}progress{width:100%;height:18px}#pregunta{font-size:24px}#feedback{white-space:pre-wrap;line-height:1.6}table{width:100%;border-collapse:collapse}td,th{text-align:left;border-bottom:1px solid #43516a;padding:10px}.scroll{overflow:auto}a{color:#79e7ce}</style>
<main><h1>Aula Numérica · Ecuador</h1><p>Explora matemáticas y física desde 5.º EGB hasta 3.º Bachillerato. Cada misión tiene 10 retos: gana 100 puntos por acierto y una estrella con 8 aciertos.</p><p class="small">Secuencia pedagógica propuesta por curso, orientada por bloques curriculares. Esta versión practica los temas del catálogo; no certifica cobertura de todas las destrezas oficiales. En EGB, física corresponde a contenidos de Ciencias Naturales.</p>
<div class="panel"><label>Estudiante <input id="nombre" maxlength="60" placeholder="Escribe tu nombre"></label><label>Curso <select id="curso"></select></label><label>Asignatura <select id="materia"></select></label><button id="verInforme">Resultados</button><button id="exportar">Exportar CSV</button></div>
<section id="catalogo"><h2>Tu ruta de aprendizaje</h2><p class="small">Sigue la numeración. Puedes repetir cualquier misión o elegir un tema para repasar.</p><div id="temas" class="grid"></div></section>
<section id="juego" class="panel" hidden><button id="volver">Volver a temas</button><h2 id="titulo"></h2><div id="estado"></div><progress id="barra" max="10" value="0"></progress><p id="regla" class="tag"></p><p id="pregunta"></p><p class="small">Escribe solo el número, usando punto o coma decimal, una fracción como 3/4 o notación científica como 2e-5. Hasta 4 cifras significativas es suficiente.</p><input id="respuesta" aria-label="Respuesta numérica" autocomplete="off"><button id="comprobar">Comprobar</button><button id="siguiente" hidden>Siguiente reto</button><p id="feedback" role="status"></p><div id="fin"></div><canvas id="cohete" width="600" height="100" style="width:100%;max-width:600px" aria-label="Avance de la misión"></canvas></section>
<section id="informe" class="panel" hidden><button id="cerrarInforme">Volver a temas</button><h2>Resultados del estudiante</h2><p id="resumen"></p><div class="scroll"><table><thead><tr><th>Curso / Tema</th><th>Intentos</th><th>Aciertos</th><th>Precisión</th><th>Tiempo medio</th><th>Orientación</th></tr></thead><tbody id="filas"></tbody></table></div><p class="small">Los resultados se guardan en este navegador. El tiempo mide cuánto tardas en responder, incluido el tiempo de lectura. Exporta un CSV para conservar o analizar los resultados.</p></section></main>
<script>const DATOS=__DATOS__,CURSOS=__CURSOS__;const $=id=>document.getElementById(id);let registros=[];try{registros=JSON.parse(localStorage.getItem('aula_resultados_v1')||'[]');if(!Array.isArray(registros))registros=[]}catch(e){}let jugador="",activo=null,qs=[],indice=0,aciertos=0,puntos=0,inicio=0,bloqueado=false;const fmt=n=>Number(n.toPrecision(5)).toString();function estudiante(){return $('nombre').value.trim()||'Invitado'}function options(el,values){el.replaceChildren();values.forEach(v=>{const o=document.createElement('option');o.value=v;o.textContent=v;el.append(o)})}options($('curso'),CURSOS);
function materias(){options($('materia'),[...new Set(DATOS.filter(t=>t.curso==$('curso').value).map(t=>t.materia))]);catalogo()}function catalogo(){$('temas').replaceChildren();DATOS.filter(t=>t.curso==$('curso').value&&t.materia==$('materia').value).forEach((t,i)=>{const b=document.createElement('button');b.className='card';let rows=registros.filter(r=>r.estudiante===estudiante()&&r.tema===t.nombre&&r.curso===t.curso);b.textContent=`${i+1}. ${t.nombre}\n${rows.length?Math.round(rows.filter(r=>r.correcto).length/rows.length*100)+'% de aciertos':'Sin practicar'}`;b.onclick=()=>empezar(t);$('temas').append(b)})}
function empezar(t){jugador=estudiante();activo=t;qs=[...t.preguntas].sort(()=>Math.random()-.5).slice(0,10);indice=aciertos=puntos=0;$('catalogo').hidden=true;$('informe').hidden=true;$('juego').hidden=false;$('titulo').textContent=t.curso+' · '+t.nombre;$('fin').replaceChildren();mostrar()}
function dibujar(){let ctx=$('cohete').getContext('2d');ctx.clearRect(0,0,600,100);ctx.fillStyle='#101827';ctx.fillRect(0,0,600,100);ctx.strokeStyle='#63728b';ctx.beginPath();ctx.moveTo(20,65);ctx.lineTo(570,65);ctx.stroke();ctx.fillStyle='#79e7ce';ctx.font='30px sans-serif';ctx.fillText('🚀',20+indice*50,55);ctx.fillStyle='#ffe098';ctx.fillText('★',555,55)}
function mostrar(){bloqueado=false;$('estado').textContent=`Reto ${indice+1}/10 · ${puntos} puntos · ${aciertos} aciertos`;$('barra').value=indice;$('regla').textContent=activo.regla;$('pregunta').textContent=qs[indice].texto;$('respuesta').value='';$('respuesta').disabled=false;$('comprobar').disabled=false;$('siguiente').hidden=true;$('feedback').textContent='';inicio=performance.now();dibujar();$('respuesta').focus()}
function parsear(s){s=s.trim().replace(',','.');if(/^[+-]?(\d+(\.\d*)?|\.\d+)(e[+-]?\d+)?$/i.test(s))return Number(s);if(/^[+-]?\d+(\.\d+)?\s*\/\s*[+-]?\d+(\.\d+)?$/.test(s)){let [a,b]=s.split('/').map(Number);return b!==0?a/b:NaN}return NaN}
function comprobar(){if(bloqueado)return;let n=parsear($('respuesta').value);if(!Number.isFinite(n)){$('feedback').textContent='Introduce un número válido. Ejemplo: 0,75 o 3/4.';return}let q=qs[indice],ok=Math.abs(n-q.respuesta)<=Math.max(Math.abs(q.respuesta)*0.0005,1e-30);bloqueado=true;if(ok){aciertos++;puntos+=100}registros.push({fecha:new Date().toISOString(),estudiante:jugador,curso:activo.curso,materia:activo.materia,tema:activo.nombre,pregunta:q.texto,respuesta:n,esperado:q.respuesta,correcto:ok,segundos:Number(((performance.now()-inicio)/1000).toFixed(2))});let aviso='';try{localStorage.setItem('aula_resultados_v1',JSON.stringify(registros))}catch(e){aviso='\nNo se pudo guardar en el navegador. Exporta los resultados.'}$('feedback').textContent=(ok?'¡Correcto! +100 puntos':'Esta vez no. Puedes seguir practicando.')+'\n'+activo.regla+'\nResultado: '+fmt(q.respuesta)+aviso;$('respuesta').disabled=true;$('comprobar').disabled=true;$('siguiente').hidden=false;$('siguiente').textContent=indice===9?'Ver resultado de la misión':'Siguiente reto'}
function siguiente(){indice++;if(indice<10){mostrar();return}$('barra').value=10;dibujar();$('siguiente').hidden=true;$('estado').textContent=`Misión terminada · ${puntos} puntos`;$('fin').textContent=`${aciertos}/10 aciertos. ${aciertos>=8?'★ ¡Estrella conseguida!':'Repite la misión para reforzar el tema.'}`}
function informe(){$('informe').hidden=false;$('juego').hidden=true;$('catalogo').hidden=true;const rows=registros.filter(r=>r.estudiante===estudiante());$('resumen').textContent=rows.length?`${rows.length} respuestas · ${Math.round(rows.filter(r=>r.correcto).length/rows.length*100)}% de aciertos`:'Todavía no hay resultados para este estudiante.';$('filas').replaceChildren();let grupos=new Map();rows.forEach(r=>{let k=r.curso+' · '+r.tema;if(!grupos.has(k))grupos.set(k,[]);grupos.get(k).push(r)});grupos.forEach((rs,k)=>{let ac=rs.filter(r=>r.correcto).length,pc=ac/rs.length*100,tr=document.createElement('tr');[k,rs.length,ac,Math.round(pc)+'%',(rs.reduce((s,r)=>s+r.segundos,0)/rs.length).toFixed(1)+' s',pc>=80?'Continuar y repasar':pc>=50?'Practicar nuevamente':'Revisar la explicación y practicar'].forEach(v=>{let td=document.createElement('td');td.textContent=v;tr.append(td)});$('filas').append(tr)})}
function exportar(){let rows=registros.filter(r=>r.estudiante===estudiante());if(!rows.length){alert('Este estudiante todavía no tiene resultados.');return}let keys=Object.keys(rows[0]);function celda(v){let s=String(v);if(/^[=+@\-\t\r]/.test(s))s="'"+s;return '"'+s.replaceAll('"','""')+'"'}let csv='\uFEFF'+[keys,...rows.map(r=>keys.map(k=>r[k]))].map(row=>row.map(celda).join(';')).join('\r\n');let u=URL.createObjectURL(new Blob([csv],{type:'text/csv;charset=utf-8'})),a=document.createElement('a');a.href=u;a.download='resultados_aula.csv';a.click();setTimeout(()=>URL.revokeObjectURL(u),1000)}
$('curso').onchange=materias;$('materia').onchange=catalogo;$('nombre').onchange=catalogo;$('comprobar').onclick=comprobar;$('siguiente').onclick=siguiente;$('respuesta').onkeydown=e=>{if(e.key==='Enter')comprobar()};$('volver').onclick=()=>{$('juego').hidden=true;$('catalogo').hidden=false;catalogo()};$('verInforme').onclick=informe;$('exportar').onclick=exportar;$('cerrarInforme').onclick=()=>{$('informe').hidden=true;$('catalogo').hidden=false;catalogo()};materias();
</script></html>'''

def pagina(seed=None):
    return HTML.replace('__DATOS__',json.dumps(banco(seed),ensure_ascii=False)).replace('__CURSOS__',json.dumps(CURSOS,ensure_ascii=False))

def main():
    contenido=pagina().encode('utf-8')
    class Handler(BaseHTTPRequestHandler):
        def do_GET(self):
            if self.path not in ('/','/index.html'):
                self.send_error(404);return
            self.send_response(200)
            self.send_header('Content-Type','text/html; charset=utf-8')
            self.send_header('Content-Length',str(len(contenido)))
            self.end_headers();self.wfile.write(contenido)
        def log_message(self,*args): pass
    server=ThreadingHTTPServer(('127.0.0.1',0),Handler)
    url=f'http://127.0.0.1:{server.server_port}/'
    # Puerto variable: el HTML independiente ofrece persistencia estable de resultados.
    archivo=Path(__file__).with_name('AulaNumerica.html')
    archivo.write_text(contenido.decode('utf-8'),encoding='utf-8')
    print('Aula Numérica lista. Abriendo:',archivo)
    print('Si no abre, abre AulaNumerica.html con Chrome, Edge o Firefox.')
    webbrowser.open(archivo.resolve().as_uri())
    print('También disponible en:',url,' | Para salir: Ctrl+C')
    try: server.serve_forever()
    except KeyboardInterrupt: pass
    finally: server.server_close()

if __name__=='__main__': main()

