# Esquema de Room 

## Room
- Librería de persistencia para bases de datos SQLite en Android.
- Facilita el acceso a la base de datos sin escribir consultas SQL manualmente.
- Proporciona una capa de abstracción sobre SQLite.

## Dependencias
- `implementation (libs.androidx.room.runtime)`
- `ksp (libs.androidx.room.compiler.v252)`
- `implementation (libs.androidx.room.ktx)`

## Cómo usar Room

### Creación de la Entidad (Entity)
- Representa una tabla en la base de datos.
- Define atributos como claves primarias y columnas.
  ```kotlin
     @Entity(tableName = "user_table")
     data class User(
         @PrimaryKey(autoGenerate = true) val id: Int,
         @ColumnInfo(name = "first_name") val firstName: String,
         @ColumnInfo(name = "last_name") val lastName: String
     )
     ```

### Creación de la DAO (Objeto de acceso a datos)
- Contiene los métodos para interactuar con la base de datos, como insertar, actualizar, eliminar y consultar datos.
  ```kotlin
     @Dao
     interface UserDao {
         @Insert
         suspend fun insert(user: User)

         @Query("SELECT * FROM user_table")
         fun getAllUsers(): LiveData<List<User>>
     }
     ```

### Creación de la Base de Datos (RoomDatabase)
- Clase abstracta anotada con `@Database`, que lista las entidades asociadas.
- Define métodos abstractos para acceder a las clases DAO.
```kotlin
     @Database(entities = [User::class], version = 1, exportSchema = false)
     abstract class UserDatabase : RoomDatabase() {
         abstract fun userDao(): UserDao
     }
```

### Inicialización de Room
- Usa `Room.databaseBuilder` para crear la base de datos.

### Uso de Room en el Proyecto
- Realiza operaciones de inserción, actualización, eliminación y consulta a través del DAO.

  ```kotlin
     val userDao = UserDatabase.getDatabase(applicationContext).userDao()
     userDao.getAllUsers().observe(this, Observer { users ->
         // Actualizar UI con la lista de usuarios
     })
     ```

  ```kotlin
     val user = User(0, "Angel", "Rodriguez")
     userDao.insert(user)
     ```
