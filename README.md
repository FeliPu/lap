# lap

<?php

$db_hostname = '127.0.0.1';
$db_username = 'root';
$db_password = '';
$db_database = 'patientenakte';

$con = new PDO("mysql:host=$db_hostname;dbname=$db_database", $db_username, $db_password);
if (!$con) {
	die('Could not connect: ' . mysqli_error($link));
}
?>

<!DOCTYPE html>
<html lang="en">
<body>
	<main>
		<div class="title">
			Enter Person Name here to see Medication
		</div>
		<form action="" method="post">
			<input type="text" name="enterhere">
			<input type="submit" name="submit">
		</form>
		<div class="output">
			Pescribed Drug:
			<?php
			if (isset($_POST['submit'])) {
				$search = $_POST['enterhere'];
				$statement = $con->prepare("SELECT * FROM patientenakte.befund as b
			 	Join patientenakte.Medikamente as m
				ON b.medikamente_idMedikamente = m.idmedikamente
				Join patientenakte.patient as p
				ON b.Patient_idPatient = p.idPatient
				WHERE Patient_Name = ?
				");
				$statement->execute(array($search));
				while ($row = $statement->fetch()) {
					echo $row['Dosierung'];
					echo str_repeat('&nbsp;', 1);
					echo $row['Name'];
				}
			}
			?>
		</div>
	</main>
	<div class="input">
		<div class="title">
			Input Medication:
		</div>
		<form action="" method="post">
			<input type="text" name="mednr">
			<input type="text" name="medna">
			<input type="submit" name="imput">
		</form>
		<div class="output">
		<?php
		if (isset($_POST['imput'])) {
			$in1 = $_POST['mednr'];
			$in2 = $_POST['medna'];
			$postitman = $con->prepare("INSERT INTO patientenakte.Medikamente VALUES (?, ?)");
			$postitman->execute(array($in1, $in2));
		}
		$con = null;
		?>
		</div>
	</div>
</body>

</html>
