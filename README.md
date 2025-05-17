# Altera-Produto.PHP
Criação do alteraproduto.php

<?php
include('conn.php');

// Verifica se foi passado um ID via GET
if (isset($_GET['id'])) {
    $id = $_GET['id'];

    // Busca o produto atual
    $sql = "SELECT * FROM tb_produtos WHERE id_produto = $id";
    $result = mysqli_query($link, $sql);
    $produto = mysqli_fetch_assoc($result);

    if (!$produto) {
        echo "Produto não encontrado.";
        exit;
    }
} elseif ($_SERVER['REQUEST_METHOD'] == 'POST') {
    // Dados do formulário
    $id = $_POST['id'];
    $nome = $_POST['nome'];
    $descricao = $_POST['descricao'];
    $caracteristica = $_POST['caracteristica'];
    $estoque = $_POST['estoque'];
    $valor = $_POST['valor'];
    $barcode = $_POST['barcode'];
    $status = $_POST['status'];

    // Se imagem foi enviada
    if (!empty($_FILES['imagem']['name'])) {
        $imagem = $_FILES['imagem']['name'];
        // Atualiza com imagem
        $sql = "UPDATE tb_produtos SET 
            nome_produto='$nome',
            descricao_produto='$descricao',
            caracteristica_produto='$caracteristica',
            estoque_produto='$estoque',
            valor_produto='$valor',
            imagem_produto='$imagem',
            barcode_produto='$barcode',
            status_produto='$status'
            WHERE id_produto=$id";
    } else {
        // Atualiza sem mexer na imagem
        $sql = "UPDATE tb_produtos SET 
            nome_produto='$nome',
            descricao_produto='$descricao',
            caracteristica_produto='$caracteristica',
            estoque_produto='$estoque',
            valor_produto='$valor',
            barcode_produto='$barcode',
            status_produto='$status'
            WHERE id_produto=$id";
    }

    mysqli_query($link, $sql);
    mysqli_close($link);
    header('Location: listaprodutos.php');
    exit();
} else {
    echo "ID do produto não informado.";
    exit;
}
?>

<!DOCTYPE html>
<html lang="pt-br">
<head>
    <meta charset="UTF-8">
    <title>Alterar Produto</title>
    <link rel="stylesheet" href="cadastra.css">
</head>
<body>
    <div class="container">
        <h1>Alterar Produto</h1>
        <form action="alteraproduto.php" method="post" enctype="multipart/form-data">
            <input type="hidden" name="id" value="<?= $produto['id_produto'] ?>">
            <label for="nome">Nome:</label>
            <input type="text" name="nome" id="nome" value="<?= $produto['nome_produto'] ?>" required><br>
            
  <label for="descricao">Descrição:</label>
            <input type="text" name="descricao" id="descricao" value="<?= $produto['descricao_produto'] ?>" required><br>
            
  <label for="caracteristica">Característica:</label>
            <input type="text" name="caracteristica" id="caracteristica" value="<?= $produto['caracteristica_produto'] ?>" required><br>
            
  <label for="estoque">Estoque:</label>
            <input type="number" name="estoque" id="estoque" value="<?= $produto['estoque_produto'] ?>" required><br>
            
  <label for="valor">Valor:</label>
            <input type="number" name="valor" id="valor" value="<?= $produto['valor_produto'] ?>" step="0.01" required><br>
            
  <label for="imagem">Imagem (deixe em branco para manter a atual):</label>
            <input type="file" name="imagem" id="imagem"><br>

  <label for="barcode">Código de Barras:</label>
            <input type="number" name="barcode" id="barcode" value="<?= $produto['barcode_produto'] ?>"><br>

  <label for="status">Status:</label>
            <input type="text" name="status" id="status" value="<?= $produto['status_produto'] ?>"><br>

  <input type="submit" value="Atualizar">
        </form>
    </div>
</body>
</html>
