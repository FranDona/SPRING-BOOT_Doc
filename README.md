# SPRING-BOOT_Doc

[//]: # (version: 1.0)
[//]: # (author: Fran Dona Villar)
[//]: # (date: 2024-03-14)

- [SPRING-BOOT\_Doc](#spring-boot_doc)
  - [¿Que es Spring Boot?](#que-es-spring-boot)
  - [Instalaicon Spring Boot](#instalaicon-spring-boot)
    - [Poner en Español STS](#poner-en-español-sts)
  - [Crear un nuevo Proyecto](#crear-un-nuevo-proyecto)
  - [CRUD](#crud)
    - [Consultas Genericas](#consultas-genericas)
    - [CRUD Completos](#crud-completos)

## ¿Que es Spring Boot?
Spring Boot es un framework de desarrollo de aplicaciones Java que simplifica la configuración y el desarrollo mediante la automatización de tareas repetitivas. Proporciona configuraciones predeterminadas inteligentes y herramientas integradas para crear aplicaciones rápidamente con un mínimo esfuerzo.

## Instalaicon Spring Boot
1. Empezaremos dirigiéndonos a la [página de spring](https://spring.io/tools)
   
2. Seleccionaremos la versión que necesitemos en función de nuestro SO (En nuestro caso `Linux x86_64`)
   
3. Nos dirigimos a Descargas y creamos una carpeta llamada sts 
   
4. Movemos el archivo descargado a la carpeta sts
   
5. Descomprimimos el archivo con tar **-xvf nombre_archivo**
   
6. Movemos el archivo descomprimido sts-4.22.0.RELEASE hacia /usr/sahre/sts
    ```bash
    sudo mv sts-4.22.0.RELEASE /usr/sahre/sts
    ```
    La carpeta se creará automáticamente

7. Nos colocamos en la ruta mencionada anteriormente
   
8. Arrancamos la aplicación sts con:
   ```bash
   ./SpringToolSuite4
   ```

9.  Nos pedirá donde queremos guardar el área de trabajo, en mi caso usare **/home/nombre/Documentos/sts**

Con esto ya tendremos funcionando STS pero vamos a crear un acceso directo para poder abrirlo más cómodamente.

```bash
cd /usr/share/applications
sudo nano sts.desktop
```

Agregamos el siguiente contenido para crear el acceso directo:

```console
[Desktop Entry]
Version=1.0
Type=Application
Name=STS
GenericName=STS
Comment=STS
Exec=/usr/share/sts/SpringToolSuite4
TryExec=/usr/share/sts/SpringToolSuite4
Icon=/usr/share/sts/icon.xpm
Terminal=false

Para actualizar los enlaces usamos:
#sudo update-desktop-database /usr/share/applications
```

### Poner en Español STS
Para ponerlo en español hacemos lo siguiente:
1. Para ponerlo en español: Help > Install new software
2. En Name ponemos Babel y en Location: https://download.eclipse.org/technology/babel/update-site/latest/
3. Tras unos segundos (¡tarda un poquito!), le damos a Babel Language Packs in Spanish, marcando la casilla y pulsamos [Next]
4. De nuevo tras varios segundos, le damos a [next], aceptamos la licencia y [Finish]. Cuando termine la instalación (miramos la esquina inferior derecha), reiniciamos Eclipse.

## Crear un nuevo Proyecto
Crear el proyecto desde la aplicación:

1. Entramos dentro de STS y vamos a: Archivo>Nuevo>Spring Starter Project

2. Rellenamos los datos que nos piden:
   - Le ponemos un nombre al proyecto si queremos
   - Maven
   - 17
   - Jar
   - Java

3. Pulsamos siguiente y añadimos las dependencias
   - Spring Boot DevTools
   - Thymeleaf
   - Spring Web

4. Pulsamos Finalizar para terminar.
---
Crear el proyecto desde [start.spring.io](https://start.spring.io):

1. Entramos al enlace [https://start.spring.io](https://start.spring.io)<br>
   
    Seleccionamos la sigueintes opciones
   - Project: Maven
   - Spring Boot: 3.2.3
   - Project Metadata: Colocamos los nombres que queramos usar
   - Dependencies: Spring Boot DevTools, Thymeleaf, Spring Web

2. Generamos el archivo pulsando CTRL + ENTER
   
3. Guardamos el archivo en nuestra area de trabajo de STS para tenerlo más controlado

4. Entramos dentro de STS y vamos a: Archivo>Incorporar proyectos del sistema de archivos. Pulsamos directorio y lo buscamos

## CRUD
### Consultas Genericas

Tendremos que hacer una serie de pasos:
   1. Definir el modelos (ClientesModel)
   2. Desarrollar la Interfaz repositorio (que hereda de JPA)
   3. Usar la interfaz en el Servicio
   4. Implementar el controlador

1. Modificamos el arvhivo ClientesModel /src/main/java/nombre_pro/repositories/ClietnesModelo

```java
package com.soltel.islantilla.models;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.OneToMany;
import jakarta.persistence.Table;
import java.util.Set;


/**
 * IMPORTANTE: Para las entidades el orden es:
 * 1) ClientesModel (la tabla principal)
 * 2) ReservasId (por la clave compuesta)
 * 3) ReservasModel (la tabla secundaria)
 */
@Entity
@Table(name = "clientes")
public class ClientesModel {

    // Atributos (campos BBDD)
    @Id
    private String nif;

    @Column (name = "nombre")
    private String nombre;

    @Column
    private int edad;

    // OJO, se guarda en la BBDD como 1 o 0 (tinyint)
    @Column
    private int sexo;

    // Hay que poner en la relación de tablas, OneToMay en el 1 y ManyToOne en el muchos
    // Aquí ponemos el OneToMany. El mapeo hay que ponerle el nombre de la entidad en singular
	// mappedBy = "cliente" debe coincidir con el nombre del campo en la tabla Reservas
    @OneToMany (mappedBy = "cliente")
	private Set<ReservasModel> reservas;
    
    // Setter y Getter
    
	public String getNif() {
		return nif;
	}

	public void setNif(String nif) {
		this.nif = nif;
	}

	public String getNombre() {
		return nombre;
	}

	public void setNombre(String nombre) {
		this.nombre = nombre;
	}

	public int getEdad() {
		return edad;
	}

	public void setEdad(int edad) {
		this.edad = edad;
	}

	public int getSexo() {
		return sexo;
	}

	public void setSexo(int sexo) {
		this.sexo = sexo;
	}

	// Constructores
	public ClientesModel() {}
	
	public ClientesModel(String nif, String nombre, int edad, int sexo) {
		super();
		this.nif = nif;
		this.nombre = nombre;
		this.edad = edad;
		this.sexo = sexo;
	}
    
}
```

2. Ahora modificamos el repositorio /src/main/java/nombre_pro/repositories/IClientesRepository

```java
package com.soltel.islantilla.repositories;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.soltel.islantilla.models.ClientesModel;

// Se hereda de JpaRepository y se especifica (Modelo, TipoPK)
@Repository
public interface IClientesRepository extends JpaRepository<ClientesModel, String> {

}
```

3. Ahora modificamos el controlador /src/main/java/nombre_pro/controllers/ClientesController

```java
@RestController
@RequestMapping("/clientes")
public class ClientesController {

   // Atributo principal
   private final ClientesService clientesService;

   // Constructor
   public ClientesController (ClientesService clientesService) {
      this.clientesService = clientesService;
   }

   // Método de consulta general
   // endpoint de ejemplo: http://localhost:8100/clientes/consultar
   @GetMapping("/consultar")
   public ResponseEntity<List<ClientesModel>> getAllClientes(){
      return ResponseEntity.ok(clientesService.findAllClientes());
   }
}

```

### CRUD Completos

```java
@Service
public class ClientesService {

    private final ClientesRepository clientesRepository;

    public ClientesService(ClientesRepository clientesRepository) {
        this.clientesRepository = clientesRepository;
    }

    // CREATE (guardar)
    public ClientesModel saveCliente(ClientesModel cliente) {
        return clientesRepository.save(cliente);
    }

    // READ (Consultar)
    public Optional<ClientesModel> getById(Integer id) {
        return clientesRepository.findById(id);
    }

    // UPDATE (Actualizar)
    // Nota: Este método se correspondería con una operación PUT
    public ClientesModel updateById(ClientesModel request, Integer id) {
        Optional<ClientesModel> clienteOptional = clientesRepository.findById(id);
        if (clienteOptional.isPresent()) {
            ClientesModel cliente = clienteOptional.get();
            cliente.setNombre(request.getNombre());
            cliente.setApellido(request.getApellido());
            cliente.setEmail(request.getEmail());
            // Aquí puedes añadir más campos que desees actualizar

            return clientesRepository.save(cliente); // Guardar y devolver el cliente actualizado
        }
        return null; // O puedes manejar el caso en que el cliente no exista, lanzar una excepción, etc.
    }

    // DELETE (Borrar)
    public boolean deleteCliente(Integer id) {
        try {
            clientesRepository.deleteById(id);
            return true;
        } catch (Exception e) {
            return false;
        }
    }
}
```