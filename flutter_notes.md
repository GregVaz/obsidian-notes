## Arquitectura de software
### 30/05/2022 

Palabaras clave
- estructura, diseñar y proyectar

La arquitectura de software
- indica la estructura, funcionamiento e interaccion dentre las partes del software
- Teer el _código organizado_ para que todo el equipo pueda verlo

Dependiendo de la arquitectura y alcance del proyecto es donde podemos entender y visualizar el alcance del mismo y por ende tambien el tipo de arquitectura que podría ser aplicada a esta

Tener una arquitectura apoyara a que cada integrante pueda encontrar y enfocarse en su parte del código 
Por ejemplo si tenemos un UX Designer, UI Designer, DB Admin, Dev o PM


Razones para implementar una arquitectura
- Si la aplicación ya funciona con solicitud y envio de Datos
- Si la aplicación limpia, transforma, prepara datos
- Si la aplicación Inserta y obtiene una base de datos

Tipos de arquitecturas en Flutter
- Vanilla
- Scoped Model
- BLoC


### Arquitectura Vanilla

- En esta arquitectura **la lógica y la vista** se colocan en el **Widget**.

Su principal beneficio es que es simple y autónoma. Conectado en cualquier parte de tu aplicación, recuperará y renderizará datos.

-> Escribir Widgets de esta manera puede generar caos en el documento de vista de la app, sobre todo cuando la lógica empieza a extenderse a bifurcarse o es más avanzado.

```dart
Widget _buildInit() {
	return Center(
		child: RaisedButton(
			child: const Text('Load user data'),
			onPressed: () {
				setState(() {
					_isLoading = true;
				});
				widget._repository.getUser().then((user) {
					setState(() {
						_user = user;
						_isLoading = false;
					})
				}
			}
		),
	)
}
```

> Como recomendación puntual, este tipo de Arquitectura aunque es sencilla, en realidad rómpe con uno de los prinicpios SOLID, Single responsability, el cuál dice que una clase debe tener solo una responsabilidad. En este caso, _la vista y el controlador_ son dos responsabilidades que estan cayendo en la misma clase


### Arquitectura Scoped Model

En esta arquitectura cuando un Widget cambia de estado se reconstruye el árbol completo (la pantalla completa).
Esto no suena lo mas conveniente ya que lo que se busca usualmente es que se reconstruya solo el widget que está cambiando y no los demas.

Esta arquitectura es buena pues cumple con el Principio de Single Responsability pues separá la logica del negocio de la UI, pero en general **el mantenimiento se vuelve complejo** por la grande dependencia entre Widgets, debes controlar muchos casos para lograr el efecto que quieresdar a tu aplicación.

### Arquitectura BLoC (Business Logic Components)

- Es un patro de diseño que separa la lógica del negocio de la interfaz gráfica.
  - Nace por Paolo Solares y Cong Hui

Este patron se divide en dos partes
- View
  - Vistas, botones, cards, etc, Capa especial para las vistas 
- BLoc
  - La vista del negocio sera separada en en otra parte

Estructura

View (UI Screen) <-> BLoC <-> Repository <-> Data/Model

-> La vista tendrá toda la interaccion con las vistas, donde la separaremos en screens y widgets, donde los widgets seran los componentes especificos o atomos viedolo de forma distinta, y las screens como lo comenta la palabra, la unión de dichos componentes para formar una vista completa

-> La capa del negocio, aquí se reuniran todos los casos de uso de la aplicación, si el usuario tiene cierta accion de autenticación, estas fases se encontrarán encapsuladas en esta capa

-> El Repository Se encuentran las clases que se conectan a una fuente de datos, como una API, Endpoints, DataBase. Un patron donde podemos switchear entre las fuentes de información

-> Data/Model Capa de modelos, se muestran modelos que nos ayudan a manejar los datos, PODO PLain Old Dart Object, esta clase donde se registran los atributos. En el caso de Dart tenemos estos archivos PODO files ya que representan la definicion de datos.


Asi tendríamos la estructura de directorio en un proyecto

- src 
	- blocs
	- models
	- resources
	- ui
	  app.dart
	main.dart
- test

Se agregara BLoC y Clean Architecture para dar mas claridad al proyecto.

La arquitectura de Clean Architecture muestra o expone las entidades y aplica BLoC hacia dentro
Es decir, cada entidad tendrá implementada BLoC pattern para que cada entidad tenga su propia logica del negocio y demas entidades

src/
	Place
		bloc/
		model/
		repository/
		ui/
			screens/
			widgets/
	User
		bloc/
		model/
		repository/
		ui/
			screens/
			widgets/
	Widgets -> Uso general
	main.dart
	trips.dart

