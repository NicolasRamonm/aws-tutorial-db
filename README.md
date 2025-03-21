# aws-tutorial-db
Aprendendo a utilizar EC2 e RDS

Abra o vídeo clicando [aqui.](https://youtu.be/Ok3WJ1xbAD8)


### **Comandos para criar a tabela no banco de dados RDS**:

No MySQL (RDS), para criar a tabela **`products`** com 4 campos e 3 tipos de dados diferentes, o comando foi:

```sql
CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,        -- Tipo de dado: INT
    name VARCHAR(100) NOT NULL,               -- Tipo de dado: VARCHAR
    price DECIMAL(10, 2) NOT NULL,            -- Tipo de dado: DECIMAL
    stock INT NOT NULL,                       -- Tipo de dado: INT
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP -- Tipo de dado: TIMESTAMP
);
```

---

### **Código PHP para a página de conexão com o banco de dados (`db.php`)**:


```php
<?php
define('DB_SERVER', 'tutorial-db-instance.c0fbfkakyyms.us-east-1.rds.amazonaws.com');
define('DB_USERNAME', 'tutorial_user');
define('DB_PASSWORD', '**********');
define('DB_DATABASE', 'sample');

$conn = new mysqli(DB_SERVER, DB_USERNAME, DB_PASSWORD, DB_DATABASE);

if ($conn->connect_error) {
    die("Conexão falhou: " . $conn->connect_error);
}
?>
```

---

### **Código para a página de cadastro e listagem de produtos (`index.php`)**:

Arquivo PHP para permitir a inserção e listagem de produtos:

```php
<?php
include 'db.php';

if (isset($_POST['add_product'])) {
    $name = $_POST['name'];
    $price = $_POST['price'];
    $stock = $_POST['stock'];

    $sql = "INSERT INTO products (name, price, stock) VALUES ('$name', '$price', '$stock')";
    if ($conn->query($sql) === TRUE) {
        echo "Produto adicionado com sucesso!";
    } else {
        echo "Erro: " . $sql . "<br>" . $conn->error;
    }
}

$sql = "SELECT * FROM products";
$result = $conn->query($sql);
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Cadastro de Produtos</title>
</head>
<body>
    <h1>Cadastro de Produtos</h1>
    
    <!-- Formulário para adicionar produto -->
    <form method="POST">
        <label for="name">Nome:</label>
        <input type="text" name="name" required><br>
        <label for="price">Preço:</label>
        <input type="number" name="price" step="0.01" required><br>
        <label for="stock">Estoque:</label>
        <input type="number" name="stock" required><br>
        <button type="submit" name="add_product">Adicionar Produto</button>
    </form>

    <h2>Lista de Produtos</h2>
    <table border="1">
        <tr>
            <th>ID</th>
            <th>Nome</th>
            <th>Preço</th>
            <th>Estoque</th>
            <th>Data de Criação</th>
        </tr>
        <?php while ($row = $result->fetch_assoc()) { ?>
        <tr>
            <td><?php echo $row['id']; ?></td>
            <td><?php echo $row['name']; ?></td>
            <td><?php echo $row['price']; ?></td>
            <td><?php echo $row['stock']; ?></td>
            <td><?php echo $row['created_at']; ?></td>
        </tr>
        <?php } ?>
    </table>
</body>
</html>

<?php
$conn->close();
?>
```

---

<div align="center">
<sub>AWS-EC2 Painel:</sub><br>
<img src="images\922B8A18-E8E9-41A0-851E-A2DCE242FA0C.png" width="80%" ><br>
</div>

<div align="center">
<sub>AWS-RDS Painel:</sub><br>
<img src="images\08AEB7E1-C284-40AB-A6E8-30932D9FBE20.png" width="80%" ><br>
</div>

<div align="center">
<sub>AWS-RDS Stats:</sub><br>
<img src="images\27462E1F-37C9-4D54-BF37-CE67DB17D008.png" width="80%" ><br>
</div>

<div align="center">
<sub>AWS Terminal conectado MySQL:</sub><br>
<img src="images\728D6631-F18B-41F6-98E5-6EEC806A6494.png" width="80%" ><br>
</div>