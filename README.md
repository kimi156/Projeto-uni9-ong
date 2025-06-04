# Projeto-uni9-ong
Segue abaixo os códigos utlizados para a criação da ONG Vanci
<?php
include 'banco.php';

$resultado = $conn->query("SELECT * FROM voluntarios");
?>

<!DOCTYPE html>
<html>
<head>
    <title>Gestão de Voluntários - ONG Vanci</title>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Gestão de Voluntários - ONG Vanci</h1>

    <h2>Cadastrar Voluntário</h2>
    <form action="cadastrar_voluntario.php" method="post">
        <input type="text" name="nome" placeholder="Nome" required><br>
        <input type="text" name="telefone" placeholder="Telefone" required><br>
        <input type="email" name="email" placeholder="Email" required><br>
        <input type="text" name="função" placeholder="Função" required><br>
        <input type="text" name="disponibilidade" placeholder="Disponibilidade" required><br>
        <input type="submit" value="Cadastrar">
    </form>

    <h2>Voluntários Cadastrados</h2>
    <table border="1">
        <tr>
            <th>Nome</th><th>Telefone</th><th>Email</th><th>Função</th><th>Disponibilidade</th><th>Ações</th>
        </tr>
        <?php while ($voluntario = $resultado->fetch_assoc()) { ?>
        <tr>
            <td><?= $voluntario['nome'] ?></td>
            <td><?= $voluntario['telefone'] ?></td>
            <td><?= $voluntario['email'] ?></td>
            <td><?= $voluntario['função'] ?></td>
            <td><?= $voluntario['disponibilidade'] ?></td>
            <td>
                <a href="editar_voluntario.php?id=<?= $voluntario['id'] ?>">Editar</a> |
                <a href="excluir_voluntario.php?id=<?= $voluntario['id'] ?>" onclick="return confirm('Tem certeza que deseja excluir?')">Excluir</a>
            </td>
        </tr>
        <?php } ?>
    </table>
</body>
</html>
<?php
$conn = new mysqli("localhost", "root", "", "ong");

if ($conn->connect_error) {
    die("Erro de conexão: " . $conn->connect_error);
}
?>
<?php
include 'banco.php';

$nome = $_POST['nome'];
$telefone = $_POST['telefone'];
$email = $_POST['email'];
$funcao = $_POST['função'];
$disponibilidade = $_POST['disponibilidade'];

$sql = "INSERT INTO voluntarios (nome, telefone, email, função, disponibilidade) 
        VALUES ('$nome', '$telefone', '$email', '$funcao', '$disponibilidade')";

$conn->query($sql);
header("Location: index.php");
exit();
?>
<?php
include 'banco.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $id = $_POST['id'];
    $nome = $_POST['nome'];
    $telefone = $_POST['telefone'];
    $email = $_POST['email'];
    $funcao = $_POST['função'];
    $disponibilidade = $_POST['disponibilidade'];

    $conn->query("UPDATE voluntarios SET 
        nome='$nome', 
        telefone='$telefone', 
        email='$email', 
        função='$funcao', 
        disponibilidade='$disponibilidade' 
        WHERE id=$id");

    header("Location: index.php");
    exit();
} else {
    $id = $_GET['id'];
    $resultado = $conn->query("SELECT * FROM voluntarios WHERE id=$id");
    $voluntario = $resultado->fetch_assoc();
}
?>

<!DOCTYPE html>
<html>
<head>
    <title>Editar Voluntário</title>
    <meta charset="UTF-8">
</head>
<body>
    <h1>Editar Voluntário</h1>
    <form method="post" action="editar_voluntario.php">
        <input type="hidden" name="id" value="<?= $voluntario['id'] ?>">
        <input type="text" name="nome" value="<?= $voluntario['nome'] ?>" required><br>
        <input type="text" name="telefone" value="<?= $voluntario['telefone'] ?>" required><br>
        <input type="email" name="email" value="<?= $voluntario['email'] ?>" required><br>
        <input type="text" name="função" value="<?= $voluntario['função'] ?>" required><br>
        <input type="text" name="disponibilidade" value="<?= $voluntario['disponibilidade'] ?>" required><br>
        <input type="submit" value="Salvar">
    </form>
</body>
</html>
<?php
include 'banco.php';

$id = $_GET['id'];
$conn->query("DELETE FROM voluntarios WHERE id=$id");

header("Location: index.php");
exit();
?>
