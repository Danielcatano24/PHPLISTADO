# PHPLISTADO
LISTADO
<?php
require_once 'model.php';

ini_set('display_errors', 1);
ini_set('display_startup_errors', 1);
error_reporting(E_ALL);

$modelo = new Usuarios(); //4

switch ($_GET['op']) { //6
	case 'listar': //7
		$respuesta = $modelo->listar();
		$data = array();
			while ($row = $respuesta->fetch_assoc()) {
				$data[] = array(
					'0' => $row['name'],
					'1' => $row['password'],
					'2' => '<button class="btn btn-warning" onclick="mostrar('.$row['id'].')">Editar</button>',
					'3' => '<button class="btn btn-danger" onclick="deleteRow('.$row['id'].')">Eliminar</button>'
				);
			}

		$results = array(
 			"sEcho"=>1, //Información para el datatables
 			"iTotalRecords"=>count($data), //enviamos el total registros al datatable
 			"iTotalDisplayRecords"=>count($data), //enviamos el total registros a visualizar
 			"aaData"=>$data);

		echo json_encode($results);
	break;

	case 'mostrar':
		$id = $_POST['id'];
		$respuesta = $modelo->mostrar($id);
		echo json_encode($respuesta);
		break;



	case 'guardaryeditar':
		if (empty($_POST['id'])) {
			$respuesta = $modelo->guardar($_POST['nombre'], $_POST['pass']);
			echo $respuesta ? 'Guardado' : 'No se pudo guardar'; 
		}else{
			$respuesta = $modelo->editar($_POST['id'], $_POST['nombre'], $_POST['pass']);
			echo $respuesta ? 'Actualizado' : 'No se pudo editar'; 
		}
		break;



		case 'deleteRow':
			$respuesta = $modelo->deleteRow($_POST['id']);
			echo $respuesta ? 'Eliminado' : 'No se pudo eliminar'; 
		break;
	

}

?>
