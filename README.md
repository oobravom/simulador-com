![imagen inexistente](activos/imagenes/TlacoTec-logo.png)


# Requerimientos

- java 11 o superior
- apache maven 3.8.5 o superior
- docker 20.10.17-ce o superior


# Instalación o actualización del _Simulador_

1. cd $DIRECTORIO_DE_TRABAJO/simulador-pql/
   - docker build -t oobravom/simulador-pql:1.0-SNAPSHOT .
2. cd $DIRECTORIO_DE_TRABAJO/simulador-mdb/
   - docker build -t oobravom/simulador-mdb:1.0-SNAPSHOT .
3. cd $DIRECTORIO_DE_TRABAJO/simulador-key/
   - docker build -t oobravom/simulador-key:1.0-Final .
4. cd $DIRECTORIO_DE_TRABAJO/simulador-api/
   - docker build -t oobravom/simulador-api:1.0-SNAPSHOT .
5. cd $DIRECTORIO_DE_TRABAJO/simulador-web/
   - docker build -t oobravom/simulador-web:1.0-SNAPSHOT .

# Configuración Inicial del _Simulador_
Ir al directorio $DIRECTORIO_DE_TRABAJO/simulador-com

  - docker-compose down
  - docker-compose up -d simulador-pql
  - docker-compose logs -f

Ir al directorio $DIRECTORIO_DE_TRABAJO/simulador-com/config

  - docker-compose down
  - docker-compose up -d 
  - docker-compose logs -f

## Inicializar los perfiles de seguridad en keycloak

- Ir a la siguiente dirección: https://localhost:8443/admin/
- Autentificarse con las siguientes credenciales:
  - Username: simulador-key
  - Password: TlacoTec

![imagen inexistente](activos/imagenes/keycloak_login.png)

### Agregar el usuario inicial: *oobravom*

1. Seleccionar el realm adecuado en este caso **_Simulador-key_**

   ![imagen inexistente](activos/imagenes/keycloak_select_realm.png)

   ![imagen inexistente](activos/imagenes/keycloak_selected_realm.png)

   ***NOTA: En caso de haber agregado el usuario con anterioridad ir a la sección [Configurar el usuario inicial](#id-config-usr).***

2. Ir a la seccion _Manage -> Users_, clic en **_Create new user_**:

   ![imagen inexistente](activos/imagenes/keycloak_create_new_user.png)

3. Ingresar la información requerida, clic en **_Create_**:

   ***NOTA: Es muy importante capturar en campo de Username el valor de "oobravom" como se muestra en la siguiente imagen.***

   ![imagen inexistente](activos/imagenes/keycloak_create.png)

### Configurar el usuario inicial: *oobravom*

1. Ir a la pestaña _Credentials_, clic en **_Set password_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_credentials_set_password.png)
   
2. Ingresar la información solicitada:
   - Usar **tlacotepec** para _Password/Password Confirmation_, clic en **_Save_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_credentials_set_password_save.png)
   
   - Clic en **_Save password_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_credentials_set_password_save_password.png)

3. Ir a la pestaña _Role Mapping_, clic en **_Assing role_**:
    
   ![imagen inexistente](activos/imagenes/keycloak_role_mapping_assing_role.png)
   
   - Seleccionar sus roles necesarios (sólo requerimos **administrador**), clic en **_Assing_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_role_mapping_assing_role_assing.png)
   
   - Verificar que se hayan asignado los roles correctamente:
   
   ![imagen inexistente](activos/imagenes/keycloak_role_mapping.png)

4. Ir a la pestaña _Groups_, clic en **_Join Group_**:

   ![imagen inexistente](activos/imagenes/keycloak_groups_join_group.png)

   - Asignar sus grupos necesarios (sólo requerimos **usuarios-simulador**), clic en **_Join_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_groups_join_group_join.png)

   - Verificar que se hayan asignado los grupos correctamente:
   
   ![imagen inexistente](activos/imagenes/keycloak_groups.png)
   
6. Ir a la pestaña _Details_:

   ![imagen inexistente](activos/imagenes/keycloak_details.png)

   - Verificar que en campo _Required User Actions_ esté vacío:
   
   ![imagen inexistente](activos/imagenes/keycloak_details_clean.png)
   
   - En caso de tener acciones, se deben eliminar, clic en **_Save_**:
   
   ![imagen inexistente](activos/imagenes/keycloak_details_clean_save.png)
   
## Verificar la generación de las credenciales para el _Simulador_ (opcional)

Usando un cliente HTTP como es _Postman_, solicitar una petición **POST** con las siguientes características:

- URL: https://localhost:8443/realms/simulador-key/protocol/openid-connect/token
  - realm: simulador-key
  - grant_type: password
  - client_id: simulador-web
  - username: oobravom
  - password: tlacotepec

![imagen inexistente](activos/imagenes/postman_access_token.png)


## Validar que el token devuelto sea adecuado (opcional)

Ir a la siguiente dirección https://jwt.io/ e inspeccionar el *access token* devuelto en la solicitud anterior, 
se recomienda revisar principalmente el **PAYLOAD** verficando los valores configurados en secciones anteriores:

![imagen inexistente](activos/imagenes/jwt_access_token_1.png)
![imagen inexistente](activos/imagenes/jwt_access_token_2.png)

***NOTA: Es muy importante dar de baja los servicios asociados a la configuración del usuarios.***

Ir al directorio $DIRECTORIO_DE_TRABAJO/simulador-com/config

  - docker-compose stop

# Ejecución del _Simulador_

Se debe proceder a levantar los servicios asociados al simulador.

Ir al directorio $DIRECTORIO_DE_TRABAJO/simulador-com/

  - docker-compose up -d 
  - docker-compose logs -f

# Probar el _Simulador_

***NOTA: Es muy importante asegurarse se encuentren levantados los servicio de manera correcta.***

Ir a la siguiente dirección: http://localhost:9090/simulador/login.jsf

- Autentificarse con las siguientes credenciales:
  - Usuario: oobravom
  - Contraseña: tlacotepec
  
  ![imagen inexistente](activos/imagenes/simulador_login.png)

- Verificar que se cargue el catálogo de **enunciados** y se pueda manipular adecuadamente.

  ![imagen inexistente](activos/imagenes/simulador_catalogo_enunciados.png)

***NOTA: Es muy importante dar de baja los servicios asociados a simulador.***

Ir al directorio $DIRECTORIO_DE_TRABAJO/simulador-com

  - docker-compose stop
