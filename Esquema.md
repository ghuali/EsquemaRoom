# Esquema de Room 

## Room
- Librería de persistencia para bases de datos SQLite en Android.
- Facilita el acceso a la base de datos sin escribir consultas SQL manualmente.
- Proporciona una capa de abstracción sobre SQLite.
### Creación de la Entidad (Entity) 
- Definir la clase que representa una tabla en la base de datos.
- El siguiente código define una entidad de datos User. Cada instancia de User representa una fila en una tabla de user en la base de datos de la app.
- @Entity
      data class User(
        @PrimaryKey val uid: Int,
        @ColumnInfo(name = "first_name") val firstName: String?,
        @ColumnInfo(name = "last_name") val lastName: String?
      )
### Creación de la DAO (Objeto de acceso a datos) 
- El DAO define los métodos de acceso a la base de datos (consultas).
- El siguiente código define un DAO llamado UserDao. UserDao proporciona los métodos que el resto de la app usa para interactuar con los datos de la tabla user.

      @Dao
      interface UserDao {
          @Insert
          suspend fun insert(user: User)
      
          @Update
          suspend fun update(user: User)
      
          @Delete
          suspend fun delete(user: User)
      
          @Query("SELECT * FROM user_table")
          suspend fun getAllUsers(): List<User>
      }
