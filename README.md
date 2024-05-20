# el-codigo-para-copiar

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora de Edad</title>
</head>
<body>
    <h1>Calculadora de Edad</h1>
    <form method="post">
        <label for="fecha_nacimiento">Fecha de Nacimiento:</label>
        <input type="date" id="fecha_nacimiento" name="fecha_nacimiento">
        <input type="submit" value="Calcular">
    </form>

    <?php
    if ($_SERVER["REQUEST_METHOD"] == "POST") {
        // Obtener la fecha de nacimiento del formulario
        $fecha_nacimiento = $_POST["fecha_nacimiento"];

        // Encriptar la fecha de nacimiento usando varias funciones
        // 1. Usar hash() para obtener un hash SHA-256
        $hash_fecha = hash('sha256', $fecha_nacimiento);

        // 2. Usar password_hash() para encriptar con BCRYPT
        $password_hash_fecha = password_hash($fecha_nacimiento, PASSWORD_BCRYPT);

        // 3. Usar openssl_encrypt() para encriptar con AES-256-CBC
        $clave_encriptacion = '12345678901234567890123456789012'; // Clave de 32 caracteres
        $iv = openssl_random_pseudo_bytes(openssl_cipher_iv_length('aes-256-cbc'));
        $openssl_encrypted_fecha = openssl_encrypt($fecha_nacimiento, 'aes-256-cbc', $clave_encriptacion, 0, $iv);
        $openssl_encrypted_fecha = base64_encode($openssl_encrypted_fecha . '::' . $iv);

        // Calcular la edad en segundos, minutos, horas, días y semanas
        $fecha_actual = time();
        $fecha_nac_timestamp = strtotime($fecha_nacimiento);
        $diferencia_segundos = $fecha_actual - $fecha_nac_timestamp;
        $diferencia_minutos = $diferencia_segundos / 60;
        $diferencia_horas = $diferencia_minutos / 60;
        $diferencia_dias = $diferencia_horas / 24;
        $diferencia_semanas = $diferencia_dias / 7;

        // Mostrar resultados
        echo "<p>Edad en segundos: " . $diferencia_segundos . "</p>";
        echo "<p>Edad en minutos: " . $diferencia_minutos . "</p>";
        echo "<p>Edad en horas: " . $diferencia_horas . "</p>";
        echo "<p>Edad en días: " . $diferencia_dias . "</p>";
        echo "<p>Edad en semanas: " . $diferencia_semanas . "</p>";
        
        // Mostrar cadenas encriptadas
        echo "<p>Hash SHA-256 de la fecha de nacimiento: " . $hash_fecha . "</p>";
        echo "<p>Password Hash (BCRYPT) de la fecha de nacimiento: " . $password_hash_fecha . "</p>";
        echo "<p>OpenSSL Encrypt (AES-256-CBC) de la fecha de nacimiento: " . $openssl_encrypted_fecha . "</p>";
    }
    ?>
</body>
</html>
