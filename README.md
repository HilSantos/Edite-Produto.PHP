# Edite-Produto.PHP
Criação do editeproduto.php

<?php
// Inclui arquivos de segurança, cabeçalho da página e conexão com o banco de dados //
include('segurancadez.php');
include('cabecalho.php');
include('conn.php');

// Verifica se o formulário foi enviado via método POST (edição do produto) //
if($_SERVER['REQUEST_METHOD']=='POST'){

    // Captura os dados enviados pelo formulário //
    $id = $_POST['id'];
    $nome = $_POST['nome'];
    $descricao = $_POST['descricao'];
    $caracteristica = $_POST['caracteristica'];
    $estoque = $_POST['estoque'];
    $valor = $_POST['valor'];
    $imagem = $_FILES['imagem']['name']; // Nome do arquivo de imagem enviado //
    $barcode = $_POST['barcode'];
    $img_atual = $_POST['imagem_atual']; // Nome da imagem atual (caso nenhuma nova seja enviada) //

    // Se nenhuma nova imagem foi enviada, mantém a imagem atual //
    if($imagem == ""){
        $imagem = $img_atual;
    }

    // Monta a query para atualizar os dados do produto //
    $sql = "UPDATE `tb_produtos` SET 
        `nome_produto`='$nome',
        `descricao_produto`='$descricao',
        `caracteristica_produto`='$caracteristica',
        `estoque_produto`='$estoque',
        `valor_produto`='$valor',
        `imagem_produto`='$imagem',
        `barcode_produto`='$barcode' 
        WHERE id_produto = $id";

    // Executa a atualização no banco de dados //
    mysqli_query($link, $sql);

    // Fecha a conexão com o banco //
    mysqli_close($link);

    // Redireciona para a lista de produtos após a edição //
    header('Location: listaprodutos.php');
    exit;
}

// Se não houver ID passado via GET, redireciona para a lista de produtos //
if (!isset($_GET['id'])) {
    header('Location: listaprodutos.php');
    exit();
}

// Obtém o ID do produto a partir da URL //
$id = $_GET['id'];

// Consulta os dados do produto no banco de dados //
$sql = "SELECT * FROM tb_produtos WHERE id_produto = $id";
$result = mysqli_query($link, $sql);
$tbl = mysqli_fetch_array($result); // Armazena os dados do produto //

// Fecha a conexão com o banco //
mysqli_close($link);
?>

<!DOCTYPE html>
<html lang="pt-BR">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Detalha Produto</title>
    <link href="https://fonts.googleapis.com/css2?family=Libre+Barcode+39&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="cadastra.css">
    <style>
        .container {
            max-width: 500px;
            margin: 50px auto;
            background-color: #fff;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

fieldset {
            border: none;
        }

label {
            display: block;
            margin-top: 15px;
            font-weight: bold;
        }

input[type="submit"] {
            display: block;
            width: 100%;
            margin-top: 30px;
        }

.barcode {
            font-family: 'Libre Barcode 39', cursive;
            font-size: 65px;
        }
    </style>
</head>

<body>
    <div class="container">
        <h1>Cadastra Produto</h1>
        <br>
        <form action="editeproduto.php" method="post" enctype="multipart/form-data">
            <fieldset>
                <input type="hidden" name="id" value="<?=$tbl[0]?>">
                <label for="nome">Produto:</label>
                <input type="text" value="<?= $tbl[1] ?>" name="nome">
                <br>
                <label for="descricao">Descrição:</label>
                <textarea name="descricao"><?= $tbl[2] ?></textarea>
                <br>
                <label for="caracteristica">Caracteristica:</label>
                <textarea name="caracteristica"><?= $tbl[3] ?></textarea>
                <br>
                <label for="estoque">Estoque:</label>
                <input type="number" value="<?= $tbl[4] ?>" name="estoque">
                <br>
                <label for="valor">Valor:</label>
                <input type="number" value="<?= $tbl[5] ?>" name="valor">
                <br>
                <label for="imagem">Imagem:</label>
                <img src="imagens/<?= $tbl[6] ?>" width="150" id="imagem">
                <input type="file" name="imagem" id="file" onchange="foto()">
                <input type="hidden" name="imagem_atual" value="<?=$tbl[6]?>">
                <br>
                
<label for="barcode">Código de Barras:</label>
                <input type="text" value="<?= $tbl[7] ?>" onkeyup="cb()" id="cbt" name="barcode" maxlength="13" minlength="13">
                <br>
                <div class="barcode" id="cb"><?=$tbl[7]?></div>
                <br>
                <br>
                <input type="submit" value="Gravar">
            </fieldset>
        </form>
    </div>
</body>

</html>
<script>
    function cb(){
        const n_cb = document.getElementById('cbt').value;
        document.getElementById('cb').innerHTML = n_cb;
    }
    function foto(){
        const img = document.getElementById('file').value;
        img_n = img.slice(12);
        document.getElementById('imagem').src = "imagens/" + img_n;
    }
</script>
