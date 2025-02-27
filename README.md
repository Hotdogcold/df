<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Cart</title>
    <script>
        window.onload = function() {
            document.getElementById('barcode').focus();
        };
    </script>
</head>
<body>

    <form action="barcode.php" method="POST" id="form">
        <label for="barcode">Scan your barcode:</label>
        <input type="text" name="barcode" id="barcode" required>
        <button type="submit">Submit</button>
    </form>

    <form action="barcode.php" method="POST" id="stopForm">
        <button type="submit" name="stop_shopping" value="1">Stop Shopping</button>
    </form>

    <div>
        <h2>Shopping Cart</h2>
        <?php
        session_start(); 
        $items = [
            "0000000036" => ["name" => "dave", "price" => 1],
            "0000000041" => ["name" => "harold", "price" => 0.5],
        ];

       
        if (!isset($_SESSION['total_bill'])) {
            $_SESSION['total_bill'] = 0;  
        }

       
        if ($_SERVER['REQUEST_METHOD'] == 'POST') {
        
            if (isset($_POST['stop_shopping']) && $_POST['stop_shopping'] == '1') {
                echo "<p>Shopping session is complete. No more items will be added.</p>";
              
                session_destroy();
                $_SESSION = []; 
            }
            elseif (isset($_POST['barcode'])) {
                $barcode = trim($_POST['barcode']);

                if ($barcode == "endcode") {
                    echo "<p>Shopping done</p>";
                } else {
                    if (array_key_exists($barcode, $items)) {
                        $item = $items[$barcode];
                        echo "<p>This is: " . $item['name'] . ", price is: " . $item['price'] . "</p>";

                        $_SESSION['total_bill'] += $item['price'];
                    } else {
                        echo "<p>Item not found: $barcode</p>";
                    }
                    echo "<p>Your total bill is: " . $_SESSION['total_bill'] . "</p>";
                }
            }
        }

        
        ?>
    </div>
</body>
</html>
