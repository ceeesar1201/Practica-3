Práctica 3 – Sala de minijuegos en VR

Desarrollo de Experiencias de Realidad Virtual. Sala de minijuegos en VR con cuatro estaciones: bolos, Wreck-It Ralph, Mr. Potato y dardos.

Versiones y cómo abrir el proyecto
Unreal Engine: 5.8
Meta XR Plugin: 1.207.0
Meta XR Simulator: v81 (portable, no incluido en el repositorio)

Controles en el simulador: WASD para moverse, U para agarrar (mantener), clic izquierdo o T para el gatillo, Y/H/G/J para el joystick.

Minijuegos
Bolos
Cómo se juega: agarra la bola con U, avanza hacia los bolos con W y suelta U en movimiento para lanzarla. En el simulador conviene subir Movement Speed a 4 (panel Inputs) para lanzar con fuerza.
Blueprints: BP_Bola (mesh bowling con componente de agarre). Los bolos son Static Mesh Actors (bowling_pin_ball) con física.
Wreck-It Ralph
Cómo se juega: coge el martillo con U, acércate al jarrón del pedestal y golpéalo. El jarrón se rompe en trozos, que quedan recogidos por las paredes que rodean el pedestal. En el simulador conviene bajar Movement Speed a 1 para golpear con control.
Blueprints y assets:
BP_Martillo: mesh de un martillo de FAB con física, CCD, Simulation Generates Hit Events y componente de agarre.
GC_vase: Geometry Collection creada con el modo Fracture a partir de un jarrón de FAB, fracturada con Uniform (20 fragmentos Voronoi).
Mr. Potato
Cómo se juega: coge una pieza de la mesa con U, llévala al cuerpo y suelta. Si toca el cuerpo, se queda acoplada donde la dejaste. Puedes volver a cogerla para recolocarla.
Blueprints:
BP_PotatoCuerpo: mesh del cuerpo con una Sphere Collision (OverlapAllDynamic) que detecta las piezas.
BP_PotatoPieza: mesh de la pieza con física y componente de agarre (BP_GrabComponent). En On Dropped comprueba con Is Overlapping Actor si toca el cuerpo; si es así, desactiva Simulate Physics y la acopla con Attach Actor To Actor (reglas Keep World). Al volver a agarrarla, el propio componente de agarre la engancha a la mano y la separa del cuerpo.
Dardos
Cómo se juega: coge un dardo con U, sube la mano a la altura de la diana con R, avanza hacia ella con W y suelta U en movimiento. El dardo se queda clavado y el marcador de al lado suma los puntos: 50 en el centro y 10 menos por cada anillo de 4 cm.
Blueprints:
El dardo tiene la gravedad desactivada para que vuele recto, porque en el simulador los lanzamientos con teclado salen con muy poca fuerza.
BP_Dardo: mesh del dardo con física, CCD, Simulation Generates Hit Events y componente de agarre. En On Component Hit, si choca con la diana, desactiva Simulate Physics, se acopla con Attach Actor To Actor (reglas Keep World) y calcula los puntos según la distancia entre el punto de impacto y el centro de la diana.
BP_Diana: mesh de la diana, variables Puntos y AnchoAnillo, evento SumarPuntos y un Widget Component en el mundo que muestra el marcador.
WBP_Marcador: Widget Blueprint con el texto de la puntuación.

Valores físicos principales
Objeto	Parámetro	Valor
Bola	Masa	6 kg
Bola	Linear Damping	0
Bola	Fricción (Physical Material)	0,1
Bola	CCD	Activado
Bolos	Masa	1,5 kg
Jarrón (GC_vase)	Damage Threshold	500
Martillo	Masa	2 kg
Martillo	CCD	Activado
Dardo	CCD	Activado
Dardo	Gravedad	Desactivada (ajuste para lanzar en el simulador)
Diana	Ancho de anillo de puntuación	4 cm
Diana	Altura del centro	173 cm
Simulador	Movement Speed (bolos y dardos)	4
Simulador	Movement Speed (Wreck-It Ralph)	1
Mejoras implementadas
Marcador de dardos acumulativo: suma la puntuación de todos los dardos lanzados.
Paredes de contención alrededor del jarrón para que los fragmentos no se dispersen por la sala.
Material físico de baja fricción en la bola de bolos (PM_Bola) para que ruede hasta los bolos.
