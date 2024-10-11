````
<?php
// data/items.php

// Permitir solicitudes desde cualquier origen (CORS)
header("Access-Control-Allow-Origin: *");
header("Content-Type: application/json; charset=UTF-8");

// Obtener el método HTTP
$method = $_SERVER['REQUEST_METHOD'];

// Obtener los datos enviados en la solicitud
$input = json_decode(file_get_contents('php://input'), true);

// Leer el archivo JSON
$itemsFile = _DIR_ . '/items.json';
$items = json_decode(file_get_contents($itemsFile), true);

if ($method == 'GET' && !isset($_GET['id'])) {
    // Retornar todos los items
    echo json_encode($items);
}
elseif ($method == 'GET' && isset($_GET['id'])) {
    $id = intval($_GET['id']);
    $item = null;
    foreach ($items as $i) {
        if ($i['id'] == $id) {
            $item = $i;
            break;
 }
    }
    if ($item) {
        echo json_encode($item);
    } else {
        http_response_code(404);
        echo json_encode(["mensaje" => "Item no encontrado"]);
    }
}
elseif ($method == 'POST') {
    if ($input !== null && isset($input['nombre']) && isset($input['descripcion'])) {
        $nombre = $input['nombre'];
        $descripcion = $input['descripcion'];
        $nuevoId = end($items)['id'] + 1;
        $nuevoItem = [
            "id" => $nuevoId,
            "nombre" => $nombre,
            "descripcion" => $descripcion
        ];
        $items[] = $nuevoItem;
        // Guardar en el archivo JSON
        file_put_contents($itemsFile, json_encode($items, JSON_PRETTY_PRINT));
        http_response_code(201);
        echo json_encode($nuevoItem);
    echo json_encode($nuevoItem);
    } else {
        http_response_code(400);
        echo json_encode(["mensaje" => "Faltan campos requeridos o error en el formato."]);
    }
}

elseif ($method == 'PUT' && isset($_GET['id'])) {
    $id = intval($_GET['id']);
    $nombre = $input['nombre'];
    $descripcion = $input['descripcion'];
    $itemActualizado = null;
    foreach ($items as &$i) {
        if ($i['id'] == $id) {
            $i['nombre'] = $nombre;
            $i['descripcion'] = $descripcion;
            $itemActualizado = $i;
            break;
        }
    }
    if ($itemActualizado) {
        // Guardar en el archivo JSON
        file_put_contents($itemsFile, json_encode($items, JSON_PRETTY_PRINT));
        echo json_encode($itemActualizado);
    } else {
        http_response_code(404);
echo json_encode(["mensaje" => "Item no encontrado"]);
    }
}
elseif ($method == 'DELETE' && isset($_GET['id'])) {
    $id = intval($_GET['id']);
    $index = null;
    foreach ($items as $key => $i) {
        if ($i['id'] == $id) {
            $index = $key;
            break;
        }
    }
    if ($index !== null) {
        $itemEliminado = $items[$index];
        unset($items[$index]);
        // Reindexar el arreglo
        $items = array_values($items);
        // Guardar en el archivo JSON
        file_put_contents($itemsFile, json_encode($items, JSON_PRETTY_PRINT));
        echo json_encode($itemEliminado);
    } else {
        http_response_code(404);
        echo json_encode(["mensaje" => "Item no encontrado"]);    }
}
else {
    http_response_code(405);
    echo json_encode(["mensaje" => "Método no permitido"]);
}
