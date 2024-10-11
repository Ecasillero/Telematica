
```
<?php
// Configuración de conexión a la base de datos
$conn = new mysqli('localhost', 'lasucreña', '12345678', 'lasucreñaa');

// Verificar la conexión
if ($conn->connect_error) {
    die("Conexión fallida: " . $conn->connect_error);
}

// Consulta SQL
$sql = "SELECT * FROM mi_tabla"; // Asegúrate de reemplazar "mi_tabla" con el nombre real de tu tabla
$result = $conn->query($sql);

// Verificar si hay resultados
if ($result->num_rows > 0) {
    // Salida de cada fila
    while($row = $result->fetch_assoc()) {
        echo "id: " . $row["id"] . " - Nombre: " . $row["nombre"] . "<br>"; // Asegúrate de que "id" y "nombre" son col>
    }
} else {
    echo "0 resultados";
}

// Cerrar la conexión
$conn->close();
?>
