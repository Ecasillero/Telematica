````
<?php
// index.php
// Permitir solicitudes desde cualquier origen (CORS)
header("Access-Control-Allow-Origin: *");
header("Content-Type: application/json; charset=UTF-8");

// Obtener el método HTTP
$method = $_SERVER['REQUEST_METHOD'];

// Obtener la ruta solicitada
$request = $_SERVER['REQUEST_URI'];

// Eliminar parámetros de consulta de la URL
$request = explode('?', $request, 2)[0];

// Eliminar el prefijo del directorio (si es necesario)
$request = str_replace('/api-restful-basica', '', $request);

// Definir las rutas
switch ($request) {
    case '/api/items':
    case '/api/items/':
        require _DIR_ . '/data/items.php';
        break;
    default:
        // Si la ruta no coincide, devolver un 404
        http_response_code(404);
        echo json_encode(["mensaje" => "Recurso no encontrado"]);
        break;
}
